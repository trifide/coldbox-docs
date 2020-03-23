# For Newbies

## 60 Minute Quick Start

This guide has been designed to get you started with ColdBox in fewer than 60 minutes. We will take you by the hand and help you build a RESTFul application in 60 minutes or fewer. After you complete this guide, we encourage you to move on to the [Getting Started Guide](../../getting-started/getting-started-guide.md) and then to the other guides in this book.

### Requirements

* Please make sure you download and install the latest [CommandBox CLI](https://www.ortussolutions.com/products/commandbox). We will show you how in the [Installing ColdBox section](installing-coldbox.md).
* Grab a cup of coffee
* Get comfortable

## Installing ColdBox

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

That's it. CommandBox can now track this version of ColdBox for you in this directory. In the [next section](my-first-coldbox-application.md) we will scaffold a ColdBox application using an application template.

{% hint style="success" %}
You can find many scaffolding templates for ColdBox in our Github organization: [github.com/coldbox-templates](https://github.com/coldbox-templates)
{% endhint %}

### Uninstalling ColdBox

To uninstall ColdBox from this application folder just type `uninstall coldbox`.

### Updating ColdBox

To update ColdBox from a previous version, just type `update coldbox`.

## My First ColdBox Application

CommandBox comes with a `coldbox create app` command that can enable you to create application skeletons using one of our official skeletons or [your own](../../digging-deeper/recipes/application-templates.md):

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
You can find all our template skeletons here: [github.com/coldbox-templates](https://github.com/coldbox-templates)
{% endhint %}

### Scaffolding Our Application

So let's create our first app using the _default_ template skeleton **AdvancedScript**:

```bash
coldbox create app MyApp
```

This will scaffold the application and also install ColdBox for you. The following folders/files are generated for you:

```text
+coldbox // The ColdBox framework library (CommandBox Tracked)
+config // Configuration files
+handlers // Your handlers/controllers
+includes // static assets
+interceptors // global interceptors
+layouts // Your layouts
+models // Your Models
+modules // CommandBox Tracked Modules
+modules_app // Custom modules
+tests // Test harness
+views // Your Views
+Application.cfc // Bootstrap
+box.json // CommandBox package descriptor
+index.cfm // Front controller
```

Now let's start a server so we can see our application running:

```text
server start --rewritesEnable
```

{% hint style="info" %}
This will start up a [Lucee](https://www.lucee.org) 5 open source CFML engine \(If you are in CommandBox 4\). If you would like an **Adobe ColdFusion** server then just add to the command: `cfengine=adobe@{version}` where `{version}` can be: `2016,11,10,9.`

If you are using CommandBox 3 and below, you will be using a Lucee 4.5 Server.
{% endhint %}

This command will start a server with URL rewrites enabled, open a web browser for you and execute the `index.cfm`which in turn executes the **default event** by convention in a ColdBox application: `main.index`.

{% hint style="success" %}
**Tip:** ColdBox Events map to handlers \(**cfc**\) and appropriate actions \(**functions**\)
{% endhint %}

That's it, you have just created your first application. Hooray, onward!

{% hint style="success" %}
**Tip:** Type `coldbox create app help` to get help on all the options for creating ColdBox applications.
{% endhint %}

### File/Folder Conventions

ColdBox is a conventions based framework. The location of files and functions matter. Since we scaffolded our first application, let's write down in a table below with the different conventions that exist in ColdBox.

| **File/Folder Convention** | **Mandatory** | **Description** |
| :--- | :--- | :--- |
| `config/Coldbox.cfc` | false | The application configuration file |
| `config/Router.cfc` | false | The application URL router |
| `handlers` | false | Event Handlers \(controllers\) |
| `layouts` | false | Layouts |
| `models` | false | Model objects |
| `modules` | false | CommandBox Tracked Modules |
| `modules_app` | false | Custom Modules You Write |
| `views` | false | Views |

{% hint style="info" %}
What is the common denominator in all the conventions? That they are all optional.
{% endhint %}

### Re-initializing The Application

There will be times when you make configuration or code changes that are not reflected immediately in the application due to caching. You can tell the framework to \_reinit \_or restart the application for you via the URL by leveraging the special URL variable `fwreinit`.

```text
http://localhost:{port}/?fwreinit=1
```

You can also use CommandBox to reinit the application:

```bash
coldbox reinit
```

{% hint style="success" %}
**Tip:** You can add a password to the **reinit** procedures for further security, please see the [configuration section](https://github.com/ortus-docs/coldbox-docs/tree/7a8d2250f812e1b65cfc9c2888a8489110724897/the-basics/configuration/coldbox.cfc).
{% endhint %}

## My First Handler & View

Now let's create our first controller, which in ColdBox is called **Event Handler**. Let's go to CommandBox again:

```bash
coldbox create handler name="hello" actions="index"
```

This will generate the following files:

* A new handler called `hello.cfc` inside of the `handlers` folder
* A view called `index.cfm` in the `views/hello` folder 
* An integration test at `tests/specs/integration/helloTest.cfc`. 

Now go to your browser and the following URL to execute the generated event:

```text
# With rewrites enabled
http://localhost:{port}/hello/index

# With no rewrites enabled
http://localhost:{port}/index.cfm/hello/index
```

You will now see a big `hello.index` outputted to the screen. You have now created your first handler and view combination.

### Handler Code

Let's check out the handler code:

```java
component{

    /**
     * Default Action
     */
     function index( event, rc, prc ){
        event.setView( "hello/index" );
     }


}
```

As you can see, a handler is a simple CFC with functions on them. Each function maps to an **action** that is executed via the URL. The default action in ColdBox is `index()`which receives three arguments:

* `event` - An object that models and is used to work with the current request
* `rc` - A struct that contains both `URL/FORM` variables \(unsafe data\)
* `prc` - A secondary struct that is **private** only settable from within your application \(safe data\)

{% hint style="info" %}
The **event** object is used for many things, in the case of this function we are calling a `setView()` method which tells the framework what view to render to the user once execution of the action terminates.
{% endhint %}

{% hint style="success" %}
**Tip:** The view is not rendered in line 7, but rendered after the execution of the action by the framework.
{% endhint %}

### Executing Events

Did you detect a convention here?

The sections in the URL are the same as the name of the event handler CFC \(`hello.cfc`\) and method that was generated `index()`. By convention, this is how you execute events in ColdBox by leveraging the following URL pattern that matches the name of a handler and action function.

{% hint style="info" %}
You can also nest handlers into folders and you can also pass the name of the folder\(s\) as well.
{% endhint %}

```text
http://localhost:{port}/folder/handler/action
http://localhost:{port}/handler/action
http://localhost:{port}/handler
```

If no `action` is defined in the URL then the default action of `index` will be used.

All of this URL magic happens thanks to the URL mappings capabilities in ColdBox. By convention, you can write beautiful URLs that are RESTFul and by convention. You can also extend them and create more expressive URL Mappings by leveraging the `config/Router.cfc` which is your application router.

{% hint style="success" %}
**Tip:** Please see the [event handlers](../../the-basics/event-handlers/) guide for more in-depth information.
{% endhint %}

### My First Virtual Event

Now let's create a virtual event, which is basically just a view we want to execute with no event handler controller needed. This is a great way to incorporate non-mvc files into ColdBox, baby steps!

```bash
coldbox create view name="virtual/hello"
```

Open the view now \(`/views/virtual/hello.cfm`\) and add the following:

```markup
<h1>Hello from ColdBox Land!</h1>
```

Then go execute the virtual event:

```text
http://localhost:{port}/virtual/hello
```

You will get the `Hello From ColdBox Land!` displayed! This is a great way to create tests or even bring in legacy/procedural templates into an MVC framework.

{% hint style="success" %}
**Tip:** You can see our [layouts and views](../../the-basics/layouts-and-views/) section for more in-depth information.
{% endhint %}

## Linking Events Together

ColdBox provides you with a nice method for generating links between events by leveraging an object called `event` that is accessible in all of your layouts/views and event handlers. This `event` object is called behind the scenes the **request context object,** which models the incoming request and even contains all of your incoming `FORM` and `URL` variables in a structure called `rc`.

{% hint style="success" %}
**Tip**: You will use the event object to set views, set layouts, set HTTP headers, read HTTP headers, convert data to other types \(json,xml,pdf\), and much more.
{% endhint %}

### Building Links

The method in the request context that builds links is called: `buildLink()`. Here are some of the arguments you can use:

```java
/**
* Builds links to events or URL Routes
*
* @to The event or route path you want to create the link to
* @translate Translate between . to / depending on the SES mode on to and queryString arguments. Defaults to true.
* @ssl Turn SSl on/off on URL creation, by default is SSL is enabled, we will use it.
* @baseURL If not using SES, you can use this argument to create your own base url apart from the default of index.cfm. Example: https://mysample.com/index.cfm
* @queryString The query string to append
*/
string function buildLink(
    to,
    boolean translate=true,
    boolean ssl,
    baseURL="",
    queryString="");
```

### Edit Your View

Edit the `views/virtual/hello.cfm` page and wrap the content in a `cfoutput` and create a link to the main ColdBox event, which by convention is `main.index`.

```text
<cfoutput>
<h1>Hello from ColdBox Land!</h1>
<p><a href="#event.buildLink( "main.index" )#">Go home</a></p>
</cfoutput>
```

This code will generate a link to the `main.index` event in a search engine safe manner and in SSL detection mode. Go execute the event: `http://localhost:{port}/virtual/hello` and click on the generated URL, you will now be navigating to the default event `/main/index`. This technique will also apply to FORM submissions:

```markup
<form action="#event.buildLink( 'user.save' )#" method="post">
...
</form>
```

{% hint style="success" %}
**Tip** You can visit our API Docs for further information about the `event` object and the `buildLink` method: [http://apidocs.ortussolutions.com/coldbox/current/index.html?coldbox/system/web/context/RequestContext.html](http://apidocs.ortussolutions.com/coldbox/current/index.html?coldbox/system/web/context/RequestContext.html).

For extra credit try to use more of the `buildLink` arguments.
{% endhint %}

### URL Structure & Mappings

ColdBox allows you to manipulate the incoming URL so you can create robust URL strategies especially for RESTFul services. This is all done by convention and you can configure it via the application router: `config/Router.cfc` for more granular control.

{% hint style="info" %}
Out of the box we provide you with convention based routing that maps the URL to modules/folders/handlers and actions.

`route( "/:handler/:action" ).end()`
{% endhint %}

We have now seen how to execute events via nice URLs. Behind the scenes, ColdBox translates the URL into an executable event string just like if you were using a normal URL string:

* `/main/index` -&gt; `?event=main.index`
* `/virtual/hello` -&gt; `?event=virtual.hello`
* `/admin/users/list` -&gt; `?event=admin.users.list`
* `/handler/action/name/value` -&gt; `?event=handler.action&name=value`
* `/handler/action/name` -&gt; `?event=handler.action&name=`

By convention, any name-value pairs detected after an event variable will be treated as an incoming `URL` variables. If there is no pair, then the value will be an empty string.

{% hint style="success" %}
**Tip:** By default the ColdBox application templates are using full URL rewrites. If your web server does not support them, then open the `config/Router.cfc` and change the full rewrites method to **false**: `setFullRewrites( false ).`
{% endhint %}

## Working With Event Handlers

Event handlers are the controller layer in ColdBox and is what you will be executing via the `URL`or a `FORM`post. All event handlers are **singletons**, which means they are cached for the duration of the application, so always remember to var scope your variables in your functions.

{% hint style="success" %}
**Tip:** For development we highly encourage you to turn handler caching off or you will have to reinit the application in every request, which is annoying. Open the `config/ColdBox.cfc` and look for the `coldbox.handlerCaching` setting.
{% endhint %}

Once you started the server in the previous section and opened the browser, the default event got executed which maps to an event handler CFC \(controller\) `handlers/main.cfc` and the method/action in that CFC called `index()`. Go open the `handlers/main.cfc` and let's explore the code.

### Handler Code

```javascript
// Default Action
function index( event, rc, prc ){
    prc.welcomeMessage = "Welcome to ColdBox!";
    event.setView( "main/index" );
}
```

Every action in ColdBox receives three arguments:

* `event` - An object that models and is used to work with the current request
* `rc` - A struct that contains both URL/FORM variables \(unsafe data\)
* `prc` - A secondary struct that is private only settable from within your application \(safe data\)

This line `event.setView( "main/index" )` told ColdBox to render a view back to the user found in `views/main/index.cfm` using a default layout, which by convention is called `Main.cfm` which can be found in the `layouts` folder.

### Executing Events

We have now seen how to add handlers via CommandBox using the `coldbox create handler` command and also execute them by convention by leveraging the following URL pattern:

```text
http://localhost:{port}/folder/handler/action
http://localhost:{port}/handler/action
http://localhost:{port}/handler
```

Also remember, that if no `action` is defined in the incoming URL then the default action of `index` will be used.

{% hint style="info" %}
Remember that the URL mappings support in ColdBox is what allows you to execute events in such a way from the URL. These are controlled by your application router: `config/Router.cfc`
{% endhint %}

### Working With Incoming Data

Now, let's open the handler we created before called `handlers/hello.cfc` and add some public and private variables to it so our views can render the variables.

```javascript
function index( event, rc, prc ){
    // param an incoming variable.
    event.paramValue( "name", "nobody" );
    // set a private variable
    prc.when = dateFormat( now(), "full" );
    // set the view to render
    event.setView( "hello/index" );
}
```

Let's open the view now: `views/hello/index.cfm` and change it to this:

```markup
<cfoutput>
<p>Hello #encodeForHTML( rc.name )#, today is #prc.when#</p>
</cfoutput>
```

{% hint style="danger" %}
Please note that we used the ColdFusion function `encodeForHTML()` \([https://www.cfdocs.org/encodeforhtml](https://www.cfdocs.org/encodeforhtml)\) on the public variable. Why? Because you can **never** trust the client and what they send, make sure you use the built-in ColdFusion encoding functions in order to avoid XSS hacks or worse on incoming public \(`rc`\) variables.
{% endhint %}

If you execute the event now: `http://localhost:{port}/hello/index` you will see a message of `Hello nobody`.

Now change the incoming URL to this: `http://localhost:{port}/hello/index?name=ColdBox` and you will see a message of `Hello ColdBox`.

{% hint style="success" %}
**Tip:** Please see the [layouts and views](../../the-basics/layouts-and-views/) section for in-depth information about them.
{% endhint %}

## Adding A Layout

Every time the framework renders a view, it will try to leverage the default layout which is located in `layouts/Main.cfm` by convention. This is an HTML file that gives format to your output and contains the location of where the view you want should be rendered.

### Layout Code

This location is identified by the following code:

```markup
<div id="maincontent">
#renderView()#
</div>
```

The call to the `renderView()` method with no arguments tells the framework to render the view that was set using `event.setView()`. This is called a **rendering region**. You can use as many rendering regions within layouts or even within views themselves.

{% hint style="info" %}
**Named Regions:** The `setView()` method even allows you to name these regions and then render them in any layout or other views using their name.
{% endhint %}

### Creating A Layout

Let's create a new simple layout with two rendering regions. Open up CommandBox and issue the following commands:

```bash
# Create a Funky layout
coldbox create layout name="Funky"

# Create a footer
coldbox create view name="main/footer"
```

Open the `layouts/Funky.cfm` layout and let's modify it a bit by adding the footer view as a rendering region.

```markup
<h1>funky Layout</h1>
<cfoutput>#renderView()#</cfoutput>

<hr>
<cfoutput>#renderView( "main/footer" )#</cfoutput>
```

### Using The Layout

Now, let's open the handler we created before called `handlers/hello.cfc` and add some code to use our new layout explicitly via adding a `layout` argument to our `setView()` call.

```javascript
function index( event, rc, prc ){
    // param an incoming variable.
    event.paramValue( "name", "nobody" );
    // set a private variable
    prc.when = dateFormat( now(), "full" );

    // set the view to render with our new layout
    event.setView( view="hello/index", layout="Funky" );
}
```

Go execute the event now: `http://localhost:{port}/hello/index` and you will see the view rendered with the words `funky layout` and `footer view` at the bottom. Eureka, you have now created a layout.

{% hint style="info" %}
You can also leverage the function `event.setLayout( "Funky" )` to change layouts and even concatenate the calls:

`event.setView( "hello/index" )    
.setLayout( "Funky" );`
{% endhint %}

## Adding A Model

Let's complete our saga into MVC by developing the **M**, which stands for [model](https://en.wikipedia.org/wiki/Domain_model). This layer is all your business logic, queries, external dependencies, etc. of your application, which represents the problem to solve or the domain to solve.

### WireBox

This layer is controlled by [WireBox](https://wirebox.ortusbooks.com), the dependency injection framework within ColdBox, which will give you the flexibility of wiring your objects and persisting them for you.

### Creating A Service Model

Let's create a simple contact listing, so open up CommandBox and issue the following command:

```bash
coldbox create model name="ContactService" methods="getAll" persistence="singleton"
```

This will create a `models/ContactService.cfc` with a `getAll()` method and a companion unit test at `tests/specs/unit/ContactServiceTest.cfc`. Let's open the model object:

```javascript
component singleton accessors="true"{      

    ContactService function init(){         
        return this;     
    }

    function getAll(){      

    }

}
```

{% hint style="warning" %}
Notice the `singleton` annotation on the component tag. This tells WireBox that this service should be cached for the entire application life-span. If you remove the annotation, then the service will become a _transient_ object, which means that it will be re-created every time it is requested.
{% endhint %}

### Add Some Data

Let's mock an array of contacts so we can display them later. We can move this to a SQL call later.

```javascript
component singleton accessors="true"{

    ContactService function init(){
        variables.data = [
            { id=1, name="coldbox" },
            { id=2, name="superman" },
            { id=3, name="batman" }
        ];    
        return this;

    }

    function getAll(){
        return variables.data;
    }

}
```

{% hint style="success" %}
We also have created a project to mock any type of data: [MockDataCFC](https://www.forgebox.io/view/mockdatacfc). Just use CommandBox to install it: `install mockdatacfc`

You can then leverage it to mock your contacts or any simple/complex data requirement.
{% endhint %}

### Wiring Up The Model To a Handler

We have now created our model so let's tell our event handler about it. Let's create a new handler using CommandBox:

```bash
coldbox create handler name="contacts" actions="index"
```

This will create the `handler/contacts.cfc` handler with an `index()` action, the `views/contacts/index.cfm` view and the accompanying integration test `tests/specs/integration/contactsTest.cfc`.

Let's open the handler and add a new ColdFusion `property` that will have a reference to our model object.

```javascript
component{ 

    property name="contactService" inject="ContactService";

    any function index( event, rc, prc ){ 
        event.setView( "contacts/index" ); 
    }
}
```

Please note that `inject` annotation on the `property` definition. This tells WireBox what model to inject into the handler's `variables`scope.

By convention it looks in the `models` folder for the value, which in our case is `ContactService`. Now let's call it and place some data in the private request collection `prc` so our views can use it.

```javascript
any function index( event, rc, prc ){
    prc.aContacts = contactService.getAll();
    event.setView( "contacts/index" );
}
```

### Presenting The Data

Now that we have put the array of contacts into the `prc` struct as `aContacts`, let's display it to the screen using ColdBox's HTML Helper.

The ColdBox HTML Helper is a companion class that exists in all layouts and views that allows you to generate semantic HTML5 without the needed verbosity of nesting, or binding to ORM/Business objects.

{% hint style="info" %}
Please check out the API Docs to discover the HTML Helper: [http://apidocs.ortussolutions.com/coldbox/current/index.html?coldbox/system/modules/HTMLHelper/models/HTMLHelper.html](http://apidocs.ortussolutions.com/coldbox/current/index.html?coldbox/system/modules/HTMLHelper/models/HTMLHelper.html)
{% endhint %}

Open the `contacts/index.cfm` and add the following to the view:

```markup
<cfoutput>
<h1>My Contacts</h1>

#html.table( data=prc.aContacts, class="table table-striped" )#
</cfoutput>
```

{% hint style="warning" %}
Note: If your models are `singletons`, they will persist for the life-span of your ColdFusion application. To see code changes for singletons, you have to reinit the framework by using the `?fwreinit={password}` Url action or via CommandBox using `coldbox reinit`. Please check out the API Docs to discover CommandBox: \[[https://apidocs.ortussolutions.com/commandbox/4.5.0/index.html](https://apidocs.ortussolutions.com/commandbox/4.5.0/index.html)\]
{% endhint %}

That's it! Execute the event: `http://localhost:{port}/contacts/index` and view the nice table of contacts being presented to you.

Congratulations, you have made a complete **MVC** circle!

> **Tip** You can find much more information about models and dependency injection in our [full docs](https://coldbox.ortusbooks.com/the-basics/models)

## RESTFul Data

Out of the box, ColdBox gives you all the RESTFul capabilities you will need to create robust and scalable RESTFul services. Let's add some RESTFul capabilities to our contact listing we created in the previous section.

{% hint style="success" %}
**Tip:** You can find much more information about building ColdBox RESTFul services in our [full docs.](../../digging-deeper/recipes/building-rest-apis.md)
{% endhint %}

### Producing JSON

If you know beforehand what type of format you will be responding with, you can leverage ColdBox 5's auto-marshalling in your handlers. By default, ColdBox detects any return value from handlers and if they are complex it will convert them to JSON automatically for you:

{% code title="contacts.cfc" %}
```javascript
any function index( event, rc, prc ){
    return contactService.getAll();    
}
```
{% endcode %}

That's it! ColdBox detects the array and automatically serializes it to JSON. Easy Peasy!

If you want your incoming JSON requestbody to be handled the same as FORM and URL variables you can enable `coldbox.jsonPayloadToRC = true` in your coldbox config. See releasenotes [`5.1.0`](../../intro/introduction/whats-new-with-5.1.0.md#new-auto-deserialization-of-json-payloads) and [5.1.2 ](../../intro/introduction/whats-new-with-5.1.2.md#automatic-json-payload-setting)for details.

### renderData\(\)

The request context object has a special function called `renderData()` that can take any type of data and marshall it for you to other formats like `xml, json, wddx, pdf, text, html` or your own type.

{% hint style="success" %}
**Tip**: You can find more information at the API Docs for `renderData()` here [http://apidocs.ortussolutions.com/coldbox/current/index.html?coldbox/system/web/context/RequestContext.html\#renderData\(](http://apidocs.ortussolutions.com/coldbox/current/index.html?coldbox/system/web/context/RequestContext.html#renderData%28)\)
{% endhint %}

So let's open the `handlers/contacts.cfc` and add to our current code:

{% code title="contacts.cfc" %}
```javascript
any function index( event, rc, prc ){
    prc.aContacts = contactService.getAll();    
    event.renderData( data=prc.aContacts, formats="xml,json,pdf,html" );
}
```
{% endcode %}

We have added the following line:

```javascript
event.renderData( data=prc.aContacts, formats="xml,json,pdf,html" );
```

This tells ColdBox to render the contacts data in 4 formats: xml, json, pdf and html. WOW! So how would you trigger each format? Via the URL of course.

### Format Detection

ColdBox has the ability to detect formats via URL extensions or an incoming `Accepts` header. If no extension is sent, then ColdBox attempts to determine the format by inspecting the `Accepts` header. If we still can't figure out what format to choose, the default of `html` is selected for you.

```text
# Default: The view is presented using no extension or html,cfm
http://localhost:{port}/contacts/index
http://localhost:{port}/contacts/index.html
http://localhost:{port}/contacts/index.cfm

# JSON output
http://localhost:{port}/contacts/index.json
# OR Accepts: application/json

# XML output 
http://localhost:{port}/contacts/index.xml
# OR Accepts: application/xml

# PDF output
http://localhost:{port}/contacts/index.pdf
# OR Accepts: application/pdf
```

{% hint style="success" %}
**Tip:** You can also avoid the extension and pass a URL argument called `format` with the correct format type: `?format=json`.
{% endhint %}

### Routing

Let's add a new route to our system that is more RESTFul than `/contacts/index.json`. You will do so by leveraging the application's router found at `config/Router.cfc`. Find the `configure()` method and let's add a new route:

```java
// Restuful Route
route( 
    pattern="/api/contacts",
    target="contacts.index",
    name="api.contacts"
);

// Default Route
route( ":handler/:action?" ).end();
```

The `route()` method allows you to register new URL patterns in your application and immediately route them to a target event. You can even give it a human readable name that can be later referenced in the `buildLink()` method.

{% hint style="danger" %}
Make sure you add routes above the default ColdBox route. If not, your route will never fire.
{% endhint %}

We have now created a new URL route called `/api/contacts` that if detected will execute the `contacts.index` event. Now reinit the application, why, well we changed the application router and we need the changes to take effect.

{% hint style="success" %}
**Tip:** Every time you add new routes make sure you reinit the application: `http://localhost:{port}/?fwreinit`.
{% endhint %}

You can now visit the new URL pattern and you have successfully built a RESTFul API for your contacts.

```text
http://localhost:{port}/api/contacts.json
```

{% hint style="info" %}
You can find much more about routing in our [full docs](../../the-basics/routing/)
{% endhint %}

## Next Steps

Congratulations! Did you finish this guide in less than 60 minutes? If you did please leave us some great feedback below. If you didn't, then please do tell us why, we would love to improve our guides.

You can now move on to the next level in becoming a ColdBox Guru! Choose your path below:

* [Learn about HMVC via ColdBox Modules](../../getting-started/configuration/coldbox.cfc/configuration-directives/modules.md)
* [Learn about Dependency Injection](https://wirebox.ortusbooks.com)
* [Learn about Caching](https://cachebox.ortusbooks.com)
* [Learn about Logging](https://logbox.ortusbooks.com)
* [Learn about Testing](https://testbox.ortusbooks.com)

#### Getting Help

If you run into issues or just have questions, please jump on our [ColdBox Google Group](https://groups.google.com/forum/#!forum/coldbox) and our [Slack team](http://boxteam.herokuapp.com/) and ask away.

ColdBox is Professional Open Source under the Apache 2.0 license. We'd love to have your help with the product.

## 60 Minute Quick Start

This guide has been designed to get you started with ColdBox in fewer than 60 minutes. We will take you by the hand and help you build a RESTFul application in 60 minutes or fewer. After you complete this guide, we encourage you to move on to the [Getting Started Guide](../../getting-started/getting-started-guide.md) and then to the other guides in this book.

### Requirements

* Please make sure you download and install the latest [CommandBox CLI](https://www.ortussolutions.com/products/commandbox). We will show you how in the [Installing ColdBox section](installing-coldbox.md).
* Grab a cup of coffee
* Get comfortable

## Installing ColdBox

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

That's it. CommandBox can now track this version of ColdBox for you in this directory. In the [next section](my-first-coldbox-application.md) we will scaffold a ColdBox application using an application template.

{% hint style="success" %}
You can find many scaffolding templates for ColdBox in our Github organization: [github.com/coldbox-templates](https://github.com/coldbox-templates)
{% endhint %}

### Uninstalling ColdBox

To uninstall ColdBox from this application folder just type `uninstall coldbox`.

### Updating ColdBox

To update ColdBox from a previous version, just type `update coldbox`.

## My First ColdBox Application

CommandBox comes with a `coldbox create app` command that can enable you to create application skeletons using one of our official skeletons or [your own](../../digging-deeper/recipes/application-templates.md):

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
You can find all our template skeletons here: [github.com/coldbox-templates](https://github.com/coldbox-templates)
{% endhint %}

### Scaffolding Our Application

So let's create our first app using the _default_ template skeleton **AdvancedScript**:

```bash
coldbox create app MyApp
```

This will scaffold the application and also install ColdBox for you. The following folders/files are generated for you:

```text
+coldbox // The ColdBox framework library (CommandBox Tracked)
+config // Configuration files
+handlers // Your handlers/controllers
+includes // static assets
+interceptors // global interceptors
+layouts // Your layouts
+models // Your Models
+modules // CommandBox Tracked Modules
+modules_app // Custom modules
+tests // Test harness
+views // Your Views
+Application.cfc // Bootstrap
+box.json // CommandBox package descriptor
+index.cfm // Front controller
```

Now let's start a server so we can see our application running:

```text
server start --rewritesEnable
```

{% hint style="info" %}
This will start up a [Lucee](https://www.lucee.org) 5 open source CFML engine \(If you are in CommandBox 4\). If you would like an **Adobe ColdFusion** server then just add to the command: `cfengine=adobe@{version}` where `{version}` can be: `2016,11,10,9.`

If you are using CommandBox 3 and below, you will be using a Lucee 4.5 Server.
{% endhint %}

This command will start a server with URL rewrites enabled, open a web browser for you and execute the `index.cfm`which in turn executes the **default event** by convention in a ColdBox application: `main.index`.

{% hint style="success" %}
**Tip:** ColdBox Events map to handlers \(**cfc**\) and appropriate actions \(**functions**\)
{% endhint %}

That's it, you have just created your first application. Hooray, onward!

{% hint style="success" %}
**Tip:** Type `coldbox create app help` to get help on all the options for creating ColdBox applications.
{% endhint %}

### File/Folder Conventions

ColdBox is a conventions based framework. The location of files and functions matter. Since we scaffolded our first application, let's write down in a table below with the different conventions that exist in ColdBox.

| **File/Folder Convention** | **Mandatory** | **Description** |
| :--- | :--- | :--- |
| `config/Coldbox.cfc` | false | The application configuration file |
| `config/Router.cfc` | false | The application URL router |
| `handlers` | false | Event Handlers \(controllers\) |
| `layouts` | false | Layouts |
| `models` | false | Model objects |
| `modules` | false | CommandBox Tracked Modules |
| `modules_app` | false | Custom Modules You Write |
| `views` | false | Views |

{% hint style="info" %}
What is the common denominator in all the conventions? That they are all optional.
{% endhint %}

### Re-initializing The Application

There will be times when you make configuration or code changes that are not reflected immediately in the application due to caching. You can tell the framework to \_reinit \_or restart the application for you via the URL by leveraging the special URL variable `fwreinit`.

```text
http://localhost:{port}/?fwreinit=1
```

You can also use CommandBox to reinit the application:

```bash
coldbox reinit
```

{% hint style="success" %}
**Tip:** You can add a password to the **reinit** procedures for further security, please see the [configuration section](https://github.com/ortus-docs/coldbox-docs/tree/7a8d2250f812e1b65cfc9c2888a8489110724897/the-basics/configuration/coldbox.cfc).
{% endhint %}

## My First Handler & View

Now let's create our first controller, which in ColdBox is called **Event Handler**. Let's go to CommandBox again:

```bash
coldbox create handler name="hello" actions="index"
```

This will generate the following files:

* A new handler called `hello.cfc` inside of the `handlers` folder
* A view called `index.cfm` in the `views/hello` folder 
* An integration test at `tests/specs/integration/helloTest.cfc`. 

Now go to your browser and the following URL to execute the generated event:

```text
# With rewrites enabled
http://localhost:{port}/hello/index

# With no rewrites enabled
http://localhost:{port}/index.cfm/hello/index
```

You will now see a big `hello.index` outputted to the screen. You have now created your first handler and view combination.

### Handler Code

Let's check out the handler code:

```java
component{

    /**
     * Default Action
     */
     function index( event, rc, prc ){
        event.setView( "hello/index" );
     }


}
```

As you can see, a handler is a simple CFC with functions on them. Each function maps to an **action** that is executed via the URL. The default action in ColdBox is `index()`which receives three arguments:

* `event` - An object that models and is used to work with the current request
* `rc` - A struct that contains both `URL/FORM` variables \(unsafe data\)
* `prc` - A secondary struct that is **private** only settable from within your application \(safe data\)

{% hint style="info" %}
The **event** object is used for many things, in the case of this function we are calling a `setView()` method which tells the framework what view to render to the user once execution of the action terminates.
{% endhint %}

{% hint style="success" %}
**Tip:** The view is not rendered in line 7, but rendered after the execution of the action by the framework.
{% endhint %}

### Executing Events

Did you detect a convention here?

The sections in the URL are the same as the name of the event handler CFC \(`hello.cfc`\) and method that was generated `index()`. By convention, this is how you execute events in ColdBox by leveraging the following URL pattern that matches the name of a handler and action function.

{% hint style="info" %}
You can also nest handlers into folders and you can also pass the name of the folder\(s\) as well.
{% endhint %}

```text
http://localhost:{port}/folder/handler/action
http://localhost:{port}/handler/action
http://localhost:{port}/handler
```

If no `action` is defined in the URL then the default action of `index` will be used.

All of this URL magic happens thanks to the URL mappings capabilities in ColdBox. By convention, you can write beautiful URLs that are RESTFul and by convention. You can also extend them and create more expressive URL Mappings by leveraging the `config/Router.cfc` which is your application router.

{% hint style="success" %}
**Tip:** Please see the [event handlers](../../the-basics/event-handlers/) guide for more in-depth information.
{% endhint %}

### My First Virtual Event

Now let's create a virtual event, which is basically just a view we want to execute with no event handler controller needed. This is a great way to incorporate non-mvc files into ColdBox, baby steps!

```bash
coldbox create view name="virtual/hello"
```

Open the view now \(`/views/virtual/hello.cfm`\) and add the following:

```markup
<h1>Hello from ColdBox Land!</h1>
```

Then go execute the virtual event:

```text
http://localhost:{port}/virtual/hello
```

You will get the `Hello From ColdBox Land!` displayed! This is a great way to create tests or even bring in legacy/procedural templates into an MVC framework.

{% hint style="success" %}
**Tip:** You can see our [layouts and views](../../the-basics/layouts-and-views/) section for more in-depth information.
{% endhint %}

## Linking Events Together

ColdBox provides you with a nice method for generating links between events by leveraging an object called `event` that is accessible in all of your layouts/views and event handlers. This `event` object is called behind the scenes the **request context object,** which models the incoming request and even contains all of your incoming `FORM` and `URL` variables in a structure called `rc`.

{% hint style="success" %}
**Tip**: You will use the event object to set views, set layouts, set HTTP headers, read HTTP headers, convert data to other types \(json,xml,pdf\), and much more.
{% endhint %}

### Building Links

The method in the request context that builds links is called: `buildLink()`. Here are some of the arguments you can use:

```java
/**
* Builds links to events or URL Routes
*
* @to The event or route path you want to create the link to
* @translate Translate between . to / depending on the SES mode on to and queryString arguments. Defaults to true.
* @ssl Turn SSl on/off on URL creation, by default is SSL is enabled, we will use it.
* @baseURL If not using SES, you can use this argument to create your own base url apart from the default of index.cfm. Example: https://mysample.com/index.cfm
* @queryString The query string to append
*/
string function buildLink(
    to,
    boolean translate=true,
    boolean ssl,
    baseURL="",
    queryString="");
```

### Edit Your View

Edit the `views/virtual/hello.cfm` page and wrap the content in a `cfoutput` and create a link to the main ColdBox event, which by convention is `main.index`.

```text
<cfoutput>
<h1>Hello from ColdBox Land!</h1>
<p><a href="#event.buildLink( "main.index" )#">Go home</a></p>
</cfoutput>
```

This code will generate a link to the `main.index` event in a search engine safe manner and in SSL detection mode. Go execute the event: `http://localhost:{port}/virtual/hello` and click on the generated URL, you will now be navigating to the default event `/main/index`. This technique will also apply to FORM submissions:

```markup
<form action="#event.buildLink( 'user.save' )#" method="post">
...
</form>
```

{% hint style="success" %}
**Tip** You can visit our API Docs for further information about the `event` object and the `buildLink` method: [http://apidocs.ortussolutions.com/coldbox/current/index.html?coldbox/system/web/context/RequestContext.html](http://apidocs.ortussolutions.com/coldbox/current/index.html?coldbox/system/web/context/RequestContext.html).

For extra credit try to use more of the `buildLink` arguments.
{% endhint %}

### URL Structure & Mappings

ColdBox allows you to manipulate the incoming URL so you can create robust URL strategies especially for RESTFul services. This is all done by convention and you can configure it via the application router: `config/Router.cfc` for more granular control.

{% hint style="info" %}
Out of the box we provide you with convention based routing that maps the URL to modules/folders/handlers and actions.

`route( "/:handler/:action" ).end()`
{% endhint %}

We have now seen how to execute events via nice URLs. Behind the scenes, ColdBox translates the URL into an executable event string just like if you were using a normal URL string:

* `/main/index` -&gt; `?event=main.index`
* `/virtual/hello` -&gt; `?event=virtual.hello`
* `/admin/users/list` -&gt; `?event=admin.users.list`
* `/handler/action/name/value` -&gt; `?event=handler.action&name=value`
* `/handler/action/name` -&gt; `?event=handler.action&name=`

By convention, any name-value pairs detected after an event variable will be treated as an incoming `URL` variables. If there is no pair, then the value will be an empty string.

{% hint style="success" %}
**Tip:** By default the ColdBox application templates are using full URL rewrites. If your web server does not support them, then open the `config/Router.cfc` and change the full rewrites method to **false**: `setFullRewrites( false ).`
{% endhint %}

## Working With Event Handlers

Event handlers are the controller layer in ColdBox and is what you will be executing via the `URL`or a `FORM`post. All event handlers are **singletons**, which means they are cached for the duration of the application, so always remember to var scope your variables in your functions.

{% hint style="success" %}
**Tip:** For development we highly encourage you to turn handler caching off or you will have to reinit the application in every request, which is annoying. Open the `config/ColdBox.cfc` and look for the `coldbox.handlerCaching` setting.
{% endhint %}

Once you started the server in the previous section and opened the browser, the default event got executed which maps to an event handler CFC \(controller\) `handlers/main.cfc` and the method/action in that CFC called `index()`. Go open the `handlers/main.cfc` and let's explore the code.

### Handler Code

```javascript
// Default Action
function index( event, rc, prc ){
    prc.welcomeMessage = "Welcome to ColdBox!";
    event.setView( "main/index" );
}
```

Every action in ColdBox receives three arguments:

* `event` - An object that models and is used to work with the current request
* `rc` - A struct that contains both URL/FORM variables \(unsafe data\)
* `prc` - A secondary struct that is private only settable from within your application \(safe data\)

This line `event.setView( "main/index" )` told ColdBox to render a view back to the user found in `views/main/index.cfm` using a default layout, which by convention is called `Main.cfm` which can be found in the `layouts` folder.

### Executing Events

We have now seen how to add handlers via CommandBox using the `coldbox create handler` command and also execute them by convention by leveraging the following URL pattern:

```text
http://localhost:{port}/folder/handler/action
http://localhost:{port}/handler/action
http://localhost:{port}/handler
```

Also remember, that if no `action` is defined in the incoming URL then the default action of `index` will be used.

{% hint style="info" %}
Remember that the URL mappings support in ColdBox is what allows you to execute events in such a way from the URL. These are controlled by your application router: `config/Router.cfc`
{% endhint %}

### Working With Incoming Data

Now, let's open the handler we created before called `handlers/hello.cfc` and add some public and private variables to it so our views can render the variables.

```javascript
function index( event, rc, prc ){
    // param an incoming variable.
    event.paramValue( "name", "nobody" );
    // set a private variable
    prc.when = dateFormat( now(), "full" );
    // set the view to render
    event.setView( "hello/index" );
}
```

Let's open the view now: `views/hello/index.cfm` and change it to this:

```markup
<cfoutput>
<p>Hello #encodeForHTML( rc.name )#, today is #prc.when#</p>
</cfoutput>
```

{% hint style="danger" %}
Please note that we used the ColdFusion function `encodeForHTML()` \([https://www.cfdocs.org/encodeforhtml](https://www.cfdocs.org/encodeforhtml)\) on the public variable. Why? Because you can **never** trust the client and what they send, make sure you use the built-in ColdFusion encoding functions in order to avoid XSS hacks or worse on incoming public \(`rc`\) variables.
{% endhint %}

If you execute the event now: `http://localhost:{port}/hello/index` you will see a message of `Hello nobody`.

Now change the incoming URL to this: `http://localhost:{port}/hello/index?name=ColdBox` and you will see a message of `Hello ColdBox`.

{% hint style="success" %}
**Tip:** Please see the [layouts and views](../../the-basics/layouts-and-views/) section for in-depth information about them.
{% endhint %}

## Adding A Layout

Every time the framework renders a view, it will try to leverage the default layout which is located in `layouts/Main.cfm` by convention. This is an HTML file that gives format to your output and contains the location of where the view you want should be rendered.

### Layout Code

This location is identified by the following code:

```markup
<div id="maincontent">
#renderView()#
</div>
```

The call to the `renderView()` method with no arguments tells the framework to render the view that was set using `event.setView()`. This is called a **rendering region**. You can use as many rendering regions within layouts or even within views themselves.

{% hint style="info" %}
**Named Regions:** The `setView()` method even allows you to name these regions and then render them in any layout or other views using their name.
{% endhint %}

### Creating A Layout

Let's create a new simple layout with two rendering regions. Open up CommandBox and issue the following commands:

```bash
# Create a Funky layout
coldbox create layout name="Funky"

# Create a footer
coldbox create view name="main/footer"
```

Open the `layouts/Funky.cfm` layout and let's modify it a bit by adding the footer view as a rendering region.

```markup
<h1>funky Layout</h1>
<cfoutput>#renderView()#</cfoutput>

<hr>
<cfoutput>#renderView( "main/footer" )#</cfoutput>
```

### Using The Layout

Now, let's open the handler we created before called `handlers/hello.cfc` and add some code to use our new layout explicitly via adding a `layout` argument to our `setView()` call.

```javascript
function index( event, rc, prc ){
    // param an incoming variable.
    event.paramValue( "name", "nobody" );
    // set a private variable
    prc.when = dateFormat( now(), "full" );

    // set the view to render with our new layout
    event.setView( view="hello/index", layout="Funky" );
}
```

Go execute the event now: `http://localhost:{port}/hello/index` and you will see the view rendered with the words `funky layout` and `footer view` at the bottom. Eureka, you have now created a layout.

{% hint style="info" %}
You can also leverage the function `event.setLayout( "Funky" )` to change layouts and even concatenate the calls:

`event.setView( "hello/index" )    
.setLayout( "Funky" );`
{% endhint %}

## Adding A Model

Let's complete our saga into MVC by developing the **M**, which stands for [model](https://en.wikipedia.org/wiki/Domain_model). This layer is all your business logic, queries, external dependencies, etc. of your application, which represents the problem to solve or the domain to solve.

### WireBox

This layer is controlled by [WireBox](https://wirebox.ortusbooks.com), the dependency injection framework within ColdBox, which will give you the flexibility of wiring your objects and persisting them for you.

### Creating A Service Model

Let's create a simple contact listing, so open up CommandBox and issue the following command:

```bash
coldbox create model name="ContactService" methods="getAll" persistence="singleton"
```

This will create a `models/ContactService.cfc` with a `getAll()` method and a companion unit test at `tests/specs/unit/ContactServiceTest.cfc`. Let's open the model object:

```javascript
component singleton accessors="true"{      

    ContactService function init(){         
        return this;     
    }

    function getAll(){      

    }

}
```

{% hint style="warning" %}
Notice the `singleton` annotation on the component tag. This tells WireBox that this service should be cached for the entire application life-span. If you remove the annotation, then the service will become a _transient_ object, which means that it will be re-created every time it is requested.
{% endhint %}

### Add Some Data

Let's mock an array of contacts so we can display them later. We can move this to a SQL call later.

```javascript
component singleton accessors="true"{

    ContactService function init(){
        variables.data = [
            { id=1, name="coldbox" },
            { id=2, name="superman" },
            { id=3, name="batman" }
        ];    
        return this;

    }

    function getAll(){
        return variables.data;
    }

}
```

{% hint style="success" %}
We also have created a project to mock any type of data: [MockDataCFC](https://www.forgebox.io/view/mockdatacfc). Just use CommandBox to install it: `install mockdatacfc`

You can then leverage it to mock your contacts or any simple/complex data requirement.
{% endhint %}

### Wiring Up The Model To a Handler

We have now created our model so let's tell our event handler about it. Let's create a new handler using CommandBox:

```bash
coldbox create handler name="contacts" actions="index"
```

This will create the `handler/contacts.cfc` handler with an `index()` action, the `views/contacts/index.cfm` view and the accompanying integration test `tests/specs/integration/contactsTest.cfc`.

Let's open the handler and add a new ColdFusion `property` that will have a reference to our model object.

```javascript
component{ 

    property name="contactService" inject="ContactService";

    any function index( event, rc, prc ){ 
        event.setView( "contacts/index" ); 
    }
}
```

Please note that `inject` annotation on the `property` definition. This tells WireBox what model to inject into the handler's `variables`scope.

By convention it looks in the `models` folder for the value, which in our case is `ContactService`. Now let's call it and place some data in the private request collection `prc` so our views can use it.

```javascript
any function index( event, rc, prc ){
    prc.aContacts = contactService.getAll();
    event.setView( "contacts/index" );
}
```

### Presenting The Data

Now that we have put the array of contacts into the `prc` struct as `aContacts`, let's display it to the screen using ColdBox's HTML Helper.

The ColdBox HTML Helper is a companion class that exists in all layouts and views that allows you to generate semantic HTML5 without the needed verbosity of nesting, or binding to ORM/Business objects.

{% hint style="info" %}
Please check out the API Docs to discover the HTML Helper: [http://apidocs.ortussolutions.com/coldbox/current/index.html?coldbox/system/modules/HTMLHelper/models/HTMLHelper.html](http://apidocs.ortussolutions.com/coldbox/current/index.html?coldbox/system/modules/HTMLHelper/models/HTMLHelper.html)
{% endhint %}

Open the `contacts/index.cfm` and add the following to the view:

```markup
<cfoutput>
<h1>My Contacts</h1>

#html.table( data=prc.aContacts, class="table table-striped" )#
</cfoutput>
```

{% hint style="warning" %}
Note: If your models are `singletons`, they will persist for the life-span of your ColdFusion application. To see code changes for singletons, you have to reinit the framework by using the `?fwreinit={password}` Url action or via CommandBox using `coldbox reinit`. Please check out the API Docs to discover CommandBox: \[[https://apidocs.ortussolutions.com/commandbox/4.5.0/index.html](https://apidocs.ortussolutions.com/commandbox/4.5.0/index.html)\]
{% endhint %}

That's it! Execute the event: `http://localhost:{port}/contacts/index` and view the nice table of contacts being presented to you.

Congratulations, you have made a complete **MVC** circle!

> **Tip** You can find much more information about models and dependency injection in our [full docs](https://coldbox.ortusbooks.com/the-basics/models)

## RESTFul Data

Out of the box, ColdBox gives you all the RESTFul capabilities you will need to create robust and scalable RESTFul services. Let's add some RESTFul capabilities to our contact listing we created in the previous section.

{% hint style="success" %}
**Tip:** You can find much more information about building ColdBox RESTFul services in our [full docs.](../../digging-deeper/recipes/building-rest-apis.md)
{% endhint %}

### Producing JSON

If you know beforehand what type of format you will be responding with, you can leverage ColdBox 5's auto-marshalling in your handlers. By default, ColdBox detects any return value from handlers and if they are complex it will convert them to JSON automatically for you:

{% code title="contacts.cfc" %}
```javascript
any function index( event, rc, prc ){
    return contactService.getAll();    
}
```
{% endcode %}

That's it! ColdBox detects the array and automatically serializes it to JSON. Easy Peasy!

If you want your incoming JSON requestbody to be handled the same as FORM and URL variables you can enable `coldbox.jsonPayloadToRC = true` in your coldbox config. See releasenotes [`5.1.0`](../../intro/introduction/whats-new-with-5.1.0.md#new-auto-deserialization-of-json-payloads) and [5.1.2 ](../../intro/introduction/whats-new-with-5.1.2.md#automatic-json-payload-setting)for details.

### renderData\(\)

The request context object has a special function called `renderData()` that can take any type of data and marshall it for you to other formats like `xml, json, wddx, pdf, text, html` or your own type.

{% hint style="success" %}
**Tip**: You can find more information at the API Docs for `renderData()` here [http://apidocs.ortussolutions.com/coldbox/current/index.html?coldbox/system/web/context/RequestContext.html\#renderData\(](http://apidocs.ortussolutions.com/coldbox/current/index.html?coldbox/system/web/context/RequestContext.html#renderData%28)\)
{% endhint %}

So let's open the `handlers/contacts.cfc` and add to our current code:

{% code title="contacts.cfc" %}
```javascript
any function index( event, rc, prc ){
    prc.aContacts = contactService.getAll();    
    event.renderData( data=prc.aContacts, formats="xml,json,pdf,html" );
}
```
{% endcode %}

We have added the following line:

```javascript
event.renderData( data=prc.aContacts, formats="xml,json,pdf,html" );
```

This tells ColdBox to render the contacts data in 4 formats: xml, json, pdf and html. WOW! So how would you trigger each format? Via the URL of course.

### Format Detection

ColdBox has the ability to detect formats via URL extensions or an incoming `Accepts` header. If no extension is sent, then ColdBox attempts to determine the format by inspecting the `Accepts` header. If we still can't figure out what format to choose, the default of `html` is selected for you.

```text
# Default: The view is presented using no extension or html,cfm
http://localhost:{port}/contacts/index
http://localhost:{port}/contacts/index.html
http://localhost:{port}/contacts/index.cfm

# JSON output
http://localhost:{port}/contacts/index.json
# OR Accepts: application/json

# XML output 
http://localhost:{port}/contacts/index.xml
# OR Accepts: application/xml

# PDF output
http://localhost:{port}/contacts/index.pdf
# OR Accepts: application/pdf
```

{% hint style="success" %}
**Tip:** You can also avoid the extension and pass a URL argument called `format` with the correct format type: `?format=json`.
{% endhint %}

### Routing

Let's add a new route to our system that is more RESTFul than `/contacts/index.json`. You will do so by leveraging the application's router found at `config/Router.cfc`. Find the `configure()` method and let's add a new route:

```java
// Restuful Route
route( 
    pattern="/api/contacts",
    target="contacts.index",
    name="api.contacts"
);

// Default Route
route( ":handler/:action?" ).end();
```

The `route()` method allows you to register new URL patterns in your application and immediately route them to a target event. You can even give it a human readable name that can be later referenced in the `buildLink()` method.

{% hint style="danger" %}
Make sure you add routes above the default ColdBox route. If not, your route will never fire.
{% endhint %}

We have now created a new URL route called `/api/contacts` that if detected will execute the `contacts.index` event. Now reinit the application, why, well we changed the application router and we need the changes to take effect.

{% hint style="success" %}
**Tip:** Every time you add new routes make sure you reinit the application: `http://localhost:{port}/?fwreinit`.
{% endhint %}

You can now visit the new URL pattern and you have successfully built a RESTFul API for your contacts.

```text
http://localhost:{port}/api/contacts.json
```

{% hint style="info" %}
You can find much more about routing in our [full docs](../../the-basics/routing/)
{% endhint %}

## Next Steps

Congratulations! Did you finish this guide in less than 60 minutes? If you did please leave us some great feedback below. If you didn't, then please do tell us why, we would love to improve our guides.

You can now move on to the next level in becoming a ColdBox Guru! Choose your path below:

* [Learn about HMVC via ColdBox Modules](../../getting-started/configuration/coldbox.cfc/configuration-directives/modules.md)
* [Learn about Dependency Injection](https://wirebox.ortusbooks.com)
* [Learn about Caching](https://cachebox.ortusbooks.com)
* [Learn about Logging](https://logbox.ortusbooks.com)
* [Learn about Testing](https://testbox.ortusbooks.com)

#### Getting Help

If you run into issues or just have questions, please jump on our [ColdBox Google Group](https://groups.google.com/forum/#!forum/coldbox) and our [Slack team](http://boxteam.herokuapp.com/) and ask away.

ColdBox is Professional Open Source under the Apache 2.0 license. We'd love to have your help with the product.

