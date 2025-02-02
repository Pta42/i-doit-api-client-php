# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

**Notice: Support for PHP 5.6 is finally dropped. Version 7.0 is deprecated. Support will be dropped in a further release. Please upgrade to at least version 7.1. Version 7.3 is recommended.**

### Added

-   `CMDBStatus`: Provide API calls for namespace `cmdb.status`
-   Provide cURL error code in case of connection problems
-   Check for errors when initiating cURL
-   Add virtual category constant "C__CATG__DATABASE_FOLDER" which will be blacklisted by some methods
-   Run environment in a Docker container

### Changed

-   Drop support for PHP version 5.6
-   Mark PHP version 7.0 as deprecated
-   Recommend PHP 7.3
-   Declare strict types

## [0.8] – 2019-04-16

It's spring time! 🌱

To get the full experience, please update your i-doit to version >= 1.12.2 and API add-on to version >= 1.10.2.

### Added

-   `Idoit::getLicense()`: Read license information
-   Configuration option `API::BYPASS_SECURE_CONNECTION`: Disable security-related cURL options (boolean)
-   `CMDBObject::readAll()`: Read all information about object including category entries
-   `CMDBCategory::batchRead()`: Add new optional parameter "status"

### Changed

-   `CheckMKStaticTag::create()`, `CheckMKStaticTag::batchCreate()`: Do not require parameter `tag` anymore
-   `CheckMKStaticTag`: Remove parameter `export` from static tags (not available any more in add-on "Check_MK 2")
-   `CMDBObject::load()` is deprecated, because it is pretty slow. Use `CMDBObject::readAll()` instead!
-   Suppress HTTP header `Expect` to prevent broken responses

### Fixed

-   `API::disconnect()`: Let `curl_close()` delete resource is enough

## [0.7] – 2018-12-17

This release comes with new features and tons of unit tests. To get the full experience, please update your i-doit to version >= 1.11.2 and API add-on to version >= 1.10.

### Added

-   `CMDBObject::createWithCategories()`: Create new object with category entries
-   `CMDBObject::recycle()`: Restore object to "normal" status
-   `CMDBObject::markAsTemplate()`: Convert object to template
-   `CMDBObject::markAsMassChangeTemplate()`: Convert object to mass change template
-   `CMDBObjects::read()` & Co.: Optionally, fetch category entries for each object
-   `CMDBObjects::recycle()`: Restore objects to "normal" status
-   `CMDBCategory::save()`: Create new or update existing category entry for a specific object
-   `CMDBCategory::read()`, `CMDBCategory::readOneByID()`: Filter entry/entries by status
-   `CMDBCategory::archive()`: Archive entry in a multi-value category for a specific object
-   `CMDBCategory::delete()`: Mark entry in a multi-value category for a specific object as deleted
-   `CMDBCategory::purge()`: Purge entry in a single- or multi-value category for a specific object
-   `CMDBCategory::recycle()`: Restore entry in a multi-value category for a specific object to "normal" state
-   `CMDBDialog::create()`: Reference parent entry by its title (string) or by its identifier (integer)
-   `CMDBDialog::delete()`: Purge value from drop-down menu
-   `Idoit::getAddOns()`: Read information about installed add-ons
-   `CMDBImpact`: Filter relations by status
-   `CMDBObjectsByRelation`: Filter relations by status
-   `CMDBLocationTree`: Filter relations by status
-   `CMDBLogbook`: Set optional limit when reading entries
-   Execute CLI commands over API (method namespace `console`)
-   `CheckMKTags`: Read host tags by one or more objects from category `C__CATG__CMK_TAG`
-   `API::rawRequest()`: Perform a low level API request
-   `CMDBCategoryInfo::testGetVirtualCategoryConstants()`: Get list of constants for virtual categories
-   `CMDBImpact::readByTypes()`: Perform an impact analysis for a specific object by one ore more relation type constant or identifiers

### Changed

-   `CMDBObject::archive()`, `CMDBObjects::archive()`: Change to new API method `cmdb.object.archive`
-   `CMDBObject::delete()`, `CMDBObjects::delete()`: Change to re-newed API method `cmdb.object.delete`
-   `CMDBObject::purge()`, `CMDBObjects::purge()`: Change to new API method `cmdb.object.purge`
-   `CMDBObject::load()`: Include custom categories with user-defined attributes
-   `CMDBObject::load()`: Ignore virtual categories which have no data
-   `CMDBCategory::clear()`: Change to new API method `cmdb.object.archive`
-   `CMDBCategory::purge()`: Re-name method `purge()` to `quickpurge()`
-   Add HTTP header `Expect: 100-continue` to each API call, useful for huge calls/slow hosts
-   Validate error object in response and throw all details about it

