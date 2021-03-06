# Change Log
All notable changes to this project will be documented in this file.

This project adheres to [Semantic Versioning](http://semver.org/).

The format is based on [Keep a Changelog](http://keepachangelog.com/).

## 5.0.0 - 2021-03-09

### Fixed
- Replaced *request* with *node-fetch* 2.6.1
- Replaced *requestretry* with *fetch-retry* 4.1.0

## 4.2.0 - 2021-02-22

### Added
- Node.js 14.x support

### Fixed
- Updated *@sap/hdbext* to 7.1.3
- Updated *lodash* to 4.17.21

## 4.1.0 - 2021-02-12

### Added
- Added support for passing HTTP(s) request agent options to the Service transport.
  This feature is experimental and is therefore not documented. It may be changed or removed anytime.

## 4.0.0 - 2021-02-09

### Fixed
- Update devdeps
- Update debug to 4.2.0
- Update requestretry to 4.1.1
- Allow empty strings in objectWithTypeAndId (utils.js) check

### Removed
- Node.js 6.x support

## 3.2.0 - 2020-08-10

### Added
- Add support for multiple data subjects in Data access messages, v2 API

## 3.1.1 - 2020-07-27

### Fixed
- Update dependencies

## 3.1.0 - 2019-12-06

### Added
- Node.js 12.x support
- Retry logic when accessing the auditlog

### Fixed
- Update dependencies

## 3.0.2 - 2019-07-16

### Fixed
- Update dev dependencies

## 3.0.1 - 2019-05-17

### Fixed
- Transactional messages: uuid and time fields are now updated for each log operation

## 3.0.0 - 2019-04-23

### Removed
- Node.js v0.12 support
- Node.js v4 support

## 2.3.0 - 2018-12-18

### Added
- Node.js version 10 support

### Fixed
- Update `lodash` to 4.17.11

## 2.2.4 - 2018-08-14

### Fixed
- Update dependencies.

## 2.2.3 - 2018-07-17

### Fixed
- Update request to v2.87.0.

## 2.2.2 - 2018-05-18

### Fixed
- Update request to v2.86.0.

## 2.2.1 - 2018-04-05

### Fixed
- Update dependencies.

## 2.2.0 - 2018-01-23

### Added
- npm-shrinkwrap.json.
- Audit log service v2 support.

### Fixed
- Documentation for data access messages.

## 2.1.1 - 2017-10-13

### Changed
- Dependencies' versions

## 2.1.0 - 2017-08-08

### Added
- Support for old and new values in data modification messages.
- Support for setting a tenant.
- Support for Node.js v8.
- Documentation improvements.

## 2.0.1 - 2017-05-30

### Fixed
- Improved debug messages.

## 2.0.0 - 2017-03-13

### Added
- The library requires the application to be bound to an instance of the **Audit log service**.
- Instantiating the library now requires a configuration object containing the `credentials` from the service binding to be provided (see README.md).
- `configurationChange` method is added to the API (see README.md).
- `updateConfigurationChange` method is added to the API (see README.md).

### Changed
- `read`, `update` and `securityMessage` methods have some changes to their argument lists (see README.md).

### Removed
- `delete` method is removed from the API.
- `create` method is removed from the API.
