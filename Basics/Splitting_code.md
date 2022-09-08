# Splitting up your code

When starting to develop a new feature, it is often faster to keep all things at hand in one file.
This especially true when developing is in the "still experimenting - do not know where i'll settle" mode.
When experimenting it is often quite a bit faster to not only keep everything in one file, but also in one or two
miles long functions. This allows really fast pivots and refactoring in that phase. While a good method to get going,
it is definitely not something You want to have left as is. There are a lot of downsides to this:

1. Only readable until you remember what you did.

   You will discover that after some time, when you forget where everything was and what it did, the long file becomes 
   very hard to read and navigate. It will be even more exaggerated, if someone else has modified the code meanwhile.
   
   Long boolean statements are a really special specimen of this sub-section. I can say that a lot, maybe even most, logic 
   mistakes in codes happen there and can be quite a headache to find.

2. Some parts of code might be re-useable, but are packed into a long sausage that can not be re-used,
   thus other developers might start duplicating parts of this code elsewhere. And code duplication is usually a bad
   thing - all fixes and improvements have to be carried to multiple places in the future and there is usually no
   way to find this out before it's in testing or even worse in production.

3. Even when reusable code blocks and need-to-export constants are spliced to separate functions and exported, multiple
   exports from one file is not problem free and can cause a lot of unexpected code leakage.

   A quite often occurring example of really bad code leakage:
  
   ```jsx
   // Button.js - A Component with multiple exports 
   import 'button.scss';
 
   export const buttonStyles = ['primary', 'secondary'];
 
   class Button extends PureComponent {
     handleClick = () => {}
     
     render() {
       const { style, title } = this.props;
 
       let dataStyle = buttonStyles[0];
       if (buttonStyles.includes(style)) {
         dataStyle = style;
       } else {
         console.error(`Unknown style for button ${ style}`);
       }
 
       return (
         <button onClick={ this.handleClick } data-style={ style }>
           { title }
         </button>
       )
     }
   }
 
   export default Button;
   ```

   ```jsx
   // This import in another file silently imports 'button.scss' also and adds it to your browsers html head section.
   import { buttonStyles } from './Button.js';
   ```


4. Big files - more conflicts in version control systems

   When using version control systems like GIT and branching process, where branches are not merged to master before tested,
   all big long file become a target for merge conflicts. Even though exactly same lines were not targeted with code
   git might get confused. Also long methods quite often require re-indenting (pushing inwards or outwards) nested code
   blocks. This effectively creates a lot of meaningless lines changed. We all know solving merge conflicts is not a
   pleasant and motivating thing to do. The last thing You want to have in an one-of-an-art javascript project,
   is these demotivating conflict solving interludes surfacing at the moment of inspiration.


## Splitting techniques

## 1. Split Multiple boolean statements

It is a good idea to split long boolean/if statements by assigning all sub-sections to readable single meaning
named variables as it makes code more readable and makes spotting logic mistakes a lot easier. This kind
of splitting and naming is actually a quite useful commenting technique (without actual comments). It allow adding more
readable and useful context to code. Also splitting up booleans allows targeting comments to the exact point of need.

Badly readable:
```javascript
cosnt isTimepickerVisible = (
  isAvailableDateRangeValid(availableDateRange) &&
  (charts.length > 0 || !(view.breakdownTimepicker === false || view.breakdownTimepicker === undefined)
);
```
Better readable with naming adding context:
```javascript {.good-code}
const isTimeSupportedInApp = isAvailableDateRangeValid(availableDateRange);

const hasCharts = charts.length > 0;

/** Default behavior or view if `breakdownTimepicker` property is omitted is visible */
const isTimepickerConfiguredInView = !(
  view.breakdownTimepicker === false ||
  view.breakdownTimepicker === undefined
);

const isTimepickerVisible = isTimeSupportedInApp && (hasCharts || isTimepickerConfiguredInView);
```

I can say that a lot, maybe even most, logic mistakes in code, happen in these long if statements. They can be quite a
headache to find. The long boolean declaration do not really have a good place to even put explaining comments, not to 
mention that adding variable names already is adding some context. I find it often, that when these lines are changed,
some blocks are kept blindly in belief that "if it was written, probably it is needed somewhere". So split up long
boolean statements and make sure that next developer has all knowledge needed to add modifications. It will stop
a lot of office-fighting and -blaming.