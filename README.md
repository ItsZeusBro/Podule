# Podule
A place for my Node Module and Package Management scripts and notes

## Packages
Package.json files describe file trees as described by the Node docs. It's useful to think about each package.json file as its own seperate tree, suggesting the leaf node of a package is the last directory BEFORE entering a new directory with a pacakge.json or node_modules.

### ECMA Script modules
- When you have these use cases under your node instance's initial directory you are using ECMA Script modules:
  - Files with an .mjs extension.

  - Files with a .js extension when the nearest parent package.json file contains a top-level "type" field with a value of "module".

  - Strings passed in as an argument to --eval, or piped to node via STDIN, with the flag --input-type=module
### CommonJS modules is the default mode:
- When you are not using any ECMA script features, the package is looked at from a Common JS perspective (using Common JS rules). This suggests that CommonJS is the default, and ECMA modules are an exception to the general rule. 
