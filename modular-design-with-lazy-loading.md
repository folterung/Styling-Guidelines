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
  'noext!./refrigerator.service.js'
], function(angular, RefrigeratorService) {
  var RefrigeratorModule = angular.module('refrigerator.module', []);

  RefrigeratorModule.factory('RefrigeratorService', RefrigeratorService);

  return RefrigeratorModule;
});
```