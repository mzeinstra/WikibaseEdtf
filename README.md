# Wikibase EDTF

[![GitHub Workflow Status](https://img.shields.io/github/workflow/status/ProfessionalWiki/WikibaseEdtf/CI/master)](https://github.com/ProfessionalWiki/WikibaseEdtf/actions?query=workflow%3ACI)
[![Latest Stable Version](https://poser.pugx.org/professional-wiki/wikibase-edtf/version.png)](https://packagist.org/packages/professional-wiki/wikibase-edtf)
[![Download count](https://poser.pugx.org/professional-wiki/wikibase-edtf/d/total.png)](https://packagist.org/packages/professional-wiki/wikibase-edtf)
[![License](https://img.shields.io/packagist/l/professional-wiki/wikibase-edtf)](LICENSE)

[MediaWiki] extension that adds support for the [Extended Date/Time Format (EDTF) Specification][EDTF] to [Wikibase] via a new data type.

Wikibase EDTF has been made possible with the financial support of the Luxembourg Ministry of Culture.
It is an open source project developed and maintained by [Professional.Wiki]. Contributions are welcome!

You can find a demo of this extension at https://edtf.wikibase.wiki

- [Usage](#usage)
	- [RDF export](#rdf-export)
- [Installation](#installation)
- [Running the tests](#running-the-tests)
- [Release notes](#release-notes)

## Usage

<a href="https://www.youtube.com/watch?v=U5ndjtuDPf8"><img src=".github/youtube.png" width="430px" title="Play video" /></a>

### RDF export

Wikibase EDTF turns EDTF values into standard Wikibase time values that are then given to the native RDF export mechanism. Because Wikibase time values are a lot less expressive, the EDTF values are simplified in this process.

* `EDTF date or time`: Precision and time zone are retained. Qualifications and unspecified digits are discarded.
* `EDTF Set`: Each date in the set is exported.
* `EDTF Season`: One date for each month is exported, each having month precision.
* `EDTF Interval`: Nothing is exported (since there does not seem to be a reasonable default).

For cases where multiple dates are put in the RDF export, like with seasons and sets, there is nothing in the RDF indicating these values logically belong together.

If you can read PHP, you can see the simplification code in [TimeValueBuilder.php](src/Services/TimeValueBuilder.php).

## Installation

Platform requirements:

* [PHP] 7.4 or later, including PHP 8.x
* [MediaWiki] 1.35.x, 1.36.x or 1.37.x
* [Wikibase Repository] REL1_35, REL1_36 or REL1_37

See the [release notes](#release-notes) for more information on the different versions of this extension.

First install MediaWiki and Wikibase Repository.

The recommended way to install Wikibase EDTF is using [Composer] with
[MediaWiki's built-in support for Composer][Composer install].

On the commandline, go to your wikis root directory. Then run these two commands:

If you have MediaWiki 1.37, use `^2.0.0` instead of `^1.2.0`

```shell script
COMPOSER=composer.local.json composer require --no-update professional-wiki/wikibase-edtf:^1.2.0
```
```shell script
composer update professional-wiki/wikibase-edtf --no-dev -o
```

**Enabling the extension**

Then enable the extension by adding the following to the bottom of your wikis "LocalSettings.php" file:

```php
wfLoadExtension( 'WikibaseEdtf' );
```

You can verify the extension was enabled successfully by opening your wiki's "Special:Version" page in your browser.

## Running the tests

* PHP tests: `php tests/phpunit/phpunit.php -c extensions/WikibaseEdtf/`

## Release notes

### Version 2.0.1 - 2022-03-26

* Added missing messages for Special:ListDatatypes

### Version 2.0.0 - 2022-01-23

* Added support for MediaWiki and Wikibase 1.37
* Raised minimum MediaWiki and Wikibase versions to 1.37

### Version 1.2.0 - 2021-04-28

* Improved humanization of sets
* Improved validation of intervals and sets
* Fixed DoS vector

### Version 1.1.0 - 2021-04-04

* Added plain EDTF value to RDF output

### Version 1.0.0 - 2021-03-19

* Initial release for MediaWiki/Wikibase 1.35 ([Release announcement], [Demo video])
* EDTF datatype with
  	* Support for EDTF levels 0, 1 and 2
	* Input validation
	* Display of humanized and internationalized version in the reading UI
	* RDF export (using standard Wikibase dates) for most values

[Professional.Wiki]: https://professional.wiki
[EDTF]: https://www.loc.gov/standards/datetime/
[Wikibase]: https://wikibase.consulting/what-is-wikibase/
[MediaWiki]: https://www.mediawiki.org
[PHP]: https://www.php.net
[Wikibase Repository]: https://www.mediawiki.org/wiki/Extension:Wikibase_Repository
[Composer]: https://getcomposer.org
[Composer install]: https://professional.wiki/en/articles/installing-mediawiki-extensions-with-composer
[Release announcement]: https://wikibase.consulting/wikibase-edtf/
[Demo video]: https://www.youtube.com/watch?v=U5ndjtuDPf8
