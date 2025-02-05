# Interledger.JS Monorepo

> This is a WIP and will ultimately replace a number of stand-alone modules

## Background

Interledger.JS has a long history of modules that have been added as experiments and abandoned or deprecated and replaced by new versions or alternatives. Since late 2018 a few key modules have stabilized and become key dependencies for various others.

For [a while](https://forum.interledger.org/t/interledgerjs-monorepo/318) the community has been keen to put many of the core modules into a single monorepo.

## Design Decisions

 - Use lerna and yarn workspaces.
 - Keep it simple. Minimal custom stuff.
 - DRY. Where possible avoid repetition in packages
 - Use the @interledger scope instead of `ilp` prefix in package names.
 - Use package categories `utils`, `codecs`, `protocols` (others may be added) instead of naming convention.
 - New package names allow for some refactoring to do away with legacy hacks to accommodate JS module funnies.
 - Switch to synchronized versioning.
 - Switch to eslint from tslint (See: https://eslint.org/blog/2019/01/future-typescript-eslint).

## Process

 1. Setup a monorepo framework with a few small modules
 2. [Import](#importing) existing modules one by one and deprecate the old module
 3. Rename the module to fit the new structure and naming
 4. Update the new package to use the same build, test and linting scripts as others
 5. Remove dev dependencies
 6. Update dependencies and imports to use new module names
 7. Fix linting or build errors

## Importing

This process preserves the commit history of the legacy modules.  
_(This assumes the module being imported should go into `protocols`, as opposed to a `codecs` or `utils`)_

```sh
git clone git@github.com:adrianhopebailie/interledgerjs.git
git clone git@github.com:interledgerjs/legacy-module.git
cd legacy-module
git pull
cd ../interledgerjs
lerna import ../legacy-module --dest=packages/protocols --preserve-commit --flatten
```

## Scripts

- postinstall : Ensure the repo is ready to go after install
- clean: Clean everything
- clean:artifacts: Recursively clean packages
- clean:packages: Run `lerna clean --yes`
- clean:root: Remove root `node_modules`
- build: Build each package in topological order
- test: Run tests in each package
- test:quick: Run tests in each package (in parallel)
- cover: Run test coverage in all packages
- prepare:release: Run `lerna version`
- prepare:prerelease: Prepare a prerelease (alpha)
- publish:canary: Run `lerna publish --canary`
- publish:release: Run `lerna publish from-package`
- lint: Lint the provided files (requires a glob param)
- lint:all: Lint everything
- lint:staged: Run `lint-staged`