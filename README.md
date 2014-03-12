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

SnapSearch provides multi-language framework agnostic middleware to easily integrate SnapSearch into your existing stack. Make sure to register an account on SnapSearch before utilising these libraries. You will need your email and key to access SnapSearch's robot over SSL encrypted HTTP Basic Authorization.

All of the libraries that are ran on the application level requiring a server side programming language.

For specific usage and installation instructions navigate to the specific middleware submodules.

We welcome pull-requests for new middlewares or additions to the `resources/robots.json` or `resources/extensions.json`. Individual project repositories have their own `resources/robots.json` and `resources/extensions.json`, but you will need to submit pull-requests to this repository so we can distribute it to all of the language bindings.

Documentation
-------------

For API documentation and best practices check out https://snapsearch.io/documentation

Development
-----------

### Cloning this repository

You can clone this repository with all of its submodules by doing:

```
git clone --recursive https://github.com/SnapSearch/Snapsearch-Clients.git
```

Older git versions see: http://stackoverflow.com/q/3796927/582917

### Pulling in submodules

Update this repository's submodules by doing:

```
git submodule foreach git pull origin master
```

### Pushing to submodules

To push changes to submodules, make sure to pull in the latest from the submodules and then run:

```
git push --recurse-submodules=on-demand
```

### Adding new submodules

Add new middleware by doing:

```
git submodule add <uri> language/reponame
```