# Documenting and commenting

Countless times I've heard this *cliché* phrase:

> “Good code is self-documenting.”

Though there is some truth to it, it has been mostly abused, without actually knowing what it means, as an excuse 
to skip commenting and documenting. Even the best self-documenting code needs additional documentation and commenting.

Very often this phrase is used referring to really bad and toxic examples of code commenting.
I'm referring to commenting the obvious:

```javascript
// Declare count variable
let count = 0;

// loop 5 times
for (let i = 1; i <= 5; i++) {
  
  // increase count by 1
  count++;

}

// An array of names
let a = ["Alice", "Bob", "Charlie"]
```

**Do not do this!** All these comments are just useless and meaningless as they just duplicate the code that can be
as easily be read from code itself. 

Instead here are some examples of where and how to use commenting so it would have a purpose and actually complement
Your code.
## 1. Provide some "Insider" information

Providing some information about why the code exists, that can not be read directly from the code, is one of the best
ways comments can be utilized. Very often these places in code are overlooked, as the writer of the code has just read
the task at hand, where the same lines are probably present. Thus it seems un-necessary to repeat them here.

This is a mistake though, as another developer later reading the code needs to find the task that these lines refer to
from project management system or just plain guess and hope he's right. This generates a lot of wasted time and
interrupts the code reading flow. Also some the might have been fixed or improved by later commits, making finding
the initial task really displeasing. 

**Good examples of comments:**

  ```typescript
  /**
   * Returns if user has sufficient permissions for given app id (true/false)
   * If appId is configured in `api/rw/2/conf/shared/applications` but has no `rolesAll` or `rolesAny`, returns `true`.
   * If app is not configured in `api/rw/2/conf/shared/applications`, returns `true`.
   */
  function* isValidApp(appId: string): Generator<any, boolean, any> {
    //...
  ```

  ```javascript

  /** Missing value criteria:
  *
  * `columnId` has translations (object not nullish)
  * `columnId` has `values` translations present (non nullish)
  * `valueId` is missing in `values` list
  * 
  * For columns that are untranslatable (defined by `values` parameter nullish)
  * we currently do not detect and show missing values in UI.
  **/
  let valueIsMissing = false;
  if (attributeTranslation?.values) {
    if (isTranslationValue(value)) {
      valueIsMissing = !(
        Object.values(attributeTranslation.values).find(v => v.translation === value.translation)
      );
    } else {
      valueIsMissing = !(attributeTranslation?.values?.[value.valueId]);
    }
  }
  ```

  Very often the reason finding is skipped by next developer and blind updates and fixes are made to code using just 
  guesswork. Keep in mind that whoever reads it can see what it does, but may not know if it is what the code should do, 
  in case there is an error in code, and why. This can result in conditions and pieces of code left behind that are
  never utilized or even do something plain stupid. These left-behind lines will make the code even harder to understand
  for next developers snowballing into a disaster. Also it would be much harder for another developer to see that maybe
  the task at hand is already conflicting with existing code.

## 2. Provide short summary for a lengthy block
 
Comments can also be used to shorten code reading time and improve discover-ability by providing reader with short
insight to what a lengthy method or piece of code does. With good short comment, reading through the whole code could be
skipped to understand its function.

```javascript
/** 
 * Does 1d breakdowns from database for getting `population` values and 
 * converts results from slow to handle array formats to hashmaps for fast access.
 */
function breakdown1DAsMap(breakdownColumnIds, filter) {
  // ...
}
```