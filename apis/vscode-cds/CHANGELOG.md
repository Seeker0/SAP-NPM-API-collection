# Changelog

All notable changes to this project will be documented in this file.

This project adheres to [Semantic Versioning](https://semver.org/).
The format is based on [Keep a Changelog](https://keepachangelog.com/).

## 4.1.1 - 2021-07-05

### Changed

- consume cds-lsp 5.1.1
- consume cds-compiler 2.4.4
- new performance relevant user settings
  + `cds.workspace.debounceFastChanges`:  skip intermediate compilations when typing - enabled by default
  + `cds.workspace.scanDependentModules`: skip scanning of node_modules - enabled by default, speeds up start-up time
    
    Note:
    - when using code completion for global identifiers (see `cds.completion.workspaceSymbols.minPrefixLength`) this option needs to be enabled
    - for code completion of import paths in `using` statements this option needs to be enabled
    - to include definitions from nodejs dependent modules in workspace symbols this option needs to be enabled


## 4.1.0 - 2021-06-22

### Note

This is a quality release focusing on performance for large models.
There are new user settings and some have changed their defaults.
Best performance is achieved with default settings, 
except `cds.contributions.enablement.odata` which should be switched `off` to speed up compilation, unless feature is needed.

Additonal hints to increase performance:
- Within _SAP Business Application Studio_: close `CAP Data Models and Services` view. Otherwise it will ask for all workspace symbols at every change.
- Settings: `Cds � Contributions � Enablement: Odata`: switch off as already mentioned above
- Settings: `Editor � Goto Location: Alternative Definition Command`: do not select `goToReferences`. Otherwise being already on a definition will trigger find references which requires all dirty models to be recompiled.
- Settings: `Workbench � Editor � Limit: Enabled`: switch on
- Settings: `Workbench � Editor � Limit: Value`: lower the number. If open editors have `using` dependencies, a change in one editor will lead to a recompile of releated editors.
- Commands `Go to References` / `Find All References` will recompile all models that might have changed due to a change in a depending model. If there are index models it often means the complete workspace is being recompiled. 
Until a further change, reference calculation is resonably fast.
- Command `Go to Symbol in Workspace` will recompile the complete workspace once, then it is reasonable fast
- Changing settings in `CDS` section will currently perform a complete workspace invalidation i.e. required indexes will lead to recompilations on-demand as described above
- Changing certain `cds.env` settings e.g. folder configurations will invalidate the workspace as well

### Changed

- consume cds-lsp 5.1.0
- consume cds-compiler 2.3.2
- user settings
    + cds.workspaceValidationMode new default: OpenEditorsOnly
    + cds.workspace.scanCsn new settings with default switch off (before implicitly on)
    + cds.quickfix.importArtifact new setting with default off (before implicitly on)
- command `Install CDS Development Kit (@sap/cds-dk) globally` will now show a progress dialog as long as it is running

## Version 4.0.4 - 2021-05-11

### Changed
- uses `@sap/cds-lsp@5.0.5`
- new user option `cds.env.fetch` to dynamically fetch _cds.env_ when using non-default i18n file/folder names

### Fixed
- asynchronous scanning of workspace blocked and led to high cpu usage (mostly on Linux/macOS)

## Version 4.0.2 - 2021-05-06

### Changed
- uses `@sap/cds-lsp@5.0.3`
- uses `@sap/cds-compiler@2.2.4`

## Version 4.0.1 - 2021-04-22

### Changed
- now requires Visual Studio Code `>=1.54`
- entry `Preview as hdbtable` replaces `Preview as hana` in `CDS Preview` menu
- uses `@sap/cds-lsp@5.0.1` which includes fixes for namespace handling

## Version 4.0.0 - 2021-03-31

### Note
This is a major release which comes with @sap/cds-compiler v2.
It still supports cds-compiler v1 if used in your project
or via global @sap/cds-dk.

### Added
- semantic highlighting - to be enabled via user setting `cds.semanticHighlighting.enabled`
- new API for annotation handler to support semantic highlighting

### Changed
- consume cds-compiler 2.1.4

## Version 3.4.0 - 2021-03-25

### Added
- preliminary support for cds-compiler 2.x

### Changed
- now requires Visual Studio Code `>=1.53`
- uses `@sap/cds-lsp@4.4.1`
- uses `@sap/cds-compiler@1.50.0`

## Version 3.3.0 - 2021-02-01
### Added
- find references for annotations

### Changed
- extension is now called `SAP CDS language support`
- uses `axios@0.21.1`
- uses `@sap/cds-lsp@4.3.0` (see corresponding changelog for details)
- uses `@sap/cds-compiler@1.49.0`
- new user options for where-used requests<br/>
until now this functionality was enabled by default and now needs to be enabled via user options
  + generic annotations - where a certain annotation 'class' or 'namespace' is used
  + strings literals - where same string literals are used

### Fixed
- temporary folder for `CDS Preview` commands is no longer part of project to avoid files being checked in
- revalidates workspace after an initial annotation plugin installation to show annotation errors w/o the need of manual code edit

## Version 3.2.0 - 2020-11-27

### Added
- quickfix for deprecated identifiers
- support for quickfixes in annotation handler

### Changed
- using native submenu for `CDS Preview` commands
- now requires Visual Studio Code `>=1.50`
- updated npm modules
  + `cds-lsp 4.2.0`
  + `cds-compiler 1.46.4`

### Fixed
- improvements to the `cds section` scheme used in `.cdsrc.json` and `package.json`
- bug fixes for annotation handler and others

## Version 3.1.4 - 2020-11-12

### Fixed
- file system watching no longer worked with latest VSCode

### Changed
- updated npm modules
  + `cds-lsp 4.1.2`

## Version 3.1.3 - 2020-11-10

### Fixed
- internal refactoring and bug fixes

## Version 3.1.2 - 2020-11-09

### Fixed
- support for mono repo file system layouts did not work in certain cases
- completion proposals for annotations were not shown at top of the list if inside an annotation
- only update workspace settings file if needed due to changed fileSystemWatch setting

### Changed
- updated npm modules
  + `cds-lsp 4.1.1`

## Version 3.1.0 - 2020-11-02

### Added
- release notes page shows loading text while loading content
- user setting `cds.releaseNotes.showAutomatically` to enable or disable automatic display of `CAP Release Notes` when a new version is available
- support mono repo file system layouts
- user setting to disable odata plugin
- detection of slow running odata plugin (when validating) incl. user settings to disable and fine tune
- user setting for omitRedundantTypesInSnippets for annotations
- user setting to enable file system watching to track installation of @sap/cds in project

### Changed
- install/update contributions completely async
- completion no longer suggests types when values are meant
- project cds-lsp settings overrule all
- consume cds-compiler 1.45.0

### Fixed
- validation of annotation plugins led to 100% cpu load
- globally installed cds was not reliably found
- code completion for annotation plugins did not work inside annotations at @ characters
- bug fixes


## Version 3.0.1 - 2020-10-23

### Changed
- release notes page uses longer timeout (30sec) when waiting for content

### Fixed
- `preview as...` commands now overwrite preview file content instead of appending
- `preview as...` commands only fail when `cds compile` returned with exit code != 0 (severe error)


## Version 3.0.0 - 2020-09-30

### Added
- Plugin support for domain specific annotation handlers, featuring
  + diagnostics
  + code completion
  + hover information
  + goto definition
  + quickfix to maintain translation
  + auto-installation/update with user setting for npm registry

- code completion inside string literals and `![...]` identifiers is automatically triggered by `/` character (additionally to `.` and `@`)

- snippets applied via code completion are now formatted

- `action`s and their parameters are now indexed and support code navigation, hover etc.

### Changed
- updated npm modules
  + `cds-lsp 4.0.0`
  + `cds-compiler 1.42.2`

### Fixed
- bug fixes


## Version 2.6.1 - 2020-09-24

### Changed
- using `axios` for https access to CAP release notes
- `CAP release notes` are now displayed when new version is available on `Capire`

### Fixed
- the `preview as...` commands are more robust and generate preview files without cds extension

### Added
- command `install cds-dk` now available to install `CDS Development Kit (@sap/cds-dk)` globally


## Version 2.6.0 - 2020-09-01

### Added
- various commands for generating preview files from a cds file
- Code completion proposes identifiers not yet imported in current file and generates corresponding `using` statement
  + user setting: minimum number of characters required to propose those identifiers (`cds.completion.workspaceSymbols.minPrefixLength`). Default is -1 (=off)
  + user setting: limit number of global identifiers (`cds.completion.workspaceSymbols.maxProposals`). Default is -1 (=unlimited)
- Code formatting: options `whitespaceBeforeColon` and `whitespaceAfterColon` are now supported inside `Composition` structs
- Code formatting: support configuration of alignment of `Composition` structs (option `alignCompositionStructToRight`)

### Changed
- updated npm modules
    + `cds-lsp 3.5.0`
    + `cds-compiler 1.39.0`


## Version 2.5.0 - 2020-08-05

### Added
- various commands for generating preview files from a cds file

### Changed
- now requires Visual Studio Code `>=1.46`
- updated npm modules
    + `cds-lsp 3.4.3`
    + `cds-compiler 1.35.0`


## Version 2.4.2 - 2020-07-29

### Added
- issue reporting url pointing to `https://answers.sap.com/`
- new user setting `cds.completion.workspaceSymbols` (default `off`) to add workspace symbols to code completion proposals

### Changed
- updated npm modules
    + `cds-lsp 3.4.2`
    + `cds-compiler 1.34.0`
- internal refactorings

## Version 2.4.1 - 2020-07-13

### Changed
- extension is now hosted on Visual Studio Marketplace and updates from there
  + update configuration and update command have been removed

## Version 2.4.0 - 2020-07-10

### Added

- support for docComments (/** ... */)
  + hover over artifact will consider docComment
  + snippet to create
  + formatting will beautify docComments, incl. new option maxDocCommentLine
  + quickFix to convert @cds.doc annotation to docComment incl. beautify
  + distinct syntax highlighting for docComments
- quickFix to generate using statement for not yet imported artifacts
  + sorted by module and artifact name, beautified
- semantic code completion for elements, enums, actions and parameters in annotate and extend (via cds-compiler 1.32.0)
- syntax highlighting identifiers, including variants delimited with \![...]
- identifier completion proposals are prioritized by message severity
- messages may now contain related information
- added INSTALLATION.md for 3rd party IDE integrations

### Changed
- updated npm modules
    + `cds-lsp 3.4.0`
    + `cds-compiler 1.32.0`
- improved `README.md`
  + updated feature list

### Fixed
- enum keyword was wrongly syntax highlighted
- when an ignored source file is closed, potential messages are wiped
- annotations of extensions were not indexed
- bug fixes

## Version 2.3.3 - 2020-06-26

### Added
- added properties `dest` and `mtx` to code completion list for `tasks` and `for` in cap project package.json files

### Fixed
- syntax highlighting: colorize identifiers, including variants delimited with \![...]
- minor clean up in welcome page handling


## Version 2.3.2 - 2020-06-17

### Changed
- using new npm modules
    + got 11.1.4


## Version 2.3.1 - 2020-05-25

### Added
- add a welcome page with current release information


## Version 2.3.0 - 2020-04-26

### Added
- use global @sap/cds/common if locally not available

### Changed
- now requires Visual Studio Code 1.41+
- using new npm modules
    + cds-lsp 3.3.1
    + cds-compiler 1.26.2
- (alpha): diagnostics and hover support for external annotation providers

### Fixed
- Minor fixes and improvements


## Version 2.2.0

### Added
- install/update global cds-dk

### Changed
- using new npm modules
    + cds-lsp 3.2.0
    + cds-compiler 1.24.3
- remove option for compiler origin - compiler will always be picked in that order: Project -> Global(via DK&CDS) -> BuiltIn
- remove irrelevent formatting option (trimTrailingWhitespace)

### Fixed
- global npm root for Business App Studio was not found with compiler origin option ProjectThenGlobalThenBuiltIn


## Version 2.1.4

### Changed
- using new npm modules
    + cds-lsp 3.1.3
    + cds-compiler 1.23.2

### Added
- pick up compiler and env via global cds-dk if cds not (yet) in project


## Version 2.1.3

### Changed
- using new npm modules
    + cds-lsp 3.1.2
    + cds-compiler 1.23.1

### Added
- code formatting
    - options can now be overridden in source comments e.g. // @formatter tabSize:3
    - new option to add/remove final line break
    - new option to trim trailing whitespace

### Fixed
- in some cases csn files with .json extension where not detected and thus workspace symbols were incomplete


## Version 2.1.2

### Fixed
- Minor bugs and improvements


## Version 2.1.1

### Changed
- using new npm modules
    + cds-lsp 3.1.1

### Added
- code formatting
    - option to keep original empty lines
    - ability to start config UI from any .cdsprettier.json
    - use most relevant .cdsprettier.json when starting from source file
    - use .cdsprettier.json in user home when none available in project

### Fixed
- formatting options were taken from homedir instead of preferring from project


## Version 2.1.0

### Changed
- using new npm modules
    + cds-lsp 3.1.0
    + cds-compiler 1.22.0

### Added
- extended translation support
    - now all runtime-supported formats (.properties, .json, .csv) incl. quick fixes to create missing entries
    - now all runtime-supported customizations (filename, folder name, fallback_bundle, default_language)
        - if entries of fallback language are missing but are defined for default language the latter ones are used
        - quick fix will use default language if file/json-node/section for fallback language does not exist
        - quick fix will try to keep entries sorted
- allow .cdsprettier.json to be located in user home dir

### Changed
- code formatting
    - improve alignment of types, values, and preceding `:` or `=` operators

### Fixed
- code formatting
    - fix, improve, and allow to better adjust alignments and whitespace
    - fix alignment of annotations in `annotate` statement
    - fix casing of and indentation after `Association` and `Composition`
    - fix formatting of parts of `select` statement in case of nesting and after `in`
    - fix positioning of brace `{` after annotation if requested to be kept in previous line
    - fix bug where token starting with `$` was merged
- config UI
    - fix cap/issues#3498 - prettierrc file appears with content
    - fix: no longer selects editor content
- file watching for cds related files did no longer work in recent versions of VSCode e.g. package.json, .cdsrc.json, all supported translation file formats, ignore files


## Version 2.0.0
Official support for code formatting

### Changed
- consume cds-lsp 3.0.0
    - code completion
        - more snippet variants for extend
        - no longer differ entity suggestions between within service or outside
        - base types with parameters now suggested as simply keyword w/o params and additional suggestion as snippet (param names now enclosed in < >)
        - changed label indicator for snippets from <> to ellipsis
    - code formatting
        - rework formatting options: add/remove options according to relevance, rename/group options for clarity, change default behavior in some cases
        - various improvements, including in case statements and bracketed conditions
- consume cds-compiler 1.21.1

### Fixed
- code formatting
    - fix alignment of annotations in views
    - safely identify unreserved keywords


## Version 1.4.0

### Changed
- **requires `Visual Studio Code` version `1.40.0` or higher**
- consume cds-lsp 2.1.21
- consume cds-compiler 1.20.3

### Added
- support LSP configuration via settings file in workspace incl. logs in workspace

### Fixed
- Fix indentation after element definitions using Association or Composition 
- fix: extraction of root models did no longer work since @sap/cds#3.19
- code formatting
    - Fix indentation of keywords `union`, `except`, and `minus`
    - Make alignments more consistent generally
    - Improve readability by padding operators with blanks
    - Improve alignment of bracketed conditions


## Version 1.3.1
- fix reloading formatting options in config UI
- consume cds-lsp 2.1.19
    - fixes and enhancements for code formatting
- consume cds-compiler 1.20.0

## Version 1.3.0
- configuration UI for formatting options
- consumes cds-lsp 2.1.17
    - fixes and enhancements for code formatting
        - renamed settings file to .cdsprettier.json
- renamed tmLanguage for syntax highlighting
- consume cds-compiler 1.19.2
- requires `Visual Studio Code` version `1.38.0` or higher

## Version 1.2.12
- deprecate `updatesite` setting
- requires `Visual Studio Code` version `1.36.0` or higher

## Version 1.2.11
- consumes cds-lsp 2.1.10
    - fixes and enhancements in syntax highlighting and code formatting

## Version 1.2.10
- requires `Visual Studio Code` version `1.35.0` or higher.
- consumes cds-lsp 2.1.9
    - Syntax highlighting now uses different classifications (=colors) for annotations and comments
    - Support user settings in $LSP/.cds-lsp/.settings.json for IDEs which do not support user settings (Eclipse, Intellij, ...)
    - No longer report compiler messages for csn files
    - Preparation for config editor for formatting options
    - Deadlock detection and prevention for code formatting incl. option to cancel or try best in case
    - Use json parsing from compiler for csn files (which allows comments etc.)
    - Fixes and enhancements in source formatting
- consume cds-compiler 1.17.3

## Version 1.2.9
- consume cds-lsp 2.1.8
    - code formatting - many fixes and enhancements
        - now on by default but still experimental
    - syntax highlighting reworked for CDL and CQL keywords
    - where-used index now with using path segments
    - updated cds documentation (in code completion)
    - outline now shows contexts and services with fully qualified name
    - other fixes and improvements
- consume cds-compiler 1.17.0

## Version 1.2.8
- consume cds-lsp 2.1.7
  - consume cds-compiler 1.16.2

## Version 1.2.7
- consume cds-lsp 2.1.6

## Version 1.2.6
- consume cds-lsp 2.1.5
- take syntax file form LSP

## Version 1.2.5
- use internal extension install command instead of command line call to `code` executable to install vsix file.
- requires `Visual Studio Code` version `1.33.0` or higher.
- consume cds-lsp 2.1.4
  - consume cds-compiler 1.15.0
  - experimental support for beautify

## Version 1.2.3
- consume cds-lsp 2.1.3
  - consume cds-compiler 1.14.0

## Version 1.2.2
- consume cds-lsp 2.1.2
  - consume cds-compiler 1.13.4
- Use cds-compiler from project workspace by default (option now called 'cds.compiler.origin' with default 'Project_Then_BuiltIn')
- Option to log internal compiler errors: 'cds.compiler.showInternalErrors' (default true). Same errors are reported at most once per minute.
- minor fixes


## Version 1.2.1
- consume cds-lsp 2.1.1
  - consume compiler 1.13.1
  - performance improvements (less compilations and reduced memory footprint)
  - Links to folders in using statement are now resolved

## Version 1.2.0
- consume cds-lsp 2.1.0
  - consume compiler 1.12.1
  - new where-used index with less memory consumption and support for references of custom annotations, built-in types, string constants and better tracking of identifiers in CQL
  - targets of using statement now rendered as links (via documentLink request)

## Version 1.1.5
- Updated cds-lsp to 2.0.8
  - performance optimizations for translation support
  - consume cds-compiler 1.10.0
- Updated json schema to reflect latest cap config changes.
- Use json schema also for `.cdsrc.json` file.

## Version 1.1.4
- Updated cds-lsp to 2.0.7
    - new user setting: cds.compiler.markMissingI18nDefault (default: off) to show problem markers for unresolved i18n references

## Version 1.1.3
- Updated cds-lsp to 2.0.6 - bugfixes plus
- Updated cds-compiler to 1.9.0

## Version 1.1.2
- Updated cds-lsp to 2.0.5 with
    - suggest @sap/cds/common as default path in using snippet if dependency to @sap/cds is present
- Updated cds-compiler to 1.8.1

### Also see
- Changes of `@sap/cds-lsp@2.0.5`
- Changes of `@sap/cds-compiler@1.8.1`

## Version 1.1.1
- Updated cds-lsp to 2.0.3 with
    - doc support (snippet for localization, hover on translation id)
- Updated cds-compiler to 1.8.0

### Also see
- Changes of `@sap/cds-lsp@2.0.3`
- Changes of `@sap/cds-compiler@1.8.0`

## Version 1.1.0
### Added
Support for documentation and translation in CDS source files

### Also see
- Changes of `@sap/cds-lsp@2.0.0`
- Changes of `@sap/cds-compiler@1.6.0`

## Version 1.0.37
- Updated cds-lsp to 1.4.25 with
    - support for documentation tags and translation
        - show documentation when hovering
        - show documentation at code completion
        - take @title, @description and @cds.doc into account
        - resolve translation and show warning marker when translation not available
        - quickfix to create missing translation
        - snippet for @cds.doc
- Updated compiler to 1.6.0
    - especially with support for code completion of items in using statement

## Version 1.0.36
- Updated cds-lsp to 1.4.24
- Updated compiler to 1.5.0

## Version 1.0.35
- New icon
- Updated cds-lsp to 1.4.23
- Updated compiler to 1.3.0
- Added code completion for paths in using statement
- CSN files are now detected and including in where-used index e.g. for workspace symbols
- Introduce per-file compilation mode which speeds up where-used features expecially for large workspaces. User setting cds.compilation.mode (default PerFile)
- Installation of dependencies in package.json files
    - now configurable via user setting cds.autoInstallDependencies (default off)
    - if turned on it only reinstalls if dependency definitions exist and have changed since last install. Other changes in package.json files are ignored
    - package.json files are no longer reformatted when installing dependencies
    - cdsignore can be used to exclude paths from installation e.g. test folders with package.json files
- Check for updates when user changes update config
- Bug fixes

## Version 1.0.34
- Updated compiler to 1.1.3
- bug fixes

## Version 1.0.33
- Updated compiler to 1.1.2
- bug fixes

## Version 1.0.32
- Snippets now with language documentation
- New snippets for annotate and enum type
- Add json validator for cds entry in package.json
- Add package.json snippet for npm script "watch"
- Replace auto build with npm script (see npm script "watch")
- Robustness
- No more logging from LSP to CDS console

## Version 1.0.31
- Support for multiple workspace folders
- Editor now detects much more identifiers
- Compiler 1.1.0
- Improve error handling when update site is invalid or incorrectly configured

## Version 1.0.30
- Bug fixes (update, ...)

## Version 1.0.29
- Use cds-lsp 1.4.12

## Version 1.0.28
- Use cds-lsp 1.4.10

## Version 1.0.27
- Use os environment proxy settings as fallback when available (http_proxy, https_proxy, no_proxy)

## Version 1.0.26
- Use cds-lsp 1.4.8
- Proxy settings (http.proxy, http.proxyAuthorization, http.proxyStrictSSL) will be used when downloading update
- Bug fixes

## Version 1.0.25
- Use cds-lsp 1.4.7

## Version 1.0.24
- Bug fixes (installation, ...)

## Version 1.0.23
- Use cds-lsp 1.4.4
    - New setting: cds.workspaceValidationMode
        - OpenEditorsOnly
        - OpenEditorsAndDirectSources
        - OpenEditorsDirectSourcesAndDirectDependencies
- Show compiler version on status bar when it changes for 10secs
- Bug fixes (code completion, ...)

## Version 1.0.22
- Add external update mechanism
