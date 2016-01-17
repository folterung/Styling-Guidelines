##Modular Design (lazy loading)

The information below is based on the following stack:

- AngularJS
- RequireJS
- ocLazyLoad

Ok so this topic can be quite confusing but I will try to explain it to the best of my ability.
Given that we have an application which has a component called "Refrigerator" and the application is structured as follows:

- index.html
- node_modules
- bower_components
  - Refrigerator
    - Components
      - thermostat.directive.js
      - components.module.js
    - Freezer
      - partials
      - styling
      - freezer.controller.js
      - freezer.service.js
      - freezer.module.js
    - Fridge
      - fridge.controller.js
      - fridge.service.js
      - fridge.module.js
    - refrigerator.service.js
    - refrigerator.module.js
- src
  - app.js
  
If you just finished reading the "Folder Structure" guide then this might look slightly different.
First off, I have provided a more realistic folder structure with our feature in the bower_components folder.
Our feature is in the bower_components folder because in this project the Refrigerator component is being maintained in a different github repo and is pulled in by our app.

Lets started doing some code examples for the Refrigerator component!

**NOTE: We are not going to talk about app.js or configuration to keep this quicker.**

Starting with "refrigerator.module.js" file:

What does it look like?

```javascript
define([
  'angular',
  'noext!./refrigerator.service.js',
  'noext!./Components/components.module.js',
  'noext!./Freezer/freezer.module.js',
  'noext!./Fridge/fridge.module.js'
], function(angular, RefrigeratorService) {
  var RefrigeratorModule = angular.module('refrigerator.module', ['components.module', 'freezer.module', 'fridge.module']);

  RefrigeratorModule.factory('RefrigeratorService', RefrigeratorService);

  return RefrigeratorModule;
});
```

If you are familiar with RequireJS then you should be familiar with AMD module definition.
If not then you can read up about it [here.](http://requirejs.org)

One other thing that you will notice is the "noext!" before our file paths.
This is intentional as we are using the RequireJS plugin noext.

**NOTE: noext is a plugin for RequireJS which stops RequireJS from putting .js as the file extension automatically (this comes in handy when working with ocLazyLoad)**

Aside from that this file looks like some pretty standard AngularJS code.

So what does the code look like for "refrigerator.service.js?

```javascript
define([
  'angular'
], function(angular) {
  RefrigeratorService.$inject = ['$http'];
  
  function RefrigeratorService($http) {
    return {
      getRefrigeratorSpecs: getRefrigeratorSpecs
    };
    
    function getRefrigeratorSpecs() {
      return $http.get('/refrigerator/spec/5');
    }
  }
  
  return RefrigeratorService;
});
```

The only true reference to AngularJS in the file is the $inject for the function which uses AngularJS's dependency injection.
Aside from that we just return a constructor that is used in our "refrigerator.module.js" file.