### Fixed

-   `File::add()`, `File::batchAdd()`: Use renamed constant for category "file versions"
-   Avoid "PHP Notice" when there is a detailed error description available

## [0.6] – 2018-06-21

Happy summer time ⛱️

### Changed

-   Throw SPL exceptions to provide you more semantics on errors
-   `CMDBWorkstationComponents`: Re-named methods `readByEmail()` and `readByEmails()`
-   Require PHP >= 7.1 on dev/ci environments (only relevant if you want to [contribute](CONTRIBUTING.md))

### Fixed

-   `CMDBCategory::clear()`: Archiving zero category entries results in a broken API request

##  [0.5] – 2018-04-25

### Added

-   `CMDBCategoryInfo::readAll()`: Try to fetch information about all available categories
-   `API::request()`: Allow to overwrite `language` parameter
-   Enhance unit tests, mostly for testing fixed bugs in i-doit 1.10.2 and API add-on 1.9.1

### Changed

-   HTTP body message from server response will be added to thrown exception if request fails with unknown error
-   `CMDBCategory::batchUpdate` returns itself (neither the result nor the entry identifiers)

### Fixed

-   `CMDBCategory::readFirst()` now returns an empty array `[]` instead of `false` (boolean)
-   Validation error for missing proxy settings while proxy is disabled by `proxy.active=false`

## [0.4] – 2018-02-21

### Added

-   Archive category entries for a specific object with `CMDBCategory::clear()`
-   Create, read, update and delete monitoring instances (monitoring add-on)
-   Create, read, update and delete static host tags (Check_MK add-on)
-   Update several category entries with `CMDBCategory::batchUpdate()`
-   List requirements in [documentation](README.md)
-   More assertions in unit tests

### Changed

-   Bump required versions of i-doit (>= 1.10) and its API add-on (>= 1.9)
-   Remove `idoitapi.php` because Composer is the prefered way to use
-   Require entry identifier in methods `CMDBCategory::archive()`, `delete()` and `purge()`
-   Methods `cmdb.category.create`, `cmdb.category_info.read` (and others, too) do not need parameters `catg` or `cats`. Parameter `category` seems to be sufficient.
-   Make `CMDBCategory::purge()` a lot faster due to method `cmdb.category.quickpurge`
-   Return empty array for reports with no results (class `CMDBReports`)
-   Remove many dependencies from unit tests

### Fixed

-   Use correct setting for proxy type and check if username is set

## [0.3] – 2017-07-25

### Added

-   Check whether connection timed out or i-doit host sends HTTP status code that indicates something went wrong
-   Throw more useful exceptions when connection to Web server failed
-   Throw exception in method `CMDBObject::load()` when object not found
-   Limit batch requests in `Select::find()`

## [0.2] – 2017-04-05

### Added

-   Upload image files with class Image
-   Get last server response with method `API::getLastResponse()`
-   Find more objects by their attributes with method `Select::find()`
-   Script for debugging purposes in `README.md`
-   Many more unit tests

### Fixed

-   Broken batch request in method `Image::batchAdd()`
-   Broken error message in method `CMDBCategory::batchCreate()`
-   In a batch request sub results have no key id in method `CMDBCategory::batchCreate()`
-   Broken Exception message in `CMDBObject::upsert()`
-   Typos in `README.md`

## [0.1] – 2017-02-09

Initial release

[Unreleased]: https://github.com/bheisig/i-doit-api-client-php/compare/0.8...HEAD
[0.8]: https://github.com/bheisig/i-doit-api-client-php/compare/0.7...0.8
[0.7]: https://github.com/bheisig/i-doit-api-client-php/compare/0.6...0.7
[0.6]: https://github.com/bheisig/i-doit-api-client-php/compare/0.5...0.6
[0.5]: https://github.com/bheisig/i-doit-api-client-php/compare/0.4...0.5
[0.4]: https://github.com/bheisig/i-doit-api-client-php/compare/0.3...0.4
[0.3]: https://github.com/bheisig/i-doit-api-client-php/compare/0.2...0.3
[0.2]: https://github.com/bheisig/i-doit-api-client-php/compare/0.1...0.2
