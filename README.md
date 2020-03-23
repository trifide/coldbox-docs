# Introduction

## Introduction

```text
   ____      _     _ ____            
  / ___|___ | | __| | __ )  _____  __
 | |   / _ \| |/ _` |  _ \ / _ \ \/ /
 | |__| (_) | | (_| | |_) | (_) >  < 
  \____\___/|_|\__,_|____/ \___/_/\_\
```

### ColdBox Manual - v5.0.0

ColdBox is a [conventions-based](https://en.wikipedia.org/wiki/Convention_over_configuration) [HMVC](https://en.wikipedia.org/wiki/Model–view–controller) development framework for [ColdFusion](http://en.wikipedia.org/wiki/Adobe_ColdFusion) \([CFML](https://en.wikipedia.org/wiki/ColdFusion_Markup_Language)\). It provides a set of reusable code and tools that can be used to increase your development productivity as well as a development standard for working in team environments.

ColdBox is natively based on [modular architecture](https://en.wikipedia.org/wiki/Modular_design) which helps address most infrastructure concerns of typical web applications and thus called an HMVC framework.

### Versioning

ColdBox is maintained under the [Semantic Versioning](http://semver.org) guidelines as much as possible.Releases will be numbered with the following format:

```text
<major>.<minor>.<patch>
```

And constructed with the following guidelines:

* Breaking backward compatibility bumps the major \(and resets the minor and patch\)
* New additions without breaking backward compatibility bumps the minor \(and resets the patch\)
* Bug fixes and misc changes bumps the patch

### License

The ColdBox Platform is open source and licensed under the [Apache 2](https://www.apache.org/licenses/LICENSE-2.0.html) License. If you use ColdBox, please try to make mention of it in your code or web site or add a Powered By Coldbox icon.

* Copyright by Ortus Solutions, Corp
* ColdBox is a registered trademark by Ortus Solutions, Corp

> **Info**: The ColdBox Websites, Documentation, logo and content have a separate license and they are a separate entity.

### Discussion & Help

The ColdBox help and discussion group can be found here: [https://groups.google.com/forum/\#!forum/coldbox](https://groups.google.com/forum/#!forum/coldbox)

### Reporting a Bug

We all make mistakes from time to time :\) So why not let us know about it and help us out. We also love pull requests, so please star us and fork us: [https://github.com/coldbox/coldbox-platform](https://github.com/coldbox/coldbox-platform)

* By Email: [bugs@coldbox.org](mailto:bugs@coldbox.org)
* By Jira: [https://ortussolutions.atlassian.net/browse/COLDBOX](https://ortussolutions.atlassian.net/browse/COLDBOX)

### Professional Open Source

ColdBox is a professional open source software backed by [Ortus Solutions, Corp](https://www.ortussolutions.com/services) offering services like:

* Custom Development
* Professional Support & Mentoring
* Training
* Server Tuning
* Security Hardening
* Code Reviews
* [Much More](http://www.ortussolutions.com/services)

### Resources

* Official Site: [https://www.coldbox.org](https://www.coldbox.org)
* Source Code: [https://github.com/coldbox/coldbox-platform](https://github.com/coldbox/coldbox-platform)
* Bug Tracker: [https://ortussolutions.atlassian.net/browse/COLDBOX](https://ortussolutions.atlassian.net/browse/COLDBOX)
* Twitter: [@coldbox](http://www.twitter.com/coldbox)
* Facebook: [https://www.facebook.com/coldboxplatform](https://www.facebook.com/coldboxplatform)
* Google+: [https://www.google.com/+ColdboxOrg](https://www.google.com/+ColdboxOrg)
* Vimeo Channel: [https://vimeo.com/channels/coldbox](https://vimeo.com/channels/coldbox)

#### HONOR GOES TO GOD ABOVE ALL

Because of His grace, this project exists. If you don't like this, then don't read it, it's not for you.

> "Therefore being justified by **faith**, we have peace with God through our Lord Jesus Christ: By whom also we have access by **faith** into this **grace** wherein we stand, and rejoice in hope of the glory of God." Romans 5:5

## What's New With 5.6.0

ColdBox 5.6.0 is a minor version update with lots of fixes, improvements, performance enhancements and some nice new features. Below are the major areas of improvement and the full release notes. To update ColdBox or any of the standalone libraries just leverage CommandBox:

* `update coldbox`
* `update logbox`
* `update wirebox`
* `update cachebox`

### Major Updates

#### Performance

We had two specific tickets that have resulted in extreme performance improvements for ALL ColdBox requests. You will feel and see the difference:

* \[[COLDBOX-799](https://ortussolutions.atlassian.net/browse/COLDBOX-799)\] - Event Handler Bean: Single instance per handler action for major performance improvements

This ticket was contributed by Dom Watson \([https://twitter.com/dom\_watson](https://twitter.com/dom_watson)\) one of the lead engineers of the amazing PresideCMS project built on top of ColdBox. We worked together to avoid the creation of handler beans on each runnable event. We now cache each event handler bean representation which results in an extreme boost in performance. Thanks Dom!

* \[[COLDBOX-810](https://ortussolutions.atlassian.net/browse/COLDBOX-810)\] - Remove afterInstanceAutowire interceptor in handlerService as afterHandlerCreation is now officially removed.

Thanks to our local mad scientist Brad Wood, he reported that the handler services still listened to ALL CFC creations in an application in order to relay an `afterHandlerCreation` interception point from the good 'ol 2.6 days. This has been finally removed and boom, another big boost in performance!

#### Better Bug Reports

We have enhanced the bug reporting templates to include much more information when dealing with exceptions:

* Show SQL error details on Adobe CF
* Current route, params and debug info
* Contributing module for the current routed URL

#### Merging of HTTP Verbs

Thanks to our very own Eric Peterson, you can now merge HTTP verbs on the same route pattern, which you could not do before:

```java
router
    .post( "photos/", "photos.create" )
    .get( "photos/", "photos.index" )
    .delete( "photos/", "photos.remove" );
```

### ColdBox Core  Release Notes

#### Bugs

* \[[COLDBOX-778](https://ortussolutions.atlassian.net/browse/COLDBOX-778)\] - ModuleService to add default route doesn't work correctly
* \[[COLDBOX-794](https://ortussolutions.atlassian.net/browse/COLDBOX-794)\] - Fix default bug report to show SQL error detail for adobe SQL exceptions
* \[[COLDBOX-796](https://ortussolutions.atlassian.net/browse/COLDBOX-796)\] - When doing package resolving if you are in a module it still tries to resolve a module
* \[[COLDBOX-806](https://ortussolutions.atlassian.net/browse/COLDBOX-806)\] - Error in HTML helper WRAPPERATTRS doesn't exist in argument scope
* \[[COLDBOX-811](https://ortussolutions.atlassian.net/browse/COLDBOX-811)\] - Include the colon for non 80 or 443 port numbers \#419 in github

#### New Features

* \[[COLDBOX-812](https://ortussolutions.atlassian.net/browse/COLDBOX-812)\] - Allow merging of HTTP verbs when doing separate verbs for the same route
* \[[COLDBOX-813](https://ortussolutions.atlassian.net/browse/COLDBOX-813)\] - Update cfconfig to use env variables instead of inline mixins, modernizeOrDie

#### Improvements

* \[[COLDBOX-795](https://ortussolutions.atlassian.net/browse/COLDBOX-795)\] - Add more current route information to the BugReport.cfm template
* \[[COLDBOX-797](https://ortussolutions.atlassian.net/browse/COLDBOX-797)\] - Ability for bug reports and app to know which module contributed the incoming URL route.
* \[[COLDBOX-798](https://ortussolutions.atlassian.net/browse/COLDBOX-798)\] - Use of .keyExists\(\) can needlessly use memory in requests, suggest StructKeyExists\(\) instead
* \[[COLDBOX-799](https://ortussolutions.atlassian.net/browse/COLDBOX-799)\] - Event Handler Bean: Single instance per handler action for major performance improvements
* \[[COLDBOX-800](https://ortussolutions.atlassian.net/browse/COLDBOX-800)\] - HandlerService.cfc$newHandler\(\): declares variables that are never used
* \[[COLDBOX-810](https://ortussolutions.atlassian.net/browse/COLDBOX-810)\] - Remove afterInstanceAutowire interceptor in handlerService as afterHandlerCreation is now officially removed.

### CacheBox Release Notes

#### Bugs

* \[[CACHEBOX-56](https://ortussolutions.atlassian.net/browse/CACHEBOX-56)\] - AbstractCacheProvider.getOrSet\(\): local var unscoped when checking if null

## What's New With 5.5.0

ColdBox 5.5.0 is a minor version update with lots of fixes, improvements, performance enhancements and some nice new features. Below are the major areas of improvement and the full release notes. To update your ColdBox installation just leverage CommandBox:

* `update coldbox`
* `update logbox`
* `update wirebox`
* `update cachebox`

The major libraries updated in this release are ColdBox MVC and WireBox.

### Compatibility Notes

If you are using annotations for component **aliases** you will have to tell WireBox explicitly to process those mappings. As by default, we no longer process mappings on startup.

```javascript
// Process a single mapping immediately
map( name ).process();

// Process all mappings in the mapDiretory() call
mapDirectory( packagePath="my.path", process=true )
mapDirectory( "my.path" ).process();

// Map all models in a module via the this.autoProcessModels in the ModuleConfig.cfc
this.autoProcessModels = true
```

### Major Updates

#### Performance

This release is a big performance boost for two areas of operation: **modules, and models**. The Module Service has been completely revised to make it as fast as possible when registering and activating modules. If you have an extensive usage of modules, you will feel the difference when booting up or re-initing the framework.

The other area is the inspection of models that has been lazy evaluated. This allows for faster bootups and reinits as models are only inspected on demand or when explicitly marked.

#### More Environment Detection

Our environment functions have now been added to the Framework SuperType and thus all objects in the ColdBox eco-system receive it:

* `getEnv(), getSystemSetting() and getSystemProperty()`

#### Custom Resource Patterns

As resources have become more mainstream in ColdBox, we now give you the ability to choose the URL pattern you want to attach to the resource. This allows you to create a-la-carte resource patterns and also allow you to nest resources via patterns.

```javascript
resources(
  pattern="/site/:siteId/agents",
  resource="agents"
);
```

### ColdBox Release Notes

#### Bugs

* \[[COLDBOX-786](https://ortussolutions.atlassian.net/browse/COLDBOX-786)\] - HTMLHelper typo on `getSetting` call via `elixirpath()`
* \[[COLDBOX-788](https://ortussolutions.atlassian.net/browse/COLDBOX-788)\] - Private method in handler is accessible from public \( direct link \)

#### New Features

* \[[COLDBOX-783](https://ortussolutions.atlassian.net/browse/COLDBOX-783)\] - New module directive: `autoProcessModels` which defaults to **false** to defer to lazy processing of models
* \[[COLDBOX-789](https://ortussolutions.atlassian.net/browse/COLDBOX-789)\] - Added `getEnv(), getSystemSetting() and getSystemProperty()` to super type for easier environment setting retrievals
* \[[COLDBOX-790](https://ortussolutions.atlassian.net/browse/COLDBOX-790)\] - Added much more logging and info when booting up the Module Service
* \[[COLDBOX-791](https://ortussolutions.atlassian.net/browse/COLDBOX-791)\] - `buildLink(), relocate()` now will translate raw `: to / in URL` with appropriate module entry points thanks to richard herbert
* \[[COLDBOX-792](https://ortussolutions.atlassian.net/browse/COLDBOX-792)\] - Allow nested resources and the ability for custom URL patterns for resources

#### Improvements

* \[[COLDBOX-782](https://ortussolutions.atlassian.net/browse/COLDBOX-782)\] - Add TestBox 3 and code coverage to the core
* \[[COLDBOX-785](https://ortussolutions.atlassian.net/browse/COLDBOX-785)\] - Module service performance optimizations for module activations.
* \[[COLDBOX-787](https://ortussolutions.atlassian.net/browse/COLDBOX-787)\] - Update RequestContext.cfc `getFullUrl()` to include port number

### WireBox Release Notes

#### New Features

* \[[WIREBOX-84](https://ortussolutions.atlassian.net/browse/WIREBOX-84)\] - Remove auto processing of all mappings defer to lazy loading
* \[[WIREBOX-85](https://ortussolutions.atlassian.net/browse/WIREBOX-85)\] - `MapDirectory` new boolean argument `process` which defers to **false**, if **true**, the mappings will be auto processed
* \[[WIREBOX-86](https://ortussolutions.atlassian.net/browse/WIREBOX-86)\] - New binder method: `process()` if chained from a mapping, it will process the mapping's metadata automatically.

#### Improvement

* \[[WIREBOX-87](https://ortussolutions.atlassian.net/browse/WIREBOX-87)\] - AOP debug logging as serialized CFCs which clogs log files

## What's New With 5.4.0

ColdBox 5.4.0 is a minor version update with lots of fixes, improvements, performance enhancements and some nice new features. Below are the major areas of improvement and the full release notes. To update your ColdBox installation just leverage CommandBox:

* `update coldbox`
* `update logbox`
* `update wirebox`
* `update cachebox`

### Box Namespace

In our initiative to make all Modules sharable between ColdBox, CommandBox and whatever other boxes we make in the future. We created the `box` alias for injections. In our case any injection with the prefix `coldbox` can be used as `box`

### RunRoute\(\)

We have created a new internal runner called `runRoute()` which is similar to `runEvent()` but it allows you to abstract your events by leveraging named routes. So just like you can create links based on named routes and params, you can execute named routes and params as well internally via `runRoute()`

```javascript
/**
 * Executes internal named routes with or without parameters. If the named route is not found or the route has no event to execute then this method will throw an `InvalidArgumentException`.
 * If you need a route from a module then append the module address: `@moduleName` or prefix it like in run event calls `moduleName:routeName` in order to find the right route.
 * The route params will be passed to events as action arguments much how eventArguments work.
 *
 * @name The name of the route
 * @params The parameters of the route to replace
 * @cache Cached the output of the runnable execution, defaults to false. A unique key will be created according to event string + arguments.
 * @cacheTimeout The time in minutes to cache the results
 * @cacheLastAccessTimeout The time in minutes the results will be removed from cache if idle or requested
 * @cacheSuffix The suffix to add into the cache entry for this event rendering
 * @cacheProvider The provider to cache this event rendering in, defaults to 'template'
 * @prePostExempt If true, pre/post handlers will not be fired. Defaults to false
 *
 * @throws InvalidArgumentException
 */
