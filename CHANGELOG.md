# Changelog

All notable changes to this project will be documented in this file.
The format is based on [Keep a Changelog].

## Unreleased
### New features
* `el-patch-let` now allows more than one body form; all body forms
  are spliced into the containing s-expression. This is fully
  backwards compatible.
* `el-patch-literal` always allowed more than one body form, but this
  fact is now documented.
* The new patch directive `el-patch-concat` may be used to modify
  string literals in a patch without repeating their contents twice
  ([#14]).
* You may now define functions analogous to `el-patch-defun` for your
  own definition types using the new `el-patch-deftype` macro, which
  operates on the new user option `el-patch-deftype-alist` ([#24]).
* `el-patch` now provides integration with `use-package`, controlled
  by the variable `el-patch-enable-use-package-integration` and the
  minor mode `el-patch-use-package-mode`. Two new keywords,
  `:init/el-patch` and `:config/el-patch`, are defined ([#25]).

### Enhancements
* When a definition is patched, `el-patch` now automatically appends a
  note to the end of the docstring indicating that a patch was made
  ([#14]).

### Bugfixes
* Using `el-patch-let` could previously cause a circular list error.
  For example:

      (el-patch-let ((x y)) (x x x))

  That has now been fixed.

[#14]: https://github.com/raxod502/el-patch/issues/14
[#24]: https://github.com/raxod502/el-patch/issues/24
[#25]: https://github.com/raxod502/el-patch/issues/25

## 2.0.1 (released 2018-06-21)
### Internal changes
* `el-patch` now uses lexical binding.
* `el-patch` is now tested on all supported Emacs versions.
* `el-patch` no longer uses the deprecated [`elint`][elint] testing
  framework.

## 2.0 (released 2018-05-10)
### Changed
* `el-patch` can now be used in your init-file without being loaded at
  runtime. In other words, the patch definition macros expand, at
  compile time, to code that does not reference `el-patch` ([#11]).
* `el-patch` now uses [`elint`][elint] to run its tests and stylistic
  checks. Lines longer than 80 characters now fail the build.
* `el-patch` forms now have a well-defined return value, namely the
  same as would have been returned by the corresponding non-`el-patch`
  form ([#15]).

### Fixed
* `el-patch` previously failed with an error if you attempted to patch
  an object which used a literal cons cell or improper list in its
  definition. This has been fixed ([#13]).

## 1.2 (released 2017-07-23)
### Added
* `el-patch` now has a Makefile for convenient byte-compilation,
  linting, and table-of-contents updating.
* `el-patch` now checks for byte-compilation and checkdoc warnings on
  Travis CI ([#7]).
* New macro `el-patch-feature` (unrelated to the old obsolete
  `el-patch-feature` macro that was removed) which is a convenient way
  to add feature-requiring functions to `el-patch-pre-validate-hook`
  ([#10]).
* New user option `el-patch-require-function` for advanced users only.
  It allows you to integrate `el-patch-feature` with your package
  manager, and other things like that.

### Removed
* Deprecated patch directive `el-patch-feature` has been removed.

### Fixed
* Messages emitted by `el-patch-validate-all` now have correct
  pluralization ([#9]).
* The documentation on `el-patch-validate` was not updated to reflect
  its new signature in version 1.1.1. This has now been fixed.

## 1.1.2 (released 2017-06-05)
### Added
* Patches can now be defined for more types of objects:
  * `el-patch-defconst`
* Custom group for `el-patch`.
* New user option `el-patch-aggressive-defvar` to allow overriding the
  existing value of a variable.

### Changed
* Variables `el-patch-pre-validate-hook` and
  `el-patch-post-validate-hook` are now Custom variables.

### Fixed
* Since version 1.1, the "But how does it work?" section of the README
  contained an erroneous explanation implying that `set` and `fset`
  were used to establish patched definitions. This has been replaced
  with a correct explanation.

## 1.1.1 (released 2017-02-27)
### Changed
* The function `el-patch-validate` now takes a patch name and type
  rather than a patch definition. The remaining arguments `NOMSG` and
  `RUN-HOOKS` have not changed.
* The meaning of the prefix argument has been inverted for
  `el-patch-validate`. Now it runs `el-patch-pre-validate-hook` and
  `el-patch-post-validate-hook` by default, and only omits doing so if
  a prefix argument is provided.

### Fixed
* Since version 1.1, `el-patch-validate` failed with an error in
  `el-patch--classify-definition-type`. This has been fixed
  ([#8]).
* Since version 1.1, if a patch was defined in the same file as the
  function it patched, the patch could not be validated. This has been
  fixed.
* Previously, when `el-patch-validate` was called noninteractively
  with a non-nil value for `RUN-HOOKS`, it failed to run
  `el-patch-pre-validate-hook`. This has been fixed.

## 1.1 (released 2017-02-11)
### Added
* Patches can now be defined for more types of objects:
  * `el-patch-defvar`
  * `el-patch-defcustom`
  * `el-patch-define-minor-mode`
* New function `el-patch-get` to get a patch definition by name.
* Objects can now be renamed in a patch using `el-patch-swap` on the
  name ([#4]).
* Actual description in the Commentary section of `el-patch.el`
  ([#5]).
* MIT license ([melpa/melpa#4512]).
* Arbitrary functions can be run (to load patches) by adding things to
  the new hook `el-patch-pre-validate-hook`. Cleanup can then be done
  in `el-patch-post-validate-hook`.

### Changed
* It is no longer necessary to specify `el-patch-feature` due to
  improvements in the `el-patch` patching mechanism.
* The functions `el-patch-ediff-patch`, `el-patch-ediff-conflict`, and
  `el-patch-unpatch` now take an object name and definition type
  rather than a patch definition (or function name in the case of
  `el-patch-unpatch`). Interactive usage has not changed.

### Deprecated
* As a result of the aforementioned improvements, `el-patch-feature`
  is now a deprecated no-op.

### Fixed
* `el-patch-splice` previously acted the same as `el-patch-wrap`. Now
  it functions correctly.
* The patch definition macros now have their docstrings colored
  correctly by the syntax highlighter (by specifying `(declare
  (doc-string N))`).
* `.dir-locals.el` is now ignored when validating patches ([#2]).

## 1.0 (released 2017-01-21)
### Added
* Public API functions:
  * Functions for defining patches:
    * `el-patch-defun`
    * `el-patch-defmacro`
    * `el-patch-defsubst`
  * Patch directives:
    * `el-patch-add`
    * `el-patch-remove`
    * `el-patch-swap`
    * `el-patch-wrap`
    * `el-patch-splice`
    * `el-patch-let`
    * `el-patch-literal`
    * `el-patch-feature`
  * Functions for validating patches:
    * `el-patch-validate`
    * `el-patch-validate-all`
  * Functions for viewing patches:
    * `el-patch-ediff-patch`
    * `el-patch-ediff-conflict`
  * Functions for removing patches:
    * `el-patch-unpatch`

[#2]: https://github.com/raxod502/el-patch/issues/2
[#4]: https://github.com/raxod502/el-patch/issues/4
[#5]: https://github.com/raxod502/el-patch/issues/5
[#7]: https://github.com/raxod502/el-patch/issues/7
[#8]: https://github.com/raxod502/el-patch/issues/8
[#9]: https://github.com/raxod502/el-patch/issues/9
[#10]: https://github.com/raxod502/el-patch/issues/10
[#11]: https://github.com/raxod502/el-patch/pull/11
[#13]: https://github.com/raxod502/el-patch/issues/13
[#15]: https://github.com/raxod502/el-patch/issues/15

[melpa/melpa#4512]: https://github.com/melpa/melpa/pull/4512#issuecomment-274682089

[elint]: https://github.com/raxod502/elint
[keep a changelog]: [Keep a Changelog]: http://keepachangelog.com/
