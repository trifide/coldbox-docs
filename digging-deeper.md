# Digging deeper

## Interceptors

Interceptors are CFC listeners that react on incoming events. Events can be announced by the core framework or be custom events from your application. These interceptors can also be stacked to form interceptor chains that can be executed implicitly for you. This is a powerful feature that can help developers and framework contributors share and interact with their work. \(Read more on [Intercepting Filters](http://www.corej2eepatterns.com/Patterns2ndEd/InterceptingFilter.htm)\)

### Event Driven Programming

The way that interceptors are used is usually referred to as event-driven programming, which can be very familiar if you are already doing any JavaScript or AngularJS coding. You can listen and execute intercepting points anywhere you like in your application, you can even produce content whenever you announce:

**Custom Announcements**

```markup
<!-- Announce an event in the div and produce content -->
<div>#announceInterception( 'onSidebar' )#</div>

// Announce inside a function
if( userCreated ){
    announceInterception( 'onUserCreate', { user = prc.oUser } );
}
```

If you are familiar with design patterns, custom interceptors can give you an implementation of observer/observable listener objects, much like any event-driven system can provide you. In a nutshell, an observer is an object that is registered to listen for certain types of events, let's say as an example `onError` is a custom interception point and we create a CFC that has this `onError` method. Whenever in your application you announce or broadcast that an event of type onError occurred, this CFC will be called by the ColdBox interceptor service.

**Interceptor Example**

```javascript
component extends="coldbox.system.Interceptor"{

    function onError( event, interceptData={} ){
        // Listen to onError events
    }

}
```

#### Resources

* [http://en.wikipedia.org/wiki/Observer\_pattern](http://en.wikipedia.org/wiki/Observer_pattern)
* [http://sourcemaking.com/design\_patterns/observer](http://sourcemaking.com/design_patterns/observer)
* [http://java.sun.com/blueprints/corej2eepatterns/Patterns/InterceptingFilter.html](http://java.sun.com/blueprints/corej2eepatterns/Patterns/InterceptingFilter.html)

#### For what can I use them

Below are just a few applications of ColdBox Interceptors:

* Security
* Event based Security
* Method Tracing
* AOP Interceptions
* Publisher/Consumer operations
* Implicit chain of events
* Content Appending or Pre-Pending
* View Manipulations
* Custom SES support
* Cache advices on insert and remove
* Much much more...

  \*

## How do they work?

Interceptors are CFCs that extend the ColdBox Interceptor class \(`coldbox.system.Interceptor`\), implement a configuration method called `configure()`, and then contain methods for the events it will listen for. All interceptors are treated as **singletons** in your application, so make sure they are thread safe and var scoped.

```text
/**
* My Interceptor
*/
component extends="coldbox.system.Interceptor"{

    function configure(){}

    function preProcess( event, interceptData, buffer, rc, prc ){}
}
```

> **Info** You can also remove the inheritance from the CFC \(preferred method\) and WireBox will extend the `coldbox.system.Interceptor` for you using [Virtual Inheritance](https://wirebox.ortusbooks.com/content/virtual_inheritance/).

You can use CommandBox to create interceptors as well:

```bash
coldbox create interceptor help
```

### Registration

Interceptors can be registered in your `Coldbox.cfc` configuration file using the `interceptors` struct, or they can be registered manually via the system's interceptor service.

In `ColdBox.cfc`:

```text
// Interceptors registration
interceptors = [
    {
        class   = "cfc.path",
        name    = "unique name/wirebox ID",
        properties = {
            // configuration
        }
    }
];
```

Registered manually:

```text
controller.getInterceptorService().registerInterceptor( interceptorClass="cfc.path" );
```

### The `configure()` Method

The interceptor has one important method that you can use for configuration called `configure()`. This method will be called right after the interceptor gets created and injected with your interceptor properties.

> **Info** These properties are local to the interceptor only!

As you can see on the diagram, the interceptor class is part of the ColdBox framework super type family, and thus inheriting the functionality of the framework.

## Conventions

By convention, any interceptor CFC must create a method with the same name as the event they want to listen to. This method has a return type of `boolean` and receives 3 arguments. So let's explore their rules.

```javascript
boolean function {point}( event, interceptData, buffer, rc, prc );
```

### Arguments

* `event` which is the request context object
* `interceptData` which is a structure of data that the broadcaster sends
* `buffer` which is a request buffer object you can use to elegantly produce content that is outputted to the user's screen
* `rc` reference to the request collection struct
* `prc` reference to the private request collection struct

### Return type

The intercepting method returns `boolean` or `void`. If boolean then it means something:

* **True** means break the chain of execution, so no other interceptors in the chain will fire.
* **False** or `void` continue execution

```javascript
component extends="coldbox.system.Interceptor"{

    function configure(){}

    boolean function afterConfigurationLoad(event,interceptData,buffer){
        if( getProperty('interceptorCompleted') eq false){
            parseAndSet();    
            setProperty('interceptorCompleted',true);
        }

        return false;
    }
}
```

Also remember that all interceptors are created by WireBox, so you can use dependency injection, configuration binder's, and even [AOP](http://wirebox.ortusbooks.com) on interceptor objects. Here is a more complex sample:

**HTTP Security Example:**

```javascript
/**
* Intercepts with HTTP Basic Authentication
*/
component {

    // Security Service
    property name="securityService" inject="id:SecurityService";

    void function configure(){
        if( !propertyExists("enabled") ){
            setProperty("enabled", true );
        } 
    }

    void function preProcess(event,struct interceptData, buffer){

        // verify turned on
        if( !getProperty("enabled") ){ return; }

        // Verify Incoming Headers to see if we are authorizing already or we are already Authorized
        if( !securityService.isLoggedIn() OR len( event.getHTTPHeader("Authorization","") ) ){

            // Verify incoming authorization
            var credentials = event.getHTTPBasicCredentials();
            if( securityService.authorize(credentials.username, credentials.password) ){
                // we are secured woot woot!
                return;
            };

            // Not secure!
            event.setHTTPHeader(name="WWW-Authenticate",value="basic realm=""Please enter your username and password for our Cool App!""");

            // secured content data and skip event execution
            event.renderData(data="<h1>Unathorized Access<p>Content Requires Authentication</p>",statusCode="401",statusText="Unauthorized")
                .noExecution();
        }    

    }    

}
```

## Interceptor Declaration

Interceptors can be declared in the `Coldbox.cfc` configuration file or programmatically at runtime. If you register them in the configuration file, then you have control over the order in which they fire. If you register interceptors programmatically, you won't have control over the order of execution.

In the configuration file, interceptors are declared as an array of structures in an element called `interceptors`. The elements of each interceptor structure are:

* `class` - The required instantiation path of the CFC.
* `name` - An optional unique name for the interceptor. If this is not passed then the name of the CFC will be used. We highly encourage the use of a name to avoid collisions.
* `properties` - A structure of configuration properties to be passed to the interceptor

In `ColdBox.cfc`:

```text
// Interceptors registration
interceptors = [
    { 
        class = "cfc.path",
        name = "unique name/wirebox ID",
        properties = { 
            // configuration
        }
    }
];
```

**Example**

```text
// Interceptors registration
interceptors = [
    {
        class="coldbox.system.interceptors.SES",
        properties={}
    },
    {
        class="interceptors.RequestTrim",
        properties={
            trimAll=true
        }
    },
    {
        class="coldbox.system.interceptors.Security",
        name="AppSecurity",
        properties={
            rulesSource="model",
            rulesModel="securityRuleService@cb",
            rulesModelMethod="getSecurityRules",
            validatorModel="securityService@cb"
        }
    }
];
```

> **Info** Remember that order is important!

## Interceptor Registration

You can also register CFCs as interceptors programmatically by talking to the application's Interceptor Service that lives inside the main ColdBox controller. You can access this service like so:

```javascript
// via the controller
controller.getInterceptorService();

// dependency injection
property name="interceptorService" inject="coldbox:interceptorService";
```

Once you have a handle on the interceptor service you can use the following methods to register interceptors:

* `registerInterceptor()` - Register an instance or CFC path and discover all events it listens to by conventions
* `registerInterceptionPoint()` - Register an instance in a specific event only

Here are the method signatures:

```javascript
public any registerInterceptor([any interceptorClass], [any interceptorObject], [any<struct> interceptorProperties='[runtime expression]'], [any customPoints=''], [any interceptorName])

public any registerInterceptionPoint(any interceptorKey, any state, any oInterceptor)
```

**Examples**

```javascript
// register yourself to listen to all events declared
controller.getInterceptorService()
    .registerInterceptor( interceptorObject=this );

// register yourself to listen to all events declared and register new events: onError, onLogin
controller.getInterceptorService()
    .registerInterceptor( interceptorObject=this, customPoints="onError,onLogin" );

// Register yourself to listen to the onException event ONLY
controller.getInterceptorService()
    .registerInterceptionPoint( 
        interceptorKey="MyService", 
        state="onException", 
        oInterceptor=this
     );
```

## Core Interception Points

There are many interception points that occur during an MVC and Remote life cycle in a ColdBox application. We highly encourage you to follow our flow diagrams in our [ColdBox Overview]() section so you can see where in the life cycle these events fire. Also remember that each event can pass a data structure that can be used in your application.

## Application Life Cycle Events

| Interception Point | Intercept Structure | Description |
| :--- | :--- | :--- |
| afterConfigurationLoad | --- | This occurs after the framework loads and your applications' configuration file is read and loaded. An important note here is that your application aspects have not been configured yet and no modules have been loaded. |
| afterAspectsLoad | --- | This occurs after the configuration loads and the aspects have been configured. This is a great way to intercept on application start after all modules have loaded. |
| preReinit | --- | This occurs every time the framework is re-initialized |
| onException | exception - The _cfcatch_ exception structure | This occurs whenever an exception occurs in the system. This event also produces data you can use to respond to it. |
| onRequestCapture | --- | This occurs once the FORM/URL variables are merged into the request collection but before any event caching or processing is done in your application. This is a great event to use for incorporating variables into the request collections, altering event caching with custom variables, etc. |
| onInvalidEvent | invalidEvent - The invalid event | This occurs once an invalid event is detected in the application. This can be a missing handler or action. This is a great interceptor to use to alter behavior when events do not exist or to produce 404 pages. |
| applicationEnd | --- | This occurs after a request is received and made ready for processing. This simulates an on request start interception point. Please note that this interception point occurs before the request start handler. |
| sessionStart | _session_ structure | This occurs when a user's session starts |
| sessionEnd | sessionReference - A reference to the session structure | This occurs when a user's session ends |
| preProcess | --- | This occurs after a request is received and made ready for processing. This simulates an on request start interception point. Please note that this interception point occurs before the request start handler. |
| preEvent | processedEvent - The event that is about to be executed | This occurs before ANY event execution, whether it is the current event or called via the run event method. It is important to note that this interception point occurs before the preHandler convention. \(See Event Handler Guide Chapter\) |
| postEvent | processedEvent - The event that is about to be executed | This occurs after ANY event execution, whether it is the current event or called via the run event method. It is important to note that this interception point occurs after the postHandler convention \(See Event Handler Chapter\) |
| postProcess | --- | This occurs after rendering, usually the last step in an execution. This simulates an on request end interception point. |
| preProxyResults | {proxyResults} | This occurs right before any ColdBox proxy calls are returned. This is the last chance to modify results before they are returned. |

## Object Creating Events

| Interception Point | Intercept Structure | Description |
| :--- | :--- | :--- |
| afterHandlerCreation | handlerPath - The handler path | This occurs whenever a handler is created |
| afterInstanceCreation | mapping - The object mapping | This occurs whenever a [wirebox](http://wiki.coldbox.org/wiki/Wirebox.cfm) object is created |

[WireBox](http://wirebox.ortusbooks.com/content/wirebox_event_model/index.html) also announces several other events in the object creation life cycles, so please see [the WireBox events](http://wirebox.ortusbooks.com/content/wirebox_event_model/index.html)

## Layout-View Events

| Interception Point | Intercept Structure | Description |
| :--- | :--- | :--- |
| preLayout | --- | This occurs before any rendering or layout is executed |
| preRender | {renderedContent} | This occurs after the layout+view is rendered and this event receives the produced content |
| postRender | --- | This occurs after the content has been rendered to the buffer output |
| preViewRender | view - The name of the view to render | This occurs before any view is rendered |
| postViewRender | All of the data above plus: | This occurs after any view is rendered and passed the produced content |
| preLayoutRender | layout - The name of the layout to render | This occurs before any layout is rendered |
| postLayoutRender | Everything above plus: | This occurs after any layout is rendered and passed the produced content |

## Module Events

| Interception Point | Intercept Structure | Description |
| :--- | :--- | :--- |
| preModuleLoad | {moduleLocation, moduleName} | This occurs before any module is loaded in the system |
| postModuleLoad | {moduleLocation, moduleName, moduleConfig} | This occurs after a module has been loaded in the system |
| preModuleUnload | {moduleName} | This occurs before a module is unloaded from the system |
| postModuleUnload | {moduleName} | This occurs after a module has been unloaded from the system |

## CacheBox Events

Our caching engine, [CacheBox](http://cachebox.ortusbooks.com/content/cachebox_event_model/index.html), also announces several events during its life cycle, so please see The [CacheBox Events Section](http://cachebox.ortusbooks.com/content/cachebox_event_model/index.html) as well.

## Restricting Execution

You can restrict the execution of an interception point by using the `eventpattern` annotation. This annotation will be placed in the function and the value is a regular expression that the interceptor service uses to match against the incoming event. If the regex matches, the interception function executes, else it skips it. This is a great way for you to create interceptors that only fire not only for the specific interception point you want, but also on the specific incoming event.

```javascript
// only execute for the admin package
void function preProcess(event,struct interceptData) eventPattern="^admin\."{}

// only execute for the blog module
void function preProcess(event,struct interceptData) eventPattern="^blog:"{}

// only execute for the blog module and the home handler
void function preProcess(event,struct interceptData) eventPattern="^blog:home\."{}

// only execute for the blog, forums, and shop modules
void function preProcess(event,struct interceptData) eventPattern="^(blog|forum|shop):"{}
```

## Interceptor Output Buffer

Every interception point receives a unique request output buffer that can be used to elegantly produce output. Once the interception point is executed, the interceptor service will check to see if the output buffer has content, if it does it will advice to write the output to the ColdFusion output stream. This way, you can produce output very cleanly from your interception points, without adding any messy-encapsulation breaking `output=true` tags to your interceptors. \(**BAD PRACTICE**\). This is an elegant solution that can work for both core and custom interception points.

```javascript
// Using methods, meaning you inherited from Interceptor or registered at configuration time.
function preRender( event, interceptData, buffer, rc, prc ){
    //clear all of it first, just in case.
    arguments.buffer.clear();
    //Append to buffer
    arguments.buffer.append( '<h1>This software is copyright by Funky Joe!</h1>' );    
}
```

> **Hint** Each execution point will have its own clean buffer to work with. As each interception point has finalized execution, the output buffer is flushed, only if content exists.

### Output Buffer Argument

Here are some examples using the `buffer` argument:

```javascript
function onSidebar( event, interceptData, buffer, rc, prc ){
    savecontent variable="local.data"{
        /// HTML HERE
    }

    arguments.buffer.append( local.data );
}

// using argument
function preRender( event, interceptData, buffer, rc, prc ){
    //clear all of it first, just in case.
    arguments.buffer.clear();
    //Append to buffer
    arguments.buffer.append('<h1>This software is copyright by Funky Joe!</h1>');    
}
```

Here are some common methods on the buffer object:

* `append( str )` Append strings to the buffer
* `clear()` Clear the entire buffer
* `length()` Get size of the buffer
* `getString()` Get the entire string in the buffer
* `getBufferObject()` Get the actual java String Builder object

## Custom Events

In addition to all the events announced by the framework, you can also register your own custom events.

[Curt Gratz](https://vimeo.com/gratzc) shares a brief example and use case for ColdBox custom interception points in his video [ColdBox Custom Interception Points - How To](https://vimeo.com/17409335).

## Configuration Registration

In the `ColdBox.cfc` configuration file, there is a structure called `interceptorSettings` with two keys:

* `throwOnInvalidStates` - This tells the interceptor service to throw an exception if the state announced for interception is not valid or does not exist. By default it does not throw an exception but ignore the announcement.
* `customInterceptionPoints` - This key is a comma delimited list of custom interception points you will be registering for execution.

```javascript
//Interceptor Settings
interceptorSettings = {
    throwOnInvalidStates = false,
    customInterceptionPoints = "onLogin,onWikiTranslation,onAppClose"
};
```

The `customInterceptionPoints` is what interest us. This can be a list or an array of events your system can broadcast. This way, whenever interceptors are registered, they will be inspected for those events.

## Programmatic Registration

You can use the interceptor service to register new events or interception points via the `appendInterceptionPoints()` method. This way you can dynamically register events in your system:

```javascript
public any appendInterceptionPoints( array or list customPoints )
```

The value of the `customPoints` argument can be a list or an array of interception points to register so the interceptor service can manage them for you:

```javascript
controller.getInterceptorService()
    .appendInterceptionPoints( [ "onLog", "onError", "onRecordInserted" ] );
```

## Listening

Once your custom interception or event points are registered and CFC are registered then you can write the methods for listening to those events just like any other interceptor event:

```javascript
component{

    function onLog(event,interceptData,buffer){
        // your code here
    }

    function onRecordInserted(event,interceptData,buffer){
        // your code here
    }

}
```

## Announcing Interceptions

The last piece of the puzzle is how to announce events. You will do so via the inherited super type method `announceInterception()` that all your handlers,plugins and even the interceptors themselves have or via the interceptor service `processState()` method. This method accepts an incoming data struct which will be broadcasted alongside your event:

```javascript
// Announce with no data
announceInterception( "onExit" );

// Announce with data
announceInterception( 'onLog', {
    time = now(),
    user = event.getValue( "user" ),
    dataset = prc.dataSet
} );

// Announce via interceptor service
controller.getInterceptorService().processState( "onRecordInsert", {} );
```

> **Hint** Announcing events can also get some asynchronous love, read the [Interceptor Asynchronicity](../interceptor-asynchronicity/) for some asynchronous love.

## Unregistering Interceptors

Each interceptor can unregister itself from any event by using the `unregister( state )` method. This method is found in all Interceptors or can be accessed via the interceptor service as well.

```javascript
// Inside an interceptor
unregister( 'preProcess' );

// From the interceptor service
controller.getInterceptorService()
    .unregister( interceptorName="MyInterceptor", state="preProcess" );
```

## Reporting Methods

There are several reporting and utility methods in the interceptor service that I recommend you explore. Below are some sample methods:

```javascript
//get the entire state container for preProcess For metadata or reporting
getController().getInterceptorService().getStateContainer('preProcess');

//Get all the interception state containers for metadata or reporting
getController().getInterceptorService().getInterceptionStates();

//Get all the interception points registered in the application
getController().getInterceptorService().getInterceptionPoints();
```

## Interceptor Asynchronicity

In honor of one of my favorite bands and album, \[The Police\]\([http://en.wikipedia.org/wiki/Synchronicity\_\(The\_Police\_album](http://en.wikipedia.org/wiki/Synchronicity_%28The_Police_album)\) - [Synchronicity](http://www.youtube.com/watch?v=CMBufJmTTSA), we have some asynchrounous capabilities in ColdBox Interceptors. These features are thanks to the sponsorship of [Guardly](https://www.guardly.com/), Inc, Alert, Connect, Stay Safe. So please make sure to check them out and thank them for sponsoring this great feature set. The core interceptor service and announcement methods have some arguments that can turn asynchronicity on or off and can return a structure of threading data.

```javascript
any announceInterception(state, interceptData, async, asyncAll, asyncAllJoin, asyncJoinTimeout, asyncPriority);
```

The asynchronous arguments are listed in the table below:

| Argument | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| async | boolean | false | false | Threads the interception call so the entire execution chain is ran in a separate thread. Extra arguments that can be used: asyncPriority. |
| asyncAll | boolean | false | false | Mutually exclusive with the async argument. If true, this will execute the interception point but multi-thread each of the CFCs that are listening to the interception point. So if 3 CFCs are listening, then 3 separate threads will be created for each CFC call. By default, the calling thread will wait for all 3 separate threads to finalize in order to continue with the execution. Extra arguments that can be used: _asyncAllJoin_, _asyncTimeout_, _asyncPriority_. |
| asyncAllJoin | boolean | false | true | This flag is used only when combined with the asyncAll argument. If true \(default\), the calling thread will wait for all intercepted CFC calls to execute. If false, then the calling thread will not join the multi-threaded interception and continue immediate execution while the CFC's continue to execute in the background. |
| asyncPriority | string : _low,normal,high_ | false | normal | The thread priority that will be sent to each _cfthread_ call that is made by the system. |
| asyncJoinTimeout | numeric | false | 0 | This argument is only used when using the _asyncAll_ and _asyncAllJoin=true_ arguments. This argument is the number of milliseconds the calling thread should wait for all the threaded CFC listeners to execute. By default it waits until all threads finalize. |

All asynchronous calls will return a structure of thread information back to you when using the `announceInterception()` method or directly in the interceptor service, the `processState()` method. The structure contains information about all the threads that where created during the call and their respective information like: status, data, errors, monitoring, etc.

```javascript
threadData = announceInterception(
    state           = "onLogin", 
    interceptData   = { user=user }, 
    asyncAll        = true
);
```

Now that you have seen all asynchronous arguments and their features, let's investigate each use case one by one.

## Async Announcements

The first case involves where you want to completely detach an interception call into the background. You will accomplish this by using the `async=true` argument. This will then detach the execution of the interception into a separate thread and return to you a structure containing information about the thread it created for you. This is a great way to send work jobs, emails, processing and more to the background instead of having the calling thread wait for execution.

```javascript
threadData = announceInterception(
    state           = "onPageCreate", 
    interceptData   = { page= local.page }, 
    asyncAll        = true
);
```

### Configuration Arguments

You can also combine this call with the following arguments:

* `asyncPrirority` : The priority level of the detached thread. By default it uses `normal` priority level.

```javascript
threadData = announceInterception(
    state           = "onPageCreate", 
    interceptData   = { page= local.page }, 
    asyncAll        = true,
    asyncPriority   = "high"
);
```

## Async Listeners With Join

The second use case is where you want to run the interception but multi-thread **all** the interceptor CFCs that are listening to that interception point. So let's say we have our `onPageCreate` announcement and we have 3 interceptors that are listening to that interception point. Then by using the `asyncAll=true` argument, ColdBox will create 3 separate threads, one for each of those interceptors and execute their methods in their appropriate threads. This is a great way to delegate long running processes simultaneously on a specific piece of data. Also, by default, the caller will wait for all of those 3 threads to finalize before continuing execution. So it is a great way to process data and wait until it has become available.

```javascript
var threadData = announceInterception(
    state           = "onPageCreate", 
    interceptData   = {}, 
    asyncAll        = true
);
```

> **Caution** Please remember that you are also sharing state between interceptors via the `event` and `interceptData`, so make sure you either lock or are careful in asynchronous land.

### Configuration Arguments

You can also combine this call with the following arguments:

* `asyncPriority` : The priority level of each of the spawned threads. By default it uses `normal` priority level 
* `asyncJoinTimeout` : The timeout in milliseconds to wait for all spawned threads. By default it is set to 0, which waits until ALL threads finalize no matter how long they take.
* `asyncAllJoin` : The flag that determines if the caller should wait for all spawned threads or should just spawn all threads and continue immediately. By default, it waits until all spawned threads finalize.

```javascript
var threadData = announceInterception(state="onPageCreate", interceptData={}, asyncAll=true, asyncAllJoin=false);
var threadData = announceInterception(
    state           = "onPageCreate", 
    interceptData   = {}, 
    asyncAll        = true,
    asyncAllJoin    = false,
    asyncJoinTimeout = 30,
    asyncPriority   = "high"
);
```

## Async Listeners No Join

The third use case is exactly the same scenario as the async listeners but with the exception that the caller will **not** wait for the spawned threads at all. This is accomplished by using the `asyncAllJoin=false` flag. This tells ColdBox to just spawn, return back a structure of thread information and continue execution of the calling code.

```javascript
var threadData = announceInterception(
    state           = "onPageCreate", 
    interceptData   = {}, 
    asyncAll        = true, 
    asyncAllJoin    = false
);
```

### Configuration Arguments

You can also combine this call with the following arguments:

* `asyncPriority` : The priority level of each of the spawned threads. By default it uses `normal` priority level 

```javascript
var threadData = announceInterception(
    state           = "onPageCreate", 
    interceptData   = {}, 
    asyncAll        = true, 
    asyncAllJoin    = false,
    asyncPriority   = "high"
);
```

## Asynchronous Annotations

We have also extended the interceptor registration process so you can annotate interception points to denote threading. You will do so with the following two annotations:

| Argument | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| `async` | none | false | false | If the annotation exists, then ColdBox will execute the interception point in a separate thread only if not in a thread already. |
| `asyncPriority` | string : _low,normal,high_ | false | _normal_ | The thread priority that will be sent to each cfthread call that is made by the system. |

This allows you the flexibility to determine in code which points are threaded, which is a great way to use for emails, logging, etc.

```javascript
function preProcess( event, interceptData ) async asyncPriority="low"{
    // Log current request information
    log.info("Executing request: #event.getCurrentEvent()#", getHTTPRequestData() );    
}
```

So if you have 3 interceptors in a chain and only 1 of them is threaded, then the 2 will execute in order while the third one in the background. Overall, your processing power and choices have now grown.

## Flash RAM

The purpose of the Flash RAM is to allow variables to be persisted seamlessly from one request and be picked up in a subsequent request\(s\) by the same user. This allows you to hide implementation variables and create web flows or conversations in your ColdBox applications. So why not just use session or client variables? Well, most developers forget to clean them up and sometimes they just end up overtaking huge amounts of RAM and no clean cut definition is found for them. With Flash RAM, you have the facility already provided to you in an abstract and encapsulated format. This way, if you need to change your flows storage scope from session to client scope, the change is seamless and painless.

Every handler, plugin, interceptor, layout and view has a flash object in their variables scope already configured for usage. The entire flash RAM capabilities are encapsulated in a flash object that you can use in your entire ColdBox code. Not only that, but the ColdBox Flash object is based on an interface and you can build your own Flash RAM implementations very easily. It is extremely flexible to work on a concept of a Flash RAM object than on a storage scope directly. So future changes are easily done at the configuration level.

Our Flash Scope also can help you maintain flows or conversations between requests by using the `discard()` and `keep()` methods. This will continue to expand in our 3.X releases as we build our own conversations DSL to define programmatically wizard like or complex web conversations. Also, the ColdBox Flash RAM has the capability to not only maintain this persistence scope but it also allows you to seamlessly re-inflate these variables into the request collection or private request collection, both or none at all.

## Flash Storage

There are times where you need to store user related variables in some kind of permanent storage then relocate the user into another section of your application, be able to retrieve the data, use it and then clean it. All of these tedious operations are definitely doable but why reinvent the wheel if we can have the platform give us a tool for maintaing conversation variables across requests. The key point for Flash RAM is where will the data be stored so that it is unique per user. ColdFusion gives us several persistent scopes that we can use and we have also created several flash storages for this purpose. Since the ColdBox flash scope is based on an interface, the flash scope storage can be virtually anywhere. You will find all of these implementations in the following package: coldbox.system.web.flash. In order to choose what implementation to use in your application you need to tell the `ColdBox.cfc` which one to use via flash configuration structure:

### Configuration

```javascript
// flash scope configuration
flash = {
    scope = "session,client,cluster,cache,or full path",
    properties = {}, // constructor properties for the flash scope implementation
    inflateToRC = true, // automatically inflate flash data into the RC scope
    inflateToPRC = false, // automatically inflate flash data into the PRC scope
    autoPurge = true, // automatically purge flash data for you
    autoSave = true // automatically save flash scopes at end of a request and on relocations.
};
```

Below is a nice chart of all the keys in this configuration structure so you can alter behavior of the Flash RAM objects:

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| scope | string or instantiation path | false | _session_ | Determines what scope to use for Flash RAM. The available aliases are: session, client, cluster, cache or a custom instantiation path |
| properties | struct | false | {} | Properties that can be used inside the constructor of any Flash RAM implementation |
| inflateToRC | boolean | false | true | Whatever variables you put into the Flash RAM, they will also be inflated or copied into the request collection for you automatically. |
| inflateToPRC | boolean | false | false | Whatever variables you put into the Flash RAM, they will also be inflated or copied into the private request collection for you automatically |
| autoPurge | boolean | false | true | This is what makes the Flash RAM work, it cleans itself for you. Be careful when setting this to false as it then becomes your job to do the cleaning |
| autoSave | boolean | false | true | The Flash RAM saves itself at the end of requests and on relocations via setNextEvent\(\). If you do not want auto-saving, then turn it off and make sure you save manually |

### Core Flash Implementations

The included flash implementations for ColdBox are:

| Name | Class | Description |
| :--- | :--- | :--- |
| Session | coldbox.system.web.flash.SessionFlash | Persists variables in session scope |
| Cluster | coldbox.system.web.flash.ClusterFlash | Persists variables in cluster scope via Railo only |
| Client | coldbox.system.web.flash.ClientFlash | Persists variables in client scope |
| Mock | coldbox.system.web.flash.MockFlash | Mocks the storage of Flashed variables. Great for unit/integration testing. |
| Cache | coldbox.system.web.flash.ColdboxCacheFlash | Persists variables in the ColdBox Cache |

#### Configuration Properties

Each RAM implementation can also use properties in order to alter its behavior upon construction via the `properties` configuration struct. Below are the properties our core implementations can use:

**Session Flash Settings**

* none

**Cluster Flash Settings**

* none

**Client Flash Settings**

* none

**Mock Flash Settings**

* none

**Cache Flash Settings**

* `cacheName` : The cache provider name declared in CacheBox

```javascript
// flash scope configuration
flash = {
    scope = "cache",
    properties = { cacheName="cluster" }
};
```

## Using Flash RAM

There are several ways to interact with the ColdBox Flash RAM:

* Using the `flash` scope object \(Best Practice\)
* Using the `persistVariables()` method from the super type and ColdBox Controller
* Using the `persistence` arguments in the `setNextEvent()` method from the super type and ColdBox Controller.

All of these methods interact with the Flash RAM object but the last two methods not only place variables in the temporary storage bin but actually serialize the data into the Flash RAM storage immediately. The first approach queues up the variables for serialization and at the end of a request it serializes the variables into the correct storage scope, thus saving precious serialization time. In the next section we will learn what all of this means.

### Flash Scope Object

The `flash` scope object is our best practice approach as it clearly demarcates the code that the developer is using the flash scope for persistence. Any flash scope must inherit from our `AbstractFlashScope` and has access to several key methods that we will cover in this section. However, let's start with how the flash scope stores data:

1. The flash persistence methods are called for saving data, the data is stored in an internal temporary request bin and awaiting serialization and persistence either through relocation or termination of the request.
2. If the flash methods are called with immediate save arguments, then the data is immediately serialized and stored in the implementation's persistent storage.
3. If the flash's `saveFlash()` method is called then the data is immediately serialized and stored in the implementation's persistent storage.
4. If the application relocates via setNextEvent\(\) or a request finalizes then if there is data in the request bin, it will be serialized and stored in the implementation's storage.

> **Caution** By default the Flash RAM queues up serializations for better performance, but you can alter the behavior programmatically or via the configuration file.
>
> **Info** If you use the `persistVariables()` method or any of the persistence arguments on the `setNextEvent()` method, those variables will be saved and persisted immediately.

To review the Flash Scope methods, please [go to the API](http://apidocs.ortussolutions.com/coldbox/current) and look for the correct implementation or the `AbstractFlashScope`.

> **Info** Please note that the majority of a Flash scope methods return itself so you can concatenate method calls. Below are the main methods that you can use to interact with the Flash RAM object:

#### clear\(\)

Clears the temporary storage bin

```javascript
flash.clear();
```

#### clearFlash\(\)

Clears the persistence flash storage implementation

```javascript
flash.clearFlash();
```

#### discard\(\)

```javascript
any discard([string keys=''])
```

Discards all or some keys from flash storage. You can use this method to implement flows.

```javascript
// discard all flash variables
flash.discard();

// discard some keys
flash.discard('userID,userKey,cardID');
```

#### exists\(\)

```javascript
boolean exists(string name)
```

Checks if a key exists in the flash storage

```javascript
if( flash.exists('notice') ){
 // do something
}
```

#### get\(\)

```javascript
any get(string name, [any default])
```

Get a value from flash storage and optionally pass a default value if it does not exist.

```javascript
// Get a flash key that you know exists
cardID = flash.get("cardID");

// Render some flash content
<div class="notice">#flash.get("notice","")#</div>
```

#### getKeys\(\)

Get a list of all the objects in the temp flash scope.

```javascript
Flash Keys: #flash.getKeys()#
```

#### getFlash\(\)

Get a reference to the permanent storage implementation of the flash scope.

```javascript
<cfdump var="#flash.getFlash()#">
```

#### getScope\(\)

Get the flash temp request storage used throughout a request until flashed at the end of a request.

```javascript
<cfdump var="#flash.getScope()#">
```

#### isEmpty\(\)

Check if the flash scope is empty or not.

```javascript
<cfif !flash.isEmpty()>
</cfif>
```

#### keep\(\)

```javascript
any keep([string keys=''])
```

Keep flash temp variable\(s\) alive for another relocation. Usually called from interceptors or event handlers to create conversations and flows of data from event to event.

```javascript
function step2(event){
    // keep variables for step 2 wizard
    flash.keep('userID,fname,lname');

    // keep al variables
    flash.keep();
}
```

#### persistRC\(\)

```javascript
any persistRC([string include=''], [string exclude=''], [boolean saveNow='false'])
```

Persist variable\(s\) from the ColdBox request collection into flash scope. Persist the entire request collection or limit the variables to be persisted by providing the keys in a list. "Include" will only try to persist request collection variables with keys in the list. "Exclude" will try to persist the entire request collection except for variables with keys in the list.

```javascript
// persist some variables that can be reinflated into the RC upon relocation
flash.persistRC(include="name,email,address");
setNextEvent('wizard.step2');

// persist all RC variables using exclusions that can be reinflated into the RC upon relocation
flash.persistRC(exclude="ouser");
setNextEvent('wizard.step2');

// persist some variables that can be reinflated into the RC upon relocation and serialize immediately
flash.persistRC(include="email,addressData",savenow=true);
```

#### put\(\)

```javascript
any put(string name, any value, [boolean saveNow='false'], [boolean keep='true'], [boolean inflateToRC=FROMConfig], [boolean inflateToPRC=FROMConfig], [boolean autoPurge=FROMConfig)
```

The main method to place data into the flash scope. Optional arguments control whether to save the flash immediately, inflate to RC or PRC on the next request, and if the data should be auto-purged. You can also use the configuration settings to have a consistent flash experience, but you can most certainly override the defaults. By default all variables placed in flash RAM are automatically purged in the next request once they are inflated UNLESS you use the keep\(\) methods in order to persist them longer or create flows. However, you can also use the autoPurge argument and set it to false so you can control when the variables will be removed from flash RAM. Basically a glorified ColdFusion scope that you can use.

```javascript
// persist some variables that can be reinflated into the RC upon relocation
flash.put(name="userData",value=userData);
setNextEvent('wizard.step2');

// put and serialize immediately.
flash.put(name="userData",value=userData,saveNow=true);

// put and mark them to be reinflated into the PRC only
flash.put(name="userData",value=userData,inflateToRC=false,inflateToPRC=true);


// put and disable autoPurge, I will remove it when I want to!
flash.put(name="userData",value=userWizard,autoPurge=false);
```

#### putAll\(\)

```javascript
any putAll(struct map, [boolean saveNow='false'], [boolean keep='true'], [boolean inflateToRC='[runtime expression]'], [boolean inflateToPRC='[runtime expression]'], [boolean autoPurge='[runtime expression]'])
```

Pass an entire structure of name-value pairs into the flash scope \(similar to the put\(\) method\).

```javascript
var map = {
    addressData = rc.address,
    userID = securityService.getUserID(),
    loggedIn = now()
};

// put all the variables in flash scope as single items
flash.putAll(map);

// Use them later
if( flash.get("loggedIn") ){

}
```

#### remove\(\)

```javascript
any remove(string name, [boolean saveNow='false'])
```

Remove an object from the temporary flash scope so it will not be included when the flash scope is serialized. To remove a key from the flash scope and make sure your changes are effective in the persistence storage immediately, use the saveNow argument.

```javascript
// mark object for removal
flash.remove('notice');

// mark object and remove immediately from flash storage
flash.remove('notice',true);
```

#### removeFlash\(\)

Remove the entire flash storage. We recommend using the clearing methods instead.

#### saveFlash\(\)

Save the flash storage immediately. This process looks at the temporary request flash scope, serializes it if needed, and persists to the correct flash storage on demand.

> **Info** We would advice to not overuse this method as some storage scopes might have delays and serializations

```javascript
flash.saveFlash();
```

#### size\(\)

Get the number of the items in flash scope.

```javascript
You have #flash.size()# items in your cart!
```

#### More Examples

```javascript
// handler code:
function saveForm(event){
    // save post
    flash.put("notice","Saved the form baby!");
    // relocate to another event
    setNextEvent('user.show');
}
function show(event){
    // Nothing to do with flash, inflating by flash object automatically
    event.setView('user.show');
}

// User/show.cfm template using if statements
<cfif flash.exists("notice")>
    <div class="notice">#flash.get("notice")#</div>
</cfif>

// User/show.cfm using defaults
<div class="notice">#flash.get(name="notice",defaultValue="")</div>
```

## Creating Your Own Flash Scope

The ColdBox Flash capabilities are very flexible and you can easily create your own Flash Implementations by doing two things:

1. Create a CFC that inherits from `coldbox.system.web.flash.AbstractFlashScope`
2. Implement the following functions: `clearFlash(), saveFlash(), flashExists(), and getFlash()`

### Implementable Methods

| Method | ReturnType | Description |
| :--- | :--- | :--- |
| clearFlash\(\) | void | Will destroy or clear the entire flash storage structure. |
| saveFlash\(\) | void | Will be called before relocations or on demand in order to flash the storage. This method usually talks to the getScope\(\) method to retrieve the temporary flash variables and then serialize and persist. |
| flashExists\(\) | boolean | Checks if the flash storage is available and has data in it. |
| getFlash\(\) | struct | This method needs to return a structure of flash data to reinflate and use during a request. |

> **Caution** It is the developer's responsibility to provide consistent storage locking and synchronizations.

All of the methods must be implemented and they have their unique purposes as you read in the description. Let's see a real life example, below you can see the flash implementation for the session scope:

```javascript
/**
*********************************************************************************
* Copyright Since 2005 ColdBox Framework by Luis Majano and Ortus Solutions, Corp
* www.coldbox.org | www.luismajano.com | www.ortussolutions.com
********************************************************************************
* This flash scope is smart enough to not create unecessary session variables
* unless data is put in it.  Else, it does not abuse session.
* @author Luis Majano <lmajano@ortussolutions.com>
*/
component extends="coldbox.system.web.flash.AbstractFlashScope" accessors="true"{

    // The flash key
    property name="flashKey";

    /**
    * Constructor
    * @controller.hint ColdBox Controller
    * @defaults.hint Default flash data packet for the flash RAM object=[scope,properties,inflateToRC,inflateToPRC,autoPurge,autoSave]
    */
    function init( required controller, required struct defaults={} ){
        super.init( argumentCollection=arguments );

        variables.flashKey = "cbox_flash_scope";

        return this;
    }

    /**
    * Save the flash storage in preparing to go to the next request
    * @return SessionFlash
    */
    function saveFlash(){
        lock scope="session" type="exclusive" throwontimeout="true" timeout="20"{
            session[ variables.flashKey ] = getScope();
        }
        return this;
    }

    /**
    * Checks if the flash storage exists and IT HAS DATA to inflate.
    */
    boolean function flashExists(){
        // Check if session is defined first
        if( NOT isDefined( "session" ) ) { return false; }
        // Check if storage is set and not empty
        return ( structKeyExists( session, getFlashKey() ) AND NOT structIsEmpty( session[ getFlashKey() ] ) );
    }

    /**
    * Get the flash storage structure to inflate it.
    */
    struct function getFlash(){
        if( flashExists() ){
            lock scope="session" type="readonly" throwontimeout="true" timeout="20"{
                return session[ variables.flashKey ];
            }
        }

        return {};
    }

    /**
    * Remove the entire flash storage
    * @return SessionFlash
    */
    function removeFlash(){
        lock scope="session" type="exclusive" throwontimeout="true" timeout="20"{
             structDelete( session, getFlashKey() );
        }
        return this;
    }

}
```

As you can see from the implementation, it is very straightforward to create a useful session flash RAM object.

## HTML Helper

### Introduction

The HTML Helper is a core ColdBox CFC that abstracts the creation of any HTML entity. It provides consistent and secure rendering of HTML within ColdBox applications.

```text
coldbox.system.core.dynamic.HTMLHelper
```

{% hint style="info" %}
Please check out the [latest CFC Docs ](http://apidocs.ortussolutions.com/coldbox/current)for all the methods available to the HTML Helper.
{% endhint %}

There is no special setup needed to use it in a ColdBox application, it's already baked in. Just reference the object by the `html` prefix and call the desired function within any layout or view.

```markup
// CFML
#html.button(name = "searchView", value = "View")#

// HTML Output
<button name="searchView" id="searchView" type="button">View<button>
```

By specifying the `name` of the element and not the `id`, both attributes are output using the same value. You can specify a different value for the `id` attribute by explicitly defining it.

```markup
// CFML w/ differnet name and id
#html.button(name = "searchView", id="id_searchView", value = "View")#

// HTML Output
<button name="searchView" id="id_searchView" type="button">View<button>
```

You can also pass in any attribute to the registered functions and if it does not match an argument it is passed through into the HTML element:

```markup
// CFML w/ differnet name and id
#html.button(
  name = "searchView", 
  id="id_searchView",
  value = "View",
  class = "btn btn-default"
)#

// HTML Output
<button 
  name="searchView" 
  id="id_searchView" 
  type="button" 
  class="btn btn-default">
  View
<button>
```

### Injecting the HTML Helper

You can inject the HTML helper anywhere you like by using the following registered mapping: `HTMLHelper@coldbox`

```javascript
property name="html" inject="HTMLHelper@coldbox";
```

## ColdBox Proxy

The ColdBox proxy enables remote applications or technologies like Flex, AIR, SOAP WebServices, Event Gateways, ColdFusion RESTful Webservices to communicate with ColdBox and provide an event driven model framework for those applications or for it to act as an enhanced service layer.

**The one key feature here, is that ColdBox morphs into a remote event-driven framework and no longer an MVC framework that produces HTML.**

## Getting Started

The concept behind the ColdBox proxy is to create CFC's that extend our proxy class: `coldbox.system.remote.ColdboxProxy`. This will give you the ability to locate and talk to your running ColdBox application so you can proxy in requests from remote systems like Flex/Air, Event Gateways, ColdFusion REST/Soap Web Services and even CFC data binding.

```javascript
component extends="coldbox.system.remote.ColdboxProxy"{

}
```

The concept of a [Proxy](http://en.wikipedia.org/wiki/Proxy_pattern) is to give access to another system. As Wikipedia mentions:

> "A proxy, in its most general form, is a class functioning as an interface to something else. The proxy could interface to anything: a network connection, a large object in memory, a file, or some other resource that is expensive or impossible to duplicate" Wikipedia

The proxy will give you access to your entire ColdBox application assets but also allow you to proxy in request to the normal ColdBox event model. You will do this via our `process()` method. This method expects an `event` argument to be passed to it which is the event string that will be executed for you and all the other arguments passed to this method will be converted and merged into the request collection according to their passed name.

Then your event handlers can respond to these requests just like normal requests and even return data back to the caller.

> **Hint** The advanced ColdBox templates gives you a sample proxy object in your `remote/MyProxy.cfc` folder.

### Organization

Most remote APIs are strongly typed so it makes sense to create as many ColdBox proxy objects as you see fit. Don't just create one proxy with 1000 methods on it. Try to apply identity to these objects as well. We also recommend you create a `remote` folder in your application where you can store all your remote proxy objects.

```javascript
/Application
  /remote
    + MyProxy.cfc
```

Here is a sample proxy object that just proxies a remote call

```javascript
component extends="coldbox.system.remote.ColdboxProxy"{

    /**
    * Get user data
    * @id The id of the user list to return
    */
    array function getData( required numeric id ){
        arguments.event = "users.getListData";

        var results = super.process( argumentCollection=arguments );

        return results ?: [];
    }
}
```

### AppMapping

However, since some of these requests won't be done via HTTP but other protocols like Flex/Air binary protocols or event gateways, your ColdBox application must know where in your server the application is located in, so the `Application.cfc` methods fire. By default, when using HTTP calls, ColdBox can auto-locate your application with no issues at all, but with Flex/AIR or other protocols you must set this location in your `Application.cfc` via the `COLDBOX_APP_MAPPING` directive **ONLY if not in the webroot of an application**.

```javascript
COLDBOX_APP_MAPPING   = "";
```

This tells the framework where in the web server \(sub-folder\) your application is located in. By default, your application is the root of your website so this value is empty or `/` and you won't modify this. But if your application is in a sub-folder then add the full instantation path here. So if your application is under `/apps/myApp` then the value would be:

```javascript
COLDBOX_APP_MAPPING   = "apps.myApp";
```

> **Info** The `COLDBOX_APP_MAPPING` value is an instantiation path value

## The Base Proxy Object

Here are some common methods of our ColdBox proxy object. However, we encourage you to see the [API docs](http://apidocs.ortussolutions.com/coldbox/current) for that latest and greatest.

| Method | Description |
| :--- | :--- |
| announceInterception\(state, data\) | Processes a remote interception. |
| getCacheBox\(\) | Get a reference to [CacheBox](http://wiki.coldbox.org/wiki/CacheBox.cfm) |
| getCache\(cacheName='default'\) | Get a reference to a named cache provider |
| getController\(\) | Returns the ColdBox controller instance |
| getInterceptor\(\) | Get a named interceptor |
| getLogBox\(\) | Get a reference to LogBox |
| getInstance\(name,dsl,initArguments\) | Get a [Wirebox](http://wiki.coldbox.org/wiki/Wirebox.cfm) model object |
| getWireBox\(\) | Get a reference to [WireBox](http://wiki.coldbox.org/wiki/WireBox.cfm) |
| process\(\) | Processes a remote call that will execute a coldbox event and returns data/objects back. |
| loadColdBox\(\) | Gives you the ability to load any external coldbox application in the application scope. Great for remotely loading any coldbox application, it can be located anywhere. |
| getRemotingUtil\(\) | Return a utility class to manipulate output and buffer |

API Docs: [http://apidocs.ortussolutions.com/coldbox/current](http://apidocs.ortussolutions.com/coldbox/current)

## The Event Handlers

The event handlers that you will produce for remote interaction are exactly the same as your other handlers, with the exception that they have a return type and return data back to the caller; our proxies. Then our proxies can strong type the return data elements:

**Handler:**

```javascript
function getCacheKeys(event,rc,prc){
    return getCache( rc.cacheProvider ).getKeys();
}

function listUsers(event,rc,prc){
    prc.data = userService.list();

    if( event.isProxyRequest() ){
        return prc.data;
    }

    event.setView("users/listUsers");
}
```

> **Hint** The request context has a method called `isProxyRequest()` which tells the application if the request is coming from a ColdBox proxy.

**Proxy:**

```javascript
array function getCachekEys(string cacheProvider='default'){
    arguments.event = "proxy.getCacheKeys";
    return process(argumentCollection=arguments);
}

string function getUsersJSON(){
    arguments.event = "proxy.listUsers";
    return serializeJSON( process(argumentCollection=arguments) );
}
```

## Distinguishing Request Types

Now, what if you want to distinguish between a normal request and a proxy request? Well, the request context object, most commonly known as the event object has a method called:

* `isProxyRequest` : This boolean method determines what type of request is being executed.

```javascript
function listUsers(event,rc,prc){
    prc.data = userService.list();

    if( event.isProxyRequest() ){
        return prc.data;
    }

    event.setView("users/listUsers");
}
```

## RenderData\(\)

The ColdBox Proxy also has the ability to use the request context's `renderData()` method. So you can build a system that just uses this functionality to transform data into multiple requests. Even have the ability for the same handler to respond to REST/SOAP and MVC all in one method:

**Event Handler:**

```javascript
function list(event,rc,prc){
    prc.users = userService.list();
    event.renderData( data=prc.users, formats="xml,json,html" );
}
```

**Proxy:**

```javascript
string function getUsers(string format="json"){
    arguments.event = "users.list";
    return process(argumentCollection=arguments);
}
```

This handler can now respond to HTML requests, SOAP requests, Flex/Air Requests and even RESTFul requests. How about that!

## Proxy Events

The ColdBox Proxy also has a different life cycle than traditional MVC. All of a request's interception points fire with one addition: `preProxyResults`. This event fires right before the proxy returns results back to proxies. This is a great way to do transformations, logging, etc.

```javascript
component{

    function preProxyResults(event, interceptData){
        log.debug("Proxy request finalized: ", interceptData.proxyResults );
    }

}
```

## Standard Return Types

The `ColdBox.cfc` has one setting that affects proxy operation:

* `ProxyReturnCollection` : This boolean setting determines if the proxy should return what the event handlers return or just return the request collection structure every time a `process()` method is called. This can be a very useful setting, if you don't even want to return any data from the handlers \(via the `process()` method\). The framework will just always return the request collection structure back to the proxy caller. By default, the framework has this setting turned to **false** so you can have more control and flexibility.

```javascript
coldbox.proxyReturnCollection = true;
```

## Caveats & Gotchas

The most important gotchas in using the ColdBox proxy for remoting or even event gateways is pathing. Paths are totally different if you are using `expandPath()` or per-application mappings. Per-Application mappings can sometimes be a hassle for `onSessionEnd()`. So always be careful when setting up your paths and configurations for remoting. Try to always have the correct paths assigned and tested.

## Request Context Decorator

The **request context object** is bound to the framework release and as we all know, each application is different in requirements and architecture. Thus, we have the application of the [Decorator Pattern](https://www.tutorialspoint.com/design_pattern/decorator_pattern.htm) to our request context object in order to help developers program to their needs.

So what does this mean? Plain and simply, you can decorate the ColdBox request context object with one of your own. You can extend the functionality to the specifics of the software you are building. You are not modifying the framework code base, but extending it and building on top of what ColdBox offers you.

{% hint style="info" %}
"A decorator pattern allows new/additional behavior to be added to an existing method of an object dynamically." **Wikipedia**
{% endhint %}

#### Creating The Decorator

The very first step is to create your own request context decorator component. You can see in the diagram below of the ColdBox request context design pattern.

Create a component that extends `coldbox.system.web.context.RequestContextDecorator`, this is to provide all the functionality of an original request context decorator as per the design pattern. Once you have done this, you will create a `configure()` method that you can use for custom configuration when the request context gets created by the framework. Then it’s up to you to add your own methods or override the original request context methods.

{% hint style="success" %}
**Tip**: In order to access the original request context object you will use the provided method called: `getRequestContext()`.
{% endhint %}

**Declaration**

The following is a simple decorator class \(`MyDecorator.cfc`\) that auto-trims values when calling the `getValue()` method. You can override methods or create new ones.

{% code title="MyDecorator.cfc" %}
```java
component extends="coldbox.system.web.context.RequestContextDecorator"{

    function configure(){

        return this;
    }

    /**
     * @overriden
     * Get a value from the public or private request collection. and auto-trim it
     * @name The key name
     * @defaultValue default value
     * @private Private or public, defaults public.
     */
    function getValue( required name, defaultValue, boolean private=false ){
        var originalValue = "";

        // Check if the value exists via original object, else return blank
        if( getRequestContext().valueExists( arguments.name ) ){
            originalValue = getRequestContext().getValue( argumentCollection=arguments );
            // check if simple
            if( isSimpleValue( originalValue ) ){
                originalValue = trim( originalValue );
            }
        }

        return originalValue;
    }

}
```
{% endcode %}

As you can see from the code above, the possibilities to change behavior are endless. It is up to your specific requirements and it’s easy!

**Leveraging The Controller**

The request context decorator receives a reference to the ColdBox `controller` object and it can be retrieved by using the `getController()` method or via `variables.controller`. This will give you the ability to call settings, interceptions and much more, right from your decorator object.

#### Configuration

Now that we have created our `MyDecorator.cfc` let's tell ColdBox to use the decorator open your `ColdBox.cfc` and add the following `coldbox` directive:

```java
coldbox = {
    requestContextDecorator = "models.system.MyDecorator";
};
```

The value of the setting is the instantiation path of your request context decorator CFC. That's it. From now on the framework will use your request context decoration.

## Controller Decorator

The ColdBox controller object is bound to the framework release and as we all know, each application is different in requirements and architecture. Thus, we have the application of the [Decorator Pattern ](https://sourcemaking.com/design_patterns/decorator)to our ColdBox Controller object in order to help developers program to their needs. So what does this mean? Plain and simply, you can decorate the ColdBox Controller object with one of your own. You can extend the functionality to the specifics of the software you are building. You are not modifying the framework code base, but extending it and building on top of what ColdBox offers you.

> "A decorator pattern allows new/additional behavior to be added to an existing method of an object dynamically." Wikipedia

### For what can I use this?

* You can override the original framework methods to meet your needs.
* These new functionalities can be specific to your application and architectural needs.
* Have different Controllers for different protocols.
* Much more.

### How does it work

Create a component that extends `coldbox.system.web.ControllerDecorator`, this is to provide all the functionality of an original ColdBox Controller as per the design pattern. Once you have done this, you will create a `configure()` method that you can use for custom configuration when the Controller gets created by the framework on application start. Then it’s up to you to add your own methods or override the original methods. \([See API](https://apidocs.ortussolutions.com/coldbox/current)\).

{% hint style="info" %}
In order to access the original controller object you will use the provided method called: `getController().`
{% endhint %}

#### **Declaration**

Here is the Controller Decorator code:

{% code title="models/MyControllerDecorator.cfc" %}
```javascript
component extends="coldbox.system.web.ControllerDecorator"{

    function configure(){

    }

    function setNextEvent(){
        // Add SSL eq true to ALL relocations
        arguments.ssl = true;
        // Send the relocation back to the existing controller
        getController().setNextEvent( argumentCollection=arguments );
    }

}
```
{% endcode %}

#### Configuration

The last step in this guide is to show you how to declare your decorator in your ColdBox configuration file. Look at the sample snippet below:

{% code title="config/Coldbox.cfc" %}
```javascript
coldbox.controllerDecorator = "models.MyControllerDecorator";
```
{% endcode %}

The value of the setting is the instantiation path of your Controller decorator CFC.

#### Conclusion

As you can see from this guide, constructing ColdBox Controller Decorators are very easy. You have a pre-defined structure that you need to implement and an easy setting to configure. You now have a great way to expand on your application requirements and add additional functionality without modifying the framework. Just remember: With great power, comes great responsibility. The possibilities are endless with this feature. Enjoy!

## Recipes

A collection of useful recipes for the ColdBox Framework.

## Building REST APIs

### Overview

REST APIs are a popular and easy way to add HTTP endpoints to your web applications to act as web services for third parties or even other internal systems. REST is simpler and requires less verbosity and overhead than other protocols such as SOAP or XML-RPC.

Creating a fully-featured REST API is easy with the **ColdBox Platform**. Everything you need for creating routes, working with headers, basic auth, massaging data, and enforcing security comes out of the box. We even have several application templates just for REST, so you can use CommandBox to create your first RESTFul app:

```bash
## create app
coldbox create app name=MyRestAPP skeleton=rest

## Start up a server with rewrites
server start
```

You will then see the following JSON output:

```javascript
{
    data: "Welcome to my ColdBox RESTFul Service",
    error: false,
    messages: [ ],
    errorcode: "0"
}
```

{% hint style="info" %}
The `rest` template is a basic REST template that does not rely on modules or versioning. If you would like to add versioning and HMVC modularity use the `rest-hmvc` template. You can also find a full demo here: [https://github.com/lmajano/hmvc-presso-demo](https://github.com/lmajano/hmvc-presso-demo)
{% endhint %}

### Quick Reference Card

Below you can download our quick reference card on RESTFul APIs

[Download RefCard](https://github.com/ColdBox/cbox-refcards/raw/master/ColdBox%20REST%20APIs/ColdBox-REST-APIs-Refcard.pdf)

### Introduction To REST

REST stands for **Representational State Transfer** and builds upon the basic idea that data is represented as **resources** and accessed via a **URI**, or unique address. An HTTP client \(such as a browser, or the `CFHTTP` tag\) can send requests to a URI to interact with it. The HTTP verb \(GET, POST, etc\) sent in the header of the request tells the server how the client wants to interact with that resource.

As far as how your data is formatted or how you implement security is left up to you. REST is less prescriptive than other standards such as SOAP \(which uses tons of heavy XML and strictly-typed parameters\). This makes it more natural to understand and easier to test and debug.

### Defining Resources

A REST API can define its resources on its own domain \(`https://api.example.com`\), or after a static placeholder that differentiates it from the rest of the app \(`https://www.example.com/api/`\). We'll use the latter for these examples.

{% hint style="info" %}
Please note that we have an extensive [Routing mechanism](../../the-basics/routing/). Please check out our [routing sections](../../the-basics/routing/routing-dsl/) of the docs.
{% endhint %}

Let's consider a resource we need to represent called `user`. Resources should usually be nouns. If you have a verb in your URL, you're probably doing it wrong.

{% hint style="success" %}
**Hint** It is also important to note that REST is a style of URL architecture not a mandate, so it is an open avenue of sorts. However, you must stay true to its concepts of resources and usage of the HTTP verbs.
{% endhint %}

Here are a few pointers when using the HTTP verbs:

```text
http://www.example.com/api/user
```

* `GET /api/user` will return a representation of all the users. It is permissible to use a query string to control pagination or filtering.
* `POST /api/user/` will create a new user
* `GET /api/user/53` will return a representation of user 53
* `PUT /api/user/53` will update user 53
* `DELETE /api/user/53` will delete user 53

{% hint style="info" %}
GET, PUT, and DELETE methods should be **idempotent** which means repeated requests to the same URI don't do anything. Repeated POST calls however, would create multiple users.
{% endhint %}

In ColdBox, the easiest way to represent our `/api/user` resource is to create a handler called `user.cfc` in the `/handlers/api/` directory. In this instance, ColdBox will consider the `api` to be a handler package. You can leverage CommandBox for this:

```bash
coldbox create handler name=api.user actions=index,view,save,remove
```

{% hint style="success" %}
**Hint** This command will create all the necessary files for you and even the integration tests for you.
{% endhint %}

Here in my handler, I have stubbed out actions for each of the operations I need to perform against my user resource.

**/handlers/api/user.cfc**

```javascript
component {

  function index( event, rc, prc ) {
    // List all users
  }

  function view( event, rc, prc ) {
    // View a single user
  }

  function save( event, rc, prc ) {
    // Save a user
  }

  function remove( event, rc, prc ) {
    // Remove a user
  }

}
```

### Defining URL Routes

Now that we have this skeleton in place to represent our user resource, let's move on to show how you can have full control of the URL as well as mapping HTTP verbs to specific handler actions.

The default route for our `user.cfc` handler is `/api/user`, but what if we want the resource in the URL to be completely different than the handler name convention? To do this, use the `/config/Router.cfc.` file to declare URL routes we want the application to capture and define how to process them. This is your [URL Router](../../the-basics/routing/application-router.md) and it is your best friend!

{% hint style="success" %}
Install the `route-visualizer` module to visualize the router graphically. This is a huuuuge help when building APIs or anything with routes.

`install route-visualizer`
{% endhint %}

Let's add our new routes BEFORE the default route. We add them BEFORE because you must declare routes from the most specific to the most generic. Remember, routes fire in declared order.

```javascript
// Map route to specific user.  Different verbs call different actions!
component{

    function configure(){
        setFullRewrites( true );

        // User Resource
        route( "/api/user/:userID" )
            .withAction( {
                GET    = 'view',
                POST   = 'save',
                PUT    = 'save',
                DELETE = 'remove'
            } )
            .toHandler( "api.user" );

        route( ":handler/:action?" ).end();
    }

}
```

You can see if that if action is a string, all HTTP verbs will be mapped there, however a `struct` can also be provided that maps different verbs to different actions. This gives you exact control over how the requests are routed. We recommend you check out our [Routing DSL guide](../../the-basics/routing/routing-dsl/) as you can build very expressive and detailed URL patterns.

#### Route Placeholders

The `:userID` part of the [route pattern is a placeholder](../../the-basics/routing/routing-dsl/pattern-placeholders.md). It matches whatever text is in the URL in that position. The value of the text that is matched will be available to you in the request collection as `rc.userID`. You can get even more specific about what kind of text you want to match in your route pattern.

**Numeric Pattern Matcher**

Append `-numeric` to the end of the placeholder to only match numbers.

`addRoute( pattern = 'user/:userID-numeric' );`

This route will match `user/123` but not `user/bob`.

**Alphabetic Pattern Matcher**

Append `-alpha` to the end of the placeholder to only match upper and lowercase letters.

`addRoute( pattern = 'page/:slug-alpha' );`

This route will match `page/contactus` but not `page/contact-us3`.

**Regex Pattern Matcher**

For full control, you can specify your own regex pattern to match parts of the route

`addRoute( pattern = 'api/:resource-regex(user|person)' );`

This route will match `api/user` and `api/person`, but not `/api/contact`

**Placeholder Quantifiers**

You can also add the common regex `{}` quantifier to restrict how many digits a placeholder should have or be between:

```javascript
// two digits
:state{2}

// 2-4 digits
:year{2,4}

// 2 or more
:page{2,}
```

{% hint style="success" %}
If a route is not matched it will be skipped and the next route will be inspected. If you want to validate parameters and return custom error messages inside your handler, then don't put the validations on the route.
{% endhint %}

As you can see, you have many options to craft the URL routes your API will use. Routes can be as long as you need. You can even nest levels for URLs like `/api/users/contact/address/27` which would refer to the address resource inside the contact belonging to a user.

### Returning Representations \(Data\)

REST does not dictate the format you use to represent your data. It can be JSON, XML, WDDX, plain text, a binary file or something else of your choosing.

#### Handler Return Data

The most common way to return data from your handlers is to simply return it. This leverages the [auto marshalling](../../the-basics/event-handlers/rendering-data.md) capabilities of ColdBox, which will detect the return variables and marshal accordingly:

* `String` =&gt; HTML
* `Complex` =&gt; JSON

```java
function index( event, rc, prc ) {
    // List all users
    return userService.getUserList();
}

function 404( event, rc, prc ) {
    return "<h1>Page not found</h1>";
}
```

{% hint style="info" %}
This approach allows the user to render back any string representation and be able to output any content type they like.
{% endhint %}

#### renderData\(\)

The next most common way to return data from your handler's action is to use the Request Context `renderData()` method. It takes complex data and turns it into a [string representation](../../the-basics/event-handlers/rendering-data.md). Here are some of the most common formats supported by `event.renderData()`:

* XML
* JSON/JSONP
* TEXT
* WDDX
* PDF
* Custom

```javascript
// xml marshalling
function getUsersXML( event, rc, prc ){
    var qUsers = getUserService().getUsers();
    event.renderData( type="XML", data=qUsers );
}
//json marshalling
function getUsersJSON( event, rc, prc ){
    var qUsers = getUserService().getUsers();
    event.renderData( type="json", data=qUsers );
}
// pdf marshalling
function getUsersAsPDF( event, rc, prc ){
    prc.user = userService.getUser( rc.id );
    event.renderData( data=renderView( "users/pdf" ), type="pdf" );
}
// Multiple formats
function listUsers( event, rc, prc ){
    prc.users = userService.getAll();
    event.renderData( data=prc.users, formats="json,xml,html,pdf" );
}
```

#### Format Detection

Many APIs allow the user to choose the format they want back from the endpoint. ColdBox will inspect the `Accepts` header to determine the right format to use by default or the URI for an extension.

Another way to do this is by appending a file `extension` to the end of the URL:

```text
http://www.example.com/api/user.json
http://www.example.com/api/user.xml
http://www.example.com/api/user.text
```

ColdBox has built-in support for detecting an extension in the URL and will save it into the request collection in a variable called `format`. What's even better is that `renderData()` can find the format variable and automatically render your data in the appropriate way. All you need to do is pass in a list of valid rendering formats and `renderData()` will do the rest.

```javascript
function index( event, rc, prc ) {
    var qUsers = getUserService().getUsers();
    // Correct format auto-detected from the URL
    event.renderData( data=qUsers, formats="json,xml,text" );
}
```

#### Status Codes

Status codes are a core concept in HTTP and REST APIs use them to send messages back to the client. Here are a few sample REST status codes and messages.

* `200` - OK - Everything is hunky-dory
* `201` - Created - The resource was created successfully
* `202` - Accepted - A 202 response is typically used for actions that take a long while to process.  It indicates that the request has been accepted for processing, but the processing has not been completed
* `400` - Bad Request - The server couldn't figure out what the client was sending it
* `401` - Unauthorized - The client isn't authorized to access this resource
* `404` - Not Found - The resource was not found
* `500` - Server Error - Something bad happened on the server

You can easily set status codes as well as the status message with `renderData()`. HTTP status codes and messages are not part of the response body. They live in the HTTP header.

```javascript
function view( event, rc, prc ){
    var qUser = getUserService().getUser( rc.userID );
    if( qUser.recordCount ) {
        event.renderData( type="JSON", data=qUser );
    } else {
        event.renderData( type="JSON", data={}, statusCode=404, statusMessage="User not found");
    }
}

function save( event, rc, prc ){
    // Save the user represented in the request body
    var userIDNew = userService.saveUser( event.getHTTPContent() );
    // Return back the new userID to the client
    event.renderData( type="JSON", data={ userID = userIDNew }, statusCode=201, statusMessage="We have created your user");
}
```

Status codes can also be set manually by using the `event.setHTTPHeader()`method in your handler.

```javascript
function worldPeace( event, rc, prc ){
    event.setHTTPHeader( statusCode=501, statusText='Not Implemented' );
    return 'Try back later.';
}
```

#### Caching

One of the great benefits of building your REST API on the ColdBox platform is tapping into awesome features such as event caching. Event caching allows you to cache the entire response for a resource using the incoming `FORM` and `URL` variables as the cache key. To enable event caching, set the following flag to true in your ColdBox config: `Coldbox.cfc`:

{% code title="config/ColdBox.cfc" %}
```java
coldbox.eventCaching = true;
```
{% endcode %}

Next, simply add the `cache=true` annotation to any action you want to be cached. That's it! You can also get fancy, and specify an optional `cacheTimeout` and `cacheLastAccesstimeout` \(in minutes\) to control how long to cache the data.

```javascript
// Cache for default timeout
function showEntry( event, rc, prc ) cache="true" {
    prc.qEntry = getEntryService().getEntry( event.getValue( 'entryID', 0 ) );        
    event.renderData( type="JSON", data=prc.qEntry );
}

// Cache for one hour, or 20 minutes from the last time accessed.
function showEntry( event, rc, prc ) cache="true" cacheTimeout="60" cacheLastAccessTimeout="20" {
    prc.qEntry = getEntryService().getEntry( event.getValue( 'entryID', 0 ) );        
    event.renderData( type="JSON", data=prc.qEntry );
}
```

{% hint style="info" %}
Data is stored in CacheBox's `template` cache. You can configure this cache to store its contents anywhere including a Couchbase cluster!
{% endhint %}

### Auto-Deserialization of JSON Payloads

If you are working with any modern JavaScript framework, this feature is for you. ColdBox on any incoming request will inspect the HTTP Body content and if the payload is JSON, it can deserialize it for you and if it is a structure/JS object, it will append itself to the request collection for you. So if we have the following incoming payload:

```javascript
{
    "name" : "Jon Clausen",
    "type" : "awesomeness",
    "data" : [ 1,2,3 ]
}
```

The request collection will have 3 keys for **name**, **type** and **data** according to their native CFML type.

{% hint style="warning" %}
Because of some issues with backwards compatibility you have to **enable** this feature in your Coldbox config: `coldbox.jsonPayloadToRC = true`
{% endhint %}

### Security

Adding authentication to an API is a common task and while there is no standard for REST, ColdBox supports just about anything you might need.

#### Requiring SSL

To prevent man-in-the-middle attacks or HTTP sniffing, we recommend your API require SSL. \(This assumes you have purchased an SSL Cert and installed it on your server\). When you define your routes, you can add `withSSL()` and ColdBox will only allow those routes to be accessed securely

```javascript
// Secure Route
route( "/api/user/:userID" )
    .withSSL()
    .withAction( {
        GET    = 'view',
        POST   = 'save',
        PUT    = 'save',
        DELETE = 'remove'
    } )
    .toHandler( "api.user" );
```

If your client is capable of handling cookies \(like a web browser\), you can use the session or client scopes to store login details. Generally speaking, your REST API should be stateless, meaning nothing is stored on the server after the request completes. In this scenario, authentication information is passed along with every request. It can be passed in HTTP headers or as part of the request body. How you do this is up to you.

{% hint style="info" %}
Another approach to force SSL for all routes is to create an [interceptor](../../getting-started/configuration/coldbox.cfc/configuration-directives/interceptors.md) that listens to the request and inspects if ssl is enabled.
{% endhint %}

#### Basic HTTP Auth

One of the simplest and easiest forms of authentication is Basic HTTP Auth. Note, this is not the most robust or secure method of authentication and most major APIs such as Twitter and FaceBook have all moved away from it. In Basic HTTP Auth, the client sends a header called `Authorization` that contains a base 64 encoded concatenation of the username and password.

You can easily get the username and password using `event.getHTTPBasicCredentials()`.

```javascript
function preHandler( event, action, eventArguments ){
    var authDetails = event.getHTTPBasicCredentials();
    if( !securityService.authenticate( authDetails.username, authDetails.password ) ) {
        event.renderData( type="JSON", data={ message = 'Please check your credentials' }, statusCode=401, statusMessage="You're not authorized to do that");
    }
}
```

#### Custom

The previous example put the security check in a `preHandler()` method which will get automatically [run prior to each action in that handler](../../the-basics/event-handlers/interception-methods/pre-advices.md). You can implement a broader solution by tapping into any of the [ColdBox interception](../../getting-started/configuration/coldbox.cfc/configuration-directives/interceptors.md) points such as `preProcess` which is announced at the start of every request.

{% hint style="success" %}
Remember interceptors can include an `eventPattern` annotation to limit what ColdBox events they apply to.
{% endhint %}

In addition to having access to the entire request collection, the event object also has handy methods such as `event.getHTTPHeader()` to pull specific headers from the HTTP request.

**/interceptors/APISecurity.cfc**

```javascript
/**
* This interceptor secures all API requests
*/
component{
    // This will only run when the event starts with "api."
    function preProcess( event, interceptData, buffer ) eventPattern = '^api\.' {
        var APIUser = event.getHTTPHeader( 'APIUser', 'default' );

        // Only Honest Abe can access our API
        if( APIUser != 'Honest Abe' ) {
            // Every one else will get the error response from this event
            event.overrideEvent( 'api.general.authFailed' );
        }
    }
}
```

Register the interceptor with ColdBox in your `ColdBox.cfc`:

{% code title="config/ColdBox.cfc" %}
```javascript
interceptors = [
  { class="interceptors.APISecurity" }
];
```
{% endcode %}

As you can see, there are many points to apply security to your API. One not covered here would be to tap into [WireBox's AOP](https://wirebox.ortusbooks.com/aspect-oriented-programming/aop-intro) and place your security checks into an advice that can be bound to whatever API method you need to be secured.

#### Restricting HTTP Verbs

In our route configuration we mapped HTTP verbs to handlers and actions, but what if users try to access resources directly with an invalid HTTP verb? You can easily enforce valid verbs \(methods\) by adding `this.allowedMethods` at the top of your handler. In this handler the `list()` method can only be accessed via a GET, and the `remove()` method can only be accessed via POST and DELETE.

```javascript
component{

    this.allowedMethods = { 
        remove = "POST,DELETE",
        list   = "GET"
    };

    function list( event, rc, prc ){}

    function remove( event, rc, prc ){}
}
```

The key is the name of the action and the value is a list of allowed HTTP methods. If the action is not listed in the structure, then it means allow all. If the request action HTTP method is not found in the list then it throws a 405 exception. You can catch this scenario and still return a properly-formatted response to your clients by using the `onError()` or the `onInvalidHTTPMethod()` convention in your handler or an exception handler which applies to the entire app.

### Error Handling

ColdBox REST APIs can use all the same error faculties that an ordinary ColdBox application has. We'll cover three of the most common ways here.

#### Handler `onError()`

If you create a method called `onError()` in a handler, ColdBox will automatically call that method for runtime errors that occur while executing any of the actions in that handler. This allows for localized error handling that is customized to that resource.

```javascript
// error uniformity for resources
function onError( event, rc, prc, faultaction, exception ){
    prc.response = getModel("ResponseObject");

    // setup error response
    prc.response.setError(true);
    prc.response.addMessage("Error executing resource #arguments.exception.message#");

    // log exception
    log.error( "The action: #arguments.faultaction# failed when requesting resource: #arguments.event.getCurrentRoutedURL()#", getHTTPRequestData() );

    // display
    arguments.event.setHTTPHeader(statusCode="500",statusText="Error executing resource #arguments.exception.message#")
        .renderData( data=prc.response.getDataPacket(), type="json" );
}
```

#### Handler `onInvalidHTTPMethod()`

If you create a method called `onInvalidHTTPMethod()` in a handler, ColdBox will automatically call that method whenever an action is trying to be executed with an invalid HTTP verb. This allows for localized error handling that is customized to that resource.

```javascript
/**
* on invalid http verbs
*/
function onInvalidHTTPMethod( event, rc, prc, faultAction, eventArguments ){
    // Log Locally
    log.warn( "InvalidHTTPMethod Execution of (#arguments.faultAction#): #event.getHTTPMethod()#", getHTTPRequestData() );
    // Setup Response
    prc.response = getModel( "Response" )
        .setError( true )
        .setErrorCode( 405 )
        .addMessage( "InvalidHTTPMethod Execution of (#arguments.faultAction#): #event.getHTTPMethod()#" )
        .setStatusCode( 405 )
        .setStatusText( "Invalid HTTP Method" );
    // Render Error Out
    event.renderData( 
        type        = prc.response.getFormat(),
        data         = prc.response.getDataPacket(),
        contentType = prc.response.getContentType(),
        statusCode     = prc.response.getStatusCode(),
        statusText     = prc.response.getStatusText(),
        location     = prc.response.getLocation(),
        isBinary     = prc.response.getBinary()
    );
}
```

#### Global Exception Handler

The global exception handler will get called for any runtime errors that happen anywhere in the typical flow of your application. This is like the `onError()` convention but covers the entire application. First, configure the event you want called in the `ColdBox.cfc` config file. The event must have the handler plus action that you want called.

{% code title="config/ColdBox.cfc" %}
```javascript
coldbox = {
    ...
    exceptionHandler = "main.onException"
    ...
};
```
{% endcode %}

Then create that action and put your exception handling code inside. You can choose to do error logging, notifications, or custom output here. You can even run other events.

```javascript
// /handlers/main.cfc
component {
    function onException( event, rc, prc ){
        // Log the exception via LogBox
        log.error( prc.exception.getMessage() & prc.exception.getDetail(), prc.exception.getMemento() );

        // Flash where the exception occurred
        flash.put("exceptionURL", event.getCurrentRoutedURL() );

        // Relocate to fail page
        setNextEvent("main.fail");
    }
}
```

### ColdBox Relax

![](https://www.ortussolutions.com/__media/relax-185-logo.png)

ColdBox Relax is a set of ReSTful Tools For Lazy Experts. We pride ourselves in helping you \(the developer\) work smarter and ColdBox Relax is a tool to help you document your projects faster. ColdBox Relax provides you with the necessary tools to automagically model, document and test your ReSTful services. One can think of ColdBox Relax as a way to describe ReSTful web services, test ReSTful web services, monitor ReSTful web services and document ReSTful web services–all while you relax!

```bash
install relax --saveDev
```

**Please note:** Installing relax via CommandBox installs without the examples, if required you will need to obtain the examples from the Relax Github repo here: [https://github.com/ColdBox/coldbox-relax](https://github.com/ColdBox/coldbox-relax)

To install the examples place them into the models directory in a subdirectory called 'resources' \(as per the Github repo\), then add the following `relax` structure to your `Coldbox.cfc` file:

```javascript
relax = {
    // The location of the relaxed APIs, defaults to models.resources
    APILocation = "models.resources",
    // Default API to load, name of the directory inside of resources
    defaultAPI = "myapi",
    // History stack size, the number of history items to track in the RelaxURL
    maxHistory = 10
};
```

You can then visit `http://[yourhost]/relax` to view Relax.

You can read more about Relax on the Official Relax Doc page \([http://www.ortussolutions.com/products/relax](http://www.ortussolutions.com/products/relax)\) or at it's repository: [https://github.com/coldbox/coldbox-relax](https://github.com/coldbox/coldbox-relax)

Ref Card - [https://github.com/ColdBox/cbox-refcards/raw/master/ColdBox REST APIs/ColdBox-REST-APIs-Refcard.pdf](https://github.com/ColdBox/cbox-refcards/raw/master/ColdBox%20REST%20APIs/ColdBox-REST-APIs-Refcard.pdf)

## Application Templates

We have a Github organization called [coldbox-templates](https://github.com/coldbox-templates) that has a great collection of application startup templates. You can collaborate with the existing ones or send us new ones to add.

### CommandBox Integration

The `coldbox create app` command has integration to our application templates via the `skeleton` argument. This can be the name of each of the templates in our repositories or you can use the following alternatives:

* Name of template in our organization: `advanced,simple,super-simple,rest, rest-hmvc, vuejs, etc`
* A name of a ForgeBox entry: `cbtemplate-advanced-script,cbtemplate-simple`
* A Github shortcut: `github-username/repo`
* An HTTP/S URL to a zip file containing a template: [http://myapptemplates.com/template.zip](http://myapptemplates.com/template.zip)
* A folder containing a template: `/opt/shared/templates/my-template`
* A zip file containing the template: `/opt/shared/templates/my-template.zip`

## ColdBox Exception Handling

ColdBox provides you with several ways to handle different types of exceptions during a typical ColdBox request execution:

* Global exception handler
* Global onException interceptor
* Global invalid event handler
* Global onInvalidEvent interceptor
* Global missing template handler
* Handler `onMissingAction()`
* Handler `onError()`
* Handler `onInvalidHTTPMethod`

### Global Exception Handler

The global exception handler will manage any runtime exception that occurs during the flow of a typical ColdBox request execution. This could be an exception at the handler, model, or view levels. This feature is activated by configuring the `coldbox.exceptionhandler` setting in your configuration `ColdBox.cfc`. The value of the setting is the event that will act as your global exception handler.

```javascript
coldbox = {
    ...
    exceptionHandler = "main.onException"
    ...
};
```

This event can the be used for logging the exception, relocating to a fail page or aborting the request. By default once the exception handler executes, ColdBox will continue the request by presenting the user the default ColdBox error page or a customized error template of your choosing via the `coldbox.customErrorTemplate` setting. ColdBox will also place an object in the **private** request collection called `exception`. This object models the entire exception and request data that created the exception. You can then use it to get any information needed to handle the exception.

```javascript
function onException(event,rc,prc){
    // Log the exception via LogBox
    log.error( prc.exception.getMessage() & prc.exception.getDetail(), prc.exception.getMemento() );

    // Flash where the exception occurred
    flash.put("exceptionURL", event.getCurrentRoutedURL() );

    // Relocate to fail page
    setNextEvent("main.fail");
}
```

> **Caution** Please note that if you set a view for rendering or renderdata in this exception handler, nothing will be rendered. The framework is in exception mode and will not allow any custom renderings or views as that could also produce exceptions as well.

### Global onException Interceptor

This is a standard ColdBox interception point that can be used to intercept whenever a global exception has occurred in the system. The interceptor call is actually made before the global exception handler, any automatic logging or presentation of the error page to the user. This is the first line of defense towards exceptions. You might be asking yourself, what is the difference between writing an `onException()` interceptor and just a simple exception handler? Well, the difference is in the nature of the design.

Interceptors are designed to be decoupled classes that can react to announced events, thus an event-driven approach. You can have as many CFCs listening to the `onException` event and react accordingly without them ever knowing about each other and doing one job and one job only. This is a much more flexible and decoupled approach than calling a single event handler where you will procedurally decide what happens in an exception.

```javascript
component extends="coldbox.system.Interceptor"{

    function onException(event, interceptData){
        // Get the exception
        var exception = arguments.interceptData.exception;

        // Do some logging only for some type of error and relocate
        if( exception.type eq "myType" ){
            log.error( exception.message & exception.detail, exception );
            // relocate
            setNextEvent( "page.invalidSave" );
        }
    }
}
```

Also remember that you need to register the interceptor in your configuration file or dynamically so ColdBox knows about it:

```javascript
interceptors = [

    // Register exception handler
    { class = "interceptors.ExceptionHandler", properties = {} }

];
```

> **Caution** Remember that the `onException()` interceptors execute before the global exception handler, any automatic error logging and presentation of the core or custom error templates.

### Global Invalid Event Handler

The global invalid event handler allows you to configure an event to execute whenever ColdBox detects that the requested event does not exist. This is a great way to present the user with page not found exceptions and 404 error codes. The setting is called `coldbox.invalidEventHandler` and can be set in your configuration `ColdBox.cfc`. The value of the setting is the event that will handle these missing events.

```javascript
coldbox = {
    ...
    invalidEventHandler = "main.pageNotFound"
    ...
};
```

Please note the importance of setting a 404 header as this identifies to the requester that the requested event does not exist in your system. Also note that if you do not set a view for rendering or render data back, the request might most likely fail as it will try to render the implicit view according to the incoming event.

ColdBox will also place the invalid event requested in the **private** request collection with the value of `invalidevent`.

```javascript
function pageNotFound(event,rc,prc){
    // Log a warning
    log.warn( "Invalid page detected: #prc.invalidEvent#");

    // Do a quick page not found and 404 error
    event.renderData( data="<h1>Page Not Found</h1>", statusCode=404 );

    // Set a page for rendering and a 404 header
    event.setView( "main/pageNotFound" ).setHTTPHeader( "404", "Page Not Found" );
}
```

### Global `onInvalidEvent` Interceptor

This is a standard ColdBox interception point that can be used to intercept whenever a requested event in your application was invalid. You might be asking yourself, what is the difference between writing an `onInvalidEvent()` interceptor and just a simple invalid event handler? Well, the difference is in the nature of the design.

Interceptors are designed to be decoupled classes that can react to announced events, thus an event-driven approach. You can have as many CFCs listening to the `onInvalidEvent` event and react accordingly without them ever knowing about each other and doing one job and one job only. This is a much more flexible and decoupled approach than calling a single event handler where you will procedurally decide what happens in an invalid event.

The `interceptData` argument receives the following variables:

* `invalidEvent` : The invalid event string
* `ehBean` : The internal ColdBox event handler bean
* `override` : A boolean indicator that you overwrote the event

You must tell ColdBox that you want to override the invalid event \(`override = true`\) and you must set in the `ehBean` to tell ColdBox what event to execute:

```javascript
component extends="coldbox.system.Interceptor"{

    function onInvalidEvent(event, interceptData){
        // Log a warning
        log.warn( "Invalid page detected: #arguments.interceptData.invalidEvent#");

        // Set the invalid event to run
        arguments.interceptData.ehBean
            .setHandler("Main")
            .setMethod("pageNotFound");

        // Override
        arguments.interceptData.override = true;
    }
}
```

Also remember that you need to register the interceptor in your configuration file or dynamically so ColdBox knows about it:

```javascript
interceptors = [

    // Register invalid event handler
    { class = "interceptors.InvalidHandler", properties = {} }

];
```

### Global Missing Template Handler

The global missing template handler allows you to configure an event to execute whenever ColdBox detects a request to a non-existent CFML page. This is a great way to present the user with page not found exceptions or actually use it to route the request in a dynamic matter, just like if those pages existed on disk. The setting is called `coldbox.missingTemplateHandler` and can be set in your configuration `ColdBox.cfc`. The value of the setting is the event that will handle these missing pages.

> **Info** Note that in order for this functionality to work the method `onMissingTemplate()` must exist in the `Application.cfc` with the default ColdBox handler code.

```javascript
coldbox = {
    ...
    missingTemplateHandler = "main.missingTemplate"
    ...
};
```

ColdBox will place the path to the requested page as a request collection variable called `missingTemplate`, which you can parse and route or just use.

```javascript
function missingTemplate(event,rc,prc){
    // Log a warning
    log.warn( "Missing page detected: #rc.missingTemplate#");

    // Do a quick page not found and 404 error
    event.renderData( data="<h1>Page Not Found</h1>", statusCode=404 );

    // Set a page for rendering and a 404 header
    event.setView( "main/pageNotFound" ).setHTTPHeader( "404", "Page Not Found" );
}
```

### Handler `onMissingAction()`

This approach allows you to intercept at the handler level when someone requested an action \(method\) that does not exist in the specified handler. This is really useful when you want to respond to dynamic requests like `/page/contact-us, /page/hello`, where page points to a `Page.cfc` and the rest of the URL will try to match to an action that does not exist. You can then use that portion of the URL to lookup a dynamic record. However, you can also use it to detect when invalid actions are sent to a specific handler.

```javascript
function onMissingAction(event,rc,prc,missingAction,eventArguments){

    // lookup the missing action in our mini cms
    prc.page = pageService.findBySlug( arguments.missingAction );

    if( !prc.page.isPersisted() ){
        prc.missingPage = arguments.missingAction;
        event.setView( "page/notFound" );
    }

    // Else present the page in a dynamic layout
    event.setView( view="page/display", layout=prc.page.getLayout() );

}
```

### Handler `onError()`

This approach allows you to intercept at the handler level whenever a runtime exception has occurred. This is a great approach when creating a family of event handlers and you create a base handler with the `onError()` defined in it. We have found tremendous success with this approach when building ColdBox RESTFul services in order to provide uniformity for all RESTFul handlers.

> _\*Caution_ Please note that this only traps runtime exceptions, compiler exceptions will bubble up to a global exception handler or interceptor.

```javascript
// error uniformity for resources
function onError(event,rc,prc,faultaction,exception){
    prc.response = getInstance("ResponseObject");

    // setup error response
    prc.response.setError(true);
    prc.response.addMessage("Error executing resource #arguments.exception.message#");

    // log exception
    log.error( "The action: #arguments.faultaction# failed when requesting resource: #arguments.event.getCurrentRoutedURL()#", getHTTPRequestData() );

    // display
    arguments.event.setHTTPHeader(statusCode="500",statusText="Error executing resource #arguments.exception.message#")
        .renderData( data=prc.response.getDataPacket(), type="json" );
}
```

### Handler `onInvalidHTTPMethod()`

This approach allows you to intercept at the handler level whenever an action execution is requested with an invalid HTTP verb. This is a great approach when creating a family of event handlers and you create a base handler with the `onInvalidHTTPMethod()` defined in it. We have found tremendous success with this approach when building ColdBox RESTFul services in order to provide uniformity for all RESTFul handlers.

```javascript
/**
* on invalid http verbs
*/
function onInvalidHTTPMethod( event, rc, prc, faultAction, eventArguments ){
    // Log Locally
    log.warn( "InvalidHTTPMethod Execution of (#arguments.faultAction#): #event.getHTTPMethod()#", getHTTPRequestData() );
    // Setup Response
    prc.response = getInstance( "Response" )
        .setError( true )
        .setErrorCode( 405 )
        .addMessage( "InvalidHTTPMethod Execution of (#arguments.faultAction#): #event.getHTTPMethod()#" )
        .setStatusCode( 405 )
        .setStatusText( "Invalid HTTP Method" );
    // Render Error Out
    event.renderData( 
        type        = prc.response.getFormat(),
        data         = prc.response.getDataPacket(),
        contentType = prc.response.getContentType(),
        statusCode     = prc.response.getStatusCode(),
        statusText     = prc.response.getStatusText(),
        location     = prc.response.getLocation(),
        isBinary     = prc.response.getBinary()
    );
}
```

## Debugging ColdBox Apps

We have a special module just for helping you monitor and debug ColdBox applications. To install, fire up CommandBox and install it as a dependency in your application:

```bash
install cbdebugger --saveDev
```

Remember that this module is for development so use the `--saveDev` flag. This will install the ColdBox Debugger module for you, which will attach a debugger to the end of the request and give you lots of flexibility. Please read the instructions here in order to spice it up as you see fit: [https://github.com/ColdBox/cbox-debugger](https://github.com/ColdBox/cbox-debugger)

## Clearing the View Cache

### Introduction

If you use any of the view partial caching mechanisms in Coldbox either through `setView()` or`renderView()` you might be asking yourself:

> Is there an easy, programmatic way to remove a specific element from the view cache?

The answer is, of course! All view and event caching occurs in a cache provider called template and you can retrieve it like so from your handlers, layouts, views, plugins and interceptors:

```javascript
var cache = cachebox.getCache("template");
```

You can also use the WireBox injection DSL

```javascript
property name="cache" inject="cachebox:template">
```

### Clearing methods

There are a few methods that will help you clear views:

* `clearView(viewSnippet)` - Clear views with a snippet
* `clearAllViews(async)` - Clear all views
* `clearViewMulti(viewSnippets)` - Clear multiple view snippets with a list or array of snippets

```javascript
getCache( "template" ).clearView('home');
```

Very easy! Just send in what you need and it will be purged.

## Building a simple Basic HTTP Authentication Interceptor

### Introduction

In this recipe we will create a simple interceptor that will be in charge of challenging users with HTTP Basic Authentication. It features the usage of all the new RESTful methods in our Request Context that will make this interceptor really straightforward. We will start by knowing that this interceptor will need a security service to verify security, so we will also touch on this.

### The Interceptor

Our simple security interceptor will be intercepting at preProcess so all incoming requests are inspected for security. Remember that I will also be putting best practices in place and will be creating some unit tests and mocking for all classes. So check out our interceptor:

```javascript
/**
* Intercepts with HTTP Basic Authentication
*/
component {

    // Security Service
    property name="securityService" inject="id:SecurityService";


    void function configure(){
    }

    void function preProcess(event,struct interceptData){

        // Verify Incoming Headers to see if we are authorizing already or we are already Authorized
        if( !securityService.isLoggedIn() OR len( event.getHTTPHeader("Authorization","") ) ){

            // Verify incoming authorization
            var credentials = event.getHTTPBasicCredentials();
            if( securityService.authorize(credentials.username, credentials.password) ){
                // we are secured woot woot!
                return;
            };

            // Not secure!
            event.setHTTPHeader(name="WWW-Authenticate",value="basic realm=""Please enter your username and password for our Cool App!""");

            // secured content data and skip event execution
            event.renderData(data="<h1>Unathorized Access<p>Content Requires Authentication</p>",statusCode="401",statusText="Unauthorized")
                .noExecution();
        }

    }

}
```

As you can see it relies on a SecurityService model object that is being wired via:

```javascript
// Security Service
property name="securityService" inject="id:SecurityService";
```

Then we check if a user is logged in or not and if not we either verify their incoming HTTP basic credentials or if none, we challenge them by setting up some cool headers and bypass event execution:

```javascript
// Verify Incoming Headers to see if we are authorizing already or we are already Authorized
if( !securityService.isLoggedIn() OR len( event.getHTTPHeader("Authorization","") ) ){

    // Verify incoming authorization
    var credentials = event.getHTTPBasicCredentials();
    if( securityService.authorize(credentials.username, credentials.password) ){
        // we are secured woot woot!
        return;
    };

    // Not secure!
    event.setHTTPHeader(name="WWW-Authenticate",value="basic realm=""Please enter your username and password for our Cool App!""");

    // secured content data and skip event execution
    event.renderData(data="<h1>Unathorized Access<p>Content Requires Authentication</p>",statusCode="401",statusText="Unauthorized")
        .noExecution();
}
```

The renderData\(\) is essential in not only setting the 401 status codes but also concatenating to a noExecution\(\) method so it bypasses any event as we want to secure them.

### Interceptor Test

So to make sure this works, here is our Interceptor Test Case with all possibilities for our Security Interceptor.

```javascript
component extends="coldbox.system.testing.BaseInterceptorTest" interceptor="simpleMVC.interceptors.SimpleSecurity"{

    function setup(){
        super.setup();
        // mock security service
        mockSecurityService = getMockBox().createEmptyMock("simpleMVC.model.SecurityService");
        // inject mock into interceptor
        interceptor.$property("securityService","variables",mockSecurityService);
    }

    function testPreProcess(){
        var mockEvent = getMockRequestContext();

        // TEST A
        // test already logged in and mock authorize so we can see if it was called
        mockSecurityService.$("isLoggedIn",true).$("authorize",false);
        // call interceptor
        interceptor.preProcess(mockEvent,{});
        // verify nothing called
        assertTrue( mockSecurityService.$never("authorize") );

        // TEST B
        // test NOT logged in and NO credentials, so just challenge
        mockSecurityService.$("isLoggedIn",false).$("authorize",false);
        // mock incoming headers and no auth credentials
        mockEvent.$("getHTTPHeader").$args("Authorization").$results("")
            .$("getHTTPBasicCredentials",{username="",password=""})
            .$("setHTTPHeader");
        // call interceptor
        interceptor.preProcess(mockEvent,{});
        // verify authorize called once
        assertTrue( mockSecurityService.$once("authorize") );
        // Assert Set Header
        AssertTrue( mockEvent.$once("setHTTPHeader") );
        // assert renderdata
        assertEquals( "401", mockEvent.getRenderData().statusCode );

        // TEST C
        // Test NOT logged in With basic credentials that are valid
        mockSecurityService.$("isLoggedIn",false).$("authorize",true);
        // reset mocks for testing
        mockEvent.$("getHTTPBasicCredentials",{username="luis",password="luis"})
            .$("setHTTPHeader");
        // call interceptor
        interceptor.preProcess(mockEvent,{});
        // Assert header never called.
        AssertTrue( mockEvent.$never("setHTTPHeader") );
    }


}
```

As you can see from our A,B, anc C tests that we use MockBox to mock the security service, the request context and methods so we can build our interceptor without knowledge of other parts.

### Security Service

Now that we have our interceptor built and fully tested, let's build the security service.

```javascript
component accessors="true" singleton{

    // Dependencies
    property name="sessionStorage"     inject="coldbox:plugin:SessionStorage";

    /**
    * Constructor
    */
    public SecurityService function init(){

        variables.username = "luis";
        variables.password = "coldbox";

        return this;
    }

    /**
    * Authorize with basic auth
    */
    function authorize(username,password){

        // Validate Credentials, we can do better here
        if( variables.username eq username AND variables.password eq password ){
            // Set simple validation
            sessionStorage.setVar("userAuthorized",  true );
            return true;
        }

        return false;
    }

    /**
    * Checks if user already logged in or not.
    */
    function isLoggedIn(){
        if( sessionStorage.getVar("userAuthorized","false") ){
            return true;
        }
        return false;
    }

}
```

We use a simple auth with luis and coldbox as the password. Of course, you would get fancy and store these in a database and have a nice object model around it. For this purposes was having a simple 1 time username and password. We also use the SessionStorage plugin in order to interact with the session scope with extra pizass and the most important thing: We can mock it!

### Security Service Test

```javascript
component extends="coldbox.system.testing.BaseModelTest" model="simpleMVC.model.SecurityService"{

    function setup(){
        super.setup();
        // init model
        model.init();
        // mock dependency
        mockSession = getMockBox().createEmptyMock("coldbox.system.plugins.SessionStorage");
        model.$property("sessionStorage","variables",mockSession);
    }

    function testauthorize(){

        // A: Invalid
        mockSession.$("setVar");
        r = model.authorize("invalid","invalid");
        assertFalse( r );
        assertTrue( mockSession.$never("setVar") );

        // B: Valid
        mockSession.$("setVar");
        r = model.authorize("luis","coldbox");
        assertTrue( r );
        assertTrue( mockSession.$once("setVar") );


    }

    function testIsLoggedIn(){

        // A: Invalid
        mockSession.$("getVar",false);
        assertFalse( model.isLoggedIn() );

        // B: Valid
        mockSession.$("getVar",true);
        assertTrue( model.isLoggedIn() );
    }

}
```

Again, you can see that now we use our `BaseModelTest` case and continue to use mocks for our dependencies.

### Interceptor Declaration

Open your `Coldbox.cfc` configuration file and add it.

```javascript
//Register interceptors as an array, we need order
interceptors = [
    //SES
    {class="coldbox.system.interceptors.SES"},
    // Security
    { class="interceptors.SimpleSecurity" }
];
```

We just add our Simple Security interceptor and reinit your application and you should be now using simple security.

