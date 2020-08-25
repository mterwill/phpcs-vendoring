# phpcs-vendoring

:link: https://github.com/squizlabs/PHP_CodeSniffer/issues/3056

My organization has several applications that use the same [PHP_CodeSniffer][] standard.
Our code standard is a Composer package that is vendored by each application.
We commit our `vendor/` directory to source control.
We use the [PHPCompatibility][] plugin in our standard.

This repository demonstrates an issue with `installed_paths` when configured in `ruleset.xml`:

```
$ ./my-standard/script/test

FILE: /Volumes/cAseSensitive/phpcs-vendoring/my-standard/poorly_formatted.php
-----------------------------------------------------------------------------
FOUND 3 ERRORS AFFECTING 1 LINE
-----------------------------------------------------------------------------
 3 | ERROR | [ ] Expected "function abc(...)"; found "function abc (...)"
 3 | ERROR | [x] Expected 0 spaces before opening parenthesis; 1 found
 3 | ERROR | [x] Opening brace should be on a new line
-----------------------------------------------------------------------------
PHPCBF CAN FIX THE 2 MARKED SNIFF VIOLATIONS AUTOMATICALLY
-----------------------------------------------------------------------------

Time: 61ms; Memory: 10MB

$ ./my-app/script/test
ERROR: Referenced sniff "PHPCompatibility" does not exist

Run "phpcs --help" for usage information
```


You can see that PHPCompatibility is not correctly loaded with the vendored code standard.

Something like [DealerDirect/phpcodesniffer-composer-installer][] is not a good fit for us,
as since we commit our `vendor/` directory, engineers do not regularly run `composer install`.

My goal is to find a configuration such that anyone can pull down this
repository and run `./my-app/script/test` without error.

[PHP_CodeSniffer]: https://github.com/squizlabs/PHP_CodeSniffer
[PHPCompatibility]: https://github.com/PHPCompatibility/PHPCompatibility
[DealerDirect/phpcodesniffer-composer-installer]: https://github.com/DealerDirect/phpcodesniffer-composer-installer
