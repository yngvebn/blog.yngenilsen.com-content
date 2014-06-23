This post is part of a series of posts on AngularJS, and here are the parts so far:

- [AngularJS Part 1 - Getting started with the basics](http://blog.novanet.no/angularjs-getting-started-with-the-basics-2/)
- [AngularJS Part 2 - Getting started with Controllers](http://blog.novanet.no/angularjs-getting-started-with-controllers/)
- AngularJS Part 3 - Routes and Views
- [AngularJS Part 4 - The Factory](http://blog.novanet.no/angularjs-part4-the-factory/)
<hr>

So far, we've covered [Modules](http://blog.novanet.no/angularjs-getting-started-with-the-basics-2/) and [Controllers](http://blog.novanet.no/angularjs-getting-started-with-controllers/), and now it's time to move on to some navigation and views. 

**Views**

There are a couple of ways of rendering some HTML dynamically in AngularJS. In this post we'll cover `ng-include` and `ng-view`. The first will render a HTML-template inline in your application based on either a fixed string, a string composed by values from your controller or just a plain Javascript expression. We'll stick to the first option for now, and look at more advanced techniques later on. 

I decided I needed some style in our web-app, so I added [Twitter Bootstrap 3](http://getbootstrap.com) to my solution, and created a simple layout from the examples on their website. Let's start by extracting the entire navbar out into an external View. We create a new folder in our app called "Navbar", and add a Navbar.html to that folder. Our structure should look like this:

    /
        /app/app.js
        /app/Home/Home.js
        /app/Navbar/Navbar.html
        /scripts/angular.js
        /views/main/index.cshtml

We then take the entire markup within the outer navbar-div and move into that HTML-file. In order to show this in our Index.cshtml, we are going to use the `ng-include` directive from AngularJS. It's pretty straight-forward:

<pre class="language-markup"><code class="language-markup">&lt;div ng-include=&quot;&#x27;/app/navbar/navbar.html&#x27;&quot; ng-controller=&quot;Navbar&quot; class=&quot;navbar navbar-inverse navbar-fixed-top&quot; role=&quot;navigation&quot;&gt;
&lt;/div&gt;
	</code>
</pre> 

This simply states that we would like the contents of this element to be rendered from `/app/navbar/navbar.html`. If you didn't notice I've also stated that the content should utilize a new controller named `Navbar`, so let's add navbar.js to the folder Navbar (next to the Navbar.html file):

<pre class="language-javascript"><code class="language-javascript">angular.module(&#x27;app&#x27;).controller(&#x27;Navbar&#x27;,
    [&#x27;$scope&#x27;, function($scope) {

    }]
);</code>
</pre>

Keep it empty for now, and we'll add some functionality later on.

Reload, and the page should look the same as before we extracted the navbar. 

Now we can make some proper use of the bound textbox from our first couple of posts. Let's move that to our navbar and use it as a search-box! I'm not going to go into detail on the markup, but it looks like this (pay attention to the div with the form in it):

<pre class="language-markup"><code class="language-markup">&lt;div class=&quot;container&quot;&gt;
        &lt;div class=&quot;col-md-9&quot;&gt;
            &lt;button type=&quot;button&quot; class=&quot;navbar-toggle&quot; data-toggle=&quot;collapse&quot; data-target=&quot;.navbar-collapse&quot;&gt;
                &lt;span class=&quot;sr-only&quot;&gt;Toggle navigation&lt;/span&gt;
                &lt;span class=&quot;icon-bar&quot;&gt;&lt;/span&gt;
                &lt;span class=&quot;icon-bar&quot;&gt;&lt;/span&gt;
                &lt;span class=&quot;icon-bar&quot;&gt;&lt;/span&gt;
            &lt;/button&gt;
            &lt;a class=&quot;navbar-brand&quot; href=&quot;#&quot;&gt;Project name&lt;/a&gt;
        &lt;/div&gt;
        &lt;div class=&quot;col-md-3 pull-right&quot;&gt;
            &lt;form class=&quot;navbar-form navbar-right form-inline&quot; ng-submit=&quot;search(searchTerm)&quot; role=&quot;form&quot;&gt;

                &lt;div class=&quot;input-group&quot;&gt;
                    &lt;input type=&#x27;text&#x27; placeholder=&quot;Search&quot; name=&quot;q&quot; ng-model=&quot;searchTerm&quot; class=&quot;form-control&quot;&gt;
                    &lt;span class=&quot;input-group-addon&quot;&gt;
                        &lt;i class=&quot;glyphicon glyphicon-search&quot;&gt;&lt;/i&gt;
                    &lt;/span&gt;
                &lt;/div&gt;
                &lt;button class=&quot;btn sr-only&quot; type=&quot;submit&quot;&gt;&lt;/button&gt;

            &lt;/form&gt;
        &lt;/div&gt;&lt;!--/.navbar-collapse --&gt;
    &lt;/div&gt;
</code>
</pre>

As you see I've added a new directive to our form called `ng-submit`. This simply binds to the submit-action of our form and executes the expression. In this case it executes the `search(searchTerm)` function, and if you look at our textbox, it's now bound to the scope-property searchTerm in `ng-model`. (Remember how AngularJS can bind to values even if they're not defined on our controller? Well that's what's going on here). Even though AngularJS understands the value `searchTerm` it wont know what to do with the function `search(searchTerm)` in `ng-submit`, so we'll need to create that function in our Navbar-controller:

<pre class="language-javascript">
<code class="language-javascript">
angular.module(&#x27;app&#x27;).controller(&#x27;Navbar&#x27;,
    [&#x27;$scope&#x27;, function($scope) {
        $scope.search = function(term) {
        };
    }]
);
</code>
</pre>

If you type something in the box and hit search, the function will be triggered, but nothing will happen, since nothing is happening in our function. We'll tend to that in a minute, but first let's take a detour.

**Routing**

AngularJS includes a module for routing called `ng-route` that requires us to add angular-route.js to our solution. The module includes some services and providers we can use in order to create navigation in our app. We'll start off with some pretty basic routing, and cover advanced topics later on. I want to continue with my 1-feature-1-file structure, so we'll add `app.routes.js` to the root next to `app.js`:

    /
        /app/app.js
        /app/app.routes.js
        /app/Home/Home.js
        /app/Navbar/Navbar.html
        /scripts/angular.js
        /views/main/index.cshtml

Before we can start using routes, we'll need to add a dependency to our main app module:

`angular.module(app, ['ngRoute'])` 

This tells AngularJS to make all modules and services in the `ngRoute` module to be available to our application.

In the new file we'll need to hook on our modules `config` function. Every module we create has a `config()` and a `run()` function we can hook on to. The first does pre-startup configuration, and is used for doing stuff that needs to be set up before the app can function. We'll look at `run()` later on, let's focus on `config()` for now.

The `config()` function takes in a function just like we set up controllers and the other stuff in AngularJS, and that means we can take in dependencies. That's great, because we need a dependency on `$routeProvider` from `angular-route.js` in order to set up our routes. Let's start by adding the following code:

<pre class="language-javascript"><code class="language-javascript">angular.module(&#x27;app&#x27;).config([&#x27;$routeProvider&#x27;, function($routeProvider) { 

}]);</code>
</pre>

Setting up a route is straight-forward, and revolves around using the function `.when()` on the `$routeProvider`. The function takes two arguments - a URL and an object with options. 

<pre class="language-javascript"><code class="language-javascript">angular.module(&#x27;app&#x27;).config([&#x27;$routeProvider&#x27;, function($routeProvider) {
        $routeProvider.when(&#x27;/&#x27;, {
            controller: &#x27;Home&#x27;,
            templateUrl: &#x27;/app/Home/Home.html&#x27;
        }).otherwise({ redirectTo: '/'});
     }
]);</code>
</pre>

