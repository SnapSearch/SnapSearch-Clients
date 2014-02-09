Snapsearch Clients
==================

Multi-Language HTTP Client Middleware Libraries for SnapSearch (https://snapsearch.io/)

Description
-----------

Snapsearch is a search engine optimisation (SEO) and robot proxy for complex front-end javascript & AJAX enabled (potentially realtime) HTML5 web applications.

Search engines like Google's crawler and dumb HTTP clients such as Facebook's image extraction robot cannot execute complex javascript applications. Complex javascript applications include websites that utilise AngularJS, EmberJS, KnockoutJS, Dojo, Backbone.js, Ext.js, jQuery, JavascriptMVC, Meteor, SailsJS, Derby, RequireJS and much more. Basically any website that utilises javascript in order to bring in content and resources asynchronously after the page has been loaded, or utilises javascript to manipulate the page's content while the user is viewing them such as animation.

Snapsearch intercepts any requests made by search engines or robots and sends its own javascript enabled robot to extract your page's content and creates a cached snapshot. This snapshot is then passed through your own web application back to the search engine, robot or browser.

Snapsearch's robot is an automated load balanced Firefox browser. This Firefox browser is kept up to date with the nightly versions, so we'll always be able to serve the latest in HTML5 technology. Our load balancer ensures your requests won't be hampered by other user's requests.

For more details on how this works and the benefits of usage see https://snapsearch.io/

Usage
-----

Make sure to register an account on SnapSearch before utilising these libraries. You will need your email and key to access SnapSearch's robot over SSL encrypted HTTP Basic Authorization.

These client libraries first automatically detect if an HTTP request comes from a search engine or robot. If it is indeed a search engine, it sends an HTTP POST request to https://snapsearch.io/ passing in parameters configuring how SnapSearch's robot should extract your content. SnapSearch will then send a HTTP GET request to the same URL and return the HTTP response (status code, headers and content) as a JSON response to the library. The client library then returns that data back to your application. You will have to select which data to present to the search engine. It is recommended to return the status code and content but not the headers, due to potential header mismatch with content encoding. However if you have specific headers that are important, then first test if it works with a simple HTTP client before deploying it.

The libraries are highly configurable including: 

- The list of clients to intercept based on the User Agent. 
- A list routes and extensions to ignore intercepting.
- Request parameters to Snapsearch.

All of the libraries that are ran on the application level requiring a server side programming language.

For specific usage and installation instructions navigate to the top level directories and then to specific frameworks provided.

Please submit pull-requests for new libraries and middlewares or additions to the list of Robots.json. We're always happy to have more. Individual project repositories have their own Robots.json, but you will need to submit pull-requests to this Robots.json so we can distribute it to all of the language bindings.

You can clone this repository and all the submodule repositories by running `git clone --recursive https://github.com/SnapSearch/Snapsearch-Clients.git`. Older git versions see: http://stackoverflow.com/q/3796927/582917

To update this repository and all submodule repositories run `git submodule foreach git pull origin master`.

To push changes to submodules, make sure to pull in the latest from the submodules and then `git push --recurse-submodules=on-demand`.

API Documentation for https://snapsearch.io/
-------------------------------------------

API documentation can be found here: https://snapsearch.io/docs

Notes
-----

### Dealing with non-HTML resources
You need to make sure that non-HTML resources are not intercepted by SnapSearch. Non-HTML resources refer to:

1. Static files that are served through your application and not through an HTTP server such as NGINX or Apache.
2. Downloads that served through an application level controller.
3. Text data interchange formats that are not meant to be used for the end user's browser. For example: JSON, XML, RSS... etc.
4. API resources that don't display the front end site, but are there for interaction between machines.
5. Any connections that do not go through the HTTP protocol. 

You can prevent SnapSearch from intercepting these non-HTML resources by:

1. SnapSearch clients can optionally check if the URL path leads directly a real file on the webserver. This option can be switched on in order to ignore the request, however it will still intercept the request if the real file ends up bing an application specific file suchas `.php` for PHP applications or `.js` for Node applications. You should only do this if you know that your application serves static files and the paths are not predictable.
2. Setup an array of whitelisted regular expression routes which will be matched against the request URL. SnapSearch will not intercept any routes that are not on the whitelist.
3. Setup an array of blacklisted regular expression routes which will be matched against the request URL. SnapSearch will not intercept any routes that are on the blacklist.
4. In MVC style applications, you may have a single controller which is responsible for displaying the front end code. If you execute our clients inside that particular controller, then you will not have any problems with non-HTML resources, since it can only intercept requests that go to the front end.

### Hashbang & Pushstate Urls
Talk about Google's AJAX crawling scheme! https://developers.google.com/webmasters/ajax-crawling/docs/specification

### SSL issues
SnapSearch will not be able to capture from sites that have invalid SSL certificates. Make sure your SSL is a valid certificate that will work a normal browser before using SnapSearch. This is on our todo list to fix.

### Dealing with redirects
Snapsearch will follow through any redirects and correctly parse the final destination. It works with:
	
1. Header redirects
2. Javascript synchronous redirects
3. Javascript asynchronous redirects
4. Meta refresh tag redirects

If your site redirects infinitely, Snapsearch will return with an error. The maximum number of redirects is 20 for header redirects and 10 for JS and Meta refresh tag redirects.

### Ensuring Analytics Works with Snapsearch
When Snapsearch visits your site, it will come with a UserAgent of "SnapSearch". You can however configure this to your own liking. Use this user agent in order to filter out our requests when using web analytics.

### Large content
SnapSearch will timeout the request if the initial request to your site and its synchronous resources takes longer than 30 seconds.

### Supporting JS disabled Browsers
It's not possible to detect if the HTTP client supports Javascript on the first page load. Therefore you have to know the user agents beforehand. A workaround involves the HTML Meta Refresh tag. You set an HTML meta refresh tag which will refresh the browser and point it to the same url but with query parameter indicating to the server that the client doesn't run javascript. This meta refresh tag can be then be cancelled using javascript. Another approach would be to use the Noscript tag and place the meta refresh tag there. None of these methods are guaranteed to work. but if you're interested check out: http://stackoverflow.com/q/3252743/582917

### Soft 404s
Soft 404s should be avoided. We believe that the final representation to the search engine should be exactly the same as normal user with a normal browser would see. You should manipulate the final presentation of information. So if you need real 404s, make them real by handling them on the server, or redirecting them from the client to the server.

### API Limits
You should select a plan that best matches your estimates for the number of "crawls" that search engine robots execute on your site. The number of crawls can be estimated by getting the average monthly number of crawls from the relevant search engines. This information can be extracted from the web master tools or server side analytics that track robots. Google analytics or javascript based analytics cannot be used since most search engines do not execute javascript. For example, let's take the 3 largest international search engines, that is Google, Bing and Yahoo. To get Google's crawl rate, you need to have your webmaster tools setup and check the Crawl Stats. This will give you the monthly crawl rate. Then go to Bing's webmaster tools and check the Crawl Information. This will give the monthly crawl rate that is combined from Bing and Yahoo. Add these 2 up and you get a good estimate. Now there are other search engines involved, but it may be difficult to get crawl statistics. For each crawler you want to satisfy you'll need to take the average monthly visits between each crawler and multiply them by the number of crawlers you want to satisfy. These crawl rates can change quickly, so check often to see if you're hitting your limits.

Cached pages can be free. Number of pages is unlimited. API limits is on fresh requests.