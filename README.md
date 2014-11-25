# `version`: functions module for [shellfire]

This module provides a simple framework for comparing software versions in a [shellfire] application. An example user is the [curl] module, which uses it to check for version-specific support. As a rule-of-thumb, version checking for features is fraught with pitfalls (not least because of out-of-tree patches, distribution-specific adjustments and the challenge of parsing versions from help strings). However, it is sometimes unavoidable.

The version comparison used in this module assumes numeric, hyphen or period delimited versions and should handle release candidates\*. However, given the range of versioning strategies, it is not a generic cure all. Caveat emptor.

\* Unfortunately, we can't use GNU coreutils' `sort --version` as it is not widely supported.

## Overview

For example let's say you want to use the option `--netrc-file` with `curl`. For that to work, you'll need at least `curl` version `7.21.5`:-

```bash
curlVersion="$(curl --version)"

if version_isGreaterThanOrEqualTo "$curlVersion" '7.21.5'; then
	curl_supportsNetrc='yes'
else
	curl_supportsNetrc='no'
fi
```

## Importing

To import this module, add a git submodule to your repository. From the root of your git repository in the terminal, type:-
```bash
mkdir -p lib/shellfire
cd lib/shellfire
git submodule add "https://github.com/shellfire-dev/version.git"
cd -
git submodule init --update
```

You may need to change the url `https://github.com/shellfire-dev/version.git` above if using a fork.

You will also need to add paths - include the module [paths.d].

## Namespace `version`

This namespace exposes functions to compare versions. All of the functions in this module are intended to be used with the shell's `if` statement or `&&` operators, and return exit codes to that effect. Calling a function outside of such a statement in [shellfire] will cause your program to exit.

### To use in code

If calling from another [shellfire] module, add to your shell code the line
```bash
core_usesIn version
```
in the global scope (ie outside of any functions). A good convention is to put it above any function that depends on functions in this module. If using it directly in a program, put this line _inside_ the `_program()` function:-

```bash
_program()
{
	core_usesIn version
	…
}
```

### Functions

***
#### `version_isLessThan`

|Parameter|Value|Optional|
|---------|-----|--------|
|`leftVersion`|Left-hand version.|_No_|
|`rightVersion`|Right-hand version.|_No_|

Compares as if `leftVersion < rightVersion`. Returns an exit code of `0` for true and `1` for false.

***
#### `version_isEqualTo`

|Parameter|Value|Optional|
|---------|-----|--------|
|`leftVersion`|Left-hand version.|_No_|
|`rightVersion`|Right-hand version.|_No_|

Compares as if `leftVersion == rightVersion`. Returns an exit code of `0` for true and `1` for false.

***
#### `version_isGreaterThan`

|Parameter|Value|Optional|
|---------|-----|--------|
|`leftVersion`|Left-hand version.|_No_|
|`rightVersion`|Right-hand version.|_No_|

Compares as if `leftVersion > rightVersion`. Returns an exit code of `0` for true and `1` for false.

***
#### `version_isLessThanOrEqualTo`

|Parameter|Value|Optional|
|---------|-----|--------|
|`leftVersion`|Left-hand version.|_No_|
|`rightVersion`|Right-hand version.|_No_|

Compares as if `leftVersion <= rightVersion`. Returns an exit code of `0` for true and `1` for false.

***
#### `version_isGreaterThanOrEqualTo`

|Parameter|Value|Optional|
|---------|-----|--------|
|`leftVersion`|Left-hand version.|_No_|
|`rightVersion`|Right-hand version.|_No_|

Compares as if `leftVersion >= rightVersion`. Returns an exit code of `0` for true and `1` for false.

***
#### `version_compare`

|Parameter|Value|Optional|
|---------|-----|--------|
|`leftVersion`|Left-hand version.|_No_|
|`rightVersion`|Right-hand version.|_No_|

Compares as if `leftVersion` to `rightVersion`. Returns an exit code of `0` for less than,  `1` for equal to and `2` for greater than. Note that this differs to languages such as Java, since exit codes can not be non-zero in POSIX shell code.



[swaddle]: https://github.com/raphaelcohn/swaddle "Swaddle homepage"
[shellfire]: https://github.com/shellfire-dev "shellfire homepage"
[core]: https://github.com/shellfire-dev/core "shellfire core module homepage"
[paths.d]: https://github.com/shellfire-dev/paths.d "paths.d shellfire module homepage"
[github api]: https://github.com/shellfire-dev/github "github shellfire module homepage"
[jsonwriter]: https://github.com/shellfire-dev/jsonwriter "jsonwriter shellfire module homepage"
[jsonreader]: https://github.com/shellfire-dev/jsonreader "jsonreader shellfire module homepage"
[urlencode]: https://github.com/shellfire-dev/urlencode "urlencode shellfire module homepage"
[version]: https://github.com/shellfire-dev/version "version shellfire module homepage"
