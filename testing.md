# Testing

## Testing Quick Start

ColdBox tightly integrates with [TestBox](http://www.ortussolutions.com/products/testbox), the Behavior Driven Development Testing Framework for ColdFusion \(CFML\). We can easily do unit and integration testing for our application. To start, let's install TestBox via CommandBox

```bash
install testbox --saveDev
```

Please note the `--saveDev` flag we used. This tells CommandBox that this dependency is only for testing purposes and not for distribution or production usage. Check out the `box.json` under the `devDependencies`.

### Test Harness

Every ColdBox application template comes with a pre-set testing harness under the `/tests` folder:

```text
Dir           0 Apr 25,2015 11:04:11 resources
Dir           0 Apr 25,2015 11:04:11 results
Dir           0 Apr 25,2015 11:04:11 specs
File        817 Apr 13,2015 18:04:34 Application.cfc
File        693 Jan 15,2015 14:01:18 runner.cfm
File       5164 Jan 15,2015 14:01:20 test.xml
```

Every harness has its own unique `Application.cfc` which must mimic your application's settings. It also comes with an HTML runner called `runner.cfm` and an ANT runner called `test.xml`. All your test bundles and specifications will go under the `specs` directory and in the appropriate sub-directory:

```text
Dir           0 Apr 27,2015 11:04:11 integration
Dir           0 Apr 25,2015 11:04:11 modules
Dir           0 Dec 16,2013 19:12:32 unit
File          0 Jan 15,2015 14:01:18 all_tests_go_here.txt
```

Under the `integration` tests you will find the test bundles that come with the application template and the ones we generated:

```text
File       1857 Apr 27,2015 11:04:11 helloTest.cfc
File       3236 Jan 21,2015 16:01:56 MainBDDTest.cfc
```

The `helloTest` BDD test bundle can be show below:

```javascript
/*******************************************************************************
*    Integration Test as BDD (CF10+ or Railo 4.1 Plus)
*
*    Extends the integration class: coldbox.system.testing.BaseTestCase
*
*    so you can test your ColdBox application headlessly. The 'appMapping' points by default to 
*    the '/root' mapping created in the test folder Application.cfc.  Please note that this 
*    Application.cfc must mimic the real one in your root, including ORM settings if needed.
*
*    The 'execute()' method is used to execute a ColdBox event, with the following arguments
*    * event : the name of the event
*    * private : if the event is private or not
*    * prePostExempt : if the event needs to be exempt of pre post interceptors
*    * eventArguments : The struct of args to pass to the event
*    * renderResults : Render back the results of the event
*******************************************************************************/
component extends="coldbox.system.testing.BaseTestCase" appMapping="/"{

    /*********************************** LIFE CYCLE Methods ***********************************/

    function beforeAll(){
        super.beforeAll();
        // do your own stuff here
    }

    function afterAll(){
        // do your own stuff here
        super.afterAll();
    }

    /*********************************** BDD SUITES ***********************************/

    function run(){

        describe( "hello Suite", function(){

            beforeEach(function( currentSpec ){
                // Setup as a new ColdBox request for this suite, VERY IMPORTANT. ELSE EVERYTHING LOOKS LIKE THE SAME REQUEST.
                setup();
            });

            it( "index", function(){
                var event = execute( event="hello.index", renderResults=true );
                // expectations go here.

                expect( event.getRenderedContent() ).toInclude( "values here" );

            });

            it( "echo", function(){
                var event = execute( event="hello.echo", renderResults=true );
                // expectations go here.
                expect( event.getRenderedContent() ).toInclude( "values here" );            
            });


        });

    }

}
```

### Executing The Runner

To execute your application template tests and the generated tests just browse to the URL: `http://127.0.0.1:{port}/tests/runner.cfm` and you will get a full integration report:

Everything is already pre-wired for you and ready for you to do full life-cycle integration testing. Don't believe me? Try it out, go change your `echo()` action to the following code and re-run your test:

```javascript
function echo(event,rc,prc){
    event.setFunkyView( "hello/echo" );
}
```

You will get an error now: **component \[coldbox.system.web.context.RequestContext\] has no function with name \[setFunkyView\]**. Fix it and re-run it. Ok, hold on to something..... You are now doing live integration testing my friend. Simple, but yet accomplishing.

### CommandBox Runner

You can also execute the runner via CommandBox. So let's tell CommandBox where your runner is located, just change the `{port}` to your port assigned.

```bash
package set testbox.runner=http://127.0.0.1:{port}/tests/runner.cfm
```

Then use the `testbox run` command:

```bash
testbox run
```

You will then execute your tests and get a text report from it. If you want CommandBox to watch for changes and re-execute the tests, then start a watcher:

```bash
testbox watch
```

### What's Next

We have a fully dedicated section on [testing](testing-coldbox-applications/), please visit it for in-depth information.

## Testing ColdBox Applications

One of the best things you can do when you develop software applications is TEST! I know nobody likes it, but hey, you need to do it right? With the advocation of frameworks today, you get all these great tools to build your software applications, but how do you test your framework code. ColdBox has revolutionized testing HMVC and framework code, since you can unit test your event handlers, interceptors, model objects and even do **integration testing and true BDD \(Behavior Driven Development\)** and test your entire application with no browser at all.

ColdBox has already all the hooks in place to provide Behavior and Test Driven Development via [TestBox](http://www.ortussolutions.com/products/testbox) and mocking/stubbing capabilities via MockBox.

{% hint style="info" %}
**TestBox** is a testing framework for ColdFusion \(CFML\) that is based on BDD \(Behavior Driven Development\) for providing a clean obvious syntax for writing tests. It also includes MockBox for mocking and stubbing.

**TestBox is included with all ColdBox templates.**
{% endhint %}

### Installing TestBox

TestBox already comes defined in the `box.json` of all ColdBox application templates. However, if you have a custom ColdBox app or a custom template, then you can easily install it via CommandBox:

```bash
// stable
install testbox --saveDev

// bleeding edge
install testbox@be --saveDev
```

{% hint style="info" %}
The --saveDev flag tells CommandBox to save TestBox locally only for testing purposes as it will not be used to send TestBox for production \([http://commandbox.ortusbooks.com/content/packages/dependencies.html](http://commandbox.ortusbooks.com/content/packages/dependencies.html)\)
{% endhint %}

Please also note that CommandBox ships with tons of commands for interacting with TestBox and ColdBox testing classes. You can explore them by issuing the following commands or viewing the latest CommandBox API Docs \([http://apidocs.ortussolutions.com/commandbox/current](http://apidocs.ortussolutions.com/commandbox/current)\)

```bash
# testbox namespace help
testbox help

# generations
testbox create help

# run tests
testbox run help

# Watch your files and execute the tests if they change
testbox watch

# coldbox creation help
coldbox create help
```

### Benefits

It might be that testing is tedious and takes time to get into the flow of Behavior/Test Driven Development. However, there are incredible benefits to testing:

* Can improve code quality
* Quick error discovery
* Code confidence via immediate verification
* Can expose high coupling
* Encourages refactoring to produce testable code
* Testing is all about behavior and expectations

**Our recommendation is to do BDD and integration testing first. Unit testing is important but it is more important to verify that your requirements are met.**

### Resources

* [Wikipedia](http://en.wikipedia.org/wiki/Unit_test) - [http://en.wikipedia.org/wiki/Unit\_test](http://en.wikipedia.org/wiki/Unit_test)
* [TestBox](http://ortussolutions.com/products/testbox) - [http://ortussolutions.com/products/testbox](http://ortussolutions.com/products/testbox)
* [ColdFusion Builder TestBox Extensions](http://forgebox.io/view/coldbox-platform-utilities) - [http://forgebox.io/view/coldbox-platform-utilities](http://forgebox.io/view/coldbox-platform-utilities)
* [Sublime TestBox Extension](https://github.com/lmajano/cbox-coldbox-sublime) - [https://github.com/lmajano/cbox-coldbox-sublime](https://github.com/lmajano/cbox-coldbox-sublime)
* [VSCode TestBox Extension](https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-testbox) - [https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-testbox](https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-testbox)

## Test Harness

Every ColdBox application template comes with a nice test harness inside of a `tests` folder.

Here is a breakdown of what it contains:

* `Application.cfc` - A unique application file for your test harness. It should mimic exactly the one in your root application folder
* `resources` - Some basic testing resources or any of your own testing resources
* `results` - Where automated results are archived
* `runner.cfm` - The HTML runner for your test bundles
* `specs` - Where you will be writing your testing bundle specs for integration testing, unit testing and module testing.
* `test.xml` - Your ANT runner

### Application.cfc

The `Application.cfc` for your tests is extremly important as it should mimic your applications real `Application.cfc`.

```javascript
component{

    // APPLICATION CFC PROPERTIES
    this.name                 = "ColdBoxTestingSuite" & hash(getCurrentTemplatePath());
    this.sessionManagement    = true;
    this.sessionTimeout       = createTimeSpan( 0, 0, 15, 0 );
    this.applicationTimeout   = createTimeSpan( 0, 0, 15, 0 );
    this.setClientCookies     = true;

    // Create testing mapping
    this.mappings[ "/tests" ] = getDirectoryFromPath( getCurrentTemplatePath() );
    // Map back to its root
    rootPath = REReplaceNoCase( this.mappings[ "/tests" ], "tests(\\|/)", "" );
    this.mappings["/root"]   = rootPath;

    public void function onRequestEnd() { 
        structDelete( application, "cbController" );
        structDelete( application, "wirebox" );
    }

}
```

Please note that we provide already a mapping to your root application via `/root`. We would recommend you add any ORM specs here or any other mappings here as well.

{% hint style="success" %}
**Tip:** Make sure all the same settings and configs from your root `Application.cfc` are replicated in your tests `Application.cfc`
{% endhint %}

## ColdBox Testing Classes

Before we begin our adventures in testing, let's review what classes does ColdBox give you for testing and where you can find them. From the diagram you can see that our pivot class for testing is the TestBox `BaseSpec` class.

From that super class we have our own ColdBox `BaseTestCase` which is our base class for any testing in ColdBox and the class used for Integration Testing. We then spawn several child classes for targeted testing of different objects in your ColdBox applications:

* `BaseTestCase` - Used for **Integration Testing**
* `BaseModelTest` - Used for model object unit testing
* `BaseInterceptorTest` - Used for interceptor unit testing
* `BaseHandlerTest` - Used for isolated handler unit testing

## Integration Testing

### Integration Testing

We will begin our adventure with integration testing. Integration testing allows you to test a real life request to your application without using a browser. Your test bundle CFC will load a new virtual application for you to test each specification under it; all aspects of your application are loaded: caching, dependency injection, AOP, etc. Then you can target an event to test and verify its behavior accordingly. First of all, these type of tests go in your `integration` folder of your test harness or in the specific module folder if you are testing modules.

#### Basics

Here are the basics to follow for integration testing:

* Create one test bundle CFC for each event handler you would like to test or base it off your BDD requirements
* Bundle CFC inherits from `coldbox.system.testing.BaseTestCase`
* The bundle CFC can have some annotations that tell the testing framework to what ColdBox application to connect to and test
* Execution of the event is done via the `execute()` method, which returns a request context object
* Most verifications and assertions are done via the contents of the request context object \(request collections\)

```text
/**
* My integration test bundle
*/
component extends="coldbox.system.testing.BaseTestCase" appMapping="/root"{

    /*********************************** LIFE CYCLE Methods ***********************************/

    function beforeAll(){
        super.beforeAll();
        // do your own stuff here
    }

    function afterAll(){
        // do your own stuff here
        super.afterAll();
    }

/*********************************** BDD SUITES ***********************************/

    function run(){

    }
```

We will explain later the life-cycle methods and the `run()` method where you will be writing your specs.

> **Hint** Please refer to our BDD primer to start: [http://testbox.ortusbooks.com/content/primers/bdd/index.html](http://testbox.ortusbooks.com/content/primers/bdd/index.html)

#### CommandBox Generation

Also remember that you can use CommandBox to generate integration tests with a few simple commands:

```text
coldbox create integration-test handler=main actions=index,save,run --open
# help
coldbox create integration-test help
```

> **Info** Please also note that whenever you create a handler, interceptor or model with CommandBox it will automatically create the integration or unit test for you.

## Test Annotations

Here are the annotations you can add to your testing bundle CFC.

| Annotation | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| `appMapping` | string | false | `/` | The application mapping of the ColdBox application to test. By defaults it maps to the root. Extermely important this mapping is a slash notation that points to the root of the ColdBox application to test. |
| `configMapping` | string | false | `{appMapping}/config/Coldbox.cfc` | The configuration file to load for this test, which by convention uses the same configuration as the application uses. This is a dot notation path to a configuration CFC. |
| `coldboxAppKey` | string | false | `cbController` | The named key of the ColdBox controller that will be placed in application scope for you to simulate the ColdBox application. Used mostly on advanced testing cases where you have altered the default application key. |
| `loadColdBox` | boolean | false | true | By default the base test case will load the virtual application into the `application`scope so all specs can execute |
| `unloadColdBox` | boolean | false | true | The base test case will unload the virtual application from the `application` scope after all specs have executed. |

**Examples**

```text
component extends="coldbox.system.testing.BaseTestCase" appMapping="/apps/MyApp"{}

component extends="coldbox.system.testing.BaseTestCase"
    appMapping="/apps/MyApp" 
    configMapping="apps.MyApp.test.resources.Config"{}
```

> **Caution** The `AppMapping` setting is the most important one. This is how your test connects to a location of a ColdBox application to test.

## Common Methods

The base test cases all inherit from TestBox so here are a few of the common methods you can find in every test bundle CFC \([http://testbox.ortusbooks.com/content/test\_bundles/index.html](http://testbox.ortusbooks.com/content/test_bundles/index.html)\):

```text
// quick assertion methods
assert( expression, [message] )
fail(message)

// bdd methods
beforeEach( body )
afterEach( body )
describe( title, body, labels, asyncAll, skip )
xdescribe( title, body, labels, asyncAll )
it( title, body, labels, skip )
xit( title, body, labels )
expect( actual )

// extensions
addMatchers( matchers )
addAssertions( assertions )

// runners
runRemote( testSpecs, testSuites, deubg, reporter );

// utility methods
console( var, top )
debug( var, deepCopy, label, top )
clearDebugBuffer()
getDebugBuffer()
print( message )
printLn( message )

// mocking methods
makePublic( target, method, newName )
querySim( queryData )
getmockBox( generationPath )
createEmptyMock( className, object, callLogging )
createMock( className, object, clearMethods, callLogging )
prepareMock( object, callLogging )
createStub( callLogging, extends, implements )
getProperty( target, name, scope, defaultValue )
```

## Life-Cycle Events

[![](https://github.com/ortus-docs/coldbox-docs/raw/master/.gitbook/assets/testing-lifecycle.png)](https://github.com/ortus-docs/coldbox-docs/blob/master/.gitbook/assets/testing-lifecycle.png)

ColdBox testing leverages TestBox's testing life-cycle events \([http://testbox.ortusbooks.com/content/life-cycle\_methods/index.html](http://testbox.ortusbooks.com/content/life-cycle_methods/index.html)\) in order to prepare the virtual ColdBox application, request context and then destroy it. For performance, a virtual application is loaded for all test cases contained within a test bundle CFC via the `beforeAll()` and destroyed under `afterAll()`.

```text
function beforeAll(){
    super.beforeAll();
    // do your own stuff here
}

function afterAll(){
    // do your own stuff here
    super.afterAll();
}
```

> **Important** If you override any of these methods and do not funnel the super call, then you might get cached or unexpected results.

### Improving Performance: Unloading ColdBox

The default for integration testing is that the virtual ColdBox application will be destroyed or unloaded in each test. To keep the virtual application accross requests you will need to use the `unloadColdBox=false` annotation or the `this.unloadColdBox=false` setting in your `beforeAll()` method. This will stop the testing classes from destroying ColdBox and improving performance.

```text
component extends="coldbox.system.testing.BaseTestCase" unloadColdBox=false{


    function beforeAll(){
        this.unloadColdBox = false;
        super.beforeAll();
        // do your own stuff here
    }
}
```

## Test Setup

Here is a spec written for you. Please note that in the `beforeEach()` life-cycle method you need to execute the `setup()` method will will setup a new ColdBox request for each spec you run.

```text
component extends="coldbox.system.testing.BaseTestCase" appMapping="/apps/MyApp"{

    function run(){

        describe( "Main Handler", function(){

            beforeEach(function( currentSpec ){
                // Setup as a new ColdBox request, VERY IMPORTANT. ELSE EVERYTHING LOOKS LIKE THE SAME REQUEST.
                setup();
            });

            it( "+homepage renders", function(){
                var event = execute( event="main.index", renderResults=true );
                expect(    event.getValue( name="welcomemessage", private=true ) ).toBe( "Welcome to ColdBox!" );
            });

        });
    }

}
```

## The execute\(\) Method

The `execute()` method is your way of making requests in to your ColdBox application. It can take the following parameters:

| Parameter Name | Parameter Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| event | string | `false` | `""` | The event name to execute. e.g., `"Main.index"`. Either an event or a route must be provided. |
| route | string | `false` | `""` | The route to execute. e.g., `"/login"`. The route parameter appends to your default SES Base Url set in `config/routes.cfm`. Either an event or a route must be provided. |
| queryString | string | `false` | `""` | Any parameters to be passed as a query string. These will be added to the Request Context for the test request. |
| private | boolean | `false` | `false` | If `true`, sets the event execution as private. |
| prePostExempt | boolean | `false` | `false` | If `true`, skips the pre- and post- interceptors. e.g., `preEvent`, `postHandler`, etc. |
| eventArguments | struct | `false` | `{}` | A collection of arguments to passthrough to the calling event handler method. |
| renderResults | struct | `false` | `false` | If true, then it will try to do the normal rendering procedures and store the rendered content in the RC as `cbox_rendered_content`. |
| withExceptionHandling | boolean | `false` | `false` | If true, then ColdBox will process any errors through the exception handling framework instead of just throwing the error. |

## The Handler To Test

### The Handler To Test

Let's use a sample event handler so we can test it:

```text
component extends="coldbox.system.EventHandler"{

    // Default Action
    function index(event,rc,prc){
        prc.welcomeMessage = "Welcome to ColdBox!";
        event.setView("main/index");
    }

    // Do something
    function doSomething(event,rc,prc){
        setNextEvent("main.index");
    }

    /************************************** IMPLICIT ACTIONS *********************************************/

    function onAppInit(event,rc,prc){

    }

    function onRequestStart(event,rc,prc){

    }

    function onRequestEnd(event,rc,prc){

    }

    function onSessionStart(event,rc,prc){

    }

    function onSessionEnd(event,rc,prc){
        var sessionScope = event.getValue("sessionReference");
        var applicationScope = event.getValue("applicationReference");
    }

    function onException(event,rc,prc){
        //Grab Exception From private request collection, placed by ColdBox Exception Handling
        var exception = prc.exception;
        //Place exception handler below:

    }

    function onMissingTemplate(event,rc,prc){
        //Grab missingTemplate From request collection, placed by ColdBox
        var missingTemplate = event.getValue("missingTemplate");

    }

}
```

#### Mocking Relocations

I can test this entire handler without me building any views yet. I can even test the relocations that happen via `setNextEvent()`. ColdBox will wire itself up with some mocking classes to intercept those relocations for you and place those values in the request collection for you so you can assert them. It creates a key called setnextevent in the request collection and any arguments passed to the method are also saved as keys with the following pattern:

```text
setnextevent_{argumentName}
```

> **Caution** Any relocation produced by the framework via the `setNextEvent` method will produce some variables in the request collection for you to verify relocations.

## The Integration Test

Here is the integration test for the `Main` handler.

```text
function run(){

    describe( "Main Handler", function(){

        beforeEach(function( currentSpec ){
            // Setup as a new ColdBox request, VERY IMPORTANT. ELSE EVERYTHING LOOKS LIKE THE SAME REQUEST.
            setup();
        });

        it( "+homepage renders", function(){
            var event = execute( event="main.index", renderResults=true );
            expect(    event.getValue( name="welcomemessage", private=true ) ).toBe( "Welcome to ColdBox!" );
        });

        it( "+doSomething relocates", function(){
            var event = execute( event="main.doSomething" );
            expect(    event.getValue( "setnextevent", "" ) ).toBe( "main.index" );
        });

        it( "+app start fires", function(){
            var event = execute( "main.onAppInit" );
        });

        it( "+can handle exceptions", function(){
            //You need to create an exception bean first and place it on the request context FIRST as a setup.
            var exceptionBean = createMock( "coldbox.system.web.context.ExceptionBean" )
                .init( erroStruct=structnew(), extramessage="My unit test exception", extraInfo="Any extra info, simple or complex" );

            // Attach to request
            getRequestContext().setValue( name="exception", value=exceptionBean, private=true );

            //TEST EVENT EXECUTION
            var event = execute( "main.onException" );
        });

        describe( "Request Events", function(){

            it( "+fires on start", function(){
                var event = execute( "main.onRequestStart" );
            });

            it( "+fires on end", function(){
                var event = execute( "main.onRequestEnd" );
            });

        });

        describe( "Session Events", function(){

            it( "+fires on start", function(){
                var event = execute( "main.onSessionStart" );
            });

            it( "+fires on end", function(){
                //Place a fake session structure here, it mimics what the handler receives
                URL.sessionReference = structnew();
                URL.applicationReference = structnew();
                var event = execute( "main.onSessionEnd" );
            });

        });


    });

}
```

## Handler Returning Results

### Handler Returning Results

If you have written event handlers that actually return data, then you will have to get the values from the request collection. The following are the keys created for you in the request collection.

| Key | Description |
| :--- | :--- |
| `cbox_handler_results` | The value returned from the event handler method. This key will NOT be created if the handler does not return any data. |

**The handler** \(`main.cfc`\)

```text
function list(event,rc,prc){
    return "Hola Luis";
}
```

**The spec**

```text
it( "+homepage renders", function(){
    var event = execute( event="main.list", renderResults=true );
    expect(    event.getValue( name="cbox_handler_results" ) ).toBe( "Hola Luis" );
});
```

## Testing Without Virtual Application

### Testing Without Virtual Application

The `BaseTestCase` leverages an internal virtual ColdBox application so you can do integration testing. Meaning that whenever you extend from the `BaseTestCase` the virtual ColdBox will be loaded. You can tell the testing framework to NOT load it by using the `this.loadColdBox` variable:

```text
component extends="coldbox.system.testing.BaseTestCase"{
    // Do not load the Virtual Application
    this.loadColdBox = false;
}
```

That's it! You will be able to use anything from the `BaseTestCase` but the virtual application will not be loaded.

* **Rendering Results**

### Rendering Results

The `execute()` method has an argument called `renderResults` which defaults to **false**. If you pass in **true** then ColdBox will go through the normal rendering procedures and save the results in a request collection variable called: `cbox_rendered_content` and expose to you a method in the request context called `getRenderedContent()`. It will even work with `renderData()` or if you are returning RESTful information. Then you can easily assert what the content would have been for an event.

```text
it( "can render users", function(){
   var event = execute( event="rest.api.users", renderResults=true );
   expect( event.getValue( "cbox_rendered_content" ) ).toBeJSON();
} );
```

#### `getRenderedContent()`

In testing mode, the ColdBox request context `event` object has a convenience method called `getRenderedContent()` that will give you the rendered content instead of you going directly to the request context and finding the `cbox_rendered_content` variable.

```text
it( "can render users", function(){
   var event = execute( event="rest.api.users", renderResults=true );
   expect( event.getRenderedContent() ).toBeJSON();
} );
```

## HTTP Method Mocking

### HTTP Method Mocking

There are times when building RESTFul web services that you need to mock the incoming HTTP verb. This is done via MockBox:

```text
function run(){

    describe( "Main Handler", function(){

      beforeEach(function( currentSpec ){
        setup();
      });

      it( "+homepage renders", function(){
        prepareMock( getRequestContext() )
            .$( "getHTTPMethod", "DELETE" );
        FORM.userID = 123;
        var event = execute( event = "user.delete" );
      });    
    });
}
```

## Interceptor Testing

You can test interceptors directly with no need of doing integration testing via the `BaseInterceptorTest`. This way you can unit test interceptors in isolation. All you need to do is the following:

```bash
coldbox create interceptor-test path=interceptors.Deploy points=onCreate --open
```

This testing support class will create your interceptor, and decorate with mocking capabilities via MockBox and mock all the necessary companion objects around interceptors. The following are the objects that are placed in the variables scope for you to use:

* interceptor : The target interceptor to test
* mockController : A mock ColdBox controller in use by the target interceptor
* mockRequestService : A mock request service object
* mockLogger : A mock logger class in use by the target interceptor
* mockLogBox : A mock LogBox class in use by the target interceptor
* mockFlash : A mock flash scope in use by the target interceptor

All of the mock objects are essentially the dependencies of interceptor objects. You have complete control over them as they are already mocked for you.

```javascript
/**
* The base interceptor test case will use the 'interceptor' annotation as the instantiation path to the interceptor
* and then create it, prepare it for mocking, and then place it in the variables scope as 'interceptor'. It is your
* responsibility to update the interceptor annotation instantiation path.
*/
component extends="coldbox.system.testing.BaseInterceptorTest" interceptor="interceptors.test"{

    /*********************************** LIFE CYCLE Methods ***********************************/

    function beforeAll(){
        // interceptor configuration properties, if any
        configProperties = {};
        // init and configure interceptor
        super.setup();
        // we are now ready to test this interceptor
    }

    function afterAll(){
    }

    /*********************************** BDD SUITES ***********************************/

    function run(){

        describe( "interceptors.test", function(){

            it( "should configure correctly", function(){
                interceptor.configure();
            });


        });

    }

}
```

## Model Object Testing

You can test all your model objects directly with no need of doing integration testing. This way you can unit test model objects very very easily using great mocking capabilities. All you need to do is the following:

```text
coldbox create model-test path=models.UserService methods=save,list,search --open
```

This testing support class will create your model object, and decorate with mocking capabilities via MockBox and create some mocking classes you might find useful in your model object unit testing. The following are the objects that are placed in the variables scope for you to use:

* model : The target model object to test
* mockLogger : A mock logger class
* mockLogBox : A mock LogBox class
* mockCacheBox : A mock Cache Factory class
* mockWireBox : A mock WireBox Injector class

> **Caution** We do not initialize your model objects for you. This is your job as you might need some mocking first.

Basic Setup

```javascript
/**
* The base model test case will use the 'model' annotation as the instantiation path
* and then create it, prepare it for mocking and then place it in the variables scope as 'model'. It is your
* responsibility to update the model annotation instantiation path and init your model.
*/
component extends="coldbox.system.testing.BaseModelTest" model="UserService"{

    /*********************************** LIFE CYCLE Methods ***********************************/

    function beforeAll(){
        // setup the model
        super.setup();        

        // init the model object
        model.init();
    }

    function afterAll(){
    }

    /*********************************** BDD SUITES ***********************************/

    function run(){

        describe( "UserService Suite", function(){

            it( "should save", function(){

            });

            it( "should search", function(){

            });

            it( "should list", function(){

            });


        });

    }

}
```

## Tips & Tricks

Here are some useful tips for you when doing testing with ColdBox Applications:

* If you are using relative paths in your application, you might encounter problems since the running application is different from the test application. Try to always use paths based on the application's `AppMapping`
* Always use `setNextEvent()` for relocations so they can be mocked
* Leverage `querySim()` for query mocking
* Leverage [MockBox](http://testbox.ortusbooks.com/content/mockbox/index.html) for mocking and stubbing
* Integration tests are NOT the same as handler tests. Handler tests will just test the handler CFC in isolation, so it will be your job to mock everything around it.
* You can extend the `coldbox.system.testing.BaseModelTest` to test any domain object
* The [ColdBox source code testing folder](https://github.com/ColdBox/coldbox-platform/tree/master/tests) has over 5,000 tests, mocking scripts and more for you to learn from

