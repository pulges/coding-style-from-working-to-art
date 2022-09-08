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

## Glossary

Glossary is often forgotten when talking about naming conventions but it is one of the places that can really make a 
huge difference in codebase. Probably everybody has been at least once been hit with code, that does not make sense,
but somehow magically works. Then, after hours of debugging finding that the name `attribute` (or whatever the name
at hand) means really very different things in different but rather close cases by function.

From my own practice we had this really bad `attribute` over-use case, where eventually it might refer to:
  
* id of column in database
* object of column in that database
* a value id for a column in that database
* value object or translation
* sometimes even some other property not related to database columns and values at all.

We started fixing it by dropping the name "attribute" all-together and establishing in the glossary:

* `columnId` - string - id of column in database
* `valueId` - a value id for a column in that database

This also left some room for names "column" and "value" to be used more freely inside functions for whatever type needed
at the moment.

Establishing a project-wide glossary and giving some names single fixed definitions, helps not only in code readability,
but also in all communication. The mayor hurdle though is keeping the documentation in an easily accessible place and
enforce not only developers, but also project managers and analysts, to use it in all communication.