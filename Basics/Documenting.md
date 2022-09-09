# Documenting

<img src="https://geekandpoke.typepad.com/.a/6a00d8341d3df553ef0168eabe2192970c-pi" width="500">

> ## And so the habit of not even looking for a documentation begins.

There are many types of documentation:
* for end user
*  for client
* for sysadmin
* for another developer
* etc.

It is not a good idea to try to write one documentation that covers it all as the targets and thus the purpose of
documentation is completely different. This section exclusively focuses on documentation that is strictly targeted for 
other developers and analysts.

## The purpose of developer-to-developer documentation

When applications grow, so does the internal know-how about architecture and business decisions that is not really 
readable and deducible from the code and comment's alone. Also nobody has the time to actually read through and remember
the whole codebase to get the info that is deducible but scattered. In time very project gathers a pile of good ideas 
for the future that don't yet exist in the codebase, but should be left room for in the future.

Examples of what usually needs to have a separate documentation:

* Guides for new developer to get everything up and running.
* Application configuration.
* General application architecture plan and agreed upon decisions.
  * Basically everything related to business problems.
  * General data flow diagrams.
  * Explanatory guide for architecture. Programming architecture is more of a belief and gut feeling, that it will
    scale for us long term, than actual math. It will remain so until we will learn to predict the future. 
  * Style guides, naming conventions and such.
* Glossary - See [Establishing a naming convention](Naming_functions_and_variables.md).
* Architecture and data flow descriptions of sub-modules, that need to be linked from multiple places in code 
  or is just too lengthy to be written into comment.

The main purpose of a good documentation is not just to save time when onboarding new people, but also to:

 * Have a reference point for old developers to share with others, or just freshen their memory.
 * Act as a FAQ list to link and point to in internal discussions, code reviews, "how was that configured again?" questions etc.
 * Prevent code that conflicts with already established business logic or data-flows.
 * Make testing your code more beneficial and easier (testers can actually find and read markdown changes! I've seen it!)


But one of the biggest benefits that documentation gives is gained at the time of writing. When a new feature is in
design phase I often find that it is easiest to find problems and bugs in logic and architecture when using 
documentation writing as [Rubber duck debugging](https://en.wikipedia.org/wiki/Rubber_duck_debugging).
## Findable up-to-date documentation

Every good project has a substantial set of additional information that needs to live somewhere.
It is extremely obvious it needs to be besides the code, so it would be updated as code get updated.
At the same time it needs to be gathered to one simple-to-find place so everybody could access it. These two
requirements are often contradictory and result in beliefs like:

> There is no point in writing documentation as it get outdated quickly!

> There is no point in writing documentation as nobody reads it anyway!

Following is not something of a magic bullet, but a thing that seems to work for me currently, to tackle both of
these problems.