any function runRoute(
    required name,
    struct params={},
    boolean cache=false,
    cacheTimeout="",
    cacheLastAccessTimeout="",
    cacheSuffix="",
    cacheProvider="template",
    boolean prePostExempt=false
)
```

### CacheBox Rewritten

This should have been a major release on its own. The entire CacheBox framework was re-written in script and modernized from top to bottom. We removed all implicit variable access which gave us huge performance boosts and we streamlined all operations with modern techniques. The results are great and our maintenance will be much less in the future. A part from those optimizations we managed to add a few nice items:

* Adobe 2018 certified
* New setting `resetTimeoutOnAccess` which allows you to simulate session scopes on any CacheBox cache.  Every time a `get()` operation is done, that item's timeout will be reset.
* All cache providers get some multi function goodness: `setMulti(), getMulti(), lookupMulti(), clearMulti(),getCachedObjectMetadataMulti()`

#### LogBox Improvements

There are two major improvements we did with LogBox in this release:

1\) The file locking operations on file appenders have been streamlined to avoid high i/o operations.

2\) The console appender uses an asynchronous streaming technique which makes it extremely efficient and fast.

### ColdBox Release Notes

#### Bugs

* \[[COLDBOX-556](https://ortussolutions.atlassian.net/browse/COLDBOX-556)\] - `prePostExempt` doesn't skip around advices
* \[[COLDBOX-755](https://ortussolutions.atlassian.net/browse/COLDBOX-755)\] - Core interceptors in coldbox.cfc do not listen or register custom interception points that are contributed by modules
* \[[COLDBOX-761](https://ortussolutions.atlassian.net/browse/COLDBOX-761)\] - `invalidEventHandler` gets in to an infinite loop when the `invalidEventHandler` isn't a full event
* \[[COLDBOX-766](https://ortussolutions.atlassian.net/browse/COLDBOX-766)\] - ColdBox shutdown errors `onApplicationEnd` due to lack of application scope
* \[[COLDBOX-770](https://ortussolutions.atlassian.net/browse/COLDBOX-770)\] - Use of `event.sendFile` delivers a file with single quotes in the name
* \[[COLDBOX-773](https://ortussolutions.atlassian.net/browse/COLDBOX-773)\] - Remove default defaultValue as it never will throw an exception if missing on requestcontext on `getHTTPHeader()`

#### New Features

* \[[COLDBOX-765](https://ortussolutions.atlassian.net/browse/COLDBOX-765)\] - Update all elixir methods to match new version
* \[[COLDBOX-771](https://ortussolutions.atlassian.net/browse/COLDBOX-771)\] - Add Elixir version 3 path methods
* \[[COLDBOX-775](https://ortussolutions.atlassian.net/browse/COLDBOX-775)\] - Added `:` as a delimiter for the `route()` method when using modules to be consistent with run event
* \[[COLDBOX-776](https://ortussolutions.atlassian.net/browse/COLDBOX-776)\] - new runner: `runRoute()` that allows you to run routes internally with param passing

#### Improvements

* \[[COLDBOX-767](https://ortussolutions.atlassian.net/browse/COLDBOX-767)\] - Change "module already registered" from warn to debug
* \[[COLDBOX-774](https://ortussolutions.atlassian.net/browse/COLDBOX-774)\] - Introduce generic "**box**" namespace for Wirebox injections

### CacheBox Release Notes

#### Bugs

* \[[CACHEBOX-46](https://ortussolutions.atlassian.net/browse/CACHEBOX-46)\] - CacheboxProvider metadata and stores: use CFML functions on java hash maps breaks concurrency
* \[[CACHEBOX-50](https://ortussolutions.atlassian.net/browse/CACHEBOX-50)\] - getOrSet\(\) provider method doesn't work with full null support
* \[[CACHEBOX-52](https://ortussolutions.atlassian.net/browse/CACHEBOX-52)\] - getQuiet\(\), clearQuiet\(\), getSize\(\), clearAll\(\), expireAll\(\) broken in acf providers

#### New Features

* \[[CACHEBOX-48](https://ortussolutions.atlassian.net/browse/CACHEBOX-48)\] - New setting: \`resetTimeoutOnAccess\` to allow the ability to reset timeout access for entries
* \[[CACHEBOX-49](https://ortussolutions.atlassian.net/browse/CACHEBOX-49)\] - Global script conversion and code optimizations
* \[[CACHEBOX-53](https://ortussolutions.atlassian.net/browse/CACHEBOX-53)\] - lookup operations on ACF providers updated to leverage cacheIdExists\(\) improves operation by over 50x
* \[[CACHEBOX-54](https://ortussolutions.atlassian.net/browse/CACHEBOX-54)\] - setMulti\(\), getMulti\(\), lookupMulti\(\), clearMulti\(\),getCachedObjectMetadataMulti\(\) is now available on all cache providers

#### Improvements

* \[[CACHEBOX-47](https://ortussolutions.atlassian.net/browse/CACHEBOX-47)\] - Increased timeout on \`getOrSet\` lock
* \[[CACHEBOX-51](https://ortussolutions.atlassian.net/browse/CACHEBOX-51)\] - Consolidated usages of the abstract cache provider to all providers to avoid redundancy

### WireBox Release Notes

#### Bugs

* \[[WIREBOX-82](https://ortussolutions.atlassian.net/browse/WIREBOX-82)\] - `builder.toVirtualInheritance()`: scoping issues
* \[[WIREBOX-83](https://ortussolutions.atlassian.net/browse/WIREBOX-83)\] - When using sandbox security, and using a provider DSL the file existence checks blow up

### LogBox Release Notes

#### New Features

* \[[LOGBOX-34](https://ortussolutions.atlassian.net/browse/LOGBOX-34)\] - Console appender completely rewritten to support asynchronous streaming

#### Improvements

* \[[LOGBOX-33](https://ortussolutions.atlassian.net/browse/LOGBOX-33)\] - Improve file exists usage on file appenders to avoid i/o operations

## What's New With 5.3.0

ColdBox 5.3.0 is a minor version update with lots of fixes, improvements, performance enhancements and some nice new features. Below are the major areas of improvement and the full release notes. To update your ColdBox installation just leverage CommandBox: `update coldbox`

### ColdBox

#### Bugs

* \[[COLDBOX-741](https://ortussolutions.atlassian.net/browse/COLDBOX-741)\] - Defining UDF as **COLDBOX\_FAIL\_FAST** no longer returns false
* \[[COLDBOX-750](https://ortussolutions.atlassian.net/browse/COLDBOX-750)\] - Fixed typo\(s\) in call of function prePend\(\) on routing
* \[[COLDBOX-752](https://ortussolutions.atlassian.net/browse/COLDBOX-752)\] - base url not eliminating double slashes
* \[[COLDBOX-756](https://ortussolutions.atlassian.net/browse/COLDBOX-756)\] - Setter for valid extensions is setting the wrong variable
* \[[COLDBOX-759](https://ortussolutions.atlassian.net/browse/COLDBOX-759)\] - Cleanup the with closure on group routing to avoid leakage

#### New Features

* \[[COLDBOX-753](https://ortussolutions.atlassian.net/browse/COLDBOX-753)\] - Add new flag to router: `multiDomainDiscovery` that can be turned on/off

#### Improvements

* \[[COLDBOX-742](https://ortussolutions.atlassian.net/browse/COLDBOX-742)\] - Add trimming to asset loader to avoid spaces on links
* \[[COLDBOX-744](https://ortussolutions.atlassian.net/browse/COLDBOX-744)\] - InterceptorState + EventPool uses **syncrhonizedMap** that has locking issues: refactor to avoid these issues
* \[[COLDBOX-754](https://ortussolutions.atlassian.net/browse/COLDBOX-754)\] - set initiated bit for ColdBox after modules are activated
* \[[COLDBOX-760](https://ortussolutions.atlassian.net/browse/COLDBOX-760)\] - Performance improvements for interception registrations and discovery

### LogBox

#### Improvements

* \[[LOGBOX-32](https://ortussolutions.atlassian.net/browse/LOGBOX-32)\] - Add test and fix for adding a LogBox category after the fact

### WireBox

#### Improvements

* \[[WIREBOX-79](https://ortussolutions.atlassian.net/browse/WIREBOX-79)\] - Account for leading slashes in `mapDirectory()`
* \[[WIREBOX-80](https://ortussolutions.atlassian.net/browse/WIREBOX-80)\] - Throw a nicer DSL error if ColdBox is not linked
* \[[WIREBOX-81](https://ortussolutions.atlassian.net/browse/WIREBOX-81)\] - Performance improvements for the construction of transients

## What's New With 5.2.0

ColdBox 5.2.0 is a minor version update with lots of fixes, improvements and some new features not only to the ColdBox core but to LogBox and WireBox alike. Below are the major areas of improvement and the full release notes. To update your ColdBox installation just leverage CommandBox: `update coldbox`

### ColdBox

The major areas of improvement for the ColdBox core where on tons of fixes, however there are some nice new features discussed below.

#### Bugs

* \[[COLDBOX-597](https://ortussolutions.atlassian.net/browse/COLDBOX-597)\] - Function `addAsset` generate wrong link if asset path contains ".js"
* \[[COLDBOX-668](https://ortussolutions.atlassian.net/browse/COLDBOX-668)\] - Make implicit views case sensitive by default for linux systems
* \[[COLDBOX-677](https://ortussolutions.atlassian.net/browse/COLDBOX-677)\] - HTML HELPER - Fix a requirement for `columnName` if column is defined
* \[[COLDBOX-696](https://ortussolutions.atlassian.net/browse/COLDBOX-696)\] - Passing headers to \``request`\` breaks RoutingService when in test mode
* \[[COLDBOX-724](https://ortussolutions.atlassian.net/browse/COLDBOX-724)\] - Add `ENV` overload in `detectEnvironment()` via ENVIRONMENT system setting/property
* \[[COLDBOX-726](https://ortussolutions.atlassian.net/browse/COLDBOX-726)\] - setNextEvent does not exist in coldbox.system.web.Controller
* \[[COLDBOX-727](https://ortussolutions.atlassian.net/browse/COLDBOX-727)\] - fail fast strong typed to boolean, update to allow closures
* \[[COLDBOX-729](https://ortussolutions.atlassian.net/browse/COLDBOX-729)\] - getRenderData\(\) in base test case was not looking at the right request collection
* \[[COLDBOX-732](https://ortussolutions.atlassian.net/browse/COLDBOX-732)\] - Fail fast can't be turned off for original behavior
* \[[COLDBOX-736](https://ortussolutions.atlassian.net/browse/COLDBOX-736)\] - Module mappings disappear when not unloading ColdBox in base test case
* \[[COLDBOX-738](https://ortussolutions.atlassian.net/browse/COLDBOX-738)\] - Clear not working on string builders: Use setLength\(0\) since clear is not a method on StringBuilder
* \[[COLDBOX-740](https://ortussolutions.atlassian.net/browse/COLDBOX-740)\] - When using group\(\) operations with a handler and no explicit handler routing call added, route never registered the handler

#### New Features

* \[[COLDBOX-722](https://ortussolutions.atlassian.net/browse/COLDBOX-722)\] - New global directive: `autoMapModels` which if **true**, it will map all root models just like modules do.

This will allow root **models** to behave like modules where all models are registered automatically for you but with no namespace.

* \[[COLDBOX-737](https://ortussolutions.atlassian.net/browse/COLDBOX-737)\] - `toAction()` terminator is missing from the new router DSL

This terminator was missing from the new Routing DSL. This will allow you to build up routes that terminate at an action.

* \[[COLDBOX-720](https://ortussolutions.atlassian.net/browse/COLDBOX-720)\] - Register `config/Router.cfc` as an interceptor

The main application router and **ALL** module routers are now also interceptors.

#### Improvements

* \[[COLDBOX-730](https://ortussolutions.atlassian.net/browse/COLDBOX-730)\] - Implicitly pass args from `renderLayout()` into the rendered views
* \[[COLDBOX-739](https://ortussolutions.atlassian.net/browse/COLDBOX-739)\] - List modules which have already been processed when a module cannot be activated to help with debugging

### LogBox

#### Bugs

* \[[LOGBOX-29](https://ortussolutions.atlassian.net/browse/LOGBOX-29)\] - when using async option on FileAppender, nothing logs, well now it does!

#### New Features

* \[[LOGBOX-31](https://ortussolutions.atlassian.net/browse/LOGBOX-31)\] - Add `defaultValue` arguments to `getProperty()` methods on abstract appenders

#### Improvements

* \[[LOGBOX-30](https://ortussolutions.atlassian.net/browse/LOGBOX-30)\] - Leave off text `"ExtraInfo: "` from console appender if empty string

### WireBox

#### Bugs

* \[[WIREBOX-76](https://ortussolutions.atlassian.net/browse/WIREBOX-76)\] - Virtual inheritance not injecting generic **getters** and **setters** correctly on target objects
* \[[WIREBOX-77](https://ortussolutions.atlassian.net/browse/WIREBOX-77)\] - Virtual inheritance not inheriting `init` from super class

#### Improvements

* \[[WIREBOX-51](https://ortussolutions.atlassian.net/browse/WIREBOX-51)\] - Add method to binder to override alias of current mapping, by passing the current mapping to the influencer closure
* \[[WIREBOX-75](https://ortussolutions.atlassian.net/browse/WIREBOX-75)\] - Don't exclude path in parent mapping destinations
* \[[WIREBOX-78](https://ortussolutions.atlassian.net/browse/WIREBOX-78)\] - Simplify error message for missing dependency to be human readable

## What's New With 5.1.4

Short and sweet release!

#### Bug

* \[[COLDBOX-718](https://ortussolutions.atlassian.net/browse/COLDBOX-718)\] - Left one `encodeforhtml` in `textarea` that was missing.

## What's New With 5.1.3

This is a patch release for ColdBox

### Bugs

* \[[COLDBOX-715](https://ortussolutions.atlassian.net/browse/COLDBOX-715)\] - Elvis operator inconsistencies on Adobe Engines, please Adobe, patch the engines and fix your compiler!

### Improvements

* \[[COLDBOX-237](https://ortussolutions.atlassian.net/browse/COLDBOX-237)\] - Some HTMLHelper method still need escaping as certain values should never be HTML
* \[[COLDBOX-716](https://ortussolutions.atlassian.net/browse/COLDBOX-716)\] - determine session/client state via CF getApplicationMetadata\(\) instead of isDefined\(\) to avoid load issues for flash ram
* \[[COLDBOX-717](https://ortussolutions.atlassian.net/browse/COLDBOX-717)\] - RemotingUtil converted to cfscript \#367

## What's New With 5.1.2

This is a patch release for ColdBox

### Bugs/Regressions

* \[[COLDBOX-711](https://ortussolutions.atlassian.net/browse/COLDBOX-711)\] - HTML helper cannot add fontawesome icons in button due to XSS enabled by default
* \[[COLDBOX-712](https://ortussolutions.atlassian.net/browse/COLDBOX-712)\] - ColdBox shutdown code that uses CF mappings for modules fails on fwreinit
* \[[COLDBOX-713](https://ortussolutions.atlassian.net/browse/COLDBOX-713)\] - addAsset throw error when called from handlers

### Improvements

* \[[COLDBOX-714](https://ortussolutions.atlassian.net/browse/COLDBOX-714)\] - Too many issues when encoding by default for HTML Helper, revert to non-encoded and provide ways to encode globally and a-la-carte
* \[[COLDBOX-702](https://ortussolutions.atlassian.net/browse/COLDBOX-702)\] -  Framework setting for the automatic deserialization of JSON payloads to the RC: `jsonPayloadToRC`

### Automatic JSON Payload Setting

The automatic JSON to request collection feature defaults now to false to avoid backwards compatibility. You can easily turn it on via the setting: `coldbox.jsonPayloadToRC`

```javascript
coldbox = {
    jsonPayloadToRC = true
}
```

### HTML Helper Changes

The HTML Helper has been migrated to an internal module in this release. It allows you to configure it via the following configuration settings in your `ColdBox.cfc`.

```javascript
moduleSettings = {
    htmlHelper = {
        // Base path for loading JavaScript files
        js_path = "",
        // Base path for loading CSS files
        css_path = "",
        // Auto-XSS encode values when using the HTML Helper output methods
        encodeValues = false
    }
}
```

#### Injection Shortcut

You can also now inject the HTML helper anywhere using it's injection DSL Shortcut of `@HTMLHelper`

## What's New With 5.1.1

This is a patch release for ColdBox.

### Bug

* \[[COLDBOX-703](https://ortussolutions.atlassian.net/browse/COLDBOX-703)\] - regression onmissingmethod on html helper method was public and changed to private
* \[[COLDBOX-704](https://ortussolutions.atlassian.net/browse/COLDBOX-704)\] - viewModule logic was not working at all yet again.

### Improvement

* \[[COLDBOX-705](https://ortussolutions.atlassian.net/browse/COLDBOX-705)\] - Remove setting throwOnInvalidInterceptionStates, makes no sense anymore
* \[[COLDBOX-706](https://ortussolutions.atlassian.net/browse/COLDBOX-706)\] - Moved order of event manager states the injector provides to a ColdBox app so the binder can listen on itself

## What's New With 5.1.0

ColdBox 5.1.0 is a minor version update with lots of fixes, improvements and some new features. Below are the major areas of improvement and the full release notes. To update your ColdBox installation just leverage CommandBox: `update coldbox`

### Event Caching Improvements

The event caching cleanup and clearing methods did not work when using granular query strings. This has now been resolved and optimized.

### New Auto-Deserialization of JSON Payloads

If you are working with any modern JavaScript framework, this feature is for you. ColdBox on any incoming request will inspect the HTTP Body content and if the payload is JSON, it will deserialize it for you and if it is a structure/JS object, it will append itself to the request collection for you. So if we have the following incoming payload:

```javascript
{
    "name" : "Jon Clausen",
    "type" : "awesomeness",
    "data" : [ 1,2,3 ]
}
```

The request collection will have 3 keys for **name**, **type** and **data** according to their native CFML type.

### Flash Scope getAll\(\)

The flash scope needed a way to get all of its name-value pair elements in one shot, you can now with the `getAll()` method.

### Complete Rewrite of the HTML Helper

The HTML helper has been completely rewritten in 5.1 into script notation, optimized for performance and security. **All HTML output is now XSS encoded for attributes and tag content.**

### View and Directory Helper Combo

You can now declare a view and directory helper and ColdBox will use them both instead of always picking the view helper only. The order of inclusion is:

* directory helper
* view helper
* view

### ColdBox Fail Fast

This is a nice feature that will give your applications stability when doing deployments or production reinits. We have added a new application variable flag: **application.fwReinit** which is set to true when the framework is reinitializing and false when it completes. We have also added a new directive called **COLDBOX\_FAIL\_FAST** which defaults to true.

If fail fast is activated, the framework will present a nice message to users that the application is not yet available instead of holding them in a queue waiting for the reinit or application load to finish. This fail fast will release your traffic queue and produce less timeouts and failures.

The fail fast directive can also be a closure. Then we will execute your closure and you can do whatever you like within it to advice your users' about the reinit. Below you can see what happens with the fail fast code.

```javascript
// Global flag to denote if we are in mid reinit or not.
cfparam( name="application.fwReinit", default =false );

