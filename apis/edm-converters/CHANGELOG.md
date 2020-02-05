# Change Log

All notable changes to this project will be documented in this file.

This project adheres to [Semantic Versioning](http://semver.org/).

The format is based on [Keep a Changelog](http://keepachangelog.com/).

## Unreleased

## [1.0.19] - 2019-09-13

Add:
   * CSN Generation For Mock Server Use Case V2. 

## [1.0.3] - 2019-03-29

Fixed
* CSN Generation Fixes For Mock Server Use Case V4.

## [1.0.1] - 2019-03-21

## [1.0.0] - 2019-03-08

Add:
   * EDMXV4 to EDMV4 JSON Converter 
   * EDMV4 JSON to CSN Converter
   * EDMXV2 to EDMV4 JSON Converter   
   * EDMXV2 to CSN Converter   

## [0.0.18] - 2019-03-07

- # Test Release. Not for productive usage

## [0.0.17] - 2019-03-06

- # Test Release. Not for productive usage

## [0.0.16] - 2019-03-06

- Add new -t,--target option to produce OData Node.js csdl json custom format
- Remove Temporal vocabulary from autoloading
- Improve error message if XML elements can't be resolved
- Change generated $Collection = 'true' to true boolean for functions generated from function imports
- $Nullable is not set on NavigationProperty collections anymore.
- Improve error message if XML elements can't be resolved
- Change generated $Collection = 'true' to true boolean for functions generated from function imports

## [0.0.15] - 2018-12-12

- OData V2-XML to OData V4-JSON converter - Add $Collection to EntitySets

## [0.0.14] - 2018-11-30

## [0.0.13] - 2018-11-30

- # Test Release. Not for productive usage

## [0.0.12] - 2018-11-29

Add:
- Autoloading for oasis default vocabularies

Remove following changes (will be provided later):
- Change output conversion of Constant Expressions
    - See OASIS Issue: https://issues.oasis-open.org/browse/ODATA-1221

Add following changes:
- Remove $Kind from EntityContainer EntitySets,Singletons and add $Collection to EntitySets
    - See OASIS Issue: https://issues.oasis-open.org/browse/ODATA-1231

- V4 Converter
    - Now a missing referenced document does not break the processing
    - On execution end there is a new callback parameter providing all missing referenced documents and corresponding uris

## [0.0.11] - 2018-11-06

## [0.0.10] - 2018-11-06

## [0.0.9] - 2018-11-06

- Change output conversion of Constant Expressions
    - See OASIS Issue: https://issues.oasis-open.org/browse/ODATA-1221
- Remove $Kind from EntityContainer entities
    - See OASIS Issue: https://issues.oasis-open.org/browse/ODATA-1231

## [0.0.8] - 2018-10-18

## [0.0.7] - 2018-10-16

## [0.0.6] - 2018-10-16

## [0.0.5] - 2018-09-27

## [0.0.4] - 2018-09-25

## [0.0.3] - 2018-09-13

## [0.0.2] - 2018-08-22

### Added

* Release version 0.0.1 (alpha version)