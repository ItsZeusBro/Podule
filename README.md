# Podule

A place for my Node Module and Package Management scripts and notes (these notes will become less important overtime, and will be replaced with use case design for the scripts)

## Packages

Package.json files describe file trees as described by the Node docs. It's useful to think about each package.json file as its own seperate tree, suggesting the leaf node of a package is the last directory BEFORE entering a new directory with a pacakge.json or node_modules.

### In NodeJS there are two types of Module Specifier Resolution Algorithms:
1. ECMA Script ESM_RESOLVE(specifier, parentURL)
2. Common JS Require()

### Common JS Require Pseudocode
require(X) from module at path Y
- IF X is a core module,
  -  return the core module
  -  STOP
- IF X begins with '/'
   - set Y to be the filesystem root
- IF X begins with './' or '/' or '../'
   - LOAD_AS_FILE(Y + X)
   - LOAD_AS_DIRECTORY(Y + X)
   - THROW "not found"
- IF X begins with '#'
   - LOAD_PACKAGE_IMPORTS(X, dirname(Y))
- LOAD_PACKAGE_SELF(X, dirname(Y))
- LOAD_NODE_MODULES(X, dirname(Y))
- THROW "not found"

### ECMA_RESOLVE Pseudocode
- LET resolved be undefined.

- IF specifier is a valid URL, then
  - Set resolved to the result of parsing and reserializing specifier as a URL.
- ELSE IF specifier starts with "/", "./", or "../", then
  - Set resolved to the URL resolution of specifier relative to parentURL.
- ELSE IF specifier starts with "#", then
  - Set resolved to the result of PACKAGE_IMPORTS_RESOLVE(specifier, parentURL, defaultConditions).
- ELSE,
  - Note: specifier is now a bare specifier.
  - Set resolved the result of PACKAGE_RESOLVE(specifier, parentURL).
  
- LET format be undefined.

- IF resolved is a "file:" URL, then
  - IF resolved contains any percent encodings of "/" or "\" ("%2F" and "%5C" respectively), then
    - Throw an Invalid Module Specifier error.
  - IF the file at resolved is a directory, then
    - Throw an Unsupported Directory Import error.
  - IF the file at resolved does not exist, then
    - Throw a Module Not Found error.
  - SET resolved to the real path of resolved, maintaining the same URL querystring and fragment components.
  - SET format to the result of ESM_FILE_FORMAT(resolved).
- ELSE,
  - Set format the module format of the content type associated with the URL resolved.
LOAD resolved as module format, format.


## Local Module Resolution
If you have a local ECMA module somewhere on your system you can export it by making sure your package.json main field points to your export file and your name field represents your module name. Also make sure to specify that it is of type module

We will be using one of my projects as an example (comet):

    "name": "comet",
    "main": "Comet.js",
    "type": "module:
    
If you want to import this ECMA module into another ECMA script you have to create a global symlink with the global node_modules folder. 

You can use:
    npm link 

inside your exported module directory to create a systemwide symlink from the global node_modules folder to your projects path. 

Then you have to go to your importing project root directory and run 

    npm link comet 
    
to tell node that you wish to use the symlinked module inside your importing project. 

In your source file where you wish to import the module, (as per the 'comet' example) use:

    import {Comet} from "comet/Comet.js";

Notice how the pacakge.json name "comet" is used, and remember how we used "npm link comet" from within our importing directory. That is the importance of the module name field in the package.json file. For whatever reason, the "main" field was not able to be infered by node for this example. I had to actually specify the module source file i wanted, but something tells me it should be inferred by the "main". Ohh well, this works fine.
