Snapsearch Clients
==================

Multi-Language HTTP Client Middleware Libraries for SnapSearch (http://snapsearch.io)

Description
-----------

Snapsearch is a search engine optimisation (SEO) and robot proxy for complex front-end javascript & AJAX enabled (potentially realtime) HTML5 web applications.

Search engines like Google's crawler and dumb HTTP clients such as Facebook's image extraction robot cannot execute complex javascript applications. Complex javascript applications include websites that utilise AngularJS, EmberJS, KnockoutJS, Dojo, Backbone.js, Ext.js, jQuery, JavascriptMVC, Meteor, SailsJS, Derby, RequireJS and much more. Basically any website that utilises javascript in order to bring in content and resources asynchronously after the page has been loaded, or utilises javascript to manipulate the page's content while the user is viewing them such as animation.

Snapsearch intercepts any requests made by search engines or robots and sends its own javascript enabled robot to extract your page's content and creates a cached snapshot. This snapshot is then passed through your own web application back to the search engine, robot or browser.

Snapsearch's robot is an automated load balanced Firefox browser. This Firefox browser is kept up to date with the nightly versions, so we'll always be able to serve the latest in HTML5 technology. Our load balancer ensures your requests won't be hampered by other user's requests.

For more details on how this works and the benefits of usage see http://snapsearch.io

Usage
-----

Make sure to register an account on SnapSearch before utilising these libraries.

These client libraries first automatically detect if an HTTP request comes from a search engine or robot. If it is indeed a search engine, it sends an HTTP POST request to http://snapsearch.io/ passing in parameters configuring how SnapSearch's robot should extract your content. SnapSearch will then send a HTTP GET request to the same URL and return the HTTP response (headers and content) as a JSON response to the library. The client library then returns that data back to the search engine.

The libraries are highly configurable including: 

- The list of clients to intercept based on the User Agent. 
- A list routes and extensions to ignore intercepting.
- Request parameters to Snapsearch.

There's 2 types of libraries included, most of them are libraries that are ran on the application level. However some are ran on the HTTP server level. The libraries in `http_servers` are not as configurable as the application level libraries. You should only use them if your server does not involve any server side programming language.

For specific usage and installation instructions navigate to the top level directories and then to specific frameworks provided.

Please submit pull-requests for new libraries and middlewares or additions to the list of Robots.json. We're always happy to have more.

API Documentation for http://snapsearch.io/
-------------------------------------------

API documentation can be found here: [ENTER URL FOR API DOCS]

Notes
-----

### Dealing with static files
If you are using your application to serve static files and therefore not using an HTTP server such as NGINX or Apache, or when you are serving downloads through an application level controller, you need to make sure that these paths are not intercepted by SnapSearch's clients. There are two ways to do this. Firstly SnapSearch's clients can optionally check if the URL path leads directly a real file on the webserver, if so it will not intercept unless that file is on application level file such as (`.php` for PHP applications or `.js` for Node applications). However this will not resolve problems with a download controller. The second way is to set ignored routes that are regular expressions which will be matched to the current url. The second way if far more efficient then the first way because SnapSearch's clients do not have to do a filesystem check, so make sure to ignore routes that go to your static files. None of this is necessary if you are certain that static files will not be served through the application level.

### SSL issues
SnapSearch will not be able to capture from sites that have invalid SSL certificates. Make sure your SSL is a valid certificate that will work a normal browser before using SnapSearch. This is on our todo list to fix.

### Dealing with redirects
Snapsearch will follow through any redirects and correctly parse the final destination. It works with:
	
1. Header redirects
2. Javascript synchronous redirects
3. Javascript asynchronous redirects
4. Meta refresh tag redirects

If your site redirects infinitely, Snapsearch will return with an error. The maximum number of redirects is 20 for header redirects and 10 for JS and Meta refresh tag redirects.

### Dealing with non-html resources
SnapSearch will not parse non-html resources. It can however parse text based resources.

### Ensuring Analytics Works with Snapsearch
When Snapsearch visits your site, it will come with a UserAgent of "SnapSearch". You can however configure this to your own liking. Use this user agent in order to filter out our requests when using web analytics.

### Large content
SnapSearch will timeout the request if the initial request to your site and its synchronous resources takes longer than 30 seconds.

### Supporting JS disabled Browsers
It's not possible to detect if the HTTP client supports Javascript on the first page load. Therefore you have to know the user agents beforehand. A workaround involves the HTML Meta Refresh tag. You set an HTML meta refresh tag which will refresh the browser and point it to the same url but with query parameter indicating to the server that the client doesn't run javascript. This meta refresh tag can be then be cancelled using javascript. Another approach would be to use the Noscript tag and place the meta refresh tag there. None of these methods are guaranteed to work. but if you're interested check out: http://stackoverflow.com/q/3252743/582917