// Fail fast so users coming in during a reinit just get a please try again message.
if( application.fwReinit ){

    // Closure or UDF
    if( isClosure( variables.COLDBOX_FAIL_FAST ) || isCustomFunction( variables.COLDBOX_FAIL_FAST ) ){
        variables.COLDBOX_FAIL_FAST();
    } 
    // Core Fail Fast Option
    else if( isBoolean( variables.COLDBOX_FAIL_FAST ) && variables.COLDBOX_FAIL_FAST ){
        writeOutput( 'Oops! Seems ColdBox is still not ready to serve requests, please try again.' );
        // You don't have to return a 500, I just did this so JMeter would report it differently than a 200 
        cfheader( statusCode="503", statustext="ColdBox Not Available Yet!" );
    }

    return false;
}
```

### Release Notes

#### Bugs

* \[[COLDBOX-679](https://ortussolutions.atlassian.net/browse/COLDBOX-679)\] - viewmodule parameter not used in system.web.renderer.renderLayout
* \[[COLDBOX-680](https://ortussolutions.atlassian.net/browse/COLDBOX-680)\] - When using Resources the POST incorrectly sets action to UPDATE instead of CREATE
* \[[COLDBOX-681](https://ortussolutions.atlassian.net/browse/COLDBOX-681)\] - AbstractFlashScope fails on autoPurge property check
* \[[COLDBOX-683](https://ortussolutions.atlassian.net/browse/COLDBOX-683)\] - Event Caching Should Include Response Headers
* \[[COLDBOX-686](https://ortussolutions.atlassian.net/browse/COLDBOX-686)\] - coldbox create app template doesn't work with a servlet context other than /
* \[[COLDBOX-687](https://ortussolutions.atlassian.net/browse/COLDBOX-687)\] - Event caching broken due to not evaluating renderdata as a valid struct thanks to the EC OIL Team \(Christian,Sebastian,Didier\)

#### New Features

* \[[COLDBOX-682](https://ortussolutions.atlassian.net/browse/COLDBOX-682)\] - Add auto-deserialization of inbound JSON payloads into the RC on request capture
* \[[COLDBOX-689](https://ortussolutions.atlassian.net/browse/COLDBOX-689)\] - New flash method: getAll\(\) which retrieves a struct of all flash keys and their content
* \[[COLDBOX-693](https://ortussolutions.atlassian.net/browse/COLDBOX-693)\] - Complete rewrite of HTML Helper to Script
* \[[COLDBOX-694](https://ortussolutions.atlassian.net/browse/COLDBOX-694)\] - HTML Helper XSS Encodes all output from content to attributes by default

#### Improvements

* \[[COLDBOX-343](https://ortussolutions.atlassian.net/browse/COLDBOX-343)\] - Allow view helper AND directory helper at the same time.
* \[[COLDBOX-592](https://ortussolutions.atlassian.net/browse/COLDBOX-592)\] - Have ColdBox bootstrap advertize when Coldbox is reinitting, and have a fail fast routine
* \[[COLDBOX-678](https://ortussolutions.atlassian.net/browse/COLDBOX-678)\] - Default Flash Ram to client if session scope is disabled
* \[[COLDBOX-685](https://ortussolutions.atlassian.net/browse/COLDBOX-685)\] - Event Cache Key and Storage Enhancements to allow for granular querystring evictions
* \[[COLDBOX-690](https://ortussolutions.atlassian.net/browse/COLDBOX-690)\] - Add support for cgi.https to isSSL\(\)
* \[[COLDBOX-691](https://ortussolutions.atlassian.net/browse/COLDBOX-691)\] - Ignore AllowedMethods when using runEvent on non-default method calls

## What's New With 5.0.0

ColdBox 5.0.0 is a major release for the ColdBox MVC platform. It has been long standing as we have been learning so much especially around containerization and portability. This release has over 70 key issues addressed from new features, improvements and bug fixes. So let's begin our ColdBox 5 adventure.

### Global Versioning

All internal libraries now have a standard version according to the major ColdBox release of 5.0.0. Further releases of WireBox, CacheBox and LogBox will adhere to the unified version.

### Engine Deprecation

It is also yet another source code reduction due to the dropping of support for the following CFML Engines:

* Adobe ColdFusion 9
* Adobe ColdFusion 10

That's right, you will need Adobe ColdFusion 11+ or Lucee 4.5+ in order to work with ColdBox 5.

### Automation

We have fully automated all build processes with ColdBox 5 to include CommandBox and TestBox testing, Travis integration and a fully automated test suite that executes against **ALL** supported CFML engines. Our code coverage has increased dramatically due to this work. We discovered engine bugs that must have plagued our users for years. YAY for testing!

### Performance Improvements & Optimizations

As we update core files we keep optimizing the source code and migrating to full cfscript. This migration has allowed us to optimize very old code into modern times with significant performance gains. We have also moved from internal Java reflection to get file information to native CFML functions since now all engines support it. Just this alone has improved vanilla requests tremendously.

WireBox object creation and manipulation has also increased due to new locking strategies in our Mixer Util. You will especially see the difference when creating many transient objects.

### Lucee Full Null Support

Thanks to the community we have now full `null` support for the Lucee CFML engine and up-coming Adobe 2018.

### Core Framework Exception Handling

The core framework has been revised with a fine tooth comb to provide better exception messages, better helpful messages and also the ability to intercept exceptions at the framework level via normal exception handlers. You will also see that ColdBox can detect response headers now and make sure it can avoid caching exceptions when event caching is turned on. The appropriate status code will now be reported.

You will also find in the log files attempts to reinit the framework with invalid or missing passwords.

### Containers + Environments Support

ColdBox introduces two new methods that are available for your `ColdBox.cfc` and your `ModuleConfig.cfc` objects:

* `getSystemProperty( name, defaultValue )` - Retrieve a Java System property
* `getSystemSetting( name, defaultValue )` - Discover an environment variable either by searching system properties first and then system environment variables second.

> **Hint** These methods are also found in the ColdBox core utility object: `coldbox.system.core.util.Util` which can be injected anywhere it is needed.

These methods will allow you to interact with docker environment variables and override settings, add new settings, and much more.

### Modules, Modules and more Modules

We continue to innovate in the Hierarchical MVC \(HMVC\) design of ColdBox by fine-tuning the modular services and interactions. Here are the major updates to modules in ColdBox 5.

* All module interceptors are now namespaced to avoid name conflicts with other modules.
* New modules injected variable: `coldboxVersion` to be able to quickly detect what version of ColdBox they are running under. This will allow you to create modules that can respond to multiple ColdBox versions.

#### Inherited Entry Point

All modules have had a URL entry point since the beginning: `this.entryPoint = "/route"`. This entry point is registered in the URL mappings to allow for a unique URL pattern to exist for specific modules. That is great! However, in modern times and with the amount of API centric applications that we are building, we wanted to introduce an easier way to build resource centric APIs.

What if our resource URLs could match by convention our module inceptions? Well, with the new inherited entry points, you can do just that. By leveraging HMVC and module inception you can now create automatic URL nesting schemas.

You can turn this on just by adding the following flag in your `ModuleConfig.cfc`:

```java
this.inheritEntryPoint = true
```

When the module is registered it will recurse its parent tree and discover all parent entry points and assemble the entry point accordingly. Let's see an example. We are working on an API and we want to create the following URL resource: `/api/v1/users`. We can leverage HVMC and create the following modular structure:

```text
+ modules_app
  + api
    + modules_app
      + v1 
        + modules_app
          + users
```

This models our URL resource modularly. We can now go into each of the module's `ModuleConfig` and add only the appropriate URL pattern and turn on inherit entry point.

```text
api   - this.entryPoint = /api
v1    - this.entryPoint = /v1
users - this.entryPoint = /users
```

This feature will not only help you identify API routing, but help you build with modularity in mind. You will also find a new method in the request context called `getModuleEntryPoint()` to retrieve a modules inherited entry point, which is great for URL building.

#### New Module Events

You can now listen to more events of the module life-cycle:

* `preModuleRegistration` - before each module is registered
* `postModuleRegistration` - after each module is registered
* `afterModuleRegistrations` - This will fire when all modules have been registered.  This is great if you want modules to dynamically depend on each other.
* `afterModuleActivations` - This will fire when all modules have been succesfully activated. Great for caching updates, announcements, etc.

#### Modules\_app convention for inception

We have now added the `modules_app` convention for all module inceptions.

#### Default Model Export Convention

If you create a module where there is a model with the same name, then we will automatically map it in Wirebox as `@modulename`.

```java
cors = getInstance( "@cors" )
```

This is great for 1 model modules.

### Routing Enhancements

We continue to push forward in making ColdBox the best RESTFul framework for ColdFusion \(CFML\). In ColdBox 5 we have great new additions for both routing and rendering.

#### Simplified URL Rewrites

The SES interceptor now has a boolean flag to denote if rewrites are turned on/off and you will no longer set a base URL. We will automatically detect the base URLs according to multi-domain hosting. Meaning you can out of the box create multi-tenant applications with ease. We will also be adding subdomain routing in our final release.

```java
setFullRewrites( true ); // defaults to false.
```

#### Named Routes

This has been a long-time coming to ColdBox and I can't believe we had never added this before. Well, named routes are here, finally!

If you have used other frameworks in other languages, they allow you to name your routes so you can build links to them in an easier manner. We have done just the same. The `addRoute()` method accepts the `name` parameter and we have extended the request context with some new methods:

* `getCurrentRouteName()` - Gives you the name of the current route, if any
* `route()` - Allows you to build URLs according to named routes

**Define the Route**

```java
// Create a named route
addRoute(
  pattern  = "/user/:username/:page",
  name     = "user_detail"
);

// Same for modules
routes = [

  { 
    pattern  = "/detail/:username/:page",
    name     = "user_detail"
  }

];
```

**Build Links**

```text
<a href="#event.route( name="user_detail", params={ username="luis", page="1" } )#>User Details</a>
```

The `route` method signature can be seen below:

```java
/**
* Builds links to named routes with or without parameters. If the named route is not found, this method will throw an `InvalidArgumentException`.
* If you need a route from a module then append the module address: `@moduleName` in order to find the right route.
* 
* @name The name of the route
* @params The parameters of the route to replace
* @ssl Turn SSL on/off or detect it by default
* 
* @throws InvalidArgumentException
*/
string function route( required name, struct params={}, boolean ssl )
```

#### Resourceful Routes

In ColdBox 5, you can register resourceful routes to provide automatic mappings between HTTP verbs and URLs to event handlers and actions by convention. By convention, all resources map to a handler with the same name or they can be customized if needed. This allows for a standardized convention when building routed applications.

You will now have a new `resources()` method in the SES interceptor or a `resources` struct in your modules. Yes, all modules can have their own resourceful routes as well.

```java
// Creates all resources that point to a photos event handler by convention
resources( "photos" );

// Register multiple resources either as strings or arrays
resources( "photos,users,contacts" )
resources( [ "photos" , "users", "contacts" ] );

// Register multiple fluently
resources( "photos" )
    .resources( "users" )
    .resources( "contacts" );

// Creates all resources to the event handler of choice instead of convention
resources( route="photos", handler="MyPhotoHandler" );

// All resources in a module
resources( route="photos", handler="photos", module="api" );

