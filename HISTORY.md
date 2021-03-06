== 0.5.2 [03/14/2010]

Fixed: 

- Error handling changed from 0.5.1 killed forms that weren't supposed to be caught by sammy.

New: 

- New package.json file for easy install support with Jim. Also places some project meta-data in a universal place.

== 0.5.1 [03/06/2010]

New:

- EventContext#swap is a shortcut to Application#swap (used within `partial`)
- The contents of a regexp route match are now forwarded to the route callback as args.

Changed: 

- Error handling has been refactored. All errors are now sent through #error() which will raise or just log errors based on the `raise_errors` setting. the  `silence_404` setting has been removed.
- use() now sends errors through error()
- The Sammy.Application no longer adds the app to the elements .data()
- The Sammy.Store KVO better conforms to Sammy.Application#bind() [Thanks dpree!]

Fixed:

- The unload bindings have been updated to correctly unload on window.onbeforeunload
- Sammy core now passes JSLint (basic settings).
- Sammy.Store#set() should return the original value, not the stringified one [Thanks Mark Needham]
- Updated all the examples (especially the backend example to work with latest sammy. [Thanks to SamDeLaGarza]
- SessionStorage in Firefox does not completely conform to the HTML5 Storage spec, added workaround for Sammy.Storage [Thanks dpree!]


== 0.5.0 [02/15/10]

New:

- Sammy.Haml is a plugin wrapper around haml-js that provides client side rendering of Haml templates (Thanks Tim Caswell!)
- Sammy.Mustache can now accept Mustache partials as a third argument or passed as {partials:} in data. (thanks dpree!)
- around filters with Sammy.Application#around(). Wraps an entire route execution path in a function()
- Sammy.Application#contextMatchesOptions() as a method for filtering which before() filters to process when running a route.
- test_server is a simple Sinatra/vegas app for running the tests on a local ruby server (allows for testing functionality that requires the http:// protocol)
- Sammy.Store#load(key, path) loads file at path into key
- partial() will now iterate over an array of data, calling the callback for each element, or appending the collected result.
- route() takes the psuedo-verb 'any' which appends the path/callback to all the verbs. Also, added the shortcut method any(). If only a path and callback are supplied to route(), the verb is assumed to be 'any'.
- Passing a string as the callback argument to route() looks up that method on the application.
- Sammy.Application#mapRoutes() takes an array of routes (as arrays of arguments) and adds them to the app.
- Sammy() is a function itself which provides an easy hook for looking up/creating/and extending applications with the element_selector as the unique identifier.
- The application level setting 'template_engine' lets you define a default template engine to fall back on for partial() rendering regardless of extension.
- All application modifying methods return the application instance allowing for jQuery-esque chaining for app creation.

Changed:

- the separate Sammy.Cache plugin is now deprecated in favor of one included/built-upon Sammy.Storage. It is still in the repository, but will be removed completely in 1.0
- Tests are now on top of qunit (http://github.com/jquery/qunit). jQunit has been unmaintained for a while. Also, removed dependency on external jqunit-spec repo.
- Updated to latest mustache.js (0.2.2) in Sammy.Mustache
- Removed .extend() and .clone() from Sammy.Object (unused)
- Sammy now requires jQuery >= 1.4.1
- Forms bound to post/put/delete routes no longer have to be manually re-bound with triggering('changed'). The application listens for 'submit' events on forms within the context of the element_selector instead. (Relies on the submit bubbling in jQuery 1.4.1)
- $.sammy is an alias for Sammy() instead of new Sammy.Application

Fixed:

- Only fire KVO once when setting a Sammy.Store key
- Ensure all keys are strings in Sammy.Store.
- refesh() with new location proxies (Thanks ZhangJinzhu!)


== 0.4.1 [01/11/10]

New:

- Add rake generate task that builds a simple (server-less) sammy app structure

Fixed:

- decode all parsed params (thanks jdknezek)
- Fix Sammy.JSON fails in IE6
- Fixed test suite in IE8
- Fixed console.log is not a real function in IE8

== 0.4.0 [01/04/10]

New:

- Sammy.Storage and Sammy.Session are wrappers around a new prototype Sammy.Store, which is an class that abstracts access to the various types of in browser storage, including HTML5 DOM Storage and Cookies. This allows for a unified way of accessing and storing data locally for a Sammy app.
- Sammy.Mustache is a plugin that adds support for the Mustache template framework through @janl's Mustache.js.
- Sammy.JSON is simple plugin that includes the json2.js source and also provides a simple helper for doing JSON to/from conversion
- Sammy.Application#helper() is a shortcut for adding a single helper by name. This allows for more easily defined dynamically named helpers.

Changed:

- Sammy no longer depends exclusively on polling to determine changes in its location. The work of determining location change was pulled out of Sammy.Application entirely and into LocationProxy prototypes. The new default Sammy.HashLocationProxy will use the native 'onhashchange' event where available, and resort to a single global poller when not. The LocationProxy object can be set for each Sammy.Application. Location proxies can be defined added, and will work as long as they conform to the simple prototype laid out by the HashLocationProxy.

Fixed:

- URL query params are properly URL decoded. [Thanks  Lee Semel]

== 0.3.1 [12/09/09]

New: 

- Sammy.NestedParams is a new plugin that handles form fields like Rails/Rack's nested params. See the docs for more info [thanks endor!]
 
Changed:

- Sammy.Application#_parseFormParams is now a single point of entry that takes a form and should return a set of params. NestedParams hooks into this.
  - The Sammy.Application constructor no longer requires an app function. This allows you to create multiple apps quickly and assign routes/etc. later.
  - Sammy.Template takes a second option which is the proxy/extension you want to associate it with.
  
Fixed:

- Fixed a bug in IE only where with two consecutive routes that contain params, the second set of params will not be filtered properly [thanks Scott McMillin!]
  

== 0.3.0 [09/28/09]

New:

- Sammy.Application#use() takes an app function and applies it to the current app. This is the entry point for Sammy Plugins. See docs at: http://code.quirkey.com/sammy/docs/plugins.html
  - New system for repository structure, minified files are placed in lib/min/ version numbers are appended to minified files
  - Sammy.EventContext#partial() is template engine agnostic and calls the template engine method based on the extension of the file you're trying to render.
  - New official Sammy.Cache plugin provides simple client side caching
  - Sammy.EventContext#redirect() can take any number of arguments that are all joined by '/'
  - Sammy.Application#refresh() will re-run the current route
  
Changes:

- Removed John Resig's Class() inheritance code/style in favor of doing prototypical inheritance and using $.extend()
- Sammy.Application bind() and trigger() now use jQuery's built in namespacing. This means that a Sammy application can now catch events like clicks and other events that bubble up or are triggered on the Sammy.Application#element() 
- Sammy.log and Sammy.addLogger are top level access to logging and adding additional logging paths. Sammy.Application#bindToAllEvents() replaces the functionality for the former addLogger() method
- the app functions and route callbacks both take _this_ as the first argument
- $.srender and template() are no longer part of the sammy.js and are instead included in the Sammy.Template plugin lib/plugins/sammy.template.js
- Routes are saved and looked up in order of definition instead of shortest first (May break existing applications that use RegExp based routing)
- Made the parse query more uniform with the rest of the code base
- Sammy.Object#toString() wont include functions unless you explicitly want them
  
Fixes:
- Fixed redirect() handling in post routes
- Fixed param parsing for form submission where there were multiple params with the same name

== 0.2.1 [08/28/09]

New:

- Query string parameters: You can now pass extra parameters to a route using the traditional Query string params scheme. e.g #/my/route?var1=blah will give you params['var1'] #=> 'blah' inside an Event context. (Thanks to Jesse Hallet [halletj]) See the commit (cb309b91c0ab80d4e8d6ef9bc97314607cc0da76) for more info.

Fixed:

- Redirection in POST routes wasnt working properly. (Thanks Russel Jones [CodeOfficer])
- Dont cache partial templates in debug mode. (Thanks Jonathan Vaught [gravelpup])
- Spelling and grammar fixes to README/Docs (Thanks Jason Davies [jasondavies])
  

== 0.2.0 [06/01/09]

New:

- Location Overrides: All location methods refer to two Sammy.Application methods: getLocation() and setLocation() which return and take a string, respectively. The default behavior is to pull the location from window.location.hash, but these methods can be overridden to provide alternate location strategies. Theres an example in examples/location_override. (thanks to britg, CodeOfficer) 
- Sammy.Object#toHash() returns a JS object with any functions stripped. Useful for using with params.
- Sammy.Application#swap() is the method called within partial() for changing the content of $element(). The default behavior is just to use $.fn.html(), but can be overridden to provide some fancy animations.
  
Fixed:

- The 'changed' event is only fired at run() and after a partial's callback. This is the event to bind to, to check the DOM after a partial() call. (thanks to hpoydar).
- template() (aka $.srender) handles single quoted attributes now (aka generated HAML)
- When the app booted up, run() would redirect to the start_url even if another location was present.
- the _checkLocation() method now sets last_location properly (which fixes continual checking if route isnt found)