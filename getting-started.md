# Getting Started

## Getting Started Guide

The ColdBox HMVC Platform is the de-facto enterprise-level HMVC framework for CFML developers. It's professionally backed, highly extensible, and productive. Getting started with ColdBox is quick and painless. The only thing you need to begin is [CommandBox](http://www.ortussolutions.com/products/commandbox), a command line tool for CFML developers.

This is a one-page introductory guide to ColdBox. If you are new to MVC or ColdBox, you can also leverage our [60 minute quick start guide](../for-newbies/60-minute-quick-start/) as well.

### IDE Tools

ColdBox has the following supported IDE Tools:

* Sublime - [https://packagecontrol.io/packages/ColdBox Platform](https://packagecontrol.io/packages/ColdBox%20Platform)
* VSCode - [https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-coldbox](https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-coldbox)
* CFBuilder - [https://www.forgebox.io/view/ColdBox-Platform-Utilities](https://www.forgebox.io/view/ColdBox-Platform-Utilities)

### Install CommandBox

You can read through our one-page [CommandBox Getting Started Guide](https://commandbox.ortusbooks.com/getting-started-guide). Or simply grab the CommandBox executable from the [download page](https://www.ortussolutions.com/products/commandbox#download) and double click it to run.

[http://www.ortussolutions.com/products/commandbox](http://www.ortussolutions.com/products/commandbox)

You should now be seeing a prompt that looks like this:

### Create A New Site

Now we're cooking with gas! Let's create a new ColdBox application. CommandBox comes with built-in commands for scaffolding out new sites as well as installing ColdBox and other libraries. We'll start by changing into an empty directory were we want our new app to live. If necessary, you can create a new folder.

```bash
CommandBox> mkdir playground --cd
```

Now let's ask CommandBox to create a new ColdBox app for us.

```bash
CommandBox> coldbox create app MyPlayground
```

{% hint style="success" %}
**Tip:** You can find many scaffolding templates for ColdBox in our Github organization: [github.com/coldbox-templates](https://github.com/coldbox-templates)

You can also issue a `coldbox create app help` command and get help for the creation command.
{% endhint %}

#### File/Folder Conventions

This command will place several new folders and files in your working directory. Let's run the `ls` command to view them.

```bash
CommandBox> ls
```

Here's a rundown of the important bits.

* **coldbox** - This is the ColdBox framework managed by CommandBox
* **config/Coldbox.cfc** - Your application configuration object
* **config/Router.cfc** - Your application URL Router
* **handlers** - Your controller layer, which in ColdBox they are called event handlers
* **layouts** - Your HTML layouts
* **models** - This holds your model CFCs 
* **modules** - This holds the CommandBox tracked modules
* **modules\_app** - This holds your app's modules
* **views** - Your HTML views will go here

### Start It Up

Now that our shiny new MVC app is ready to go, let's fire it up using the embedded server built into CommandBox. You don't need any other software installed on your PC for this to work. CommandBox has it all!

```bash
CommandBox> start --rewritesEnable
```

In a few seconds, a browser window will appear with your running application. This is a full server with access to the web administrator where you can add data sources, mappings, or adjust the server settings. Notice the handy icon added to your system tray as well. The `--rewritesEnable` flag will turn on some basic URL rewriting so we have nice, pretty URLs.

{% hint style="success" %}
**Tip:** If you are creating an app to run on any other server than the commandbox server, you will need to manually set up URL rewriting. More info here: [/the-basics/routing/requirements](../the-basics/routing/requirements/)
{% endhint %}

### Take A Look Around

ColdBox uses easy conventions to define the controllers and views in your app. Let's open up our main app controller in your default editor to have a looksie.

```bash
CommandBox> edit handlers/main.cfc
```

At the top, you'll see a function named "index". This represents the **default action** that runs for this controller, which in ColdBox land they are referred to as event handlers.

```javascript
// Default Action
function index(event,rc,prc){
    prc.welcomeMessage = "Welcome to ColdBox!";
    event.setView("main/index");
}
```

Now let's take a look in the `main/index` view. It's located int he `views` folder.

```bash
CommandBox> edit views/main/index.cfm
```

This line of code near the top of the view is what outputs the `prc.welcomeMessage` variable we set in the controller.

```markup
<h1>#prc.welcomeMessage#</h1>
```

Try changing the value being set in the handler and refresh your browser to see the change.

```javascript
prc.welcomeMessage = "This is my new welcome message";
```

### Building On

Let's define a new event handler now. Your controllers act as event handlers to respond to requests, REST API, or remote proxies.

Pull up CommandBox again and run this command.

```bash
CommandBox> coldbox create handler helloWorld index,add,edit,list
```

That's it! You don't need to add any special configuration to declare your handler. Now we have a new handler called `helloWorld` with actions `index`, `add`, `edit`, and `list`. The command also created a test case for our handler as well as stubbed-out views for each of the actions.

Now, let's re-initialize the framework to pick up our new handler by typing `?fwreinit=1` at the end of the URL.

Let's hit this new controller we created with a URL like so. Your port number will probably be different.

> 127.0.0.1:43272/helloWorld

{% hint style="info" %}
Normally the URL would have `index.cfm` before the `/helloWorld` bit, but our `--rewritesEnable` flag when we started the server makes this nicer URL possible.
{% endhint %}

### Install Packages

ColdBox's MVC is simple, but it's true power comes from the wide selection of modules you can install into your app to get additional functionality. You can checkout the full list of modules available on the Forgebox directory: [www.forgebox.io](https://www.forgebox.io).

> [forgebox.io/type/modules](http://forgebox.io/type/modules)

Here's some useful examples:

* **BCrypt** -- Industry-standard password hashing
* **cbdebugger** -- For debugging Coldbox apps
* **cbjavaloader** - For interacting with Java classes and libraries
* **cbMarkdown** - For writing in markdown
* **cbMessagebox** -- Display nice error/success messages
* **cborm** -- Awesome ORM Services
* **cb18n** -- For multilingual sites
* **cbt** - ColdBox templating language
* **cbValidation** - Back-end validation framework
* **qb** - Fluent query builder and schema builder
* **route-visualizer** - For visualizing your application routes

Install `cbmessagebox` from the CommandBox prompt like this:

```bash
CommandBox> install cbmessagebox
```

We can see the full list of packages by using the `list` command.

```bash
CommandBox> list
Dependency Hierarchy for myApp (0.0.0)
+-- cbmessagebox (1.0.0)
+-- coldbox (4.0.0)
```

Right now we can see that our app depends on `coldbox` and `cbmessagebox` to run. We'll use our new `cbmessagebox` module in a few minutes. But first, we'll create a simple Model CFC to round out our `MVC` app.

### Creating A Model

Models encapsulate the business logic your application. They can be services, beans, or DAOs. We'll use CommandBox to create a `GreeterService` in our new app with a `sayHello` method.

```bash
CommandBox> coldbox create model GreeterService sayHello --open
```

{% hint style="success" %}
**Tip:** The `--open` is a nice shortcut that opens our new model in our default editor after creating it.
{% endhint %}

Let's finish implementing the `sayHello()` method by adding this return statement and save the file.

We can also add the word `singleton` to the component declaration. This will tell **WireBox** to only create one instance of our service.

```javascript
component singleton {

    function sayHello(){
        return 'Hey, you sexy thing!';
    }

}
```

{% hint style="info" %}
What is WireBox?

WireBox is a dependency injection framework that is included with ColdBox. It will manage all object creations, persistence and assembling. You don't have to worry about using `new` or `createobject()` for CFCs anymore.
{% endhint %}

### Tie It All Together

Ok, let's open up that `helloWorld` handler we created a while back. Remember, you can hit tab while typing to auto-complete your file names.

```bash
CommandBox> edit handlers/helloWorld.cfc
```

We'll inject our `greeterService` and the `cbmessagebox` service into the handler by adding these properties to the top of `/handlers/helloWorld.cfc`.

{% hint style="info" %}
What is this magical injection? Injection is a way to get references of other objects placed in the `variables` scope of other objects. This makes your life easier as you don't have to be creating objects manually or even knowing where they exist.
{% endhint %}

This will put the instance of our services in the `variables` scope where we can access it in our action methods.

```javascript
component {

    property name='greeterService' inject='greeterService';
    property name='messageBox' inject='@cbmessagebox';

    ...
}
```

And now in our `index` method, we'll set the output of our service into an `info` message.

```javascript
function index( event, rc, prc ){
    messageBox.info( greeterService.sayHello() );
    event.setView( "helloWorld/index" );
}
```

One final piece. Open up the default layout located in `layouts/Main.cfm` and find the `#renderView()#`. Add this line right before it to render out the message box that we set in our handler.

```markup
#getInstance( 'messagebox@cbmessageBox').renderIt()#
<div class="container">#renderView()#</div>
```

Now hit your `helloWorld` handler one final time with `?fwreinit=1` in the URL to see it all in action! \(Again, your port number will most likely be different.

> 127.0.0.1:43272/helloWorld?fwreinit=1

### What's Next?

**Congratulations**! In a matter of minutes, you have created a full MVC application. You installed a community module from ForgeBox, created a new handler/view and tied in business logic from a service model.

As easy as that was, you're just scratching the surface of what ColdBox can do for you. Continue reading this book to learn more about:

* Environment-specific configuration
* Easy SES URL routing
* Tons of 3rd party modules
* Drop-in security system
* Sweet REST web service support

#### Getting Help

If you run into issues or just have questions, please jump on our [ColdBox Google Group](https://groups.google.com/forum/#!forum/coldbox) and our [Slack team](http://boxteam.herokuapp.com/) and ask away.

ColdBox is Professional Open Source under the Apache 2.0 license. We'd love to have your help with the product.

## Installation

**Welcome to the world of ColdBox!**

We are excited you are taking this development journey with us. Before we get started with ColdBox let's install CommandBox CLI, which will allow you to install/uninstall dependencies, start servers, have a REPL tool and much more.

### IDE Tools

ColdBox has the following supported IDE Tools:

* Sublime - [https://packagecontrol.io/packages/ColdBox Platform](https://packagecontrol.io/packages/ColdBox%20Platform)
* VSCode - [https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-coldbox](https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-coldbox)
* CFBuilder - [https://www.forgebox.io/view/ColdBox-Platform-Utilities](https://www.forgebox.io/view/ColdBox-Platform-Utilities)

### CommandBox CLI

The first step in our journey is to [install](https://commandbox.ortusbooks.com/content/setup/installation.html) CommandBox. [CommandBox](https://www.ortussolutions.com/products/commandbox) is a ColdFusion \(CFML\) Command Line Interface \(CLI\), REPL, Package Manager and Embedded Server. We will be using CommandBox for almost every excercise in this book and it will also allow you to get up and running with ColdFusion and ColdBox in a much speedier manner.

> **Note** : However, you can use your own ColdFusion server setup as you see fit. We use CommandBox as everything is scriptable and fast!

#### Download CommandBox

You can download CommandBox from the official site: [https://www.ortussolutions.com/products/commandbox\#download](https://www.ortussolutions.com/products/commandbox#download) and install in your preferred Operating System \(Windows, Mac, \*unix\). CommandBox comes in two flavors:

1. No Java Runtime \(30mb\)
2. Embedded Runtime \(80mb\)

So make sure you choose your desired installation path and follow the instructions here: [https://commandbox.ortusbooks.com/content/setup/installation.html](https://commandbox.ortusbooks.com/content/setup/installation.html)

#### Starting CommandBox

Once you download and expand CommandBox you will have the `box.exe` or `box` binary, which you can place in your Windows Path or \*Unix `/usr/bin` folder to have it available system wide. Then just open the binary and CommandBox will unpack itself your user's directory: `{User}/.CommandBox`. This happens only once and the next thing you know, you are in the CommandBox interactive shell!

We will be able to execute a-la-carte commands from our command line or go into the interactive shell for multiple commands. We recommend the interactive shell as it is faster and can remain open in your project root.

{% hint style="info" %}
All examples in this book are based on the fact of having an interactive shell open.
{% endhint %}

### Installing ColdBox

To get started open the CommandBox binary or enter the shell by typing `box` in your terminal or console. Then let's create a new folder and install ColdBox into a directory.

```bash
mkdir myapp --cd
install coldbox
```

CommandBox will resolve `coldbox` from ForgeBox \([www.forgebox.io](https://www.forgebox.io)\), use the **latest version** available, download and install it in this folder alongside a `box.json` file which represents your application package.

```text
Dir 0 Apr 25,2018 11:04:05 coldbox
File 112 Apr 25,2018 11:04:05 box.json
```

{% hint style="info" %}
You can also install the latest bleeding edge version by using the `coldbox@be` slug instead, or any previous version.
{% endhint %}

That's it! CommandBox can now track this version of ColdBox for you in this directory.

#### Scaffolding ColdBox Applications

CommandBox comes with a `coldbox create app` command that can enable you to create application skeletons using one of our official skeletons or by creating [your own](../digging-deeper/recipes/application-templates.md) application template:

* **Advanced** : A tag based advanced template
* **AdvancedScript**  \(`default`\): A script based advanced template
* **elixir** : A ColdBox Elixir based template
* **ElixirBower** : A ColdBox Elixir + Bower based template
* **ElixirVueJS** : A ColdBox Elixir + Vue.js based template
* **rest**: A RESTFul services template
* **rest-hmvc**: A RESTFul service built with modules
* **Simple** : A traditional simple template
* **SuperSimple** : The bare-bones template

{% hint style="success" %}
You can find many scaffolding templates for ColdBox in our Github organization: [github.com/coldbox-templates](https://github.com/coldbox-templates)

Type `coldbox create app` help in CommandBox to get tons of help for scaffolding apps.
{% endhint %}

### Uninstalling ColdBox

To uninstall ColdBox from this application folder just type `uninstall coldbox`.

### Updating ColdBox

To update ColdBox from a previous version, just type `update coldbox`.

## Conventions

The core conventions delineate the contract between ColdBox and you for file/directory locations and more. Below is a table of the core conventions:

### Directory/File Conventions

* **config/Coldbox.cfc** - Your application configuration object \(_optional_\)
* **config/Router.cfc** - Your application URL Router \(_optional_\)
* **handlers** - This holds the app's controllers
* **layouts** - Your HTML layouts
* **models** - This holds your app's CFCs 
* **modules** - This holds the CommandBox tracked modules
* **modules\_app** - This holds your app's modules
* **views** - Your HTML views will go here

### Execution Conventions

| **Convention** | **Default Value** | **Description** |
| :--- | :--- | :--- |
| Default Event | `main.index` | The default event to execute when no event is specified |
| Default Action | `index()` | The default action to execute in an event handler controller if none is specified |
| Default Layout | `layouts/Main.cfm` | The default system layout to use |

## Configuration

In order to create a ColdBox application you must adhere to some naming conventions and a pre-set directory structure. This is needed in order for ColdBox to find what it needs and to help you find modules more easily instead of configuring every single area of your application.

{% hint style="info" %}
ColdBox relies on conventions instead of configurations.
{% endhint %}

You can also configure many aspects of ColdBox, third-party modules and application settings in our programmtic configuration object: `ColdBox.cfc`.

### Reinitializing An Application

Please note that anytime you make any configuration changes or there are things in memory you wish to clear out, you will be using a URL action that will tell the ColdBox [Bootstrapper](bootstrapper-application.cfc.md) to reinitialize the application. This special URL variable is called `fwreinit` and can be any value or a specific password you setup in the [ColdBox configuration directive](coldbox.cfc/configuration-directives/).

```text
// reinit with no password
index.cfm?fwreinit=1

// reinit with password
index.cfm?fwreinit=mypass
```

You can also use CommandBox to reinit your application if you are using its embedded server:

```bash
CommandBox>coldbox reinit
```

## ColdBox.cfc

The ColdBox configuration CFC is the heart of your ColdBox application. It contains the initialization variables for your application and extra information used by third-party modules and ultimately how your application boots up. In itself, it is also an event listener or [ColdBox Interceptor](configuration-directives/interceptors.md), so it can listen to life-cycle events of your application.

This CFC is instantiated by ColdBox and decorated at runtime so you can take advantage of some dependencies.

* **Controller** : Reference to the main ColdBox Controller object \(`coldbox.system.web.Controller`\)
* **AppMapping** : The location of the application on the web server
* **LogBoxConfig** : A reference to a LogBox configuration object

### Configuration Storage

Once the application starts up, a reference to the instantiated configuration CFC will be stored in the configuration settings with the key `coldboxConfig`. You can then retrieve it later in your handlers, interceptors, modules, etc.

```javascript
// retrieve it
config = getSetting( 'coldboxConfig' );

// inject it
property name="config" inject="coldbox:setting:coldboxConfig";
```

### Configuration Interceptor

Another cool concept for the Configuration CFC is that it is also registered as a [ColdBox Interceptor](../../../digging-deeper/interceptors/) once the application starts up automatically for you. Create functions that will listen to application events:

```javascript
function preProcess( event, interceptData, buffer, rc, prc ){
    writeDump( 'I just hijacked your app!' );abort;
}
```

{% hint style="danger" %}
Note that the config CFC does not have the same variables mixed into it that a "normal" interceptor has. You can still access everything you need, but will need to get it from the `controller` in the variables scope.
{% endhint %}

```javascript
function preRender( event, interceptData, buffer, rc, prc ){
    controller.getWirebox().getInstance( 'loggerService' ).doSomething();
}
```

## Configuration Directives

The basic configuration object has 1 method for application configuration called `configure()` where you will place all your configuration directives and settings:

{% code title="ColdBox.cfc" %}
```javascript
/**
* A simple CFC that configures a ColdBox application.  You can even extend, compose, strategize and do your OO goodness.
*/
component{

    // Mandatory configuration method
    function configure(){
        coldbox = {

        };
    }

}
```
{% endcode %}

### Directives

Inside of this configuration method you will place several core and third-party configuration structures that can alter your application settings and behavior. Below are the core directives you can define:

| Directive | Type | Description |
| :--- | :--- | :--- |
| [cachebox](cachebox.md) | struct | An optional structure used to configure CacheBox. If not setup the framework will use its default configuration found in `/coldbox/system/web/config/CacheBox.cfc` |
| [coldbox](coldbox.md) | struct | The main coldbox directives structure that holds all the coldbox settings. |
| [conventions](conventions.md) | struct | A structure where you will configure the application convention names |
| [environments](environments.md) | struct | A structure where you will configure environment detection patterns |
| [flash](flash.md) | struct | A structure where you will configure the [FlashRAM](https://github.com/ortus/coldbox-platform-documentation/tree/24d3f3d16693b36ca41bf5ce0329c6ff33316ef0/full/configuration/coldboxcfc/configuration_directives/flash_ram/flash_ram.md) |
| [interceptorSettings](interceptorsettings.md) | struct | An optional structure to configure application wide interceptor behavior |
| [interceptors](interceptors.md) | array | An optional array of interceptor declarations for your application |
| [layoutSettings](layoutsettings.md) | struct | A structure where you define how the layout manager behaves in your application |
| [layouts](layouts.md) | array | An array of layout declarations for implicit layout-view-folder pairings in your application |
| [logbox](logbox.md) | struct | An optional structure to configure the logging and messaging in your application via LogBox |
| [modules](modules.md) | struct | An optional structure to configure application wide module behavior |
| [moduleSettings](modulesettings.md) | struct | An optional structure to configure individual modules installed in your application. |
| [settings](settings.md) | struct | A structure where you can put your own application settings |
| [wirebox](wirebox.md) | struct | An optional structure used to define how WireBox is loaded |

## CacheBox

The CacheBox structure is based on the [CacheBox declaration DSL](https://cachebox.ortusbooks.com/), and it allows you to customize the caches in your application. Below are the main keys you can fill out, but we recommend you review the CacheBox documentation for further detail.

```javascript
//cachebox configuration
cachebox = {
    // Location of the configuration CFC for CacheBox
    configFile = "config/CacheBox.cfc",
    // Scope registration for CacheBox
    scopeRegistration = {enabled=true,scope=application,key=cacheBox},
    // Default Cache Configuration
    defaultCache  = "views",
    // Caches Configuration
    caches      = {}
};
```

> **Info** : We would recommend you create a `config/CacheBox.cfc` and put all your caching configuration there instead of in the main ColdBox configuration file. This will give you further portability and decoupling.

### ConfigFile

An absolute or relative path to the CacheBox configuration CFC or XML file to use instead of declaring the rest of the keys in this structure. So if you do not define a cacheBox structure, the framework will look for the default value: `config/CacheBox.cfc` and it will load it if found. If not found, it will use the default CacheBox configuration found in `/coldbox/system/web/config/CacheBox.cfc`

### ScopeRegistration

A structure that enables scope registration of the CacheBox factory in either server, cluster, application or session scope.

### DefaultCache

The configuration of the default cache which will have an implicit name of default which is a reserved cache name. It also has a default provider of CacheBox which cannot be changed.

### Caches

A structure where you can create more named caches for usage in your CacheBox factory.

## ColdBox

The ColdBox directive is where you configure the framework for operation.

### Application Setup

```javascript
coldbox = {
    // The name of the application
    appName   = "My App",
    // The name of the incoming URL/FORM/REMOTE variable that tells the framework what event to execute. Ex: index.cfm?event=users.list
    eventName = "event"
};
```

> **Info** : Please note that there are no mandatory settings as of ColdBox 4.2.0. If fact, you can remove the config file completely and your app will run. It will be impossible to reinit the app however without a reinit password set.

### Development Settings

```javascript
coldbox = {
    reinitPassword = "h1cker",
    handlersIndexAutoReload = true
};
```

#### **reinitPassword**

Protect the reinitialization of the framework URL actions. For security, if this setting is omitted, we will create a random password. Setting it to an empty string will allow you to reinitialize without a password. **Always have a password set for public-facing sites.**

```text
// reinit with no password
http://localhost/?fwreinit=1
// reinit with password
http://localhost/?fwreinit=mypass
```

#### **handlersIndexAutoReload**

Will scan the conventions directory for new handler CFCs on each request if activated. Use **false** for production, this is only a development true setting.

### Implicit Event Settings

```javascript
coldbox={
    //Implicit Events
    defaultEvent  = "Main.index",
    requestStartHandler     = "Main.onRequestStart",
    requestEndHandler   = "Main.onRequestEnd",
    applicationStartHandler = "Main.onAppInit",
    applicationEndHandler = "Main.onAppEnd",
    sessionStartHandler = "Main.onSessionStart",
    sessionEndHandler = "Main.onSessionEnd",
    missingTemplateHandler = "Main.onMissingTemplate"
}
```

These settings map 1-1 from ColdBox events to the `Application.cfc` life-cycle methods. The only one that is not is the `defaultEvent`, which selects what event the framework will execute when no incoming event is detected via URL/FORM or REMOTE executions.

### Extension Points

The ColdBox extension points are a great way to create federated applications that can reuse a centralized core instead of the local conventions. It is also a great way to extend some core classes with your own.

```javascript
coldbox={
    //Extension Points
    applicationHelper             = "includes/helpers/ApplicationHelper.cfm",
    viewsHelper                    = "",
    modulesExternalLocation        = [],
    viewsExternalLocation        = "",
    layoutsExternalLocation     = "",
    handlersExternalLocation      = "",
    requestContextDecorator     = "",
    controllerDecorator         = ""
}
```

#### **applicationHelper**

A list or array of absolute or relative paths to a UDF helper file. The framework will load all the methods found in this helper file globally. Meaning it will be injected in ALL handlers, layouts and views.

#### **viewsHelper**

A list or array of absolute or relative paths to a UDF helper file. The framework will load all the methods found in this helper in layouts and views only.

#### **modulesExternalLocation**

A list or array of locations of where ColdBox should look for modules to load into your application. The path can be a cf mapping or `cfinclude` compatible location. Modules are searched and loaded in the order of the declared locations. The first location ColdBox will search for modules is the conventions folder `modules`

#### **viewsExternalLocation**

The CF include path of where to look for secondary views for your application. Secondary views look just like normal views except the framework looks in the conventions folder first and if not found then searches this location.

#### **layoutsExternalLocation**

The CF include path of where to look for secondary layouts for your application. Secondary layouts look just like normal layouts except the framework looks in the conventions folder first and if not found then searches this location.

#### **handlersExternalLocation**

The CF dot notation path of where to look for secondary events for your application. Secondary events look just like normal events except the framework looks in the conventions folder first and if not found then searches this location.

#### **requestContextDecorator**

The CF dot notation path of the CFC that will decorate the system Request Context object.

#### **controllerDecorator**

The CF dot notation path of the CFC that will decorate the system Controller

### Exception Handling

```javascript
coldbox = {
    // Error/Exception Handling handler
    exceptionHandler        = "",
    // Invalid HTTP method Handler
    invalidHTTPMethodHandler = "",
    // The handler to execute on invalid events
    invalidEventHandler = "",
    // The default error template    
    customErrorTemplate     = "/coldbox/system/includes/BugReport-Public.cfm"
}
```

#### **exceptionHandler**

The event handler to call whenever ANY non-catched exception occurs anywhere in the request lifecycle execution. Before this event is fired, the framework will log the error and place the exception in the prc as `prc.exception`.

#### **invalidHTTPMethodHandler**

The event handler to call whenever a route or event is accessed with an invalid HTTP method.

#### **invalidEventHandler**

This is the event handler that will fire masking a non-existent event that gets requested. This is a great place to place 302 or 404 redirects whenever non-existent events are being requested.

#### **customErrorTemplate**

The relative path from the application's root level of where the custom error template exists. This template receives a key in the private request collection called `exception` that contains the exception. By default ColdBox does not show robust exceptions, you can turn on robust exceptions by choosing the following template:

```text
coldbox.customErrorTemplate = "/coldbox/system/includes/BugReport.cfm";
```

### Application Aspects

```javascript
coldbox = {
    // Persist handlers
    handlerCaching             = false,
    // Activate event caching
    eventCaching            = true,
    // Activate view  caching
    viewCaching              = true,
    // Return RC struct on Flex/Soap Calls
    proxyReturnCollection     = false,
    // Activate implicit views
    implicitViews           = true,
    // Case for implicit views
    caseSensitiveImplicitViews = true,
    // Auto register all model objects in the `models` folder into WireBox
    autoMapModels     = true
}
```

#### **handlerCaching**

This is useful to be set to false in development and true in production. This tells the framework to cache your event handler objects as singletons.

#### **eventCaching**

This directive tells ColdBox that when events are executed they will be inspected for caching metadata. This does not mean that ALL events WILL be cached if this setting is turned on. It just activates the inspection mechanisms for whenever you annotate events for caching or using the `runEvent()` caching methods.

#### **viewCaching**

This directive tells ColdBox that when views are rendered, the `cache=true` parameter will be obeyed. Turning on this setting will not cause any views to be cached unless you are also passing in the caching parameters to your `renderView()` or `event.setView()` calls.

#### **proxyReturnCollection**

This is a boolean setting used when calling the ColdBox proxy's `process()` method from a Flex or SOAP/REST call. If this setting is set to **true**, the proxy will return back to the remote call the entire request collection structure ALWAYS! If set to **false**, it will return, whatever the event handler returned back. Our best practice is to always have this **false** and return appropriate data back.

#### **implicitViews**

Allows you to use implicit views in your application and view dispatching. You can get a performance boost if you disable this setting.

#### **caseSensitiveImplicitViews**

By default implicit views are case sensitive since ColdBox version 5.2.0, before this version the default was **false**.

#### **autoMapModels**

ColdBox by convention can talk to, use and inject models from the `models` folder by just using their name. However, once you built complex hierarchies you would have to specify the specific hierarchy or folder structure for the model; This is a limitation of conventions. Your choices before v5.2.0 was to map the objects yourself in the WireBox binder. Now, if you turn this setting to **true**, we will issue a `mapDirectory()` on the `models` directory for you.

## Conventions

This element defines custom conventions for your application. By default, the framework has a default set of conventions that you need to adhere too. However, if you would like to implement your own conventions for a specific application, you can use this setting, otherwise do not declare it:

```javascript
//Conventions
conventions = {
    handlersLocation = "controllers",
    viewsLocation      = "views",
    layoutsLocation  = "views",
    modelsLocation      = "model",
    modulesLocation  = "modules",
    eventAction      = "index"
};
```

## Environments

The configuration CFC has embedded environment control and detection built-in. Environments kan be detected by:

* regex matching against cgi.http\_host
* detection of an environmental variable called ENVIRONMENT \( Coldbox 5.2 and higher \)
* usage of a `detectEnvironment()` function

The first option \(regex matching\) is the easiest to use, but not very reliable if you are using multiple hostnames or commandbox for re-initialization.

{% hint style="warning" %}
If you are using `commandbox` please read ALL options below
{% endhint %}

### Default: Regex matching against cgi.http\_host

To detect your environments you will setup a structure called `environments` in your coldbox configuration with the named environments and their associated regular expressions for its `cgi` host names to match for you automatically. If the framework matches the regex with the associated `cgi.http_host`, it will set a setting called `Environment` in your configuration settings and look for that environment setting name in your CFC as a method by convention. That's right, it will check if your CFC has a method with the same name as the environment and if it exists, it will call it for you. Here is where you basically override, remove, or add any settings according to your environment.

> **Warning** : The environment detection occurs AFTER the `configure()` method is called. Therefore, whatever settings or configurations you have on the `configure()` method will be stored first, treat those as **Production** settings.

```javascript
environments = {
    // The key is the name of the environment
    // The value is a list of regex to match against cgi.http_host
    development = "^cf2016.,^lucee.,localhost",
    staging = "^stg"
};
```

The regex match will also create a global setting called "environment" which you can access and use like this:

```javascript
if ( getSetting('environment') == 'development' ){
    doSomeMajik();
}
```

In the above example, I declare a **development** key with a value list of regular expressions. If I am in a host that starts with **cf2016**, this will match and set the environment setting equal to **development**. It will then look for a development method in this CFC and execute it.

```javascript
/**
* Executed whenever the development environment is detected
*/
function development(){
    // Override coldbox directives
    coldbox.handlerCaching = false;
    coldbox.eventCaching = false;
    coldbox.debugPassword = "";
    coldbox.reinitPassword = "";

    // Add dev only interceptors
    arrayAppend( interceptors, {class="#appMapping#.interceptors.CustomLogger} );
}
```

### Detection of an environmental variable called ENVIRONMENT

If you are using environmental variables for your different environments, you can specify an environmental variable called ENVIRONMENT and name it `staging`, `development`, `testing` etcetera, depending on the required environment. As in the regex example, a function named after your environment \(e.g. `staging()` or `development()` \) will be called after your `configure` method.

{% hint style="info" %}
This method is more reliable than relying on cgi.http\_host, since it will never change once configured correctly.
{% endhint %}

### Custom Environment Detection

If you are NOT using environmental variables you can use your own detection algorithm instead of looking at the `cgi.http_host` variable. You will NOT fill out an environments structure but actually create a method with the following signature:

```javascript
string public detectEnvironment(){
}
```

This method will be executed for you at startup and it must return the name of the environment the application is on. You can check for any condition which distinguishes your environment from your other environments. Als long as you return a environment name based on your own logic it will then store it and execute the method if it exists.

## Flash

This directive is how you will configure the [Flash RAM](../../../../digging-deeper/flash-ram/) for operation. Below are the configuration keys and their defaults:

```javascript
// flash scope configuration
flash = {
    // Available scopes are: session,client,cache,mock or your own class path
    scope = "session",
    // constructor properties for the flash scope implementation
    properties = {},
    // automatically inflate flash data into the RC scope at the beginning of a request
    inflateToRC = true, 
    // automatically inflate flash data into the PRC scope at the beginning of a request
    inflateToPRC = false, 
    // automatically purge flash data for you
    autoPurge = true, 
    // automatically save flash scopes at end of a request and on relocations.
    autoSave = true 
};
```

## InterceptorSettings

This structure configures the interceptor service in your application.

```javascript
//Interceptor Settings
interceptorSettings = {
    throwOnInvalidStates = false,
    customInterceptionPoints = "onLogin,onWikiTranslation,onAppClose"
};
```

### `throwOnInvalidStates`

This tells the interceptor service to throw an exception if the state announced for interception is not valid or does not exist. Defaults to **false**.

### `customInterceptionPoints`

This key is a comma delimited list or an array of custom interception points you will be registering for custom announcements in your application. This is the way to provide an observer-observable pattern to your applications.

> **Info** Please see the [Interceptors](../../../../digging-deeper/interceptors/) section for more information.

## Interceptors

This is an array of interceptor definitions that you will use to register in your application. The key about this array is that **ORDER** matters. The interceptors will fire in the order that you register them whenever their interception points are announced, so please watch out for this caveat. Each array element is a structure that describes what interceptor to register.

```javascript
//Register interceptors as an array, we need order
interceptors = [

    { 
        // The CFC instantiation path
        class="",
        // The alias to register in WireBox, if not defined it uses the name of the CFC
        name="",
        // A struct of data to configure the interceptor with.
        properties={}
    }

    { class="mypath.MyInterceptor",
      name="MyInterceptor",
      properties={useSetterInjection=false}
    },
    { class="coldbox.system.interceptors.SES", name="MySES" }
];
```

> **Warning** : Important: Order of declaration matters! Also, when declaring multiple instances of the same CFC \(interceptor\), make sure you use the name attribute in order to distinguish them. If not, only one will be registered \(the last one declared\).

## Layouts

The layouts array element is used to define implicit associations between layouts and views/folders, this does not mean that you need to register ALL your layouts. This is a convenience for pairing them, we are in a conventions framework remember.

Before any renderings occur or lookups, the framework will check this array of associations to try and match in what layout a view should be rendered in. It is also used to create aliases for layouts so you can use aliases in your code instead of the real file name and locations.

```javascript
//Register Layouts
layouts = [
    { 
        // The alias of a layout
        name="",
        // The layout file
        file="",
        // A list of view names to render within this layout
        views="",
        // A list of regex names to match a view
        folders=""
    }

    // Examples
    { name="tester",file="Layout.tester.cfm",views="vwLogin,test",folders="tags,pdf/single"    },
    { name="login",file="Login.cfm",folders="^admin/security"}
];
```

## LayoutSettings

This structure allows you to define a system-wide default layout and view.

```javascript
//Layout Settings
layoutSettings = {
    // The default layout to use if NOT defined in a request
    defaultLayout = "Main.cfm",
    // The default view to use to render if NOT defined in a request
    defaultView   = "youForgot.cfm"
};
```

> **Hint** Please remember that the default layout is `Main.cfm`

## LogBox

The logBox structure is based on the LogBox declaration DSL, see the [LogBox Documentation](https://logbox.ortusbooks.com/) for much more information.

```javascript
//LogBox DSL
logBox = {
    // The configuration file without fileextension to use for operation, instead of using this structure
    configFile = "config/LogBox", 
    // Appenders
    appenders = {
        appenderName = {
            class="class.to.appender", 
            layout="class.to.layout",
            levelMin=0,
            levelMax=4,
            properties={
                name  = value,
                prop2 = value 2
            }
    },
    // Root Logger
    root = {levelMin="FATAL", levelMax="DEBUG", appenders="*"},
    // Granualr Categories
    categories = {
        "coldbox.system" = { levelMin="FATAL", levelMax="INFO", appenders="*"},
        "model.security" = { levelMax="DEBUG", appenders="console"}
    }
    // Implicit categories
    debug  = ["coldbox.system.interceptors"],
    info   = ["model.class", "model2.class2"],
    warn   = ["model.class", "model2.class2"],
    error  = ["model.class", "model2.class2"],
    fatal  = ["model.class", "model2.class2"],
    off    = ["model.class", "model2.class2"]
};
```

> **Info** : If you do not define a logBox DSL structure, the framework will look for the default configuration file `config/LogBox.cfc`. If it does not find it, then it will use the framework's default logging settings.

**ConfigFile**

You can use a configuration CFC instead of inline configuration by using this setting. The default value is `config/LogBox.cfc`, so by convention you can just use that location. If no values are defined or no config file exists, the default configuration file is `coldbox/system/web/config/LogBox.cfc`.

## Modules

The modules structure is used to configure the behavior of the [ColdBox Modules](../../../../hmvc/modules/).

```javascript
modules = {
    // Will auto reload the modules in each request. Great for development but can cause some loading/re-loading issues
    autoReload = true,
    // An array of modules to load ONLY
    include = [],
    // An array of modules to EXCLUDE for operation
    exclude = [ "paidModule1", "paidModule2" ]
};
```

> **Danger** Please be very careful when using the `autoReload` flag as module routing can be impaired and thread consistency will also suffer. This is PURELY a development flag that you can use at your own risk.

## ModuleSettings

This structure is used to house module configurations. Please refer to each module's documentation on how to create the configuration structures. Usually the keys will match the name of the module to be configured.

```javascript
component {

     function configure() {

         moduleSettings = {
             myModule = {
                someSetting = "overridden"
             }
        };
    }
}
```

## Settings

These are custom application settings that you can leverage in your application.

```javascript
// Custom Settings
settings = {
    useSkins = true,
    myCoolArray = [1,2,3,4],
    skinsPath = "views/skins",
    myUtil = createObject("component","#appmapping#.model.util.MyUtility")
};
```

You can read our [Using Settings](../../using-settings.md) section to discover how to use all the settings in your application.

## WireBox

This configuration structure is used to configure the [WireBox](https://wirebox.ortusbooks.com) dependency injection framework embedded in ColdBox.

```javascript
// wirebox integration
wirebox = {
    binder = 'config.WireBox',
    singletonReload = true
};
```

**binder**

The location of the WireBox configuration binder to use for the application. If empty, we will use the binder in the `config` folder called by conventions: `WireBox.cfc`

**singletonReload**

A great flag for development. If enabled, WireBox will flush its singleton objects on every request so you can develop without any headaches of reloading.

> **Warning** : This operation can cause some thread issues and it is only meant for development. Use at your own risk.

## Getting Started Guide

The ColdBox HMVC Platform is the de-facto enterprise-level HMVC framework for CFML developers. It's professionally backed, highly extensible, and productive. Getting started with ColdBox is quick and painless. The only thing you need to begin is [CommandBox](http://www.ortussolutions.com/products/commandbox), a command line tool for CFML developers.

This is a one-page introductory guide to ColdBox. If you are new to MVC or ColdBox, you can also leverage our [60 minute quick start guide](../for-newbies/60-minute-quick-start/) as well.

### IDE Tools

ColdBox has the following supported IDE Tools:

* Sublime - [https://packagecontrol.io/packages/ColdBox Platform](https://packagecontrol.io/packages/ColdBox%20Platform)
* VSCode - [https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-coldbox](https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-coldbox)
* CFBuilder - [https://www.forgebox.io/view/ColdBox-Platform-Utilities](https://www.forgebox.io/view/ColdBox-Platform-Utilities)

### Install CommandBox

You can read through our one-page [CommandBox Getting Started Guide](https://commandbox.ortusbooks.com/getting-started-guide). Or simply grab the CommandBox executable from the [download page](https://www.ortussolutions.com/products/commandbox#download) and double click it to run.

[http://www.ortussolutions.com/products/commandbox](http://www.ortussolutions.com/products/commandbox)

You should now be seeing a prompt that looks like this:

### Create A New Site

Now we're cooking with gas! Let's create a new ColdBox application. CommandBox comes with built-in commands for scaffolding out new sites as well as installing ColdBox and other libraries. We'll start by changing into an empty directory were we want our new app to live. If necessary, you can create a new folder.

```bash
CommandBox> mkdir playground --cd
```

Now let's ask CommandBox to create a new ColdBox app for us.

```bash
CommandBox> coldbox create app MyPlayground
```

{% hint style="success" %}
**Tip:** You can find many scaffolding templates for ColdBox in our Github organization: [github.com/coldbox-templates](https://github.com/coldbox-templates)

You can also issue a `coldbox create app help` command and get help for the creation command.
{% endhint %}

#### File/Folder Conventions

This command will place several new folders and files in your working directory. Let's run the `ls` command to view them.

```bash
CommandBox> ls
```

Here's a rundown of the important bits.

* **coldbox** - This is the ColdBox framework managed by CommandBox
* **config/Coldbox.cfc** - Your application configuration object
* **config/Router.cfc** - Your application URL Router
* **handlers** - Your controller layer, which in ColdBox they are called event handlers
* **layouts** - Your HTML layouts
* **models** - This holds your model CFCs 
* **modules** - This holds the CommandBox tracked modules
* **modules\_app** - This holds your app's modules
* **views** - Your HTML views will go here

### Start It Up

Now that our shiny new MVC app is ready to go, let's fire it up using the embedded server built into CommandBox. You don't need any other software installed on your PC for this to work. CommandBox has it all!

```bash
CommandBox> start --rewritesEnable
```

In a few seconds, a browser window will appear with your running application. This is a full server with access to the web administrator where you can add data sources, mappings, or adjust the server settings. Notice the handy icon added to your system tray as well. The `--rewritesEnable` flag will turn on some basic URL rewriting so we have nice, pretty URLs.

{% hint style="success" %}
**Tip:** If you are creating an app to run on any other server than the commandbox server, you will need to manually set up URL rewriting. More info here: [/the-basics/routing/requirements](../the-basics/routing/requirements/)
{% endhint %}

### Take A Look Around

ColdBox uses easy conventions to define the controllers and views in your app. Let's open up our main app controller in your default editor to have a looksie.

```bash
CommandBox> edit handlers/main.cfc
```

At the top, you'll see a function named "index". This represents the **default action** that runs for this controller, which in ColdBox land they are referred to as event handlers.

```javascript
// Default Action
function index(event,rc,prc){
    prc.welcomeMessage = "Welcome to ColdBox!";
    event.setView("main/index");
}
```

Now let's take a look in the `main/index` view. It's located int he `views` folder.

```bash
CommandBox> edit views/main/index.cfm
```

This line of code near the top of the view is what outputs the `prc.welcomeMessage` variable we set in the controller.

```markup
<h1>#prc.welcomeMessage#</h1>
```

Try changing the value being set in the handler and refresh your browser to see the change.

```javascript
prc.welcomeMessage = "This is my new welcome message";
```

### Building On

Let's define a new event handler now. Your controllers act as event handlers to respond to requests, REST API, or remote proxies.

Pull up CommandBox again and run this command.

```bash
CommandBox> coldbox create handler helloWorld index,add,edit,list
```

That's it! You don't need to add any special configuration to declare your handler. Now we have a new handler called `helloWorld` with actions `index`, `add`, `edit`, and `list`. The command also created a test case for our handler as well as stubbed-out views for each of the actions.

Now, let's re-initialize the framework to pick up our new handler by typing `?fwreinit=1` at the end of the URL.

Let's hit this new controller we created with a URL like so. Your port number will probably be different.

> 127.0.0.1:43272/helloWorld

{% hint style="info" %}
Normally the URL would have `index.cfm` before the `/helloWorld` bit, but our `--rewritesEnable` flag when we started the server makes this nicer URL possible.
{% endhint %}

### Install Packages

ColdBox's MVC is simple, but it's true power comes from the wide selection of modules you can install into your app to get additional functionality. You can checkout the full list of modules available on the Forgebox directory: [www.forgebox.io](https://www.forgebox.io).

> [forgebox.io/type/modules](http://forgebox.io/type/modules)

Here's some useful examples:

* **BCrypt** -- Industry-standard password hashing
* **cbdebugger** -- For debugging Coldbox apps
* **cbjavaloader** - For interacting with Java classes and libraries
* **cbMarkdown** - For writing in markdown
* **cbMessagebox** -- Display nice error/success messages
* **cborm** -- Awesome ORM Services
* **cb18n** -- For multilingual sites
* **cbt** - ColdBox templating language
* **cbValidation** - Back-end validation framework
* **qb** - Fluent query builder and schema builder
* **route-visualizer** - For visualizing your application routes

Install `cbmessagebox` from the CommandBox prompt like this:

```bash
CommandBox> install cbmessagebox
```

We can see the full list of packages by using the `list` command.

```bash
CommandBox> list
Dependency Hierarchy for myApp (0.0.0)
+-- cbmessagebox (1.0.0)
+-- coldbox (4.0.0)
```

Right now we can see that our app depends on `coldbox` and `cbmessagebox` to run. We'll use our new `cbmessagebox` module in a few minutes. But first, we'll create a simple Model CFC to round out our `MVC` app.

### Creating A Model

Models encapsulate the business logic your application. They can be services, beans, or DAOs. We'll use CommandBox to create a `GreeterService` in our new app with a `sayHello` method.

```bash
CommandBox> coldbox create model GreeterService sayHello --open
```

{% hint style="success" %}
**Tip:** The `--open` is a nice shortcut that opens our new model in our default editor after creating it.
{% endhint %}

Let's finish implementing the `sayHello()` method by adding this return statement and save the file.

We can also add the word `singleton` to the component declaration. This will tell **WireBox** to only create one instance of our service.

```javascript
component singleton {

    function sayHello(){
        return 'Hey, you sexy thing!';
    }

}
```

{% hint style="info" %}
What is WireBox?

WireBox is a dependency injection framework that is included with ColdBox. It will manage all object creations, persistence and assembling. You don't have to worry about using `new` or `createobject()` for CFCs anymore.
{% endhint %}

### Tie It All Together

Ok, let's open up that `helloWorld` handler we created a while back. Remember, you can hit tab while typing to auto-complete your file names.

```bash
CommandBox> edit handlers/helloWorld.cfc
```

We'll inject our `greeterService` and the `cbmessagebox` service into the handler by adding these properties to the top of `/handlers/helloWorld.cfc`.

{% hint style="info" %}
What is this magical injection? Injection is a way to get references of other objects placed in the `variables` scope of other objects. This makes your life easier as you don't have to be creating objects manually or even knowing where they exist.
{% endhint %}

This will put the instance of our services in the `variables` scope where we can access it in our action methods.

```javascript
component {

    property name='greeterService' inject='greeterService';
    property name='messageBox' inject='@cbmessagebox';

    ...
}
```

And now in our `index` method, we'll set the output of our service into an `info` message.

```javascript
function index( event, rc, prc ){
    messageBox.info( greeterService.sayHello() );
    event.setView( "helloWorld/index" );
}
```

One final piece. Open up the default layout located in `layouts/Main.cfm` and find the `#renderView()#`. Add this line right before it to render out the message box that we set in our handler.

```markup
#getInstance( 'messagebox@cbmessageBox').renderIt()#
<div class="container">#renderView()#</div>
```

Now hit your `helloWorld` handler one final time with `?fwreinit=1` in the URL to see it all in action! \(Again, your port number will most likely be different.

> 127.0.0.1:43272/helloWorld?fwreinit=1

### What's Next?

**Congratulations**! In a matter of minutes, you have created a full MVC application. You installed a community module from ForgeBox, created a new handler/view and tied in business logic from a service model.

As easy as that was, you're just scratching the surface of what ColdBox can do for you. Continue reading this book to learn more about:

* Environment-specific configuration
* Easy SES URL routing
* Tons of 3rd party modules
* Drop-in security system
* Sweet REST web service support

#### Getting Help

If you run into issues or just have questions, please jump on our [ColdBox Google Group](https://groups.google.com/forum/#!forum/coldbox) and our [Slack team](http://boxteam.herokuapp.com/) and ask away.

ColdBox is Professional Open Source under the Apache 2.0 license. We'd love to have your help with the product.

## Installation

**Welcome to the world of ColdBox!**

We are excited you are taking this development journey with us. Before we get started with ColdBox let's install CommandBox CLI, which will allow you to install/uninstall dependencies, start servers, have a REPL tool and much more.

### IDE Tools

ColdBox has the following supported IDE Tools:

* Sublime - [https://packagecontrol.io/packages/ColdBox Platform](https://packagecontrol.io/packages/ColdBox%20Platform)
* VSCode - [https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-coldbox](https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-coldbox)
* CFBuilder - [https://www.forgebox.io/view/ColdBox-Platform-Utilities](https://www.forgebox.io/view/ColdBox-Platform-Utilities)

### CommandBox CLI

The first step in our journey is to [install](https://commandbox.ortusbooks.com/content/setup/installation.html) CommandBox. [CommandBox](https://www.ortussolutions.com/products/commandbox) is a ColdFusion \(CFML\) Command Line Interface \(CLI\), REPL, Package Manager and Embedded Server. We will be using CommandBox for almost every excercise in this book and it will also allow you to get up and running with ColdFusion and ColdBox in a much speedier manner.

> **Note** : However, you can use your own ColdFusion server setup as you see fit. We use CommandBox as everything is scriptable and fast!

#### Download CommandBox

You can download CommandBox from the official site: [https://www.ortussolutions.com/products/commandbox\#download](https://www.ortussolutions.com/products/commandbox#download) and install in your preferred Operating System \(Windows, Mac, \*unix\). CommandBox comes in two flavors:

1. No Java Runtime \(30mb\)
2. Embedded Runtime \(80mb\)

So make sure you choose your desired installation path and follow the instructions here: [https://commandbox.ortusbooks.com/content/setup/installation.html](https://commandbox.ortusbooks.com/content/setup/installation.html)

#### Starting CommandBox

Once you download and expand CommandBox you will have the `box.exe` or `box` binary, which you can place in your Windows Path or \*Unix `/usr/bin` folder to have it available system wide. Then just open the binary and CommandBox will unpack itself your user's directory: `{User}/.CommandBox`. This happens only once and the next thing you know, you are in the CommandBox interactive shell!

We will be able to execute a-la-carte commands from our command line or go into the interactive shell for multiple commands. We recommend the interactive shell as it is faster and can remain open in your project root.

{% hint style="info" %}
All examples in this book are based on the fact of having an interactive shell open.
{% endhint %}

### Installing ColdBox

To get started open the CommandBox binary or enter the shell by typing `box` in your terminal or console. Then let's create a new folder and install ColdBox into a directory.

```bash
mkdir myapp --cd
install coldbox
```

CommandBox will resolve `coldbox` from ForgeBox \([www.forgebox.io](https://www.forgebox.io)\), use the **latest version** available, download and install it in this folder alongside a `box.json` file which represents your application package.

```text
Dir 0 Apr 25,2018 11:04:05 coldbox
File 112 Apr 25,2018 11:04:05 box.json
```

{% hint style="info" %}
You can also install the latest bleeding edge version by using the `coldbox@be` slug instead, or any previous version.
{% endhint %}

That's it! CommandBox can now track this version of ColdBox for you in this directory.

#### Scaffolding ColdBox Applications

CommandBox comes with a `coldbox create app` command that can enable you to create application skeletons using one of our official skeletons or by creating [your own](../digging-deeper/recipes/application-templates.md) application template:

* **Advanced** : A tag based advanced template
* **AdvancedScript**  \(`default`\): A script based advanced template
* **elixir** : A ColdBox Elixir based template
* **ElixirBower** : A ColdBox Elixir + Bower based template
* **ElixirVueJS** : A ColdBox Elixir + Vue.js based template
* **rest**: A RESTFul services template
* **rest-hmvc**: A RESTFul service built with modules
* **Simple** : A traditional simple template
* **SuperSimple** : The bare-bones template

{% hint style="success" %}
You can find many scaffolding templates for ColdBox in our Github organization: [github.com/coldbox-templates](https://github.com/coldbox-templates)

Type `coldbox create app` help in CommandBox to get tons of help for scaffolding apps.
{% endhint %}

### Uninstalling ColdBox

To uninstall ColdBox from this application folder just type `uninstall coldbox`.

### Updating ColdBox

To update ColdBox from a previous version, just type `update coldbox`.

## Conventions

The core conventions delineate the contract between ColdBox and you for file/directory locations and more. Below is a table of the core conventions:

### Directory/File Conventions

* **config/Coldbox.cfc** - Your application configuration object \(_optional_\)
* **config/Router.cfc** - Your application URL Router \(_optional_\)
* **handlers** - This holds the app's controllers
* **layouts** - Your HTML layouts
* **models** - This holds your app's CFCs 
* **modules** - This holds the CommandBox tracked modules
* **modules\_app** - This holds your app's modules
* **views** - Your HTML views will go here

### Execution Conventions

| **Convention** | **Default Value** | **Description** |
| :--- | :--- | :--- |
| Default Event | `main.index` | The default event to execute when no event is specified |
| Default Action | `index()` | The default action to execute in an event handler controller if none is specified |
| Default Layout | `layouts/Main.cfm` | The default system layout to use |

## Configuration

In order to create a ColdBox application you must adhere to some naming conventions and a pre-set directory structure. This is needed in order for ColdBox to find what it needs and to help you find modules more easily instead of configuring every single area of your application.

{% hint style="info" %}
ColdBox relies on conventions instead of configurations.
{% endhint %}

You can also configure many aspects of ColdBox, third-party modules and application settings in our programmtic configuration object: `ColdBox.cfc`.

### Reinitializing An Application

Please note that anytime you make any configuration changes or there are things in memory you wish to clear out, you will be using a URL action that will tell the ColdBox [Bootstrapper](bootstrapper-application.cfc.md) to reinitialize the application. This special URL variable is called `fwreinit` and can be any value or a specific password you setup in the [ColdBox configuration directive](coldbox.cfc/configuration-directives/).

```text
// reinit with no password
index.cfm?fwreinit=1

// reinit with password
index.cfm?fwreinit=mypass
```

You can also use CommandBox to reinit your application if you are using its embedded server:

```bash
CommandBox>coldbox reinit
```

## ColdBox.cfc

The ColdBox configuration CFC is the heart of your ColdBox application. It contains the initialization variables for your application and extra information used by third-party modules and ultimately how your application boots up. In itself, it is also an event listener or [ColdBox Interceptor](configuration-directives/interceptors.md), so it can listen to life-cycle events of your application.

This CFC is instantiated by ColdBox and decorated at runtime so you can take advantage of some dependencies.

* **Controller** : Reference to the main ColdBox Controller object \(`coldbox.system.web.Controller`\)
* **AppMapping** : The location of the application on the web server
* **LogBoxConfig** : A reference to a LogBox configuration object

### Configuration Storage

Once the application starts up, a reference to the instantiated configuration CFC will be stored in the configuration settings with the key `coldboxConfig`. You can then retrieve it later in your handlers, interceptors, modules, etc.

```javascript
// retrieve it
config = getSetting( 'coldboxConfig' );

// inject it
property name="config" inject="coldbox:setting:coldboxConfig";
```

### Configuration Interceptor

Another cool concept for the Configuration CFC is that it is also registered as a [ColdBox Interceptor](../../../digging-deeper/interceptors/) once the application starts up automatically for you. Create functions that will listen to application events:

```javascript
function preProcess( event, interceptData, buffer, rc, prc ){
    writeDump( 'I just hijacked your app!' );abort;
}
```

{% hint style="danger" %}
Note that the config CFC does not have the same variables mixed into it that a "normal" interceptor has. You can still access everything you need, but will need to get it from the `controller` in the variables scope.
{% endhint %}

```javascript
function preRender( event, interceptData, buffer, rc, prc ){
    controller.getWirebox().getInstance( 'loggerService' ).doSomething();
}
```

## Configuration Directives

The basic configuration object has 1 method for application configuration called `configure()` where you will place all your configuration directives and settings:

{% code title="ColdBox.cfc" %}
```javascript
/**
* A simple CFC that configures a ColdBox application.  You can even extend, compose, strategize and do your OO goodness.
*/
component{

    // Mandatory configuration method
    function configure(){
        coldbox = {

        };
    }

}
```
{% endcode %}

### Directives

Inside of this configuration method you will place several core and third-party configuration structures that can alter your application settings and behavior. Below are the core directives you can define:

| Directive | Type | Description |
| :--- | :--- | :--- |
| [cachebox](cachebox.md) | struct | An optional structure used to configure CacheBox. If not setup the framework will use its default configuration found in `/coldbox/system/web/config/CacheBox.cfc` |
| [coldbox](coldbox.md) | struct | The main coldbox directives structure that holds all the coldbox settings. |
| [conventions](conventions.md) | struct | A structure where you will configure the application convention names |
| [environments](environments.md) | struct | A structure where you will configure environment detection patterns |
| [flash](flash.md) | struct | A structure where you will configure the [FlashRAM](https://github.com/ortus/coldbox-platform-documentation/tree/24d3f3d16693b36ca41bf5ce0329c6ff33316ef0/full/configuration/coldboxcfc/configuration_directives/flash_ram/flash_ram.md) |
| [interceptorSettings](interceptorsettings.md) | struct | An optional structure to configure application wide interceptor behavior |
| [interceptors](interceptors.md) | array | An optional array of interceptor declarations for your application |
| [layoutSettings](layoutsettings.md) | struct | A structure where you define how the layout manager behaves in your application |
| [layouts](layouts.md) | array | An array of layout declarations for implicit layout-view-folder pairings in your application |
| [logbox](logbox.md) | struct | An optional structure to configure the logging and messaging in your application via LogBox |
| [modules](modules.md) | struct | An optional structure to configure application wide module behavior |
| [moduleSettings](modulesettings.md) | struct | An optional structure to configure individual modules installed in your application. |
| [settings](settings.md) | struct | A structure where you can put your own application settings |
| [wirebox](wirebox.md) | struct | An optional structure used to define how WireBox is loaded |

## CacheBox

The CacheBox structure is based on the [CacheBox declaration DSL](https://cachebox.ortusbooks.com/), and it allows you to customize the caches in your application. Below are the main keys you can fill out, but we recommend you review the CacheBox documentation for further detail.

```javascript
//cachebox configuration
cachebox = {
    // Location of the configuration CFC for CacheBox
    configFile = "config/CacheBox.cfc",
    // Scope registration for CacheBox
    scopeRegistration = {enabled=true,scope=application,key=cacheBox},
    // Default Cache Configuration
    defaultCache  = "views",
    // Caches Configuration
    caches      = {}
};
```

> **Info** : We would recommend you create a `config/CacheBox.cfc` and put all your caching configuration there instead of in the main ColdBox configuration file. This will give you further portability and decoupling.

### ConfigFile

An absolute or relative path to the CacheBox configuration CFC or XML file to use instead of declaring the rest of the keys in this structure. So if you do not define a cacheBox structure, the framework will look for the default value: `config/CacheBox.cfc` and it will load it if found. If not found, it will use the default CacheBox configuration found in `/coldbox/system/web/config/CacheBox.cfc`

### ScopeRegistration

A structure that enables scope registration of the CacheBox factory in either server, cluster, application or session scope.

### DefaultCache

The configuration of the default cache which will have an implicit name of default which is a reserved cache name. It also has a default provider of CacheBox which cannot be changed.

### Caches

A structure where you can create more named caches for usage in your CacheBox factory.

## ColdBox

The ColdBox directive is where you configure the framework for operation.

### Application Setup

```javascript
coldbox = {
    // The name of the application
    appName   = "My App",
    // The name of the incoming URL/FORM/REMOTE variable that tells the framework what event to execute. Ex: index.cfm?event=users.list
    eventName = "event"
};
```

> **Info** : Please note that there are no mandatory settings as of ColdBox 4.2.0. If fact, you can remove the config file completely and your app will run. It will be impossible to reinit the app however without a reinit password set.

### Development Settings

```javascript
coldbox = {
    reinitPassword = "h1cker",
    handlersIndexAutoReload = true
};
```

#### **reinitPassword**

Protect the reinitialization of the framework URL actions. For security, if this setting is omitted, we will create a random password. Setting it to an empty string will allow you to reinitialize without a password. **Always have a password set for public-facing sites.**

```text
// reinit with no password
http://localhost/?fwreinit=1
// reinit with password
http://localhost/?fwreinit=mypass
```

#### **handlersIndexAutoReload**

Will scan the conventions directory for new handler CFCs on each request if activated. Use **false** for production, this is only a development true setting.

### Implicit Event Settings

```javascript
coldbox={
    //Implicit Events
    defaultEvent  = "Main.index",
    requestStartHandler     = "Main.onRequestStart",
    requestEndHandler   = "Main.onRequestEnd",
    applicationStartHandler = "Main.onAppInit",
    applicationEndHandler = "Main.onAppEnd",
    sessionStartHandler = "Main.onSessionStart",
    sessionEndHandler = "Main.onSessionEnd",
    missingTemplateHandler = "Main.onMissingTemplate"
}
```

These settings map 1-1 from ColdBox events to the `Application.cfc` life-cycle methods. The only one that is not is the `defaultEvent`, which selects what event the framework will execute when no incoming event is detected via URL/FORM or REMOTE executions.

### Extension Points

The ColdBox extension points are a great way to create federated applications that can reuse a centralized core instead of the local conventions. It is also a great way to extend some core classes with your own.

```javascript
coldbox={
    //Extension Points
    applicationHelper             = "includes/helpers/ApplicationHelper.cfm",
    viewsHelper                    = "",
    modulesExternalLocation        = [],
    viewsExternalLocation        = "",
    layoutsExternalLocation     = "",
    handlersExternalLocation      = "",
    requestContextDecorator     = "",
    controllerDecorator         = ""
}
```

#### **applicationHelper**

A list or array of absolute or relative paths to a UDF helper file. The framework will load all the methods found in this helper file globally. Meaning it will be injected in ALL handlers, layouts and views.

#### **viewsHelper**

A list or array of absolute or relative paths to a UDF helper file. The framework will load all the methods found in this helper in layouts and views only.

#### **modulesExternalLocation**

A list or array of locations of where ColdBox should look for modules to load into your application. The path can be a cf mapping or `cfinclude` compatible location. Modules are searched and loaded in the order of the declared locations. The first location ColdBox will search for modules is the conventions folder `modules`

#### **viewsExternalLocation**

The CF include path of where to look for secondary views for your application. Secondary views look just like normal views except the framework looks in the conventions folder first and if not found then searches this location.

#### **layoutsExternalLocation**

The CF include path of where to look for secondary layouts for your application. Secondary layouts look just like normal layouts except the framework looks in the conventions folder first and if not found then searches this location.

#### **handlersExternalLocation**

The CF dot notation path of where to look for secondary events for your application. Secondary events look just like normal events except the framework looks in the conventions folder first and if not found then searches this location.

#### **requestContextDecorator**

The CF dot notation path of the CFC that will decorate the system Request Context object.

#### **controllerDecorator**

The CF dot notation path of the CFC that will decorate the system Controller

### Exception Handling

```javascript
coldbox = {
    // Error/Exception Handling handler
    exceptionHandler        = "",
    // Invalid HTTP method Handler
    invalidHTTPMethodHandler = "",
    // The handler to execute on invalid events
    invalidEventHandler = "",
    // The default error template    
    customErrorTemplate     = "/coldbox/system/includes/BugReport-Public.cfm"
}
```

#### **exceptionHandler**

The event handler to call whenever ANY non-catched exception occurs anywhere in the request lifecycle execution. Before this event is fired, the framework will log the error and place the exception in the prc as `prc.exception`.

#### **invalidHTTPMethodHandler**

The event handler to call whenever a route or event is accessed with an invalid HTTP method.

#### **invalidEventHandler**

This is the event handler that will fire masking a non-existent event that gets requested. This is a great place to place 302 or 404 redirects whenever non-existent events are being requested.

#### **customErrorTemplate**

The relative path from the application's root level of where the custom error template exists. This template receives a key in the private request collection called `exception` that contains the exception. By default ColdBox does not show robust exceptions, you can turn on robust exceptions by choosing the following template:

```text
coldbox.customErrorTemplate = "/coldbox/system/includes/BugReport.cfm";
```

### Application Aspects

```javascript
coldbox = {
    // Persist handlers
    handlerCaching             = false,
    // Activate event caching
    eventCaching            = true,
    // Activate view  caching
    viewCaching              = true,
    // Return RC struct on Flex/Soap Calls
    proxyReturnCollection     = false,
    // Activate implicit views
    implicitViews           = true,
    // Case for implicit views
    caseSensitiveImplicitViews = true,
    // Auto register all model objects in the `models` folder into WireBox
    autoMapModels     = true
}
```

#### **handlerCaching**

This is useful to be set to false in development and true in production. This tells the framework to cache your event handler objects as singletons.

#### **eventCaching**

This directive tells ColdBox that when events are executed they will be inspected for caching metadata. This does not mean that ALL events WILL be cached if this setting is turned on. It just activates the inspection mechanisms for whenever you annotate events for caching or using the `runEvent()` caching methods.

#### **viewCaching**

This directive tells ColdBox that when views are rendered, the `cache=true` parameter will be obeyed. Turning on this setting will not cause any views to be cached unless you are also passing in the caching parameters to your `renderView()` or `event.setView()` calls.

#### **proxyReturnCollection**

This is a boolean setting used when calling the ColdBox proxy's `process()` method from a Flex or SOAP/REST call. If this setting is set to **true**, the proxy will return back to the remote call the entire request collection structure ALWAYS! If set to **false**, it will return, whatever the event handler returned back. Our best practice is to always have this **false** and return appropriate data back.

#### **implicitViews**

Allows you to use implicit views in your application and view dispatching. You can get a performance boost if you disable this setting.

#### **caseSensitiveImplicitViews**

By default implicit views are case sensitive since ColdBox version 5.2.0, before this version the default was **false**.

#### **autoMapModels**

ColdBox by convention can talk to, use and inject models from the `models` folder by just using their name. However, once you built complex hierarchies you would have to specify the specific hierarchy or folder structure for the model; This is a limitation of conventions. Your choices before v5.2.0 was to map the objects yourself in the WireBox binder. Now, if you turn this setting to **true**, we will issue a `mapDirectory()` on the `models` directory for you.

## Conventions

This element defines custom conventions for your application. By default, the framework has a default set of conventions that you need to adhere too. However, if you would like to implement your own conventions for a specific application, you can use this setting, otherwise do not declare it:

```javascript
//Conventions
conventions = {
    handlersLocation = "controllers",
    viewsLocation      = "views",
    layoutsLocation  = "views",
    modelsLocation      = "model",
    modulesLocation  = "modules",
    eventAction      = "index"
};
```

## Environments

The configuration CFC has embedded environment control and detection built-in. Environments kan be detected by:

* regex matching against cgi.http\_host
* detection of an environmental variable called ENVIRONMENT \( Coldbox 5.2 and higher \)
* usage of a `detectEnvironment()` function

The first option \(regex matching\) is the easiest to use, but not very reliable if you are using multiple hostnames or commandbox for re-initialization.

{% hint style="warning" %}
If you are using `commandbox` please read ALL options below
{% endhint %}

### Default: Regex matching against cgi.http\_host

To detect your environments you will setup a structure called `environments` in your coldbox configuration with the named environments and their associated regular expressions for its `cgi` host names to match for you automatically. If the framework matches the regex with the associated `cgi.http_host`, it will set a setting called `Environment` in your configuration settings and look for that environment setting name in your CFC as a method by convention. That's right, it will check if your CFC has a method with the same name as the environment and if it exists, it will call it for you. Here is where you basically override, remove, or add any settings according to your environment.

> **Warning** : The environment detection occurs AFTER the `configure()` method is called. Therefore, whatever settings or configurations you have on the `configure()` method will be stored first, treat those as **Production** settings.

```javascript
environments = {
    // The key is the name of the environment
    // The value is a list of regex to match against cgi.http_host
    development = "^cf2016.,^lucee.,localhost",
    staging = "^stg"
};
```

The regex match will also create a global setting called "environment" which you can access and use like this:

```javascript
if ( getSetting('environment') == 'development' ){
    doSomeMajik();
}
```

In the above example, I declare a **development** key with a value list of regular expressions. If I am in a host that starts with **cf2016**, this will match and set the environment setting equal to **development**. It will then look for a development method in this CFC and execute it.

```javascript
/**
* Executed whenever the development environment is detected
*/
function development(){
    // Override coldbox directives
    coldbox.handlerCaching = false;
    coldbox.eventCaching = false;
    coldbox.debugPassword = "";
    coldbox.reinitPassword = "";

    // Add dev only interceptors
    arrayAppend( interceptors, {class="#appMapping#.interceptors.CustomLogger} );
}
```

### Detection of an environmental variable called ENVIRONMENT

If you are using environmental variables for your different environments, you can specify an environmental variable called ENVIRONMENT and name it `staging`, `development`, `testing` etcetera, depending on the required environment. As in the regex example, a function named after your environment \(e.g. `staging()` or `development()` \) will be called after your `configure` method.

{% hint style="info" %}
This method is more reliable than relying on cgi.http\_host, since it will never change once configured correctly.
{% endhint %}

### Custom Environment Detection

If you are NOT using environmental variables you can use your own detection algorithm instead of looking at the `cgi.http_host` variable. You will NOT fill out an environments structure but actually create a method with the following signature:

```javascript
string public detectEnvironment(){
}
```

This method will be executed for you at startup and it must return the name of the environment the application is on. You can check for any condition which distinguishes your environment from your other environments. Als long as you return a environment name based on your own logic it will then store it and execute the method if it exists.

## Flash

This directive is how you will configure the [Flash RAM](../../../../digging-deeper/flash-ram/) for operation. Below are the configuration keys and their defaults:

```javascript
// flash scope configuration
flash = {
    // Available scopes are: session,client,cache,mock or your own class path
    scope = "session",
    // constructor properties for the flash scope implementation
    properties = {},
    // automatically inflate flash data into the RC scope at the beginning of a request
    inflateToRC = true, 
    // automatically inflate flash data into the PRC scope at the beginning of a request
    inflateToPRC = false, 
    // automatically purge flash data for you
    autoPurge = true, 
    // automatically save flash scopes at end of a request and on relocations.
    autoSave = true 
};
```

## InterceptorSettings

This structure configures the interceptor service in your application.

```javascript
//Interceptor Settings
interceptorSettings = {
    throwOnInvalidStates = false,
    customInterceptionPoints = "onLogin,onWikiTranslation,onAppClose"
};
```

### `throwOnInvalidStates`

This tells the interceptor service to throw an exception if the state announced for interception is not valid or does not exist. Defaults to **false**.

### `customInterceptionPoints`

This key is a comma delimited list or an array of custom interception points you will be registering for custom announcements in your application. This is the way to provide an observer-observable pattern to your applications.

> **Info** Please see the [Interceptors](../../../../digging-deeper/interceptors/) section for more information.

## Interceptors

This is an array of interceptor definitions that you will use to register in your application. The key about this array is that **ORDER** matters. The interceptors will fire in the order that you register them whenever their interception points are announced, so please watch out for this caveat. Each array element is a structure that describes what interceptor to register.

```javascript
//Register interceptors as an array, we need order
interceptors = [

    { 
        // The CFC instantiation path
        class="",
        // The alias to register in WireBox, if not defined it uses the name of the CFC
        name="",
        // A struct of data to configure the interceptor with.
        properties={}
    }

    { class="mypath.MyInterceptor",
      name="MyInterceptor",
      properties={useSetterInjection=false}
    },
    { class="coldbox.system.interceptors.SES", name="MySES" }
];
```

> **Warning** : Important: Order of declaration matters! Also, when declaring multiple instances of the same CFC \(interceptor\), make sure you use the name attribute in order to distinguish them. If not, only one will be registered \(the last one declared\).

## Layouts

The layouts array element is used to define implicit associations between layouts and views/folders, this does not mean that you need to register ALL your layouts. This is a convenience for pairing them, we are in a conventions framework remember.

Before any renderings occur or lookups, the framework will check this array of associations to try and match in what layout a view should be rendered in. It is also used to create aliases for layouts so you can use aliases in your code instead of the real file name and locations.

```javascript
//Register Layouts
layouts = [
    { 
        // The alias of a layout
        name="",
        // The layout file
        file="",
        // A list of view names to render within this layout
        views="",
        // A list of regex names to match a view
        folders=""
    }

    // Examples
    { name="tester",file="Layout.tester.cfm",views="vwLogin,test",folders="tags,pdf/single"    },
    { name="login",file="Login.cfm",folders="^admin/security"}
];
```

## LayoutSettings

This structure allows you to define a system-wide default layout and view.

```javascript
//Layout Settings
layoutSettings = {
    // The default layout to use if NOT defined in a request
    defaultLayout = "Main.cfm",
    // The default view to use to render if NOT defined in a request
    defaultView   = "youForgot.cfm"
};
```

> **Hint** Please remember that the default layout is `Main.cfm`

## LogBox

The logBox structure is based on the LogBox declaration DSL, see the [LogBox Documentation](https://logbox.ortusbooks.com/) for much more information.

```javascript
//LogBox DSL
logBox = {
    // The configuration file without fileextension to use for operation, instead of using this structure
    configFile = "config/LogBox", 
    // Appenders
    appenders = {
        appenderName = {
            class="class.to.appender", 
            layout="class.to.layout",
            levelMin=0,
            levelMax=4,
            properties={
                name  = value,
                prop2 = value 2
            }
    },
    // Root Logger
    root = {levelMin="FATAL", levelMax="DEBUG", appenders="*"},
    // Granualr Categories
    categories = {
        "coldbox.system" = { levelMin="FATAL", levelMax="INFO", appenders="*"},
        "model.security" = { levelMax="DEBUG", appenders="console"}
    }
    // Implicit categories
    debug  = ["coldbox.system.interceptors"],
    info   = ["model.class", "model2.class2"],
    warn   = ["model.class", "model2.class2"],
    error  = ["model.class", "model2.class2"],
    fatal  = ["model.class", "model2.class2"],
    off    = ["model.class", "model2.class2"]
};
```

> **Info** : If you do not define a logBox DSL structure, the framework will look for the default configuration file `config/LogBox.cfc`. If it does not find it, then it will use the framework's default logging settings.

**ConfigFile**

You can use a configuration CFC instead of inline configuration by using this setting. The default value is `config/LogBox.cfc`, so by convention you can just use that location. If no values are defined or no config file exists, the default configuration file is `coldbox/system/web/config/LogBox.cfc`.

## Modules

The modules structure is used to configure the behavior of the [ColdBox Modules](../../../../hmvc/modules/).

```javascript
modules = {
    // Will auto reload the modules in each request. Great for development but can cause some loading/re-loading issues
    autoReload = true,
    // An array of modules to load ONLY
    include = [],
    // An array of modules to EXCLUDE for operation
    exclude = [ "paidModule1", "paidModule2" ]
};
```

> **Danger** Please be very careful when using the `autoReload` flag as module routing can be impaired and thread consistency will also suffer. This is PURELY a development flag that you can use at your own risk.

## ModuleSettings

This structure is used to house module configurations. Please refer to each module's documentation on how to create the configuration structures. Usually the keys will match the name of the module to be configured.

```javascript
component {

     function configure() {

         moduleSettings = {
             myModule = {
                someSetting = "overridden"
             }
        };
    }
}
```

## Settings

These are custom application settings that you can leverage in your application.

```javascript
// Custom Settings
settings = {
    useSkins = true,
    myCoolArray = [1,2,3,4],
    skinsPath = "views/skins",
    myUtil = createObject("component","#appmapping#.model.util.MyUtility")
};
```

You can read our [Using Settings](../../using-settings.md) section to discover how to use all the settings in your application.

## WireBox

This configuration structure is used to configure the [WireBox](https://wirebox.ortusbooks.com) dependency injection framework embedded in ColdBox.

```javascript
// wirebox integration
wirebox = {
    binder = 'config.WireBox',
    singletonReload = true
};
```

**binder**

The location of the WireBox configuration binder to use for the application. If empty, we will use the binder in the `config` folder called by conventions: `WireBox.cfc`

**singletonReload**

A great flag for development. If enabled, WireBox will flush its singleton objects on every request so you can develop without any headaches of reloading.

> **Warning** : This operation can cause some thread issues and it is only meant for development. Use at your own risk.

## System Settings \(Java Properties and Environment Variables\)

ColdBox makes it easy to access the configuration stored in your Java system properties and your server's environment variables, even if you don't know which one it is in! Three methods are provided for your convenience:

| Name | Arguments | Description |
| :--- | :--- | :--- |
| `getSystemSetting` | `( key, defaultValue )` | Looks for `key` in properties first, env second. Returns the `defaultValue` if neither exist. |
| `getSystemProperty` | `( key, defaultValue )` | Returns the Java System property for `key`. Returns the `defaultValue` if it does not exist. |
| `getEnv` | `( key, defaultValue )` | Returns the server environment variable for `key`. Returns the `defaultValue` if it does not exist. |

### Accessing System Settings in `config/ColdBox.cfc` or a `ModuleConfig.cfc`

If you are inside `config/ColdBox.cfc` or a `ModuleConfig.cfc` or a `config/WireBox.cfc` you can use the three system settings functions directly! No additional work required.

### Accessing System Settings in `Application.cfc`

If you would like to access these methods in your `Application.cfc`, create an instance of `coldbox.system.core.util.Util` and access them off of that component. This is required when adding a datasource from environment variables.

Example:

{% code title="Application.cfc" %}
```javascript
component {

    variables.util = new coldbox.system.core.util.Util();

    this.datasources[ "my_datasource" ] = {
        driver = util.getSystemSetting( "DB_DRIVER" ),
        host = util.getSystemSetting( "DB_HOST" ),
        port = util.getSystemSetting( "DB_PORT" ),
        database = util.getSystemSetting( "DB_DATABASE" ),
        username = util.getSystemSetting( "DB_USERNAME" ),
        password = util.getSystemSetting( "DB_PASSWORD" )
    };

}
```
{% endcode %}

### Accessing System Settings in other files

If you need to access these configuration values in other components, consider adding the values to your [ColdBox settings](configuration-directives/settings.md) and injecting the values into your other components [via dependecy injection.](../using-settings.md)

## Using Settings

The ColdBox Controller \(stored in ColdFusion `application` scope\) stores all your **application** settings and also your **system** settings:

* **ColdboxSettings** : Framework specific system settings
* **ConfigSettings** : Your application settings

You can use the following methods to retrieve/set/validate settings in your handlers/layouts/views and interceptors:

{% code title="FrameworkSuperType.cfc" %}
```javascript
/**
 * Get a setting from the system
 * @name The key of the setting
 * @fwSetting Retrieve from the config or fw settings, defaults to config
 * @defaultValue If not found in config, default return value
 */
function getSetting( required name, boolean fwSetting=false, defaultValue )

/**
 * Verify a setting from the system
 * @name The key of the setting
 * @fwSetting Retrieve from the config or fw settings, defaults to config
 */
boolean function settingExists( required name, boolean fwSetting=false )

/**
 * Set a new setting in the system
 * @name The key of the setting
 * @value The value of the setting
 *
 * @return FrameworkSuperType
 */
any function setSetting( required name, required value )
```
{% endcode %}

You can also get access to these methods via the ColdBox Controller component:

```javascript
controller.getSetting()
controller.setSetting()
controller.settingExists()
controller.getConfigSettings()
controller.getColdBoxSettings()
```

### Injecting Settings

You can use the WireBox injection DSL to inject settings in your models or non-coldbox objects. Below are the available DSL notations:

* `coldbox:setting:{key}` : Inject a specified config setting key
* `coldbox:fwsetting:{key}` : Inject a specified system setting key
* `coldbox:configSettings` : Inject a reference to the application settings structure
* `coldbox:fwSettings` : Inject a reference to the ColdBox System settings structure

```javascript
component{

    property name="mysetting" inject="coldbox:setting:mysetting";
    property name="path" inject="coldbox:fwSetting:path";
    property name="config" inject="coldbox:configSettings";
    property name="settings" inject="coldbox:fwSettings";

}
```

## Bootstrapper - Application.cfc

The `Application.cfc` is one of the most important files in your application as it is where you define all the implicit ColdFusion engine events, session, client scopes, ORM, etc. It is also how you tell ColdFusion to bootstrap the ColdBox Platform for your application. There are two ways to bootstrap your application:

1. Leverage composition and bootstrap ColdBox \(**Default**\)
2. Leverage inheritance and bootstrap ColdBox

{% hint style="info" %}
The composition approach allows you to have a more flexible configuration as it will allow you to use per-application mappings for the location of the ColdBox Platform.
{% endhint %}

{% hint style="success" %}
**Tip:** To see the difference, just open the appropriate `Application.cfc` in the application templates.
{% endhint %}

### Composition

{% code title="Application.cfc" %}
```javascript
component{
    // Application properties
    this.name = hash( getCurrentTemplatePath() );
    this.sessionManagement = true;
    this.sessionTimeout = createTimeSpan(0,0,30,0);
    this.setClientCookies = true;

    // COLDBOX STATIC PROPERTY, DO NOT CHANGE UNLESS THIS IS NOT THE ROOT OF YOUR COLDBOX APP
    COLDBOX_APP_ROOT_PATH = getDirectoryFromPath( getCurrentTemplatePath() );
    // The web server mapping to this application. Used for remote purposes or static purposes
    COLDBOX_APP_MAPPING   = "";
    // COLDBOX PROPERTIES
    COLDBOX_CONFIG_FILE      = "";
    // COLDBOX APPLICATION KEY OVERRIDE
    COLDBOX_APP_KEY          = "";

    // application start
    public boolean function onApplicationStart(){
        application.cbBootstrap = new coldbox.system.Bootstrap( COLDBOX_CONFIG_FILE, COLDBOX_APP_ROOT_PATH, COLDBOX_APP_KEY, COLDBOX_APP_MAPPING );
        application.cbBootstrap.loadColdbox();
        return true;
    }

    // request start
    public boolean function onRequestStart(String targetPage){
        // Process ColdBox Request
        application.cbBootstrap.onRequestStart( arguments.targetPage );

        return true;
    }

    public void function onSessionStart(){
        application.cbBootStrap.onSessionStart();
    }

    public void function onSessionEnd( struct sessionScope, struct appScope ){
        arguments.appScope.cbBootStrap.onSessionEnd( argumentCollection=arguments );
    }

    public boolean function onMissingTemplate( template ){
        return application.cbBootstrap.onMissingTemplate( argumentCollection=arguments );
    }

}
```
{% endcode %}

### Inheritance

{% code title="Application.cfc" %}
```javascript
component extends="coldbox.system.Bootstrap"{

    // Application properties
    this.name = hash( getCurrentTemplatePath() );
    this.sessionManagement = true;
    this.sessionTimeout = createTimeSpan(0,0,30,0);
    this.setClientCookies = true;

    // COLDBOX STATIC PROPERTY, DO NOT CHANGE UNLESS THIS IS NOT THE ROOT OF YOUR COLDBOX APP
    COLDBOX_APP_ROOT_PATH = getDirectoryFromPath( getCurrentTemplatePath() );
    // The web server mapping to this application. Used for remote purposes or static purposes
    COLDBOX_APP_MAPPING   = "";
    // COLDBOX PROPERTIES
    COLDBOX_CONFIG_FILE      = "";
    // COLDBOX APPLICATION KEY OVERRIDE
    COLDBOX_APP_KEY          = "";
}
```
{% endcode %}

### Directives

You can set some variables in the `Application.cfc` that can alter Bootstrapping conditions:

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| `COLDBOX_APP_ROOT_PATH` | App Directory | Automatically set for you. This path tells the framework what is the base root location of your application and where it should start looking for all the agreed upon conventions. You usualy will never change this, but you can. |
| `COLDBOX_APP_MAPPING` | `/` | The application mapping is ESSENTIAL when dealing with Flex or Remote \(SOAP\) applications. This is the location of the application from the root of the web root. So if your app is at the root, leave this setting blank. If your application is embedded in a sub-folder like MyApp, then this setting will be auto-calculated to `/MyApp`. |
| `COLDBOX_CONFIG_FILE` | `config/ColdBox.cfc` | The absolute or relative path to the configuration CFC file to load. This bypasses the conventions and uses the configuration file of your choice. |
| `COLDBOX_APP_KEY` | `cbController` | The name of the key the framework will store the application controller under in the application scope. |

#### Lock Timeout

The Boostrapper also leverages a default locking timeout of 30 seconds when doing loading operations. You can modify this timeout by calling the `setLockTimeout()` method on the Bootsrapper object.

```javascript
application.bootstrapper.setLockTimeout( 10 );
```