// Resources in a ModuleConfig
resources = [
  { resource="photos" },
  { resource="users", handler="user" }
];
```

This single resource declaration will create all the necessary variations of URL patterns and HTTP Verbs to actions to handle the resource. It will even create named routes for you.

For in-depth usage of the `resources()` method, let's investigate the API Signature:

```java
/**
* Create all RESTful routes for a resource. It will provide automagic mappings between HTTP verbs and URLs to event handlers and actions.
* By convention, the name of the resource maps to the name of the event handler.
* Example: `resource = photos` Then we will create the following routes:
* - `/photos` : `GET` -> `photos.index` Display a list of photos
* - `/photos/new` : `GET` -> `photos.new` Returns an HTML form for creating a new photo
* - `/photos` : `POST` -> `photos.create` Create a new photo
* - `/photos/:id` : `GET` -> `photos.show` Display a specific photo
* - `/photos/:id/edit` : `GET` -> `photos.edit` Return an HTML form for editing a photo
* - `/photos/:id` : `POST/PUT/PATCH` -> `photos.update` Update a specific photo
* - `/photos/:id` : `DELETE` -> `photos.delete` Delete a specific photo
* 
* @resource         The name of a single resource or a list of resources or an array of resources
* @handler         The handler for the route. Defaults to the resource name.
* @parameterName     The name of the id/parameter for the resource. Defaults to `id`.
* @only             Limit routes created with only this list or array of actions, e.g. "index,show"
* @except             Exclude routes with an except list or array of actions, e.g. "show"
* @module             If passed, the module these resources will be attached to.
* @namespace         If passed, the namespace these resources will be attached to.
*/
function resources(
  required resource,
  handler=arguments.resource,
  parameterName="id",
  only=[],
  except=[],
  string module="",
  string namespace=""
)
```

### Event Execution

We have also done several updates for event executions, event caching and overall MVC operations:

* You can now return the `event` object from event handlers and the framework will not fail.  It will be ignored.
* `setNextEvent()` is now deprecated in favor of a `relocate()` method.
* Request context has a `getOnly( keys, private=false )` method to allow for retrieval of certain keys only from the public or private request contexts. Great for functional programming.

#### Event Caching Enhancements

**Dynamic Cache Suffix**

You can now leverage the cache suffix property in handlers to be declared as a closure so it can be evaluated at runtime instead of a static prefix.

```java
this.EVENT_CACHE_SUFFIX = function( eventHandlerBean ){
  return "a localized string, etc";
};
```

With this ability you can enable dynamic cache suffixes according to runtime environments on a per user basis.

**Cache Provider Annotations**

You can now add a `cacheProvider` annotation to your cache enabled functions and decide to which CacheBox provider the output will be cached too instead of the default provider of `template`:

```java
function index( event, rc, prc ) cache=true cacheProvider=couchbase{

}
```

### Rendering Enhancements

We have also done tremendous updates to the rendering engines in ColdBox 5, especially for RESTFul responses and content negotiation.

* ColdBox now detects the `rc.format` not only to incoming URL extensions, but also via the `Accept` Header as content-negotiation.
* New interception point `afterRenderInit` which will allow you to add your own injections to the main ColdBox renderer and modify the renderer at runtime.
* The request context can now deliver files to users via our `sendFile()` method.

#### Native JSON Responses + Handler Auto-Marshalling

JSON is the native response now for event handlers that return complex variables.

```java
function data( event, rc, prc ){
  return [1,2,3];
}

function data( event, rc, prc ){
  return myservice.getQuery();
}
```

That's it! If you return a complex variable from an event handler, ColdBox will convert it to JSON for you automatically. We will even inspect the return object and if it has a `$renderdata()` method, we will call it for you and return your very own marshalled data! But there's more. You can add a `renderData` annotation to your action and add any valid `renderdata()` type and it will return it accordingly.

```java
function data( event, rc, prc ) renderdata="xml"{
  return [1,2,3];
}

function data( event, rc, prc ) renderdata="pdf"{
  event.setView( "users/index" );
}
```

You can even add a default response type by adding the `renderdata` annotation to the `component` construct:

```java
component renderdata="json"{

}
```

#### Named Regions

We have introduced the concept of named rendering regions with ColdBox 5. As we increase the reuse of views and create many self-sufficient views or widgets, we end up with `setView()` or `renderView()` method calls that expose too much logic, location, parameters, etc. With named regions, we can actually abstract or describe how a region should be rendered and then render it when we need it via a name.

**Definition**

```java
event.setView(
  view = "users/detail",
  args = { data = userData, border = false },
  layout = "main",
  name = "userDetail"
);

event.setView(
  view = "users/banner",
  args = { data = userData },
  name = "userBanner"
);
```

**Rendering**

```text
<div id="banner">#renderView( name="userBanner" )#</div>

<div id="detail">#renderView( name="userDetail" )#</div>
```

### Testing Enhancements

Here are some of the major updates for integration testing with ColdBox and TestBox:

* Reset the response when calling `setup()` in integration testing to avoid duplicate headers within same request executions.
* Base test case doesn't allow for inherited annotations.  It now does since we moved the testing life-cycle methods to be annotation based instead of by name.
* Added dynamic methods `getRenderData()` and `getStatusCode()` helpers to the request context upon `execute()` execution.  This will allow you a shorthand approach to getting response status codes and response rendering data struct.

## Upgrading to ColdBox 5

The major compatibility issues will be covered as well as how to smoothly upgrade to this release from previous ColdBox versions. You can also check out the [What's New](https://coldbox.ortusbooks.com/intro/introduction/whats-new-with-5.0.0) guide to give you a full overview of the changes.

### `event.getCurrentHandler()` returns different results

In ColdBox 4, calling `getCurrentHandler()` would return the module if it existed:

```text
module:handler
```

In ColdBox 5, we correctly remove the **module** name so it can be retrieved via `getCurrentModule()` instead.

```text
handler
```

### `setNextEvent()` deprecated in favor of `relocate()`

The `setNextEvent()` method has been renamed to `relocate()` to better adjust to modern times. This deprecation will be removed in future versions of ColdBox.

### Modules AutoReload deprecated

The modules `autoReload` flag has been deprecated. This causes more headaches than anything. If you want changes reflected, reinit the framework.

### `onInvalidEvent` renamed to `invalidEventHandler`

The ColdBox construct setting `onInvalidEvent` has been renamed now to `invalidEventHandler`. This will be removed in future versions of ColdBox.

### `setAutoReload()` Removed

The `setAutoReload()` flag in the SES interceptor has been removed. It is evil I tell you. If you want your routing to be re-read, then reinit the framework.

### BuildLink LinkTo Argument Deprecated

The `buildLink()` method had the argument `linkTo` to denote the event or route to link to. This was verbose, so we shortened it to `to`:

```text
<!-- Before -->
<a href="#event.buildLink( linkTo="/about/us" )#">About Us</a>

<!-- After -->
<a href="#event.buildLink( to="/about/us" )#">About Us</a>
```

### Railo Support Dropped

Railo support dropped. Any classes that started with the word `Railo` need to be changed to `Lucee` especially on the CacheBox providers.

### ColdFusion 9-10 Support Dropped

ColdFusion 9-10 support has been dropped. Adobe doesn't support them anymore, so neither do we.

### Datasources Configuration Dropped

The `datasources` configuration setting directive has been dropped in favor of just leveraging the `settings` directive. Just move your datasource metadata into the `settings` struct and reference it using the settings DSL.

**Previous**

```javascript
// Datasource definitions
datasources = {
    mydsn = {
        type = "mysql",
        username = "luis",
        name = "mydsn"
    }
}
```

```text
// Injections
property name="dsn" inject="coldbox:datasource:mydsn"
```

**New**

```javascript
// Settings
settings = {
    mydsn = {
        type = "mysql",
        username = "luis",
        name = "mydsn"
    }
}
```

```text
// Injections

