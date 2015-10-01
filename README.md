# ANGULAR STATE HANDLER

Control view with routes and providing extra functionalities for easy route management.

## Initial Route Configuration

    $stateHandleProvider
      .when('/', {
        templateUrl: 'views/main.html',
        controller: 'MainCtrl',
        controllerAs: 'main'
      })
      .when('/about', {
        templateUrl: 'views/about.html',
        controller: 'AboutCtrl',
        controllerAs: 'about',
        authentication: true
      })
      .when('/c/:prms',{
        resolve:{
          message:function(){
          }
        },
        authentication: true
      })
      .otherwise({
        redirectTo: '/'
      });
      
**Looks similar to the normal route configuration nothing too different.**

## Assigning Routes Late

Now you know, to set above configuration using pure angular code can only be done within "app.config()" . **Angular State Handler** allows to specify a route whenever you wish . For setting a route late you can use "$stateHandle" factory availabe . Below is how you can specify late routes using "$stateHandle":


    $stateHandle
      .when('/contact', {
        templateUrl: 'views/main.html',
        controller: 'AboutCtrl',
        controllerAs: 'main'
      });
      
**Looks nothing different from the configuration code just the difference is that we used "$stateHandleProvider" while configuration and "$stateHandle" for assigning late routes.**

## View Handling

**Javascript Observer Pattern** is used to handle view based on route changes(*To better understand about observer pattern you can visit http://bumbu.me/javascript-observer-publish-subscribe-pattern/*). Whereever params of a specific route is required you can publish for that route which will respond with parameters .Below is the related code :

    angular.module('frameworkApp')
      .controller('MainCtrl', function ($scope, $stateHandle) {
      
        // Here subscription is done to "/c/:prms"
        $stateHandle.subscribe('main',"/c/:prms").response(function(params){
          $scope.val.prm = params.prms;
        });
    });
    
**.subscribe(** *subscriber_name* **,** *path_or_expression* **)** **:**
 It takes two parameters:

1st parameter -> Name of the subscriber used to uniquely identify a subscriber .Can be any string but it would be recommended to use name similar to the controller or directive or factory block in which its contained .

2nd parameter -> path/expression to which to subscribe.

**.response(** *callback_to_execute_on_publish* **)** **:**
 The callback will be executed when the path is set in the browser to which subscription was done .
    
> Note:- State change of an existing view you can see if you do not specify a "template" or "templateUrl" property to a route and you have subscribed for that route for which in the response function you are changing the view by changing $scope properties or by any other way (*See the third "when" block in the route configuration where no "template" or "templateUrl" property is specified*).You can even see in that route, "controller" property is also not specified .For those cases the controller before route stays till a route change happens in which any other controller is specified .

----------
----------

> **Generated with Yeoman**
> 
> This project is generated with [yo angular generator](https://github.com/yeoman/generator-angular)
> version 0.12.1.
>
> **Build & development**
>
> Run `grunt` for building and `grunt serve` for preview.
>
> **Testing**
>
> Running `grunt test` will run the unit tests with karma.
