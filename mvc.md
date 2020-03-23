# MVC

## Modules

![](https://github.com/ortus-docs/coldbox-docs/raw/master/.gitbook/assets/Modules.png)

ColdBox Modules are self-contained subsets of a ColdBox application that can be dropped in to any ColdBox application and become alive as part of the host application. They will bring re-usability and extensibility to any ColdBox application, as now you can break them down further into a collection of modules instead of monolithic approaches. CommandBox will also help you manage, install, track and uninstall modules as part of your development workflow.

This concept of modularization has been around in software design for a long time as it is always essential to partition a system into manageable modules or parts.

> "In structured design and data-driven design, a module is a generic term used to describe a named and addressable group of program statements" by Craig Borysowich \(Chief Technology Tactician\)

The key to the statement is that these modules are named and addressable, which is exactly what ColdBox Modules will offer to your application. So by building an application with a methodology of modules, you will inherently achieve:

* **Manageability** \(i.e., small and simple parts that can be easily understood and worked on\)
* **Independence** \(i.e., a module can live on its own if necessary and tested outside of its environments, produces very nice low coupling between core and modules\)
* **Isolation** \(i.e., some modules can be completely isolated and decoupled\)
* **Extensibility** \(i.e., you can easily extend ANY application by just building on top of the modular architecture\)
* **Reusability** \(i.e., modules have independence and can therefore be shared and reused\)
* **Sanitability** \(Yes! That's a word\) from the root Sanity

ColdBox Modules will change the way you approach application development as you can now have a foundation architecture that can scale easily and provide you enough manageability to reduce maintenance and increase development. Such a design means that development, testing and maintenance becomes easier, faster and with a much lower cost. **Welcome to a brave new world!**

## Core Modules

There are several core modules that are maintained by Ortus Solutions that will bring several enhancements to a core ColdBox MVC installation.

> **Info**: They used to be part of the core ColdBox Framework before 4.0.0 modularization.

### `cbantisamy` - OWASP AntiSamy

* Source: [https://github.com/ColdBox/cbox-antisamy](https://github.com/ColdBox/cbox-antisamy)
* ForgeBox: [http://forgebox.io/view/cbantisamy](http://forgebox.io/view/cbantisamy)

OWASP AntiSamy Module that provides XSS cleanup operations to ColdBox 4 applications

```text
install cbantisamy
```

### `cbcommons` - Common Utilities

* Source: [https://github.com/ColdBox/cbox-commons](https://github.com/ColdBox/cbox-commons)
* ForgeBox: [http://forgebox.io/view/cbcommons](http://forgebox.io/view/cbcommons)

A collection of model objects for your using pleasure:

* DateUtils
* FileUtils
* JVMUtils
* QueryHelper
* Zip

```text
install cbcommons
```

### `csrf` - Cross Site Request Forgery

* Source: [https://github.com/ColdBox/cbox-csrf](https://github.com/ColdBox/cbox-csrf)
* ForgeBox: [http://forgebox.io/view/csrf](http://forgebox.io/view/csrf)

A module that protects you against CSRF attacks by generating unique FORM/client tokens and providing your ColdBox application with new functions for protection.

```text
install csrf
```

### `cbdebugger` - ColdBox Debugger

* Source: [https://github.com/ColdBox/cbox-debugger](https://github.com/ColdBox/cbox-debugger)
* ForgeBox: [http://forgebox.io/view/cbdebugger](http://forgebox.io/view/cbdebugger)

This module will enhance your application with debugger capabilities, a nice debugging panel and much more to make your ColdBox application development nicer, funer and greater! Yes, funer is a word!

```text
install cbdebugger
```

### `cbfeeds` - Feeds Support

* Source: [https://github.com/ColdBox/cbox-feeds](https://github.com/ColdBox/cbox-feeds)
* ForgeBox: [http://forgebox.io/view/cbfeeds](http://forgebox.io/view/cbfeeds)

A nice and fancy way to consume and produce RSS, ATOM feeds the ColdBox way!

```text
install cbfeeds
```

### `cbi18n` - Localization & Internationalization

* Source: [https://github.com/ColdBox/cbox-i18n](https://github.com/ColdBox/cbox-i18n)
* ForgeBox: [http://forgebox.io/view/cbi18n](http://forgebox.io/view/cbi18n)

This module will enhance your ColdBox applications with i18n capabilities, resource bundles and localization.

```text
install cbi18n
```

### `cbioc` - Third-Party Dependency Injection

* Source: [https://github.com/ColdBox/cbox-ioc](https://github.com/ColdBox/cbox-ioc)
* ForgeBox: [http://forgebox.io/view/cbioc](http://forgebox.io/view/cbioc)

The ColdBox IOC module allows you to integrate third-party dependency injection and inversion of control frameworks into your ColdBox Applications like Di/1, ColdSpring, etc.

```text
install cbioc
```

### `cbjavaloader` - JavaLoader

* Source: [https://github.com/ColdBox/cbox-javaloader](https://github.com/ColdBox/cbox-javaloader)
* ForgeBox: [http://forgebox.io/view/cbjavaloader](http://forgebox.io/view/cbjavaloader)

The CB JavaLoader module will interface with Mark Mandel's JavaLoader to allow you to do a network class loader, compiler and proxy.

```text
install cbjavaloader
```

### `cbmailservices` - Mail Services

* Source: [https://github.com/ColdBox/cbox-mailservices](https://github.com/ColdBox/cbox-mailservices)
* ForgeBox: [http://forgebox.io/view/cbmailservices](http://forgebox.io/view/cbmailservices)

The ColdBox Mail services module will allow you to send email the OO way in multiple protocols for many environments. The supported protocols are:

* CFMail - Traditional cfmail sending
* Files - Write emails to disk
* [Postmark API](https://www.forgebox.io/view/PostBox) - Send via the PostMark Service \([https://postmarkapp.com/](https://postmarkapp.com/)\)
* [SendGrid](https://www.forgebox.io/view/send-grid-protocol) - Send via the SendGrid Service \([https://sendgrid.com/](https://sendgrid.com/)\)

You can easily add your own mail protocols by building upon our standards.

```text
install cbmailservices
```

### `cbmessagebox` - MessageBox

* Source: [https://github.com/ColdBox/cbox-messagebox](https://github.com/ColdBox/cbox-messagebox)
* ForgeBox: [http://forgebox.io/view/cbmessagebox](http://forgebox.io/view/cbmessagebox)

A nice producer of flash scoped based messages that's skinnable

```text
install cbmessagebox
```

### `cborm` - ORM Extensions

* Source: [https://github.com/ColdBox/cbox-cborm](https://github.com/ColdBox/cbox-cborm)
* ForgeBox: [http://forgebox.io/view/cborm](http://forgebox.io/view/cborm)

This module provides you with several enhancements when interacting with the ColdFusion ORM via Hibernate. It provides you with virtual service layers, active record patterns, criteria and detached criteria queries, entity compositions, populations and so much more to make your ORM life easier!

```text
install cborm
```

### `cbsecurity` - Security Engine

* Source: [https://github.com/ColdBox/cbox-security](https://github.com/ColdBox/cbox-security)
* ForgeBox: [http://forgebox.io/view/cbsecurity](http://forgebox.io/view/cbsecurity)

This module will provide your application with a security rule engine. For more information visit the documentation here: [https://github.com/ColdBox/cbox-security/wiki](https://github.com/ColdBox/cbox-security/wiki)

```text
install cbsecurity
```

### `cbsoap` - SOAP Helper

* Source: [https://github.com/ColdBox/cbox-soap](https://github.com/ColdBox/cbox-soap)
* ForgeBox: [http://forgebox.io/view/cbsoap](http://forgebox.io/view/cbsoap)

A module to help you interact with SOAP web services

```text
install cbsoap
```

### `cbstorages` - Persistent Storages

* Source: [https://github.com/ColdBox/cbox-storages](https://github.com/ColdBox/cbox-storages)
* ForgeBox: [http://forgebox.io/view/cbstorages](http://forgebox.io/view/cbstorages)

A collection of model objects to facade and help with native ColdFusion persistence structures.

```text
install cbstorages
```

### `cbswagger` - Swagger Support for ColdBox Applications

* Source: [https://github.com/coldbox-modules/cbSwagger](https://github.com/coldbox-modules/cbSwagger)
* ForgeBox: [http://forgebox.io/view/cbSwagger](http://forgebox.io/view/cbSwagger)

This module automatically generates OpenAPI \( fka Swagger \) documentation from your configured application and module routes. This module utilizes the v3.0 OpenAPI Specification

```text
install cbswagger
```

### `cbvalidation` - Validation

* Source: [https://github.com/ColdBox/cbox-validation](https://github.com/ColdBox/cbox-validation)
* ForgeBox: [http://forgebox.io/view/cbvalidation](http://forgebox.io/view/cbvalidation)

ColdBox sports its own server side validation engine so it can provide you with a unified approach to object and form validation.

```text
install cbvalidation
```

## Locations

By convention every ColdBox application will have two folders for modules:

* **/modules** : Used by CommandBox to place tracked modules. You would usually NOT commit these modules to source control.
* **/modules\_app** : Used for custom non-tracked modules

### External Locations

You can also have more external locations that ColdBox will scan for modules by leveraging the `coldbox.modulesExternalLocation` setting. This setting is an array of locations you want to tell ColdBox to look for modules in your `ColdBox.cfc`. Each array element is the instantiation location which can use ColdFusion mappings or an absolute reference from the root of your application.

> **Caution** Internally each of those entries will be expanded for you, so please be aware of this.

```javascript
coldbox = {

 ...

 modulesExternalLocation = [ "/shared/modules", "/codedepot/modules/customer1" ]

};
```

### Duplicate Names

You might be asking yourself, what happens if there is another module in the external locations with the same name as in another location? Or even in the conventions? Well, the first one found takes precedence. So if we have a module called `funky` in our conventions folder and also in our external locations, only the one in the conventions will load as it is discovered first.

## Parent Configuration

There are a few parent application settings when dealing with modules. In your `ColdBox.cfc` you can have a `modules` structure with some configuration settings.

```text
modules = {
    // reload and unload modules in every request
    // DEPRECATED in coldbox 5
    autoReload = false,
    // An array or list of the module names that will load ONLY
    include = [],
    // An array or list of the module names that will be EXCLUDED
    exclude = ["paidModule1","paidModule2"]
};
```

## Module Layout

In order to create a module you must first create a nicely named directory within the modules conventions directory. For example, let's build a simple _hello world_ module. CommandBox to the rescue!

```bash
coldbox create module helloworld
```

Here is the output:

```text
Created /Users/lmajano/tmp/myapp/modules_app/helloworld
Created /Users/lmajano/tmp/myapp/modules_app/helloworld/handlers
Created /Users/lmajano/tmp/myapp/modules_app/helloworld/handlers/Home.cfc
Created /Users/lmajano/tmp/myapp/modules_app/helloworld/models
Created /Users/lmajano/tmp/myapp/modules_app/helloworld/models/models_here.txt
Created /Users/lmajano/tmp/myapp/modules_app/helloworld/ModuleConfig.cfc
Created /Users/lmajano/tmp/myapp/modules_app/helloworld/views
Created /Users/lmajano/tmp/myapp/modules_app/helloworld/views/home
Created /Users/lmajano/tmp/myapp/modules_app/helloworld/views/home/index.cfm
```

The layout of a ColdBox Module is optional except for one file: `ModuleConfig.cfc`. This is a simple CFC that boots up your module and tells the host application how your module is loaded, unloaded and behaves. If you are leveraging CommandBox then you can also declare a `box.json` for the module itself in order to declare dependencies and development dependencies for it.

Below are all the possible combinations of a module layout, you will notice that it is EXACTLY the same as a ColdBox application.

```javascript
+Modules_app
  + {ModuleName - Unique}
    + ModuleConfig.cfc (The module configuration object Mandatory)
    + box.json (optional - if using CommandBox)
    + handlers (optional)
    + layouts  (optional)
    + views    (optional)
    + interceptors (optional)
    + models    (optional)
```

As you can see, the only mandatory resources for a module is the directory name in which it lives and a `ModuleConfig.cfc`. The module developer can choose to implement a simple module or a very complex module. All folders are optional and only what is used will be loaded. Not only are modules reusable and extensible, but you can easily create a module with dual functionality: A standalone application or a module. This is true reusability and flexibility. I don't know about you, but this is really exciting \(Geek Alert!\).

## Changing The Module Layout

If you are picky and you would like to change the folder layout for your module, you can. This is achieved from within the `ModuleConfig.cfc` by adding a `conventions` structure that will explain to the ColdBox Module engine how to locate and configure your module.

```javascript
// Module Conventions
conventions = {
  handlersLocation  = "handlers",
  viewsLocation     = "views",
  layoutsLocation   = "layouts",
  modelsLocation    = "models"
};
```

## Module Service

The beauty of ColdBox Modules is that you have an internal module service that you can tap in order to dynamically interact with the ColdBox Modules. This service is available by talking to the main ColdBox controller and calling its `getModuleService()` method or via dependency injection.

```javascript
// get module service from handlers, plugins, layouts, interceptors or views.
ms = controller.getModuleService();

// You can also inject it via our autowire DSL
property name="moduleService" inject="coldbox:moduleService";
```

## Module Lifecycle

However, before we start reviewing the module service methods let's review how modules get loaded in a ColdBox application. Below is a simple bullet point of what happens in your application when it starts up:

1. ColdBox main application and configuration loads
2. ColdBox Cache, Logging and WireBox are created
3. Module Service calls on `registerAllModules()` to read all the modules in the modules locations \(with include/excludes\) and start registering their configurations one by one. If the module had parent settings, interception points, datasoures or webservices, these are registered here.
4. All main application interceptors are loaded and configured
5. ColdBox is marked as initialized
6. Module service calls on `activateAllModules()` so it begins activating only the registered modules one by one. This registers the module's SES URL Mappings, model objects, etc
7. `afterConfigurationLoad` interceptors are fired

## Module Registration

Below you can see a diagram of what happens when modules get registered:

## Module Activation

Below you can see a diagram of what happens when modules get activated right after registration:

## Module Unloading

Below you can see a diagram of what happens when modules get deactivated and unloaded

## Common Methods

Here are the most common methods you can use to manage modules:

* `reloadAll()` : Reload all modules in the application. This clears out all module settings, re-registers from disk, re-configures them and activates them
* `reload(module)` : Target a module reload by name
* `unloadAll()` : Unload all modules
* `unload(module)` : Target a module unload by name
* `registerAllModules()` : Registers all module configurations
* `registerModule(moduleName,[instantiationPath])` : Target a module configuration registration
* `activateAllModules()` : Activate all registered modules
* `activateModule(moduleName)` : Target activate a module that has been registered already
* `getLoadedModules()` : Get an array of loaded module names
* `rebuildModuleRegistry()` : Rescan all the module locations for newly installed modules and rebuild the registry so these modules can be registered and activated.
* `registerAndActivateModule(moduleName,[instantiationPath])` : Register and Activate a new module

With these methods you can get creative and target the reloading, unloading or loading of specific modules. These methods really open the opportunity to build an abstraction API where you can install modules in your application on the fly and then register and activate them. You can also do the inverse and deactivate modules in a running application.

## Loading New Modules

If you want to load a new module in your application that you have just installed you need to do a series of steps.

1. Drop the module in any of the module locations defined or an a-la-carte location
2. Call `registerAndActivateModule(moduleName,[instantiationPath])` to register and activate the module

```javascript
controller.getModuleService().registerAndActivateModule('TaskManager');
```

## Loading A-la-carte Modules

You can also use the same module service methods to load ANY module in ANY ColdFusion reachable location. This is huge for applications that need granular control of loading and unloading of modules. You will do this with our magic method:

```javascript
registerAndActivateModule(moduleName,[instantiationpath])
```

This time, you will pass an instantiation path to the method. This is the dot notation path up to the folder that contains the module. So let's say your module to load is located here:

```javascript
/shared
  /modules
    /Calendar
      +ModuleConfig.cfc
```

You will then issue the following:

```javascript
controller.getModuleService().registerAndActivateModule('Calendar','shared.modules')
```

> **Caution** The instantiation path does **NOT** include the name of the module on disk as the name is the module name you already pass into the method.

## Module Events

The module service also announces several events or interception points as you saw from the life cycle diagrams. Below are the events announced:

### preModuleLoad

Fired before a module is about to be activated.

**Data:**

* `moduleLocation` - The location of the loaded module
* `moduleName` - The name of the module

### postModuleLoad

Fired after a module has been successfully activated

**Data:**

* `moduleLocation` - The location of the loaded module
* `moduleName` - The name of the module
* `moduleConfig` - The module configuration structure

### preModuleUnload

Fired before a module is about to be unloaded

**Data:**

* `moduleName` - The name of the module

### postModuleUnload

Fired after a module has been unloaded

**Data:**

* `moduleName` - The name of the module

## ModuleConfig

The module configuration object: `ModuleConfig.cfc` is the boot code of your module and where you can tell the host application how this module will behave. This component is a simple CFC, no inheritance needed, because it is decorated with methods and variables by ColdBox at runtime.

The only requirement is that this object MUST exist in the root of the module folder and contain a `configure()` method. This is the way modules are discovered, so if there is no `ModuleConfig.cfc` in the root of that folder inside of the modules folder, the framework skips it. So let's investigate what we need or can do in this component.

```javascript
component{

    function configure(){

    }

}
```

## Public Module Properties\/Directives

Here is a listing of all public properties that can be defined in a module.

| Property | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| **title** | string | false | --- | The title of the module |
| **author** | string | false | --- | The author of the module |
| **webURL** | string | false | --- | The web URL associated with this module. Maybe for documentation, blog, links, etc. |
| **description** | string | false | --- | A short description about the module |
| **version** | string | false | --- | The version of the module |
| **viewParentLookup** | boolean | false | true | If true, coldbox checks for views in the parent overrides first, then in the module. If false, coldbox checks for views in the module first, then the parent. |
| **layoutParentLookup** | boolean | false | true | If true, coldbox checks for layouts in the parent overrides first, then in the module. If false, coldbox checks for layouts in the module first, then the parent. |
| **entryPoint** | route | false | --- | The module's  default route \(ex:`/forgebox`\) that ColdBox will use to create an entry point pattern link into the module. Similar to the default event setting. The SES interceptor will also use this to auto-register the module's routes if used by calling the method `addModuleRoutes()` for you. |
| **inheritEntryPoint** | boolean | false | false | If true, then the `entrypoint` will be constructed by looking at its parent hierarchy chain.  This is a great way to build automated nesting schemas for APIs |
| **activate** | boolean | false | true | You can tell ColdBox to register the module but NOT to activate it. By default, all modules activate. |
| **parseParentSettings** | boolean | false | true | If true, ColdBox will merge any settings found in `moduleSettings[ this.modelNamespace ]` in the `config/ColdBox.cfc` file with the module settings, overriding them where the keys are the same.  Otherwise, settings in the module will override the parent configuration. |
| **aliases** | array | false | \[\] | An array of names that can be used to execute the module instead of only the module folder name |
| **autoMapModels** | boolean | false | true | Will automatically map all model objects under the models folder in WireBox using `@modulename` as part of the alias. |
| **autoProcessModels** | boolean | false | false | If false, then all models will not be processed by WireBox metadata to improve performance.  If true, then all object models will be inspected for metadata which can be time consuming. |
| **cfmapping** | string | false | empty | The ColdFusion mapping that should be registered for you that points to the root of the module. |
| **disabled** | boolean | false | false | You can manually disable a module from loading and registering |
| **dependencies** | array | false | \[\] | An array of dependent module names. All dependencies will be registered and activated FIRST before the module declaring them. |
| **modelNamespace** | string | false | _moduleName_ | The name of the namespace to use when registering models in WireBox. By default it uses the name of the module. |
| **applicationHelper** | array | false | \[\] | An array of files from the module to load as application helper UDF mixins |

Below you can see an example of declarations for the configuration object:

```javascript
component{
    // Module Properties
    this.title      = "My Test Module";
    this.author       = "Luis Majano";
    this.webURL       = "http://www.coldbox.org";
    this.description    = "A funky test module";
    this.version      = "1.0.0";
    // If true, looks for views in the parent first, if not found, then in the module. Else vice-versa
    this.viewParentLookup   = true;
    // If true, looks for layouts in the parent first, if not found, then in module. Else vice-versa
    this.layoutParentLookup = true;
    // The module entry point using SES
    this.entryPoint     = "/testing";
    this.inheritEntryPoint = true;
    this.autoMapModels = true;
    this.autoProcessModels = false;
    this.modelNamespace = "store";
    this.aliases = [ "store", "ecommerce", "shop" ];
    this.cfmapping = "cbstore";
    this.parseParentSettings = true;
    this.dependencies = [ "JavaLoader", "CFCouchbase" ];
    this.applicationHelper = [ "includes/mixins.cfm" ]

    function configure(){
    }
}
```

## The Decorated Variables

At runtime, the configuration object will be created by ColdBox and decorated with the following private properties \(available in the `variables` scope\):

| Property | Description |
| :--- | :--- |
| appMapping | The `appMapping` setting of the current host application |
| binder | The WireBox configuration binder object |
| cachebox | A reference to CacheBox |
| controller | A reference to the application's ColdBox Controller |
| log | A pre-configured LogBox Logger object for this specific class object \(`coldbox.system.logging.Logger`\) |
| logBox | A Reference to LogBox |
| moduleMapping | The `moduleMapping` setting of the current module. This is the path needed in order to instantiate CFCs in the module. |
| modulePath | The absolute path to the current loading module |
| wirebox | A Reference to WireBox |

You can use any of these private variables to create module settings, load CFCs, add binder mappings, etc.

## The configure\(\) Method

Once the public properties are set, we are now ready to configure our module. You will do this by creating a simple method called `configure()` and adding variables to the following configuration structures:

| Property | Type | Description |
| :--- | :--- | :--- |
| parentSettings | struct | Settings that will be appended and override the host application settings |
| settings | struct | Custom module settings that will only be available to the module. If `parseParentSettings` is set to true \(default\), then settings from `config/Coldbox.cfc` for this module will be merged with these settings. \(Think of these as default settings in that case.\) Please see the retrieving settings section |
| conventions | struct | A structure that explains the layout of the handlers, plugins, layouts and views of this module. |
| datasources | struct | A structure of datasource metadata that will append and override the host application datasources configuration |
| interceptorSettings | struct | A structure of settings for interceptor interactivity which includes the following sub-keys: |
| interceptors | array of structs | An array of declared interceptor structures that should be loaded in the entire application. Follows the same pattern as the [ConfigurationCFC](http://wiki.coldbox.org/wiki/ConfigurationCFC.cfm) interceptor declarations. |
| layoutSettings | struct | A structure of elements that setup layout configuration data for the module with the following keys: |
| routes | array | An array of declared URL routes or locations of routes for this module. The keys of the structure are the same as the _addRoute\(\)_ method of the [SES interceptor](http://wiki.coldbox.org/wiki/URLMappings.cfm) or a simple string location to the routes file to include. |
| wirebox | struct | A structure of [WireBox](http://wiki.coldbox.org/wiki/WireBox.cfm) configuration data, please refer to the \[WireBox Configuration\]\([http://wiki.coldbox.org/wiki/WireBox.cfm\#Configure\(\)\_method](http://wiki.coldbox.org/wiki/WireBox.cfm#Configure%28%29_method)\) area or just use the injected binder object for mappings. |

It is important to note that there is only ONE running application and the modules basically leach on to it. So the following structures will append their contents into the running application settings: parentSettings, datasources, webservices, customInterceptionPoints and interceptors.

All of the configuration settings that are declared in your configuration object will be added to a key in the host application settings called modules. Inside of this structure all module configurations will be stored according to their module name and remember that the module name comes from the name of the directory on disk. So if you have a module called helloworld then the settings will be stored in the following location: ConfigSettings.modules.helloworld

Below is an example of some settings:

```javascript
function configure(){

  // parent settings
  parentSettings = {
    woot = "Module set it!"
  };

  // module settings - stored in the main configuration settings struct as modules.{moduleName}.settings
  settings = {
    display = "core"
  };

  // layout settings
  layoutSettings = {
    defaultLayout = "MyModuleLayout.cfm"
  };

  // Module Conventions
  conventions = {
    handlersLocation = "handlers",
    viewsLocation = "views",
    layoutsLocation = "layouts",
    pluginsLocation = "plugins",
    modelsLocation = "model"
  };

  // datasources
  datasources = {
    mysite   = {name="mySite", dbType="mysql", username="root", password="root"}
  };

  // SES Routes
  routes = [
    // load the routes.cfm in the config folder of the module
    "config/routes",
    // explicit route declared here
    {pattern="/api-docs", handler="api",action="index"}   
  ];    

  // Interceptor Config
  interceptorSettings = {
    customInterceptionPoints = "onModuleError"
  };
  interceptors = [
    {name="modulesecurity",class="#moduleMapping#.interceptors.ModuleSecurity",
     properties={
      URLMatch = '/api-docs',
      loginURL = '/api-docs/login'
     }
  ];  

  // binder mappings
  binder.map("MyModuleService").to("#moduleMapping#.model.ModuleService");
  // or easy map entire directory
  binder.mapDirectory("#moduleMapping#.model");
}
```

## Module Settings

### Retrieving Module Settings

Modules configurations are stored in the host parent configuration structure under a `modules` key according to their name. To retrieve them you can do

* `getModuleSettings( module )` : Returns the structure of module settings by the module name.
* `getModuleConfig( module )` : Returns the module's configuration structure

### Injecting Module Settings

You can also use our injection DSL to inject module settings and configurations:

```javascript
// Inject all module settings
property name="settings" inject="coldbox:modulesettings:{module}";
// Inject the module configuration structure
property name="config" inject="coldbox:moduleConfig:{module}";
// Inject a single module setting
property name="customSetting" inject="coldbox:setting:customSetting@{module}";
```

The injection DSL pattern is the following:

* `coldbox:setting:mySetting@module` : You can now inject easily a module setting by using the @module directive.
* `coldbox:moduleSettings:{module}` : You can now inject the entire module settings structure
* `coldbox:moduleConfig:{module}` : You can now inject the entire module configuration structure 

### Overriding Module Settings

If you have `parseParentSettings` set to true in your `ModuleConfig.cfc` \(which is the default\), ColdBox will look for a struct inside the `moduleSettings` struct of your `config/ColdBox.cfc` with a key equal to your module's `this.modelNamespace` \(default is the module name\) and merge them with your modules `settings` struct \(as defined in your module's `configure()` method\) with the parent settings overwritting the module settings. This allows a user of your module to easily overwrite specific settings for their needs.

```javascript
// myModule/ModuleConfig.cfc
component {
  function configure() {
    settings = {
      someSetting = "default",
      anotherSetting = "default"
    };
  }
}

// config/ColdBox.cfc
component {
  function configure() {
    moduleSettings = {
      myModule = {
        someSetting = "overridden" 
      }
    };
  }
}

// end result
{
  someSetting = "overridden",
  anotherSetting = "default"
}
```

If `parseParentSettings` is set to `false`, your module's `settings` will instead overwrite the settings set in the same `moduleSettings` struct.

### Using Overridden Settings in your `ModuleConfig.cfc`

If you want to use the overridden settings in your `ModuleConfig.cfc`, you will need to use it in the `postModuleLoad` interceptor. Remember, all your modules register the `ModuleConfig.cfc` as an interceptor, so all you need to do is add the `postModuleLoad` function and you're off!

```javascript
function postModuleLoad( event, interceptData, buffer, rc, prc ) {
  binder.map( "MyAlias@MyModule" )
    .to( "#moduleMapping#.models.#settings.moduleName#" );
}
```

## Environment Control

If you are using per-environment control in your parent application via the `ColdBox.cfc`, you can also use that in your Module Configuration object. So the same conventions that are used in the parent configuration can be used in the module; having the name of the environment match a method name in your module config. So if the following environments are declared in your parent configuration file and the **dev** environment is detected, the `dev()` method is called in your configuration object:

```javascript
environments = {
  dev = "^railo.*,^cf.*,^local.*"
};

function dev(){
  // my overrides here
  coldbox.handlerCaching = false;
}
```

But if you declare that `dev()` method in your Module Configuration object, it will be called as well **after** your `configure()` method is called:

```javascript
function configure(){}

function dev(){
  // override module settings for dev here.
}
```

## Interceptor Events

The module configuration object is also treated as an Interceptor once it is created and configured.

### Life-cycle Events

There are two life-cycle callback events you can declare in your `ModuleConfig.cfc`:

* `onLoad()` : Called when the module is loaded and activated
* `onUnLoad()` : Called when the module is unloaded from memory

This gives you great hooks for you to do bootup and shutdown commands for this specific module. You can build a [ContentBox](http://ortussolutions.com/products/contentbox/) or [Wordpress](http://wordpress.org/) like application very easily all built in to the developing platform.

```javascript
function onLoad(){
  // Register some tables in my database and activate some features
  controller.getWireBox().getInstance('MyModuleService').activate();
    log.info( 'Module #this.title# loaded correctly' );
}

function onUnLoad(){
  // Cleanup some stuff and logit
  controller.getWireBox().getInstance('MyModuleService').shutdown();

  // Log we unloaded
  log.info( 'My module unloaded successfully!' );
}
```

### Custom Events

Also, remember that the configuration object itself is an interceptor so you can declare all of the framework's interception points in the configuration object and they will be registered as interceptors.

```javascript
function preProcess(event, interceptData){
  // I just intercepted ALL incoming requests to the application
  log.info('The event executed is #arguments.event.getCurrentEvent()#');
}
```

This is a powerful feature, you can create an entire module based on a single CFC and just treat it as an interceptor. So get your brain into gear and let the gas run, you are going for a ride baby!

## Module Event Executions

Module event executions are done almost exactly the same way we are used to in our ColdBox applications using event syntax patterns. By now we understand that an event comes in via the URL/FORM or Remotely to the framework and then the framework decides what event to execute. We also know that we can abstract our incoming events by using the ColdBox URL Routing, but at the end of the day, we will always have an `event` variable in the request collection that tells the framework what event to execute. The typical event syntax pattern we have learned is:

```javascript
// Pattern
event=[package.]handler[.action]

// Examples:
event=blog.posts
event=home.index
event=admin.dashboard.index
```

This typical approach maps the event to a package, handler and method combination. With the addition of modules our event syntax pattern now morphs to the following:

```javascript
// Pattern
event=[module:][package.]handler[.action]

// Examples:
event=blog:posts.index
event=cms:page.show
event=admin:dashboard
```

As you can see, we can prefix the event syntax with a module name and then followed by a colon `{module}:`. This is how ColdBox can know to what module to redirect the execution to. You can even execute the `runEvent()` methods and target modules:

```javascript
// Execute a viewlet in a module called viewlets:
#runEvent('viewlets:users.dashboard')#
```

> **Hint** Please remember that this is great for securing your applications as the event patterns you can match against with regular expressions will help you tremendously, as you can pinpoint modules directly.

In summary, the event syntax has been updated to support module executions via the `{module:}` prefix. However, please note that our preference is to abstract URLs and incoming event variables \(via FORM/URL\) by using ColdBox URL Routing. In the next section we will revise how to make module URL Routings work.

## URL Routing

All modules have the capability to leverage URL routing in a portable manner. They will automatically register an entry point URL pattern via the `this.entryPoint` setting. If an incoming URL has that specific pattern, then it will search for the module's routing table for a match.

```javascript
this.entryPoint = "/mymodule";
```

### Parent Manual Routing

You can also add these entry points manually in the host application's routing file: `config/Routes.cfm`. However, you will loose all module portability. We do this by using a method called `addModuleRoutes()` method.

* `addModuleRoutes(pattern, module)` : Insert the module routes at this location in the configuration file with the applied URL pattern.

The URL pattern is the normal URL Mappings pattern we are used to and the module is the name of the module you want to target. What this does is create an entry point pattern that will identify the module's routing capabilities. For example, we create the following:

```javascript
addModuleRoutes(pattern="/blog",module="simpleblog");
addModuleRoutes(pattern="/admin",module="admin");
```

What the previous method calls do is bind a static URL entry pattern to a module. So if the framework detects an incoming URL with the starting point to be /blog, it will then match the simpleblog routes. Once matched, it will now try to match the rest of the incoming URL with the module's custom routes. Let's do a full example, below are some custom routes for my blog module in its `ModuleConfig.cfc`:

```javascript
// In my ModuleConfig.cfc
routes = [
    {pattern="/", handler="blog", action="showPosts"},
    {pattern="/:year-numeric/:month-numeric?", handler="blog", action="showPosts"}
    {pattern="/comment/:action", handler="comments"}
]
```

Now, let's say the URL that is incoming is:

```javascript
http://mysite.com/blog
```

Then this will resolve to the `/blog` pattern in the host that says, now look in the module simpleblog for routes. The left over part of the URL is nothing or `/` so the pattern that matches is my first declared pattern:

```javascript
{pattern="/", handler="blog", action="showPosts"}
```

This means, that we will execute the modules' `blog` handler and the `showPosts` method. Now, let's say the next URL that comes is:

```javascript
http://mysite.com/blog/2009/09
```

Then this will match the `simpleblog` module via the static `/blog` entry point and then it tries to find a match for `/2009/09` in the modules' routes and it does!

## Default Route Execution

By now, we should all know the default SES route ColdBox offers: `addRoute(":handler/:action?")`. This means that we can target a handler with or without a package and an action for execution. In ColdBox we have also added a package resolver that will detect this pattern for module and package directories so we can do URL safe and SES friendly URLs for these executions by convention.

If we do not have this feature, this is how the URLs would look like:

```javascript
http://mysite.com/module:handler/action
or
http://mysite.com/module:package.handler/action
```

However, thanks to package resolving in the SES interceptor we can do links like this:

```javascript
http://mysite.com/module/handler/action
or
http://mysite.com/module/package/handler/action
```

Much better and nicer huh? Of course! So potentially, with one route we could write entire applications.

## Module Routes Files

A module can also include one or more custom routing files in order to take advantage of our routing DSL and also have better separation as they will be stored outside of the module configuration object. You do this by giving the path to the custom file to include in your routes structure:

```javascript
routes = [
  "config/routes"
];
```

This will look for a `routes.cfm` template in your module's `config` folder and load it. Please note that you do not need to specify a `.cfm` if you don't want to. You can load as many route files as you like.

### ColdBox 5 Router

If you want to use the ColdBox 5 `Router.cfc` inside your module you can simply place the `Router.cfc` inside the `config` folder and it will load by convention.

## Request Context Module Methods

The request context object \(`event` parameter received in handlers/layouts/views\) has also been expanded to have the following module methods:

* `getCurrentModule()` : Returns the module name of the currently executing module event.
* `getModuleRoot([moduleName])` : Returns the web accessible root to the modules root directory. If you do not pass the explicit module name, we will default to use the `getCurrentModule()` in the request.

The last method is what is really interesting when building visual modules that need assets from within the module itself. You can easily target the web-accessible path to the module by using the `getModuleRoot()` method. Below are some examples:

```javascript
<head>
  <meta name="author" content="Luis Majano" />
  <meta http-equiv="content-type" content="text/html;charset=utf-8" />

    <---< Base HREF if using SES --->
  <cfif event.isSES()>
    <base href="#getSetting('htmlBaseURL')#">
  </cfif>

    <---<Module  CSS --->
  <link rel="stylesheet" href="#event.getModuleRoot()#/includes/css/style.css" type="text/css" />
  <link rel="stylesheet" href="#event.getModuleRoot()#/includes/js/ratings/jquery.ratings.css" type="text/css" />

  <---< Module Javascript --->
  <script type="text/javascript" src="#event.getModuleRoot()#/includes/js/jquery-latest.pack.js"></script>
  <script type="text/javascript" src="#event.getModuleRoot()#/includes/js/forgebox.js"></script>
  <script type="text/javascript" src="#event.getModuleRoot()#/includes/js/jquery.simplemodal-latest.min.js"></script>
  <script type="text/javascript" src="#event.getModuleRoot()#/includes/js/jquery.uidivfilter.js"></script>
  <script type="text/javascript" src="#event.getModuleRoot()#/includes/js/ratings/jquery.ratings.pack.js"></script>

  <title>The Awesome ForgeBox Module!</title>
</head>
```

As you can see, this is essential when building module UIs or layouts.

## Layout and View Renderings

The ColdBox rendering engine has been adapted to support module renderings and also to support multiple discovery algorithms when rendering module layouts and views. When you declare a module you can declare two of its public properties to determine how rendering overrides occur. These properties are:

| Property | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| viewParentLookup | boolean | false | true | If true, coldbox checks for views in the parent overrides first, then in the module. If false, coldbox checks for views in the module first, then the parent. |
| layoutParentLookup | boolean | false | true | If true, coldbox checks for layouts in the parent overrides first, then in the module. If false, coldbox checks for layouts in the module first, then the parent. |

## Layout\/View Discovery

The default order of overrides ColdBox offers is both `viewParentLookup & layoutParentLookup` to **true**. This means that if the layout or view requested to be rendered by a module exists in the overrides section of the host application, then the host application's layout or view will be rendered instead. Let's investigate the order of discover:

**viewParentLookup = true**

1. Host override module specific `(e.g. {HOST}/views/modules/myModule/myView.cfm)`
2. Host override common `(e.g. {HOST}/views/modules/myView.cfm)`
3. Module view `(e.g. /modules/myModule/views/myView.cfm)`
4. Default view discovery from host `(e.g. {HOST}/views/myView.cfm)`

**viewParentLookup = false**

1. Module view `(e.g. /modules/myModule/views/myView.cfm)`
2. Host override module specific `(e.g. {HOST}/views/modules/myModule/myView.cfm)`
3. Host override common `(e.g. {HOST}/views/modules/myView.cfm)`
4. Default view discovery from host `(e.g. {HOST}/views/myView.cfm)`

**layoutParentLookup = true**

1. Host override module specific `(e.g. {HOST}/layouts/modules/myModule/myLayout.cfm)`
2. Host override common `(e.g. {HOST}/layouts/modules/myLayout.cfm)`
3. Module layout `(e.g. /modules/myModule/layouts/myLayout.cfm)`
4. Default layout discovery from host `(e.g. {HOST}/layouts/Default.cfm)`

**layoutParentLookup = false** 1. Module layout `(e.g. /modules/myModule/layouts/myLayouts.cfm)` 2. Host override module specific `(e.g. {HOST}/layouts/modules/myModule/myLayout.cfm)` 3. Host override common `(e.g. {HOST}/layouts/modules/myLayout.cfm)` 4. Default layout discovery from host `(e.g. {HOST}/layouts/Default.cfm)`

Let's do some real examples, I am building a simple module with 1 layout and 1 view. Here is my directory structure for them:

```javascript
/simpleModule
  + layouts
    + main.cfm
  + views
    + simple
       + index.cfm
```

Now, in my handler code I just want to render the view by using our typical `event.setView()` method calls or implicit views.

```javascript
// In a handler called simple.cfc
component{

  function index(event){
      // DO SOME CODE HERE

        // Set the view to render with a layout
      event.setView(view='simple/index',layout="main");
    }
}
```

## Overriding Views

This tells the framework to render the `simple/index.cfm` view in the module `simpleModule`. However, let's override the view first. Go to the host application's views folder and create a folder called `modules` and then a folder according to the module name, in our case `simpleModule`:

```javascript
/MyApp
  + views 
    + modules 
       + simpleModule
```

What this does, is create the override structure that you need. So now, you can map 1-1 directly from this overrides folder to the module's views folder. So whatever view you place here that matches 1-1 to the module, the parent will take precedence. Now remember this is because our `viewParentLookup` property is set to true. If we disable it or set it to false, then the module takes precedence first. If not found in the module, then it goes to the parent or host application and tries to locate the view. That's it, so if I place a `simple/index.cfm` view in my parent structure, the that one will take precedence.

## Overriding Layouts

Now, let's say you want to override the layout for a module. Go to the host application's layouts folder and create a folder called `modules` and then a folder according to the module name, in our case `simpleModule`:

```javascript
/MyApp
  + layouts 
    + modules 
       + simpleModule
```

What this does, is create the override structure that you need. So now, you can map 1-1 directly from this overrides folder to the module's layouts folder. So whatever layout you place here that matches 1-1 to the module, the parent will take precedence. Now remember this is because our layouParentLookup property is set to true. If we disable it or set it to false, then the module takes precedence first. If not found in the module, then it goes to the parent or host application and tries to locate the layout. That's it, so if I place a main.cfm layout in my parent structure, the that one will take precedence.

```javascript
/MyApp
  + layouts 
    + modules 
       + simpleModule
         + main.cfm
```

These features are great for skinning modules or just overriding view capabilities a-la-carte. So please take note of them as it is very powerful.

## Default Module Layout

You can also implicitly setup a default layout to use when rendering views from the specific module. This only works when you call the `event.setView()` method from your module event handlers. Once called, the framework will discover the default layout to render that view in. You set up the default layout for a module by using the layoutSettings structure in your configuration:

```javascript
layoutSettings = {
  defaultLayout = "MyLayout.cfm"
};
```

> **Caution** The set default layout MUST exist in the layouts folder of your module and the declaration must have the `.cfm` extension attached.

## Explicit Module Renderings

You can also explicitly render layouts or views directly from a module via the Renderer plugin's `renderLayout()` and `renderView()` methods. These methods now can take an extra argument called module.

* `renderLayout(layout, view, module)`
* `renderView(view, cache, cacheTimeout,cacheLastAccessTimeout,cacheSuffix, module)`

So you can easily pinpoint renderings if needed.

> **Caution** Please note that whenever these methods are called, the override algorithms ALSO are in effect. So please refer back to the view and layout parent lookup properties in your modules' configuration.

## Models

When you declare a module and you define a `models` folder then the framework automatically register all models in that folder for you using a namespace of `@moduleName`. This means that all models are registered according to their CFC name plus the namespace.

> **Info** Internally, ColdBox uses WireBox's `mapDirectory()` to map the entire `models` directory for you.

Let's say you have a module called `store` and a `OrderService.cfc` inside of the `models` folder. That object will have a WireBox id of `OrderService@store`.

```javascript
property name="orderService" inject="OrderService@store";
```

> **Hint** You can alter this behavior by setting the `this.autoMapModels` configuration setting to **false**. You can also alter the namespace used via the `this.modelNamespace` configuration property.

## Module CF Mappings

Every module can tell ColdBox what ColdFusion mapping to register for it that points to the module root location on disk when deployed. This is a huge feature for portability and the ability to influence the ColdFusion mappings for you via ColdBox. Just create a `this.cfmapping` property in your `ModuleConfig.cfc`:

```javascript
this.cfmapping = "cbstore";
```

## Module Dependencies

Modules can declare other module dependencies in the `ModuleConfig.cfc` via the `this.dependencies` property. This means that **before** the declared module is activated, the dependencies will be registered and activated **FIRST** and then the declared module will load.

```javascript
this.dependencies = [ "javaloader" ];
```

This means that the `javaloader` module HAS to load and activate first before this module can finish registering and activating.

## Module Helpers

Every module can declare an array of helper templates that contain **methods** that will be added as **global** application helpers. This is a great way to collaborate global functions to the running MVC application.

This is done via the `this.applicationHelper` directive in your `ModuleConfig.cfc`. This is an array of relative paths in the module that ColdBox will load as mixins.

```javascript
this.applicationHelper = [
    "includes/helpers.cfm"
];
```

Here is a sample of a `helpers.cfm`:

```javascript
<cfscript>
    function auth() {
        return wirebox.getInstance( "AuthenticationService@cbauth" );
    }
</cfscript>
```

## Module Bundles

You can bundle your modules into an organizational folder that has the convention name of `{name}-bundle`. This will allow you to bundle different modules together for organizational purposes only. Great for git ignores.

```text
coldbox-bundle
  * cbstorages
  * cborm
  * cbsecurity
```

## Module Inception

ColdBox 4 allows you to nest modules within modules up to the Nth degree. You can package a module with other modules that can even contain other modules within them. It really opens a great opportunity for better packaging, delivery and a further break from monolithic applications. To use, just create a `modules` folder in your module and drop the modules there as well. You can also just create a `box.json` in the root of your module and let CommandBox manage your module dependencies. Here is an example of the ColdBox ORM Module:

```javascript
{
    "name"         : "ColdBox ORM Extensions",
    "version"    : "1.0.3",
    "author"     : "Luis Majano <lmajano@ortussolutions.com",
    "slug"        : "cborm",
    "type"        : "modules",
    "homepage"    : "http://www.coldbox.org",
    "documentation"        : "http://wiki.coldbox.org",
    "repository"        : { "type" : "git", "url" : "https://github.com/ColdBox/cbox-cborm" },
    "shortDescription"    : "Enhances the ColdFusion ORM with tons of utilities.",
    "license"            : [
        { "type" : "Apache2", "url" : "http://www.apache.org/licenses/LICENSE-2.0.html" }
    ],
    "contributors"        : [
        "Brad Wood <bdw429s@gmail.com>", "Curt Gratz <gratz@computerknowhow.com>", "Joel Watson <existdissolve@gmail.com>"
    ],
    "dependencies"     :{
        "cbvalidation" : "1.0.2"
    },
    "ignore":[
        "**/.*",
        "tests",
        "apidocs",
        "*/.md"
    ]
}
```