We start off by a simple root URL for our first route. In our options we tell AngularJS to use a controller and a template at a specific path. Go ahead and add the Home.html-file and type something in it. I've also added a call to `otherwise()` which tells AngularJS what to do if the URL entered doesn't match any known routes. In our case we just redirect to '/'.

Now we need to instruct AngularJS to inject this template at some place in our markup. This is achieved by the directive `ng-view`, and we can add this directive as an attribute on an element somewhere on our index-page. I've removed all the dummy content between the navbar and the footer, and just let this simple markup remain:

<pre class="language-markup"><code class="language-markup">&lt;div class=&quot;container&quot; ng-view&gt;     
&lt;/div&gt;</code>
</pre>

> Don't forget to add our newly created .js-files as references in your HTML. Things might silently (or not so silently) crash if you forget :) All your modules, controllers, etc. go after the app.js-reference.

Now that we have our initial route configured and some element in our markup to render the template, AngularJS will take care of the rest. Since we added the `otherwise()` route, AngularJS will automatically reroute our application to #/, because it defaults to using hashbangs, and # is the default separator. 

**_Diversion:_**

_All your URLs will look like that in your application, but it can be easily overridden by adding some stuff to our configuration (remember to also take a dependency on $locationProvider in addition to $routeProvider in the config-call):_

