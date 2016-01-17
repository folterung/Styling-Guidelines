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
      - styles
      - freezer.controller.js
      - freezer.service.js
      - freezer.module.js
    - Fridge
      - partials
      - styles
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

To provide a complete reference for the Refrigerator I am going to provide the code snippets for the rest of the files without much explanation because it is almost the same thing.

###components.module.js
```javascript
define([
  'angular',
  'noext!./thermostat.directive.js'
], function(angular, ThermostateDirective) {
  var ComponentsModule = angular.module('refrigerator.components.module', []);
  
  ComponentsModule.directive('thermostat', ThermostatDirective);
  
  return ComponentsModule;
});
```


###thermostat.directive.js
```javascript
define([

], function() {
  ThermostatDirective.$inject = [];
  
  function ThermostatDirective() {
    return {
      restrict: 'A',
      link: link
    };
    
    function link(scope, element, attrs) {
      //scope is reference to the current scope unless an isolate scope is defined
      //element is a reference to the jQlite element of the DOM node that this directive is defined on
      //attrs is a reference to the attributes of the DOM element that this directive is defined on
    }
  }
  
  return ThermostatDirective;
});
```

###freezer.module.js
```javascript
define([
  'angular',
  'noext!./freezer.service.js',
  'noext!./freezer.controller.js'
], function(angular, FreezerService, FreezerController) {
  var FreezerModule = angular.module('refrigerator.freezer.module', []);
  
  FreezerModule.factory('FreezerService', FreezerService);
  FreezerModule.controller('FreezerController', FreezerController);
  
  return FreezerModule;
});
```

###freezer.service.js
```javascript
define([
  'angular'
], function(angular) {
  FreezerService.$inject = [];
  
  function FreezerService() {
    var temperature = 0;
    var isDoorOpen = false;
    
    var FreezerService = {
      setTemperature: setTemperature,
      getTemperature: getTemperature,
      openDoor: openDoor,
      closeDoor: closeDoor,
      getDoorStatus: getDoorStatus
    };
    
    return FreezerService;
    
    function setTemperature(newTemperature) {
      temperature = newTemperature;
    }
    
    function getTemperature() {
      return temperature;
    }
    
    function openDoor() {
      if(isDoorOpen) { return; }
      isDoorOpen = true;
    }
    
    function closeDoor() {
      if(!isDoorOpen) { return; }
      isDoorOpen = false;
    }
    
    function getDoorStatus() {
      return isDoorOpen;
    }
  }
  
  return FreezerService;
});
```

###freezer.controller.js
```javascript
define([
  'angular'
], function(angular) {
  FreezerController.$inject = ['FreezerService'];

  function FreezerController(FreezerService) {
    var freezer = this; //Reference to controller used in our view
    
    _updateTemperature(); //freezer.temperature === 0
    _updateDoorStatus(); //freezer.isDoorOpen === false
    
    _updateTemperature(40); //freezer.temperature === 40
    
    FreezerService.openDoor();
    
    _updateDoorStatus(); //freezer.isDoorOpen === true;
    
    function _updateTemperature(newTemperature) {
      if(newTemperature) {
        FreezerService.setTemperature(newTemperature);
      }
      
      freezer.temperature = FreezerService.getTemperature();
    }
    
    function _updateDoorStatus() {
      freezer.isDoorOpen = FreezerService.getDoorStatus();
    }
  }
  
  return FreezerController;
});
```

###fridge.module.js
```javascript
define([
  'angular',
  'noext!./fridge.service.js',
  'noext!./fridge.controller.js'
], function(angular, FridgeService, FridgeController) {
  var FridgeModule = angular.module('refrigerator.fridge.module', []);
  
  FridgeModule.factory('FridgeService', FridgeService);
  FridgeModule.controller('FridgeController', FridgeController);
  
  return FridgeModule;
});
```

###fridge.service.js
```javascript
define([
  'angular'
], function(angular) {
  FridgeService.$inject = [];
  
  function FridgeService() {
    var temperature = 0;
    var isDoorOpen = false;
    
    var FridgeService = {
      setTemperature: setTemperature,
      getTemperature: getTemperature,
      toggleDoor: toggleDoor,
      getDoorStatus: getDoorStatus
    };
    
    return FridgeService;
    
    function setTemperature(newTemperature) {
      temperature = newTemperature;
    }
    
    function getTemperature() {
      return temperature;
    }
    
    function toggleDoor() {
      isDoorOpen = !isDoorOpen;
    }
    
    function getDoorStatus() {
      return isDoorOpen;
    }
  }
  
  return FridgeService;
});
```

###fridge.controller.js
```javascript
define([
  'angular'
], function(angular) {
  FridgeController.$inject = ['FridgeService'];

  function FridgeController(FridgeService) {
    var fridge = this; //Reference to controller used in view
    
    _updateTemperature(); //fridge.temperature === 0
    _updateDoorStatus(); //fridge.isDoorOpen === false
    
    _updateTemperate(60); //fridge.temperature === 60
    
    FridgeService.toggleDoor();
    
    _updateDoorStatus(); //fridge.isDoorOpen === true
    
    function _updateTemperature(newTemperature) {
      if(newTemperature) {
        FridgeService.setTemperature(newTemperature);
      }
      
      fridge.temperature = FridgeService.getTemperature();
    }
    
    function _updateDoorStatus() {
      fridge.isDoorOpen = FridgeService.getDoorStatus();
    }
  }
  
  return FridgeController;
});
```