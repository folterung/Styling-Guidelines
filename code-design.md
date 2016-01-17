##Code Design Guide

**NOTE: Pertains to the below stack**

- AngularJS
- RequireJS
- ocLazyLoad 

I am going to start with some code snippets and then we will talk about them.

###Controller Example
```javascript
define([
  'angular'
], function(angular) {
  ExampleController.$inject = [];

  function ExampleController() {
    var example = this; //Used to reference the controller in the view when using controllerAs syntax
    
    example.giveExample = giveExample; //Bind our function to the view reference of our function
    
    _setTitle('Example Title Loading...');
    _setTitle('Example Title');
    
    //Function that will be used in the view (exampleToGive will be passed in when called from the view)
    function giveExample(exampleToGive) {
      window.alert(exampleToGive);
    }
    
    //We use an "_" to denote that this function is private (all private functions are grouped together)
    function _setTitle(newTitle) {
      example.title = newTitle;
    }
  }
  
  return ExampleController;
});
```

In this example we can see some interesting design patterns that may seem unnecessary at first.

  - We use an "_" to denote that a function is private.

    - This allows other developers that are using your code to identify what is exposed to the view very quickly
      - For performance reasons we should try to keep things exposed to the view to a minimum (only what is necessary)

  - We define anything that we are exposing to the view near the top of the file.

    - This makes it easy to see everything exposed to the view almost instantly
  
  - All functions are defined at the bottom of the controller function and grouped by public/private

    - This makes code that is extreme complex easy to navigate, maintain and continue to scale
  
  - All variables are defined at the top of the function (even if they are not immediately initialized)

    - This makes it easy to locate where variables are defined

###Angular Specific Guidelines
  - File names should be all lowercase
  - Module names should be lowercase and separated with periods (e.g. example.module)
    - File names for modules should contain "module" (e.g. example.module.js)
  - Controller names should be capitalized and have the suffix "Controller" (e.g. ExampleController)
    - File names for controllers should contain "controller" (e.g. example.controller.js)
  - Service names should be capitalized and have the suffix "Service" (e.g. ExampleService)
    - File names for services should contain "service" (e.g. example.service.js)
    - Services are often times used frequently in which case it may be beneficial to exclude the "Service" suffix
  - Filter names should be capitalized (e.g. StripCapitalLetters)
    - File names for filters should contain "filter" (e.g. strip.capital.letters.filter.js)
    - Angular automatically addes the suffix "Filter" which is why we leave it off
  - Providers are very similar to Services and follow the same pattern please refer to the AngularJS documentation for more information on Providers