<pre class="language-javascript"><code class="language-javascript">$locationProvider
  .html5Mode(false)
  .hashPrefix('!');</code>
</pre>

_This will substitute # with ! and the
<code>html5mode(false)</code> setting tells AngularJS it has to use hashbangs. We usually do this because some other routing-mechanism (ASP.NET MVC-routing for instance) will conflict with html5mode (which makes your URLs look like normal URLs)._

Now that we have our basic route set up, and our view we should see our app render the `Home.html` template where we added the `ng-view` directive.

Let's move on and make that search-box do something worthwhile. We'll add another route, and since the `.when()` function returns the `$routeProvider` instance, we can simply chain more routes onto our config:

<pre class="language-javascript"><code class="language-javascript">angular.module(&#x27;app&#x27;).config([&#x27;$routeProvider&#x27;, function($routeProvider) {
        $routeProvider.when(&#x27;/&#x27;, {
            controller: &#x27;Home&#x27;,
            templateUrl: &#x27;/app/Home/Home.html&#x27;
        }).when(&#x27;/Search&#x27;, {
            controller: &#x27;Results&#x27;,
            templateUrl: &#x27;/app/Search/Results.html&#x27;
        }).otherwise({ redirectTo: '/'});
     }
]);</code>
</pre>

I've added a new route called `Search` which requires a view and a controller. So let's create a simple controller called `Results.js` in a new folder we call `Search`. Throw in a `Results.html` in the same folder, and we can move on. Leave the controller empty for a second, while we revisit `Navbar.js`:

<pre class="language-javascript"><code class="language-javascript">angular.module('app').controller('Navbar',
    ['$scope', '$location',  function($scope, $location) {
        $scope.search = function(term) {
$location.path('/search/results').search('q', term);
        };
    }]
);</code>
</pre>

I've added a new dependency in the Navbar-controller called `$location`. This is our go-to provider when we need to navigate around our application from code. The provider has a couple of methods that take care of setting the URL in our browser according to how we've set up the `$locationProvider` earlier. 

In the code above, we'll use `.path()` which simply sets the path after the #, and `.search()` which appends a querystring to that URL. The result will be something like `#/search/results?q=term` where term is whatever we pass in as the second argument in `.search()`. The code above dictates that when my search-form is submitted (remember `ng-submit` from earlier?) the application should be redirected to the search-results page. Let's put some code in our `Results.js` and some simple markup in `Results.html`:

**Results.js**

<pre class="language-javascript"><code class="language-javascript">angular.module(&#x27;app&#x27;).controller(&#x27;Results&#x27;,
    [&#x27;$scope&#x27;,&#x27;$location&#x27;, function($scope, $location) {
            $scope.searchTerm = $location.search()[&#x27;q&#x27;];
        }]
);</code>
</pre>

**Results.html**

`<h3>You searched for {{searchTerm}}</h3>`

Here we simply extract the value of `q` from the current querystring (`.search()` returns an object with all querystring values neatly organized), and assign it to a scope-value. The value is simply displayed in the markup. Now when we search for something in our search-box, we're redirected to the results-page and the term is displayed. 

**Wrapping up**

In this post we've seen how easy it is to get navigation and views up and running with AngularJS. As we move on, we'll find some data to display in our search-results, and for that we'll go into more detail on the various services AngularJS enables us to create. Hope you like the series so far, and stay tuned for more!

Thanks for reading!

<hr>[Complete code for this post on Github.com](https://github.com/yngvebn/blog-angular/tree/Part3)
