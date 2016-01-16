##Folder Structure Recommendations

My most recent project involves the following stack:

- AngularJS
- RequireJS
- ocLazyLoad

**NOTE: We decided to utilize RequireJS as our primary file loader and used ocLazyLoad to register new modules with AngularJS for "on-the-fly" module registration with AngularJS**

Alright now that we have established some of the main components of our client application lets talk folder structure.

We originally started with a very simplistic folder structure that looked much like the following:

- App
  - Feature
    - src
      - partials
      - styles
      - feature-controllers.js
      - feature-module.js
      - feature-services.js
      - feature-filters.js
      
    - test
      - feature-controllers.spec.js
      - feature-module.spec.js
      - feature-services.js
      - feature-filters.js
    
We started with this layout because the project was still in its' infancy stages and this kept things less complicated for as long as possible.
As time progressed and the complexity grew we found ourselves finding it difficult to 