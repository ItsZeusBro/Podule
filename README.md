# Podule

A place for my Node Module and Package Management scripts and notes (these notes will become less important overtime, and will be replaced with use case design for the scripts)

## Packages

Package.json files describe file trees as described by the Node docs. It's useful to think about each package.json file as its own seperate tree, suggesting the leaf node of a package is the last directory BEFORE entering a new directory with a pacakge.json or node_modules.

### In NodeJS there are two types of Module Specifier Resolution Algorithms:
1. ECMA Script ESM_RESOLVE(specifier, parentURL)
2. Require()

### Require Pseudocode
require(X) from module at path Y
1. If X is a core module,
  -  return the core module
  -  STOP
2. If X begins with '/'
   - set Y to be the filesystem root
3. If X begins with './' or '/' or '../'
   - LOAD_AS_FILE(Y + X)
   - LOAD_AS_DIRECTORY(Y + X)
   - THROW "not found"
4. If X begins with '#'
   - LOAD_PACKAGE_IMPORTS(X, dirname(Y))
5. LOAD_PACKAGE_SELF(X, dirname(Y))
6. LOAD_NODE_MODULES(X, dirname(Y))
7. THROW "not found"

### ECMA_RESOLVE Pseudocode
1. Let resolved be undefined.
2. If specifier is a valid URL, then
  - Set resolved to the result of parsing and reserializing specifier as a URL.
3. Otherwise, if specifier starts with "/", "./", or "../", then
  - Set resolved to the URL resolution of specifier relative to parentURL.
4. Otherwise, if specifier starts with "#", then
  - Set resolved to the result of PACKAGE_IMPORTS_RESOLVE(specifier, parentURL, defaultConditions).
5. Otherwise,
  - Note: specifier is now a bare specifier.
  - Set resolved the result of PACKAGE_RESOLVE(specifier, parentURL).
6. Let format be undefined.
7. If resolved is a "file:" URL, then
  - If resolved contains any percent encodings of "/" or "\" ("%2F" and "%5C" respectively), then
    - Throw an Invalid Module Specifier error.
  - If the file at resolved is a directory, then
    - Throw an Unsupported Directory Import error.
  - If the file at resolved does not exist, then
    - Throw a Module Not Found error.
  - Set resolved to the real path of resolved, maintaining the same URL querystring and fragment components.
  - Set format to the result of ESM_FILE_FORMAT(resolved).
8. Otherwise,
  - Set format the module format of the content type associated with the URL resolved.
9. Load resolved as module format, format.
