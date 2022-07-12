# Podule

A place for my Node Module and Package Management scripts and notes (these notes will become less important overtime, and will be replaced with use case design for the scripts)

## Packages

Package.json files describe file trees as described by the Node docs. It's useful to think about each package.json file as its own seperate tree, suggesting the leaf node of a package is the last directory BEFORE entering a new directory with a pacakge.json or node_modules.

### ECMA Script modules

- When you have these use cases under your node instance's initial directory you are using ECMA Script modules:
  - Files with an .mjs extension.

  - Files with a .js extension when the nearest parent package.json file contains a top-level "type" field with a value of "module".

  - Strings passed in as an argument to --eval, or piped to node via STDIN, with the flag --input-type=module


### CommonJS modules is the default mode:

- When you are not using any ECMA script features, the package is looked at from a Common JS perspective (using Common JS rules). This suggests that CommonJS is the default, and ECMA modules are an exception to the general rule. 

- It is still preferable (best practice) that you be more explicit because ECMA Script seems to be taking over a bit more. To specify explicitly that you are using Common JS modules for a package tree, use the following:
  - Files with a .cjs extension.
  - Files with a .js extension when the nearest parent package.json file contains a top-level field "type" with a value of "commonjs".
  - Strings passed in as an argument to --eval or --print, or piped to node via STDIN, with the flag --input-type=commonjs.

From the documentation:
- Package authors should include the "type" field, even in packages where all sources are CommonJS. Being explicit about the type of the package will future-proof the package in case the default type of Node.js ever changes, and it will also make things easier for build tools and loaders to determine how the files in the package should be interpreted.

### Module Loaders:
Just in time compilation does not seem to have a static executable file. It looks like it exists in RAM during runtime only and is cleaned up before returning from the program. This means the loader acts like a realtime linker that links modules as needed.
