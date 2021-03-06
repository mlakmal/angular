# Consumer Portal Angular/JavaScript Coding Standards

## Table of Contents

  1. [Guide to Start Coding in Consumer Portal Code Base](#guide-to-start-coding-in-consumer-portal-code-base)
  1. [Application Structure](#application-structure)
    1. [Single Responsibility](#single-responsibility)
    1. [Directory Structure](#directory-structure)
    1. [Overall Guidelines](#overall-guidelines)
      1. [Folders by Feature Structure](#folders-by-feature-structure)
    1. [Naming Guidelines](#naming-guidelines)
      1. [File Names by Type](#file-name-by-type)
      1. [Test File Naming](#test-file-naming)
      1. [Controller Naming](#controller-naming)
      1. [Factory Naming](#factory-naming)
      1. [Service Naming](#service-naming)
      1. [Directive Naming](#directive-naming)
      1. [Filter Naming](#filter-naming)
      1. [Module Naming](#module-naming)
        1. [Module Configuration Naming](#module-configuration-naming)
      1. [Routes Naming](#routes-naming)
    1. [Asynchronous Module Definition](#asynchronous-module-definition)
      1. [RequireJS Module Definition](#requirejs-module-definition)
      1. [RequireJS Configuration](#requirejs-configuration)
        1. [Define New Angular Component Path](#define-new-angular-component-path)
        1. [Define Sub Dependencies](#define-sub-dependencies)
        1. [Define Main Dependencies for angular.bootstrap](#define-main-dependencies-for-angular.bootstrap)
  1. [Angular Component Coding](#angular-component-coding)
    1. [Modules](#modules)
    1. [Controllers](#controllers)
      1. [Using vm with controllerAs](#controlleras-with-vm)
      1. [Bindable Members and Functions](#bindable-members-up-top)
        1. [Function Usage in Controllers](#function-usage-in-controllers)
      1. [Services and Controller Logic](#services-and-controller-logic)
      1. [Controller Initialization](#controller-initialization)
      1. [Controllers with Routes](#controllers-with-routes)
    1. [Services](#services)
    1. [Factories](#factories)
      1. [Public Members and Functions](#public-members-and-functions)
      1. [Function Declarations](#function-declarations)
    1. [REST API Wrapper Services](#rest-api-wrapper-services)
    1. [Directives](#directives)
      1. [Directive Restrict Guidelines](#directive-restrict-guidelines)
      1. [Use of ControllerAs with Directives](#use-of-controlleras-with-directives)
    1. [Filters](#filters)
  1. [Resolving Routes](#resolving-routes)
  1. [Angular Dependency Injection](#angular-dependency-injection)
    1. [Manually Inject Dependencies](#manually-inject-dependencies)
    1. [Use ngAnnonate to Inject Dependencies](#use-ngannonate-to-inject-dependencies)
      1. [Gulp Configuration for ngAnnonate](#gulp-configuration-for-ngannonate)
  1. [Exception Handling](#exception-handling)
    1. [Angular Specific Exceptions](#angular-specific-exceptions)
    1. [Global Exceptions](#global-exceptions)
    1. [Routing Specific Errors](#routing-specific-errors)
  1. [Angular $ Wrapper Services](#angular--wrapper-services)
  1. [Testing](#testing)
    1. [Organizing Test Files](#organizing-test-files)
    1. [Configure Testing Framework](#configure-testing-framework)
    1. [How to Write spec File for Unit Testing](#how-to-write-spec-file-for-unit-testing)
  1. [Animations](#animations)
  1. [Code Commenting](#code-commenting)
  1. [Validate Coding Standards and Styles](#validate-coding-standards-and-styles)
    1. [Validate Coding Standards](#validate-coding-standards)
    1. [Validate Coding Styles](#validate-coding-styles)
  1. [File Templates and Snippets](#file-templates-and-snippets)
  1. [Task Automation](#task-automation)
  1. [JavaScript Coding Standards](#javascript-coding-standards)
    1. [Types](#types)
    1. [Objects](#objects)
    1. [Arrays](#arrays)
    1. [Strings](#strings)
    1. [Functions](#functions)
    1. [Properties](#properties)
    1. [Variables](#variables)
    1. [Hoisting](#hoisting)
    1. [Comparison Operators & Equiality](#comparison-operators-&-equiality)
    1. [Blocks](#blocks)
    1. [Comments](#comments)
    1. [Whitespace](#whitespace)
    1. [Commas](#commas)
    1. [Semicolons](#semicolons)
    1. [Type Casting & Coercion](#type casting & coercion)
    1. [Naming Conventions](#naming conventions)
    1. [Accessors](#accessors)
    1. [Constructors](#Constructors)
    1. [Events](#events)
    1. [Single Literal Object](#single-literal-object)
    1. [jQuery](#jquery)
    1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)
    1. [Performance](#performance)
  1. [Modularity](#modularity)
  1. [Angular Docs](#angular-docs)
  1. [References](#references)


## Guide to Start Coding in Consumer Portal Code Base

- Understand the [application structure](#application-structure), [component coding guidelines](#angular-component-coding) and [naming patterns](#naming-guidelines) before diving into coding for consumer portal project.

- Create the required angular components (directive/controller/data service etc...) for your task/feature.

- Go through the [asynchronous module definition](#asynchronous-module-definition) guidelines, this will allow you to understand how to configure your component to be loaded into angular application through RequireJS.

- Once you configure the RequireJS configuration with your new component, you can update your view (incase you are using directive, or setup the route template in webcenter for new route). and test your changes through application.

- As part of testing your changes, create the unit test spec file for your component code. then use [testing guidelines](#testing) to setup your spec file with testing framework, and run the unit test and make sure atleast 80% of your functional code is unit tested.

## Application Structure

### Single Responsibility

  - Define 1 component per file.

  The following example defines the `app` module and its dependencies, defines a controller, and defines a factory all in the same file.

  ```javascript
  /* avoid */
  angular
      .module('app', ['ngRoute'])
      .controller('SomeController', SomeController)
      .factory('someFactory', someFactory);

  function SomeController() { }

  function someFactory() { }
  ```

  The same components are now separated into their own files.

  ```javascript
  /* recommended */

  // app.js
  angular
      .module('app', ['ngRoute']);
  ```

  ```javascript
  /* recommended */

  // someCtrl.js
  angular
      .module('app')
      .controller('SomeController', SomeController);

  function SomeController() { }
  ```

  ```javascript
  /* recommended */

  // someSvc.js
  angular
      .module('app')
      .factory('someFactory', someFactory);

  function someFactory() { }
  ```

### Directory Structure

    ```
    .
    ├── app
    │   ├── app.js
    │   ├── common
    │   │   ├── controllers
    │   │   │   ├── firstCtrl.js
    │   │   │   └── secondCtrl.js
    │   │   └── directives
    │   │       └── firstDir.js
    │   ├── dashboard
    │   │   ├── controllers
    │   │   |    └── firstCtrl.js
    │   │   ├── directives
    │   │   |    └── firstDir.js
    │   │   ├── services
    │   │   │   └── dirstSvc.js
    │   │   └── filters
    │   │       └── firstFilt.js
    │   ├── benefits
    │   │   ├── controllers
    │   │   |    └── firstCtrl.js
    │   │   ├── directives
    │   │   |    └── firstDir.js
    │   │   ├── services
    │   │   │   └── dirstSvc.js
    │   │   └── filters
    │   │       └── firstFilt.js
    │   ├── assets
    │   │   ├── images
    │   │   |    └── applicatio-specific-images.js
    │   │   ├── styles
    │   │   |    └── css-style-sheets.js
    │   │   ├── scripts
    │   │   |    └── other-script-files.js
    │   │   └── libs
    │   │   |    └── external-library.js
    │   └── etc...
    ├── bower_components
    └── test
        ├── common
        ├── benefits
        └── dashboard
    ```

### Overall Guidelines
  - Organize files first by major feature, then by file type.

  - Common/shared files for the app should go into “common”.

  - Each feature folder will have subfolders for different file types
      controllers
      models
      directives
      services
      views

  - Images, CSS, and other JS libraries go under “assets”

  - Unit test files go into the same folder as their target. Unit test files should have “.spec.” in their file names
  
  - File naming:
      Directory names should be all lower case - in case the directory name contains multiple words, use lisp-case syntax. Example: secure-messaging.
      See [Naming Guidelines](#naming) to understnad how file naming and angular component naming should be handled.

#### Folders by Feature Structure

  - Create folders named for the feature they represent. When a folder grows to contain more than 7 files, start to consider creating a folder for them. 

    *Why?*: A developer can locate the code, identify what each file represents at a glance, the structure is flat as can be, and there is no repetitive nor redundant names.

    *Why?*: Helps reduce the app from becoming cluttered through organizing the content and keeping them aligned with the guidelines.

    *Why?*: When there are a lot of files (10+) locating them is easier with a consistent folder structures and more difficult in flat structures.

    Note: Do not use structuring using folders-by-type. This requires moving to multiple folders when working on a feature and gets unwieldy quickly as the app grows to 5, 10 or 25+ views and controllers (and other features), which makes it more difficult than folder-by-feature to locate files.

### Naming Guidelines

  - Organize files first by major feature, then by file type.
    * the file path (`app\benefits\medicalCtrl.js`)
    * the registered component name with Angular (`MedicalController` for controller components)

    *Why?*: Naming conventions help provide a consistent way to find content at a glance. Consistency within the project is vital. Consistency with a team is important. Consistency across a company provides tremendous efficiency.

    *Why?*: The naming conventions should simply help you find your code faster and make it easier to understand.

#### File Names by Type

  - Use consistent names for all components following a pattern that describes the component's feature then (optionally) its type. 
    * Append 'Ctrl' suffix for cotnroller file name
    * Append 'Svc' suffix for data services that are using factory components. Excempt to this, if service or factory is used to define common feature in the application (ex: session handler, cookie helper etc...)
    * Append 'Dir' suffix for any directive defined in the application.
    * Append 'Filt' suffix for any filter defined in the application.
    * Append 'Module' suffix for any module definition in the application.
    * Append 'Config' suffix for any configuration initialization for given module in the application. ex: moduleNameConfig.js -> for app module appConfig.js
    * Append 'Const' suffix for any constant definitions in module. ex: moduleNameConst.js -> for app module appConst.js


    *Why?*: Provides a consistent way to quickly identify components.

    *Why?*: Provides pattern matching for any automated tasks.

    ```javascript
    /**
     * recommended
     */

    // controllers
    medicalCtrl.js
    medicalCtrl.spec.js

    // data service using factory component
    dataSvc.js
    dataSvc.spec.js

    // filter
    benefitFilt.js
    benefitFilt.spec.js

    // directives
    modalDir.js
    modalDir.spec.js

    // module
    adminModule.js

    // module specific routes
    appRoutes.js
    appRoutes.spec.js

    // module specific configuration
    appConfig.js

    // module specific constants
    appConst.js
    adminConst.js

    ```

#### Test File Naming

  - Name test specifications similar to the component they test with a suffix of `spec`.

    *Why?*: Provides a consistent way to quickly identify components.

    *Why?*: Provides pattern matching for [karma](http://karma-runner.github.io/) or other test runners.

    ```javascript
    /**
     * recommended
     */
    medicalCtrl.spec.js
    loggerSvc.spec.js
    appRoutes.spec.js
    modalDir.spec.js
    ```

#### Controller Naming

  - Use consistent names for all controllers named after their feature. Use UpperCamelCase for controllers, as they are constructors.

    *Why?*: Provides a consistent way to quickly identify and reference controllers.

    *Why?*: UpperCamelCase is conventional for identifying object that can be instantiated using a constructor.

  - Append the controller name with the suffix `Controller`.

    *Why?*: The `Controller` suffix is more commonly used and is more explicitly descriptive.

    ```javascript
    /**
     * recommended
     */

    // medicalCtrl.js
    'use strict';
    define(['app'], function (app) {

        var injectParams = ['$scope',
                            '$location',
                            '$filter',
                            '$window',
                            '$timeout',
                            'cookieHelper'];

        var MedicalController = function ($scope,
                                       $location,
                                       $filter,
                                       $window,
                                       $timeout,
                                       cookieHelper) {
                                         
            var vm = this;
            init();

            function init() {
            }


        };

        MedicalController.$inject = injectParams;
        app.register.controller('MedicalCtrl', MedicalController);
    });

    ```



#### Factory Naming

  - Use consistent names for all factories named after their feature. Use camel-casing for services and factories. Avoid prefixing factories and services with `$`.

  - Use Factories for data service and http interceptor definitions.

    *Why?*: Provides a consistent way to quickly identify and reference factories.

    *Why?*: Avoids name collisions with built-in factories and services that use the `$` prefix.

    ```javascript
    /**
     * recommended for data services
     */

    // claimSvc.js
    'use strict';
    define(['app'], function (app) {

      var injectParams = ['$q',
                          '$location',
                          '$localStorage'];

      var claimService = function ($q,
                                    $location,
                                    $localStorage) {
        var factory = {
          getClaimSummary: getClaimSummary,
          getClaimDetails: getClaimDetails
        };

        function getClaimSummary() {
        }

        function getClaimDetails() {
        }
      };

      claimService.$inject = injectParams;

      app.factory('claimService', claimService);

    });
    ```

#### Service Naming

  - Use angular service for common feature components. Ex: cookie helper, session helper

    ```javascript
    /**
     * services are recommended for common feature components
     * Ex: session helper, cookie helper
     * Below example is for cookie helper component
     */

    // cookieHlpr.js
    'use strict';
    define(['app'], function (app) {

      var injectParams = ['$q',
                          '$location',
                          '$localStorage'];

      var cookieHelper = function ($q,
                                    $location,
                                    $localStorage) {
        this.getValue = function(){
          ////////
        };

        this.setValue = function(){
          ////////
        };

        function internalFunction1() {
        }

        function internalFunction2() {
        }
      };

      cookieHelper.$inject = injectParams;

      app.service('cookieHelper', cookieHelper);

    });

    ```

#### Directive Naming

  - Use consistent names for all directives using camel-case. Use a short prefix to describe the area that the directives belong (use "tcp" prefix for consumer portal directive components).

    *Why?*: Provides a consistent way to quickly identify and reference components.

    ```javascript
    /**
     * recommended
     */

    // ajaxLoadingDir.js
    ﻿'use strict';

    define(['app'], function (app) {

        var injectParams = ['$q',
                            '$parse',
                            '$location',
                            '$filter',
                            '$window',
                            '$timeout',
                            'ajaxInterceptor'];

        var ajaxLoadingDir = function ($q,
                                       $parse,
                                       $location,
                                       $filter,
                                       $window,
                                       $timeout,
                                       ajaxInterceptor) {
            return {
                restrict: 'AEC',
                templateUrl: 'common/views/ajaxLoading.html',
                scope: {
                },
                link: function (scope, element, attrs, controllers) {
                    scope.queue = [];
                    init();

                    function init() {
                        wireUpAjaxInterceptor();
                    }
                    function wireUpAjaxInterceptor() {
                    }
                }
            };
        };

        ajaxLoadingDir.$inject = injectParams;

        //'tcp' prefix used for component naming only.
        app.directive('tcpAjaxLoadingDir', ajaxLoadingDir);
        //below register can be used if directive is registered after angular.bootstrap
        //app.register.directive('tcpAjaxLoading', ajaxLoading);

    });

    ```

#### Filter Naming

- Use 'Filter' suffix for component name.

    ```javascript
    ﻿'use strict';
    define(['app'], function (app) {

      var claimTypeFilter = function () {

          return function (value) {
              var result = "Unknown";
              switch (value) {
                  case -1:
                      result = "View All"; break;
                  case 1:
                      result = "Medical"; break;
                  case 2:
                      result = "Pharmacy"; break;
                  case 0:
                      result = "Dental"; break;
                  case 3:
                      result = "Vision"; break;

              }
              return result;
          };
      };

      app.register.filter('claimTypeFilter', claimTypeFilter);
    });

    ```

#### Module Naming

  - When there are multiple modules, the main module file is named `app.js` while other dependent modules are named after what they represent. For example, an admin module is named `adminModule.js`. The respective registered module names would be `app` and `admin`.

    *Why?*: Provides consistency for multiple module apps, and for expanding to large applications.

    *Why?*: Provides easy way to use task automation to load all module definitions first, then all other angular files (for bundling).

##### Module Configuration Naming

  - Separate configuration for a module into its own file named after the module. A configuration file for the main `app` module is named `appConfig.js`. A configuration for a module named `adminModule.js` is named `adminConfig.js`.

    *Why?*: Separates configuration from module definition, components, and active code.

    *Why?*: Provides an identifiable place to set configuration for a module.


#### Routes Naming

  - Separate route configuration into its own file. Examples will be `appRoutes.js` for the main module and `adminRoutes.js` for the `admin` module.

### Asynchronous Module Definition

- We use RequireJS to load the angular components asynchronously when required.

*Why?*: This helps modules to be loaded asynchrnously through requirejs. Helps to reduce the initial javascript file size loaded to the browser and to improve performance.

#### RequireJS Module Definition

  - Wrap Angular components in an RequireJs module definition.

  ```javascript
  /* avoid */
  // loggerSvc.js
  angular
      .module('app')
      .factory('logger', logger);

  // logger function is added as a global variable
  function logger() { }

  // storageSvc.js
  angular
      .module('app')
      .factory('storage', storage);

  // storage function is added as a global variable
  function storage() { }
  ```

  ```javascript
  /**
   * recommended
   *
   */

  // loggerSvc.js
  ﻿'use strict';

  //requirejs definition for the current component
  define(['app'], function (app) {

      var injectParams = ['$q'];

      var loggerService = function ($q) {

          this.someFunction2 = function (objectName) {
          };

          this.someFunction2 = function (objectName, objectValue) {
          };

      };

    loggerService.$inject = injectParams;

    app.service('loggerService', loggerService);

  });

  ```

  - Use app.service, app.controller, app.directive, app.filter, app.factory etc to register the angular component with app module. 

  - This method of component registering can be used before angular.bootstrap statement is exected.

  - For requirejs components loaded with below statement.

  ```javascript
  /**
   * recommended
   *
   */

  //bootstrap the angular app to load after initial require js modules are loaded for application start
  //don't include all the modules defined above in this, this should only specify the initial modules that are needed for startup the app.
  //modules listed below will be bundled to single js file while building the application for deployment.
  require([ 'app',
            'someDir',
            'someCtrl',
            'someSvc',
            'someFac',
            'someFilt'
      ], function () {
          angular.bootstrap(document, ['anthemMemberPortal']);
  });

  ```


  ```javascript
  /**
   * usage
   *
   */

  define(['app'], function (app) {

      ....

    //if component is a service
    app.service('someSvc', someService);
    //if component is a factory
    app.factory('someFac', someFactory);
    //if component is a controller
    app.controller('someCtrl', someController);
    //if component is a controller
    app.filter('someFltr', someFilter);
    //if component is a directive
    app.directive('someDir', someDirective);

  });

  ```

  - Use app.register.service, app.register.controller, app.register.directive, app.register.filter, app.register.factory etc to register the angular component with app module.

  - This method of component registering can be used after angular.bootstrap statement is exected.

  ```javascript
  /**
   * recommended
   *
   */

  define(['app'], function (app) {

      ....

    //if component is a service
    app.register.service('someSvc', someService);
    //if component is a factory
    app.register.factory('someSvc', someService);
    //if component is a controller
    app.register.controller('someCtrl', someController);
    //if component is a controller
    app.register.filter('someFltr', someFilter);
    //if component is a directive
    app.register.filter('someDir', someDirective);

  });

  ```

#### RequireJS Configuration

- Before using RequireJS to load components asynchronously, we need to configure RequireJS to specify what all components are available in our application and how to get to those script files (so when requested to be loaded, it knows which url to use to load the script file in browser)

- RequireJS configuration is defined in \app\core\requirejsConfig.js file. Following is the current(4/13/2015)  of that file.

```javascript
﻿//this allows loading only the required js files with initial application
require.config({
    baseUrl: '',
    //build hash value used to browser cache busting. this value will be updated with build process. so don't make any changes to this value.
    urlArgs: '@buildHash',
    //specify the javascript file location with abbreviation. any angular component should be listed in below array to bve used with RequireJS.
    paths: {
        //abbrevation: "path to your angular component script file from app folder root, don't use js extension when giving path here"
        'app': 'app',
        'appCfg': 'core/appConfig',
        'alDir': 'common/directives/ajaxLoadingDir',
        'tpDir': 'common/directives/tooltipPopoverDir',
        'cdDir': 'claims/directives/claimsDropdownDir',
        'benefitSrvc': 'benefits/services/benefitsSrvc',
        'providerSrvc': 'provider/services/providerSrvc',
        'claimsSrvc': 'claims/services/claimsSrvc',
        'userSrvc': 'register/userSrvc',
        'bFilt': 'benefits/filters/benefitFltr',
        'cFilt': 'claims/filters/claimsFltr',
        'cpDir': 'benefits/directives/coveragePeriodDir',
        'sDir': 'common/directives/sessionDir',
        'tnDir': 'common/directives/topNavDir',
        'clDir': 'claims/directives/claimsListDir',
        'cHlpr': 'common/services/cookieHlpr',
        'contSrvc': 'common/services/contentSrvc',
        'msgSrvc': 'common/services/messageSrvc',
        'msgDir': 'common/directives/contentMessageDir',
        'mainCtrl': 'home/controllers/mainCtrl',
        'loginCtrl': 'login/controllers/loginCtrl',
        'regCtrl': 'register/registerCtrl',
        'regDir': 'register/registerDir',
        'dashCtrl': 'dashboard/controllers/dashboardCtrl',
        'coCtrl': 'claims/controllers/claimsOverviewCtrl',
        'medCtrl': 'benefits/controllers/medicalCtrl',
        'authSrvc': 'login/services/authSrvc',
        'authIntSrvc': 'common/services/httpInterceptor',
        'modHlpr': 'common/services/modalHlpr',
        'wtDir': 'common/directives/warnTimerDir',
        'siHlpr': 'common/services/sessionIdleHlpr',
        'wcsCtrl': 'common/controllers/wcsCtrl',
        'cdeDir' : 'benefits/directives/coverageDetailsDir',
        'doDir': 'benefits/directives/deductibleOopDir',
        'welDir': 'dashboard/directives/welcomeToDir',
        'pdDir': 'common/directives/pageHeadDataDir'
    },
    //if any of the above listed files have additional dependencies, they can be listed below.
    shim: {
        /* assume we have defined test1, test2, test3 components in above "path" array. 
           and test1 has dependencies with test2 and test3, meaning test2 and test3 needs to loaded before loading test1 component.
        'test1' : {
          deps: ['test2', 'test3'],
          exports: 'test1'
        }
        */
        'dashCtrl': {
            deps: ['clDir', 'msgDir','welDir'],
            exports: 'dashCtrl'
        },
        'coCtrl': {
            deps: ['clDir'],
            exports: 'cOverCtrl'
        },
        'medCtrl': {
            deps: ['benefitSrvc', 'cpDir', 'cdeDir', 'doDir'],
            exports: 'medCtrl'
        },
        'regCtrl': {
            deps: ['userSrvc', 'regDir'],
            exports: 'regCtrl'
        },
        'clDir': {
            deps: ['claimsSrvc', 'cFilt', 'cdDir'],
            exports: 'clDir'
        },
        'cpDir': {
            deps: ['bFilt', 'benefitSrvc', 'providerSrvc'],
            exports: 'cpDir'
        },
        'cdeDir': {
            deps: ['bFilt'],
            exports: 'cdeDir'
        },
        'doDir': {
            deps: ['bFilt'],
            exports: 'doDir'
        }
    }
});

//bootstrap the angular app to load after initial require js modules are loaded for application start
//don't include all the modules defined above in this, this should only specify the initial modules that are needed for startup the app.
//modules listed below will be bundled to single js file while building the application for deployment.
require(['common/services/routeResolver',
          'app',
          'appCfg',
          'authIntSrvc',
          'wtDir',
          'modHlpr',
          'siHlpr',
          'alDir',
          'tpDir',
          'cHlpr',
          'authSrvc',
          'msgSrvc',
          'sDir',
          'tnDir',
          'pdDir'
    ], function () {
        angular.bootstrap(document, ['anthemMemberPortal']);
});

```

##### Define New Angular Component Path.

- Define the directive/filter/service/controller path in "paths" array.

```javascript
paths: {
        ////////////
        'testDir' : 'test/directives/testDir' // abbrevation for directive : directive path (from components folder)
        'testCtrl' : 'test/controllers/testCtrl' // abbrevation for controller : controller path (from components folder)
        /////////
    }
```

##### Define Sub Dependencies

Note: Ex: Assume testCtrl define above require/use testDir directive. so before initiating the controller we need to make sure browser also load the dependent directive.

- To achieve this, we will use the "shim" array to define sub dependencies like above.

```shell
shim: {
        'dashCtrl': {
            deps: ['cFilt', 'cdDir', 'clDir', 'msgDir'],
            exports: 'dashCtrl'
        },
        etc...
    //below shim object define the sub dependecy between testCtrl and testDir
        'testCtrl': {
            deps: ['testDir'], // list any sub dependent modules in this array for testCtrl. (requirejs will make sure these dependencies are loaded prior to loading testCtrl in browser.
            exports: 'testCtrl'
        }
    }
```

##### Define Main Dependencies for angular.bootstrap

- If any controller/service/directive/filter is required before angular app is initiated (bootstrap). list those dependencies in "require" function.

- In most cases you won't need to load any directives/controllers in this, since those will be loaded per route basis. if you are using any framework level services or data services that will be executed as part of angular.config or angular.run, you will need to include them in this configuration section. so they are pre-loaded to browser when angular start bootstrap and execute its own angaulr.config and angular.run functions.

```javascript
require(
//array with required modules/components before angular app bootstrap. requirejs will make sure these modules are pre-loaded into browser before bootstrap is executed.
['common/services/routeResolver',
          'app',
          'authIntSrvc',
          etc...
          'tnDir'
    ], function () {
    angular.bootstrap(document, ['anthemPocApp']);
});
```


## Angular Component Coding

### Modules

- Avoid Naming Collisions

  - Use unique naming conventions with separators for sub-modules.

  *Why?*: Unique names help avoid module name collisions. Separators help define modules and their submodule hierarchy. For example `app` may be your root module while `app.providerfinder` and `app.admin` may be modules that are used as dependencies of `app`.

  - Inject code into [module configuration](https://docs.angularjs.org/guide/module#module-loading-dependencies) that must be configured before running the angular app. Ideal candidates include providers and constants.

    *Why?*: This makes it easier to have a less places for configuration.

  ```javascript
  app.config(configure);

  configure.$inject =
      ['routerHelperProvider', 'exceptionHandlerProvider', 'toastr'];

  function configure (routerHelperProvider, exceptionHandlerProvider, toastr) {
      exceptionHandlerProvider.configure(config.appErrorPrefix);
      configureStateHelper();

      ////////////////

      function configureStateHelper() {
          routerHelperProvider.configure({
              docTitle: 'NG-Modular: '
          });
      }
  }
  ```

  - Any code that needs to run when an application starts should be declared in a factory, exposed via a function, and injected into the [run block](https://docs.angularjs.org/guide/module#module-loading-dependencies).

    *Why?*: Code directly in a run block can be difficult to test. Placing in a factory makes it easier to abstract and mock.

  ```javascript
  app.run(runBlock);

  runBlock.$inject = ['authenticator', 'translator'];

  function runBlock(authenticator, translator) {
      authenticator.initialize();
      translator.initialize();
  }
  ```




### Controllers

- Do not manipulate DOM in your controllers, this will make your controllers harder for testing and will violate the Separation of Concerns principle. Use directives instead.

- Use below Controller definition for new controller creation.

  ```javascript
  'use strict'; //used to make sure global varialbles are not used.

  define(['app'], function (app) {

      //define any dependency injections using this variable, so minifications won't break the DI parameter naming.
      var injectParams = ['$scope'];

      //specify the dipendency injected variable names as function parameters.
      var SomeController = function ($scope) {
                                       
          var vm = this;
          vm.translationComplete = true;
          init();

          //initialization code goes into this function
          function init() {
          }


      };

      //manually inject  dependency injected variable array.
      SomeController.$inject = injectParams;

      // use app.register since we load the controllers asynchronously using requirejs for each route
      app.register.controller('SomeCtrl', SomeController);
  });
  ```

  - Define a controller for a view, and try not to reuse the controller for other views. Instead, move reusable logic to factories and keep the controller simple and focused on its view. Exception for this would be webcenter specific pages that doesn't have any controller logic and will purely be used as cms page.

    *Why?*: Reusing controllers with several views is brittle and good end to end (e2e) test coverage is required to ensure stability across large applications.

#### Using vm with controllerAs

  - Use a capture variable for `this` when using the `controllerAs` syntax. Choose a consistent variable name such as `vm`, which stands for ViewModel.

  *Why?*: The `this` keyword is contextual and when used within a function inside a controller may change its context. Capturing the context of `this` avoids encountering this problem.

  ```javascript
  /* avoid */
  var SomeController = function ($scope) {                                       
          $scope.translationComplete = true;
          init();

          function init() {
          }
      };
  ```

  ```javascript
  /* recommended */
  var SomeController = function ($scope) {                                       
          var vm = this;
          vm.translationComplete = true;
          init();

          function init() {
          }
      };
  ```

  Note: You can avoid any [jshint](http://www.jshint.com/) warnings by placing the comment above the line of code. However it is not needed when the function is named using UpperCasing, as this convention means it is a constructor function, which is what a controller is in Angular.

  ```javascript
  /* jshint validthis: true */
  var vm = this;
  ```

  Note: When creating watches in a controller using `controller as`, you can watch the `vm.*` member using the following syntax. (Create watches with caution as they add more load to the digest cycle.)

  ```html
  <input ng-model="vm.title"/>
  ```

  ```javascript
  var SomeController = function ($scope, $log) {  
      var vm = this;
      vm.title = 'Some Title';

      $scope.$watch('vm.title', function(current, original) {
          $log.info('vm.title was %s', original);
          $log.info('vm.title is now %s', current);
      });
  };
  ```

#### Bindable Members and Functions

  - Place bindable members at the top of the controller, alphabetized, and not spread through the controller code.

    *Why?*: Placing bindable members at the top makes it easy to read and helps you instantly identify which members of the controller can be bound and used in the View.

    *Why?*: Setting anonymous functions in-line can be easy, but when those functions are more than 1 line of code they can reduce the readability. Defining the functions below the bindable members (the functions will be hoisted) moves the implementation details down, keeps the bindable members up top, and makes it easier to read.

  ```javascript
  /* avoid */
  var SomeController = function ($scope, $log) {  
      var vm = this;

      vm.gotoSession = function() {
        /* ... */
      };
      vm.refresh = function() {
        /* ... */
      };
      vm.search = function() {
        /* ... */
      };
      vm.sessions = [];
      vm.title = 'Sessions';
  ```

  ```javascript
  /* recommended */
  var SomeController = function ($scope, $log) {  
      var vm = this;

      vm.gotoSession = gotoSession;
      vm.refresh = refresh;
      vm.search = search;
      vm.sessions = [];
      vm.title = 'Sessions';

      ////////////

      function gotoSession() {
        /* */
      }

      function refresh() {
        /* */
      }

      function search() {
        /* */
      }
  ```

  Note: If the function is a 1 liner consider keeping it right up top, as long as readability is not affected.

  ```javascript
  /* avoid */
  var SomeController = function ($scope) {  
      var vm = this;

      vm.gotoSession = gotoSession;
      vm.refresh = function() {
          /**
           * lines
           * of
           * code
           * affects
           * readability
           */
      };
      vm.search = search;
      vm.sessions = [];
      vm.title = 'Sessions';
  ```

  ```javascript
  /* recommended */
  var SomeController = function ($scope, dataservice) {  
      var vm = this;

      vm.gotoSession = gotoSession;
      vm.refresh = dataservice.refresh; // 1 liner is OK
      vm.search = search;
      vm.sessions = [];
      vm.title = 'Sessions';
  ```

##### Function Usage in Controllers

  - Use function declarations to hide implementation details. Keep your bindable members up top. When you need to bind a function in a controller, point it to a function declaration that appears later in the file. This is tied directly to the section Bindable Members Up Top. For more details see [this post](http://www.johnpapa.net/angular-function-declarations-function-expressions-and-readable-code).

    *Why?*: Placing bindable members at the top makes it easy to read and helps you instantly identify which members of the controller can be bound and used in the View. (Same as above.)

    *Why?*: Placing the implementation details of a function later in the file moves that complexity out of view so you can see the important stuff up top.

    *Why?*: Function declaration are hoisted so there are no concerns over using a function before it is defined (as there would be with function expressions).

    *Why?*: You never have to worry with function declarations that moving `var a` before `var b` will break your code because `a` depends on `b`.

    *Why?*: Order is critical with function expressions

  ```javascript
  /**
   * avoid
   * Using function expressions.
   */
  var SomeController = function (dataservice, logger) {  
      var vm = this;
      vm.avengers = [];
      vm.title = 'Avengers';

      var activate = function() {
          return getAvengers().then(function() {
              logger.info('Activated Avengers View');
          });
      }

      var getAvengers = function() {
          return dataservice.getAvengers().then(function(data) {
              vm.avengers = data;
              return vm.avengers;
          });
      }

      vm.getAvengers = getAvengers;

      activate();
  }
  ```

  Notice that the important stuff is scattered in the preceding example. In the example below, notice that the important stuff is up top. For example, the members bound to the controller such as `vm.avengers` and `vm.title`. The implementation details are down below. This is just easier to read.

  ```javascript
  /*
   * recommend
   * Using function declarations
   * and bindable members up top.
   */
  var SomeController = function (dataservice, logger) {  
      var vm = this;
      vm.avengers = [];
      vm.getAvengers = getAvengers;
      vm.title = 'Avengers';

      activate();

      function activate() {
          return getAvengers().then(function() {
              logger.info('Activated Avengers View');
          });
      }

      function getAvengers() {
          return dataservice.getAvengers().then(function(data) {
              vm.avengers = data;
              return vm.avengers;
          });
      }
  }
  ```

#### Services and Controller Logic

  - Defer logic in a controller by delegating to services and factories.

    *Why?*: Logic may be reused by multiple controllers when placed within a service and exposed via a function.

    *Why?*: Logic in a service can more easily be isolated in a unit test, while the calling logic in the controller can be easily mocked.

    *Why?*: Removes dependencies and hides implementation details from the controller.

    *Why?*: Keeps the controller slim, trim, and focused.

  ```javascript

  /* avoid */
  var SomeController = function ($http, $q, config, userInfo) {  
      var vm = this;
      vm.checkCredit = checkCredit;
      vm.isCreditOk;
      vm.total = 0;

      function checkCredit() {
          var settings = {};
          // Get the credit service base URL from config
          // Set credit service required headers
          // Prepare URL query string or data object with request data
          // Add user-identifying info so service gets the right credit limit for this user.
          // Use JSONP for this browser if it doesn't support CORS
          return $http.get(settings)
              .then(function(data) {
               // Unpack JSON data in the response object
                 // to find maxRemainingAmount
                 vm.isCreditOk = vm.total <= maxRemainingAmount
              })
              .catch(function(error) {
                 // Interpret error
                 // Cope w/ timeout? retry? try alternate service?
                 // Re-reject with appropriate error for a user to see
              });
      };
  }
  ```

  ```javascript
  /* recommended */
  var SomeController = function ($http, $q, creditService) {  
      var vm = this;
      vm.checkCredit = checkCredit;
      vm.isCreditOk;
      vm.total = 0;

      function checkCredit() {
         return creditService.isOrderTotalOk(vm.total)
            .then(function(isOk) { vm.isCreditOk = isOk; })
            .catch(showServiceError);
      };
  }
  ```

### Controller Initialization

  - Resolve start-up logic for a controller in an `init` function.

    *Why?*: Placing start-up logic in a consistent place in the controller makes it easier to locate, more consistent to test, and helps avoid spreading out the activation logic across the controller.

    *Why?*: The controller `init` makes it convenient to re-use the logic for a refresh for the controller/View, keeps the logic together, gets the user to the View faster, makes animations easy on the `ng-view` or `ui-view`, and feels snappier to the user.

  ```javascript
  /* avoid */
  var SomeController = function ($scope, dataservice) {
      var vm = this;
      vm.avengers = [];
      vm.title = 'Avengers';

      dataservice.getAvengers().then(function(data) {
          vm.avengers = data;
          return vm.avengers;
      });
  }
  ```

  ```javascript
  /* recommended */
  var SomeController = function ($scope, dataservice) {
      var vm = this;
      vm.avengers = [];
      vm.title = 'Avengers';

      init();

      ////////////

      function init() {
          return dataservice.getAvengers().then(function(data) {
              vm.avengers = data;
              return vm.avengers;
          });
      }
  }
  ```

#### Controllers with Routes

  - When a controller must be paired with a view and either component may be re-used by other controllers or views, define controllers along with their routes.

    Note: If a View is loaded via another means besides a route, then use the `ng-controller="Avengers as vm"` syntax.

    *Why?*: Pairing the controller in the route allows different routes to invoke different pairs of controllers and views. When controllers are assigned in the view using [`ng-controller`](https://docs.angularjs.org/api/ng/directive/ngController), that view is always associated with the same controller.

    Note: Some of the sample code below is purely for understanding, actual route configuration will be defined differently in actual code base.

 ```javascript
  /* avoid - when using with a route and dynamic pairing is desired */

  // route-config.js
  angular
      .module('app')
      .config(config);

  function config($routeProvider) {
      $routeProvider
          .when('/avengers', {
            templateUrl: 'avengers.html'
          });
  }
  ```

  ```html
  <!-- avengers.html -->
  <div ng-controller="Avengers as vm">
  </div>
  ```

  ```javascript
  /* recommended */

  // route-config.js
  angular
      .module('app')
      .config(config);

  function config($routeProvider) {
      $routeProvider
          .when('/avengers', {
              templateUrl: 'avengers.html',
              controller: 'Avengers',
              controllerAs: 'vm'
          });
  }
  ```

  ```html
  <!-- avengers.html -->
  <div>
  </div>
  ```

### Services

  - Services are instantiated with the `new` keyword, use `this` for public methods and variables.

    Note: [All Angular services are singletons](https://docs.angularjs.org/guide/services). This means that there is only one instance of a given service per injector.

  ```javascript
  // service
  ﻿'use strict';

  define(['app'], function (app) {

      var injectParams = ['$q',
                          '$cookieStore'];

      var someService = function ($q,
                                   $cookieStore) {

          this.getValue = function (objectName) {
          };

          this.setValue = function (objectName, objectValue) {
          };

          function setCookie(appCookie) {
          };

          function getCookie() {
          };

      };

      someService.$inject = injectParams;

      app.service('someSvc', someService);
      //below register can be used if service is registered after angular.bootstrap
      //app.register.service('someSvc', someService);

  });

  ```


### Factories

  - Factories should have a [single responsibility](http://en.wikipedia.org/wiki/Single_responsibility_principle), that is encapsulated by its context. Once a factory begins to exceed that singular purpose, a new factory should be created.

  - Factories are singletons and return an object that contains the members of the service.

    Note: [All Angular services are singletons](https://docs.angularjs.org/guide/services).

#### Public Members and Functions

  - Expose the callable members of the service (its interface) at the top, using a technique derived from the [Revealing Module Pattern](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#revealingmodulepatternjavascript).

    *Why?*: Placing the callable members at the top makes it easy to read and helps you instantly identify which members of the service can be called and must be unit tested (and/or mocked).

    *Why?*: This is especially helpful when the file gets longer as it helps avoid the need to scroll to see what is exposed.

    *Why?*: Setting functions as you go can be easy, but when those functions are more than 1 line of code they can reduce the readability and cause more scrolling. Defining the callable interface via the returned service moves the implementation details down, keeps the callable interface up top, and makes it easier to read.


  ```javascript
  // factory
  ﻿'use strict';

  define(['app'], function (app) {

      var injectParams = ['$http',
                          '$q',
                          'cookieHelper',
                          '$translate',
                          'contentModules'];

      var someFactory = function ($http,
                                     $q,
                                     cookieHelper,
                                     $translate,
                                     contentModules) {
                                       
          var serviceBase = 'rest:api:url',
              factory = {
                        anotherPublicFunction: anotherPublicFunction,
                        loadContent: loadContent,
                        publicProperty: true
                      };

          return factory;

          function loadContent(param) {

              //call rest api to get data
              return $http.post(serviceBase + 'operation?param=' + param, {}).then(function (results) {
              }, function (error) {
              });

              function someFunction(module) {
              }

          };

          function anotherPublicFunction(){
          };
      };

      someFactory.$inject = injectParams;

      app.factory('someFac', someFactory);
      //below register can be used if service is registered after angular.bootstrap
      app.register.factory('someFac', someFactory);

  });

  ```

#### Function Declarations

  - Use function declarations to hide implementation details. Keep your accessible members of the factory up top. Point those to function declarations that appears later in the file. For more details see [this post](http://www.johnpapa.net/angular-function-declarations-function-expressions-and-readable-code). Refer above factory code.

    *Why?*: Placing accessible members at the top makes it easy to read and helps you instantly identify which functions of the factory you can access externally.

    *Why?*: Placing the implementation details of a function later in the file moves that complexity out of view so you can see the important stuff up top.

    *Why?*: Function declaration are hoisted so there are no concerns over using a function before it is defined (as there would be with function expressions).

    *Why?*: You never have to worry with function declarations that moving `var a` before `var b` will break your code because `a` depends on `b`.

    *Why?*: Order is critical with function expressions


### REST API Wrapper Services

  - Refactor logic for making data operations and interacting with data to a factory. Make data services responsible for XHR calls, local storage, stashing in memory, or any other data operations.

    *Why?*: The controller's responsibility is for the presentation and gathering of information for the view. It should not care how it gets the data, just that it knows who to ask for it. Separating the data services moves the logic on how to get it to the data service, and lets the controller be simpler and more focused on the view.

    *Why?*: This makes it easier to test (mock or real) the data calls when testing a controller that uses a data service.

    *Why?*: Data service implementation may have very specific code to handle the data repository. This may include headers, how to talk to the data, or other services such as `$http`. Separating the logic into a data service encapsulates this logic in a single place hiding the implementation from the outside consumers (perhaps a controller), also making it easier to change the implementation.


  ```javascript
  // factory
  ﻿'use strict';

  define(['app'], function (app) {

      var injectParams = ['$http',
                          '$q',
                          'cookieHelper',
                          '$translate',
                          'contentModules'];

      var someDataService = function ($http,
                                     $q,
                                     cookieHelper,
                                     $translate,
                                     contentModules) {
                                       
          var serviceBase = 'rest:api:url',
              factory = {
                        getData: getData
                      };

          return factory;

          function getData(param) {

              //call rest api to get data
              return $http.post(serviceBase + 'operation?param=' + param, {})
                          .then(handleSuccess, handleError);

              function handleSuccess(results) {
              }

              function handleError(error) {
              }

          };

          function anotherPublicFunction(){
          };
      };

      someDataService.$inject = injectParams;

      app.factory('someDataSvc', someDataService);
      //below register can be used if service is registered after angular.bootstrap
      app.register.factory('someDataSvc', someDataService);

  });

  ```

    Note: The data service is called from consumers, such as a controller, hiding the implementation from the consumers, as shown below.

  ```javascript
  /* recommended */

  // controller calling the dataservice factory
  'use strict';
  define(['app'], function (app) {

      var injectParams = ['$scope',
                          '$location',
                          '$filter',
                          '$window',
                          '$timeout',
                          'someDataSvc'];

      var SomeController = function ($scope,
                                     $location,
                                     $filter,
                                     $window,
                                     $timeout,
                                     someDataSvc) {
                                       
          $scope.awesomeThings = [1,2,3];
          var vm = this;
          vm.data = [];
          init();

          function init() {
            loadData();
          }

          function loadData(){
            someDataSvc.getData().then(function(data){
              vm.data = data;
            });
          }

      };

      SomeController.$inject = injectParams;

      app.register.controller('SomeCtrl', SomeController);
  });

  ```

### Directives

  - Create one directive per file. Name the file for the directive.

    *Why?*: It is easy to mash all the directives in one file, but difficult to then break those out so some are shared across apps, some across modules, some just for one module.

    *Why?*: One directive per file is easy to maintain.

  - Name your directives with lowerCamelCase.

  - Provide a short, unique and descriptive directive prefix such as `tcpSomeDir` which would be declared in HTML as `tcp-some-dir`.

    *Why?*: The unique short prefix identifies the directive's context and origin. For example a prefix of `tcp-` may indicate that the directive is part of a consumer portal app.

    Note: Avoid `ng-` as these are reserved for Angular directives. Research widely used directives to avoid naming conflicts, such as `ion-` for the [Ionic Framework](http://ionicframework.com/).

  - When manipulating the DOM directly, use a directive. If alternative ways can be used such as using CSS to set styles or the [animation services](https://docs.angularjs.org/api/ngAnimate), Angular templating, [`ngShow`](https://docs.angularjs.org/api/ng/directive/ngShow) or [`ngHide`](https://docs.angularjs.org/api/ng/directive/ngHide), then use those instead. For example, if the directive simply hides and shows, use ngHide/ngShow.

    *Why?*: DOM manipulation can be difficult to test, debug, and there are often better ways (e.g. CSS, animations, templates)

    > Note: "**Best Practice**: Directives should clean up after themselves. You can use `element.on('$destroy', ...)` or `scope.$on('$destroy', ...)` to run a clean-up function when the directive is removed" ... from the Angular documentation.


  ```javascript
  /* recommended */
  /* someDir.js */

  /**
   * @desc order directive that is specific to the order module at a company named Acme
   * @example <div class="tcp-some-dir"></div>
   */
  ﻿'use strict'; //used to makesure global varialbles are not defined.

  define(['app'], function (app) {

      //specify the dependency injected variable names as function parameters.
      var injectParams = ['$q',
                          '$parse',
                          '$location',
                          '$filter',
                          '$window',
                          '$timeout'];

      var someDir = function ($q,
                                     $parse,
                                     $location,
                                     $filter,
                                     $window,
                                     $timeout) {
          return {
              restrict: 'AEC',
              templateUrl: 'common/views/someView.html', //use templateUrl or template as for your requirement
              //isolated scope, you can use shared scope if you need ...
              scope: {
              },
              link: function (scope, element, attrs, controllers) {
                  scope.data = [];
                  init();

                  //initialization code goes into this function
                  function init() {
                      someFunction();
                  }
                  function someFunction() {
                    data = [1,2,3];
                  }
              }
          };
      };

      //manually inject  dependency injected variable array.
      someDir.$inject = injectParams;

      app.directive('tcpSomeDir', someDir);
      //below register can be used if service is registered after angular.bootstrap
      //app.register.directive('tcpSomeDir', someDir);

  });

  ```

    Note: See the [Naming Guidelines](#naming) section for more recommendations.

#### Directive Restrict Guidelines

  - When creating a directive that makes sense as a stand-alone element, allow restrict `E` (custom element) and optionally restrict `A` (custom attribute). Generally, if it could be its own control, `E` is appropriate. General guideline is allow `EA` but lean towards implementing as an element when it's stand-alone and as an attribute when it enhances its existing DOM element.

    *Why?*: It makes sense.

    *Why?*: While we can allow the directive to be used as a class, if the directive is truly acting as an element it makes more sense as an element or at least as an attribute.

    Note: EA is the default for Angular 1.3 +

  ```html
  <!-- avoid -->
  <div class="my-calendar-range"></div>
  ```

  ```html
  <!-- recommended -->
  <my-calendar-range></my-calendar-range>
  <div my-calendar-range></div>
  ```

#### Use of ControllerAs with Directive

  - Use `controller as` syntax with a directive to be consistent with using `controller as` with view and controller pairings.

    *Why?*: It makes sense and it's not difficult.

    Note: The directive below demonstrates some of the ways you can use scope inside of link and directive controllers, using controllerAs. I in-lined the template just to keep it all in one place.

    Note: Regarding dependency injection, see [Manually Identify Dependencies](#manual-annotating-for-dependency-injection).

    Note: Note that the directive's controller is outside the directive's closure. This style eliminates issues where the injection gets created as unreachable code after a `return`.

  ```html
  <div my-example max="77"></div>
  ```

  ```javascript

  ...
  return {
      restrict: 'EA',
      templateUrl: 'app/feature/someView.html',
      scope: {
          max: '='
      },
      link: linkFunc,
      controller: SomeController,
      controllerAs: 'vm',
      bindToController: true // because the scope is isolated
  };

  function linkFunc(scope, el, attr, ctrl) {
      console.log('LINK: scope.min = %s *** should be undefined', scope.min);
      console.log('LINK: scope.max = %s *** should be undefined', scope.max);
      console.log('LINK: scope.vm.min = %s', scope.vm.min);
      console.log('LINK: scope.vm.max = %s', scope.vm.max);
  }
  
  var SomeController = function ($scope) {
      // Injecting $scope just for comparison
      var vm = this;

      vm.min = 3;

      console.log('CTRL: $scope.vm.min = %s', $scope.vm.min);
      console.log('CTRL: $scope.vm.max = %s', $scope.vm.max);
      console.log('CTRL: vm.min = %s', vm.min);
      console.log('CTRL: vm.max = %s', vm.max);
  };

  SomeController.$inject = ['$scope'];
  ```

  ```html
  <!-- example.directive.html -->
  <div>hello world</div>
  <div>max={{vm.max}}<input ng-model="vm.max"/></div>
  <div>min={{vm.min}}<input ng-model="vm.min"/></div>
  ```

    Note: You can also name the controller when you inject it into the link function and access directive attributes as properties of the controller.

  ```javascript
  // Alternative to above example
  link: function(scope, el, attr, vm) {
      console.log('LINK: scope.min = %s *** should be undefined', scope.min);
      console.log('LINK: scope.max = %s *** should be undefined', scope.max);
      console.log('LINK: vm.min = %s', vm.min);
      console.log('LINK: vm.max = %s', vm.max);
  }
  ```

  - Use `bindToController = true` when using `controller as` syntax with a directive when you want to bind the outer scope to the directive's controller's scope.

    *Why?*: It makes it easy to bind outer scope to the directive's controller scope.

    Note: `bindToController` was introduced in Angular 1.3.0.

### Filters

  - Avoid using filters for scanning all properties of a complex object graph. Use filters for select properties.

    *Why?*: Filters can easily be abused and negatively effect performance if not used wisely, for example when a filter hits a large and deep object graph.

    ```javascript
    ﻿'use strict';
    define(['app'], function (app) {

      var claimTypeFilter = function () {

          return function (value) {
              var result = "Unknown";
              switch (value) {
                  case -1:
                      result = "View All"; break;
                  case 1:
                      result = "Medical"; break;
                  case 2:
                      result = "Pharmacy"; break;
                  case 0:
                      result = "Dental"; break;
                  case 3:
                      result = "Vision"; break;

              }
              return result;
          };
      };

      app.filter('claimTypeFilter', claimTypeFilter);
      //below register can be used if filter is registered after angular.bootstrap
      //app.register.filter('claimTypeFilter', claimTypeFilter);
    });
    ```

### Constants

  - Create an Angular Constant for vendor libraries' global variables.

    *Why?*: Provides a way to inject vendor libraries that otherwise are globals. This improves code testability by allowing you to more easily know what the dependencies of your components are (avoids leaky abstractions). It also allows you to mock these dependencies, where it makes sense.

  - Use constants for values that do not change and do not come from another service. When constants are used only for a module that may be reused in multiple applications, place constants in a file per module named after the module. Until this is required, keep constants in the main module in a `appConst.js` file.

    *Why?*: A value that may change, even infrequently, should be retrieved from a service so you do not have to change the source code. For example, a url for a data service could be placed in a constants but a better place would be to load it from a web service.

    *Why?*: Constants can be injected into any angular component, including providers.

    *Why?*: When an application is separated into modules that may be reused in other applications, each stand-alone module should be able to operate on its own including any dependent constants.

    ```javascript
    // appConst.js
    ﻿'use strict';
    define(['app'], function (app) {

        /* global 3rd party libraries */
        app.constant('toastr', toastr)
            .constant('moment', moment);

        // application constants
        app.constant('events', {
            ORDER_CREATED: 'event_order_created',
            INVENTORY_DEPLETED: 'event_inventory_depleted'
        });
    });
    ```

## Resolving Routes

  - routeResolver module defined in the application framework will allow us to load controller file asynchronously and register it with the app module before initializing the route.

  - When a controller needs to be loaded asynchronously , RequireJS require feature is used to request the controller script file and load the the browser and it's other dependent components in the `$routeProvider` before the controller logic is executed. If you need to conditionally cancel a route before the controller is activated, use a route resolver.

  - Use a route resolve when you want to decide to cancel the route before ever transitioning to the View.

    *Why?*: A route may need authentication and controller component and it's dependencies before it's initiated. That components may need to be loaded to the browser using AMD module or promise via a custom factory using [$http](https://docs.angularjs.org/api/ng/service/$http). 
    Using a [$routeProvider](https://docs.angularjs.org/api/ngRoute/provider/$routeProvider) allows the promise to resolve before the controller logic executes, so it might take action based on that data from the promise.

    *Why?*: The code executes after the route and in the controller’s init function. The view starts to load right away. Data binding kicks in when the init function data service promise resolves. 


  ```javascript
  /* better */

  // routeResolver.js
  'use strict';
  define([], function () {

      var routeResolver = function () {

          this.$get = function () {
              return this;
          };

          this.route = function () {
              //this will take route specific attributes and convert that into angular route definition to be used in "$routeProvider.when"
              var resolve = function (templateUrl, controllerName, controllerAs, secure, dependencies, requireAuth, rjsMod) {
                  var routeDef = {};
                  routeDef.templateUrl = templateUrl;
                  routeDef.controller = controllerName;
                  routeDef.controllerAs = controllerAs;
                  routeDef.secure = (secure) ? secure : false;
                  routeDef.requireAuth = requireAuth;
                  // this is the route resolve function available in angular, that can be used to pre-load any scripts or data before route controller/view is initialized.
                  // Current application will load the specific controller related to the route using this feature before initializing that controller and view.
                  routeDef.resolve = {
                      load: ['$q', '$rootScope', function ($q, $rootScope) {
                          //rjsMod parameter contains the requirejs module name that needs to be loaded to browser.
                          //we are adding that module into the dependecy array and ask RequireJS require function to load that script file.
                          dependencies.push(rjsMod);
                          return resolveDependencies($q, $rootScope, dependencies);
                      }]
                  };

                  return routeDef;
              },

              //this will load the required scripts for route through RequireJs AMD loading. 
              //as part of this script file containing the controller class/component will be loaded before route definition is given to "$routeProvider.when"
              resolveDependencies = function ($q, $rootScope, dependencies) {
                  var defer = $q.defer();
                  //RequireJS require function loads the module definitions, then resolve the route resolve function to init the controller and view for that route.
                  require(dependencies, function () {
                      defer.resolve();
                      $rootScope.$apply();
                  });

                  return defer.promise;
              };

              //this contains the list of defined routes for this application. this logic would be moved to it's own json file to handle as json data in future.
              var getRoutes = function () {
                  return [
                    {
                        //angular route url
                        route: '/',
                        //controller component name (not the file name, actual component name used for registering with angular module)
                        controllerName: 'MainCtrl',
                        //requirejs component definition abbrevation defined in requirejs configuration "path" array
                        rjsMod: 'mainCtrl',
                        //route template view url, which points to webcenter page
                        template: 'http://va10twpiss010:8081/cs/poc/home.html',
                        //use of controller as feature in angular, refer previous guidelines on controller as specifics.
                        controllerAs: 'vm',
                        //used when https is used for route
                        secure: false,
                        //additional requirejs dependencies that needs to be loaded before route is initialized. above requirejs definition for controller will be added to this array internally.
                        dependencies: [],
                        requireAuth: false
                    },
                    ///////

                    {
                        //use regex expression to identify common urls that can be served with same controller
                        route: '/consumer/:name',
                        controllerName: 'WcsCtrl',
                        rjsMod: 'wcsCtrl',
                        //use tempalte function to handle multiple url's to be used with same controller. (ex: for webcenter specific cms pages that doesnt have any application logic)
                        template: function (urlattr) {
                            return 'http://va10twpiss010:8081/cs/poc/' + urlattr.name + '.html';
                        },
                        controllerAs: 'vm',
                        secure: false,
                        dependencies: [],
                        requireAuth: true
                    }
                  ];
              }

              return {
                  resolve: resolve,
                  getRoutes: getRoutes
              }
          }();

      };

      var servicesApp = angular.module('routeResolverServices', []);

      servicesApp.provider('routeResolver', routeResolver);
  });
  ```

    Note: The example below shows the routeProvider.when usage with above routeResolver module.

  ```javascript
  /* recommended */

  // app.js

  ///////////

  var route = routeResolverProvider.route;
  var availableRoutes = route.getRoutes();

  //routes are defined in /services/routeResolver.js
  for (var i = 0; i < availableRoutes.length; i++) {
      $routeProvider.when(availableRoutes[i].route, route.resolve(availableRoutes[i].template, availableRoutes[i].controllerName, availableRoutes[i].controllerAs, availableRoutes[i].secure, availableRoutes[i].dependencies, availableRoutes[i].requireAuth, availableRoutes[i].rjsMod));
  }
  $routeProvider.otherwise({ redirectTo: '/404' });

  ///////////

  ```

## Angular Dependency Injection

### Manually Inject Dependencies

  - Avoid using the shortcut syntax of declaring dependencies without using a minification-safe approach.

    *Why?*: The parameters to the component (e.g. controller, factory, etc) will be converted to mangled variables. For example, `common` and `dataservice` may become `a` or `b` and not be found by Angular.

    ```javascript
    /* avoid - not minification-safe*/
    var MainController = function ($scope,
                                   $location,
                                   $filter,
                                   $window,
                                   $timeout,
                                   cookieHelper) {


    };

    app.register.controller('MainCtrl', MainController);
    ```

    This code may produce mangled variables when minified (like below) and thus cause runtime errors.

    ```javascript
    /* avoid - not minification-safe*/
    d = function x(q,w,e,r,t,y){}
    a.register.controller('MainCtrl', d);
    ```

  - Use `$inject` to manually identify your dependencies for Angular components.

    *Why?*: This technique mirrors the technique used by [`ng-annotate`](https://github.com/olov/ng-annotate), which I recommend for automating the creation of minification safe dependencies. If `ng-annotate` detects injection has already been made, it will not duplicate it.

    *Why?*: This safeguards your dependencies from being vulnerable to minification issues when parameters may be mangled. For example, `common` and `dataservice` may become `a` or `b` and not be found by Angular.

    *Why?*: Avoid creating in-line dependencies as long lists can be difficult to read in the array. Also it can be confusing that the array is a series of strings while the last item is the component's function.

    ```javascript
    /* recommended */
    'use strict';
    define(['app'], function (app) {

        //use injectParams variable to define the injectable components
        var injectParams = ['$scope',
                            '$location',
                            '$filter',
                            '$window',
                            '$timeout',
                            'cookieHelper'];

        var MainController = function ($scope,
                                       $location,
                                       $filter,
                                       $window,
                                       $timeout,
                                       cookieHelper) {
            init();

            function init() {
            }


        };

        //then use the angular $inject feature to point the above definied injectParams variables
        MainController.$inject = injectParams;

        app.register.controller('MainCtrl', MainController);
    });

    ```

    Note: You can use same pattern like above to inject components to other class(s) like directives/factory/service/filters ...

### Use ngAnnonate to Inject Dependencies

  Note: below guide is not applicable if we use $inject in our code when defining injectable dependencies. described in previous section.

  - Use [ng-annotate](//github.com/olov/ng-annotate) for [Gulp](http://gulpjs.com) or [Grunt](http://gruntjs.com) and comment functions that need automated dependency injection using `/** @ngInject */`

    *Why?*: This safeguards your code from any dependencies that may not be using minification-safe practices.

    *Why?*: [`ng-min`](https://github.com/btford/ngmin) is deprecated

    The following code is not using minification safe dependencies.

    ```javascript
    angular
        .module('app')
        .controller('Avengers', Avengers);

    /* @ngInject */
    function Avengers(storageService, avengerService) {
        var vm = this;
        vm.heroSearch = '';
        vm.storeHero = storeHero;

        function storeHero() {
            var hero = avengerService.find(vm.heroSearch);
            storageService.save(hero.name, hero);
        }
    }

    ```

    When the above code is run through ng-annotate it will produce the following output with the `$inject` annotation and become minification-safe.

    ```javascript
    angular
        .module('app')
        .controller('Avengers', Avengers);

    /* @ngInject */
    function Avengers(storageService, avengerService) {
        var vm = this;
        vm.heroSearch = '';
        vm.storeHero = storeHero;

        function storeHero() {
            var hero = avengerService.find(vm.heroSearch);
            storageService.save(hero.name, hero);
        }
    }

    Avengers.$inject = ['storageService', 'avengerService'];
    ```

    Note: If `ng-annotate` detects injection has already been made (e.g. `@ngInject` was detected), it will not duplicate the `$inject` code.

    > Note: Starting from Angular 1.3 you can use the [`ngApp`](https://docs.angularjs.org/api/ng/directive/ngApp) directive's `ngStrictDi` parameter to detect any potentially missing magnification safe dependencies. When present the injector will be created in "strict-di" mode causing the application to fail to invoke functions which do not use explicit function annotation (these may not be minification safe). Debugging info will be logged to the console to help track down the offending code. I prefer to only use `ng-strict-di` for debugging purposes only.
    `<body ng-app="APP" ng-strict-di>`

#### Gulp Configuration for ngAnnonate

  - Use [gulp-ng-annotate](https://www.npmjs.org/package/gulp-ng-annotate) or [grunt-ng-annotate](https://www.npmjs.org/package/grunt-ng-annotate) in an automated build task. Inject `/* @ngInject */` prior to any function that has dependencies.

    *Why?*: ng-annotate will catch most dependencies, but it sometimes requires hints using the `/* @ngInject */` syntax.

    The following code is an example of a grunt task using ngAnnotate

    ```javascript
    ngAnnotate: {
            dist: {
                files: [{
                    expand: true,
                    cwd: '.tmp/concat/scripts',
                    src: '*.js',
                    dest: '.tmp/concat/scripts'
                }]
            }
        },

    ```

## Exception Handling

### Angular Specific Exceptions

  - Use a [decorator](https://docs.angularjs.org/api/auto/service/$provide#decorator), at config time using the [`$provide`](https://docs.angularjs.org/api/auto/service/$provide) service, on the [`$exceptionHandler`](https://docs.angularjs.org/api/ng/service/$exceptionHandler) service to perform custom actions when exceptions occur.

    *Why?*: Provides a consistent way to handle uncaught Angular exceptions for development-time or run-time.

    Note: Another option is to override the service instead of using a decorator. This is a fine option, but if you want to keep the default behavior and extend it a decorator is recommended.

    ```javascript
    /* recommended */
    app.config(['$provide',function($provide){
        $provide.decorator('$exceptionHandler', extendExceptionHandler);
    }]);


    extendExceptionHandler.$inject = ['$delegate', 'toastr'];

    function extendExceptionHandler($delegate, toastr) {
        return function(exception, cause) {
            $delegate(exception, cause);
            var errorData = {
                exception: exception,
                cause: cause
            };
            /**
             * Could add the error to a service's collection,
             * add errors to $rootScope, log errors to remote web server,
             * or log locally. Or throw hard.This is TBD. will be updated in future.
             * throw exception;
             */
            //toastr.error(exception.msg, errorData);
        };
    }
    ```

### Global Exceptions

  - This will be built into core application framework, and listed in this guide for reference/usage in your contrllers/services/directives.

  - Create a factory that exposes an interface to catch and gracefully handle exceptions.

    *Why?*: Provides a consistent way to catch exceptions that may be thrown in your code (e.g. during XHR calls or promise failures).

    Note: The exception catcher is good for catching and reacting to specific exceptions from calls that you know may throw one. For example, when making an XHR call to retrieve data from a remote web service and you want to catch any exceptions from that service and react uniquely.

    ```javascript
    /* recommended */
    'use strict';

    define(['app'], function (app) {

      var injectParams = ['logger'];

      var exceptionFactory = function (logger) {
        var factory = {
          catcher: catcher
        };

        return factory;

        function catcher(message) {
            return function(reason) {
                logger.error(message, reason);
            };
        }

      };

      exceptionFactory.$inject = injectParams;

      app.factory('exceptionFac', exceptionFactory);

    });

    ```

### Routing Specific Errors

  - This will be built into core application framework, and listed in this guide for reference.

  - Handle and log all routing errors using [`$routeChangeError`](https://docs.angularjs.org/api/ngRoute/service/$route#$routeChangeError).

    *Why?*: Provides a consistent way to handle all routing errors.

    *Why?*: Potentially provides a better user experience if a routing error occurs and you route them to a friendly screen with more details or recovery options.

    ```javascript
    /* recommended */    
    var handlingRouteChangeError = false;

    function handleRoutingErrors() {
        /**
         * Route cancellation:
         * On routing error, go to the dashboard.
         * Provide an exit clause if it tries to do it twice.
         */
        $rootScope.$on('$routeChangeError',
            function(event, current, previous, rejection) {
                if (handlingRouteChangeError) { return; }
                handlingRouteChangeError = true;
                var destination = (current && (current.title ||
                    current.name || current.loadedTemplateUrl)) ||
                    'unknown target';
                var msg = 'Error routing to ' + destination + '. ' +
                    (rejection.msg || '');

                /**
                 * Optionally log using a custom service or $log.
                 * (Don't forget to inject custom service)
                 */
                logger.warning(msg, [current]);

                /**
                 * On routing error, go to another route/state.
                 */
                $location.path('/');

            }
        );
    }
    ```

## Angular $ Wrapper Services

  - Use [`$document`](https://docs.angularjs.org/api/ng/service/$document) and [`$window`](https://docs.angularjs.org/api/ng/service/$window) instead of `document` and `window`.

    *Why?*: These services are wrapped by Angular and more easily testable than using document and window in tests. This helps you avoid having to mock document and window yourself.

  - Use [`$timeout`](https://docs.angularjs.org/api/ng/service/$timeout) and [`$interval`](https://docs.angularjs.org/api/ng/service/$interval) instead of `setTimeout` and `setInterval` .

    *Why?*: These services are wrapped by Angular and more easily testable and handle Angular's digest cycle thus keeping data binding in sync.


## Testing
Unit testing helps maintain clean code.

  - Write a set of tests for every story. Start with an empty test and fill them in as you write the code for the story.

    *Why?*: Writing the test descriptions helps clearly define what your story will do, will not do, and how you can measure success.

    ```javascript
    it('should have Avengers controller', function() {
        // TODO
    });

    it('should find 1 Avenger when filtered by name', function() {
        // TODO
    });

    it('should have 10 Avengers', function() {
        // TODO (mock data?)
    });

    it('should return Avengers via XHR', function() {
        // TODO ($httpBackend?)
    });

    // and so on
    ```

  - Use [Jasmine](http://jasmine.github.io/) for unit testing.

    *Why?*: Jasmine is widely used in the Angular community and stable, well maintained, and provide robust testing features.

  - Use [Karma](http://karma-runner.github.io) as a test runner.

    *Why?*: Karma is easy to configure to run once or automatically when you change your code.

    *Why?*: Karma hooks into your Continuous Integration process easily on its own or through Grunt or Gulp.

    *Why?*: Some IDE's are beginning to integrate with Karma, such as [WebStorm](http://www.jetbrains.com/webstorm/) and [Visual Studio](http://visualstudiogallery.msdn.microsoft.com/02f47876-0e7a-4f6c-93f8-1af5d5189225).

    *Why?*: Karma works well with task automation leaders such as [Grunt](http://www.gruntjs.com) (with [grunt-karma](https://github.com/karma-runner/grunt-karma)) and [Gulp](http://www.gulpjs.com) (with [gulp-karma](https://github.com/lazd/gulp-karma)).

  - Use [PhantomJS](http://phantomjs.org/) to run your tests on a server.

    *Why?*: PhantomJS is a headless browser that helps run your tests without needing a "visual" browser. So you do not have to install Chrome, Safari, IE, or other browsers on your server.

    Note: You should still test on all browsers in your environment, as appropriate for your target audience.

  - Use [Sinon](http://sinonjs.org/) for stubbing and spying.

    *Why?*: Sinon works well with Jasmine and extends the stubbing and spying features they offer.

    *Why?*: Sinon has descriptive messages when tests fail the assertions.

  - Run JSHint on your tests.

    *Why?*: Tests are code. JSHint can help identify code quality issues that may cause the test to work improperly.

  - Relax the rules on your test code to allow for common globals such as `describe` and `expect`. Relax the rules for expressions, as Mocha uses these.

    *Why?*: Your tests are code and require the same attention and code quality rules as all of your production code. However, global variables used by the testing framework, for example, can be relaxed by including this in your test specs.

    ```javascript
    /* jshint -W117, -W030 */
    ```
    Or you can add the following to your JSHint Options file.

    ```javascript
    "jasmine": true,
    "mocha": true,
    ```


### Organizing Test Files

  - Place unit test files (specs) side-by-side with your client code. Place specs that cover server integration or test multiple components in a separate `tests` folder.

    *Why?*: Unit tests have a direct correlation to a specific component and file in source code.

    *Why?*: It is easier to keep them up to date since they are always in sight. When coding whether you do TDD or test during development or test after development, the specs are side-by-side and never out of sight nor mind, and thus more likely to be maintained which also helps maintain code coverage.

    *Why?*: When you update source code it is easier to go update the tests at the same time.

    *Why?*: Placing them side-by-side makes it easy to find them and easy to move them with the source code if you move the source.

    *Why?*: Having the spec nearby makes it easier for the source code reader to learn how the component is supposed to be used and to discover its known limitations.

    *Why?*: Separating specs so they are not in a distributed build is easy with grunt or gulp.

    ```
    /app/customers/controllers/customerCtrl.js
                             /controllers/customeCtrl.spec.js
    ```

### Configure Testing Framework

- Unit test framework uses it's own "app.js" to define the angular module for portal application. So make sure to review if your changes should be added to test\app.test.js (if you make any changes in app\app.js file, not all the changes can be(need to be) tested in unit test for angular.config and angular.run). 

- Karma is used as unit test runner, and the config for karama is located in test\karma.conf.js file. you would rarely make changes to this file, and needs to be reviewed with application lead.

- PhantomJS is cofnigured as test running browser, and since karma doesn't use same file path ass app folder to run the unit tests. we need to use a seperate requirejs configuration file for unit test(s).

- When you create new angular component make sure you also update the requirejs config file for unit test located in test\test.requirejs.config.js. karma/phantomjs will use this requirejs configuration to located the component script files to be loaded for unit test runner.

- Sample "paths" array from test requirejs.cofnig is located below. you should be able to notice that "baseUrl" has different value than what we have in app requirejs config (this is becuase karma uses different base path for the application when running unit tests ...).

```javascript
///////////

baseUrl: 'base/app',
paths: {
    'app': '/base/test/app.test',
    'alDir': 'common/directives/ajaxLoadingDir',
    'tpDir': 'common/directives/tooltipPopoverDir',
    'cdDir': 'claims/directives/claimsDropdownDir',
    'benefitSrvc': 'benefits/services/benefitsSrvc',
    'providerSrvc': 'provider/services/providerSrvc',
    'claimsSrvc': 'claims/services/claimsSrvc',
    'userSrvc': 'register/userSrvc',
    'bFilt': 'benefits/filters/benefitFltr',
    'cFilt': 'claims/filters/claimsFltr',
    'cpDir': 'benefits/directives/coveragePeriodDir',
    'tnDir': 'common/directives/topNavDir',
    'clDir': 'claims/directives/claimsListDir',
    'cHlpr': 'common/services/cookieHlpr',
    'contSrvc': 'common/services/contentSrvc',
    'msgSrvc': 'common/services/messageSrvc',
    'msgDir': 'common/directives/contentMessageDir',
    'mainCtrl': 'home/controllers/mainCtrl',
    'loginCtrl': 'login/controllers/loginCtrl',
    'regCtrl': 'register/registerCtrl',
    'dashCtrl': 'dashboard/controllers/dashboardCtrl',
    'coCtrl': 'claims/controllers/claimsOverviewCtrl',
    'medCtrl': 'benefits/controllers/medicalCtrl',
    'authSrvc': 'login/services/authSrvc',
    'authIntSrvc': 'common/services/httpInterceptor',
    'modHlpr': 'common/services/modalHlpr',
    'wtDir': 'common/directives/warnTimerDir',
    'siHlpr': 'common/services/sessionIdleHlpr',
    'wcsCtrl': 'common/controllers/wcsCtrl'
}

/////////////
```

- When configuring your components in requirejs.config, you will need to use "shim" array more excessively for unit tests. all angular components that you want to test should also be listed in "shim" array.

- Sample "shim" list is given below from test requirejs config. as you can see all the filters, directives, controllers etc ... are listed in shim array as seperate shim item (this is because, karma will use this to load the specific component file to browser). how we use this shim/path array definition for the component in spec file will be elaborated in next point.

```javascript
///////////

shim: {
    'app': {
        exports: 'app'
    },
    'contSrvc': {
        deps: ['app'],
        exports: 'contSrvc'
    },
    'cHlpr': {
        deps: ['app'],
        exports: 'cHlpr'
    },
    'regCtrl': {
        deps: ['app', 'userSrvc', 'authSrvc'],
        exports: 'regCtrl'
    },
    'mainCtrl': {
        deps: ['app', 'cHlpr'],
        exports: 'mainCtrl'
    },
    'dashCtrl': {
        deps: ['app'],
        exports: 'dashCtrl'
    },
    'medCtrl': {
        deps: ['app', 'bFilt', 'cHlpr', 'benefitSrvc'],
        exports: 'medCtrl'
    },
    'bFilt': {
        deps: ['app'],
        exports: 'bFilt'
    },
    'alDir': {
        deps: ['app'],
        exports: 'alDir'
    }
},

/////////////
```

#### How to Write spec File for Unit Testing

- Once you complete the above points to setup your component code with karama/phantomjs unit test framework, you can start coding the unit test spec file.

- First you need to use requirejs module definition pattern to define your spec file, and inject the requirejs angular component definition like below.
  - if you closely look at the below code, you should be able to see that we asking requirejs to make sure to load the "medCtrl" requirejs module before this spec file is executed in browser. since this spec file is used to unit test the MedialController ("medCtrl" requirejs module). so when we add this definition, requirejs loads the all dependecies that are required for "medCtrl" module and also load "medCtrl" component to browser and finally load this spec file to browser.

```javascript
//////

define(['medCtrl'], function () {

//////
```

- Next lets look at how we code the pre-initialization code for unit testing. (refer below sample spec file for MedicalController)
  - as part of jasmine we need to decribe the component that we are going to unit test first.
  - initialize the angular.module(s) that are needed  before start loading angualr component to be unit tested.
  - after angualr.module is loaded and initialized, use jasmine inject function to load angular providers for (controller, rootScope etc ....), jasmine will dependency inject the angualr providers properly.
  - you can use $controller, $injector to access any other angular components that are required for unit testing.
  - create new scope using $rootScope
  - use angular $httpBackend to mock any service calls if needed.
  - if the component that you are going to test is a controller, use $controller to get the angualr controller component that you need to unit test. as part of initializing the controller, pass the arguments for any dependency injected components that the controller use.
  - Use jasmine test stories to create your unit test stories.
  - Refer existing directive/filter/service spec files to understand how to test the those angular components.


```javascript
﻿'use strict';

define(['medCtrl'], function () {
   // describe the angular component to be unit tested.
   describe('Utest MedicalController', function () {
       // define the variables that are needed for unit testing purposes
       var medicalController,
         scope, $httpBackend, coverageDataHandler,
           benefitSrvcUrl = 'http://va10dwviss013:9999/api/claimsservice/getAllClaims';

       //define/init the angular.module that needs to be loaded before angular component can be unit tested
       beforeEach(module('routeResolverServices'));
       beforeEach(module('anthemMemberPortal'));

       //use jamine inject function to dependency inject the angular providers like $controller, $rootScope, $injector etc ...
       beforeEach(inject(function ($controller, $rootScope, $injector, benefitsService, contentModules) {
           //create new scope for controller to be used
           scope = $rootScope.$new();
           //get $httpBackend provider from angular $injector.
           $httpBackend = $injector.get('$httpBackend');
           //use $httpBackend to mock service call
           $httpBackend.when('POST', 'http://va10dlvwam002.wellpoint.com:9087/poc/dashboards/getwelcomemessage?module=medical')
                                  .respond({ userId: 'userX' }, { 'A-Token': 'xxx' });

           //get the angular controller component to be unit tested
           medicalController = $controller('MedicalCtrl', {
               $scope: scope,
               $location: $injector.get('$location'),
               $filter: $injector.get('$filter'),
               $window: $injector.get('$window'),
               $timeout: $injector.get('$timeout'),
               benefitsService: benefitsService
           });
       }));

       afterEach(function () {
           $httpBackend.verifyNoOutstandingExpectation();
           $httpBackend.verifyNoOutstandingRequest();
       });

       it('hidePlanData should be true', function () {
           expect(medicalController.model.hidePlanData).toBe(true);
       });

       it('calculateWidth should be 20', function () {
           expect(medicalController.model.calculateWidth(100.00, 500.00, 'accum')).toBe(20);
       });

       it('calculateWidth should be 15', function () {
           expect(medicalController.model.calculateWidth(50.00, 500.00, 'accum')).toBe(15);
       });

       it('test httpBackend', function () {
           $httpBackend.expectPOST('http://va10dlvwam002.wellpoint.com:9087/poc/dashboards/getwelcomemessage?coverageStart=1/1/2015&coverageEnd=12/31/2015&coverageMember=001').respond({});
           medicalController.model.onChangeCoveragePeriod('1/1/2015', '12/31/2015', '001');
           $httpBackend.flush();
           expect(medicalController.model.benefitLevelData.length).toBeGreaterThan(1);
       });
   });
});
```

## Animations

  - Use subtle [animations with Angular](https://docs.angularjs.org/guide/animations) to transition between states for views and primary visual elements. Include the [ngAnimate module](https://docs.angularjs.org/api/ngAnimate). The 3 keys are subtle, smooth, seamless.

    *Why?*: Subtle animations can improve User Experience when used appropriately.

    *Why?*: Subtle animations can improve perceived performance as views transition.

  - Use short durations for animations. I generally start with 300ms and adjust until appropriate.

    *Why?*: Long animations can have the reverse affect on User Experience and perceived performance by giving the appearance of a slow application.

  - Use [animate.css](http://daneden.github.io/animate.css/) for conventional animations.

    *Why?*: The animations that animate.css provides are fast, smooth, and easy to add to your application.

    *Why?*: Provides consistency in your animations.

    *Why?*: animate.css is widely used and tested.

    Note: See this [great post by Matias Niemelä on Angular animations](http://www.yearofmoo.com/2013/08/remastered-animation-in-angularjs-1-2.html)


## Code Commenting

  - Use [`jsDoc`](http://usejsdoc.org/) syntax to document function names, description, params and returns. Use `@namespace` and `@memberOf` to match your app structure.

    *Why?*: You can generate (and regenerate) documentation from your code, instead of writing it from scratch.

    *Why?*: Provides consistency using a common industry tool.

    ```javascript
    /**
     * Claim Type Filter
     * @namespace Filters
     */
     define(['app'], function (app) {

        var claimTypeFilter = function () {

            return function (value) {
                var result = "Unknown";
                switch (value) {
                    case -1:
                        result = "View All"; break;
                    case 1:
                        result = "Medical"; break;
                    case 2:
                        result = "Pharmacy"; break;
                    case 0:
                        result = "Dental"; break;
                    case 3:
                        result = "Vision"; break;

                }
                return result;
            };
        };

        app.register.filter('claimTypeFilter', claimTypeFilter);
    });
    ```

## Validate Coding Standards and Styles

### Validate Coding Standards

  - Be sure to customize the JS Hint options file and include in source control. See the [JS Hint docs](http://www.jshint.com/docs/) for details on the options.

    *Why?*: Provides a first alert prior to committing any code to source control.

    *Why?*: Provides consistency across your team.

    ```javascript
    {
        "bitwise": true,
        "camelcase": true,
        "curly": true,
        "eqeqeq": true,
        "es3": false,
        "forin": true,
        "freeze": true,
        "immed": true,
        "indent": 4,
        "latedef": "nofunc",
        "newcap": true,
        "noarg": true,
        "noempty": true,
        "nonbsp": true,
        "nonew": true,
        "plusplus": false,
        "quotmark": "single",
        "undef": true,
        "unused": false,
        "strict": false,
        "maxparams": 10,
        "maxdepth": 5,
        "maxstatements": 40,
        "maxcomplexity": 8,
        "maxlen": 120,

        "asi": false,
        "boss": false,
        "debug": false,
        "eqnull": true,
        "esnext": false,
        "evil": false,
        "expr": false,
        "funcscope": false,
        "globalstrict": false,
        "iterator": false,
        "lastsemic": false,
        "laxbreak": false,
        "laxcomma": false,
        "loopfunc": true,
        "maxerr": false,
        "moz": false,
        "multistr": false,
        "notypeof": false,
        "proto": false,
        "scripturl": false,
        "shadow": false,
        "sub": true,
        "supernew": false,
        "validthis": false,
        "noyield": false,

        "browser": true,
        "node": true,

        "globals": {
            "angular": false,
            "$": false
        }
    }
    ```

### Validate Coding Styles

  - Use JSCS for checking your coding styles your JavaScript and be sure to customize the JSCS options file and include in source control. See the [JSCS docs](http://www.jscs.info) for details on the options.

    *Why?*: Provides a first alert prior to committing any code to source control.

    *Why?*: Provides consistency across your team.

    ```javascript
    {
        "excludeFiles": ["node_modules/**", "bower_components/**"],

        "requireCurlyBraces": [
            "if",
            "else",
            "for",
            "while",
            "do",
            "try",
            "catch"
        ],
        "requireOperatorBeforeLineBreak": true,
        "requireCamelCaseOrUpperCaseIdentifiers": true,
        "maximumLineLength": {
          "value": 100,
          "allowComments": true,
          "allowRegex": true
        },
        "validateIndentation": 4,
        "validateQuoteMarks": "'",

        "disallowMultipleLineStrings": true,
        "disallowMixedSpacesAndTabs": true,
        "disallowTrailingWhitespace": true,
        "disallowSpaceAfterPrefixUnaryOperators": true,
        "disallowMultipleVarDecl": null,

        "requireSpaceAfterKeywords": [
          "if",
          "else",
          "for",
          "while",
          "do",
          "switch",
          "return",
          "try",
          "catch"
        ],
        "requireSpaceBeforeBinaryOperators": [
            "=", "+=", "-=", "*=", "/=", "%=", "<<=", ">>=", ">>>=",
            "&=", "|=", "^=", "+=",

            "+", "-", "*", "/", "%", "<<", ">>", ">>>", "&",
            "|", "^", "&&", "||", "===", "==", ">=",
            "<=", "<", ">", "!=", "!=="
        ],
        "requireSpaceAfterBinaryOperators": true,
        "requireSpacesInConditionalExpression": true,
        "requireSpaceBeforeBlockStatements": true,
        "requireLineFeedAtFileEnd": true,
        "disallowSpacesInsideObjectBrackets": "all",
        "disallowSpacesInsideArrayBrackets": "all",
        "disallowSpacesInsideParentheses": true,

        "validateJSDoc": {
            "checkParamNames": true,
            "requireParamTypes": true
        },

        "disallowMultipleLineBreaks": true,

        "disallowCommaBeforeLineBreak": null,
        "disallowDanglingUnderscores": null,
        "disallowEmptyBlocks": null,
        "disallowMultipleLineStrings": null,
        "disallowTrailingComma": null,
        "requireCommaBeforeLineBreak": null,
        "requireDotNotation": null,
        "requireMultipleVarDecl": null,
        "requireParenthesesAroundIIFE": true
    }
    ```



## File Templates and Snippets
Use file templates or snippets to help follow consistent styles and patterns. Here are templates and/or snippets for some of the web development editors and IDEs.

### Sublime Text

  - Angular snippets that follow these styles and guidelines.

    - TBD
    - Place it in your Packages folder
    - Restart Sublime
    - In a JavaScript file type these commands followed by a `TAB`

    ```javascript
    ngcontroller // creates an Angular controller
    ngdirective  // creates an Angular directive
    ngfactory    // creates an Angular factory
    ngmodule     // creates an Angular module
    ngservice    // creates an Angular service
    ngfilter     // creates an Angular filter
    ```

### Visual Studio

  - Angular file templates that follow these styles and guidelines can be found at TBD

    - Download the Visual Studio extension (vsix file)
    - Run the vsix file
    - Restart Visual Studio

### WebStorm

  - Angular snippets and file templates that follow these styles and guidelines. You can import them into your WebStorm settings:

    - Download the TBD
    - Open WebStorm and go to the `File` menu
    - Choose the `Import Settings` menu option
    - Select the file and click `OK`
    - In a JavaScript file type these commands followed by a `TAB`:

    ```javascript
    ng-c // creates an Angular controller
    ng-f // creates an Angular factory
    ng-m // creates an Angular module
    ```

### Atom

  - Angular snippets that follow these styles and guidelines.
    
    - Open Atom, then open the Package Manager (Packages -> Settings View -> Install Packages/Themes)
    - Search for the package TBD
    - Click 'Install' to install the package

  - In a JavaScript file type these commands followed by a `TAB`

    ```javascript
    ngcontroller // creates an Angular controller
    ngdirective // creates an Angular directive
    ngfactory // creates an Angular factory
    ngmodule // creates an Angular module
    ngservice // creates an Angular service
    ngfilter // creates an Angular filter
    ```

## Task Automation
Use [Grunt](http://gruntjs.com) for creating automated tasks.  Grunt leans to configuration over code.

- Add more details about Grunt to this section

## JavaScript Coding Standards

### Types

  - **Primitives**: When you access a primitive type you work directly on its value.

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1;
    var bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **Complex**: When you access a complex type you work on a reference to its value.

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2];
    var bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

### Objects

  - Use the literal syntax for object creation.

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```

  - Don't use [reserved words](http://es5.github.io/#x7.6.1) as keys. It won't work in IE8. [More info](https://github.com/airbnb/javascript/issues/61).

    ```javascript
    // bad
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // good
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - Use readable synonyms in place of reserved words.

    ```javascript
    // bad
    var superman = {
      class: 'alien'
    };

    // bad
    var superman = {
      klass: 'alien'
    };

    // good
    var superman = {
      type: 'alien'
    };
    ```

### Arrays

  - Use the literal syntax for array creation.

    ```javascript
    // bad
    var items = new Array();

    // good
    var items = [];
    ```

  - Use Array#push instead of direct assignment to add items to an array.

    ```javascript
    var someStack = [];


    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  - When you need to copy an array use Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length;
    var itemsCopy = [];
    var i;

    // bad
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    itemsCopy = items.slice();
    ```

  - To convert an array-like object to an array, use Array#slice.

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```    
### Strings

  - Use single quotes `''` for strings.

    ```javascript
    // bad
    var name = "Bob Parr";

    // good
    var name = 'Bob Parr';

    // bad
    var fullName = "Bob " + this.lastName;

    // good
    var fullName = 'Bob ' + this.lastName;
    ```

  - Strings longer than 80 characters should be written across multiple lines using string concatenation.
  - Note: If overused, long strings with concatenation could impact performance. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

    ```javascript
    // bad
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    var errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // good
    var errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  - When programmatically building up a string, use Array#join instead of string concatenation. Mostly for IE: [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items;
    var messages;
    var length;
    var i;

    messages = [{
      state: 'success',
      message: 'This one worked.'
    }, {
      state: 'success',
      message: 'This one worked as well.'
    }, {
      state: 'error',
      message: 'This one did not work.'
    }];

    length = messages.length;

    // bad
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // good
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        items[i] = '<li>' + messages[i].message + '</li>';
      }

      return '<ul>' + items.join('') + '</ul>';
    }
    ```    


### Functions

  - Function expressions:

    ```javascript
    // anonymous function expression
    var anonymous = function() {
      return true;
    };

    // named function expression
    var named = function named() {
      return true;
    };

    // immediately-invoked function expression (IIFE)
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.
  - **Note:** ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    var test;
    if (currentUser) {
      test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - Never name a parameter `arguments`. This will take precedence over the `arguments` object that is given to every function scope.

    ```javascript
    // bad
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // good
    function yup(name, options, args) {
      // ...stuff...
    }
    ```    


### Properties

  - Use dot notation when accessing properties.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // bad
    var isJedi = luke['jedi'];

    // good
    var isJedi = luke.jedi;
    ```

  - Use subscript notation `[]` when accessing properties with a variable.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```    

### Variables

  - Always use `var` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    var superPower = new SuperPower();
    ```

  - Use one `var` declaration per variable.
    It's easier to add new variable declarations this way, and you never have
    to worry about swapping out a `;` for a `,` or introducing punctuation-only
    diffs.

    ```javascript
    // bad
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    // (compare to above, and try to spot the mistake)
    var items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

  - Declare unassigned variables last. This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

    ```javascript
    // bad
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    var i;
    var items = getItems();
    var dragonball;
    var goSportsTeam = true;
    var len;

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball;
    var length;
    var i;
    ```

  - Assign variables at the top of their scope. This helps avoid issues with variable declaration and assignment hoisting related issues.

    ```javascript
    // bad
    function() {
      test();
      console.log('doing stuff..');

      //..other stuff..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // good
    function() {
      var name = getName();

      test();
      console.log('doing stuff..');

      //..other stuff..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // bad
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // good
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();

      return true;
    }
    ```


### Hoisting

  - Variable declarations get hoisted to the top of their scope, but their assignment does not.

    ```javascript
    // we know this wouldn't work (assuming there
    // is no notDefined global variable)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // The interpreter is hoisting the variable
    // declaration to the top of the scope,
    // which means our example could be rewritten as:
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - Anonymous function expressions hoist their variable name, but not the function assignment.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - Named function expressions hoist the variable name, not the function name or the function body.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // the same is true when the function name
    // is the same as the variable name.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - Function declarations hoist their name and the function body.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - For more information refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/).

### Comparison Operators & Equality

  - Use `===` and `!==` over `==` and `!=`.
  - Comparison operators are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:

    + **Objects** evaluate to **true**
    + **Undefined** evaluates to **false**
    + **Null** evaluates to **false**
    + **Booleans** evaluate to **the value of the boolean**
    + **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
    + **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0]) {
      // true
      // An array is an object, objects evaluate to true
    }
    ```

  - Use shortcuts.

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (collection.length > 0) {
      // ...stuff...
    }

    // good
    if (collection.length) {
      // ...stuff...
    }
    ```

  - For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

### Blocks

  - Use braces with all multi-line blocks.

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function() { return false; }

    // good
    function() {
      return false;
    }
    ```

  - If you're using multi-line blocks with `if` and `else`, put `else` on the same line as your
    `if` block's closing brace.

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```    

### Comments

  - Use `/** ... */` for multi-line comments. Include a description, specify types and values for all parameters and return values.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment.

    ```javascript
    // bad
    var active = true;  // is current tab

    // good
    // is current tab
    var active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

  - Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you're pointing out a problem that needs to be revisited, or if you're suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME -- need to figure this out` or `TODO -- need to implement`.

  - Use `// FIXME:` to annotate problems.

    ```javascript
    function Calculator() {

      // FIXME: shouldn't use a global here
      total = 0;

      return this;
    }
    ```

  - Use `// TODO:` to annotate solutions to problems.

    ```javascript
    function Calculator() {

      // TODO: total should be configurable by an options param
      this.total = 0;

      return this;
    }
  ```

### Whitespace

  - Use soft tabs set to 2 spaces.

    ```javascript
    // bad
    function() {
    ∙∙∙∙var name;
    }

    // bad
    function() {
    ∙var name;
    }

    // good
    function() {
    ∙∙var name;
    }
    ```

  - Place 1 space before the leading brace.

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```

  - Place 1 space before the opening parenthesis in control statements (`if`, `while` etc.). Place no space before the argument list in function calls and declarations.

    ```javascript
    // bad
    if(isJedi) {
      fight ();
    }

    // good
    if (isJedi) {
      fight();
    }

    // bad
    function fight () {
      console.log ('Swooosh!');
    }

    // good
    function fight() {
      console.log('Swooosh!');
    }
    ```

  - Set off operators with spaces.

    ```javascript
    // bad
    var x=y+5;

    // good
    var x = y + 5;
    ```

  - End files with a single newline character.

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function(global) {
      // ...stuff...
    })(this);↵
    ```

  - Use indentation when making long method chains. Use a leading dot, which
    emphasizes that the line is a method call, not a new statement.

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // bad
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width',  (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - Leave a blank line after blocks and before the next statement

    ```javascript
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;

    // bad
    var obj = {
      foo: function() {
      },
      bar: function() {
      }
    };
    return obj;

    // good
    var obj = {
      foo: function() {
      },

      bar: function() {
      }
    };

    return obj;
    ```      

### Commas

  - Leading commas: **Nope.**

    ```javascript
    // bad
    var story = [
        once
      , upon
      , aTime
    ];

    // good
    var story = [
      once,
      upon,
      aTime
    ];

    // bad
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // good
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - Additional trailing comma: **Nope.** This can cause problems with IE6/7 and IE9 if it's in quirksmode. Also, in some implementations of ES3 would add length to an array if it had an additional trailing comma. This was clarified in ES5 ([source](http://es5.github.io/#D)):

  > Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

    ```javascript
    // bad
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // good
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```


### Semicolons

  - **Yup.**

    ```javascript
    // bad
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // good
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // good (guards against the function becoming an argument when two files with IIFEs are concatenated)
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    [Read more](http://stackoverflow.com/a/7365214/1712802).

### Type Casting & Coercion

  - Perform type coercion at the beginning of the statement.
  - Strings:

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    var totalScore = this.reviewScore + '';

    // good
    var totalScore = '' + this.reviewScore;

    // bad
    var totalScore = '' + this.reviewScore + ' total score';

    // good
    var totalScore = this.reviewScore + ' total score';
    ```

  - Use `parseInt` for Numbers and always with a radix for type casting.

    ```javascript
    var inputValue = '4';

    // bad
    var val = new Number(inputValue);

    // bad
    var val = +inputValue;

    // bad
    var val = inputValue >> 0;

    // bad
    var val = parseInt(inputValue);

    // good
    var val = Number(inputValue);

    // good
    var val = parseInt(inputValue, 10);
    ```

  - If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

    ```javascript
    // good
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    var val = inputValue >> 0;
    ```

  - **Note:** Be careful when using bitshift operations. Numbers are represented as [64-bit values](http://es5.github.io/#x4.3.19), but Bitshift operations always return a 32-bit integer ([source](http://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32 bits. [Discussion](https://github.com/airbnb/javascript/issues/109). Largest signed 32-bit Int is 2,147,483,647:

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - Booleans:

    ```javascript
    var age = 0;

    // bad
    var hasAge = new Boolean(age);

    // good
    var hasAge = Boolean(age);

    // good
    var hasAge = !!age;
    ```


### Naming Conventions

  - Avoid single letter names. Be descriptive with your naming.

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

  - Use camelCase when naming objects, functions, and instances.

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {}
    var u = new user({
      name: 'Bob Parr'
    });

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - Use PascalCase when naming constructors or classes.

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // good
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - Use a leading underscore `_` when naming private properties.

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

  - When saving a reference to `this` use `_this`.

    ```javascript
    // bad
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }

    // bad
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // good
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }
    ```

  - Name your functions. This is helpful for stack traces.

    ```javascript
    // bad
    var log = function(msg) {
      console.log(msg);
    };

    // good
    var log = function log(msg) {
      console.log(msg);
    };
    ```

  - **Note:** IE8 and below exhibit some quirks with named function expressions.  See [http://kangax.github.io/nfe/](http://kangax.github.io/nfe/) for more info.

  - If your file exports a single class, your filename should be exactly the name of the class.
    ```javascript
    // file contents
    class CheckBox {
      // ...
    }
    module.exports = CheckBox;

    // in some other file
    // bad
    var CheckBox = require('./checkBox');

    // bad
    var CheckBox = require('./check_box');

    // good
    var CheckBox = require('./CheckBox');
    ```

### Accessors

  - Accessor functions for properties are not required.
  - If you do make accessor functions use getVal() and setVal('hello').

    ```javascript
    // bad
    dragon.age();

    // good
    dragon.getAge();

    // bad
    dragon.age(25);

    // good
    dragon.setAge(25);
    ```

  - If the property is a boolean, use isVal() or hasVal().

    ```javascript
    // bad
    if (!dragon.age()) {
      return false;
    }

    // good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - It's okay to create get() and set() functions, but be consistent.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function(key) {
      return this[key];
    };
    ```


### Constructors

  - Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // bad
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // good
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - Methods can return `this` to help with method chaining.

    ```javascript
    // bad
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

### Events

  - When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:

    ```js
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    prefer:

    ```js
    // good
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

### Single Literal Object (Modules)

  - Always declare `'use strict';` at the top of the module.
  - Define module by setting the variable with module name with PascalCase.
  - Define public functions/propertiesas variables. internal functions as normal function.
  - Finally return the public variables as a object with properties.

    ```javascript
    //jquery noconflict mode
    'use strict';
    var $j = jQuery;
    var StateSelector = (function () {

        var init = (function () {
           internalFunction();
        });

        var publicFunction = (function(){

        });

        function internalFunction(){}


        return {
            Init: init,
            PublicFunction: publicFunction
        };

    })();


    //usage
    StateSelector.Init();
    StateSelector.PublicFunction();
    ```


### jQuery

  - Prefix jQuery object variables with a `$`.

    ```javascript
    // bad
    var sidebar = $('.sidebar');

    // good
    var $sidebar = $('.sidebar');
    ```

  - Cache jQuery lookups.

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // good
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - For DOM queries use Cascading `$('.sidebar ul')` or parent > child `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - Use `find` with scoped jQuery object queries.

    ```javascript
    // bad
    $('ul', '.sidebar').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```


### ECMAScript 5 Compatibility

  - Refer to [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.com/es5-compat-table/).      

### Performance

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)

## Modularity

Note: May not be applicable for consumer portal application.

### Many Small, Self Contained Modules

  - Create small modules that encapsulate one responsibility.

    *Why?*: Modular applications make it easy to plug and go as they allow the development teams to build vertical slices of the applications and roll out incrementally. This means we can plug in new features as we develop them.

### Create an App Module

  - Create an application root module whose role is pull together all of the modules and features of your application. Name this for your application.

    *Why?*: Angular encourages modularity and separation patterns. Creating an application root module whose role is to tie your other modules together provides a very straightforward way to add or remove modules from your application.

### Keep the App Module Thin

  - Only put logic for pulling together the app in the application module. Leave features in their own modules.

    *Why?*: Adding additional roles to the application root to get remote data, display views, or other logic not related to pulling the app together muddies the app module and make both sets of features harder to reuse or turn off.

    *Why?*: The app module becomes a manifest that describes which modules help define the application.

### Feature Areas are Modules

  - Create modules that represent feature areas, such as layout, reusable and shared services, dashboards, and app specific features (e.g. customers, admin, sales).

    *Why?*: Self contained modules can be added to the application with little or no friction.

    *Why?*: Sprints or iterations can focus on feature areas and turn them on at the end of the sprint or iteration.

    *Why?*: Separating feature areas into modules makes it easier to test the modules in isolation and reuse code.

### Reusable Blocks are Modules

  - Create modules that represent reusable application blocks for common services such as exception handling, logging, diagnostics, security, and local data stashing.

    *Why?*: These types of features are needed in many applications, so by keeping them separated in their own modules they can be application generic and be reused across applications.

### Module Dependencies

  - The application root module depends on the app specific feature modules and any shared or reusable modules.

    *Why?*: The main app module contains a quickly identifiable manifest of the application's features.

    *Why?*: Each feature area contains a manifest of what it depends on, so it can be pulled in as a dependency in other applications and still work.

    *Why?*: Intra-App features such as shared data services become easy to locate and share from within `app.core` (choose your favorite name for this module).

    > In a small app, you can also consider putting all the shared dependencies in the app module where the feature modules have no direct dependencies. This makes it easier to maintain the smaller application, but makes it harder to reuse modules outside of this application.

## Angular docs
For anything else, API reference, check the [Angular documentation](//docs.angularjs.org/api).

## References

[AngularJS Style Guides by John Papa](https://github.com/johnpapa/angular-styleguide)

[Community Driven Best Practices for AngularJS](https://github.com/mgechev/angularjs-style-guide)

[Javascript Style Guide by AirBnb](https://github.com/airbnb/javascript)
