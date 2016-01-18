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
    
We started with this layout because the project was still in it's infancy stages and this kept things less complicated for as long as possible.
As time progressed and the complexity grew we found ourselves finding it difficult to scale and maintain the code so we came up with a plan to restructure the code to achieve this, below shows the result of our changes:

**Note: For the example below we will be using "Menu" as our feature**

- App
  - Menu
    - src
      - partials
      - styles
      - MenuHeader
        - partials
        - styles
        - menu.header.controller.js
        - menu.header.service.js
        - menu.header.module.js
      - MenuPanel
        - partials
        - styles
        - menu.panel.controller.js
        - menu.panel.service.js
        - menu.panel.module.js
      menu.service.js
      menu.module.js
      
    - test
      - MenuHeader
        - menu.header.controller.spec.js
        - menu.header.service.spec.js
        - menu.header.module.js
      - MenuPanel
        - menu.panel.controller.js
        - menu.panel.service.js
        - menu.panel.module.js
      menu.service.spec.js
      menu.module.spec.js
      
Now let us discuss the changes.
First thing that you may have noticed that there are more files (I promise there is a good reason).

1. Anyone can now quickly identify where the MenuHeader controller code is located
2. Code remains more compact which makes it easier to refactor and test
3. By separating the different sub features of the menu we are able to easily re-use those sub features or the whole feature in another project
4. Easy to identify styling for each feature/sub feature
5. All html needed by each feature/sub feature is kept with it

**NOTE: In the application that we created we kept the styling at the main feature level instead of separating it.
  If your application is large enough you should separate it because css/scss/less files can quickly become un-managable.**
