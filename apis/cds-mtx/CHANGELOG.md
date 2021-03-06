# Change Log

All notable changes to this project will be documented in this file.

This project adheres to [Semantic Versioning](http://semver.org/).

The format is based on [Keep a Changelog](http://keepachangelog.com/).


## Version 2.1.2 - 2021-07-09

## Version 2.1.1 - 2021-07-09


## Version 2.1.0 - 2021-07-07


- Tenant creation and deletion is now called via cds service `TenantPersistenceService` that
applications can add handlers for
```
@protocol:'rest'
service TenantPersistenceService {
    type JSON {
        // any json
    }

    action createTenant(tenantId: UUID, subscriptionData: JSON) returns String;
    action deleteTenant(tenantId: UUID);
}
``` 

## Version 2.0.4 - 2021-06-14


### Added

- The global meta tenant creation in `cds.mtx.in` can now be disabled by setting the `MTX_DISABLE_META_TENANT_CREATION` environment variable

### Fixed

- More error types and semantic HTTP status codes have been added
- Setting `MTX_ROLLBACK_CORRUPTED_TENANT` to `true` will now also delete and recreate an HDI container if its bindings are missing

## Version 2.0.3 - 2021-06-08


### Fixed

- MTX Bootstrap has been adapted so that application service handlers can access mtx services
again
- Logging is now consistently using cds.log
- A caching problem with the metadata persistence factory is fixed

### Changed

- File system APIs are now asynchronous

### Added

- Allow array as configuration for mandatory scopes for subscription and update
```
mtx: {
    security: {
        "subscription-scope": ["myApp.subscription","myApp.superadmin"],
        "deployment-scope": ["myApp.deployment", "myApp.superadmin"]
    }
}
```

## Version 2.0.2 - 2021-05-21


### Added
- Internal on- and offboarding API for sidecar usecase: POST `/mtx/v1/internal/provisioning/subscribe`
and POST `/mtx/v1/internal/provisioning/unsubscribe` 
with payload 
```
{
  "subscribedTenantId": <tenant id>,
  "async": <true/false>
}
```

### Changed

- A failed offboarding will now throw a `TenantOffboardingError`, instead of just logging the error.

### Fixed

- MTX APIs are now served idempotently. This fixes custom provisioning handlers not being invoked correctly.

## Version 2.0.1 - 2021-05-10


### Added

- It's now possible to provide BTP dependencies in the `mtx` settings in your `.cdsrc.json` instead of implementing a custom handler, by setting `mtx.dependencies` accordingly.

## Version 2.0.0 - 2021-05-07


### Added
- Saas provisioning operations GET, PUT and DELETE on API `/mtx/v1/provisioning/tenant/`
now require scope `mtcallback`. Upgrade calls on API `/mtx/v1/model/upgrade/` and 
`/mtx/v1/model/upgradeAsync/` now require scope `mtdeployment`.
This is now aligned with the 
[mandatory scope check required for the java runtime](../java/multitenancy#xsuaa-mt-configuration).<br>
To adapt the scope names to the java runtime scope configuration, 
the scope names can be changed using the following cds configuration:
```
mtx: {
    security: {
        "subscription-scope": "myApp.subscription",
        "deployment-scope": "myApp.deployment"
    }
}
```
- Support cds build API throwing BuildError instead of CompilationError.
- Undeployment of extensions can now be done using a simplified API:
`/mtx/v1/model/deactivate` with payload containing the extension sources to
be removed.
```
{
  "extension_files": [
    "db/ext3.cds"
  ],
  "tenant": "<tenant id>"
}
```
- Support automatic roll-back for corrupted tenants when `MTX_ROLLBACK_CORRUPTED_TENANT` is set to `true`. This should _never_ be used in production, but only for integration tests.

### Changed

- The global data meta tenant (`GLOBAL_DATA_META_TENANT`) is now created on the first application startup, instead of the first onboarding
- `@sap/hdi-deploy` and `@sap/instance-manager` are now directly required by `@sap/cds-mtx`. Therefore, they can be left out of your `package.json` `dependencies`
- Job IDs are now generated using the `uuid` package
- The default behavior of the `extension-allowlist` has changed. If `extension-allowlist`
is not configured, it is not allowed to apply any extension.<br>
Extensions can be easily enabled for all entities and services by adding the following 
to the configuration.
```
mtx: {
  "extension-allowlist" = [
      {
          "for": ["*"]
      }
  ]
```

### Fixed
- No more duplicate log entries in model upgrade result.

## Version 1.2.3 - 2021-05-01


### Fixed
- Scope check for extension undeployment (ExtendCDSDelete) is enabled again

## Version 1.2.2 - 2021-04-23


## Version 1.2.1 - 2021-04-14


### Fixed
- Undeployment for model upgrade via `advancedOptions` working again
- Undeployment of base model artifacts via 'undeploy.json' is working again
- Unallowed `@cds.persistence.journal` annotations in extensions are now checked 

### Added
- Size of job queue can now be configured via cds env
```
mtx: {
  jobqueue: {
    size: 10
  }
```
or parameter `CDS_MTX_JOBQUEUE_SIZE=10`

## Version 1.2.0 - 2021-03-30



### Added
- Multitenant applications now support extensions of entities using schema evolution based on `.hdbmigrationtable` files.
- It is now possible to specify limits for the number of extension fields per entity.
If no limit is specified, the number of extension fields is not limited.\
If this list exists, only entities and services contained in this list can be extended. 
```
"mtx" : {
  "extension-allowlist": [
    {
        'for': ['my.bookshop.Authors', 'my.bookshop.Books'],
        'new-fields': 2
    },
    {
        'for': ['CatalogService']
    }
  ]
}
```

## Version 1.1.5 - 2021-03-09

### Fixed
- The extension API `/mtx/v1/content` now returns a correct json if a collection is requested with any version of 
@sap/cds used by the application. The `cds extend` command was returning `(intermediate result) is not iterable` because
of an incorrect server response.
- Connection handling has been improved. Errors of type `TimeoutError: Acquiring client from pool timed out` are reduced.

## Version 1.1.3 - 2021-03-03


### Fixed
- `mtx/v1/model/status` now returns the job status again

## Version 1.1.2 - 2021-03-01


### Added 
- Multitenant applications **without tenant specific extensions** now support schema evolution based on `.hdbmigrationtable` files.
- Provisioning parameters for the container creation can now also be set
via cds environment `mtx.provisioning.container` or environment variable `CDS_MTX_PROVISIONING_CONTAINER`.\
Provisioning parameters that are set in the subscription request to `mtx/v1/provisioning/tenant` 
override the values from the environment.
- Dedicated hdi deployment options can now be set via environment variable
`HDI_DEPLOY_OPTIONS`, e. g. `HDI_DEPLOY_OPTIONS="{\"trace\": true }"`. \
See also 
[HDI deploy options](https://help.sap.com/viewer/4505d0bdaf4948449b7f7379d24d0f0d/2.0.05/en-US/a4bbc2dd8a20442387dc7b706e8d3070.html).

### Fixed
- Fix job-status handling.
- Persist job errors, so they can be revealed even after MTX restart.

## Version 1.0.28 - 2021-02-22

### Added 

- It is now possible to pass hdi deployment parameters `undeploy` and `path-parameter` with the model upgrade 
(`mtx/v1/model/upgrade` and `mtx/v1/model/asyncUpgrade`)

## Version 1.0.27 - 2021-02-01

### Fixed

- Extensions via `extend projection ` are now checked correctly by the linter.
- Cross hdi container access is now supported properly (see also https://help.sap.com/viewer/4505d0bdaf4948449b7f7379d24d0f0d/2.0.05/en-US/a4bbc2dd8a20442387dc7b706e8d3070.html)

## Version 1.0.26 - 2021-01-04


### Fixed

- When using`hdb` as driver for the database, the tenant updates are now logged properly

## Version 1.0.25 - 2020-11-27


### Added

- Added enhanced authentication API needed for the @sap/cds-sidecar-client 
to support authentication with clientid/clientsecret from subscriber account.
This is needed to extend multitentant applications that are provided as services.

### Fixed

- Activation via cds-sidecar-client shows linter errors again
- Offboarding of tenants does no longer cause a reconnect of cds 
(also requires update of @sap/cds dependency to minimum @sap/cds@4.3)

## Version 1.0.24 - 2020-11-07


### Fixed

- Broken compatibility with hdb driver is now fixed

## Version 1.0.23 - 2020-11-05


### Fixed

- MTX is now compatible with latest versions of @sap/hana-client
- Linters can now handle extension projects with subfolders again
- The connection pool used by mtx is now correctly updated on offboarding 
even with scaled applications

## Version 1.0.22 - 2020-10-21


### Fixed

- The application url returned to the saas registry when using asynchronous onboarding
can now also be set in the header field 'application_url'
- The build task used when onboarding do now use the right defaults. When being used
as sidecar application, build task do no longer have to have the model option.
- New entities with namespaces in extensions are now correctly 
checked by the extensibility linter on extension activation
- Call of onboarding and offboarding via javacript API is now fixed ('Cannot read property 'headers' of undefined')

## Version 1.0.21 - 2020-09-29


### Fixed

- Connections to application after off- and onboarding now work properly

## Version 1.0.20 - 2020-09-10


### Fixed
- Custom content upload using `/mtx/v1/model/updateCustomTenantContent` now also works 
with cds 4

## Version 1.0.19 - 2020-09-03

### Added
- The asynchronous model update API now accepts a callback URL (header field `MTX_STATUS_CALLBACK`) that
  is called when the update is finished
- A new REST API for activating extensions from csn sources was added. Use `POST /mtx/v1/model/activateCsn/` 
with a csn JSON as payload. Example:

```
{
        "extensionCsn": "{\"extensions\":[{\"extend\":\"sap.capire.bookshop.Books\",\"elements\":{\"Z_ISBN_101\":{\"type\":\"cds.String\"}}}]}",
        "tenant": "213a722e-ed91-43e2-adb4-3885b5e9d63e"
}
```

### Fixed
- The sequence of extensions added through `cds.mtx.activate()` is now stable, even after 
a base model update.

## Version 1.0.18 - 2020-08-28

## Version 1.0.17 - 2020-08-19

### Fixed
- Asynchronous basemodel upgrade and job status requests that failed when using @sap/cds@^4 are now fixed
- Extensions that got lost when running onboarding multiple times are now preserved
- The cds env configuration is now also available for service-manager 

## Version 1.0.16 - 2020-08-03

### Added

- Support for instances managed by Service Manager
- Support for JWT Refresh Tokens to simplify token renewal in cds-sidecar-client

### Fixed

- Idempotent asynchronous offboarding
- CDS 4 compatibility


## 1.0.15 - 2020-07-01


### Fixed
- cds 4 compatibility
- updated readme.md

## 1.0.14 - 2020-05-27


### Added

- diagnose endpoint at /mtx/v1/model/diagnose/
    - requires to be authenticated for the paas tenant
    - requires scope "MtxDiagnose"
- added memory status to debug logs. Activation with env MTX_LOG_MEMORY=true

### Fixed

- stablelized parallel extension activation
- handling of asynchronous job status across multiple mtx application instances

## 1.0.13 - 2020-04-27

### Added

- Logs are now collected by default when running asynchronous APIs
    - can be disabled with environment variable `MTX_COLLECT_LOGS=true`
    - logs can be limited by `MTX_LOG_COLLECTION_LIMIT=<limit>`

- extensibility API that accepts csn notation `cds.mtx.activate(tenantId, csn)`

### Fixed

- handling of errornous undeploy.json files: error message now points to the root problem
- handling of build errors: API now returns build and compile errors properly

## 1.0.12

### Added
- reenabled the support of different domains (BETA)

- Support for asynchronous extension activation to handle long-running jobs

## 1.0.11

### Added

- Support of asynchronous onboarding as specified by the [saas-registry](https://wiki.wdf.sap.corp/wiki/display/CPC15N/SaaS+Application+Registration+in+CF#SaaSApplicationRegistrationinCF-Asynchronouscallbacksimplementation)

## 1.0.10

### Fixed
- Compatibility with snapi compiler mode (env variable CDS_FEATURES_SNAPI=y)
    - WARNING: in this mode, it is currently not checked if annotations of the basemodel are overwritten in the extension
- Tenant upgrade with undeploy=true is now working properly

## 1.0.9

### Fixed
- Compatibility with older versions of @sap/cds

## 1.0.8

### Added
- possibility to store build / deployment logs in job log for asynchronous tenant update
    - must currently be activated by environment variables (`MTX_COLLECT_LOGS=true` and `MTX_LOG_COLLECTION_LIMIT=<limit>`)
- whitelist for entities and services that are allowed to be extended
```
"mtx": {
           "element-prefix": "Z_",
            "namespace-blacklist": ["com.sap.", "sap."],
            "entity-whitelist": ["my.bookshop.Books"],
            "service-whitelist": ["CatalogService"]
       }
```

### Fixed
- Enable compatibility with SAP HANA cloud edition (no hdbcds support)
- Deployment error with very old tenants (conflict with custom_tenant_objects.hdbtable)

## 1.0.7

### Added
- allows to enable auto-undeploy in base model update request

### Fixed
- datatype error in synchronous base model update api

## 1.0.6

## 1.0.5

### Added
- accept database_id in onboarding request

## 1.0.4

### Added
- enhanced namespace check for extensions
- metadata API now supports ETags
### Changed

### Fixed
- Datatype of custom content tables now LargeString
- Bad request when sending non-json content

### Removed

## 1.0.3
- Bug fixes

## 1.0.2
- Bug fixes

## 1.0.1
- Ignore namespace rules for customer extension entities
- Enable hdbtabledata generation

## 1.0.0 - 26.07.2019
Initial release version.