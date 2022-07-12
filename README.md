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

## Module Loaders:
Just in time compilation does not seem to have a static executable file. It looks like it exists in RAM during runtime only and is cleaned up before returning from the program. This means the loader acts like a realtime linker that links modules as needed. With that being said, there are two loaders for the two different Module support types:

### CommonJS Loaders
Top level is from the docs. My annotations underneath are not required to be accurate here (for now):
- It is fully synchronous.
  - Modules and sub components are loaded syncronously as they are called upon. This means one can predict which modules will be loaded first.
- It is responsible for handling require() calls.
- It is monkey patchable.
- It supports folders as modules.
- When resolving a specifier, if no exact match is found, it will try to add extensions (.js, .json, and finally .node) and then attempt to resolve folders as modules.
- It treats .json as JSON text files.
- .node files are interpreted as compiled addon modules loaded with process.dlopen().
- It treats all files that lack .json or .node extensions as JavaScript text files.
- It cannot be used to load ECMAScript modules (although it is possible to load ECMASCript modules from CommonJS modules). When used to load a JavaScript text file that is not an ECMAScript module, it loads it as a CommonJS module.


### ECMA Script Loaders
