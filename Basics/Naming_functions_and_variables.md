# Establish a naming convention

A good naming convention consists of 2 parts:

  * Rules of notation
  * Glossary

## Rules of notation

Rules of notation mostly focuses on the shape of identifiers when defining variables, classes, methods etc. It varies greatly
between languages and also between teams. The idea is to transfer additional information via shape to reader, making
reading code faster.

In javascript one of the most common naming notation set is:

  * Variable and function names written as *camelCase*
    ```javascript
    let numberCounter = 0;
    ```
  * Global constants written in *UPPERCASE*
    ```javascript
    const DEFAULT_VALUE = 'none';
    ```
  * Classes and functions that are intended to be used as prototypes for instances are written in Pascal case.
    This leaves room for same string variable.

    ```typescript
    Class Person {
      name: string;

      constructor(name: string) {
        this.name = name;
      }
    };
    const preson = new Pesron('John');

    const Color(red:number, green: number, blue: number){
      this.red = red;
      this.green = green;
      this.blue = blue;
    };
    let color = new Color(100, 100, 100);
    ```

For CSS classes there are many conventions used, but one very popular is BEM and BEM like extended syntaxes.

General BEM spec: [https://en.bem.info/methodology/quick-start/](https://en.bem.info/methodology/quick-start/)

Example of an extended syntax in a library (prefixed with library namespace `dgc__`):

```css
/** Top level element:
 *
 * Schema:**  `.dgc__{element}`
 */
.dgc__dropdown-list {}

/** Child elements
 *
 * Schema:** `.dgc__{element}__{subelement}`
 */
.dgc__dropdown-list__row {}

/** Status effects. Things like `active`, `selected`, `open`
 *
 * Schema:** `.dgc__{element}_{status}`
 */
.dgc__dropdown-list__row_selected {}
.dgc__collapsible-window_open {}

/** Style overlay / modifier
 *
 * Schema:** `.dgc__{ element }--{styleModifier}`
 */
.dgc__dropdown-list--borderless {}
.dgc__primary-button--blue  {}
```

The choice of notation convention can be a really controversial issue among developers, and each has probably his
favorite and also good reasons to back it up. But keep in mind when opening an already existing codebase, it a very good
idea to familiarize Yourself with the naming convention used and keep using it even if you like something else.
Only consistent naming convention enjoys the benefits:

* Easier to see to what use an identifier is put (ex: you know you'll have to initialize a class to get an instance).
* Similar shapes make custom identifiers better pop out among language native code
* When namespaces are used, it avoids naming collisions among libraries, modules, product parts etc.
* Aesthetic looks - remember its art and aesthetics are important in art!
* Benefits when refactoring when using string find/replace
* Sometimes, like in the making instance from class case, it allows leaving room for same name to be used.

To simplify new developer on-boarding and avoid useless hours arguing about naming notations, it is a good idea to
dedicate a section for it in projects README.md files or in company wide style-guide (or both).

## Extended rules for notation

To extend this common rules set and really give it some shine, I suggest adding additional (but suggested not mandatory)
guidelines to naming to make reading even faster and simpler. 

### Booleans and functions returning booleans

There is a convention to prefix boolean variables and function names with "is" or "has".
The convention is not a must, but a good starting point that often improves readability. 

```javascript
  if (isLoggedIn) {};
  if (hasAccess) {};
  if (isLoading()) {};
```

Avoid negations in boolean naming as it deeply impacts code readability.

Bad:
```javascript
  if (!account.isDisabled) {
    // ...
  }
```

```javascript
  const isNotGreen = true;
  if (!isNotGreen) {
    // ...
  } else {
    // ...
  }
```

Good:
```javascript
  if (account.isEnabled) {
    // ...
  }
```

```javascript
  const isGreen = false;
  if (isGreen) {
    // ...
  } else {
    // ...
  }
```

It is a good idea to name undefined/nullish check booleans using `is{ SOMETHING }Present` schema.

  ```javascript
    const isUserPresent = Boolean(user);
  ```

### Event handlers

### Prefixing methods, properties and variables for internal use.

### 

## Establish a glossary

Glossary is often forgotten when talking about naming conventions but it is one of the places that can really make a 
huge difference in codebase. Probably everybody has been at least once been hit with code, that does not make sense,
but somehow magically works. Then, after hours of debugging, finding that the name `attribute` (or whatever the name
at hand) means really very different things in different but rather close cases by function.

From my own practice we had exactly this really bad `attribute` over-use case, where eventually it might refer to:

* Id of a column in database.
* Object of a column in that database.
* A value id for a column in that database.
* Value object or translation.
* Occasionally even some other property not related to database columns and values at all.

We started fixing it by dropping the name "attribute" all-together and establishing a glossary:

* `columnId` - string - id of column in database.
* `valueId` - a value id for a column in that database.

This also left some room for identifiers "column" and "value" to be used more freely inside function scope for whatever
type needed at the moment.

The biggest benefit though can be gained when establishing a project-wide glossary and giving some names single fixed
definitions. It helps not only in code read-ability, but in all communication, fixing some misunderstandings in tasks, 
making documentations easier to read, etc. The mayor hurdle though is keeping the glossary in an easily accessible place,
up to date and and enforcing not only developers, but also project managers and analysts, to re-read it from time-to-time
and use it in all communication.

Coders spend quite an enormous chunk of their time finding just the right name for identifiers. In some cases more time
is spent on finding the names than actually typing. Thus establishing a glossary can also save a lot of time by taking
some guesswork and doubt that the chosen name actually fits, out of the equation.