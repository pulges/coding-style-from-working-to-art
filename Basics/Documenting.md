# Documenting

![documentation](https://geekandpoke.typepad.com/.a/6a00d8341d3df553ef0168eabe2192970c-pi)

> ## And so the habit of not even looking for a documentation begins.

There are many types of documentation: for end user, for client, for sysadmin for another developer etc. It is not a good
idea to try to write one documentation that covers it all as the targets and thus the purpose of documentation is
completely different. This section exclusively focuses on documentation that is strictly targeted for other developers
and analysts.

When applications grow, so does the internal know-how about architecture and business decisions that is not really 
readable and deducible from the code and comment's alone. Also nobody has the time to actually read through and remember
the whole codebase to get the info that is deducible but scattered. In time very project gathers a pile of good ideas 
for the future that don't yet exist in the codebase, but should be left room for in the future.

Examples of what usually needs to have a separate documentation:

* Guides for new developer to get everything up and running.
* General application architecture plan and agreed upon decisions.
  * Basically everything related to business problems.
  * General data flow diagrams.
  * Explanatory guide for architecture. Programming architecture is more of a belief and gut feeling, that it will
    scale for us long term, than actual math. It will remain so until we will learn to predict the future. 
  * Style guides, naming conventions and such.
* Glossary - See [Establishing a naming convention](Naming_functions_and_variables.md).
* Architecture and data flow descriptions of sub-modules, that need to be linked from multiple places in code 
  or is just too lengthy to be written into comment.

Every good project has a substantial set of this kind of information that needs to live somewhere, extremely obviously,
besides the code, so it would be updated as code get updated, but at the same needs to gathered to one simple to find
place.

