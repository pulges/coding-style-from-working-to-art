# Splitting up your code

When starting to develop a new feature, it is often faster to keep all things at hand in one file.
This especially true when developing is in the "still experimenting - do not know where i'll settle" mode.
When experimenting it is often quite a bit faster to not only keep everything in one file, but also in one or two
miles long functions. This allows really fast pivots and refactoring in that phase. While it is a good method to get
going, it is definitely not something You want to have left as is. There are a lot of downsides to this:

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

4. Big files - more conflicts in version control systems

   When using version control systems like GIT and branching process, where branches are not merged to master before tested,
   all big long file become a target for merge conflicts. Even though exactly same lines were not targeted with code
   git might get confused. Also long methods quite often require re-indenting (pushing inwards or outwards) nested code
   blocks. This effectively creates a lot of meaningless lines changed. We all know solving merge conflicts is not a
   pleasant and motivating thing to do. The last thing You want to have in an one-of-an-art javascript project,
   is these demotivating conflict solving interludes surfacing at the moment of inspiration.

5. Missing out on giving out more context, for the next developer, by simply having more properly named
   sections, variables etc.

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

More readable with naming also adding context:
```javascript {.good-code}
const isTimeSupportedInApp = isAvailableDateRangeValid(availableDateRange);

/** View should always show timepicker if it has charts */
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

## 2. Split code into smaller single purpose methods

There is a quite simple method that can be implemented, to know, if your method is still too long (even though it
already fits the screen).
  
  1. Try to give it a name that describes exactly what the method does.
  2. Start adding a comment block to describe its purpose and interface.
  
If any of it find it becomes rather difficult or cumbersome, then your method is probably doing too much 
and you should split the method up into smaller methods that each serves only one "simple to name" purpose.
On the best day your initial method should eventually only be an orchestrator calling these smaller methods that have
easy to understand names. Something like this:

```javascript
const async initApp = () => {
  setAppLoadingStatus(true);
  await loadUITranslations();
  await loadUserPermissions();
  if (getUserPermissions().hasDatabaseAccess) {
    await loadDatabaseTableStructureInfo();
  } else {
    setAppNotAvailable();
  }
  setAppLoadingStatus(false);
}
```

Writing unit-tests is another two-way sword that can be used to detect too lengthy methods. If a method is not well
testable or you know the test will break the moment someone changes anything, then Your method that you are testing is 
probably not enough "single purpose" (or is the orchestration method).

## 3. Keep one export per file (unless you know what you are doing)

This is not quite a "follow it blindly" suggestion. It greatly depends on what the purpose of the file is. If it is
a collection of types or constants, then the rule does not apply usually. But it is still a good practice to question
yourself if you are not doing something wrong before writing the second `export` statement to the same file as
multiple exports from one file is not problem free and can cause a lot of unexpected code leakage.

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

To fix this, You simple need to separate the `export const buttonStyles = ['primary', 'secondary'];` to a separate 
file for constants of the module.

## 4. Refactor the long file to a directory with multiple files

Now when you have split all lengthy methods and multi-boolean expressions into smaller chunks, you should make a 
directory with the same name that your function was, so all these small methods now have a place to exist as separate 
file. In node compiled projects, if You do not want to start changing all the places in code where the main method was
imported, you can have a file named `index.js` (`index.ts` if TypeScript) in your directory exporting the main method.
so a directory like this.

```
ListModule/
-- index.ts
-- types.ts
-- DataRow.ts
-- HeaderRow.ts
```

Will still be imported in other files as:

```javascript 
import ListModule from './ListModule';
```

## 5. React really likes you if you make the list nodes a separate component.

When rendering lists (array of elements, table rows etc.), consider separating list elements into separate components.
This way when using PureComponent (that you should definitely do), when a property of one element is changed 
React can just update that DOM element instead of rendering the whole list from start. It may be just a small hit if
you do it once, but usually applications grow and list elements become more and more complicated and eventually your
UI is not snappy anymore forcing You to writing a blog post how React pages drain phone batteries.

Good and expected way to use react:
```jsx {.good-code}
class Row extends PureComponent {
  render() {
    return <li>Row nr { this.props.title }</li>;
  }
}

class List extends PureComponent {
  render() {
    return (
      <ul>
        { 
          this.props.titles.map(title => (
            <Row title={ title } key={ title }/> 
          ))
        }
      </ul>
    )
  }
}
```

Bad - not as performant:
```jsx {.bad-code}
class List extends PureComponent {
  render() {
    return (
      <ul>
        { 
          this.props.titles.map(title => (
            <li>Row nr { title }</li>
          ))
        }
      </ul>
    )
  }
}
```
Separating sub-components also allows giving them a pass-through `id` property that makes event handlers code much
better on re-renders (does not force re-renders as functions are not re-generated on the go) and better for
readability too.

Bad:
```jsx {.bad-code}
handleClick = (id) => {
  // do sth
}

render() {
  // On every render the onclick handler becomes a new instance forcing rerender of sub component if parent changes
  // even if the change is not related to list
  return elements.map(element => (
    <button
      onClick={ this.handleClick.bind(element.id) }
    >
      { element.title }
    </button>
  ));
}
```

Good:
```jsx {.good-code}

class Element extends PureComponent {
  handleClick = () => {
    this.props.onClick(this.props.id);
  }

  render() {
    return <button
      onClick={ this.handleClick }
    >
      { this.props.children }
    </button>
  }
}

render() {
  // If parent changes only these sub-components render that actually have renders 
  return elements.map(element => (
    <Element
      id={ element.id }
      onClick={ this.handleClick }
    >
      { element.title }
    </Element>
  );
}
```

This kind of separation makes adding new features and improvements a lot easier too, as now list render does not 
have to as much about the list elements themselves and each list element just has to know about itself and can be even
connected directly to datastore, making data and callbacks to parent passing clutter un-necessary.

```jsx
// Row.js
class Row extends PureComponent {
  handleRowClick = () => {
    this.props.selectRow(this.props.id);
  }

  render() {
    return (
      <div onClick={ this.handleRowClick }>
        Row { this.props.title }
      </div>
    );
  }
}

const mapStateToProps = (state, ownProps) => {
  const { id } = ownProps;
  return selectRowFromState(state, id);
};

const mapDispatchToProps = (dispatch) => ({
  selectRow: (id) => dispatch({ type: 'SELECT_ROW', id }),
});

export default connect(mapStateToProps)(Row);
```

```jsx
// List.js
class List extends PureComponent {
  render() {
    return (
      <div>
        { this.props.rowsIds.map(id => <Row id={ id } key={ id }/>) }
      </div>
    );
  }
}
```