property name="dsn" inject="coldbox:setting:mydsn"
```

### DSL Builder Interface Update

The WireBox interface: `coldbox.system.ioc.dsl.IDSLBuilder` has changed in ColdBox 5, so if you are implementing your own DSLs, then you must update it like so:

{% code title="Old DSL Builder, notice the output attribute" %}
```javascript
public any function init( required any injector ) output="false"{ 
public any function process( required definition, targetObject ) output="false"{
```
{% endcode %}

In ColdBox 5 we removed the output attribute:

{% code title="New DSL Builder" %}
```javascript
public any function init( required any injector ) { 
public any function process( required definition, targetObject ) {
```
{% endcode %}

{% hint style="info" %}
Ticket Reference: [https://ortussolutions.atlassian.net/browse/COLDBOX-697](https://ortussolutions.atlassian.net/browse/COLDBOX-697)
{% endhint %}

### Interceptor Update

#### appendToBuffer\(\) Dropped

The string buffer has been updated in order to become thread safe and there are several methods that have been deprecated. Instead of using `appendToBuffer()` you need to use the `buffer` argument.

**Previous**

```javascript
appendToBuffer( generatedHTML );
```

**New**

```javascript
buffer.append( generatedHTML );
```

{% hint style="info" %}
Ticket Reference: [https://ortussolutions.atlassian.net/browse/COLDBOX-654](https://ortussolutions.atlassian.net/browse/COLDBOX-654)
{% endhint %}

## About This Book

The source code for this book is hosted in GitHub: [https://github.com/ortus-docs/coldbox-docs](https://github.com/ortus-docs/coldbox-docs). You can freely contribute to it and submit pull requests. The contents of this book is copyright by [Ortus Solutions, Corp](https://www.ortussolutions.com) and cannot be altered or reproduced without author's consent. All content is provided _"As-Is"_ and can be freely distributed.

* The majority of code examples in this book are done in `cfscript`.
* The majority of code generation and running of examples are done via **CommandBox**: The ColdFusion \(CFML\) CLI, Package Manager, REPL - [http://www.ortussolutions.com/products/commandbox](http://www.ortussolutions.com/products/commandbox)
* All ColdFusion examples designed to run on the open soure Lucee Platform or Adobe ColdFusion 11+

### External Trademarks & Copyrights

Flash, Flex, ColdFusion, and Adobe are registered trademarks and copyrights of Adobe Systems, Inc.

### Notice of Liability

The information in this book is distributed “as is”, without warranty. The author and Ortus Solutions, Corp shall not have any liability to any person or entity with respect to loss or damage caused or alleged to be caused directly or indirectly by the content of this training book, software and resources described in it.

### Contributing

We highly encourage contribution to this book and our open source software. The source code for this book can be found in our [GitHub repository](https://github.com/ortus-docs/coldbox-docs) where you can submit pull requests.

### Charitable Proceeds

10% of the proceeds of this book will go to charity to support orphaned kids in El Salvador - [https://www.harvesting.org/](https://www.harvesting.org/). So please donate and purchase the printed version of this book, every book sold can help a child for almost 2 months.

#### Shalom Children's Home

Shalom Children’s Home \([https://www.harvesting.org/](https://www.harvesting.org/)\) is one of the ministries that is dear to our hearts located in El Salvador. During the 12 year civil war that ended in 1990, many children were left orphaned or abandoned by parents who fled El Salvador. The Benners saw the need to help these children and received 13 children in 1982. Little by little, more children came on their own, churches and the government brought children to them for care, and the Shalom Children’s Home was founded.

Shalom now cares for over 80 children in El Salvador, from newborns to 18 years old. They receive shelter, clothing, food, medical care, education and life skills training in a Christian environment. The home is supported by a child sponsorship program.

We have personally supported Shalom for over 6 years now; it is a place of blessing for many children in El Salvador that either have no families or have been abandoned. This is good earth to seed and plant.

## Author

### Luis Fernando Majano Lainez

Luis Majano is a Computer Engineer with over 15 years of software development and systems architecture experience. He was born in [San Salvador, El Salvador](http://en.wikipedia.org/wiki/El_Salvador) in the late 70’s, during a period of economical instability and civil war. He lived in El Salvador until 1995 and then moved to Miami, Florida where he completed his Bachelors of Science in Computer Engineering at [Florida International University](http://fiu.edu). Luis resides in Rancho Cucamonga, California with his beautiful wife Veronica, baby girl Alexia and baby boy Lucas!

He is the CEO of [Ortus Solutions](http://www.ortussolutions.com), a consulting firm specializing in web development, ColdFusion \(CFML\), Java development and all open source professional services under the ColdBox and ContentBox stack. He is the creator of ColdBox, ContentBox, WireBox, MockBox, LogBox and anything “BOX”, and contributes to many open source ColdFusion projects. He is also the Adobe ColdFusion user group manager for the [Inland Empire](http://www.iecfug.org). You can read his blog at [www.luismajano.com](http://www.luismajano.com)

Luis has a passion for Jesus, tennis, golf, volleyball and anything electronic. Random Author Facts:

* He played volleyball in the Salvadorean National Team at the tender age of 17
* The Lord of the Rings and The Hobbit is something he reads every 5 years. \(Geek!\)
* His first ever computer was a Texas Instrument TI-86 that his parents gave him in 1986. After some time digesting his very first BASIC book, he had written his own tic-tac-toe game at the age of 9. \(Extra geek!\)
* He has a geek love for circuits, microcontrollers and overall embedded systems.
* He has of late \(during old age\) become a fan of running and bike riding with his family.

> Keep Jesus number one in your life and in your heart. I did and it changed my life from desolation, defeat and failure to an abundant life full of love, thankfulness, joy and overwhelming peace. As this world breathes failure and fear upon any life, Jesus brings power, love and a sound mind to everybody!
>
> “Trust in the LORD with all your heart, and do not lean on your own understanding.” – Proverbs 3:5

### Contributors

#### Jorge Emilio Reyes Bendeck

Jorge is an Industrial and Systems Engineer born in El Salvador. After finishing his Bachelor studies at the Monterrey Institute of Technology and Higher Education [ITESM](http://www.itesm.mx/wps/wcm/connect/ITESM/Tecnologico+de+Monterrey/English), Mexico, he went back to his home country where he worked as the COO of[ Industrias Bendek S.A.](http://www.si-ham.com/). In 2012 he left El Salvador and moved to Switzerland in pursuit of the love of his life. He married her and today he resides in Basel with his lovely wife Marta and their daughter Sofía.

Jorge started working as project manager and business developer at Ortus Solutions, Corp. in 2013, . At Ortus he fell in love with software development and now enjoys taking part on software development projects and software documentation! He is a fellow Christian who loves to play the guitar, worship and rejoice in the Lord!

> Therefore, if anyone is in Christ, the new creation has come: The old has gone, the new is here!  
> 2 Corinthians 5:17

#### Brad Wood

Brad grew up in southern Missouri where he systematically disassembled every toy he ever owned which occasionally led to unintentional shock therapy \(TVs hold charge long after they've been unplugged, you know\) After high school he majored in Computer Science with a music minor at [MidAmerica Nazarene University](http://www.mnu.edu) \(Olathe, KS\). Today he lives in Kansas City with his wife and three girls where he still disassembles most of his belongings \(including automobiles\) just with a slightly higher success rate of putting them back together again.\) Brad enjoys church, all sorts of international food, and the great outdoors.

Brad has been programming CFML for 12+ years and has used every version of CF since 4.5. He first fell in love with ColdFusion as a way to easily connect a database to his website for dynamic pages. Brad blogs at \([http://www.codersrevolution.com](http://www.codersrevolution.com)\) and likes to work on solder-at-home digitial and analog circuits with his daughter as well as building projects with Arduino-based microcontrollers.

Brad's CommandBox Snake high score is 141.

## Introduction

```text
   ____      _     _ ____            
  / ___|___ | | __| | __ )  _____  __
 | |   / _ \| |/ _` |  _ \ / _ \ \/ /
 | |__| (_) | | (_| | |_) | (_) >  < 
  \____\___/|_|\__,_|____/ \___/_/\_\
```

### ColdBox Manual - v5.0.0

ColdBox is a [conventions-based](https://en.wikipedia.org/wiki/Convention_over_configuration) [HMVC](https://en.wikipedia.org/wiki/Model–view–controller) development framework for [ColdFusion](http://en.wikipedia.org/wiki/Adobe_ColdFusion) \([CFML](https://en.wikipedia.org/wiki/ColdFusion_Markup_Language)\). It provides a set of reusable code and tools that can be used to increase your development productivity as well as a development standard for working in team environments.

ColdBox is natively based on [modular architecture](https://en.wikipedia.org/wiki/Modular_design) which helps address most infrastructure concerns of typical web applications and thus called an HMVC framework.

### Versioning

ColdBox is maintained under the [Semantic Versioning](http://semver.org) guidelines as much as possible.Releases will be numbered with the following format:

```text
<major>.<minor>.<patch>
```

And constructed with the following guidelines:

* Breaking backward compatibility bumps the major \(and resets the minor and patch\)
* New additions without breaking backward compatibility bumps the minor \(and resets the patch\)
* Bug fixes and misc changes bumps the patch

### License

The ColdBox Platform is open source and licensed under the [Apache 2](https://www.apache.org/licenses/LICENSE-2.0.html) License. If you use ColdBox, please try to make mention of it in your code or web site or add a Powered By Coldbox icon.

* Copyright by Ortus Solutions, Corp
* ColdBox is a registered trademark by Ortus Solutions, Corp

> **Info**: The ColdBox Websites, Documentation, logo and content have a separate license and they are a separate entity.

### Discussion & Help

The ColdBox help and discussion group can be found here: [https://groups.google.com/forum/\#!forum/coldbox](https://groups.google.com/forum/#!forum/coldbox)

### Reporting a Bug

We all make mistakes from time to time :\) So why not let us know about it and help us out. We also love pull requests, so please star us and fork us: [https://github.com/coldbox/coldbox-platform](https://github.com/coldbox/coldbox-platform)

* By Email: [bugs@coldbox.org](mailto:bugs@coldbox.org)
* By Jira: [https://ortussolutions.atlassian.net/browse/COLDBOX](https://ortussolutions.atlassian.net/browse/COLDBOX)

### Professional Open Source

ColdBox is a professional open source software backed by [Ortus Solutions, Corp](https://www.ortussolutions.com/services) offering services like:

* Custom Development
* Professional Support & Mentoring
* Training
* Server Tuning
* Security Hardening
* Code Reviews
* [Much More](http://www.ortussolutions.com/services)

### Resources

* Official Site: [https://www.coldbox.org](https://www.coldbox.org)
* Source Code: [https://github.com/coldbox/coldbox-platform](https://github.com/coldbox/coldbox-platform)
* Bug Tracker: [https://ortussolutions.atlassian.net/browse/COLDBOX](https://ortussolutions.atlassian.net/browse/COLDBOX)
* Twitter: [@coldbox](http://www.twitter.com/coldbox)
* Facebook: [https://www.facebook.com/coldboxplatform](https://www.facebook.com/coldboxplatform)
* Google+: [https://www.google.com/+ColdboxOrg](https://www.google.com/+ColdboxOrg)
* Vimeo Channel: [https://vimeo.com/channels/coldbox](https://vimeo.com/channels/coldbox)

#### HONOR GOES TO GOD ABOVE ALL

Because of His grace, this project exists. If you don't like this, then don't read it, it's not for you.

> "Therefore being justified by **faith**, we have peace with God through our Lord Jesus Christ: By whom also we have access by **faith** into this **grace** wherein we stand, and rejoice in hope of the glory of God." Romans 5:5

## What's New With 5.6.0

ColdBox 5.6.0 is a minor version update with lots of fixes, improvements, performance enhancements and some nice new features. Below are the major areas of improvement and the full release notes. To update ColdBox or any of the standalone libraries just leverage CommandBox:

* `update coldbox`
* `update logbox`
* `update wirebox`
* `update cachebox`

### Major Updates

#### Performance

We had two specific tickets that have resulted in extreme performance improvements for ALL ColdBox requests. You will feel and see the difference:

* \[[COLDBOX-799](https://ortussolutions.atlassian.net/browse/COLDBOX-799)\] - Event Handler Bean: Single instance per handler action for major performance improvements

This ticket was contributed by Dom Watson \([https://twitter.com/dom\_watson](https://twitter.com/dom_watson)\) one of the lead engineers of the amazing PresideCMS project built on top of ColdBox. We worked together to avoid the creation of handler beans on each runnable event. We now cache each event handler bean representation which results in an extreme boost in performance. Thanks Dom!

* \[[COLDBOX-810](https://ortussolutions.atlassian.net/browse/COLDBOX-810)\] - Remove afterInstanceAutowire interceptor in handlerService as afterHandlerCreation is now officially removed.

Thanks to our local mad scientist Brad Wood, he reported that the handler services still listened to ALL CFC creations in an application in order to relay an `afterHandlerCreation` interception point from the good 'ol 2.6 days. This has been finally removed and boom, another big boost in performance!

#### Better Bug Reports

We have enhanced the bug reporting templates to include much more information when dealing with exceptions:

* Show SQL error details on Adobe CF
* Current route, params and debug info
* Contributing module for the current routed URL

#### Merging of HTTP Verbs

Thanks to our very own Eric Peterson, you can now merge HTTP verbs on the same route pattern, which you could not do before:

```java
router
    .post( "photos/", "photos.create" )
    .get( "photos/", "photos.index" )
    .delete( "photos/", "photos.remove" );
```

### ColdBox Core  Release Notes

#### Bugs

* \[[COLDBOX-778](https://ortussolutions.atlassian.net/browse/COLDBOX-778)\] - ModuleService to add default route doesn't work correctly
* \[[COLDBOX-794](https://ortussolutions.atlassian.net/browse/COLDBOX-794)\] - Fix default bug report to show SQL error detail for adobe SQL exceptions
* \[[COLDBOX-796](https://ortussolutions.atlassian.net/browse/COLDBOX-796)\] - When doing package resolving if you are in a module it still tries to resolve a module
* \[[COLDBOX-806](https://ortussolutions.atlassian.net/browse/COLDBOX-806)\] - Error in HTML helper WRAPPERATTRS doesn't exist in argument scope
* \[[COLDBOX-811](https://ortussolutions.atlassian.net/browse/COLDBOX-811)\] - Include the colon for non 80 or 443 port numbers \#419 in github

#### New Features

* \[[COLDBOX-812](https://ortussolutions.atlassian.net/browse/COLDBOX-812)\] - Allow merging of HTTP verbs when doing separate verbs for the same route
* \[[COLDBOX-813](https://ortussolutions.atlassian.net/browse/COLDBOX-813)\] - Update cfconfig to use env variables instead of inline mixins, modernizeOrDie

#### Improvements

* \[[COLDBOX-795](https://ortussolutions.atlassian.net/browse/COLDBOX-795)\] - Add more current route information to the BugReport.cfm template
* \[[COLDBOX-797](https://ortussolutions.atlassian.net/browse/COLDBOX-797)\] - Ability for bug reports and app to know which module contributed the incoming URL route.
* \[[COLDBOX-798](https://ortussolutions.atlassian.net/browse/COLDBOX-798)\] - Use of .keyExists\(\) can needlessly use memory in requests, suggest StructKeyExists\(\) instead
* \[[COLDBOX-799](https://ortussolutions.atlassian.net/browse/COLDBOX-799)\] - Event Handler Bean: Single instance per handler action for major performance improvements
* \[[COLDBOX-800](https://ortussolutions.atlassian.net/browse/COLDBOX-800)\] - HandlerService.cfc$newHandler\(\): declares variables that are never used
* \[[COLDBOX-810](https://ortussolutions.atlassian.net/browse/COLDBOX-810)\] - Remove afterInstanceAutowire interceptor in handlerService as afterHandlerCreation is now officially removed.

### CacheBox Release Notes

#### Bugs

* \[[CACHEBOX-56](https://ortussolutions.atlassian.net/browse/CACHEBOX-56)\] - AbstractCacheProvider.getOrSet\(\): local var unscoped when checking if null

## What's New With 5.5.0

ColdBox 5.5.0 is a minor version update with lots of fixes, improvements, performance enhancements and some nice new features. Below are the major areas of improvement and the full release notes. To update your ColdBox installation just leverage CommandBox:

* `update coldbox`
* `update logbox`
* `update wirebox`
* `update cachebox`

The major libraries updated in this release are ColdBox MVC and WireBox.

### Compatibility Notes

If you are using annotations for component **aliases** you will have to tell WireBox explicitly to process those mappings. As by default, we no longer process mappings on startup.

```javascript
// Process a single mapping immediately
map( name ).process();

// Process all mappings in the mapDiretory() call
mapDirectory( packagePath="my.path", process=true )
mapDirectory( "my.path" ).process();

// Map all models in a module via the this.autoProcessModels in the ModuleConfig.cfc
this.autoProcessModels = true
```

### Major Updates

#### Performance

This release is a big performance boost for two areas of operation: **modules, and models**. The Module Service has been completely revised to make it as fast as possible when registering and activating modules. If you have an extensive usage of modules, you will feel the difference when booting up or re-initing the framework.

The other area is the inspection of models that has been lazy evaluated. This allows for faster bootups and reinits as models are only inspected on demand or when explicitly marked.

#### More Environment Detection

Our environment functions have now been added to the Framework SuperType and thus all objects in the ColdBox eco-system receive it:

* `getEnv(), getSystemSetting() and getSystemProperty()`

#### Custom Resource Patterns

As resources have become more mainstream in ColdBox, we now give you the ability to choose the URL pattern you want to attach to the resource. This allows you to create a-la-carte resource patterns and also allow you to nest resources via patterns.

```javascript
resources(
  pattern="/site/:siteId/agents",
  resource="agents"
);
```

### ColdBox Release Notes

#### Bugs

* \[[COLDBOX-786](https://ortussolutions.atlassian.net/browse/COLDBOX-786)\] - HTMLHelper typo on `getSetting` call via `elixirpath()`
* \[[COLDBOX-788](https://ortussolutions.atlassian.net/browse/COLDBOX-788)\] - Private method in handler is accessible from public \( direct link \)

#### New Features

* \[[COLDBOX-783](https://ortussolutions.atlassian.net/browse/COLDBOX-783)\] - New module directive: `autoProcessModels` which defaults to **false** to defer to lazy processing of models
* \[[COLDBOX-789](https://ortussolutions.atlassian.net/browse/COLDBOX-789)\] - Added `getEnv(), getSystemSetting() and getSystemProperty()` to super type for easier environment setting retrievals
* \[[COLDBOX-790](https://ortussolutions.atlassian.net/browse/COLDBOX-790)\] - Added much more logging and info when booting up the Module Service
* \[[COLDBOX-791](https://ortussolutions.atlassian.net/browse/COLDBOX-791)\] - `buildLink(), relocate()` now will translate raw `: to / in URL` with appropriate module entry points thanks to richard herbert
* \[[COLDBOX-792](https://ortussolutions.atlassian.net/browse/COLDBOX-792)\] - Allow nested resources and the ability for custom URL patterns for resources

#### Improvements

* \[[COLDBOX-782](https://ortussolutions.atlassian.net/browse/COLDBOX-782)\] - Add TestBox 3 and code coverage to the core
* \[[COLDBOX-785](https://ortussolutions.atlassian.net/browse/COLDBOX-785)\] - Module service performance optimizations for module activations.
* \[[COLDBOX-787](https://ortussolutions.atlassian.net/browse/COLDBOX-787)\] - Update RequestContext.cfc `getFullUrl()` to include port number

### WireBox Release Notes

#### New Features

* \[[WIREBOX-84](https://ortussolutions.atlassian.net/browse/WIREBOX-84)\] - Remove auto processing of all mappings defer to lazy loading
* \[[WIREBOX-85](https://ortussolutions.atlassian.net/browse/WIREBOX-85)\] - `MapDirectory` new boolean argument `process` which defers to **false**, if **true**, the mappings will be auto processed
* \[[WIREBOX-86](https://ortussolutions.atlassian.net/browse/WIREBOX-86)\] - New binder method: `process()` if chained from a mapping, it will process the mapping's metadata automatically.

#### Improvement

* \[[WIREBOX-87](https://ortussolutions.atlassian.net/browse/WIREBOX-87)\] - AOP debug logging as serialized CFCs which clogs log files

## What's New With 5.4.0

ColdBox 5.4.0 is a minor version update with lots of fixes, improvements, performance enhancements and some nice new features. Below are the major areas of improvement and the full release notes. To update your ColdBox installation just leverage CommandBox:

* `update coldbox`
* `update logbox`
* `update wirebox`
* `update cachebox`

### Box Namespace

In our initiative to make all Modules sharable between ColdBox, CommandBox and whatever other boxes we make in the future. We created the `box` alias for injections. In our case any injection with the prefix `coldbox` can be used as `box`

### RunRoute\(\)

We have created a new internal runner called `runRoute()` which is similar to `runEvent()` but it allows you to abstract your events by leveraging named routes. So just like you can create links based on named routes and params, you can execute named routes and params as well internally via `runRoute()`

```javascript
/**
 * Executes internal named routes with or without parameters. If the named route is not found or the route has no event to execute then this method will throw an `InvalidArgumentException`.
 * If you need a route from a module then append the module address: `@moduleName` or prefix it like in run event calls `moduleName:routeName` in order to find the right route.
 * The route params will be passed to events as action arguments much how eventArguments work.
 *
 * @name The name of the route
 * @params The parameters of the route to replace
 * @cache Cached the output of the runnable execution, defaults to false. A unique key will be created according to event string + arguments.
 * @cacheTimeout The time in minutes to cache the results
 * @cacheLastAccessTimeout The time in minutes the results will be removed from cache if idle or requested
 * @cacheSuffix The suffix to add into the cache entry for this event rendering
 * @cacheProvider The provider to cache this event rendering in, defaults to 'template'
 * @prePostExempt If true, pre/post handlers will not be fired. Defaults to false
 *
 * @throws InvalidArgumentException
 */
any function runRoute(
    required name,
    struct params={},
    boolean cache=false,
    cacheTimeout="",
    cacheLastAccessTimeout="",
    cacheSuffix="",
    cacheProvider="template",
    boolean prePostExempt=false
)
```

### CacheBox Rewritten

This should have been a major release on its own. The entire CacheBox framework was re-written in script and modernized from top to bottom. We removed all implicit variable access which gave us huge performance boosts and we streamlined all operations with modern techniques. The results are great and our maintenance will be much less in the future. A part from those optimizations we managed to add a few nice items:

* Adobe 2018 certified
* New setting `resetTimeoutOnAccess` which allows you to simulate session scopes on any CacheBox cache.  Every time a `get()` operation is done, that item's timeout will be reset.
* All cache providers get some multi function goodness: `setMulti(), getMulti(), lookupMulti(), clearMulti(),getCachedObjectMetadataMulti()`

#### LogBox Improvements

There are two major improvements we did with LogBox in this release:

1\) The file locking operations on file appenders have been streamlined to avoid high i/o operations.

2\) The console appender uses an asynchronous streaming technique which makes it extremely efficient and fast.

### ColdBox Release Notes

#### Bugs

* \[[COLDBOX-556](https://ortussolutions.atlassian.net/browse/COLDBOX-556)\] - `prePostExempt` doesn't skip around advices
* \[[COLDBOX-755](https://ortussolutions.atlassian.net/browse/COLDBOX-755)\] - Core interceptors in coldbox.cfc do not listen or register custom interception points that are contributed by modules
* \[[COLDBOX-761](https://ortussolutions.atlassian.net/browse/COLDBOX-761)\] - `invalidEventHandler` gets in to an infinite loop when the `invalidEventHandler` isn't a full event
* \[[COLDBOX-766](https://ortussolutions.atlassian.net/browse/COLDBOX-766)\] - ColdBox shutdown errors `onApplicationEnd` due to lack of application scope
* \[[COLDBOX-770](https://ortussolutions.atlassian.net/browse/COLDBOX-770)\] - Use of `event.sendFile` delivers a file with single quotes in the name
* \[[COLDBOX-773](https://ortussolutions.atlassian.net/browse/COLDBOX-773)\] - Remove default defaultValue as it never will throw an exception if missing on requestcontext on `getHTTPHeader()`

#### New Features

* \[[COLDBOX-765](https://ortussolutions.atlassian.net/browse/COLDBOX-765)\] - Update all elixir methods to match new version
* \[[COLDBOX-771](https://ortussolutions.atlassian.net/browse/COLDBOX-771)\] - Add Elixir version 3 path methods
* \[[COLDBOX-775](https://ortussolutions.atlassian.net/browse/COLDBOX-775)\] - Added `:` as a delimiter for the `route()` method when using modules to be consistent with run event
* \[[COLDBOX-776](https://ortussolutions.atlassian.net/browse/COLDBOX-776)\] - new runner: `runRoute()` that allows you to run routes internally with param passing

#### Improvements

* \[[COLDBOX-767](https://ortussolutions.atlassian.net/browse/COLDBOX-767)\] - Change "module already registered" from warn to debug
* \[[COLDBOX-774](https://ortussolutions.atlassian.net/browse/COLDBOX-774)\] - Introduce generic "**box**" namespace for Wirebox injections

### CacheBox Release Notes

#### Bugs

* \[[CACHEBOX-46](https://ortussolutions.atlassian.net/browse/CACHEBOX-46)\] - CacheboxProvider metadata and stores: use CFML functions on java hash maps breaks concurrency
* \[[CACHEBOX-50](https://ortussolutions.atlassian.net/browse/CACHEBOX-50)\] - getOrSet\(\) provider method doesn't work with full null support
* \[[CACHEBOX-52](https://ortussolutions.atlassian.net/browse/CACHEBOX-52)\] - getQuiet\(\), clearQuiet\(\), getSize\(\), clearAll\(\), expireAll\(\) broken in acf providers

#### New Features

* \[[CACHEBOX-48](https://ortussolutions.atlassian.net/browse/CACHEBOX-48)\] - New setting: \`resetTimeoutOnAccess\` to allow the ability to reset timeout access for entries
* \[[CACHEBOX-49](https://ortussolutions.atlassian.net/browse/CACHEBOX-49)\] - Global script conversion and code optimizations
* \[[CACHEBOX-53](https://ortussolutions.atlassian.net/browse/CACHEBOX-53)\] - lookup operations on ACF providers updated to leverage cacheIdExists\(\) improves operation by over 50x
* \[[CACHEBOX-54](https://ortussolutions.atlassian.net/browse/CACHEBOX-54)\] - setMulti\(\), getMulti\(\), lookupMulti\(\), clearMulti\(\),getCachedObjectMetadataMulti\(\) is now available on all cache providers

#### Improvements

* \[[CACHEBOX-47](https://ortussolutions.atlassian.net/browse/CACHEBOX-47)\] - Increased timeout on \`getOrSet\` lock
* \[[CACHEBOX-51](https://ortussolutions.atlassian.net/browse/CACHEBOX-51)\] - Consolidated usages of the abstract cache provider to all providers to avoid redundancy

### WireBox Release Notes

#### Bugs

* \[[WIREBOX-82](https://ortussolutions.atlassian.net/browse/WIREBOX-82)\] - `builder.toVirtualInheritance()`: scoping issues
* \[[WIREBOX-83](https://ortussolutions.atlassian.net/browse/WIREBOX-83)\] - When using sandbox security, and using a provider DSL the file existence checks blow up

### LogBox Release Notes

#### New Features

* \[[LOGBOX-34](https://ortussolutions.atlassian.net/browse/LOGBOX-34)\] - Console appender completely rewritten to support asynchronous streaming

#### Improvements

* \[[LOGBOX-33](https://ortussolutions.atlassian.net/browse/LOGBOX-33)\] - Improve file exists usage on file appenders to avoid i/o operations

## What's New With 5.3.0

ColdBox 5.3.0 is a minor version update with lots of fixes, improvements, performance enhancements and some nice new features. Below are the major areas of improvement and the full release notes. To update your ColdBox installation just leverage CommandBox: `update coldbox`

### ColdBox

#### Bugs

* \[[COLDBOX-741](https://ortussolutions.atlassian.net/browse/COLDBOX-741)\] - Defining UDF as **COLDBOX\_FAIL\_FAST** no longer returns false
* \[[COLDBOX-750](https://ortussolutions.atlassian.net/browse/COLDBOX-750)\] - Fixed typo\(s\) in call of function prePend\(\) on routing
* \[[COLDBOX-752](https://ortussolutions.atlassian.net/browse/COLDBOX-752)\] - base url not eliminating double slashes
* \[[COLDBOX-756](https://ortussolutions.atlassian.net/browse/COLDBOX-756)\] - Setter for valid extensions is setting the wrong variable
* \[[COLDBOX-759](https://ortussolutions.atlassian.net/browse/COLDBOX-759)\] - Cleanup the with closure on group routing to avoid leakage

#### New Features

* \[[COLDBOX-753](https://ortussolutions.atlassian.net/browse/COLDBOX-753)\] - Add new flag to router: `multiDomainDiscovery` that can be turned on/off

#### Improvements

* \[[COLDBOX-742](https://ortussolutions.atlassian.net/browse/COLDBOX-742)\] - Add trimming to asset loader to avoid spaces on links
* \[[COLDBOX-744](https://ortussolutions.atlassian.net/browse/COLDBOX-744)\] - InterceptorState + EventPool uses **syncrhonizedMap** that has locking issues: refactor to avoid these issues
* \[[COLDBOX-754](https://ortussolutions.atlassian.net/browse/COLDBOX-754)\] - set initiated bit for ColdBox after modules are activated
* \[[COLDBOX-760](https://ortussolutions.atlassian.net/browse/COLDBOX-760)\] - Performance improvements for interception registrations and discovery

### LogBox

#### Improvements

* \[[LOGBOX-32](https://ortussolutions.atlassian.net/browse/LOGBOX-32)\] - Add test and fix for adding a LogBox category after the fact

### WireBox

#### Improvements

* \[[WIREBOX-79](https://ortussolutions.atlassian.net/browse/WIREBOX-79)\] - Account for leading slashes in `mapDirectory()`
* \[[WIREBOX-80](https://ortussolutions.atlassian.net/browse/WIREBOX-80)\] - Throw a nicer DSL error if ColdBox is not linked
* \[[WIREBOX-81](https://ortussolutions.atlassian.net/browse/WIREBOX-81)\] - Performance improvements for the construction of transients

## What's New With 5.2.0

ColdBox 5.2.0 is a minor version update with lots of fixes, improvements and some new features not only to the ColdBox core but to LogBox and WireBox alike. Below are the major areas of improvement and the full release notes. To update your ColdBox installation just leverage CommandBox: `update coldbox`

### ColdBox

The major areas of improvement for the ColdBox core where on tons of fixes, however there are some nice new features discussed below.

#### Bugs

* \[[COLDBOX-597](https://ortussolutions.atlassian.net/browse/COLDBOX-597)\] - Function `addAsset` generate wrong link if asset path contains ".js"
* \[[COLDBOX-668](https://ortussolutions.atlassian.net/browse/COLDBOX-668)\] - Make implicit views case sensitive by default for linux systems
* \[[COLDBOX-677](https://ortussolutions.atlassian.net/browse/COLDBOX-677)\] - HTML HELPER - Fix a requirement for `columnName` if column is defined
* \[[COLDBOX-696](https://ortussolutions.atlassian.net/browse/COLDBOX-696)\] - Passing headers to \``request`\` breaks RoutingService when in test mode
* \[[COLDBOX-724](https://ortussolutions.atlassian.net/browse/COLDBOX-724)\] - Add `ENV` overload in `detectEnvironment()` via ENVIRONMENT system setting/property
* \[[COLDBOX-726](https://ortussolutions.atlassian.net/browse/COLDBOX-726)\] - setNextEvent does not exist in coldbox.system.web.Controller
* \[[COLDBOX-727](https://ortussolutions.atlassian.net/browse/COLDBOX-727)\] - fail fast strong typed to boolean, update to allow closures
* \[[COLDBOX-729](https://ortussolutions.atlassian.net/browse/COLDBOX-729)\] - getRenderData\(\) in base test case was not looking at the right request collection
* \[[COLDBOX-732](https://ortussolutions.atlassian.net/browse/COLDBOX-732)\] - Fail fast can't be turned off for original behavior
* \[[COLDBOX-736](https://ortussolutions.atlassian.net/browse/COLDBOX-736)\] - Module mappings disappear when not unloading ColdBox in base test case
* \[[COLDBOX-738](https://ortussolutions.atlassian.net/browse/COLDBOX-738)\] - Clear not working on string builders: Use setLength\(0\) since clear is not a method on StringBuilder
* \[[COLDBOX-740](https://ortussolutions.atlassian.net/browse/COLDBOX-740)\] - When using group\(\) operations with a handler and no explicit handler routing call added, route never registered the handler

#### New Features

* \[[COLDBOX-722](https://ortussolutions.atlassian.net/browse/COLDBOX-722)\] - New global directive: `autoMapModels` which if **true**, it will map all root models just like modules do.

This will allow root **models** to behave like modules where all models are registered automatically for you but with no namespace.

* \[[COLDBOX-737](https://ortussolutions.atlassian.net/browse/COLDBOX-737)\] - `toAction()` terminator is missing from the new router DSL

This terminator was missing from the new Routing DSL. This will allow you to build up routes that terminate at an action.

* \[[COLDBOX-720](https://ortussolutions.atlassian.net/browse/COLDBOX-720)\] - Register `config/Router.cfc` as an interceptor

The main application router and **ALL** module routers are now also interceptors.

#### Improvements

* \[[COLDBOX-730](https://ortussolutions.atlassian.net/browse/COLDBOX-730)\] - Implicitly pass args from `renderLayout()` into the rendered views
* \[[COLDBOX-739](https://ortussolutions.atlassian.net/browse/COLDBOX-739)\] - List modules which have already been processed when a module cannot be activated to help with debugging

### LogBox

#### Bugs

* \[[LOGBOX-29](https://ortussolutions.atlassian.net/browse/LOGBOX-29)\] - when using async option on FileAppender, nothing logs, well now it does!

#### New Features

* \[[LOGBOX-31](https://ortussolutions.atlassian.net/browse/LOGBOX-31)\] - Add `defaultValue` arguments to `getProperty()` methods on abstract appenders

#### Improvements

* \[[LOGBOX-30](https://ortussolutions.atlassian.net/browse/LOGBOX-30)\] - Leave off text `"ExtraInfo: "` from console appender if empty string

### WireBox

#### Bugs

* \[[WIREBOX-76](https://ortussolutions.atlassian.net/browse/WIREBOX-76)\] - Virtual inheritance not injecting generic **getters** and **setters** correctly on target objects
* \[[WIREBOX-77](https://ortussolutions.atlassian.net/browse/WIREBOX-77)\] - Virtual inheritance not inheriting `init` from super class

#### Improvements

* \[[WIREBOX-51](https://ortussolutions.atlassian.net/browse/WIREBOX-51)\] - Add method to binder to override alias of current mapping, by passing the current mapping to the influencer closure
* \[[WIREBOX-75](https://ortussolutions.atlassian.net/browse/WIREBOX-75)\] - Don't exclude path in parent mapping destinations
* \[[WIREBOX-78](https://ortussolutions.atlassian.net/browse/WIREBOX-78)\] - Simplify error message for missing dependency to be human readable

## What's New With 5.1.4

Short and sweet release!

#### Bug

* \[[COLDBOX-718](https://ortussolutions.atlassian.net/browse/COLDBOX-718)\] - Left one `encodeforhtml` in `textarea` that was missing.

## What's New With 5.1.3

This is a patch release for ColdBox

### Bugs

* \[[COLDBOX-715](https://ortussolutions.atlassian.net/browse/COLDBOX-715)\] - Elvis operator inconsistencies on Adobe Engines, please Adobe, patch the engines and fix your compiler!

### Improvements

* \[[COLDBOX-237](https://ortussolutions.atlassian.net/browse/COLDBOX-237)\] - Some HTMLHelper method still need escaping as certain values should never be HTML
* \[[COLDBOX-716](https://ortussolutions.atlassian.net/browse/COLDBOX-716)\] - determine session/client state via CF getApplicationMetadata\(\) instead of isDefined\(\) to avoid load issues for flash ram
* \[[COLDBOX-717](https://ortussolutions.atlassian.net/browse/COLDBOX-717)\] - RemotingUtil converted to cfscript \#367

## What's New With 5.1.2

This is a patch release for ColdBox

### Bugs/Regressions

* \[[COLDBOX-711](https://ortussolutions.atlassian.net/browse/COLDBOX-711)\] - HTML helper cannot add fontawesome icons in button due to XSS enabled by default
* \[[COLDBOX-712](https://ortussolutions.atlassian.net/browse/COLDBOX-712)\] - ColdBox shutdown code that uses CF mappings for modules fails on fwreinit
* \[[COLDBOX-713](https://ortussolutions.atlassian.net/browse/COLDBOX-713)\] - addAsset throw error when called from handlers

### Improvements

* \[[COLDBOX-714](https://ortussolutions.atlassian.net/browse/COLDBOX-714)\] - Too many issues when encoding by default for HTML Helper, revert to non-encoded and provide ways to encode globally and a-la-carte
* \[[COLDBOX-702](https://ortussolutions.atlassian.net/browse/COLDBOX-702)\] -  Framework setting for the automatic deserialization of JSON payloads to the RC: `jsonPayloadToRC`

### Automatic JSON Payload Setting

The automatic JSON to request collection feature defaults now to false to avoid backwards compatibility. You can easily turn it on via the setting: `coldbox.jsonPayloadToRC`

```javascript
coldbox = {
    jsonPayloadToRC = true
}
```

### HTML Helper Changes

The HTML Helper has been migrated to an internal module in this release. It allows you to configure it via the following configuration settings in your `ColdBox.cfc`.

```javascript
moduleSettings = {
    htmlHelper = {
        // Base path for loading JavaScript files
        js_path = "",
        // Base path for loading CSS files
        css_path = "",
        // Auto-XSS encode values when using the HTML Helper output methods
        encodeValues = false
    }
}
```

#### Injection Shortcut

You can also now inject the HTML helper anywhere using it's injection DSL Shortcut of `@HTMLHelper`

## What's New With 5.1.1

This is a patch release for ColdBox.

### Bug

* \[[COLDBOX-703](https://ortussolutions.atlassian.net/browse/COLDBOX-703)\] - regression onmissingmethod on html helper method was public and changed to private
* \[[COLDBOX-704](https://ortussolutions.atlassian.net/browse/COLDBOX-704)\] - viewModule logic was not working at all yet again.

### Improvement

* \[[COLDBOX-705](https://ortussolutions.atlassian.net/browse/COLDBOX-705)\] - Remove setting throwOnInvalidInterceptionStates, makes no sense anymore
* \[[COLDBOX-706](https://ortussolutions.atlassian.net/browse/COLDBOX-706)\] - Moved order of event manager states the injector provides to a ColdBox app so the binder can listen on itself

## What's New With 5.1.0

ColdBox 5.1.0 is a minor version update with lots of fixes, improvements and some new features. Below are the major areas of improvement and the full release notes. To update your ColdBox installation just leverage CommandBox: `update coldbox`

### Event Caching Improvements

The event caching cleanup and clearing methods did not work when using granular query strings. This has now been resolved and optimized.

### New Auto-Deserialization of JSON Payloads

If you are working with any modern JavaScript framework, this feature is for you. ColdBox on any incoming request will inspect the HTTP Body content and if the payload is JSON, it will deserialize it for you and if it is a structure/JS object, it will append itself to the request collection for you. So if we have the following incoming payload:

```javascript
{
    "name" : "Jon Clausen",
    "type" : "awesomeness",
    "data" : [ 1,2,3 ]
}
```

The request collection will have 3 keys for **name**, **type** and **data** according to their native CFML type.

### Flash Scope getAll\(\)

The flash scope needed a way to get all of its name-value pair elements in one shot, you can now with the `getAll()` method.

### Complete Rewrite of the HTML Helper

The HTML helper has been completely rewritten in 5.1 into script notation, optimized for performance and security. **All HTML output is now XSS encoded for attributes and tag content.**

### View and Directory Helper Combo

You can now declare a view and directory helper and ColdBox will use them both instead of always picking the view helper only. The order of inclusion is:

* directory helper
* view helper
* view

### ColdBox Fail Fast

This is a nice feature that will give your applications stability when doing deployments or production reinits. We have added a new application variable flag: **application.fwReinit** which is set to true when the framework is reinitializing and false when it completes. We have also added a new directive called **COLDBOX\_FAIL\_FAST** which defaults to true.

If fail fast is activated, the framework will present a nice message to users that the application is not yet available instead of holding them in a queue waiting for the reinit or application load to finish. This fail fast will release your traffic queue and produce less timeouts and failures.

The fail fast directive can also be a closure. Then we will execute your closure and you can do whatever you like within it to advice your users' about the reinit. Below you can see what happens with the fail fast code.

```javascript
// Global flag to denote if we are in mid reinit or not.
cfparam( name="application.fwReinit", default =false );

// Fail fast so users coming in during a reinit just get a please try again message.
if( application.fwReinit ){

    // Closure or UDF
    if( isClosure( variables.COLDBOX_FAIL_FAST ) || isCustomFunction( variables.COLDBOX_FAIL_FAST ) ){
        variables.COLDBOX_FAIL_FAST();
    } 
    // Core Fail Fast Option
    else if( isBoolean( variables.COLDBOX_FAIL_FAST ) && variables.COLDBOX_FAIL_FAST ){
        writeOutput( 'Oops! Seems ColdBox is still not ready to serve requests, please try again.' );
        // You don't have to return a 500, I just did this so JMeter would report it differently than a 200 
        cfheader( statusCode="503", statustext="ColdBox Not Available Yet!" );
    }

    return false;
}
```

### Release Notes

#### Bugs

* \[[COLDBOX-679](https://ortussolutions.atlassian.net/browse/COLDBOX-679)\] - viewmodule parameter not used in system.web.renderer.renderLayout
* \[[COLDBOX-680](https://ortussolutions.atlassian.net/browse/COLDBOX-680)\] - When using Resources the POST incorrectly sets action to UPDATE instead of CREATE
* \[[COLDBOX-681](https://ortussolutions.atlassian.net/browse/COLDBOX-681)\] - AbstractFlashScope fails on autoPurge property check
* \[[COLDBOX-683](https://ortussolutions.atlassian.net/browse/COLDBOX-683)\] - Event Caching Should Include Response Headers
* \[[COLDBOX-686](https://ortussolutions.atlassian.net/browse/COLDBOX-686)\] - coldbox create app template doesn't work with a servlet context other than /
* \[[COLDBOX-687](https://ortussolutions.atlassian.net/browse/COLDBOX-687)\] - Event caching broken due to not evaluating renderdata as a valid struct thanks to the EC OIL Team \(Christian,Sebastian,Didier\)

#### New Features

* \[[COLDBOX-682](https://ortussolutions.atlassian.net/browse/COLDBOX-682)\] - Add auto-deserialization of inbound JSON payloads into the RC on request capture
* \[[COLDBOX-689](https://ortussolutions.atlassian.net/browse/COLDBOX-689)\] - New flash method: getAll\(\) which retrieves a struct of all flash keys and their content
* \[[COLDBOX-693](https://ortussolutions.atlassian.net/browse/COLDBOX-693)\] - Complete rewrite of HTML Helper to Script
* \[[COLDBOX-694](https://ortussolutions.atlassian.net/browse/COLDBOX-694)\] - HTML Helper XSS Encodes all output from content to attributes by default

#### Improvements

* \[[COLDBOX-343](https://ortussolutions.atlassian.net/browse/COLDBOX-343)\] - Allow view helper AND directory helper at the same time.
* \[[COLDBOX-592](https://ortussolutions.atlassian.net/browse/COLDBOX-592)\] - Have ColdBox bootstrap advertize when Coldbox is reinitting, and have a fail fast routine
* \[[COLDBOX-678](https://ortussolutions.atlassian.net/browse/COLDBOX-678)\] - Default Flash Ram to client if session scope is disabled
* \[[COLDBOX-685](https://ortussolutions.atlassian.net/browse/COLDBOX-685)\] - Event Cache Key and Storage Enhancements to allow for granular querystring evictions
* \[[COLDBOX-690](https://ortussolutions.atlassian.net/browse/COLDBOX-690)\] - Add support for cgi.https to isSSL\(\)
* \[[COLDBOX-691](https://ortussolutions.atlassian.net/browse/COLDBOX-691)\] - Ignore AllowedMethods when using runEvent on non-default method calls

## What's New With 5.0.0

ColdBox 5.0.0 is a major release for the ColdBox MVC platform. It has been long standing as we have been learning so much especially around containerization and portability. This release has over 70 key issues addressed from new features, improvements and bug fixes. So let's begin our ColdBox 5 adventure.

### Global Versioning

All internal libraries now have a standard version according to the major ColdBox release of 5.0.0. Further releases of WireBox, CacheBox and LogBox will adhere to the unified version.

### Engine Deprecation

It is also yet another source code reduction due to the dropping of support for the following CFML Engines:

* Adobe ColdFusion 9
* Adobe ColdFusion 10

That's right, you will need Adobe ColdFusion 11+ or Lucee 4.5+ in order to work with ColdBox 5.

### Automation

We have fully automated all build processes with ColdBox 5 to include CommandBox and TestBox testing, Travis integration and a fully automated test suite that executes against **ALL** supported CFML engines. Our code coverage has increased dramatically due to this work. We discovered engine bugs that must have plagued our users for years. YAY for testing!

### Performance Improvements & Optimizations

As we update core files we keep optimizing the source code and migrating to full cfscript. This migration has allowed us to optimize very old code into modern times with significant performance gains. We have also moved from internal Java reflection to get file information to native CFML functions since now all engines support it. Just this alone has improved vanilla requests tremendously.

WireBox object creation and manipulation has also increased due to new locking strategies in our Mixer Util. You will especially see the difference when creating many transient objects.

### Lucee Full Null Support

Thanks to the community we have now full `null` support for the Lucee CFML engine and up-coming Adobe 2018.

### Core Framework Exception Handling

The core framework has been revised with a fine tooth comb to provide better exception messages, better helpful messages and also the ability to intercept exceptions at the framework level via normal exception handlers. You will also see that ColdBox can detect response headers now and make sure it can avoid caching exceptions when event caching is turned on. The appropriate status code will now be reported.

You will also find in the log files attempts to reinit the framework with invalid or missing passwords.

### Containers + Environments Support

ColdBox introduces two new methods that are available for your `ColdBox.cfc` and your `ModuleConfig.cfc` objects:

* `getSystemProperty( name, defaultValue )` - Retrieve a Java System property
* `getSystemSetting( name, defaultValue )` - Discover an environment variable either by searching system properties first and then system environment variables second.

> **Hint** These methods are also found in the ColdBox core utility object: `coldbox.system.core.util.Util` which can be injected anywhere it is needed.

These methods will allow you to interact with docker environment variables and override settings, add new settings, and much more.

### Modules, Modules and more Modules

We continue to innovate in the Hierarchical MVC \(HMVC\) design of ColdBox by fine-tuning the modular services and interactions. Here are the major updates to modules in ColdBox 5.

* All module interceptors are now namespaced to avoid name conflicts with other modules.
* New modules injected variable: `coldboxVersion` to be able to quickly detect what version of ColdBox they are running under. This will allow you to create modules that can respond to multiple ColdBox versions.

#### Inherited Entry Point

All modules have had a URL entry point since the beginning: `this.entryPoint = "/route"`. This entry point is registered in the URL mappings to allow for a unique URL pattern to exist for specific modules. That is great! However, in modern times and with the amount of API centric applications that we are building, we wanted to introduce an easier way to build resource centric APIs.

What if our resource URLs could match by convention our module inceptions? Well, with the new inherited entry points, you can do just that. By leveraging HMVC and module inception you can now create automatic URL nesting schemas.

You can turn this on just by adding the following flag in your `ModuleConfig.cfc`:

```java
this.inheritEntryPoint = true
```

When the module is registered it will recurse its parent tree and discover all parent entry points and assemble the entry point accordingly. Let's see an example. We are working on an API and we want to create the following URL resource: `/api/v1/users`. We can leverage HVMC and create the following modular structure:

```text
+ modules_app
  + api
    + modules_app
      + v1 
        + modules_app
          + users
```

This models our URL resource modularly. We can now go into each of the module's `ModuleConfig` and add only the appropriate URL pattern and turn on inherit entry point.

```text
api   - this.entryPoint = /api
v1    - this.entryPoint = /v1
users - this.entryPoint = /users
```

This feature will not only help you identify API routing, but help you build with modularity in mind. You will also find a new method in the request context called `getModuleEntryPoint()` to retrieve a modules inherited entry point, which is great for URL building.

#### New Module Events

You can now listen to more events of the module life-cycle:

* `preModuleRegistration` - before each module is registered
* `postModuleRegistration` - after each module is registered
* `afterModuleRegistrations` - This will fire when all modules have been registered.  This is great if you want modules to dynamically depend on each other.
* `afterModuleActivations` - This will fire when all modules have been succesfully activated. Great for caching updates, announcements, etc.

#### Modules\_app convention for inception

We have now added the `modules_app` convention for all module inceptions.

#### Default Model Export Convention

If you create a module where there is a model with the same name, then we will automatically map it in Wirebox as `@modulename`.

```java
cors = getInstance( "@cors" )
```

This is great for 1 model modules.

### Routing Enhancements

We continue to push forward in making ColdBox the best RESTFul framework for ColdFusion \(CFML\). In ColdBox 5 we have great new additions for both routing and rendering.

#### Simplified URL Rewrites

The SES interceptor now has a boolean flag to denote if rewrites are turned on/off and you will no longer set a base URL. We will automatically detect the base URLs according to multi-domain hosting. Meaning you can out of the box create multi-tenant applications with ease. We will also be adding subdomain routing in our final release.

```java
setFullRewrites( true ); // defaults to false.
```

#### Named Routes

This has been a long-time coming to ColdBox and I can't believe we had never added this before. Well, named routes are here, finally!

If you have used other frameworks in other languages, they allow you to name your routes so you can build links to them in an easier manner. We have done just the same. The `addRoute()` method accepts the `name` parameter and we have extended the request context with some new methods:

* `getCurrentRouteName()` - Gives you the name of the current route, if any
* `route()` - Allows you to build URLs according to named routes

**Define the Route**

```java
// Create a named route
addRoute(
  pattern  = "/user/:username/:page",
  name     = "user_detail"
);

// Same for modules
routes = [

  { 
    pattern  = "/detail/:username/:page",
    name     = "user_detail"
  }

];
```

**Build Links**

```text
<a href="#event.route( name="user_detail", params={ username="luis", page="1" } )#>User Details</a>
```

The `route` method signature can be seen below:

```java
/**
* Builds links to named routes with or without parameters. If the named route is not found, this method will throw an `InvalidArgumentException`.
* If you need a route from a module then append the module address: `@moduleName` in order to find the right route.
* 
* @name The name of the route
* @params The parameters of the route to replace
* @ssl Turn SSL on/off or detect it by default
* 
* @throws InvalidArgumentException
*/
string function route( required name, struct params={}, boolean ssl )
```

#### Resourceful Routes

In ColdBox 5, you can register resourceful routes to provide automatic mappings between HTTP verbs and URLs to event handlers and actions by convention. By convention, all resources map to a handler with the same name or they can be customized if needed. This allows for a standardized convention when building routed applications.

You will now have a new `resources()` method in the SES interceptor or a `resources` struct in your modules. Yes, all modules can have their own resourceful routes as well.

```java
// Creates all resources that point to a photos event handler by convention
resources( "photos" );

// Register multiple resources either as strings or arrays
resources( "photos,users,contacts" )
resources( [ "photos" , "users", "contacts" ] );

// Register multiple fluently
resources( "photos" )
    .resources( "users" )
    .resources( "contacts" );

// Creates all resources to the event handler of choice instead of convention
resources( route="photos", handler="MyPhotoHandler" );

// All resources in a module
resources( route="photos", handler="photos", module="api" );

// Resources in a ModuleConfig
resources = [
  { resource="photos" },
  { resource="users", handler="user" }
];
```

This single resource declaration will create all the necessary variations of URL patterns and HTTP Verbs to actions to handle the resource. It will even create named routes for you.

For in-depth usage of the `resources()` method, let's investigate the API Signature:

```java
/**
* Create all RESTful routes for a resource. It will provide automagic mappings between HTTP verbs and URLs to event handlers and actions.
* By convention, the name of the resource maps to the name of the event handler.
* Example: `resource = photos` Then we will create the following routes:
* - `/photos` : `GET` -> `photos.index` Display a list of photos
* - `/photos/new` : `GET` -> `photos.new` Returns an HTML form for creating a new photo
* - `/photos` : `POST` -> `photos.create` Create a new photo
* - `/photos/:id` : `GET` -> `photos.show` Display a specific photo
* - `/photos/:id/edit` : `GET` -> `photos.edit` Return an HTML form for editing a photo
* - `/photos/:id` : `POST/PUT/PATCH` -> `photos.update` Update a specific photo
* - `/photos/:id` : `DELETE` -> `photos.delete` Delete a specific photo
* 
* @resource         The name of a single resource or a list of resources or an array of resources
* @handler         The handler for the route. Defaults to the resource name.
* @parameterName     The name of the id/parameter for the resource. Defaults to `id`.
* @only             Limit routes created with only this list or array of actions, e.g. "index,show"
* @except             Exclude routes with an except list or array of actions, e.g. "show"
* @module             If passed, the module these resources will be attached to.
* @namespace         If passed, the namespace these resources will be attached to.
*/
function resources(
  required resource,
  handler=arguments.resource,
  parameterName="id",
  only=[],
  except=[],
  string module="",
  string namespace=""
)
```

### Event Execution

We have also done several updates for event executions, event caching and overall MVC operations:

* You can now return the `event` object from event handlers and the framework will not fail.  It will be ignored.
* `setNextEvent()` is now deprecated in favor of a `relocate()` method.
* Request context has a `getOnly( keys, private=false )` method to allow for retrieval of certain keys only from the public or private request contexts. Great for functional programming.

#### Event Caching Enhancements

**Dynamic Cache Suffix**

You can now leverage the cache suffix property in handlers to be declared as a closure so it can be evaluated at runtime instead of a static prefix.

```java
this.EVENT_CACHE_SUFFIX = function( eventHandlerBean ){
  return "a localized string, etc";
};
```

With this ability you can enable dynamic cache suffixes according to runtime environments on a per user basis.

**Cache Provider Annotations**

You can now add a `cacheProvider` annotation to your cache enabled functions and decide to which CacheBox provider the output will be cached too instead of the default provider of `template`:

```java
function index( event, rc, prc ) cache=true cacheProvider=couchbase{

}
```

### Rendering Enhancements

We have also done tremendous updates to the rendering engines in ColdBox 5, especially for RESTFul responses and content negotiation.

* ColdBox now detects the `rc.format` not only to incoming URL extensions, but also via the `Accept` Header as content-negotiation.
* New interception point `afterRenderInit` which will allow you to add your own injections to the main ColdBox renderer and modify the renderer at runtime.
* The request context can now deliver files to users via our `sendFile()` method.

#### Native JSON Responses + Handler Auto-Marshalling

JSON is the native response now for event handlers that return complex variables.

```java
function data( event, rc, prc ){
  return [1,2,3];
}

function data( event, rc, prc ){
  return myservice.getQuery();
}
```

That's it! If you return a complex variable from an event handler, ColdBox will convert it to JSON for you automatically. We will even inspect the return object and if it has a `$renderdata()` method, we will call it for you and return your very own marshalled data! But there's more. You can add a `renderData` annotation to your action and add any valid `renderdata()` type and it will return it accordingly.

```java
function data( event, rc, prc ) renderdata="xml"{
  return [1,2,3];
}

function data( event, rc, prc ) renderdata="pdf"{
  event.setView( "users/index" );
}
```

You can even add a default response type by adding the `renderdata` annotation to the `component` construct:

```java
component renderdata="json"{

}
```

#### Named Regions

We have introduced the concept of named rendering regions with ColdBox 5. As we increase the reuse of views and create many self-sufficient views or widgets, we end up with `setView()` or `renderView()` method calls that expose too much logic, location, parameters, etc. With named regions, we can actually abstract or describe how a region should be rendered and then render it when we need it via a name.

**Definition**

```java
event.setView(
  view = "users/detail",
  args = { data = userData, border = false },
  layout = "main",
  name = "userDetail"
);

event.setView(
  view = "users/banner",
  args = { data = userData },
  name = "userBanner"
);
```

**Rendering**

```text
<div id="banner">#renderView( name="userBanner" )#</div>

<div id="detail">#renderView( name="userDetail" )#</div>
```

### Testing Enhancements

Here are some of the major updates for integration testing with ColdBox and TestBox:

* Reset the response when calling `setup()` in integration testing to avoid duplicate headers within same request executions.
* Base test case doesn't allow for inherited annotations.  It now does since we moved the testing life-cycle methods to be annotation based instead of by name.
* Added dynamic methods `getRenderData()` and `getStatusCode()` helpers to the request context upon `execute()` execution.  This will allow you a shorthand approach to getting response status codes and response rendering data struct.

## Upgrading to ColdBox 5

The major compatibility issues will be covered as well as how to smoothly upgrade to this release from previous ColdBox versions. You can also check out the [What's New](https://coldbox.ortusbooks.com/intro/introduction/whats-new-with-5.0.0) guide to give you a full overview of the changes.

### `event.getCurrentHandler()` returns different results

In ColdBox 4, calling `getCurrentHandler()` would return the module if it existed:

```text
module:handler
```

In ColdBox 5, we correctly remove the **module** name so it can be retrieved via `getCurrentModule()` instead.

```text
handler
```

### `setNextEvent()` deprecated in favor of `relocate()`

The `setNextEvent()` method has been renamed to `relocate()` to better adjust to modern times. This deprecation will be removed in future versions of ColdBox.

### Modules AutoReload deprecated

The modules `autoReload` flag has been deprecated. This causes more headaches than anything. If you want changes reflected, reinit the framework.

### `onInvalidEvent` renamed to `invalidEventHandler`

The ColdBox construct setting `onInvalidEvent` has been renamed now to `invalidEventHandler`. This will be removed in future versions of ColdBox.

### `setAutoReload()` Removed

The `setAutoReload()` flag in the SES interceptor has been removed. It is evil I tell you. If you want your routing to be re-read, then reinit the framework.

### BuildLink LinkTo Argument Deprecated

The `buildLink()` method had the argument `linkTo` to denote the event or route to link to. This was verbose, so we shortened it to `to`:

```text
<!-- Before -->
<a href="#event.buildLink( linkTo="/about/us" )#">About Us</a>

<!-- After -->
<a href="#event.buildLink( to="/about/us" )#">About Us</a>
```

### Railo Support Dropped

Railo support dropped. Any classes that started with the word `Railo` need to be changed to `Lucee` especially on the CacheBox providers.

### ColdFusion 9-10 Support Dropped

ColdFusion 9-10 support has been dropped. Adobe doesn't support them anymore, so neither do we.

### Datasources Configuration Dropped

The `datasources` configuration setting directive has been dropped in favor of just leveraging the `settings` directive. Just move your datasource metadata into the `settings` struct and reference it using the settings DSL.

**Previous**

```javascript
// Datasource definitions
datasources = {
    mydsn = {
        type = "mysql",
        username = "luis",
        name = "mydsn"
    }
}
```

```text
// Injections
property name="dsn" inject="coldbox:datasource:mydsn"
```

**New**

```javascript
// Settings
settings = {
    mydsn = {
        type = "mysql",
        username = "luis",
        name = "mydsn"
    }
}
```

```text
// Injections

property name="dsn" inject="coldbox:setting:mydsn"
```

### DSL Builder Interface Update

The WireBox interface: `coldbox.system.ioc.dsl.IDSLBuilder` has changed in ColdBox 5, so if you are implementing your own DSLs, then you must update it like so:

{% code title="Old DSL Builder, notice the output attribute" %}
```javascript
public any function init( required any injector ) output="false"{ 
public any function process( required definition, targetObject ) output="false"{
```
{% endcode %}

In ColdBox 5 we removed the output attribute:

{% code title="New DSL Builder" %}
```javascript
public any function init( required any injector ) { 
public any function process( required definition, targetObject ) {
```
{% endcode %}

{% hint style="info" %}
Ticket Reference: [https://ortussolutions.atlassian.net/browse/COLDBOX-697](https://ortussolutions.atlassian.net/browse/COLDBOX-697)
{% endhint %}

### Interceptor Update

#### appendToBuffer\(\) Dropped

The string buffer has been updated in order to become thread safe and there are several methods that have been deprecated. Instead of using `appendToBuffer()` you need to use the `buffer` argument.

**Previous**

```javascript
appendToBuffer( generatedHTML );
```

**New**

```javascript
buffer.append( generatedHTML );
```

{% hint style="info" %}
Ticket Reference: [https://ortussolutions.atlassian.net/browse/COLDBOX-654](https://ortussolutions.atlassian.net/browse/COLDBOX-654)
{% endhint %}

## About This Book

The source code for this book is hosted in GitHub: [https://github.com/ortus-docs/coldbox-docs](https://github.com/ortus-docs/coldbox-docs). You can freely contribute to it and submit pull requests. The contents of this book is copyright by [Ortus Solutions, Corp](https://www.ortussolutions.com) and cannot be altered or reproduced without author's consent. All content is provided _"As-Is"_ and can be freely distributed.

* The majority of code examples in this book are done in `cfscript`.
* The majority of code generation and running of examples are done via **CommandBox**: The ColdFusion \(CFML\) CLI, Package Manager, REPL - [http://www.ortussolutions.com/products/commandbox](http://www.ortussolutions.com/products/commandbox)
* All ColdFusion examples designed to run on the open soure Lucee Platform or Adobe ColdFusion 11+

### External Trademarks & Copyrights

Flash, Flex, ColdFusion, and Adobe are registered trademarks and copyrights of Adobe Systems, Inc.

### Notice of Liability

The information in this book is distributed “as is”, without warranty. The author and Ortus Solutions, Corp shall not have any liability to any person or entity with respect to loss or damage caused or alleged to be caused directly or indirectly by the content of this training book, software and resources described in it.

### Contributing

We highly encourage contribution to this book and our open source software. The source code for this book can be found in our [GitHub repository](https://github.com/ortus-docs/coldbox-docs) where you can submit pull requests.

### Charitable Proceeds

10% of the proceeds of this book will go to charity to support orphaned kids in El Salvador - [https://www.harvesting.org/](https://www.harvesting.org/). So please donate and purchase the printed version of this book, every book sold can help a child for almost 2 months.

#### Shalom Children's Home

Shalom Children’s Home \([https://www.harvesting.org/](https://www.harvesting.org/)\) is one of the ministries that is dear to our hearts located in El Salvador. During the 12 year civil war that ended in 1990, many children were left orphaned or abandoned by parents who fled El Salvador. The Benners saw the need to help these children and received 13 children in 1982. Little by little, more children came on their own, churches and the government brought children to them for care, and the Shalom Children’s Home was founded.

Shalom now cares for over 80 children in El Salvador, from newborns to 18 years old. They receive shelter, clothing, food, medical care, education and life skills training in a Christian environment. The home is supported by a child sponsorship program.

We have personally supported Shalom for over 6 years now; it is a place of blessing for many children in El Salvador that either have no families or have been abandoned. This is good earth to seed and plant.

## Author

### Luis Fernando Majano Lainez

Luis Majano is a Computer Engineer with over 15 years of software development and systems architecture experience. He was born in [San Salvador, El Salvador](http://en.wikipedia.org/wiki/El_Salvador) in the late 70’s, during a period of economical instability and civil war. He lived in El Salvador until 1995 and then moved to Miami, Florida where he completed his Bachelors of Science in Computer Engineering at [Florida International University](http://fiu.edu). Luis resides in Rancho Cucamonga, California with his beautiful wife Veronica, baby girl Alexia and baby boy Lucas!

He is the CEO of [Ortus Solutions](http://www.ortussolutions.com), a consulting firm specializing in web development, ColdFusion \(CFML\), Java development and all open source professional services under the ColdBox and ContentBox stack. He is the creator of ColdBox, ContentBox, WireBox, MockBox, LogBox and anything “BOX”, and contributes to many open source ColdFusion projects. He is also the Adobe ColdFusion user group manager for the [Inland Empire](http://www.iecfug.org). You can read his blog at [www.luismajano.com](http://www.luismajano.com)

Luis has a passion for Jesus, tennis, golf, volleyball and anything electronic. Random Author Facts:

* He played volleyball in the Salvadorean National Team at the tender age of 17
* The Lord of the Rings and The Hobbit is something he reads every 5 years. \(Geek!\)
* His first ever computer was a Texas Instrument TI-86 that his parents gave him in 1986. After some time digesting his very first BASIC book, he had written his own tic-tac-toe game at the age of 9. \(Extra geek!\)
* He has a geek love for circuits, microcontrollers and overall embedded systems.
* He has of late \(during old age\) become a fan of running and bike riding with his family.

> Keep Jesus number one in your life and in your heart. I did and it changed my life from desolation, defeat and failure to an abundant life full of love, thankfulness, joy and overwhelming peace. As this world breathes failure and fear upon any life, Jesus brings power, love and a sound mind to everybody!
>
> “Trust in the LORD with all your heart, and do not lean on your own understanding.” – Proverbs 3:5

### Contributors

#### Jorge Emilio Reyes Bendeck

Jorge is an Industrial and Systems Engineer born in El Salvador. After finishing his Bachelor studies at the Monterrey Institute of Technology and Higher Education [ITESM](http://www.itesm.mx/wps/wcm/connect/ITESM/Tecnologico+de+Monterrey/English), Mexico, he went back to his home country where he worked as the COO of[ Industrias Bendek S.A.](http://www.si-ham.com/). In 2012 he left El Salvador and moved to Switzerland in pursuit of the love of his life. He married her and today he resides in Basel with his lovely wife Marta and their daughter Sofía.

Jorge started working as project manager and business developer at Ortus Solutions, Corp. in 2013, . At Ortus he fell in love with software development and now enjoys taking part on software development projects and software documentation! He is a fellow Christian who loves to play the guitar, worship and rejoice in the Lord!

> Therefore, if anyone is in Christ, the new creation has come: The old has gone, the new is here!  
> 2 Corinthians 5:17

#### Brad Wood

Brad grew up in southern Missouri where he systematically disassembled every toy he ever owned which occasionally led to unintentional shock therapy \(TVs hold charge long after they've been unplugged, you know\) After high school he majored in Computer Science with a music minor at [MidAmerica Nazarene University](http://www.mnu.edu) \(Olathe, KS\). Today he lives in Kansas City with his wife and three girls where he still disassembles most of his belongings \(including automobiles\) just with a slightly higher success rate of putting them back together again.\) Brad enjoys church, all sorts of international food, and the great outdoors.

Brad has been programming CFML for 12+ years and has used every version of CF since 4.5. He first fell in love with ColdFusion as a way to easily connect a database to his website for dynamic pages. Brad blogs at \([http://www.codersrevolution.com](http://www.codersrevolution.com)\) and likes to work on solder-at-home digitial and analog circuits with his daughter as well as building projects with Arduino-based microcontrollers.

Brad's CommandBox Snake high score is 141.

