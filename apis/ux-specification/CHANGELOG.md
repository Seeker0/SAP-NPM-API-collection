# Change Log

All notable changes to this project are documented in this file.

This project adheres to [Semantic Versioning](http://semver.org/) and the changelog is formatted based on [Keep a Changelog](http://keepachangelog.com/).

## [1.90.4] - 2021-06-30

### Added

- New structure elements at the Filterbar that represent the selection fields resulting from  the UI.SelectionFields annotation.

### Changed

- In the annotationPath information of the generated JSON schemas all alias definitions are now replaced by the full vocabulary name.
- Descriptions in the schema now follow the following paradigm:
  - For facets we need use title and description, the export logic takes the title and extracts the facet key from it for concatenating the stable ID. The text that shall be shown in the Page Editor shall be the description.  
  - Thus we now generalise: all view nodes (isViewNode = true) have a description (but do not necessarily need a title).  
  - Properties (no isViewNode tag) are not listed in the outline, but in the properties panel. Here, the title shall be used as a label, and the description shall be shown on hover (old UI) or rendered directly (new UI).

### Removed

- OData V4: remove initialLoad property from Object Page config

### Deprecated

### Fixed

- The deletion of a custom section from the config file was not reflected in manifest.json.
- New page export fails if parent page does not have navigation property defined.
- Navigation is not cleared if we remove last associated page.
- The import of unknow flex change properties into the config is now avoided.
- The export of flex changes sometimes generated multiple flex changes into the dev workspace.
- The script for extracting documentation had neglected patternProperties of the schemas.
- An exception occurred in case of invalid annotation references, the schemas and config files were not generated in this case.
- Too many actions had been listed as table columns: now it is restricted to the inline actions.

### Quality

## [1.90.3] - 2021-06-28

### Fixed

- The build result did not comprise the type definitions anymore. They are available again now.

## [1.90.2] - 2021-06-17

### Added

- annotationPath added as metadata to the columns in JSON schema.

### Changed

- The enum for fragment names of column extension is more restrictive now, it does not comprise cells anymore.
- webpack uglify function names.

### Deprecated

### Fixed

- During the export of V4 configs, manifest sections were created from scratch; this led to a deletion of properties that are not considered (yet) by the specification module.
- Fix for launch config Import project which can be used to debug the import of a project.
- During the export, fragments are not deleted anymore, as they may comprise customer coding.
- If a fragment is not present yet, it gets created now, i.e. added with a <root> prefix to the list of fragments.
- yarn watch allows debugging of custom extensions now, as the templates are available again in the dist folder.
- Correct sync logic to make sure that config changes are written only to desired places in manifest.json
- An exception had occured in case of invalid target annotation paths, this is caught now.
- OData V4 - Correct sync logic so that custom sections entry in manifest.json gets deleted if entry is deleted from page config
- OData V4 - Correct sync logic, page add was not added if 'navigation' property missing in 'manifest.json'.
- OData V4 - Correct sync logic, page deletion, 'navigation' property was not cleared if there was no an navigation left.
- OData V2 - Flex changes not supported by specification module remain unaffected by the sync logic
- OData V2 - Fix sync logic to avoid creation of duplicate flex changes in case of Object Page tables
- OData V2 - ALP: Improved sync logic as some of the supported flex chagnes were not imported to config files

## [1.90.1] - 2021-06-03

### Added

### Changed

- The access to reflect metadata is now centralized in factory classes and thus runs in a generic way in most situations.

### Deprecated

### Fixed

- Path references of labels had not been resolved correctly. Common.label annotations of the target reference are now considered correctly.
- The regex pattern for binding changes was incomplete, resulting in validation errrors.
- OData V4: Enhance sync logic to ensure only properties that are supported by specification module are modified in the manifest.json

### Quality

Upgrades of dependencies to:

- "ts-loader": 9.1.2
- node version 12
- "webpack": 5.37.1
- "chalk": "4.1.1"
- "ejs": "3.1.6"
- "i18next": "20.3.1"
- "ts-json-schema-generator": "0.93.0"

Added resolutions for

- "glob-parent": "^5.1.2"
- "normalize-url": "^6.0.1"
- "ws": "^7.4.6"
- "trim-newlines": "^3.0.1"

## [1.90.0] - 2021-05-20

### Added

First specification version for SAPUI5 release 1.90 and later.

### Changed

- The initialLoad setting of V4 tables is now an enum with three different options, in accordance with sap.fe.
