mini-Routing
============

##Objectives
The purpose of this Mini Project is to get you used to structuring your Angular app and routing. These are two of the trickiest things to wrap your head around so if something doesn't make sense as you're going through, let us know and we'll come over to help you. 

###Step 1: Angular Skeleton 
* Fork this repo, then clone your fork.
* Create the basics of your Angular application. Your file structure should look like this
```
  mini-routing
    index.html
    style.css
    js
      app.js
```
Remember to include ng-app in your application and call your module 'miniRouting'. Also, remember to include the Angular CDN as a script in your HTML, along with app.js. Go ahead and create your 'miniRouting' module in your app.js file. Once you're done doing that, add these styles to your style.css page (don't forget to link it to your index).
```css
html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
}

.menu {
  width: 200px;
  float: left;
  height: 100%;
  background: #e2e2e2;
}

.view-container {
  text-align: center;
}

.view-container h1{
  margin-top: 0;
} 
```

###Step 2: Add Routing Skeleton
* Right now, you should have a very basic Angular application that has nothing more than an app.js (which created your 'miniRouting' module), an index.html page, and some styles. Check your console to make sure there are no errors. If there are, debug.
* Now we're going to prep our HTML in order to start using UI Router. 
* Before we use the UI Router module to handle our routing, there are a few steps we need to take. First, we need to include UI Router as a script, after the Angular library script, in our HTML page. You can find the CDN by googling 'cdn ui router'.
* Once you've included UI Router as a script, we need to inject UI Router into our app as a dependency. Remember how we talked about how our app.js is the hub of our application and it's the only place we use ```angular.module('appName', [])``` with the empty array? The reason that empty array exists is because it's where we inject dependencies into our application. Head over to app.js and add 'ui.router' as a dependency.
* When you're done it should look something like this
```javascript
angular.module('miniRouting', ['ui.router']);
```

###Step 3: Revamp Folder Structure
* As we discussed in the lesson, Angular can dynamically change the template or controller based on what the URL is. For example, if we're at '/users' we can tell Angular to use the 'userCtrl' controller as well as the 'userTemplate' html sheet (or view). 
* As you can imagine, your app starts to get really large as you have different routes. The Angular community has found that the best way to organize your application is by feature. For example, in our app we're going to have a home page, a products page, and a settings page. Go ahead and create those three folders and the files listed below so that your file structure looks like this:
```
  mini-routing
    index.html
    style.css
    js
      app.js
      home
        homeCtrl.js
        homeTmpl.html
      products
        productsCtrl.js
        productsService.js
        productTmpl.html
      settings
        settingsCtrl.js
        settingsTmpl.html
```
* Note that each feature has it's own controller and template (products also has it's own service). Once you're done making the folders and files above, be sure to include all your JAVASCRIPT files in your index.html page as scripts. (Note that html files do not need to be injected. We will inject them as templates later on.)
* Head over to productService.js and add this to the file:
```javascript
angular.module('miniRouting').service('productService', function(){
  this.shoeData = [
    {
      type: 'Nike',
      color: 'Red',
      size: 12
    },
    {
      type: 'Reebok',
      color: 'Blue',
      size: 9
    },
    {
      type: 'Adidas',
      color: 'Yellow',
      size: 6
    },
    {
      type: 'Puma',
      color: 'Green',
      size: 7
    }
  ];

  this.sockData = [
    {
      type: 'Stance',
      color: 'Red',
      size: 'S'
    },
    {
      type: 'Nike',
      color: 'White',
      size: 'M'
    },
    {
      type: 'Reebok',
      color: 'Green',
      size: 'L'
    },
  ];
});
```
* Note: it's just filler data that we're going to use later.

###Step 4: Revamp index.html
* What's nice about routing is that we can have certain parts of the page be static (it never changes), while other parts of the page are dynamic (changes) based on the URL. What we're going to do is have a side menu that will always stay the same no matter what page the user is on. Then, we'll use ```<div ui-view></div>``` which will be where our router kicks in. 
* Head over to your index.html page and inside the body above your script tags add this template
```html
    <div class="menu">
        <ul>
            <li><a ui-sref="home">Home</a></li>
            <li>
                Products
                <ul>
                    <li><a ui-sref="products({id: 'shoes'})">Shoes</a></li>
                    <li><a ui-sref="products({id: 'socks'})">Socks</a></li>
                </ul>
            </li>
            <li><a ui-sref="settings"> Settings </a></li>
        </ul>
    </div>

    <div class="view-container">
        <div ui-view></div>
    </div>
```

Notice that we have a side menu and then we have our `<div ui-view></div>` that will change depending on our router (which we will specify in our next step).

###Step 5: Routing
* Now that our ````<div ui-view></div>```` is set up, let's head over to app.js and actually prepare our router.
* You have the code below but I want you to really try to not look at it until you've completed all of these next steps. I promise it's really not too tricky, just try your best and ask for help if you get stuck.

* 1) add a config property onto your module that takes in an anonymous function as its only argument.
      ```angular.module('mini-routing', ['ui.router']).config(function() {});```
* 2) inject ```$stateProvider``` and ```$urlRouterProvider``` into that anonymous function you just built.
* 3) Now we're going to set up our routes. Here is the criteria.
    - use the ```state``` method of ```$stateProvider``` to create a state called ```home``` that uses ```homeTmpl.html``` as the templateUrl, ```homeCtrl``` as the controller and ('/') as the url.
    - chain another invocation of ```state``` to create a state called ```settings``` that uses ```settingsTmpl.html``` as the templateUrl, ```settingsCtrl``` as the controller and ('/settings') as the url.
    - chain another invocation of ```state``` to create another state called ```products``` that uses ```productTmpl.html``` as the templateUrl, ```productsCtrl``` as the controller and ('/products/:id') as the url. Notice that 'products' has a ```/:id``` at the end of it. This is because we're going to tell our app which product the user is looking at based on which link they clicked. For example, if the user clicks on `<li><a ui-sref="products({id: 'shoes'})">Shoes</a>` then in our controller ```$stateParams.id``` (id correlating with the /:id from earlier) is going to be 'shoes'. This is a little bit tricky, ask for help if you need it.
    - use the ```otherwise``` method of ```$urlRouterProvider``` and pass it the default url ('/'), to redirect all other routes to the default.
        
* Here's what app.js should look like when you're done.

```javascript
angular.module('miniRouting', ['ui.router']).config(function ($stateProvider, $urlRouterProvider) {

    $stateProvider
        .state('home', {
            url: '/',
            templateUrl: 'js/home/homeTmpl.html',
            controller: 'homeCtrl'
        })
        .state('settings', {
            url: '/settings',
            templateUrl: 'js/settings/settingsTmpl.html',
            controller: 'settingsCtrl'
        })
        .state('products', {
            url: '/products/:id',
            templateUrl: 'js/products/productTmpl.html',
            controller: 'productsCtrl'
        });

    $urlRouterProvider
        .otherwise('/');
});
```
###Step 6: Adding to Template
* In order for us to know if our routes are working, we need to edit all of our templates to show some sort of confirmation that we're on a certain page. Make the following changes:
* settingsTmpl.html should look like this
```html
<h1> Settings Page </h1>
<p> This is where the user would change their settings </p>
```
* homeTmpl.html should look like this
```html
<h1> Amazonia </h1>
<h3> Welcome to Amazonia </h3>
```
* productTmpl.html should look like this
```html
<h1> Product Page </h1>
<div> We're going to use this div to loop over our data later on</div>
```
* Test that everything is working by clicking on a few of the links to see if the templates change based on which link you clicked on. If it's not working, first check your console to see if there are any errors. Try to debug, if you debug for 5 minutes and are still stuck, ask for help.
* 

###Step 7: Fixing Product Pages
* The last thing we have to do is show certain product data depending on which page the user is on. For example, if the user is on the shoes page, we want to show them the shoes data. If they're on the socks page, we want to show them the socks data. Remember that in our index.html page our menu looks like this: 
```html
    <div class="menu">
        <ul>
            <li><a ui-sref="home">Home</a></li>
            <li>
                Products
                <ul>
                    <li><a ui-sref="products({id: 'shoes'})">Shoes</a></li>
                    <li><a ui-sref="products({id: 'socks'})">Socks</a></li>
                </ul>
            </li>
            <li><a ui-sref="settings"> Settings </a></li>
        </ul>
    </div>
```
* So we know that in our controller, $stateParams.id (because of :/id in our router) will be either 'socks' or 'shoes' depending on which page the user is in. With this knowledge, we can add a simple 'if' statement to check which product page the user is on.
* In your products controller, inject ```$stateParams``` and ```productService``` into your controller. 
* Now write an if statement, if ```$stateParams.id``` is equal to 'shoes', then ```$scope.productData``` should be set to ```productService.shoeData```. If ```$stateParams.id``` is equal to 'socks', then ```$scope.productData``` should be set to ```productService.sockData```.
* Now we know that we have data on the scope equal to certain product data, depending on which product the user is looking at.
* Your productCtrl.js should now look like this: (Note: Please don't just copy and paste. Try to really understand what's going on.)
```javascript
angular.module('miniRouting').controller('productsCtrl', function ($scope, $stateParams, productService) {
    if ($stateParams.id === 'shoes') {
        $scope.productData = productService.shoeData;
    }
    else if ($stateParams.id === 'socks') {
        $scope.productData = productService.sockData;
    }
});
```
* Now that our data is on the $scope of our productCtrl, head over to your productTmpl.html page and loop over $scope.productData and show the type, color, and size of the product.
* productTmpl.html should now look like this
```html
<h1> Product Page </h1>
<div ng-repeat="item in productData">
    Type: {{ item.type }} </br>
    Color: {{ item.color }} </br>
    Size: {{ item.size }} </br>
</div>
```


## Copyright

© DevMountain LLC, 2016. Unauthorized use and/or duplication of this material without express and written permission from DevMountain, LLC is strictly prohibited. Excerpts and links may be used, provided that full and clear credit is given to DevMountain with appropriate and specific direction to the original content.
