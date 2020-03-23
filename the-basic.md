# The Basic

## Request Context

On every request to a ColdBox event, the framework creates an object that models the incoming request. This object is called the **Request Context Object**\(`coldbox.system.web.context.RequestContext`\), it contains the incoming **FORM/REMOTE/URL** variables the client sent in and the object lives in the ColdFusion `request` scope.

{% hint style="info" %}
Please visit the latest [API Docs](http://apidocs.ortussolutions.com/coldbox/current) for further information about the request context.
{% endhint %}

This object contains two structures internally:

1. `RC` - The Request Collection which contains the **FORM/REMOTE/URL** data merged into a single structure.  This is considered to be **unsafe** data as it comes from any request.
2. `PRC` - The Private Request Collection which is a structure that can be used to safely store sensitive data.  This structure cannot be modified from the outside world.

{% hint style="info" %}
The order of preference of variables when merged is **FORM** first then **REMOTE** then **URL**.

**REMOTE** variables are from leveraging the [ColdBox Proxy.](../digging-deeper/coldbox-proxy/)
{% endhint %}

{% hint style="info" %}
You can enable `coldbox.jsonPayloadToRC = true` in your coldbox config if you want to merge variables from a JSON request body in the `RC`.  
See release notes[ 5.1.0](https://coldbox.ortusbooks.com/intro/introduction/whats-new-with-5.1.0#new-auto-deserialization-of-json-payloads) and [5.1.2](https://coldbox.ortusbooks.com/intro/introduction/whats-new-with-5.1.2#automatic-json-payload-setting) for details.
{% endhint %}

You will use these objects in the controller and view layer of your application to get/set values, get metadata about the request, generate URLs, transform data for RESTful requests, and so much more. It is the glue that binds the controller and view layer together. As we progress in the guides, you will progress in mastering the request context.

{% hint style="danger" %}
Note that there is no model layer in the diagram. This is by design; the model will receive data from the handlers/interceptors directly.
{% endhint %}

### Most Commonly Used Methods

Below you can see a listing of the most commonly used methods in the request context object. Please note that when interacting with a collection you usually have an equal **private** collection method.

* _buildLink\(\)_ : Build a link in SES or non SES mode for you with tons of nice abstractions.
* _clearCollection\(\)_ : Clears the entire collection
* _collectionAppend\(\)_ : Append a collection overwriting or not
* _getCollection\(\)_ : Get a reference to the collection
* _getEventName\(\)_ : The event name in use in the application \(e.g. do, event, fa\)
* _getSelf\(\)_ : Returns index.cfm?event=
* _getValue\(\)_ : get a value
* _getTrimValue\(\)_ : get a value trimmed
* _isProxyRequest\(\)_ : flag if the request is an incoming proxy request
* _isSES\(\)_ : flag if ses is turned on
* _isAjax\(\)_ : Is this request ajax based or not
* noRender\(boolean\) : flag that tells the framework to not render any html, just process and silently stop.
* _overrideEvent\(\)_ : Override the event in the collection
* _paramValue\(\)_: param a value in the collection
* _removeValue\(\)_ : remove a value
* _setValue\(\)_ : set a value
* _setLayout\(\)_ : Set the layout to use for this request
* _setView\(\)_ : Used to set a view to render
* _valueExists\(\)_ : Checks if a value exists in the collection.
* _renderData\(\)_ : Marshall data to JSON, JSONP, XML, WDDX, PDF, HTML, etc.

Some Samples:

```javascript
//Get a reference
<cfset var rc = event.getCollection()>

//test if this is an MVC request or a remote request
<cfif event.isProxyRequest()>
  <cfset event.setValue('message', 'We are in proxy mode right now')>
</cfif>

//param a variable called page
<cfset event.paramValue('page',1)>
//then just use it
<cfset event.setValue('link','index.cfm?page=#rc.page#')>

//get a value with a default value
<cfset event.setvalue('link','index.cfm?page=#event.getValue('page',1)#')>

//Set the view to render
<cfset event.setView('homepage')>

//Set the view to render with no layout
<cfset event.setView('homepage',true)>

//set the view to render with caching stuff
<cfset event.setview(name='homepage',cache='true',cacheTimeout='30')>

//override a layout
<cfset event.setLayout('Layout.Ajax')>

//check if a value does not exists
<cfif not event.valueExists('username')>

</cfif>

//Tell the framework to stop processing gracefully, no renderings
<cfset event.noRender()>

//Build a link
<form action="#event.buildLink('user.save')#" method="post">
</form>
```

Please see the online [API Docs](http://apidocs.ortussolutions.com/coldbox/current) for the latest methods and arguments.

### Request Metadata Methods

* `getCurrentAction()` : Get the current execution action \(method\)
* `getCurrentEvent()` : Get's the current incoming event, full syntax.
* `getCurrentHandler()` : Get the handler or handler/package path.
* `getCurrentLayout()` : Get the current set layout for the view to render.
* `getCurrentView()` : Get the current set view
* `getCurrentModule()` : The name of the current executing module
* `getCurrentRoutedNamespace()` : The current routed URL mapping namespace if found.
* `getCurrentRoutedURL()` : The current routed URL if matched.
* `getDebugpanelFlag()` : Get's the boolean flag if the ColdBox debugger panel will be rendered.
* `getDefaultLayout()` : Get the name of the default layout.
* `getDefaultView()` : Get the name of the default view.

## Routing

ColdBox supports a Routing Service that will provide you with robust URL mappings for building expressive applications and RESTFul services. By convention URL routing will allow you to create URL's without using verbose parameter delimiters like `?event=this.that&m1=val` and execute ColdBox events.

```javascript
// Old Style
http://localhost/index.cfm?event=home.about&page=2
http://localhost/index.cfm?city=24&page=3&county=234324324
```

```javascript
// New Routing Style
http://localhost/home/about/page/2
http://localhost/dade/miami/page/3
```

{% hint style="info" %}
If you are leveraging CommandBox as your server, then full URL rewrites are enabled by **default**. This means you do not need a web server to remove the `index.cfm` from the URL.
{% endhint %}

### What is a route?

A route is a declared URL pattern that if matched it will translate the URL into one of the following:

* A ColdBox event to execute
* A View/Layout to render
* A Reponse function to execute
* A Redirection to occur

It will also inspect the URL for **placeholders** and translate them into the incoming Request Collection variables \(`RC`\).

**Examples**

{% code title="config/Router.cfc" %}
```javascript
function configure(){

    // Routing with placeholders to an event with placeholders
    route( "/blog/:year-numeric{4}/:month?/:day?" )
        .to( "blog.list" );

    // Redirects
    route( "/old/book" )
        .redirect( "/mybook" );

    // Responses
    route( "/echo" ).toResponse( (event,rc,prc) => {
        return "hello luis";
    } );

    // Shortcut to above
    route( "/echo", (event,rc,prc) => {
        return "hello luis";
    } );

    // Show view
    route( "/contact-us" )
        .toView( "main/contact" );

    // Direct to handler with action from URL
    route( "/users/:action" )
        .toHandler( "users" );

    // Inline pattern + target and name
    route( pattern="/wiki/:page", target="wiki.show", name="wikipage" );

}
```
{% endcode %}

### Routing Benefits

There are several benefits that you will get by using our routing system:

* Complete control of how URL's are built and managed
* Ability to create or build URLs' dynamically
* Technology hiding
* Greater application portability
* URL's are more descriptive and easier to remember

### Route Visualizer

As you create route-heavy applications visualizing the routes will be challenging especially for HMVC apps with lots of modules. Just install our [ColdBox Route Visualizer](https://www.forgebox.io/view/route-visualizer) and you will be able to visually see, test and debug all your routing needs.

```bash
box install route-visualizer
```

## Requirements

Routing is enabled by **default** in the ColdBox application templates in order to work with URL's like this:

`http://localhost/index.cfm/home/about`

As you can see they still contain the `index.cfm` in the URL. In order to enable full URL rewrites that eliminates that `index.cfm` you must have a rewrite enabled webserver like Apache, nginx or IIS or a Java rewrite filter which ships with CommandBox by **default**.

`http://localhost/home/about`

CommandBox has built in rewrites powered by Tuckey and you can enable a server with rewrites by running:

```text
server start --rewritesEnable
```

{% hint style="danger" %}
**Caution** Some J2EE servlet containers do not support the forwarding of SES parameters via the routing template \(`index.cfm`\) out of the box. You might need to enable full URL rewriting either through a web server or a J2EE filter.
{% endhint %}

### Some Resources

* [Apache mod\_rewrite](http://httpd.apache.org/docs/current/mod/mod_rewrite.html) via .htaccess or configuration files \(Free\)
* [Helicon Tech](http://www.helicontech.com/) ISAPI rewrite filter for IIS \(Paid\)
* [IIS 7](http://www.iis.net/downloads/microsoft/url-rewrite) native rewrite filter \(Free\)
* [nginx](http://nginx.org/) native web server \(free\)
* [Tuckey](http://www.tuckey.org/) J2EE rewrite filter \(free\)

## Rewrite Rules

Here are just a few of those rewrite rules for you for major rewrite engines. You can spice them up as needed.

### .htaccess

```javascript
RewriteEngine on
#if this call related to adminstrators or non rewrite folders, you can add more here.
RewriteCond %{REQUEST_URI} ^/(.*(CFIDE|cfide|CFFormGateway|jrunscripts|railo-context|lucee|mapping-tag|fckeditor)).*$
RewriteRule ^(.*)$ - [NC,L]

#Images, css, javascript and docs, add your own extensions if needed.
RewriteCond %{REQUEST_URI} \.(bmp|gif|jpe?g|png|css|js|txt|xls|ico|swf)$
RewriteRule ^(.*)$ - [NC,L]

#The ColdBox index.cfm/{path_info} rules.
RewriteRule ^$ index.cfm [QSA,NS]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.cfm?%{REQUEST_URI} [QSA,L,NS]
```

### nginx

```text
################### LOCATION: ROOT #####################
location / {
     # First attempt to serve real files or directory, else it sends it to the @rewrite location for processing
     try_files $uri $uri/ @rewrite;
}

################### @REWRITE: COLDBOX SES RULES #####################
# Rewrite for ColdBox (only needed if you want SES urls with this framework)
# If you don't use SES urls you could do something like this
# location ~ \.(cfm|cfml|cfc)(.*)$ {
location @rewrite {
  rewrite ^/(.*)? /index.cfm/$request_uri last;
  rewrite ^ /index.cfm last;
}

################### CFM/CFC LUCEE HANDLER #####################
# The above locations will just redirect or try to serve cfml files
# We need this to tell NGinx that if we receive the following requests to pass them to Lucee
location ~ \.(cfm|cfml|cfc|jsp)(.*)$ {
# Include our connector
include lucee.conf;
}
```

### IIS7 web.config

Please note that URL rewriting is handled by an optional module in IIS. More info here: [https://www.iis.net/downloads/microsoft/url-rewrite](https://www.iis.net/downloads/microsoft/url-rewrite)

```javascript
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <rewrite>
            <rules>
               <rule name="Application Administration" stopProcessing="true">
                    <match url="^(.*)$" />
                    <conditions logicalGrouping="MatchAll">
                        <add input="{SCRIPT_NAME}" pattern="^/(.*(CFIDE|cfide|CFFormGateway|jrunscripts|lucee|railo-context|fckeditor)).*$" ignoreCase="false" />
                    </conditions>
                    <action type="None" />
                </rule>
                <rule name="Flash and Flex Communication" stopProcessing="true">
                    <match url="^(.*)$" ignoreCase="false" />
                    <conditions logicalGrouping="MatchAll">
                        <add input="{SCRIPT_NAME}" pattern="^/(.*(flashservices|flex2gateway|flex-remoting)).*$" ignoreCase="false" />
                    </conditions>
                    <action type="Rewrite" url="index.cfm/{PATH_INFO}" appendQueryString="true" />
                </rule>
                <rule name="Static Files" stopProcessing="true">
                    <match url="^(.*)$" />
                    <conditions logicalGrouping="MatchAll">
                        <add input="{SCRIPT_NAME}" pattern="\.(bmp|gif|jpe?g|png|css|js|txt|pdf|doc|xls)$" ignoreCase="false" />
                    </conditions>
                    <action type="None" />
                </rule>
                <rule name="Insert index.cfm" stopProcessing="true">
                    <match url="^(.*)$" ignoreCase="false" />
                    <conditions logicalGrouping="MatchAll">
                        <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
                        <add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" />
                    </conditions>
                    <action type="Rewrite" url="index.cfm/{PATH_INFO}" appendQueryString="true" />
                </rule>
            </rules>
        </rewrite>
    </system.webServer>
</configuration>
```

### Tuckey Rewrite

```text
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE urlrewrite PUBLIC "-//tuckey.org//DTD UrlRewrite 3.2//EN" "http://tuckey.org/res/dtds/urlrewrite3.2.dtd">
<urlrewrite>
    <rule>
        <note>ContentBox Media URLs</note>
        <condition type="request-uri" operator="equal">^/__media/.*$</condition>
        <from>^/(.*)$</from>
        <to type="passthrough">/index.cfm/$1</to>
    </rule>
    <rule>
        <note>Generic Front-Controller URLs</note>
        <condition type="request-uri" operator="notequal">/(index.cfm|robots.txt|osd.xml|flex2gateway|cfide|cfformgateway|railo-context|lucee|admin-context|modules/contentbox-dsncreator|modules/contentbox-installer|modules/contentbox|files|images|js|javascripts|css|styles|config).*</condition>
        <condition type="request-uri" operator="notequal">\.(bmp|gif|jpe?g|png|css|js|txt|xls|ico|swf|woff|ttf|otf)$</condition>
        <!-- For some reason this is not working now.
        <condition type="request-filename" operator="notdir"/>
        <condition type="request-filename" operator="notfile"/>
        -->
        <from>^/(.+)$</from>
        <to type="passthrough">/index.cfm/$1</to>
    </rule>
</urlrewrite>
```

## Application Router

Every ColdBox application has a URL router and can be located by convention at `config/Router.cfc`. This is called the **application router** and it is based on the router core class: `coldbox.system.web.routing.Router`. Here is where you will configure router settings and define routes using our routing DSL.

{% hint style="info" %}
Please see the latest [API Docs](http://apidocs.ortussolutions.com/coldbox/5.0.0/coldbox/system/web/routing/Router.html) for investigating all the methods and properties of the Router.
{% endhint %}

{% hint style="success" %}
**Tip:** Unlike previous versions of ColdBox, the new routing services in ColdBox 5 are automatically configured to detect the base URLs and support multi-domain hosting. There is no more need to tell the Router about your base URL.
{% endhint %}

### Application Router - `Router.cfc`

{% code title="config/Router.cfc" %}
```javascript
// Inherits from Router and FrameworkSuperType
component{

    function configure(){
        // Turn on Full URL Rewrites, no index.cfm in the URL
        setFullRewrites( true );

        // Routing by Convention
        route( "/:handler/:action?" ).end();

    }

}
```
{% endcode %}

The application router is a simple CFC that virtually inherits from the core ColdBox Router class and is configured via the `configure()` method. It will be decorated with all the capabilities to work with any request much like any event handler or interceptor. In this router you will be doing 1 of 2 things:

1. Configuring the Router
2. Adding Routes via the Routing DSL

### Router as an Interceptor

The router and indeed all module routers are also registered as full fledged ColdBox interceptors. So they can listen to any event within your application.

### Generated Settings

Once the routing service loads your Router it will create two application settings for you:

* `SESBaseURL` : The multi-domain URL base URL of your application: `http://localhost`
* `HTMLBaseURL` : The same path as `SESBaseURL`but without any `index.cfm` in it \(Just in case you are using `index.cfm` rewrite\). This is a setting used most likely by the HTML `<base>` tag.

### Configuration Methods

You can use the following methods to fine tune the configuration and operation of the routing services:

| **Method** | **Description** |
| :--- | :--- |
| `setEnabled( boolean )` | Enable/Disable routing, **enabled** by default |
| `setFullRewrites( boolean )` | If **true**, then no `index.cfm` will be used in the URLs. If **false**, then **/index.cfm/** will be added to all generated URLs. Default is **false**. |
| `setUniqueURLS( boolean )` | Enables SES only URL's with permanent redirects for non-ses urls. Default is **true.**  If true and a URL is detected with ? or & then the application will do a 301 Permanent Redirect and try to translate the URL to a valid SES URL. |
| `setBaseURL( string )` | The base URL to use for URL writing and relocations. This is automatically detected by ColdBox 5 e.g. [http://www.coldbox.org/](http://www.coldbox.org/), [http://mysite.com/index.cfm](http://mysite.com/index.cfm)'' |
| `setLooseMatching( boolean )` | By default URL pattern matching starts at the beginning of the URL, however, you can choose loose matching so it searches anywhere in the URL. Default is **false**. |
| `setExtensionDetection( boolean )` | By default ColdBox detects URL extensions like `json, xml, html, pdf` which can allow you to build awesome RESTful web services. Default is **true**. |
| `setValidExtensions( list )` | Tell the interceptor what valid extensions your application can listen to. By default it listens to: `json, jsont, xml, cfm, cfml, html, htm, rss, pdf` |
| `setThrowOnInvalidExtensions( boolean )` | By default ColdBox does not throw an exception when an invalid extension is detected. If **true**, then the interceptor will throw a 406 Invalid Requested Format Extension: {extension} exception. Default is **false**. |

```javascript
function configure(){

    setFullRewrites( true );
    setExtensionDetection( true );
    setValidExtensions( "json,cfm,pdf" );

}
```

The next sections will discus how to register routes for your application.

## Routing DSL

The ColdBox Routing DSL will be used to register routes for your application, which exists in your application or module router object. Routing takes place using several methods inside the router, which are divided into the following 3 categories:

1. **Initiators** - Starts a URL **pattern** registration, but does not fully register the route until a terminator is called \(**target**\).
2. **Modifiers** - Modifies the pattern with extra metdata to listen to from the incoming request.
3. **Terminators** - Finalizes the registration process usually by telling the router what happens when the route pattern is detected. This is refered to as the **target.**

{% hint style="warning" %}
Please note that order of declaration of the routes is imperative. Order matters.
{% endhint %}

{% hint style="info" %}
Please remember to check out the latest [API Docs](http://apidocs.ortussolutions.com/coldbox/current) for the latest methods and argument signatures.
{% endhint %}

#### Initiators

The following methods are used to initiate a route registration process.

{% hint style="danger" %}
Please note that a route will not register unless a terminator is called or the inline target terminator is passed.
{% endhint %}

* `route( pattern, [target], [name] )` - Register a new route with optional **target** terminators and a name
* `get( pattern, [target], [name] )` - Register a new route with optional **target** terminators, a name and a GET http verb restriction
* `post( pattern, [target], [name] )` - Register a new route with optional **target** terminators, a name and a POST http verb restriction
* `put( pattern, [target], [name] )` - Register a new route with optional **target** terminators, a name and a PUT http verb restriction
* `patch( pattern, [target], [name] )` - Register a new route with optional **target** terminators, a name and a PATCH http verb restriction
* `options( pattern, [target], [name] )` - Register a new route with optional **target** terminators, a name and a OPTIONS http verb restriction
* `group( struct options, body )` - Group routes together with options that will be applied to all routes declared in the `body` closure/lambda.

#### Modifiers

Modifiers will tell the routing service about certain restrictions, conditions or locations for the routing process. It will not register the route just yet.

* `header( name, value, overwrite=true )` - attach a response header if the route matches
* `headers( map, overwrite=true )` - attach multiple response headers if the route matches
* `as( name )` - Register the route as a named route
* `rc( name, value, overwrite=true )` - Add an `RC` value if the route matched
* `rcAppend map, overwrite=true )` - Add multiple values to the `RC` collection if the route matched
* `prc( name, value, overwrite=true )` - Add an `PRC` value if the route matched
* `prcAppend map, overwrite=true )` - Add multiple values to the `PRC` collection if the route matched
* `withHandler( handler )` - Map the route to execute a handler
* `withAction( action )` - Map the route to execute a single action or a struct that represents verbs and actions
* `withModule( module )` - Map the route to a module
* `withNamespace( namespace )` - Map the route to a namespace
* `withSSL()` - Force SSL
* `withCondition( condition )` - Apply a runtime closure/lambda enclosure
* `withDomain( domain )` - Map the route to a domain or subdomain
* `withVerbs( verbs )` - Restrict the route to listen to only these HTTP Verbs
* `packageResolver( toggle )` - Turn on/off convention for packages
* `valuePairTranslator( toggle )` - Turn on/off automatic name value pair translations

#### Terminators

Terminators finalize the routing process by registering the route in the Router.

* `end()` - Register the route as it exists
* `toAction( action )` - Send the route to a specific action or RESTFul action struct
* `toView( view, layout, noLayout=false, viewModule, layoutModule )` - Send the route to a view/layout 
* `toRedirect( target, statusCode=301 )` - Relocate the route to another event
* `to( event )` - Execute the event if the route matches
* `toHandler( handler )` - Execute the handler if the route matches
* `toResponse( body, statusCode=200, statusText="ok" )` - Inline response action
* `toModuleRouting( module )` - Send to the module router for evaluation
* `toNamespaceRouting( namespace )` - Send to the namespace router for evaluation

## Routing By Convention

Every router has a **default route** already defined for you in the application templates, which we refer to as routing by convention:

```javascript
route( ":handler/:action?").end();
```

The URL pattern in the default route includes two special position **placeholders,** meaning that the handler and the action will come from the URL. Also note that the `:action` has a question mark \(`?`\), which makes the placeholder optional, meaning it can exist or not from the incoming URL.

* `:handler` - The handler to execute \(It can include a Package and/or Module reference\)
* `:action` - The action to relocate to \(See the `?`, this means that the action is **optional**\)

{% hint style="info" %}
Behind the scenes the router creates two routes due to the optional placeholder in the following order:

1. route\( "/:handler/:action" \)
2. route\( "/:handler\)
{% endhint %}

{% hint style="success" %}
**Tip** The `:handler` parameter allows you to nest module names and handler names. Ex: `/module/handler/action`

If no action is passed the default action is `index`
{% endhint %}

This route can handle pretty much all your needs by convention:

```javascript
// Basic Routing
http://localhost/general -> event=general.index
http://localhost/general/index -> event=general.index

// If 'admin' is a package/folder in the handlers directory
http://localhost/admin/general/index -> event=admin.general.index 

// If 'admin' is a module
http://localhost/admin/general/index -> event=admin:general.index
```

### Convention Name-Value Pairs

Any extra name-value pairs in the remaining URL of a discovered URL pattern will be translated to variables in the request collection \(`rc`\) for you automagically.

```text
http://localhost/general/show/page/2/name/luis
# translate to
event=general.show, rc.page=2, rc.name=luis

http://localhost/users/show/export/pdf/name
# translate to
event=users.show, rc.export=pdf, rc.name={empty value}
```

{% hint style="success" %}
**Tip:** You can turn this feature off by using the `valuePairTranslator( false )` modifier in the routing DSL on a route by route basis

`route( "/pattern" ).to( "users.show" ).valuePairTranslator( false );`
{% endhint %}

## Pattern Placeholders

### Alphanumeric Placeholders

In your URL pattern you can also use the `:` syntax to denote a **variable placeholder**. These position holders are **alphanumeric** by default:

```javascript
route( "blog/:year/:month?/:day?", "blog.index" );
```

Once a URL is matched to the route above, the placeholders \(`:year/:month?/:day?`\) will become request collection \(`RC`\) variables:

```text
http://localhost/blog/2012/12/22 -> rc.year=2012, rc.month=12, rc.day=22
http://localhost/blog/2012/12-> rc.year=2012, rc.month=12
http://localhost/blog/2012-> rc.year=2012
```

### Optional Placeholders

Sometimes we will want to declare routes that are very similar in nature and since order matters, they need to be delcared in the right order. Like this one:

```java
route( "/blog/:year-numeric/:month-numeric/:day-numeric" );
route( "/blog/:year-numeric/:month-numeric" );
route( "/blog/:year-numeric/" );
route( "/blog/" );
```

However, we just wrote 4 routes for this approach when we can just use optional variables by using the `?` symbol at the end of the placeholder. This tells the processor to create the routes for you in the most detailed manner first instead of you doing it manually.

```java
route( "/blog/:year-numeric?/:month-numeric?/:day-numeric?" );
```

{% hint style="danger" %}
**Caution** Just remember that an optional placeholder cannot be followed by a non-optional one. It doesn't make sense.
{% endhint %}

### Numeric Placeholders

ColdBox gives you also the ability to declare numerical only routes by appending `-numeric` to the variable placeholder so the route will only match if the placeholder is **numeric**. Let's modify the route from above.

```javascript
route( "blog/:year-numeric/:month-numeric?/:day-numeric?", "blog.index" );
```

This route will only accept years, months and days as numbers.

### Alpha Placeholders

ColdBox gives you also the ability to declare alpha only routes by appending `-alpha` to the variable placeholder so the route will only match if the placeholder is `alpha` only.

```javascript
route( "wiki/:page-alpha", "wiki.show" );
```

This route will only accept page names that are alpha only.

There are two ways to place a regex constraint on a placeholder, using the `-regex:` placeholder or adding a `constraints` structure to the route declaration.

### Regular Expression Placeholders

You can also have the ability to declare a placeholder that must match a regular expression by using the `-regex( {regex_here} )` placeholder.

```javascript
// route with regex placeholders
route(
    pattern="/api/:format-regex:(xml|json)/",
    target="api.execute"
);
```

The `rc` variable `format` must match the regex supplied: `(xml|json)`

### Regular Expression Constraints

You can also apply a structure of regular expressions to a route instead of inlining the regular expressions in the placeholder location. You will do this using the `constraints()` method of the router.

The **key** in the structure must match the **name** of the placeholder and the **value** is a regex expression that must be enclosed by parenthesis `()`.

```javascript
// route with custom constraints
route(
    pattern = "/api/:format/:entryID",
    target  = "api.execute"
).constraints( {
    format  = "(xml|json)",
    entryID = "([0-9]{4})" 
} );
```

## Routing Methods

Apart from routing by convention, you can also register your own expressive routes. Let's investigate the routing approaches.

### Inline Terminator

The `route()` method allows you to register a **pattern** and immediately assign it to execute an event or a response via the **target** argument.

```java
route( "/wiki:pagename", "wiki.page" );
route( 
    pattern="/users/:id/profile", 
    target="users:profile.show", 
    name="userprofile" 
);
```

The first pattern registers and if matched it will execute the **wiki.page** event. The second pattern if matched it will execute the **profile.show** event from the **users** module and register the route with the **userprofile** name.

#### Inline Responses

You can also pass in a closure or lambda to the target argument and it will be treated as an inline action:

```java
route(
    pattern="/echo",
    target=function( event, rc, prc ){
        return "hello ColdBox!";
    }
);

route(
    pattern="/users",
    target=function( event, rc, prc ){
        return getInstance( "UserService" ).list();
    }
);
```

You can also pass just an HTML string with `{rc_var}` replacements for the routed variables place in the request collection

```javascript
route(
    "/echo/:name",
    "<h1>Hello {name} how are you today!</h1>"
);
```

### Routing `to` Events

If you will not use the inline terminators you can do a full expressive route definition to events using the `to()` method, which allows you to concatenate the route pattern with modifiers:

```java
route( "/wiki/:pagename" )
    .to( "wiki.show" );

route( "/users/:id/profile" )
    .as( "userProfile" )
    .to( "users:profile.show" );

route( "/users/:id/profile" )
    .as( "userProfile" )
    .withVerbs( "GET" )
    .withSSL()
    .header( "cache", false )
    .prc( "isActive", true )
    .to( "users:profile.show" );
```

### Routing To Handler Action Combinations

You can also route to a **handler** and an **action** using the modifiers instead of the `to()` method. This long-form is usually done for visibility or dynamic writing of routes. You can use the following methods:

* `withHandler()`
* `withAction()`
* `toHandler()`
* `end()`

```java
route( "wiki/:pagename" )
    as( "wikipage" )
    withAction( "show" )
    toHandler( "wiki" );

route( "wiki/:pagename" )
    .withHander( "wiki" )
    .withAction( "show" )
    .end();
```

### Routing to Views

You can also route to views and view/layout combinations by using the `toView()` terminator:

```java
route( "/contact-us" )
    .toView( 
        view = "view name",
        layout = "layout",
        nolayout = false,
        viewModule = "moduleName",
        layoutModule = "moduleName"
    );
```

### Routing to Redirects

You can also use the `toRedirect()` method to re-route patterns to other patterns.

```java
route( "/my-old/link" )
    .toRedirect( target="/new/pattern", statusCode=301 );
```

{% hint style="info" %}
The default status code for redirects are 301 redirects which are PERMANENT redirects.
{% endhint %}

### Routing to Handlers

You can also redirect a pattern to a handler using the `toHandler()` method. This is usually done if you have the action coming in via the URL or you are using RESTFul actions.

```java
// Action comes via the URL
route( "/users/:action" )
    .toHandler( "users" );
```

### Routing to RESTFul Actions

You can also route a pattern to HTTP RESTFul actions. This means that you can split the routing pattern according to incoming HTTP Verb. You will use a modifier `withAction()` and then assign it to a handler via the `toHandler()` method.

```java
// RESTFul actions
route( "/users/:id?" )
    .withAction( {
        GET : "index",
        POST : "save",
        PUT : "update",
        DELETE : "remove"
    } )
    .toHandler( "users" );
```

### Routing to Responses

The Router allows you to create inline responses via closures/lambdas or enhanced strings to incoming URL patterns. You do not need to create handler/actions, you can put the actions inline as responses.

```javascript
/**
 * Setup a response for a URL pattern
 * @body A closure/lambda or enhanced HTML string
 * @statusCode The status code to send
 * @statusText The status code to send
 */
function toResponse( 
    required body, 
    numeric statusCode = 200, 
    statusText = "Ok" 
)
```

If you use a response closure/lambda, they each accepts three arguments:

1. `event` - An object that models and is used to work with the current request \(Request Context\)
2. `rc` - A struct that contains both `URL/FORM` variables merged together \(unsafe data\)
3. `prc` - A secondary struct that is **private** only settable from within your application \(safe data\)

```java
// Simple response routing
route( "/users/hello", function( event, rc, prc ){
    return "<h1>Hello From RESTLand</h1>";
} );

// Simple response routing with placeholders
route( "/users/:username", function( event, rc, prc ){
    "<h1>Hello #encodeForHTML( rc.username )# From RESTLand</h1>";
} );

// Routing with the toResponse() method
route( "/users/:id" )
    .toResponse( function( event, rc, prc ){
        var oUser = getInstance( "UserService" ).get( rc.id ?: 0 );
        if( oUser.isLoaded() ){
            return oUser.getMemento();
        }
        event.setHTTPHeader( statusCode = 400, statusText = "Invalid User ID provided" );
        return {
            "error" : true,
            "messages" : "Invalid User ID Provided"
        };
    } );
```

If the response is an HTML string, then you can do `{rc_var}` replacements on the strings as well:

```javascript
// Routing with enhanced HTML strings
route( "/users/:id" )
    .toResponse(
        "<h1>Welcome back user: {id} how are you today!</h1>"
    );
```

### Sub-Domain Routing

You can also register routes that will respond to sub-domains and even capture portions of the sub-domain for multi-tenant applications or SaaS applications. You will do this using the `withDomain()` method.

```javascript
route( "/" )
  .withDomain( "subdomain-routing.dev" )
  .to( "subdomain.index" );

route( "/" )
  .withDomain( ":username.forgebox.dev" )
  .to( "subdomain.show" );
```

You can leverage the full routing DSL as long as you add the `withDomain()` call with the domain you want to bind the route to. Also note that the domain string can contain **placeholders** which will be translated to `RC` variables for you if matched.

{% hint style="success" %}
**Tip:** Please note that you can leverage [Routing Groups](routing-groups.md) as well for domains
{% endhint %}

### Adding Variables to RC/PRC

You can also add variables to the RC and PRC structs on a per-route basis by leveraging the following methods:

* `rc( name, value, overwrite=true )` - Add an `RC` value if the route matched
* `rcAppend map, overwrite=true )` - Add multiple values to the `RC` collection if the route matched
* `prc( name, value, overwrite=true )` - Add an `PRC` value if the route matched
* `prcAppend map, overwrite=true )` - Add multiple values to the `PRC` collection if the route matched

This is a great way to manually set variables in the incoming structures:

```java
route( "/api/v1/users/:id" )
    .rcAppend( { secured : true } )
    .prcAppend( { name : "hello" } )
    .to( "api-v1:users.show" );
```

### Routing Conditions

You can also apply runtime conditions to a route in order for it to be matched. This means that if the route matches the URL pattern then we will execute a closure/lambda to make sure that it meets the runtime conditions. We will do this with the `withCondition(`\) method.

Let's say you only want to fire some routes if they are using Firefox, or a user is logged in, or whatever.

```javascript
route( "/go/firefox" )
  withCondition( function( requestString ){
    return ( findnocase( "Firefox", cgi.HTTP_USER_AGENT ) ? true : false );
  });
  .to( "firefox.index" );
```

## Resourceful Routes

In ColdBox, you can register resourceful routes to provide automatic mappings between HTTP verbs and URLs to event handlers and actions by convention. By convention, all resources map to a handler with the same name or they can be customized if needed. This allows for a standardized convention when building routed applications.

You can leverage the `resources()` method in your router to register resourceful routes.

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

// Resources in a ModuleConfig.cfc
router.resources( "photos" )
  .resources( resource="users", handler="user" )
```

This single resource declaration will create all the necessary variations of URL patterns and HTTP Verbs to actions to handle the resource. Please see the table below with all the permutations it will create for you.

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
* - `/photos/:id` : `PUT/PATCH` -> `photos.update` Update a specific photo
* - `/photos/:id` : `DELETE` -> `photos.delete` Delete a specific photo
* 
* @resource         The name of a single resource or a list of resources or an array of resources
* @handler          The handler for the route. Defaults to the resource name.
* @parameterName    The name of the id/parameter for the resource. Defaults to `id`.
* @only             Limit routes created with only this list or array of actions, e.g. "index,show"
* @except           Exclude routes with an except list or array of actions, e.g. "show"
* @module           If passed, the module these resources will be attached to.
* @namespace        If passed, the namespace these resources will be attached to.
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

### Scaffolding Resources

We have created a scaffolding command in CommandBox to help you register and generate resourceful routes. Just run the following command in CommandBox to get all the help you will need in generating resources:

```bash
coldbox create resource help
```

## Named Routes

You can register routes in ColdBox with a human friendly name so you can reference them later for link generation and more.

#### Registering Named Routes

You will do this in two forms:

1. Using the `route()` method and the `name` argument
2. Using the `as()` method

```java
// Using the name argument
route( 
    pattern = "/users/list", 
    target = "users.index", 
    name = "usermanager" 
);

route( 
    pattern = "/user/:id/profile", 
    target = "users.show", 
    name = "userprofile"
);

// Using the as() method
route( "/users/:id/profile" )
  .as( "usersprofile" )
  .to( "users.show" )
```

#### Generating URLs to Named Routes

You will generate URLs to named routes by leveraging the `route()` method in the request context object \(**event**\).

```javascript
route(
    // The name of the route
    required name,
    // The params to pass, can be a struct or array
    struct params={},
    // Force or un-force SSL, by default we keep the same protocol of the request
    boolean ssl
);
```

Let's say you register the following named routes:

```java
route( 
    pattern = "/users/list", 
    target = "users.index", 
    name = "usermanager" 
);

route( 
    pattern = "/user/:id/profile", 
    target = "users.show", 
    name = "userprofile"
);
```

Then we can create routing URLs to them easily with the `event.route()` method:

```markup
<!-- Named Route with no params -->
<a href="#event.route( 'usermanager' )#">Manage Users</a>

<!-- Named Route with struct params -->
<a href="#event.route( 'userprofile', { id = 3 } )#">View User</a>

<!-- Named Route with array params -->
<a href="#event.route( 'userprofile', [ 3 ] )#">View User</a>
```

#### **Inspecting The Current Route**

The request context object \(**event**\) also has some handy methods to tell you the name or even the current route that was selected for execution:

* `getCurrentRouteName()` - Gives you the name of the current route, if any
* `getCurrentRoute()` - Gives you the currently executed route
* `getCurrentRoutedURL()` - Gives you the complete routed URL pattern that matched the route
* `getCurrentRoutedNamespace()` - Gives you the current routed namespace, if any

## Routing Groups

There will be a time where your routes will become very verbose and you would like to group them into logical declarations. These groupings can also help you **prefixes** repetitive patterns in many routes with a single declarative construct. These needs are met with the `group()` method in the router.

```javascript
function group( struct options={}, body );
```

The best way to see how it works is by example:

```javascript
route( pattern="/news", target="public.news.index" );
route( pattern="/news/recent", target="public.news.recent" );
route( pattern="/news/removed", target="public.news.removed" );
route( pattern="/news/add/:title", target="public.news.add" );
route( pattern="/news/delete/:slug", target="public.news.remove" );
route( pattern="/news/v/:slug", target="public.news.view" );
```

As you can see from the routes above, we have lots of repetitive code that we can clean out. So let's look at the same routes but using some nice grouping action.

```javascript
group( { pattern="/news", target="public.news." }, function(){
    route( "/", "index" )
    .route( "/recent", "recent" )
    .route( "/removed", "removed" )
    .route( "/add/:title", "add" )
    .route( "/delete/:slug", "remove" )
    .route( "/v/:slug", "view" );
} );
```

The **options** struct can contain any values that you can use within the closure. Grouping can also be very nice when creating [namespaces](routing-namespaces.md), which is our next section.

## Routing Namespaces

You can create a-la-carte namespaces for URL routes. **Namespaces** are cool groupings of routes according to a specific URL entry point. So you can say that all URLs that start with `/testing` will be found in the **testing** namespace and it will iterate through the namespace routes until it matches one of them.

Much how modules work, where you have a module entry point, you can create virtual entry point to **ANY** route by namespacing it. This route can be a module a non-module, package, or whatever you like. You start off by registering the namespace using the `addNamespace( pattern, namespace )` method or the fluent `route().toNamespaceRouting()` method.

```javascript
addNamespace( pattern="/testing", namespace="test" );
route( "/testing" ).toNamespaceRouting( "test" );

addNamespace( pattern="/news", namespace="blog" );
route( "/news" ).toNamespaceRouting( "blog" );
```

Once you declare the namespace you can use the grouping functionality to declare all the namespace routes or you can use a `route().withNamespace()` combination.

```javascript
// Via Grouping
route( "/news" ).toNamespaceRouting( "blog" )
    .group( { namespace = "blog" }, function(){
        route( "/", "blog.index" )
          .route( "/:year-numeric?/:month-numeric?/:day-numeric?", "blog.archives" );
    } );


// Via Routing DSL
addNamespace( "/news", "blog" );

route( "/" )
  .withNameSpace( "blog" )
  .to( "blog.index" );

route( "/:year-numeric?/:month-numeric?/:day-numeric?" )
  .withNameSpace( "blog" )
  .to( "blog.archives" );
```

{% hint style="info" %}
**Hint** You can also register multiple URL patterns that point to the same namespace
{% endhint %}

## Building Routable Links

In your views, layouts and handlers you can use the `buildLink` method provided by the request context object \(**event**\) to build routable links in your application.

```javascript
buildLink(
    // Where to link to
    any to, 
    // Translate periods to slashes
    [boolean translate='true'], 
    // Force or un-force SSL, by default we keep the same protocol of the request
    [boolean ssl], 
    // Update the baseURL, usually used for testing
    [any baseURL=''], 
    // Append a query string to the URL, as convention name value-pairs
    [any queryString='']
)
```

Just pass in the routed URL or event and it will create the appropriate routed URL for you:

```javascript
<a href="#event.buildLink( 'home.about' )#">About</a>
<a href="#event.buildLink( 'user.edit.id.#user.getID()#' )#">Edit User</a>
```

#### **Inspecting The Current Route**

The request context object \(**event**\) also has some handy methods to tell you the name or even the current route that was selected for execution:

* `getCurrentRouteName()` - Gives you the name of the current route, if any
* `getCurrentRoute()` - Gives you the currently executed route
* `getCurrentRoutedURL()` - Gives you the complete routed URL pattern that matched the route
* `getCurrentRoutedNamespace()` - Gives you the current routed namespace, if any

## RESTFul Extension Detection

### Usage

ColdBox allows you to detect incoming extensions from incoming paths automatically for you. This is great for building multi-type responses or to just create virtual extensions for events.

```text
http://localhost/users
http://localhost/users.json
http://localhost/users.xml
http://localhost/users.pdf
http://localhost/users.html
```

If an extension is detected in the incoming URL, ColdBox will grab it and store it in the request collection \(`RC`\) as the variable `format`. If there is no extension, then `rc.format` will not be stored and thus will not exist.

```text
http://localhost/users => rc.format is null, does not exist
http://localhost/users.json => rc.format = json
http://localhost/users.xml => rc.format = xml
http://localhost/users.pdf => rc.format = pdf
http://localhost/users.html => rc.format = html
```

### Configuration

You can configure the extension detection using the following configuration methods:

| **Method** | **Description** |
| :--- | :--- |
| `setExtensionDetection( boolean )` | By default ColdBox detects URL extensions like `json, xml, html, pdf` which can allow you to build awesome RESTful web services. Default is **true**. |
| `setValidExtensions( list )` | Tell the interceptor what valid extensions your application can listen to. By default it listens to: `json, jsont, xml, cfm, cfml, html, htm, rss, pdf` |
| `setThrowOnInvalidExtensions( boolean )` | By default ColdBox does not throw an exception when an invalid extension is detected. If **true**, then the interceptor will throw a 406 Invalid Requested Format Extension: {extension} exception. Default is **false**. |

{% hint style="warning" %}
Please note that if you have set to throw exceptions if an invalid extension is detected then a 406 exception will be thrown.
{% endhint %}

## HTTP Method Spoofing

Although we have access to all the HTTP verbs, modern browsers still only support **GET** and **POST**. With ColdBox and HTTP Method Spoofing, you can take advantage of **all** the HTTP verbs in your web forms.

By convention, ColdBox will look for an `_method` field in the **FORM** scope. If one exists, the value of this field is used as the HTTP method instead of the method from the execution. For instance, the following block of code would execute with the **DELETE** action instead of the **POST** action:

```markup
<cfoutput>
<form method="POST" action="#event.buildLink( 'posts/#prc.post.getId()#' )#">
    <input type="hidden" name="_method" value="DELETE" />
    <button type="submit">Delete</button>
</form>
</cfoutput>
```

You can manually add these `_method` fields yourselves, or you can take advantage of ColdBox's HTML Helper `startForm()` method. Just pass the method you like, we will take care of the rest:

```markup
<cfoutput>
#html.startForm( action = "posts.#prc.post.getId()#", method="DELETE" )#
    #html.submitButton( name = "Delete", class = "btn btn-danger" )#
#html.endForm()#
</cfoutput>
```

## HTML Base Tag

The `base` tag in HTML allows you to tell the browser what is the base URL for assets in your application. This is something that is always missed when using frameworks that enable routing.

{% hint style="info" %}
`base` tag defines the base location for links on a page. Relative links within a document \(such as &lt;a href="someplace.html"... or &lt;img src="someimage.jpg"... \) will become relative to the URI specified in the base tag. The base tag must go inside the head element.
{% endhint %}

We definitely recommend using this HTML tag reference as it will simplify your asset retrievals.

```javascript
<base href="#event.getHTMLBaseURL()#">
```

{% hint style="danger" %}
**Caution** If you do not use this tag, then every asset reference must be an absolute URL reference.
{% endhint %}

## Pathinfo Providers

By default, the URL mapping processor will detect routes by looking at the `CGI.PATH_INFO` variable, but you can override this and provide your own function. This feature can be useful to set flags for each request based on a URL and then clean or parse the URL to a more generic form to allow for simple route declarations. Uses may include internationalization \(i18n\) and supporting multiple experiences based on devices such as Desktop, Tablet, Mobile and TV.

To modify the URI used by the Routing Services before route detection occurs simply follow the convention of adding a function called `pathInfoProvider()` to your application Router \(`config/Router.cfc`\).

The `pathInfoProvider()` function is responsible for returning the string used to match a route.

```javascript
// Example PathInfoProvider for detecting a mobile request
function PathInfoProvider( event ){
  var rc = event.getCollection();
  var prc = event.getCollection(private=true);

  local.URI = CGI.PATH_INFO;

  if (reFindNoCase('^/m',local.URI) == 0)
  {
    // Does not look like this could be a mobile request...
    return local.URI;
  }

  // Mobile Request? Let's find out.

  // If the URI is "/m" it is easy to determine that this is a
  // request for the Mobile Homepage.
  if (len(local.URI) == 2)
  {
    prc.mobile = true;
    // Simply return "/" since they want the mobile homepage
    return "/";
  }

  // Only continue with our mobile evaluation if we have a slash after
  // our "/m". Without a slash following the /m the route is something
  // else like coldbox.org/makes/cool/stuff
  if (REFindNoCase('^/m/',local.URI) == 1)
  {
    // Looks like we are mobile!
    prc.mobile = true;

    // Remove our "/m/" determination and continue
    // processing for languages...
    local.URI = REReplaceNoCase(local.URI,'^/m/','/');
  }

  // The URI starts with an "m" but does not look like
  // a mobile request. So, simply return the URI for normal
  // route detection...
  return local.URI;
}
```

## Event Handlers

Event handlers are ColdBox's version of **controllers** in the MVC design pattern. So every time you hear "_event handler_", you are talking about a controller that can listen to external events or internal events in ColdBox. Event handlers are responsible for controlling your application flow, calling business logic, preparing a display to a user and much more.

### Locations

All your handlers will be stored in the **handlers** folder of your application template. If you get to the point where your application needs even more decoupling and separation, please consider building [ColdBox Modules](../../hmvc/modules/) instead.

{% hint style="info" %}
**Tip:** You can create packages or sub-folders inside of the **handlers** directory. This is encouraged on large applications so you can organize or package handlers logically to facilitate better maintenance and URL experience.
{% endhint %}

#### External Location

You can also declare a `HandlersExternalLocation` directive in your [Configuration CFC](../../getting-started/configuration/). This will be a dot notation path or instantiation path where more external event handlers can be found.

```javascript
coldbox.handlersExternalLocation  = "shared.myapp.handlers";
```

{% hint style="warning" %}
If an external event handler has the same name as an internal conventions event, the internal conventions event will take precedence.
{% endhint %}

#### Development Settings

By default, ColdBox will only scan for event handlers on startup. For development we highly encourage you leverage the following configuration directives:

```javascript
// Rescan handlers on each request, turn OFF in production please
coldbox.handlersIndexAutoReload = true;
// Deactivate singleton caching of the handlers, turn ON in production pleaese
coldbox.handlerCaching = false;
```

### Anatomy

Event handlers are CFCs that will respond to FORM posts, HTTP requests and/or remote requests \(like Flex,Air, SOAP, REST\) via an incoming RC variable called **event** or by [URL mappings](../routing/) \(Which we saw in the previous section\).

#### Components

{% code title="Main.cfc" %}
```javascript
component extends="coldbox.system.EventHandler"{

    /**
     * Default Action
     */
    function index( event, rc, prc ){
        prc.message = "Hello From ColdBox";
        event.setView( "main/index");
    }

    /**
     * Action returning complex data, converted to JSON automatically by ColdBox
     */
    function data( event, rc, prc ){
        var data = getInstance( "MyModel" ).getArray();
        return data; 
    }

}
```
{% endcode %}

{% hint style="info" %}
You can also remove the inheritance from the CFC and WireBox will extend the `coldbox.system.EventHandler` for you using [Virtual Inheritance](https://wirebox.ortusbooks.com/advanced-topics/virtual-inheritance).
{% endhint %}

{% hint style="info" %}
Event Handlers are treated as singletons by ColdBox, so make sure you make them thread-safe and properly scoped. Persistence is controlled by the `coldbox.handlerCaching` [directive](../../getting-started/configuration/coldbox.cfc/configuration-directives/)
{% endhint %}

#### Actions

They are composed of functions which are called **actions** that will always have the following signature:

```javascript
function name( event, rc, prc )
```

Each action receives three arguments:

1. `event` - An object that models and is used to work with the current request
2. `rc` - A struct that contains both `URL/FORM` variables \(unsafe data\)
3. `prc` - A secondary struct that is **private**.  This structure is only accessible from within your application \(safe data\)

An action will usually do one of the following:

* Set a view/layout to render
* Return HTML
* Return Complex Data which is converted to JSON by default
* Relocate to another event/URL Route

The **default action** for all event handlers is called `index()`. This means that when you execute an event, you can omit the index if you so desire.

```javascript
component extends="coldbox.system.EventHandler"{

    function index( event, rc, prc ){
        return "<h1> Hi from handler land!</h1>";
    }

    function save( event, rc, prc ){
        getInstance( "MyService" ).save( rc );
        relocate( "users/list" );
    }

    function myData( event, rc, prc ){
        return ['coldbox', 'wirebox', 'cachebox', 'logbox'];
    }
}
```

**Private Actions**

So what about `private` functions? Private functions are not executable from the outside world, but can be executed internally via a function available to all handlers called `runEvent()`, which we will explore later.

#### Composed Properties

It is important to note that there is a pre-defined object model behind every event handler controller that will enable you to do your work more efficiently. The following are the automatically generated properties every event handler makes available in their `variables` scope:\)

* **cachebox** : A reference to the [CacheBox ](https://cachebox.ortusbooks.com)library \(`coldbox.system.cache.CacheFactory`\)
* **controller** : A reference to the Application Controller \(`coldbox.system.web.Controller`\)
* **flash**: A flash memory object \(`coldbox.system.web.flash.AbstractFlashScope`\)
* **logbox**: A reference to the application [LogBox ](https://logbox.ortusbooks.com)\(`coldbox.system.logging.LogBox`\)
* **log**: A pre-configured logging[ logger object](https://logbox.ortusbooks.com/usage/using-a-logger-object) \(`coldbox.system.logging.Logger`\)
* **wirebox** : A reference to the application [WireBox Injector ](https://wirebox.ortusbooks.com)\(`coldbox.system.ioc.Injector`\)
* **$super**: A reference to the virtual super class if using non-inheritance approach.

## How are events called?

Events are determined via a special variable that can be sent in via the FORM, URL, or REMOTELY called `event`. If no event is detected as an incoming variable, the framework will look in the configuration directives for the `DefaultEvent` and use that instead. If you did not set a `DefaultEvent` setting then the framework will use the following convention for you: `main.index`

{% hint style="success" %}
**Hint** : You can even change the `event` variable name by updating the `EventName` setting in your `coldbox` [configuration directive](../../getting-started/configuration/coldbox.cfc/configuration-directives/).
{% endhint %}

Please note that ColdBox supports both normal variable routing and [URL mapping routing](../routing/), usually referred to as pretty URLs.

### Event Syntax

In order to call them you will use the following event syntax notation format:

```javascript
event={module:}{package.}{handler}{.action}
```

* **no event** : Default event by convention is `main.index`
* **event={handler}** : Default action method by convention is `index()`
* **event={handler}.{action}** : Explicit handler + action method
* **event={package}.{handler}.{action}** : Packaged notation
* **event={module}:{package}.{handler}.{action}** : Module Notation \(See [ColdBox Modules](../../hmvc/modules/)\)

This looks very similar to a Java or CFC method call, example: `String.getLength(),` but without the parentheses. Once the event variable is set and detected by the framework, the framework will tokenize the event string to retrieve the CFC and action call to validate it against the internal registry of registered events. It then continues to instantiate the event handler CFC or retrieve it from cache, finally executing the event handler's action method.

**Examples**

```text
// Call the users.cfc index() method
index.cfm?event=users.index
// Call the users.cfc index() method implicitly
index.cfm?event=users
// Call the users.cfc index() method via URL mappings
index.cfm/users/index
// Call the users.cfc index() method implicitly via URL mappings
index.cfm/users
```

## Getting & Setting Values

We all need values in our applications. That is why we interact with the [request context](../request-context.md) in order to place data from our model layer into it so our views can display it, or to retrieve data from a user's request. You will either interact with the event object to get/set values or put/read values directly via the received `rc` and `prc` references.

{% hint style="warning" %}
We would recommend you use the private request collection \(`prc`\) for setting manual data and using the standard request collection \(`rc`\) for reading the user's unsafe request variables. This way a clear distinction can be made on what was sent from the user and what was set by your code.
{% endhint %}

```javascript
//set a value for views to use
event.setValue( "name", "Luis" );
event.setPrivateValue( "name", "Luis" );

// retrieve a value the user sent
event.getValue( "name" );
// retrieve a value the user sent or give me a default value
event.getValue( "isChecked", false );

// retrieve a private value
event.getPrivateValue( "name" );
// retrieve a private value or give me a default value
event.getPrivateValue( "isChecked", false );

// param a value
event.paramValue( "user_id", "" );
// param a private value
event.paramPrivateValue( "user_id", "" );

// remove a value
event.removeValue( "name" );
// remove a private value
event.removePrivateValue( "name" );

//check if value exists
if( event.valueExists( "name" ) ){

}
//check if private value exists
if( event.privateValueExists( "name" ) ){

}

// set a view for rendering
event.setView( 'blog/index' );

// set a layout for rendering
event.setLayout( 'main' );

// set a view and layout
event.setView( view="blog/userinfo", layout="ajax" );
```

{% hint style="danger" %}
**Important** The most important paradigm shift from procedural to an MVC framework is that you NO LONGER will be talking to URL, FORM, REQUEST or any ColdFusion scope from within your handlers, layouts, and views. The request collection already has URL, FORM, and REQUEST scope capabilities, so leverage it.
{% endhint %}

## Setting Views

### Views \(Default Layout\)

The `event` object is the object that will let you set the views that you want to render, so please explore its API in the CFC Docs. To quickly set a view to render, do the following:

```javascript
event.setView( 'view' );
```

The view name is the name of the template in the **views** directory without appending the **.cfm**. If the view is inside another directory, you would do this:

```javascript
event.setView( 'mydirectory/myView' );
```

The views you set will use the default layout defined in your configuration file which by default is the `layouts/Main.cfm`

{% hint style="info" %}
We recommend that you set your views following the naming convention of your event. If your event is **users.index**, your view should be **users/index**. This will go a long way with maintainability and consistency and also will activate **implicit views** where you don't even have to use the set view method call.
{% endhint %}

### View With Custom Layouts

You can also use the `setView(), setLayout()` methods to tell the framework which view and layout combination to use:

```javascript
function index( event, rc, prc ){
    // Inline
    event.setView( view="main/index", layout="2columns" );

    // Concatenated
    event.setView( "main/index" )
        .setLayout( "2columns" );
}
```

### Views With No Layout

You can also tell the framework to set a view for rendering by itself with no layout using the `noLayout` argument

```javascript
function index( event, rc, prc ){
    // Inline
    event.setView( view="widgets/users", nolayout=true );   
}
```

### setView\(\) Arguments

Here are the arguments for the `setView()` method:

```javascript
* @view The name of the view to set. If a layout has been defined it will assign it, else if will assign the default layout. No extension please
* @args An optional set of arguments that will be available when the view is rendered
* @layout You can override the rendering layout of this setView() call if you want to. Else it defaults to implicit resolution or another override.
* @module The explicit module view
* @noLayout Boolean flag, wether the view sent in will be using a layout or not. Default is false. Uses a pre set layout or the default layout.
* @cache True if you want to cache the rendered view.
* @cacheTimeout The cache timeout in minutes
* @cacheLastAccessTimeout The last access timeout in minutes
* @cacheSuffix Add a cache suffix to the view cache entry. Great for multi-domain caching or i18n caching.
* @cacheProvider The cache provider you want to use for storing the rendered view. By default we use the 'template' cache provider
* @name This triggers a rendering region.  This will be the unique name in the request for specifying a rendering region, you can then render it by passing the unique name to renderView();
```

### Cached Views

You can leverage the caching arguments in the `setView()` method in order to render and cache the output of the views once the framework renders it. These cached views will be stored in the **template** cache region, which you can retrieve or purge by talking to it: `getCache( 'template' )`.

```javascript
// Cache in the template cache for the default amount of time
event.setView( view='myView', cache=true );
// Cache in the template cache for up to 60 minutes, or 20 minutes after the last time it's been used
event.setView( view='myView', cache=true, cacheTimeout=60, cacheLastAccessTimeout=20 );
// Cache a different version of the view for each language the site has
event.setView( view='myView', cache=true, cacheSuffix=prc.language );
```

### View Arguments

Data can be passed from your handler to the view via **rc** or **prc**. If you want to pass data to a view without polluting **rc** and **prc**, you can pass it directly via the **args** parameter, much like a method call.

```javascript
var viewData = {
  data1 = service.getData1(),
  data2 = service.getData2()
};

event.setView( view='myView', args=viewData );
```

Access the data in the view like so:

```markup
<cfoutput>
  Data 1: #args.data1#<br>
  Data 2: #args.data2#
</cfoutput>
```

### No Rendering

If you don't want to, you don't have to. The framework gives you a method in the event object that you can use if this specific request should just terminate gracefully and not render anything at all. All you need to do is use the event object to call on the `noRender()` method and it will present to the user a lovely white page of death.

```text
event.noRender();
```

## Relocating

The framework provides you with the `relocate()` method that you can use to relocate to other events thanks to the framework super type object, the grand daddy of all things ColdBox.

{% hint style="info" %}
Please see the [Super Type CFC Docs](http://apidocs.ortussolutions.com/coldbox/current) for further investigation of all the goodness of methods you have available.
{% endhint %}

```javascript
/**
* Relocate user browser requests to other events, URLs, or URIs.
*
* @event The name of the event to run, if not passed, then it will use the default event found in your configuration file
* @URL The full URL you would like to relocate to instead of an event: ex: URL='http://www.google.com'
* @URI The relative URI you would like to relocate to instead of an event: ex: URI='/mypath/awesome/here'
* @queryString The query string to append, if needed. If in SES mode it will be translated to convention name value pairs
* @persist What request collection keys to persist in flash ram
* @persistStruct A structure key-value pairs to persist in flash ram
* @addToken Wether to add the tokens or not. Default is false
* @ssl Whether to relocate in SSL or not
* @baseURL Use this baseURL instead of the index.cfm that is used by default. You can use this for ssl or any full base url you would like to use. Ex: https://mysite.com/index.cfm
* @postProcessExempt Do not fire the postProcess interceptors
* @statusCode The status code to use in the relocation
*/
void function relocate(
    event,
    URL,
    URI,
    queryString,
    persist,
    struct persistStruct,
    boolean addToken,
    boolean ssl,
    baseURL,
    boolean postProcessExempt,
    numeric statusCode
)
```

{% hint style="danger" %}
It is **extremely** important that you use this method when relocating instead of the native ColdFusion methods as it allows you to gracefully relocate to other events or external URIs. By graceful, we mean it does a lot more behind the scenes like making sure the flash scope is persisted, logging, post processing interceptions can occur and safe relocations.
{% endhint %}

So always remember that you relocate via `relocate()` and if I asked you: "Where in the world does event handlers get this method from?", you need to answer: "From the super typed inheritance".

```java
relocate( "home" );
relocate( event="shop", ssl=true );
relocate( event="user.view", queryString="id=#rc.id#" );
relocate( url="http://www.google.com" );
relocate( uri="/docs/index.html" )
```

## Rendering Data

Handler actions can return data back to its callers in different formats

* Complex Data
* HTML
* Rendered Data via `event.renderData()`

### Complex Data

By default, any complex data returned from handler actions will automatically be marshaled to **JSON** by default:

```javascript
function showData( event, rc, prc ){
    prc.data = service.getUsers();
    return prc.data;
}
```

Simple as that. ColdBox detects the complex object and tries to convert it to **JSON** for you automatically.

#### `renderdata` Action Annotation

If you want ColdBox to marshall the content to another type like XML or PDF. Then you can use the `renderdata` annotation on the action itself. The `renderdata` annotation can be any of the following values:

* json
* jsonp
* jsont
* xml
* html
* text
* pdf

```javascript
function usersAsXML( event, rc, prc ) renderdata=xml{
    prc.data = service.getUsers();
    return prc.data;
}

function usersAsPDF( event, rc, prc ) renderdata=pdf{
    prc.data = service.getUsers();
    return prc.data;
}
```

#### `renderdata` Component Annotation

You can also add the renderData annotation to the component definition and this will override the default of JSON. So if you want XML as the default, you can do this:

```javascript
component renderdata="xml"{

}
```

#### $renderData Convention

If the returned complex data is an **object** and it contains a function called `$renderData(),` then ColdBox will call it for you automatically. So instead of marshaling to JSON automatically, your object decides how to marshal itself.

```javascript
function showUser( event, rc, prc ){
    return service.getUser( 2 );
}
```

### Native HTML

By default if your handlers return simple values, then they will be treated as returning HTML.

```javascript
function index(event,rc,prc){
    return "<h1>Hello from my handler today at :#now()#</h1>";
}

function myData( event, rc, prc ){
     prc.mydata = myservice.getData();
     return renderView( "main/myData" );
}
```

### event.renderData\(\)

Using the `renderdata()` method of the **event** object is the most flexible for RESTFul web services or pure data marshaling. Out of the box ColdBox can marshall data \(structs, queries, arrays, complex or even ORM entities\) into the following output formats:

* XML
* JSON
* JSONP
* JSONT
* HTML
* TEXT
* PDF
* WDDX
* CUSTOM

Here is the method signature:

```javascript
/**
* Use this method to tell the framework to render data for you. The framework will take care of marshalling the data for you
* @type The type of data to render. Valid types are JSON, JSONP, JSONT, XML, WDDX, PLAIN/HTML, TEXT, PDF. The deafult is HTML or PLAIN. If an invalid type is sent in, this method will throw an error
* @data The data you would like to marshall and return by the framework
* @contentType The content type of the data. This will be used in the cfcontent tag: text/html, text/plain, text/xml, text/json, etc. The default value is text/html. However, if you choose JSON this method will choose application/json, if you choose WDDX or XML this method will choose text/xml for you.
* @encoding The default character encoding to use.  The default encoding is utf-8
* @statusCode The HTTP status code to send to the browser. Defaults to 200
* @statusText Explains the HTTP status code sent to the browser.
* @location Optional argument used to set the HTTP Location header
* @jsonCallback Only needed when using JSONP, this is the callback to add to the JSON packet
* @jsonQueryFormat JSON Only: This parameter can be a Boolean value that specifies how to serialize ColdFusion queries or a string with possible values "row", "column", or "struct".
* @jsonAsText If set to false, defaults content mime-type to application/json, else will change encoding to plain/text
* @xmlColumnList XML Only: Choose which columns to inspect, by default it uses all the columns in the query, if using a query
* @xmlUseCDATA XML Only: Use CDATA content for ALL values. The default is false
* @xmlListDelimiter XML Only: The delimiter in the list. Comma by default
* @xmlRootName XML Only: The name of the initial root element of the XML packet
* @pdfArgs All the PDF arguments to pass along to the CFDocument tag.
* @formats The formats list or array that ColdBox should respond to using the passed in data argument. You can pass any of the valid types (JSON,JSONP,JSONT,XML,WDDX,PLAIN,HTML,TEXT,PDF). For PDF and HTML we will try to render the view by convention based on the incoming event
* @formatsView The view that should be used for rendering HTML/PLAIN/PDF. By default ColdBox uses the name of the event as an implicit view
* @formatsRedirect The arguments that should be passed to relcoate as part of a redirect for the HTML action.  If the format is HTML and this struct is not empty, ColdBox will call relcoate with these arguments.
* @isBinary Bit that determines if the data being set for rendering is binary or not.
*/
function renderData(
    type="HTML",
    required data,
    contentType="",
    encoding="utf-8",
    numeric statusCode=200,
    statusText="",
    location="",
    jsonCallback="",
     jsonQueryFormat="true",
    boolean jsonAsText=false,
    xmlColumnList="",
    boolean xmlUseCDATA=false,
    xmlListDelimiter=",",
    xmlRootName="",
    struct pdfArgs={},
    formats="",
    formatsView="",
    formatsRedirect={},
    boolean isBinary=false
){
```

Below are a few simple examples:

```javascript
// html marshalling
function renderHTML(event,rc,prc){
    event.renderData( data="<h1>My HTML</h1>" );
}
// xml marshalling
function getUsersXML(event,rc,prc){
    var qUsers = getUserService().getUsers();
    event.renderData( type="XML", data=qUsers );
}
//json marshalling
function getUsersJSON(event,rc,prc){
    var qUsers = getUserService().getUsers();
    event.renderData( type="json", data=qUsers, statusCode=403 );
}
```

As you can see, it is very easy to render data back to the browser or caller. You can even choose plain and send HTML back if you wanted to.

#### Render PDFs

You can also render out PDFs from ColdBox using the render data method. The data argument can be either the full binary of the PDF or simple values to be rendered out as a PDF; like views, layouts, strings, etc.

```javascript
// from binary
function pdf(event,rc,prc){
  var binary = fileReadAsBinary( file.path );
  event.renderData( data=binary, type="PDF" );
}

// from content
function pdf(event,rc,prc){
  event.renderData( data=renderView("views/page"), type="PDF" );
}
```

There is also a `pdfArgs` argument in the render data method that can take in a structure of name-value pairs that will be used in the `cfdocument` \([See docs](http://help.adobe.com/en_US/ColdFusion/9.0/CFMLRef/WSc3ff6d0ea77859461172e0811cbec22c24-7c21.html)\) tag when generating the PDF. This is a great way to pass in arguments to really control the way PDF's are generated uniformly.

```javascript
// from content and with pdfArgs
function pdf(event,rc,prc){
  var pdfArgs = { bookmark = "yes", backgroundVisible = "yes", orientation="landscape" };
  event.renderData(data=renderView("views/page"), type="PDF", pdfArgs=pdfArgs);
}
```

#### Renderdata With Formats

The `renderData()` method also has two powerful arguments: `formats & formatsView`. If you currently have code like this:

```javascript
event.paramValue("format", "html");

switch( rc.format ){
    case "json" : case "jsonp" : case "xml" : {
        event.renderData(data=mydata, type=rc.format);
        break;
    } 
    case "pdf" : {
        event.renderData(data=renderView("even/action"), type="pdf");
        break;
    }
    case "html" : {
        event.setView( "event/action" );
        break;
      }
};
```

Where you need to param the incoming format extension, then do a switch and do some code for marshalling data into several formats. Well, no more, you can use our formats argument and ColdBox will marshall and code all that nasty stuff for you:

```javascript
event.renderData( data=MyData, formats="xml,json,html,pdf" );
```

That's it! ColdBox will figure out how to deal with all the passed in formats for you that `renderdata` can use. By convention it will use the name of the incoming event as the view that will be rendered for HTML and PDF; implicit views. If the event was users.list, then the view would be views/users/list.cfm. However, you can tell us which view you like if it is named different:

```javascript
event.renderData( data=MyData, formats="xml,json,html,pdf", formatsView="data/MyView" );
```

If you need to redirect for html events, you can pass any arguments you normally would pass to `setNextEvent` to `formatsRedirect`.

```javascript
event.renderData( data=MyData, formats="xml,json,html,pdf", formatsRedirect={event="Main.index"} );
```

#### Custom Data Conversion

You can do custom data conversion by convention when marshalling CFCs. If you pass in a CFC as the `data` argument and that CFC has a method called `$renderdata()`, then the marshalling utility will call that function for you instead of using the internal marshalling utilities. You can pass in the custom content type for encoding as well:

```javascript
// get an instance of your custom converter
myConverter = getInstance("MyConverter")
// put some data in it
myConverter.setData( data );
// marshall it out according to your conversions and the content type it supports
event.renderData( data= myConverter, contentType=myConverter.getContentType() );
```

The CFC converter:

```javascript
component accessors="true"{

    property name="data" type="mytype";
    property name="contentType";

    function init(){ 
        setContentType("text");
        return this; 
    }

    // The magical rendering
    function $renderdata(){
        var d = {
            n = data.getName(),
            a = data.getAge(),
            c = data.getCoo(),
            today = now()
        };

        return d.toString();
    }

}
```

In this approach your `$renderdata()` function can be much more customizable than our internal serializers. Just remember to use the right contentType argument so the browser knows what to do with it.

## Sending Files

We all need to deliver files to users at one point in time. ColdBox makes it easy to deliver any type of file even binary files via the [Request Context's](../request-context.md) \(event\) `sendFile()` method.

```javascript
function report( event, rc, prc ){
    var prc.reportFile = reportService.createReport();

    event
        .sendFile(
            file = prc.reportFile,
            name = "UserReport.xls",
            deleteFile = true
        )
        .noRender();
}
```

#### Method Signature

The API Docs can help you see the entire format of the method: [https://apidocs.ortussolutions.com/coldbox/5.1.4/coldbox/system/web/context/RequestContext.html\#sendFile\(\)](https://apidocs.ortussolutions.com/coldbox/5.1.4/coldbox/system/web/context/RequestContext.html#sendFile%28%29)

The method signature is as follows:

```javascript
/**
 * This method will send a file to the browser or requested HTTP protocol according to arguments.
 * CF11+ Compatibility
 *
 * @file The absolute path to the file or a binary file to send
 * @name The name to send to the browser via content disposition header.  If not provided then the name of the file or a UUID for a binary file will be used
 * @mimeType A valid mime type to use.  If not passed, then we will try to use one according to file type
 * @disposition The browser content disposition (attachment/inline) header
 * @abortAtEnd If true, then this method will do a hard abort, we do not recommend this, prefer the event.noRender() for a graceful abort.
 * @extension Only used for binary files which types are not determined.
 * @deleteFile Delete the file after it has been streamed to the user. Only used if file is not binary.
 */
function sendFile(
    file="",
    name="",
    mimeType="",
    disposition="attachment",
    boolean abortAtEnd="false",
    extension="",
    boolean deleteFile=false
)
```

{% hint style="info" %}
Please note that the `file` argument can be an absolute path or an actual binary file to stream out.
{% endhint %}

## Interception Methods

There are several simple implicit [AOP](http://en.wikipedia.org/wiki/Aspect-oriented_programming) \(Aspect Oriented Programming\) interceptor methods, usually referred to as **advices**, that can be declared in your event handler that the framework will use in order to execute them **before/after** and **around** an event as its fired from the current handler.

This is great for intercepting calls, pre/post processing, localized security, logging, RESTful conventions, and much more. Yes, you got that right, [Aspect Oriented Programming](http://en.wikipedia.org/wiki/Aspect-oriented_programming) just for you and without all the complicated setup involved! If you declared them, the framework will execute them.

| **Interceptor Method** | **Description** |
| :--- | :--- |
| `preHandler()` | Executes **before** any requested action \(In the same handler CFC\) |
| `pre{action}()` | Executes **before** the `{action}` requested ONLY |
| `postHandler()` | Executes **after** any requested action \(In the same handler CFC\) |
| `post{action}()` | Executes **after** the `{action}` requested ONLY |
| `aroundHandler()` | Executes **around** any request action \(In the same handler CFC\) |
| `around{action}()` | Executes **around** the `{action}` requested ONLY |

## Pre Advices

With this interceptor you can intercept local event actions and execute things **before** the requested action executes. You can do it globally by using the `preHandler()` method or targeted to a specific action `pre{actionName}()`.

```javascript
// executes before any action
function preHandler( event, rc, prc, action, eventArguments ){
}

// executes before the list() action ONLY
function preList( event, rc, prc, action, eventArguments ){
}

// concrete example
function preHandler( event, rc, prc, action, eventArguments ){
    if( !security.isLoggedIn() ){
        event.overrideEvent( 'security.login' );
        log.info( "Unauthorized accessed detected!", getHTTPRequestData() );
    }
}
function preList( event, rc, prc, action, eventArguments ){
    log.info("Starting executing the list action");
}
```

The arguments received by these interceptors are:

* `event` : The request context reference
* `action` : The action name that was intercepted
* `eventArguments` : The struct of extra arguments sent to an action if executed via `runEvent()`
* `rc` : The **RC** reference
* `prc` : The **PRC** Reference

Here are a few options for altering the default event execution:

* Use `event.overrideEvent('myHandler.myAction')` to execute a different event than the default.
* Use `event.noExecution()` to halt execution of the current event

See the [RequestContext](https://apidocs.ortussolutions.com/coldbox/5.0.0/coldbox/system/web/context/RequestContext.html) documentation for more details.

### Exceptions & Only Lists

You can fine tune these interception methods by leveraging two public properties in the handler:

* `this.prehandler_only` : A list of actions that `preHandler()` will ONLY fire on
* `this.prehandler_except` : A list of actions that `preHandler()` will NOT fire on

```javascript
// only fire for the actions: save(), delete()
this.prehandler_only = "save,delete";
// DO NOT fire for the actions: login(), doLogin(), logout()
this.prehandler_except = "login,doLogin,logout"
```

## Post Advices

With this interceptor you can intercept local event actions and execute things **after** the requested action executes. You can do it globally by using the `postHandler()` method or targeted to a specific action `post{actionName}()`.

```javascript
// executes after any action
function postHandler( event, rc, prc, action, eventArguments ){
}

// executes after the list() action ONLY
function postList( event, rc, prc, action, eventArguments ){
}

// concrete examples
function postHandler( event, rc, prc, action, eventArguments ){
    log.info("Finalized executing #action#");
}
```

The arguments received by these interceptors are:

* `event` : The request context reference
* `action` : The action name that was intercepted
* `eventArguments` : The struct of extra arguments sent to an action if executed via `runEvent()`
* `rc` : The **RC** reference
* `prc` : The **PRC** Reference

### Exceptions & Only Lists

You can fine tune these interception methods by leveraging two public properties in the handler:

* `this.posthandler_only` : A list of actions that the `postHandler()` action will fire ONLY!
* `this.posthandler_except` : A list of actions that the `postHandler()` action will NOT fire on

```javascript
// only fire for the actions: save(), delete()
this.posthandler_only = "save,delete";
// DO NOT fire for the actions: login(), doLogin(), logout()
this.posthandler_except = "login,doLogin,logout"
```

## Around Advices

Around advices are the most **powerful** of all as you completely hijack the requested action with your own action that looks, smells and feels exactly as the requested action. This is usually referred to as a [proxy design pattern.](https://sourcemaking.com/design_patterns/proxy)

```text
users.list => users.aroundHandler() <=> list()
```

This will allow you to run both **before** and **after** advices but also **surround** the method call with whatever logic you want like `transactions`, `try/catch` blocks, `locks` or even decide to NOT execute the action at all.

You can do it globally by using the `aroundHandler()` method or targeted to a specific action `around{actionName}()`.

**Examples**

```javascript
// executes around any action
function aroundHandler(event,targetAction,eventArguments,rc,prc){
}

// executes around the list() action ONLY
function aroundList(event,targetAction,eventArguments,rc,prc){
}

// Around handler advice for transactions
function aroundHandler(event,targetAction,eventArguments,rc,prc){

    // log the call
    log.debug("Starting to execute #targetAction.toString()#" );

    // start a transaction
    transaction{

        // prepare arguments for action call
        var args = {
            event = arguments.event,
            rc    = arguments.rc,
            prc   = arguments.prc

        };
        structAppend( args, eventArguments );
        // execute the action now
        var results = arguments.targetAction( argumentCollection=args );
    }

    // log the call
    log.debug( "Ended executing #targetAction.toString()#" );

    // return if it exists
    if( !isNull( results ) ){ return results; }
}

// Around handler advice for try/catches
function aroundHandler(event,targetAction,eventArguments,rc,prc){

    // log the call
    if( log.canDebug() ){
        log.debug( "Starting to execute #targetAction.toString()#" );
    }

    // try block
    try{

        // prepare arguments for action call
        var args = {
            event = arguments.event,
            rc    = arguments.rc,
            prc   = arguments.prc

        };
        structAppend( args, eventArguments );
        // execute the action now
        return arguments.targetAction( argumentCollection=args );
    }
    catch(Any e){
        // log it
        log.error("Error executing #targetAction.toString()#: #e.message# #e.detail#", e);
        // set exception in request collection and set view to render
        event.setValue( "exception", e)
            .setView( "errors/generic" );

    }

}
```

The arguments received by these interceptors are:

* `event` : The request context reference
* `targetAction` : The function pointer to the action that got the around interception. It will be your job to execute it \(Look at samples\)
* `eventArguments` : The struct of extra arguments sent to an action if any
* `rc` : The **RC** reference
* `prc` : The **PRC** Reference

### Exceptions & Only Lists

You can fine tune these interception methods by leveraging two public properties in the handler:

* `this.aroundhandler_only` : A list of actions that the `aroundHandler()` action will fire ONLY!
* `this.aroundhandler_except` : A list of actions that the `aroundHandler()` action will NOT fire on

```javascript
// only fire for the actions: save(), delete()
this.aroundhandler_only = "save,delete";
// DO NOT fire for the actions: login(), doLogin(), logout()
this.aroundhandler_except = "login,doLogin,logout"
```

## Model Integration

We have a complete section dedicated to the [Model Layer](../../models/), but we wanted to review a little here since event handlers need to talk to the model layer all the time. By default, you can interact with your models from your event handlers in two ways:

* Dependency Injection \([Aggregation](https://atomicobject.com/resources/oo-programming/object-oriented-aggregation)\)
* Request, use and discard model objects \([Association](https://en.wikipedia.org/wiki/Association_%28object-oriented_programming%29)\)

ColdBox offers its own dependency injection framework, [WireBox](https://wirebox.ortusbooks.com), which allows you, by convention, to talk to your model objects. However, ColdBox also allows you to connect to third-party dependency injection frameworks via the _IOC_ module: [http://forgebox.io/view/cbioc](http://forgebox.io/view/cbioc)

{% hint style="info" %}
Aggregation differs from ordinary composition in that it does not imply ownership. In composition, when the owning object is destroyed, so are the contained objects. - **wikipedia**
{% endhint %}

### Dependency Injection

Your event handlers can be **autowired** with dependencies from [WireBox](https://wirebox.ortusbooks.com) by convention. By autowiring dependencies into event handlers, they will become part of the life span of the event handlers \(**singletons**\), since their references will be injected into the handler's `variables` scope. This is a huge performance benefit since event handlers are wired with all necessary dependencies upon creation instead of requesting dependencies \(usage\) at runtime. We encourage you to use injection whenever possible.

{% hint style="danger" %}
**Warning** As a rule of thumb, inject only **singletons** into **singletons**. If not you can create unnecessary [scope-widening injection](https://wirebox.ortusbooks.com/advanced-topics/providers/scope-widening-injection) issues and memory leaks.
{% endhint %}

You will achieve this in your handlers via `property` injection, which is the concept of defining properties in the component with a special annotation called `inject`, which tells WireBox what reference to retrieve via the [WireBox Injection DSL](/#injection). Let's say we have a **users** handler that needs to talk to a model called **UserService**. Here is the directory layout so we can see the conventions

{% code title="Directory Layout" %}
```text
+ handlers
  + users.cfc
+ models
  + UserService.cfc
```
{% endcode %}

Here is the event handler code to leverage the injection:

{% code title="users.cfc" %}
```javascript
component name="MyHandler"{

    // Dependency injection of the model: UserService -> variables.userService
    property name="userService" inject="UserService";

    function index( event, rc, prc ){
        prc.data = userService.list()
        event.setView( "users/index" );
    }

}
```
{% endcode %}

Notice that we define a `cfproperty` with a name and `inject` attribute. The `name` becomes the name of the variable in the `variables` scope and the `inject` annotation tells WireBox what to retrieve. By default it retrieves model objects by name and path.

{% hint style="success" %}
**Tip:** The [injection DSL](/#injection) is vast and elegant. Please refer to it. Also note that you can create object aliases and references in your [config binder](https://wirebox.ortusbooks.com/configuration/configuring-wirebox): `config/WireBox.cfc`
{% endhint %}

### Requesting Model Objects

The other approach to integrating with model objects is to request and use them as [associations](http://en.wikipedia.org/wiki/Association_%28object-oriented_programming%29) via the framework super type method: `getInstance()`, which in turn delegates to WireBox's `getInstance()` method. We would recommend requesting objects if they are **transient** \(have state\) objects or stored in some other volatile storage scope \(session, request, application, cache, etc\). Retrieving of objects is okay, but if you will be dealing with mostly **singleton** objects or objects that are created only once, you will gain much more performance by using injection.

{% code title="users.cfc" %}
```javascript
component{

    function index( event, rc, prc ){
        // Request to use the user service, this would be best to inject instead 
        // of requesting it.
        prc.data = getInstance( "UserService" ).list();
        event.setView( "users/index" );
    }

    function save( event, rc, prc ){
        // request a user transient object, populate it and save it.
        prc.oUser = populateModel( getInstance( "User" ) );
        userService.save( prc.oUser );
        relocate( "users/index" );
    }

}
```
{% endcode %}

{% hint style="info" %}
**Association** defines a relationship between classes of objects that allows one object instance to cause another to perform an action on its behalf. - 'wikipedia'
{% endhint %}

### A practical example

In this practical example we will see how to integrate with our model layer via WireBox, injections, and also requesting the objects. Let's say that we have a service object we have built called `FunkyService.cfc` and by convention we will place it in our applications `models` folder.

{% code title="Directory Layout" %}
```javascript
 + application
  + models
     + FunkyService.cfc
```
{% endcode %}

**FunkyService.cfc**

```javascript
component singleton{

    function init(){
        return this;
    }

    function add(a,b){ 
        return a+b; 
    }

    function getFunkyData(){
        var data = [
            {name="Luis", age="33"},
            {name="Jim", age="99"},
            {name="Alex", age="1"},
            {name="Joe", age="23"}
        ];
        return data;
    }

}
```

Our funky service is not that funky after all, but it is simple. How do we interact with it? Let's build a Funky event handler and work with it.

#### Injection

```javascript
component{

    // Injection via property
    property name="funkyService" inject="FunkyService";

    function index(event,rc,prc){

        prc.data = funkyService.getFunkyData();

        event.renderData( data=prc.data, type="xml" );
    }    


}
```

By convention, I can create a **property** and annotate it with an `inject` attribute. ColdBox will look for that model object by the given name in the `models` folder, create it, persist it, wire it, and return it. If you execute it, you will get something like this:

```markup
<array>
    <item>
        <struct>
            <name>Luis</name>
            <age>33</age>
        </struct>
    </item>
    <item>
        <struct>
            <name>Jim</name>
            <age>99</age>
        </struct>
    </item>
    <item>
        <struct>
            <name>Alex</name>
            <age>1</age>
        </struct>
    </item>
    <item>
        <struct>
            <name>Joe</name>
            <age>23</age>
        </struct>
    </item>
</array>
```

Great! Just like that we can interact with our model layer without worrying about creating the objects, persisting them, and even wiring them. Those are all the benefits that dependency injection and model integration bring to the table.

**Alternative wiring**

You can use the value of the `inject` annotation in several ways. Below is our recommendation.

```javascript
// Injection using the DSL by default name/id lookup
property name="funkyService" inject="FunkyService";
// Injection using the DSL id namespace
property name="funkyService" inject="id:FunkyService";
// Injection using the DSL model namespace
property name="funkyService" inject="model:FunkyService";
```

#### Requesting

Let's look at the requesting approach. We can either use the following approaches:

**Via Facade Method**

```javascript
component{

    function index(event,rc,prc){

        prc.data = getInstance( "FunkyService" ).getFunkyData();

        event.renderData( data=prc.data, type="xml" );
    }    


}
```

**Directly via WireBox:**

```javascript
component{

    function index(event,rc,prc){

        prc.data = wirebox.getInstance( "FunkyService" ).getFunkyData();

        event.renderData( data=prc.data, type="xml" );
    }    


}
```

Both approaches do exactly the same thing. In reality `getInstance()` does a `wirebox.getInstance()` callback \(Uncle Bob\), but it is a facade method that is easier to remember. If you run this, you will also see that it works and everything is fine and dandy. However, the biggest difference between injection and usage can be seen with some practical math:

```javascript
1000 Requests made to users.index

- Injection: 1000 handler calls + 1 model creation and wiring call = 1001 calls
- Requesting: 1000 handler calls + 1000 model retrieval + 1 model creation call = 2002 calls
```

As you can see, the best performance is due to injection as the handler object was wired and ready to roll, while the requested approach needed the dependency to be requested. Again, there are cases where you need to request objects such as transient or volatile stored objects.

## Model Data Binding

The framework also offers you the capability to bind incoming FORM/URL/REMOTE/XML/JSON/Structure data into your model objects by convention. This is done via [WireBox's object population](https://wirebox.ortusbooks.com/advanced-topics/wirebox-object-populator) capabilities. The easiest approach is to use our `populateModel()` function which will populate the object from many incoming sources:

* request collection **RC**
* Structure
* json
* xml
* query

This will try to match incoming variable names to setters or properties in your domain objects and then populate them for you. It can even do ORM entities with ALL of their respective relationships. Here is a snapshot of the method:

```javascript
/**
* Populate a model object from the request Collection or a passed in memento structure
* @model The name of the model to get and populate or the acutal model object. If you already have an instance of a model, then use the populateBean() method
* @scope Use scope injection instead of setters population. Ex: scope=variables.instance.
* @trustedSetter If set to true, the setter method will be called even if it does not exist in the object
* @include A list of keys to include in the population
* @exclude A list of keys to exclude in the population
* @ignoreEmpty Ignore empty values on populations, great for ORM population
* @nullEmptyInclude A list of keys to NULL when empty
* @nullEmptyExclude A list of keys to NOT NULL when empty
* @composeRelationships Automatically attempt to compose relationships from memento
* @memento A structure to populate the model, if not passed it defaults to the request collection
* @jsonstring If you pass a json string, we will populate your model with it
* @xml If you pass an xml string, we will populate your model with it
* @qry If you pass a query, we will populate your model with it
* @rowNumber The row of the qry parameter to populate your model with
*/
function populateModel(
    required model,
    scope="",
    boolean trustedSetter=false,
    include="",
    exclude="",
    boolean ignoreEmpty=false,
    nullEmptyInclude="",
    nullEmptyExclude="",
    boolean composeRelationships=false,
    struct memento=getRequestCollection(),
    string jsonstring,
    string xml,
    query qry
){
```

Let's do a quick example:

**Person.cfc**

```javascript
component accessors="true"{

    property name="name";
    property name="email";

    function init(){
        setName('');
        setEmail('');
    }
}
```

**editor.cfm**

```javascript
<cfoutput>
<h1>Funky Person Form</h1>
#html.startForm(action='person.save')#

    #html.textfield(label="Your Name:",name="name",wrapper="div")#
    #html.textfield(label="Your Email:",name="email",wrapper="div")#

    #html.submitButton(value="Save")#

#html.endForm()#
</cfoutput>
```

**Event Handler -&gt; person.cfc**

```javascript
component{

    function editor(event,rc,prc){
        event.setView("person/editor");        
    }

    function save(event,rc,prc){

        var person = populateModel( "Person" );

        writeDump( person );abort;
    }

}
```

In the dump you will see that the `name` and `email` properties have been bound.

## HTTP Method Security

More often you will find that certain web operations need to be restricted in terms of what HTTP verb is used to access a resource. For example, you do not want form submissions to be done via **GET** but via **POST** or **PUT** operations. HTTP Verb recognition is also essential when building strong RESTFul APIs when security is needed as well.

### Manual Solution

You can do this manually, but why do the extra coding :\)

```javascript
function delete(event,rc,prc){
    // determine incoming http method
    if( event.getHTTPMethod() == "GET" ){
        flash.put("notice","invalid action");
        setNextEvent("users.list");
    }
    else{
        // do delete here.
    }
}
```

This solution is great and works, but it is not THAT great. We can do better.

### Allowed Methods Property

Another feature property on an event handler is called `this.allowedMethods`. It is a declarative structure that you can use to determine what the allowed HTTP methods are for any action on the event handler.

```javascript
this.allowedMethods = {
    actionName : "List of approved HTTP Verbs"
};
```

If the request action HTTP method is not found in the approved list, it will look for a `onInvalidHTTPMethod()` on the handler and call it if found. Otherwise ColdBox throws a **405 exception** that is uniform across requests.

{% hint style="info" %}
You can listen for [global invalid HTTP](../../getting-started/configuration/coldbox.cfc/configuration-directives/) methods using the `coldbox.onInvalidHTTPMethodHandler` located in your `config/ColdBox.cfc.`
{% endhint %}

```javascript
component{

    this.allowedMethods = {
        delete : "POST,DELETE",
        list   : "GET"
    };

    function list(event,rc,prc){
        // list only
    }

    function delete(event,rc,prc){
        // do delete here.
    }
}
```

{% hint style="info" %}
If the **action** is not listed in the structure, then it means that we allow **all** HTTP methods. Just remember to either use the `onError()` or `onInvalidHTTPMethod()` method conventions or an exception handler to deal with the security exceptions.
{% endhint %}

### Allowed Methods Annotation

You can tag your event actions with a `allowedMethods` annotation and add a list of the allowed HTTP verbs as well. This gives you a nice directed ability right at the function level instead of a property. It is also useful when leveraging DocBox documentation as it will show up in the API Docs that are generated.

```javascript
function index( event, rc, prc) allowedMethods="GET,POST"{
    // my code here
}
```

## Implicit Methods

Every event handler controller has some **implicit** methods that if you create them, they come alive. Just like the implicit methods in `Application.cfc`

### onMissingAction\(\)

With this convention you can create virtual events that do not even need to be created or exist in a handler. Every time an event requests an action from an event handler and that action does **not** exist in the handler, the framework will check if an `onMissingAction()` method has been declared. If it has, it will execute it. This is very similar to ColdFusion's `onMissingMethod()` but on an event-driven framework.

```javascript
function onMissingAction( event, rc, prc, missingAction, eventArguments ){

}
```

This event has an extra argument: **missingAction** which is the missing action that was requested. You can then do any kind of logic against this missing action and decide to do internal processing, error handling or anything you like. The power of this convention method is extraordinary, you have tons of possibilities as you can create virtual events on specific event handlers.

### onError\(\)

This is a localized error handler for your event handler. If any type of **runtime** error occurs in an event handler and this method exists, then the framework will call your method so you can process the error first. If the method does not exist, then normal error procedures ensue.

{% hint style="info" %}
Please note that compile time errors will not fire this method, only runtime exceptions.
{% endhint %}

```javascript
// On Error
function onError( event, rc, prc, faultAction, exception, eventArguments ){
    // prepare a data packet
    var data = {
        error = true,
        messages = exception.message & exception.detail,
        data = ""
    }

    // log via the log variable already prepared by ColdBox
    log.error("Exception when executing #arguments.faultAction# #data.messages#", exception);    

    // render out a json packet according to specs status codes and messages
    event.renderData(data=data,type="json",statusCode=500,statusMessage="Error ocurred");

}
```

### onInvalidHTTPMethod\(\)

This method will be called for you if a request is trying to execute an action in your handler without the proper approved HTTP Verb. It will then be your job to determine what to do next:

```javascript
function onInvalidHTTPMethod( faultAction, event, rc, prc ){
    return "Go away!";
}
```

## Executing Events

Apart from executing events from the URL/FORM or Remote interfaces, you can also execute events internally, either **public** or **private** from within your event handlers or from interceptors, other handlers, layouts or views.

### RunEvent\(\)

You do this by using the `runEvent()` method which is inherited from our `FrameworkSuperType` class. Here is the method signature:

```javascript
/**
* Executes events with full life-cycle methods and returns the event results if any were returned.
* @event The event string to execute, if nothing is passed we will execute the application's default event.
* @prePostExempt If true, pre/post handlers will not be fired. Defaults to false
* @private Execute a private event if set, else defaults to public events
* @defaultEvent The flag that let's this service now if it is the default event running or not. USED BY THE FRAMEWORK ONLY
* @eventArguments A collection of arguments to passthrough to the calling event handler method
* @cache Cached the output of the runnable execution, defaults to false. A unique key will be created according to event string + arguments.
* @cacheTimeout The time in minutes to cache the results
* @cacheLastAccessTimeout The time in minutes the results will be removed from cache if idle or requested
* @cacheSuffix The suffix to add into the cache entry for this event rendering
* @cacheProvider The provider to cache this event rendering in, defaults to 'template'
*/
function runEvent(
    event="",
    boolean prePostExempt=false,
    boolean private=false,
    boolean defaultEvent=false,
    struct eventArguments={},
    boolean cache=false,
    cacheTimeout="",
    cacheLastAccessTimeout="",
    cacheSuffix="",
    cacheProvider="template"
)
```

The interesting aspect of internal executions is that all the same rules apply, so your handlers can return content like widgets, views, or even data. Also, the **eventArguments** enables you to pass arguments to the method just like method calls:

#### **Executions**

```javascript
//public event
runEvent( 'users.save' );

//post exempt
runEvent( event='users.save', prePostExempt=true );

//Private event
runEvent( event='users.persist', private=true );

// Run event as a widget
<cfoutput>#runEvent(
    event         = 'widgets.userInfo',
    prePostExempt = true,
    eventArguments= { widget=true }
)#</cfoutput>

// Run with Caching
runEvent( event="users.list", cache=true, cacheTimeout=30 );
```

#### **Declaration**

```javascript
// handler responding to widget call
function userInfo( event, rc, prc, widget=false ){

    prc.userInfo = userService.get( rc.id );

    // set or widget render
    if( arguments.widget ){
        return renderView( "widgets/userInfo" );
    }

    // else set view
    event.setView( "widgets/userInfo" );
}
```

#### Caching

As you can see from the function signature you can tell ColdBox to cache the result of the event call. All of the cached content will go into the **template** cache by default unless you use the `cacheProvider` argument. The cache keys are also based on the name of the event and the signature of the `eventArguments` structure. Meaning, the framework can cache multiple permutations of the same event call as long as the `eventArguments` are different.

```javascript
runEvent( event="users.widget", eventArguments={ max=10, page=1 }, cache=true );

// Cached as a new key
runEvent( event="users.widget", eventArguments={ max=10, page=2 }, cache=true );
```

{% hint style="success" %}
**Tip**: You can disable event caching by using the `coldbox.eventCaching` directive in your `config/ColdBox.cfc`
{% endhint %}

## Executing Routes

A part from using `runEvent()` to execute events, you can also abstract it by using the `runRoute()` method. This method is fairly similar but with the added benefit of executing a NAMED route instead of the direct event it represents. This gives you the added flexibility of abstracting the direct event and leveraging the named route.

All the same feature of `runEvent()` apply to `runRoute()`

### RunRoute\(\)

Just like you can create links based on named routes and params, you can execute named routes and params as well internally via `runRoute()`

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

#### Parameters

The params argument you pass to the runRoute\(\) method will be translated into event arguments. Therefore they will be passed as arguments to the event the route represents:

```javascript
// Execute
runRoute( "userData", { id=4 } )
```

In the example above, the `userData` named route points to the `user.data` event.

{% code title="user.cfc" %}
```javascript
component{

    property name="userService" inject;

    function data( event, rc, prc, id=0 ){
        if( id == 0 )
            return {};

        return userService.getData( id );
    }
}
```
{% endcode %}

#### Module Routes

If you want to execute module routes, no problem! Just use our `@ or :` notation to tell the controller from which module's router we should pick the route from.

```javascript
# Using @ destination
runRoute( "userData@user", { id=4 } )

# Using : prefix
runRoute( "user:userData", { id=4 } )
```

## Viewlets - Reusable Events

A viewlet is a self sufficient view or a widget that can live on its own, its data is pre-fetched and can just be renderer anywhere in your system.

What in the world is this? Well, imagine a portal, in which each section of the portal is self-sufficient, with controls and data. You don't want to call all the handlers for this data for every single piece of content. It's not efficient, you need to create a separation. Well, a viewlet is such a separation that provides you with the ability to create reusable events. So how do we achieve this?

1. You will use the method `runEvent()` anywhere you want a viewlet to be displayed or the content rendered. This calls an internal event that will be in charge to prepare and render the viewlet.
2. Create the portable event but make sure it returns the produced content.

### **Simple Example**

```javascript
<div id="leftbar">
#runEvent( event='viewlets.userinfo', eventArguments={ userID=4 } )#
</div>
```

This code just renders out the results of a `runEvent()` method call. Please note that you can pass in arguments to the event using the `eventArguments` argument. This makes the event act like a method call with arguments to ti. Remember that all events you call via `runEvent()` will share the same RC/PRC.

{% hint style="info" %}
I would suggest you look at [the API docs](https://apidocs.coldbox.org/) to discover all arguments to the `runEvent()` method call.
{% endhint %}

#### **Event Code**

{% code title="viewlets.cfc" %}
```javascript
function userinfo( event, rc, prc, userID=0 ){

    // place data in prc and prefix it to avoid collisions
    prc.userinfo_qData = userService.getUserInfo( arguments.userID );

    // render out content 
    return renderView( "viewlets/userinfo" );
}
```
{% endcode %}

As you can see from the code above, the handler signature can accept arguments which are passed via the `eventArguments` structure. It talks to a service layer and place some data on the private request collection the viewlet will use. It then returns the results of a `renderView()` call that will render out the exact viewlet I want. You can be more creative and do things like:

* render a layout + view combo
* render data
* return your own custom strings
* etc

{% hint style="warning" %}
**Caution** We would suggest you namespace or prefix your private request collection variables for viewlets in order to avoid collisions from multiple viewlet events in the same execution thread or instead pass the necessary arguments into a view via the `args` argument.
{% endhint %}

#### **View Code**

{% code title="viewlets/userinfo.cfm" %}
```markup
<cfoutput>
    <div>User Info Panel</div>
    <div>Username: #prc.userinfo_qData.username#</div>
    <div>Last Login: #prc.userinfo_qData.lastLogin#</div>
</cfoutput>
```
{% endcode %}

The view is a normal standard view, it doesn't even know it is a viewlet, remember, views are DUMB!

### Content Variables

A content variable is a variable that contains HTML/XML or any kind of visual content that can easily be rendered anywhere. So instead of running the viewlet event in the view, you can abstract it to the controller layer and assign the output to a content variable:

```javascript
function home(event,rc,prc){

    // render some content variables with funky arguments
    prc.sideColumn = renderView(view='tags/sideColumn',cache=true,cacheTimeout=10);

    // set view
    event.setView('general/home');
}
```

So how do I render it?

```javascript
<div id="content">
  <div id="leftColumn">
  <cfoutput>#prc.sideColumn#</cfoutput>
  </div>

  <div id="mainView">
  <cfoutput>#renderView()#</cfoutput>
  </div>
</div>
```

Another example, is what if we do not know if the content variable will actually exist? How can we do this? Well, we use the event object for this and its magic getValue\(\) method.

```javascript
<div id="content">
  <div id="leftColumn">
  <cfoutput>#prc.sideColumn ?: ''#</cfoutput>
  </div>

  <div id="mainView">
  <cfoutput>#renderView()#</cfoutput>
  </div>
</div>
```

So now, if no content variable exists, an empty string will be rendered.

{% hint style="danger" %}
**Important** String manipulation in Java relies on immutable structures, so performance penalities might ensue. If you will be doing a lot of string manipulation, concatenation or rendering, try to leverage native java objects: StringBuilder or StringBuffer
{% endhint %}

## Event Caching

Event caching is extremely useful and easy to use. ColdBox will act like a cache proxy between your events and the clients requesting the events, much like squid, nginx or HA Proxy. All you need to do is add several metadata arguments to the action methods and the framework will cache the **output** of the event in the **template** cache provider in CacheBox. In other words, the event executes and produces output that the framework then caches. Subsequent calls to the same event with the same incoming RC variables will not do any processing, but just output the content back to the user.

For example, you have an event called `blog.showEntry`. This event executes, gets an entry from the database and sets a view to be rendered. The framework then renders the view and if event caching is turned **on** for this event, the framework will cache the HTML produced. So the next incoming show entry event will just spit out the cached HTML. The cache key is created by hashing the incoming request collection.

{% hint style="info" %}
Important to note also, that any combination of URL/FORM parameters on an event will produce a unique cacheable key. So `event=blog.showEntry&id=1` & `event=blog.showEntry&id=2` are two different cacheable events.
{% endhint %}

### Enabling Event Caching

To enable event caching, you will need to set a setting in your `ColdBox.cfc` called `coldbox.eventcaching` to `true`.

```javascript
 coldbox.eventCaching = true;
```

{% hint style="danger" %}
**Important** Enabling event caching does not mean that ALL events will be cached. It just means that you enable this feature.
{% endhint %}

### Setting Up Actions For Caching

The way to set up an event for caching is on the function declaration with the following annotations:

| **Annotation** | **Type** | **Description** |
| :--- | :--- | :--- |
| `cache` | boolean | A true or false will let the framework know whether to cache this event or not. The default is FALSE. So setting to false makes no sense |
| `cachetimeout` | numeric | The timeout of the event's output in minutes. This is an optional attribute and if it is not used, the framework defaults to the default object timeout in the cache settings. You can place a 0 in order to tell the framework to cache the event's output for the entire application timeout controlled by coldfusion, NOT GOOD. Always set a decent timeout for content. |
| `cacheLastAccesstimeout` | numeric | The last access timeout of the event's output in minutes. This is an optional attribute and if it is not used, the framework defaults to the default last access object timeout in the cache settings. This tells the framework that if the object has not been accessed in X amount of minutes, then purge it. |
| `cacheProvider` | string | The cache provider to store the results in. By default it uses the **template** cache. |

{% hint style="danger" %}
**Important** Please be aware that you should not cache output with 0 timeouts \(forever\). Always use a timeout.
{% endhint %}

```javascript
// In Script
function showEntry(event,rc,prc) cache="true" cacheTimeout="30" cacheLastAccessTimeout="15"{
    //get Entry
    prc.entry = getEntryService().getEntry(event.getValue('entryID',0));

    //set view
    event.setView('blog/showEntry');
}
```

{% hint style="danger" %}
**Alert:** DO NOT cache events as unlimited timeouts. Also, all events can have an unlimited amount of permutations, so make sure they expire and you purge them constantly. Every event + URL/FORM variable combination will produce a new cacheable entry.
{% endhint %}

### Storage

All event and view caching are stored in a named cache called `template` which all ColdBox applications have by default. You can open or create a new [CacheBox](https://cachebox.ortusbooks.com) configuration object and decide where the storage is, timeouts, providers, etc. You have complete control of how event and view caching is stored.

### Purging

We also have a great way to purge these events programmatically via our cache provider interface.

```javascript
templateCache = cachebox.getCache( "template" );
```

Methods for event purging:

* `clearEvent( string eventSnippet, string querystring="" )`: Clears all the event permutations from the cache according to snippet and querystring. Be careful when using incomplete event name with query strings as partial event names are not guaranteed to match with query string permutations
* `clearEventMulti( eventsnippets,string querystring="" )`: Clears all the event permutations from the cache according to the list of snippets and querystrings. Be careful when using incomplete event name with query strings as partial event names are not guaranteed to match with query string permutations
* `clearAllEvents( [boolean async=true] )` : Can clear ALL cached events in one shot and can be run asynchronously.

```javascript
//Trigger to purge all Events
getCache( "template" ).clearAllEvents();

//Trigger to purge all events synchronously
getCache( "template" ).clearAllEvents(async=false);

//Purge all events from the blog handler
getCache( "template" ).clearEvent('blog');

//Purge all permutations of the blog.dspBlog event
getCache( "template" ).clearEvent('blog.dspBlog');

//Purge the blog.dspBlog event with entry of 12345
getCache( "template" ).clearEvent('blog.dspBlog','id=12345')
```

#### this.event\_cache\_suffix

You can now leverage the cache suffix property in handlers to be declared as a closure so it can be evaluated at runtime so it can add dynamic suffixes to cache keys. This can allow you to incorporate elements into the cache key at runtime instead of statically. This is a great way to incorporate the user's language locale or session identifier to make unique entries.

```java
this.EVENT_CACHE_SUFFIX = function( eventHandlerBean ){
  return "a localized string, etc";
};
```

### `OnRequestCapture` - Influence Cache Keys

We have provided an interception point in ColdBox that allows you to add variables into the request collection before a snapshot is made so you can influence the cache key of a cacheable event. What this means is that you can use it to mix in variables into the request collection that can make this event cache unique for a user, a specific language, country, etc. This is a great way to leverage event caching on multi-lingual or session based sites.

```javascript
component{

    onRequestCapture(event,interceptData){
        var rc = event.getCollection();

        // Add user's locale to the request collection to influenze event caching
        rc._user_locale = getFWLocale();
    }

}
```

With the simple example above, the user's locale will be addded to all your event caching permutations and thus create entries for different languages.

### Life-Cycle Caveats

{% hint style="danger" %}
Several event interception points are NOT available during event caching.
{% endhint %}

When using event caching the framework will NOT execute ANY event at all. It will stream the content directly from the selected cache provider. This means that any interceptors or code that executes in the event is also NOT executed. The only interception points that will execute are:

* `preProcess`
* `postProcess`

So please make sure you take note of this when planning for event security.

### Monitoring

[CacheBox](http://cachebox.ortusbooks.com) has an intuitive and powerful monitor that can be used via the ColdBox Debugger Module. From the monitor you can purge, expire and view cache elements, etc.

```text
box install cbdebugger
```

## Validation

ColdBox Core MVC does not have validation built-in but it is implemented via the offical core `cbValidation` module. You can easily install the module in your application via:

```bash
box install cbvalidation
```

You can find much more information about this module in the following resources:

* **Source**: [https://github.com/coldbox/cbox-validation](https://github.com/coldbox/cbox-validation)
* **Documentation**: [https://github.com/coldbox-modules/cbox-validation/wiki](https://github.com/coldbox-modules/cbox-validation/wiki)
* **ForgeBox** : [http://forgebox.io/view/cbvalidation](http://forgebox.io/view/cbvalidation)

## Layouts & Views

ColdBox provides you with a very simple but flexible and powerful layout manager and content **renderer**. You no longer need to create module tags or convoluted broken up HTML anymore. You can concentrate on the big picture and create as many [layouts](../../getting-started/configuration/coldbox.cfc/configuration-directives/layouts.md) as your application needs. Then you can programmatically change rendering schemas \(or skinning\) and also create composite or component based views.

In this section we will explore the different rendering mechanisms that ColdBox offers and also how to utilize them. As you know, [event handlers](../event-handlers/) are our controller layer in ColdBox and we will explore how these objects can interact with the user in order to render content, whether HTML, JSON, XML or any type of rendering data.

{% hint style="info" %}
Please note that you can use ColdBox as a pure API solution with modern JavaScript frameworks for the front end like VueJS, Reactor, Angular, etc.
{% endhint %}

### Conventions

Let's do a recap of our conventions for layouts and view locations:

```javascript
+ application
  + layouts
  + views
```

### ColdBox Renderer

It is imperative to know who does the rendering in ColdBox and that is the **Renderer** class that you can see from our diagram above. As you can tell from the diagram, it includes your layouts and/or views into itself in order to render out content. So by this association and inheritance all layouts and views have some variables and methods at their disposal since they get absorbed into the object. You can visit the [API docs](http://apidocs.ortussolutions.com/coldbox/current) to learn about all the Renderer methods.

The **Renderer** is a **transient** object in order to avoid variable collisions, meaning it is recreated on each request. All of the following property members exist in all layouts and views rendered by the **Renderer**:

| **Property** | **Description** |
| :--- | :--- |
| `event` | A reference to the Request Context object |
| `rc` | A reference to the request collection inside of the request context \(For convenience\) |
| `prc` | A reference to the private request collection inside of the request context \(For convenience\) |
| `html` | A reference to the [HTML Helper](../../digging-deeper/html-helper.md)  that can help you build interactive and safe HTML |
| `cacheBox` | A reference to the [CacheBox](http://wiki.coldbox.org/wiki/CacheBox.cfm) framework factory \(_coldbox.system.cache.CacheFactory_\) |
| `controller` | A reference to the application's ColdBox Controller \(_coldbox.system.web.Controller_\) |
| `flash` | A reference to the current configured Flash Object Implementation that inherits from the AbstractFlashScope [AbstractFlashScope](http://www.coldbox.org/api) \(derived _coldbox.system.web.flash.AbstractFlashScope_\) |
| `logbox` | The reference to the [LogBox](http://wiki.coldbox.org/wiki/LogBox.cfm) library \(coldbox.system.logging.LogBox\) |
| `log` | A pre-configured LogBox [Logger](http://wiki.coldbox.org/wiki/LogBox.cfm) object for this specific class object \(_coldbox.system.logging.Logger_\) |
| `wirebox` | A reference to the [WireBox](http://wiki.coldbox.org/wiki/WireBox.cfm) object factory \(_coldbox.system.ioc.Injector_\) |

As you can see, all views and layouts have direct reference to the request collections so it makes it incredibly easy to get and put data into it. Also, remember that the Renderer inherits from the Framework SuperType so all methods are at your disposal if needed.

### Injecting In Your Models

You can also use the ColdBox Renderer in your models so you can render email templates, views, etc. We won't inject the renderer directly because remember that the renderer is a transient object. So we will use a WireBox feature called [**provider**](https://wirebox.ortusbooks.com/advanced-topics/providers), which will inject a proxy placeholder that looks like the renderer and behaves like the renderer. But behinds the scene it takes care of the persistence. So you can just use it!

```java
component{

    property name="renderer" inject="provider:coldbox:renderer";

    function renderSomething(){
        return renderer.renderView( view="mail/mymail", args={} );
    }
}
```

## Views

Views are HTML content that can be rendered inside of a layout or by themselves. They can be either rendered on demand or by being set by an event handler. Views can also produce any type of content apart from HTML like JSON/XML/WDDX via our view renderer that we will discover also. So get ready for some rendering goodness!

### Setting Views For Rendering

Usually, event handlers are the objects in charge of setting views for rendering. However, ANY object that has access to the request context object can do this also. This is done by using the `setView()` method in the request context object.

{% hint style="info" %}
Setting a view does not mean that it gets rendered immediately. It means that it is deposited in the request context. The framework will later on in the execution process pick those variables up and do the actual rendering. To do immediate rendering you will use the inline rendering methods describe later on.
{% endhint %}

{% code title="handlers/main.cfc" %}
```javascript
component
{

    function index(event,rc,prc){
        // call some model for data and put into the request collection
        prc.myQuery = getInstance('MyService').getData();
        // set the view for rendering
        event.setView( "general/index" );

    }

}
```
{% endcode %}

We use the `setView()` method to set the view `views/general/index.cfm` to be rendered. Now the cool thing about this, is that we can override the view to be rendered anytime during the flow of the request. So the last process to execute the `setView()` method is the one that counts. Also notice a few things:

* No `.cfm` extension is needed.
* You can traverse directories by using `/` like normal `cfinclude` notation.
* The view can exist in the conventions directory `views` or in your configured external locations
* You did not specify a layout for the view, so the application's default layout \(`main.cfm`\) will be used.

{% hint style="danger" %}
It is best practice that view locations should simulate the event. So if the event is **general.index**, there should be a **general** folder in the root **views** folder with a view called **index.cfm.**
{% endhint %}

**Let's look at the view code:**

{% code title="main/index.cfm" %}
```javascript
<cfoutput>
<h1>My Cool Data</h1>
#html.table( data=prc.myQuery, class="table table-striped table-hover" )#

</cfoutput>
```
{% endcode %}

I am using our cool HTML Helper class that is smart enough to render tables, data, HTML 5 elements etc and even bind to ColdFusion ORM entities.

#### Views With No Layout

So what happens if I DO NOT want the view to be rendered within a layout? Am I doomed? Of course not, just use the same method with the `noLayout` argument or `event.noLayout()` method:

```javascript
component{

    function index(event,rc,prc){
        // call some model for data and put into the request collection
        prc.myQuery = getInstance('MyService').getData();
        // set the view for rendering
        event.setView( view="general/index", noLayout=true );
    }

    function index(event,rc,prc){
        // call some model for data and put into the request collection
        prc.myQuery = getInstance('MyService').getData();
        // set the view for rendering
        event.setView( "general/index" ).noLayout();
    }
}
```

#### Views With Layouts

If you need the view to be rendered in a **specific** layout, then use the `layout` argument or the `setLayout()` method:

```javascript
component name="general"{

    function index(event,rc,prc){
        // call some model for data and put into the request collection
        prc.myQuery = getInstance('MyService').getData();
        // set the view for rendering
        event.setView( view="general/index", layout="Ajax" );
    }

    function index(event,rc,prc){
        // call some model for data and put into the request collection
        prc.myQuery = getInstance('MyService').getData();
        // set the view for rendering
        event.setView( "general/index" ).setLayout( "Ajax" );
    }

}
```

#### Views From Modules

If you need the set a view to be rendered from a specific ColdBox Module then use the `module` argument alongside any other argument combination:

```javascript
component name="general"{

    function index(event,rc,prc){

        // call some model for data and put into the request collection
        prc.myQuery = getInstance('MyService').getData();
        // set the view for rendering
        event.setView( view="general/index", module="shared-views" );

    }

}
```

### Render Nothing

You can also tell the renderer to not render back anything to the user by using the `event.noRender()` method. Maybe you just took some input and need to gracefully shutdown the request into the infamous white screen of death.

```javascript
component name="general"{

    function saveData(event,rc,prc){
        // do your work here …..

        // set for no render
        event.noRender();
    }

}
```

### Implicit Views

You can also omit the explicit `event.setView()` if you want, ColdBox will then look for the view according to the executing event's syntax by convention. So if the incoming event is called `general.index` and no view is explicitly defined in your handler, ColdBox will look for a view in the `general` folder called `index.cfm`. That is why we recommend trying to match event resolution to view resolution even if you use or not implicit views.

{% hint style="success" %}
**Tip:** This feature is more for conventions purists than anything else. However, we do recommend as best practice to use explicitly declare the view to be rendered when working with team environments as everybody will know what happens.
{% endhint %}

```javascript
component name="general"{

    function index(event,rc,prc){
        // call some model for data and put into the request collection
        prc.myQuery = getInstance('MyService').getData();    
    }

}
```

{% hint style="danger" %}
**Caution** If using implicit views, please note that the **name** of the view will **ALWAYS** be in lower case. So please be aware of this **limitation**. I would suggest creating URL Mappings with explicit event declarations so case and location can be controlled. When using implicit views you will also loose fine rendering control.
{% endhint %}

#### Disabling Implicit Views

You can also disable implicit views by using the `coldbox.implicitViews` configuration setting in your `config/ColdBox.cfc`. This is useful as implicit lookups are time-consuming.

```javascript
coldbox.implicitViews = false;
```

#### Case Sensitivity

The ColdBox rendering engine can also be tweaked to use **case-insensitive** or **sensitive** implicit views by using the `coldbox.caseSensitiveImplicitViews` directive in your `config/ColdBox.cfc`. The default is to turn all implicit views to lower case, so the value is always **false**.

```javascript
coldbox.caseSensitiveImplicitViews = true;
```

## Rendering Views

We have now seen how to set views to be rendered from our handlers. However, we can use some cool methods to render views and layouts on-demand. These methods exist in the **Renderer** and several facade methods exist in the super type so you can call it from any handler, interceptor, view or layout.

1. `renderView()`
2. `renderExternalView()`
3. `renderLayout()`

Check out the latest [API Docs](http://apidocs.ortussolutions.com/coldbox/current) for the latest arguments:

```javascript
<cfoutput>
// render inline
#renderView( 'tags/metadata' )#

// render from a module
#renderView( view="security/user", module="security" )#

// render and cache
#renderView(
    view="tags/longRendering", 
    cache=true, 
    cacheTimeout=5, 
    cacheProvider="couchbase"
)#

// render a view from the handler action
function showData(event,rc,prc){
    // data here
    return renderView( "general/showData" );    
}

// render an email body content in an email template layout
body = renderlayout( layout='email',view='templates/email_generic' );
</cfoutput>
```

{% hint style="info" %}
Inline renderings are a great asset for reusing views and doing layout compositions
{% endhint %}

### Model Rendering

If you need rendering capabilities in your model layer, we suggest using the following injection DSL:

```javascript
property name="renderer" inject="provider:coldbox:renderer";
```

This will inject a [provider](https://wirebox.ortusbooks.com/advanced-topics/providers) of a **Renderer** into your model objects. Remember that renderers are transient objects so you cannot treat them as singletons. The provider is a proxy to the transient object, but you can use it just like the normal object:

```javascript
function sendEmail(){

    // code here.
    var body = renderer.renderView( "templates/email" );

}
```

## Rendering External Views

So what if I want to render a view outside of my application without using the setting explained above? Well, you use the `renderExternalView()` method.

```javascript
<cfoutput>#renderExternalView(view='/myViewsMapping/tags/footer')#</cfoutput>
```

## Rendering With Local Variables

You can pass localized arguments to the `renderView() and renderLayout()` methods in order to encapsulate the rendering via the `args` struct argument. Much like how you make method calls with arguments. Inside of your layouts and views you will receive the same `args` struct reference as well.

This gives you great DRYness \(yes that is a word\) when building new and edit forms or views as you can pass distinct arguments to distinguish them and keep structure intact.

### Universal Form

```javascript
<h1>#args.type# User</h1>
<form method="post" action="#args.action#">
...
</form>
```

#### New Form

```javascript
#renderView(view='forms/universal',args={type='new',action='user.create'})#
```

#### Edit Form

```javascript
#renderView(view='forms/universal',args={type='edit',action='user.update'})#
```

## Rendering Collections

You have a few arguments in the `renderView()` method that deal with collection rendering. Meaning you can pass any array or query and the Renderer will iterate over that collection and render out the view as many times as the records in the colleciton.

* `collection` : A data collection that can be a query or an array of objects, structs or whatever
* `collectionAs` : The name of the variable in the variables scope that will hold the collection pivot.
* `collectionStartRow` : Defaults to 1 or your offset row for the collection rendering
* `collectionMaxRows` : Defaults to show all rows or you can cap the rendering display
* `collectionDelim` : An optional delimiter to use to separate the collection renderings. By default it is empty.

Once you call `renderView()` with a collection, the renderer will render the view once for each member in the collection. The views have access to the collection via arguments.collection or the member currently iterating. The name of the member being iterated as is by convention the same name as the view. So if we do this in any layout or simple view:

```javascript
#renderView(view='tags/comment',collection=rc.comments)#
```

Then the `tags/comment` will be rendered as many times as the collection `rc.comments` has members on it and by convention the name of the variable is comment the same as the view name.

```javascript
<h1>Title: #comment.Title# (#_counter# of #_items#</h1>
<p>Author: #comment.Author#</p>
#comment.Comment#
<hr/>
```

If you don't like that, then use the collectionAs argument:

```javascript
#renderView(view='tags/comment',collection=rc.comments,collectionAs='MyComment')#
```

So let's see the collection view now:

```javascript
<h1>Title: #MyComment.Title# (#_counter# of #_items#</h1>
<p>Author: #MyComment.Author#</p>
#MyComment.Comment#
<hr/>
```

You can see that I just call methods on the member as if I was looping \(which we are for you\). But you will also see two little variables here:

* `_counter` : A variable created for you that tells you in which record we are currently looping on
* `_items` : A variable created for you that tells you how many records exist in the collection

This will then render that specific dynamic HTML view as many times as their are records in the rc.comments array and concatenate them all for you. In my case, I separate each iteration with a simple but you can get fancy and creative.

```javascript
#renderView(view="home/news", collection=prc.news, collectionStartRow=11, collectionMaxRows=20)#
```

## View Caching

You can also pass in the caching arguments below and your view will be rendered once and then cached for further renderings. Every ColdBox application has two active cache regions: `default and template`. All view and event caching renderings go into the `template` cache.

| Argument | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| cache | boolean | false | false | Cache the view to be rendered |
| cacheTimeout | numeric | false | \(provider default\) | The timeout in minutes or whatever the cache provider defines |
| cacheLastAccessTimeout | numeric | false | \(provider default\) | The idle timeout in minutes or whatever the cache provider defines |
| cacheSuffix | string | false | --- | Adds a suffix key to the cached key. Used for providing uniqueness to the cacheable entry |

```javascript
component name="general"{

    function index(event,rc,prc){

        // call some model for data and put into the request collection
        prc.myQuery = getInstance('MyService').getData();    
        // view with caching parameters
        event.setView(
            view="general/index",
            cache=true,
            cacheTimeout=60,
            cacheLastAccessTimeout=15,
            cacheSuffix=getfwLocale()
        );
    }

}
```

### Purging Views

So now that our views are cached, how do I purge them programmatically? Well, you need to talk to the `template` cache provider and use the clearing methods:

```javascript
// get a reference to the template cache
cache = cachebox.getCache( 'template' );
// or via shortcut notation
cache = getCache( "template" );
// or injection
property name="cache" inject="cachebox:template";
```

Then we can perform several operations on views:

* `clearView(string viewSnippet)`: Used to clear a view from the cache by using a snippet matched according to name + cache suffix.
* `clearMultiView(any viewSnippets)`: Clear using a list or array of view snippets.
* `clearAllViews([boolean async=true])` : Can clear ALL cached views in one shot and can be run asynchronously.

```javascript
cachebox.getCache( 'template' ).clearView('general/index');
cachebox.getCache( 'template' ).clearAllViews(async=true);
cachebox.getCache( 'template' ).clearMultiView('general/index','index','home');
```

### Disable View Caching

To turn off view caching for your entire application, set the `viewCaching` setting to false in your `config/Coldbox.cfc` config file.

```javascript
coldbox = {
// Activate view caching
viewCaching = false
}
```

## View Helpers

This is a nifty little feature that enables you to create nice helper templates on a per-view, per-folder and per-application basis. If the framework detects the helper, it will inject it into the rendering view so you can use methods, properties or whatever. All you need to do is follow a set of conventions. Let's say we have a view in the following location:

```javascript
/views
  /general
    +home.cfm
```

Then we can create the following templates

* `homeHelper.cfm` : Helper for the home.cfm view.
* `generalHelper.cfm` : Helper for any view in the general folder.

```javascript
/views
  /general
    +home.cfm
    +homeHelper.cfm
    +generalHelper.cfm
```

**homeHelper.cfm**

```javascript
<cfscript>
// facade to get i18n resource
function $r(){ return getResource(argumentCollection=arguments); }
// cool formatted date function
function today(){ return dateFormat(now(),"full"); }
</cfscript>
```

That's it. Just append Helper to the view or folder name and there you go, the framework will use it as a helper for that view specifically. What can you put in these helper templates:

* NO BUSINESS CODE
* UI logic functions or properties
* Helper functions or properties
* Dynamic JavaScript or CSS

> **Hint** External views can also use our helper conventions

### Application wide helpers

You can also use the `coldbox.viewsHelper` directive to tell the framework what helper file to use for ALL views and layouts rendered:

```text
coldbox.viewsHelper = "includes/helpers/viewsHelper.cfm;
```

## View Events

All rendered views have associated events that are announced whenever the view is rendered via ColdBox Interceptors. These are great ways for you to be able to intercept when views are rendered and transform them, append to them, or even remove content from them in a completely decoupled approach. The way to listen for events in ColdBox is to write Interceptors, which are essential simple CFC's that by convention have a method that is the same name as the event they would like to listen to. Each event has the capability to receive a structure of information wich you can alter, append or remove from. Once you write these Interceptors you can either register them in your Configuration File or programmatically.

| Event | Data | Description |
| :--- | :--- | :--- |
| preViewRender | view - The name of the view to render | Executed before a view is about to be rendered |
| postViewRender | All of the data above plus: | Executed after a view was rendered |

> **Caution** You can disable the view events on a per-rendering basis by passing the `prePostExempt` argument as true when calling `renderView()` methods.

### Sample Interceptor

Here is a sample interceptor that trims any content before it is renderer:

```javascript
component{

    function postViewRender(event,interceptData){
        interceptData.renderedView = trim( interceptData.renderedView );
    }
}
```

Of course, I am pretty sure you will be more creative than that!

## Layouts

A layout is simply an HTML file that acts as your shell where views can be rendered in and exists in the `layouts` folder of your application. The layouts can be composed of multiple views and one main view. The main view is the view that gets set in the request collection during a request via `event.setView()` usually in your handlers or interceptors.

You can have as many layouts as you need in your application and they are super easy to override or assign to different parts of your application. Imagine switching content from a normal HTML layout to a PDF or mobile layout with one line of code. How cool would that be? Well, it's that cool with ColdBox. Another big benefit of layouts, is that you can see the whole picture of the layout, not the usual cfmodule calls where tables are broken, or divs are left open as the module wraps itself around content. The layout is a complete html document and you basically describe where views will be rendered. Much easier to build and simpler to maintain.

> **Info** Another important aspect of layouts is that they can also consume other layouts as well. So you can create nested layouts very easily via the `renderLayout()` method.

## Basic Layouts

```markup
<cfoutput>
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>#prc.title#</title>
</head>
<body>
  <!--- Header: Direct Render --->
  #renderView( view='tags/header')#

  <div id="content">
    <!--- Render set view --->
    #renderView()#
  </div>

  #renderView( view='tags/footer' )#
</body>
</html>
</cfoutput>
```

## Default Layout

### Default Layout

The default layout in a ColdBox application is `layouts/main.cfm` by convention. You can change this by using the `layoutSettings` in your `Configuration.cfc`.

```javascript
//Layout Settings
layoutSettings = {
    defaultLayout = "basic.cfm"
}
```

### Default View

There is no default view in ColdBox, but you can configure one by using the same `layoutSettings` configuration directive and the `defaultView` directive:

```javascript
//Layout Settings
layoutSettings = {
    defaultLayout = "basic.cfm",
    defaultView   = "noview.cfm"
}
```

This means that if no view is set in the request context, ColdBox will fallback to this view for rendering.

## Nested Layouts

You can also wrap layouts within other layouts and get incredible reusability. This is accomplished by using the `renderLayout()` method in the Renderer. As always, refer to the CFC API for the latest method arguments and capabilities.

```javascript
renderLayout([any layout], [any module=''], [any view=''], [struct args={}], [any viewModule=''], [boolean prePostExempt='false'])
```

So if I wanted to wrap my basic layout in a PDF wrapper layout \(`pdf.cfm`\) I could do the following:

```javascript
<cfdocument pagetype="letter" format="pdf">

    <---  Header --->
    <cfdocumentitem type="header">
    <cfoutput>
    <div>
    #dateformat(now(),"MMM DD, YYYY")# at #timeFormat(now(),"full")#
    </div>
    </cfoutput>
    </cfdocumentitem>

    <---  Footer --->
    <cfdocumentitem type="footer">
    <cfoutput>
    <div>
    Page #cfdocument.currentpagenumber# of #cfdocument.totalpagecount#
    </div>
    </cfoutput>
    </cfdocumentitem>

    <---  Main Content via nested layout --->
    #renderLayout(layout="basic")#

</cfdocument>
```

That's it! The `renderLayout()` method is extremely power as it can allow you to not only nest layouts but actually render a-la-carte layout/view combinations also.

## Overriding Layouts

Ok, now that we have started to get funky, let's keep going. How can I change the layout on the fly for a specific view? Very easily, using yet another new method from the event object, called `setLayout()` or the `layout` argument to the `setView()` method.

```javascript
event.setLayout( name );
event.setView( view="", layout=name );
```

This is great, so we can change the way views are rendered on the fly programmatically. We can switch the content to a PDF in an instant. So let's do that

```javascript
function home(event,rc,prc){

    if( event.valueExists('print') ){
        event.setLayout('layout.PDF');
    }

    // logic here

    // set view
    event.setView('general/home');

}
```

## Layouts From A Module

If you need the set layout to be rendered from a specific module then use the `module` argument from the `setLayout() or renderLayout()` methods:

```javascript
component name="general"{

    function index(event,rc,prc){

        // call some model for data and put into the request collection
        prc.myQuery = getInstance('MyService').getData();
        // set the view for rendering
        event.setLayout( name="admin", module="contentbox" );

    }

}
```

## Layout Helpers

Just like views, layouts can also have helpers on a per-layout, per-layout-folder or per-application basis. If the framework detects the helper, it will inject it into the rendering layout so you can use methods, properties or whatever. All you need to do is follow a set of conventions. Let's say we have a layout in the following location:

```javascript
/layouts
  /general
    +main.cfm
```

Then we can create the following templates

* `mainHelper.cfm` : Helper for the `main.cfm` layout.
* `generalHelper.cfm` : Helper for any layout in the `general` folder.

```javascript
/layouts
  /general
    +main.cfm
    +mainHelper.cfm
    +generalHelper.cfm
```

That's it. Just append Helper to the layout or folder name and there you go, the framework will use it as a helper for that layout specifically.

> **Caution** Please note that layout helpers will be inheritenly available to any view rendered inside of the layout.

### Application wide helpers

You can also use the `coldbox.viewsHelper` directive to tell the framework what helper file to use for ALL layouts rendered:

```text
coldbox.viewsHelper = "includes/helpers/viewsHelper.cfm;
```

## Layout Events

All rendered layouts have associated events that are announced whenever the layout is rendered via ColdBox Interceptors. These are great ways for you to be able to intercept when layouts are rendered and transform them, append to them, or even remove content from them in a completely decoupled approach. The way to listen for events in ColdBox is to write Interceptors, which are essential simple CFC's that by convention have a method that is the same name as the event they would like to listen to. Each event has the capability to receive a structure of information wich you can alter, append or remove from. Once you write these Interceptors you can either register them in your Configuration File or programmatically.

| Event | Data | Description |
| :--- | :--- | :--- |
| `preLayoutRender` | layout - The name of the layout to render | Executed before any layout is rendered |
| `postLayoutRender` | All of the data above plus: | Executed after a layout was rendered |

> **Caution** You can disable the layout events on a per-rendering basis by passing the `prePostExempt` argument as true when calling `renderLayout()` methods.

## Implicit Layout-View Declarations

Now that we have seen what layouts and views are, where they are located and some samples, let's dig deeper. Let's discover the power of implicit layout/view declarations:

```javascript
//Register Layouts
layouts = [
    { name = "login",
       file = "Layout.tester.cfm",
      views = "vwLogin,test",
      folders = "tags,pdf/single"
    }
];
```

This `layouts` setting allows you to implicitly define layout to view/folder assignments without the need of programmatically doing it. This is a time saver and a nice way to pre-define how certain views will be rendered. Let's see more examples:

```javascript
//Register Layouts
layouts = [
    { name = "Popup",
       file = "Layout.Popup.cfm",
      views = "login,test",
      folders = "^tags,pdf/single"
    },
    { name = "help",
      file = "layout.help.cfm",
      folders = "^help"
    }
];
```

In the sample, we declare the layout named `popup` and points to the file `Layout.Popup.cfm`. We can then assign it to views or folders:

* `Views` : The views to assign to this layout \(no cfm extension\)
* `Folders` : The folders and its children to assign to this layout. \(regex ok\)

This is cool, we can tell the framework that some views and some folders should be rendered within a specific layout. Wow, this opens the possibility of creating nested applications that need different rendering schemas!

## Helpers UDF's

ColdBox provides you with a way to actually inject your layouts/views with custom UDF's, so they can act as helpers. This is called mixin methods and can be done via the `includeUDF()` method in the supertype or via the following settings:

* `applicationHelper` - Global helper for layouts, views, and handlers
* `viewsHelper` - Global helper for all layouts and views only

```javascript
coldbox.applicationHelper = 'includes/helpers/applicationHelper.cfm';
coldbox.viewsHelper = "includes/helpers/viewsHelper.cfm";
```

Additionally, `applicationHelper` can accept a list or array of helper files and will include each of them.

> **Caution** If you try to inject a method that already exists, the call will fail and the CFML engine will throw an exception. Also, try not to abuse mixins, if you have too many consider refactoring into model objects or plugins.

## ColdBox Elixir

### ColdBox Elixir

ColdBox Elixir provides a clean, fluent API for defining basic [Gulp](http://gulpjs.com/) tasks for your ColdBox applications. This project was forked from the [Laravel Elixir](https://github.com/laravel/elixir) project, so many many thanks for all their hard work and ideas. Please note that ColdBox does not ship with Elixir. It is an addon library based on Nodejs to help you with your asset pipeline.

#### How it works

Elixir supports several common CSS, JavaScript pre-processors, and TestBox runner integrations. By leveraging your familiar `Gulpfile.js` configuration file, you can use method chaining and Elixir will allow you to fluently define your asset pipeline using the ColdBox conventions.

It works on the premise of two location convetions for your static assets:

* `includes` - Where your css/js will be placed after the pipeline executes
* `resources/assets` - Where all your resource files exist.

For example:

```javascript
var elixir = require( 'coldbox-elixir' );
elixir( function( mix ) {
    // Look in the 'resources/sass' folder
    mix.sass( 'app.scss' )
        // Look in the 'resourcess/css` folder
       .styles( 'modules.css' );

   // Elixir will then output all assets to ColdBox 'includes' folder by convention.
});
```

If you've ever been confused about how to get started with Gulp and asset compilation, you will love ColdBox Elixir. However, you are not required to use it while developing your application. You are free to use any asset pipeline tool you wish, or even none at all.

> **Tip** : ColdBox Elixir supports EcmaScript6, JSX syntax, LESS, SASS, babel, browserify, vueify, partialify and much more. So take advantage!

### Installation & Setup

#### Installing Node

Before triggering Elixir, you must first ensure that [Node.js](https://nodejs.org/en/) is installed on your machine.

```text
node -v
```

#### Installing Gulp

Next, you'll want to pull in Gulp as a global NPM package so you have it available for any ColdBox application.

```text
npm install --g gulp
```

If you use a version control system, you may wish to run the npm [shrinkwrap](https://docs.npmjs.com/cli/shrinkwrap) to lock your NPM requirements:

```text
npm shrinkwrap
```

Once you have run this command, feel free to commit the `npm-shrinkwrap.json` into source control.

#### Installing ColdBox Elixir

The only remaining step is to install Elixir! If you generate a ColdBox application using any of our [elixir templates](https://github.com/coldbox-templates/) then you will find a `package.json` file in the root of the application already. Think of this like your `box.json` file, except it defines Node dependencies instead of ColdFusion \(CFML\) dependencies. A typical example can look like this:

```javascript
{
  "private": true,
  "devDependencies": {
    "gulp": "^3.9.1"
  },
  "dependencies": {
    "bootstrap-sass": "^3.3.6",
    "coldbox-elixir": "^1.1.0",
    "jquery": "^2.2.3"
  }
}
```

It defines ColdBox Elixir, Bootstrap and jQuery as your dependencies. You may then install the dependencies it references by running:

```text
npm install
```

This will install ColdBox Elixir, Bootstrap and jQuery into the `node_modules` folder in your root. This folder has been already added to the `.gitignore` file as well, so no need to further ignore it.

> **Note** : If you are developing on a Windows system or you are running your VM on a Windows host system, you may need to run the `npm install` command with the `--no-bin-links` switch enabled: `npm install --no-bin-links`
>
> **Tip** : If you are integrating with Vue.js, please see our [Vue.js](https://coldbox-elixir.ortusbooks.com/vue.js-integration) section

### Running Elixir

Elixir is built on top of Gulp, so to run your Elixir tasks you only need to run the `gulp` command in your terminal, as you will already have a `Gulpfile.js` in your project root that will resemble the following:

```javascript
var elixir = require( 'coldbox-elixir' );

/*
 |--------------------------------------------------------------------------
 | Elixir Asset Management
 |--------------------------------------------------------------------------
 |
 | Elixir provides a clean, fluent API for defining some basic Gulp tasks
 | for your ColdBox application. By default, we are compiling the Sass
 | file for our application, as well as publishing vendor resources.
 |
 */

elixir( function( mix ){

} );
```

Please note that by adding the `--production` flag to the command will instruct Elixir to minify your CSS and JavaScript files:

```bash
// Run all tasks...
gulp

// Run all tasks and minify all CSS and JavaScript...
gulp --production
```

#### Watching Assets For Changes

Since it is inconvenient to run the gulp command on your terminal after every change to your assets, you may use the `gulp watch` command. This command will continue running in your terminal and watch your assets for any changes. When changes occur, new files will automatically be compiled or tested:

```text
gulp watch
```

#### Further Documentation

ColdBox Elixir is fully documented here: [https://coldbox-elixir.ortusbooks.com](https://coldbox-elixir.ortusbooks.com). So please head on over there to get a deeper perspective of our asset pipeline manager.

## Models

ColdBox allows you to integrate with the model layer easily by leveraging **WireBox** as your default dependency injection framework. However, you can integrate your model layer with any third-party DI framework via our `cbioc` module \([https://github.com/ColdBox/cbox-ioc](https://github.com/ColdBox/cbox-ioc)\) as well.

### Spaghetti Evolution

Remember this:

Did you get some spine shivers like I just did. WOW! That is the traditional spaghetti coding style. With ColdBox we are now moving into MVC land and focusing on the model layer.

However, the model layer can even be subdivided into many layers as well as we will investigate in this section.

### WireBox

WireBox, our dependency injection and AOP framework, will do all the magic of building, wiring objects with dependencies and helping you persist objects in some state \(singletons, transients, request, etc\). The main purpose for model integration is to make the developer's development workflow easier! And we all like that Easy button!

### Benefits

* Easily create and retrieve model objects by using one method: `getInstance()`
* Easily handle model or handler dependencies by using `cfproperty` and constructor argument conventions.
* An Injection DSL \(Domain Specific Language\) has been created to facilitate dependencies
* Easily determine what scope model objects should be persisted in: Transients, Singletons, Cache, Request, Session, etc.
* Easily create a configuration binder to create aliases or complex object relationships \(Java, WebServices, RSS, etc.\)
* Easily do FORM data binding or advanced ORM data binding
* Easily compose ORM relationships from request data

Wow! You can do all that? Yes and much more. So let's begin.

## Domain Modeling

The Model layer represents your data structures and business logic. It is the domain-specific representation of the information that the application operates on. Many applications also use a persistent storage mechanism \(such as a database\) to store and retrieve data. MVC does not specifically mention the data access layer because it is understood to be underneath or encapsulated by the Model Layer. This is the most important part of your application and it is usually modeled by ColdFusion components \(CFCs\). You can even create the entire model layer in another language or physical location \(Java, web services\).

All you need to understand is that this layer is the layer that runs the logic show! For the following example, I highly encourage you to also do [UML modeling](http://en.wikipedia.org/wiki/Unified_Modeling_Language), so you can visualize class relationships and design.

### UML Resources

There are tons of great UML resources and tools. Here are some great tools for you:

* Sparx Systems Enterprise Architect - [http://www.sparxsystems.com/products/ea/index.html](http://www.sparxsystems.com/products/ea/index.html)
* ColdFusion generation templates for Enterprise Architect - [https://github.com/lmajano/EA-ColdFusion-CodeGeneration](https://github.com/lmajano/EA-ColdFusion-CodeGeneration)
* ArgoUML - [http://argouml.tigris.org/](http://argouml.tigris.org/)
* Poseidon UML - [http://www.gentleware.com/](http://www.gentleware.com/)
* Learning UML - [http://shop.oreilly.com/product/9780596009823.do](http://shop.oreilly.com/product/9780596009823.do)
* UML in a nutshell - [http://shop.oreilly.com/product/9780596007959.do?green=87D3081D-5A0D-50F7-9757-95B7E8779516&cmp=af-mybuy-9780596007959.IP](http://shop.oreilly.com/product/9780596007959.do?green=87D3081D-5A0D-50F7-9757-95B7E8779516&cmp=af-mybuy-9780596007959.IP)

### ORM Extensions

Since some of the examples in this section require the usage of ColdFusion ORM and the ColdBox ORM Extensions module \([https://github.com/ColdBox/cbox-cborm](https://github.com/ColdBox/cbox-cborm)\), let's use CommandBox to install them in our application in two easy steps:

**1. Install Module**

```text
install cborm
```

**2. Add Mapping**  
Open your `Application.cfc` and add a mapping to this module:

```javascript
this.mappings[ "/cborm" ]     = COLDBOX_APP_ROOT_PATH & "modules/cborm";
```

Unfortunately, we cannot use the ColdBox Module CFMappings to do this because the ColdFusion ORM loads before anything in our system.

### Validation

ColdBox has a core validation module you can install to provide you with robust server-side validation of model data. You can install it via CommandBox:

```bash
install cbvalidation
```

You can find the documentation and source in its repository: [https://github.com/ColdBox/cbox-validation](https://github.com/ColdBox/cbox-validation)

### Book Catalog

Let's say that I want to build a simple book catalog and I want to be able to do the following:

* List how many books I have
* Search for a book by name
* Add Books
* Remove Books
* Update Books 

There are several ways I can go about this and your design will depend on the tools you use. If you use an ORM like ColdFusion ORM you most likely will use either an [ActiveEntity](http://wiki.coldbox.org/wiki/ORM:ActiveEntity.cfm) approach or build [virtual service layers](http://wiki.coldbox.org/wiki/ORM:VirtualEntityService.cfm) or build a service layer based on our [Base ORM services](http://wiki.coldbox.org/wiki/ORM:BaseORMService.cfm). If you are not using an ORM, then most likely you will have to build all object, service and data layers manually. We will concentrate first on the last approach in order to do our modeling and then build them out in different ways.

## Service Layer

I want to apply best practices and use a service layer approach for my application and model design. I will then use these service objects in my handlers in order to handle the business logic for me. Repeat after me:

> I WILL NOT PUT BUSINESS LOGIC IN EVENT HANDLERS!

The whole point of the model layer is that it is separate from the other 2 layers \(controller and views\). Remember, the model is supposed to live on its own and not be dependent on external layers \(Decoupled\). Decoupling facilitates reuse. From these simple requirements I will create the following classes:

* `BookService.cfc` - A service layer for book operations
* `Book.cfc` - Represents a book in my system

### What is a service layer?

> A Service Layer defines an application's boundary \[Cockburn PloP\] and its set of available operations from the perspective of interfacing client layers. It encapsulates the application's business logic, controlling transactions and coordinating responses in the implementation of its operations. [Martin Fowler](http://martinfowler.com/eaaCatalog/serviceLayer.html)

A service layer approach is a way to architect enterprise applications in which there is a layer that acts as a service or mediator to your domain models, data layers and so forth. This layer is the one that event handlers or remote ColdBox proxies can talk to in order to interact with the domain model. There are basically two approaches when building a service layer:

1. Designing the services around functionality that might encompass several domain model objects. This is our preferred approach as it creates a more rich service layer and you will not have class explosion.
2. Designing a service for each business object, which in turn matches a table or set of tables in the permanent storage. This approach is easy to follow, but the consequences are the responsibilities that could be grouped are not and you will end up with class explosion.

The best way to determine the most effective solution is to actually try both approaches. Once you design them and put them in practice, you will find your preference. I want to concentrate and challenge you to try these approaches out and learn from your experiences. I believe there is NO SILVER BULLET on OO design, just stick to best practices and practice "code smell", which is the art of knowing when your code is not right and therefore smells nasty!

### The Book Services

![](https://github.com/ortus-docs/coldbox-docs/tree/97b8636ca1e8f4651f1021343c097bb3a7c2e9b9/.gitbook/assets/MVC%2BORM.png)

The `BookService` object will be my API to do operations as mentioned in my requirements and this is the object that will be used by my handlers. My `Book` object will model a Book's data and behavior. It will be produced, saved and updated by the `BookService` object and will be used by event handlers in order to populate and validate them with data from the user.

The view layer will also use the `Book` object in order to present the data. As you can see, the event handlers are in charge of talking to the Domain Model for operations/business logic, controlling the user's input requests, populating the correct data into the `Book` model object and making sure that it is sent to the book service for persistence.

## Data Layers

![](https://github.com/ortus-docs/coldbox-docs/tree/97b8636ca1e8f4651f1021343c097bb3a7c2e9b9/.gitbook/assets/MVC%2Bobjects.png)

If I know that my database operations will get very complex or I want to further add separation of concerns, I could add a third class to the mix: `BookGateway.cfc` or `BookDAO.cfc` that could act as my data gateway object.

Now, there are so many design considerations, architectural requirements and even types of service layer approaches that I cannot go over them and present them. My preference is to create service layers according to my application's functionality \(Encompasses 1 or more persistence tables\) and create gateways or data layers when needed, especially when not using an ORM. The important aspect here, is that I am thinking about my project's OO design and not how I will persist the data in a database. This, to me, is key!

Understanding that I am more concerned with my object's behavior than how will I persist their data will make you concentrate your efforts on the business rules, model design and less about how the data will persist. Don't get me wrong, persistence takes part in the design, but it should not drive it.

## Book

So what can `Book.cfc` do. It can have the following private properties:

* name
* id
* createdata
* ISBN
* author
* publishDate

It can then have getters/setters for each property that I want to expose to the outside world, remember that objects should be shy. Only expose what needs to be exposed. Then I can add extra functionality or behavior as needed. You can do things like:

* have a method that checks if the publish date is within a certain amount of years
* have a method that can output the [ISBN](http://www.amazon.com/exec/obidos/ASIN/) number in certain formats
* have a method that can output the publish date in different formats and locales
* make the object save itself or persist itself \(Active Record\)
* and so much more

Now, all you OO gurus might be saying, why did he leave the author as a string and not represented by another object. Well, because of simplicity. The best practice, or that code smell you just had, is correct. The author should be encapsulated by its own model object Author that can be aggregated or used by the Book object. I will not get into details about object aggregation and composition, but just understand that if you thought about it, then you are correct. Moving along...

> **Important** Your objects are not always supposed to be dumb, or just have getters and setters \(Anemic Model\). Enrich them please!

### Book Service

Back to the book service object. This service will need a datasource name \(which could be encapsulated in a datasource object\) in order to connect to the database and persist stuff. It might also need a table prefix to use \(because I want to\) and it comes from a setting in my application's configuration file. Ok, so now we know the following dependencies or external forces:

* A datasource \(as an object or string\)
* A setting \(as a string\)

I can also now think of a few methods that I can have on my book service:

* `getBook([id:string])`:Book This method will create or retrieve a book by id.
* `searchBook(criteria:string)`:query This method can return a query or array of Books if needed
* `saveBook(book:Book)` Save or Update a book
* `deleteBook(book:Book)` Delete a book

I recommend you model these class relationships in [UML class diagrams](http://www.agilemodeling.com/artifacts/classDiagram.htm) to get a better feeling of the design. Anyways, that's it, we are doing domain modeling. We have defined a domain object called `Book` and a companion `BookService` object that will handle book operations.

Now once you build them and UNIT TEST THEM, yes UNIT TEST THEM. Then you can use them in your handlers in order to interact with them. As you can see, most of the business rules and logic are encapsulated by these domain objects and not written in your event handlers. This creates a very good design for portability, sustainability and maintainability. So let's start actually seeing how to write all of this instead of imagining it. Below you can see a more complete class diagram of this simple example.

> **Hint** Note that sometimes the design in UML will not reflect the end product. UML is our guide but not the final product.

## Conventions Location

All your model objects will be located under your `models` folder of your application root by convention. For a more secure installation, place your models outside of the web root and use a ColdFusion Mapping or WireBox Mapping to access them.

## WireBox Binder

You can have an optional WireBox configuration binder that can fine-tune the WireBox engine and also where you can create object mappings, and even more model locations by convention. Usually you will find this binder by convention in your `config/WireBox.cfc` location and it looks like this:

```javascript
component extends="coldbox.system.ioc.config.Binder"{

    function configure(){

        // Configure WireBox
        wireBox = {
            // Scope registration, automatically register a wirebox injector instance on any CF scope
            // By default it registeres itself on application scope
            scopeRegistration = {
                enabled = true,
                scope   = "application", // server, cluster, session, application
                key        = "wireBox"
            },

            // Custom DSL Namespace registrations
            customDSL = {
                // namespace = "mapping name"
            },

            // Custom Storage Scopes
            customScopes = {
                // annotationName = "mapping name"
            },

            // Package scan locations or model external locations by convention
            scanLocations = [],

            // Stop Recursions
            stopRecursions = [],

            // Parent Injector to assign to the configured injector, this must be an object reference
            parentInjector = "",

            // Register all event listeners here, they are created in the specified order
            listeners = [
                // { class="", name="", properties={} }
            ]            
        };

        // Map Bindings below
    }    
}
```

Please refer to the full Binder documentation: \([http://wirebox.ortusbooks.com/configuration/configuring-wirebox/](http://wirebox.ortusbooks.com/configuration/configuring-wirebox/)\) for further inspection.

### Mappings

By default, all objects that you place in the `models` folder are available to your application by their name. So if you create a new model object: `models/MyService.cfc`, you can refer to it as `MyService` in your application. However, if you create a model object: `models/security/SecurityService.cfc` it will be available as `security.SecurityService`. This is great and dandy, but when refactoring comes to play you will have to refactor all references to the dot-notation paths. This is where mappings come into play.

WireBox has several methods for mappings, the easiest of them are the following:

* `map( alias ).to( cfc.path )`
* `mapPath( path )`
* `mapDirectory( packagePath, include, exclude, influence, filter, namespace )`

```javascript
// map the service with an alias.
map( "SecurityService" ).to( "models.security.SecurityService" );

// map the entire models directory by CFC name as the alias
mapDirectory( "models" );
```

## Super Type Usage Methods

The super type offers 2 methods for interacting with your model layer:

* `getInstance()` - Retrieve a model object \(Instead of injection\)
* `populateModel()` - Retrieve and/or populate a model object from the request collection.

### getInstance\(\)

Here is the signature

```javascript
/**
* Get a model object
* @name.hint The mapping name or CFC path to retrieve
* @dsl.hint The DSL string to use to retrieve an instance
* @initArguments.hint The constructor structure of arguments to passthrough when initializing the instance
* @targetObject.hint The object requesting the dependency, usually only used by DSL lookups
*/
function getInstance( name, dsl, initArguments={}, targetObject="" ){}
```

**Examples**

```javascript
// Retrieve the User.cfc in the model folder
var oUser = getInstance('User');
// Retrieve the User.cfc in the model/users folder
var oUser = getInstance("users.User")
// Retrieve the User using an alias you mapped in your configuration binder
var oUser = getInstance("MyUser");
// Retrieve an object using a full instantation path
var oUtil = getInstance("mypath.utilities.MyUtil");
```

### populateModel\(\)

ColdBox can populate or bind model objects from data in the request collection by matching the name of the form element to the name of a property on the object. You can also populate model objects from JSON, XML, Queries and other structures a-la-carte by talking directly to [WireBox](http://wirebox.ortusbooks.com/content/wirebox_object_populator/index.html)'s object populator.

```javascript
/**
* Populate a model object from the request Collection or a passed in memento structure
* @model.hint The name of the model to get and populate or the acutal model object. If you already have an instance of a model, then use the populateBean() method
* @scope.hint Use scope injection instead of setters population. Ex: scope=variables.instance.
* @trustedSetter.hint If set to true, the setter method will be called even if it does not exist in the object
* @include.hint A list of keys to include in the population
* @exclude.hint A list of keys to exclude in the population
* @ignoreEmpty.hint Ignore empty values on populations, great for ORM population
* @nullEmptyInclude.hint A list of keys to NULL when empty
* @nullEmptyExclude.hint A list of keys to NOT NULL when empty
* @composeRelationships.hint Automatically attempt to compose relationships from memento
* @memento A structure to populate the model, if not passed it defaults to the request collection
* @jsonstring If you pass a json string, we will populate your model with it
* @xml If you pass an xml string, we will populate your model with it
* @qry If you pass a query, we will populate your model with it
* @rowNumber The row of the qry parameter to populate your model with
*/
function populateModel()
```

**Examples**:

```javascript
var user = ormService.populate( ormService.new("User"), data );

// populate with includes only
var user = ormService.populate( ormService.new("User"), data, "fname,lname,email" );

//populate with excludes
var user = ormService.populate(target=ormService.new("User"),memento=data,exclude="id,setup,total" );

// populate with null values when value is empty string
var user = ormService.populate(target=ormService.new("User"),memento=data,nullEmptyInclude="lastName,dateOfBirth" );

// populate many-to-one relationship
var data = {
    firstName = "Luis",
    role = 1 // "role" is the name of the many-to-one relational property, and one is the key value
};
var user = ormService.populate( target=ormService.new("User"), memento=data, composeRelationships=true );
// the role relationship will be composed, and the value will be set to the appropriate instance of the Role model

// populate one-to-many relationship
var data = {
    firstName = "Luis",
    favColors = "1,2,3" ( or [1,2,3] ) // favColors is the name of the one-to-many relational property, and 1, 2 and 3 are key values of favColor models
};
var user = ormService.populate( target=ormService.new("User"), memento=data, composeRelationships=true );
// the favColors property will be set to an array of favColor entities

// only compose some relationships
var data = {
    firstName = "Luis",
    role = 1,
    favColors = [ 1, 3, 19 ]
};
var user = ormService.populate( target=ormService.new("User"), memento=data, composeRelationships=true, exclude="favColors" );
// in this example, "role" will be composed, but "favColors" will be excluded
```

## Injection DSL

Before we start building our objects, we need to understand how WireBox injects dependencies for us. You can define injections using the configuration inside the binder \(like any other DI framework\), but the easiest approach is to use our injection annotation and conventions \(called the injection DSL\). The injection DSL can be applied to `cfproperty`, `cfargument`, `cffunction` or called via `getInstance()` as we saw previously.

You can utilize the injection DSL by adding an attribute called `inject` to properties, arguments and functions. This attribute is like your code shouting, "**Hey, I need something here. Hello! I need something!**" That something might be another object, setting, cache, etc. The value of the `inject` attribute is a string that represents the concept of retrieving an object mapped in WireBox. It can be an object path, an object ID, a setting, a datasource, custom data, etc.

> **Info:** If you don't like annotations or find them obtrusive, you don't have to use them :\). Just leverage the WireBox binder to define dependencies instead.

Here are a few examples showing how to apply the injection DSL.

Property Annotation

```javascript
property name="service" inject="MyService";
```

Constructor Argument Annotation

```text
<cfargument name="setting" inject="MyService">
```

```javascript
/**
* @myDAO.inject model:MyDAO
*/
function init(myDAO){
}
```

Setter Method Annotation

```javascript
function setMyservice(MyService) inject="MyService"{
    variables.MyService = arguments.MyService;
}
```

You can learn more about the supported injection DSLs in [the WireBox Injection DSL documentation](http://wirebox.ortusbooks.com/content/injection_dsl/index.html).

## Model Object Namespace

The default namespace is not specifying one. This namespace is used to retreive either named mappings or full component paths.

| DSL | Description |
| :--- | :--- |
| empty | Same as saying _id_. Get a mapped instance with the same name as defined in the property, argument or setter method. |
| id | Get a mapped instance with the same name as defined in the property, argument or setter method. |
| id:{name} | Get a mapped instance by using the second part of the DSL as the mapping name. |
| id:{name}:{method} | Get the {name} instance object, call the {method} and inject the results |
| model | Get a mapped instance with the same name as defined in the property, argument or setter method. |
| model:{name} | Get a mapped instance by using the second part of the DSL as the mapping name. |
| model:{name}:{method} | Get the {name} instance object, call the {method} and inject the results |

```javascript
// Let's assume we have mapped a few objects called: UserService, SecurityService and RoleService

// Empty inject, use the property name, argument name or setter name
property name="userService" inject;

// Using the name of the mapping as the value of the inject
property name="security" inject="SecurityService";

// Using the full namespace
property name="userService" inject="id:UserService";
property name="userService" inject="model:UserService";

// Simple factory method
property name="roles" inject="id:RoleService:getRoles";
```

## ColdBox Namespace

Whenever your models need anything from the ColdBox application then you can leverage the `coldbox:` namespace for injections.

| DSL | Description |
| :--- | :--- |
| `coldbox` | Get the coldbox controller reference |
| `coldbox:flash` | Get a reference to the application's flash scope object |
| `coldbox:setting:{setting}` | Get the coldbox application _{setting}_ setting and inject it |
| `coldbox:fwSetting:{setting}` | Get a ColdBox setting _{setting}_ and inject it |
| `coldbox:setting:{setting}@{module}` | Get the coldbox application _{setting}_ from the _{module}_ and inject it |
| `coldbox:loaderService` | Get a reference to the loader service |
| `coldbox:requestService` | Get a reference to the request service |
| `coldbox:handlerService` | Get a reference to the handler service |
| `coldbox:interceptorService` | Get a reference to the interceptor service |
| `coldbox:moduleService` | Get a reference to the ColdBox Module Service |
| `coldbox:renderer` | Get the ColdBox rendering engine reference |
| `coldbox:dataMarshaller` | Get the ColdBox data marshalling reference |
| `coldbox:interceptor:{name}` | Get a reference of a named interceptor _{name}_ |
| `coldbox:configSettings` | Get the application's configuration structure |
| `coldbox:fwSettings` | Get the framework's configuration structure |
| `coldbox:fwSetting:{setting}` | Get a setting from the ColdBox settings instead of the Application settings |
| `coldbox:moduleSettings:{module}` | Inject the entire _{module}_ settings structure |
| `coldbox:moduleConfig:{module}` | Inject the entire _{module}_ configurations structure |

```javascript
// some examples
property name="moduleService" inject="coldbox:moduleService";
property name="producer" inject="coldbox:interceptor:MessageProducer";
property name="appPath" inject="coldbox:fwSetting:ApplicationPath";
```

## CacheBox Namespace

This DSL namespace is only active if using CacheBox or a ColdBox application context.

| DSL | Description |
| :--- | :--- |
| cachebox | Get a reference to the application's CacheBox instance |
| cachebox:{name} | Get a reference to a named cache inside of CacheBox |
| cachebox:{name}:{objectKey} | Get an object from the named cache inside of CacheBox according to the objectKey |

```javascript
property name="cacheFactory" inject="cacheBox";
property name="cache" inject="cachebox:default";
property name="data" inject="cachebox:default:myKey";
```

## LogBox Namespace

This DSL namespace interacts with the loaded LogBox instance.

| DSL | Description |
| :--- | :--- |
| logbox | Get a reference to the application's LogBox instance |
| logbox:root | Get a reference to the root logger |
| logbox:logger:{category name} | Get a reference to a named logger by its category name |
| logbox:logger:{this} | Get a reference to a named logger using the current target object's path as the category name |

```javascript
property name="logbox" inject="logbox";
property name="log" inject="logbox:root";
property name="log" inject="logbox:logger:myapi";
property name="log" inject="logbox:logger:{this}";
```

## WireBox Namespace

Talk and get objects from the current wirebox injector

| DSL | Description |
| :--- | :--- |
| wirebox | Get a reference to the current injector |
| wirebox:parent | Get a reference to the parent injector \(if any\) |
| wirebox:eventManager | Get a reference to injector's event manager |
| wirebox:binder | Get a reference to the injector's binder |
| wirebox:populator | Get a reference to a WireBox's Object Populator utility |
| wirebox:scope:{scope} | Get a direct reference to an internal or custom scope object |
| wirebox:properties | Get the entire properties structure the injector is initialized with. If running within a ColdBox context then it is the structure of application settings |
| wirebox:property:{name} | Retrieve one key of the properties structure |

```javascript
property name="beanFactory" inject="wirebox";
property name="settings" inject="wirebox:properties";
property name="singletonCache" inject="wirebox:scope:singleton";
property name="populator" inject="wirebox:populator";
property name="binder" inject="wirebox:binder";
```

## EntityService Namespace

This injection namespace comes from the `cborm` module extensions \([https://github.com/ColdBox/cbox-cborm](https://github.com/ColdBox/cbox-cborm)\). It gives you the ability to easily inject base ORM services or binded virtual entity services for you:

| DSL | Description |
| :--- | :--- |
| entityService | Inject a BaseORMService object for usage as a generic service layer |
| entityService:{entity} | Inject a VirtualEntityService object for usage as a service layer based off the name of the entity passed in. |

```javascript
// Generic ORM service layer
property name="genericService" inject="entityService";
// Virtual service layer based on the User entity
property name="userService" inject="entityService:User";
```

## Object Scopes

You can very easily add persistence to your model+ objects via our annotations or binder configuration. The available scopes are:

* `transient or no scope` : The default scope. Meaning objects have no scope, they are recreated every single time you request them.
* `singleton` : Objects are created once and live until your Application expires
* `cachebox` : You can store your objects in any CacheBox provider and even provide timeouts for them
* `session` : Store them in the ColdFusion `session` scope
* `server` : Store them in the ColdFusion `server` scope
* `request` : Store them in the ColdFusion `request` scope
* `application` : Store them in the ColdFusion `application` scope
* `CUSTOM` : You can build your own scopes as well.

```javascript
// transient
component name="MyService"{}

// singleton
component name="MyService" singleton{}

// cache in default provider
component name="MyService" cache="true" cacheTimeout="45" cacheLastAccessTimeout="15"{}

// cache in another provider
component name="MyService" cachebox="MyProvider" cacheTimeout="45"{}

// request scope
component name="MyService" scope="request"{}

// session
component name="MyService" scope="session"{}
```

> **Hint** Pease note that using annotations is optional, you can configure every object in our configuration binder as well.

* Persistence DSL: [http://wirebox.ortusbooks.com/configuration/mapping-dsl/persistence-dsl](http://wirebox.ortusbooks.com/configuration/mapping-dsl/persistence-dsl)
* Object Persistence & Thread Safety: [http://wirebox.ortusbooks.com/advanced-topics/object-persistence-and-thread-safety](http://wirebox.ortusbooks.com/advanced-topics/object-persistence-and-thread-safety)

## Coding: Solo Style

![](https://github.com/ortus-docs/coldbox-docs/tree/97b8636ca1e8f4651f1021343c097bb3a7c2e9b9/.gitbook/assets/MVC%2Bobjects.png)

Now that we have seen all the theory and stuff, let's get down to business and do some examples. We will start with the full coding approach with **no** ORM and then spice it up with ORM, so you can see how awesome ORM can be. The examples will not show the entire application being built, but enough to get you started with the process of modeling everything. Here is a layout of what we will build:

```javascript
+ handlers
  + contacts.cfc
+ models
  + ContactService.cfc
  + ContactDAO.cfc
  + Contact.cfc
```

I will create a DAO for this small example, so we can showcase how to talk to multiple objects. You can start a new application with CommandBox if you like:

```text
mkdir solo
cd solo
coldbox create app name=solo skeleton=AdvancedScript --installColdBox
server start --rewritesEnable
```

## Datasource

Make sure you register a datasource in your ColdFusion administrator, we called ours `contacts` and then register it in your ColdBox configuration so ColdBox can build datasource structs for us. This is totally optional, but we do it to showcase more injections:

`config/ColdBox.cfc`

```javascript
// Settings
settings = {
    contacts = {
        type = "mysql",
        name = "contacts"
    }
}
```

## Contact.cfc

An object that represents a contact and self-validates using the `cbvalidation` module.

```text
coldbox create model name=Contact --open
```

Then spice up with some properties and constraints:

```javascript
component accessors="true"{

    // properties
    property name="firstName";
    property name="lastName";
    property name="email";

    // validation
    this.constraints = {
        firstName = {required=true},
        lastName = {required=true},
        email = {required=true, type="email"}
    };

    function init(){
        return this;
    }

}
```

## ContactDAO.cfc

Our Contact DAO will talk to the datasource object we declared and do a few queries. Notice that this object is a singleton and has some dependency injection.

```text
coldbox create model name=ContactDAO persistence=singleton --open
```

Then spice it up

```javascript
component accessors="true" singleton{

    // Dependency Injection
    property name="dsn" inject="coldbox:setting:contacts"

    function init(){
        return this;
    }

    query function getAll(){
        var sql = "SELECT * FROM contacts";
        return queryExecute( sql, {}, { datasource: dsn.name } );
    }

    query function getContact(required contactID){
        var params = {
            contactID: { value: arguments.contactID, cfsqltype: "numeric" }
        };
        var sql = "SELECT * FROM contacts where contactID = :contactID";
        return queryExecute( sql, params, { datasource: dsn.name } );
    }

    ... ALL OTHER METHODS HERE FOR CRUD ....

}
```

## ContactService.cfc

Here is our service layer and we have added some logging just for fun :\). Notice that this object is a singleton and has some dependency injection.

```text
coldbox create model name=ContactService persistence=singleton --open
```

Then spice it up

```javascript
component accessors="true" singleton{

    // Dependency Injection
    property name="dao" inject="ContactDAO";
    property name="log" inject="logbox:logger:{this}";
    property name="populator" inject="wirebox:populator";
    property name="wirebox" inject="wirebox";

    function init(){
        return this;
    }

    /**
    * Get all contacts as an array of objects or query
    */
    function list(boolean asQuery=false){
        var q = dao.getAllUsers();
        log.info("Retrieved all contacts", q.recordcount);

        if( asQuery ){ return q; }

        // convert to objects
        var contacts = [];
        for(var x=1; x lte q.recordcount; x++){
            arrayAppend( contacts, populator.populateFromQuery( wirebox.getInstance("Contact"), q, x ) );
        }

        return contacts;
    }

    /**
    * Get a persisted contact by ID or new one if 0 or no records
    */
    function get(required contactID=0){
        var q = dao.getContact(arguments.contactID);
        // if 0 or no records
        if( contactID eq 0 OR q.recordcount eq 0 ){
            // return a new object
            return wirebox.getInstance("Contact");
        }
        // Else return the object
        return populator.populateFromQuery( wirebox.getInstance("Contact"), q, 1 );
    }

    ... ALL OTHER METHODS HERE  ....

}
```

Now, some observations of the code:

* We use the populator object that is included in WireBox to make our lives easier so we can populate objects from queries and deal with objects.
* We also inject a reference to the object factory WireBox so it can create `Contact` objects for us. Why? Well what if those objects had dependencies as well.

## Contacts Handler

Let's put all the layers together and make the handler talk to the model. I create different saving approaches, to showcase different integration techniques:

```text
coldbox create handler name=Contacts actions=index,newContact,create,save --open
```

Spice it up now

```javascript
component{

    // Dependency Injection
    property name="contactService" inject="ContactService";

    function index(event,rc,prc){
        // Get all contacts
        prc.aContacts = contactService.list();
        event.setView("contacts/index");
    }

    function newContact(event,rc,prc){
        event.setView("contacts/newContact");
    }

    function create(event,rc,prc){
        var contact = populateModel( "Contact" );
        // validate it
        var vResults = validateModel( contact );
        // Check it
        if( vResults.hasErrors() ){
            flash.put( "errors", vResults.getAllErrors() );
            return newContact(event,rc,prc);
        }
        else{
            flash.put( "notice", "Contact Created!" );
            setNextEvent("contacts");
        }
    }

    function save(event,rc,prc){
        event.paramValue("contactID",0);
        var contact = populateModel( contactService.get( rc.contactID ) );
        // validate it
        var vResults = validateModel( contact );
        // Check it
        if( vResults.hasErrors() ){
            flash.put( "errors", vResults.getAllErrors() );
            return newContact(event,rc,prc);
        }
        else{
            flash.put( "notice", "Contact Saved!" );
            setNextEvent("contacts");
        }
    }
}
```

Now that you have finished, go execute the contacts and interact with it.

## Coding: ActiveEntity Style

Now let's build the same thing but using ColdFusion ORM and our `ActiveEntity` approach from the ColdBox ORM Extensions module.

```javascript
+ handlers 
  + contacts.cfc
+ models
  + Contact.cfc
```

## ORM

You will first make sure your `contacts` datsource exists in the Administrator and then we can declare our ORM settings in our `Application.cfc`

```javascript
// ORM Settings
this.ormEnabled       = true;
this.datasource          = "contacts";
this.ormSettings      = {
    cfclocation = "models",
    dbcreate    = "update",
    logSQL         = true,
    flushAtRequestEnd = false,
    autoManageSession = false,
    eventHandling       =  true,
    eventHandler      = "cborm.models.EventHandler"
};
```

These are the vanilla settings for using the ORM with ColdBox. Make sure that `flushAtRequestEnd` and `autoManageSession` are set to **false** as the ORM extensions will manage that for you.

In this example, we also use `dbcreate="update"` as we want ColdFusion ORM to build the database for us which allows us to concentrate on the domain problem at hand and not persistence. You also see that we add our own eventHandler which points to the extension's event handler so we can make Active Entity become well, Active!

### Activating ORM injections

Now open your `ColdBox.cfc` and add the following to activate ORM injections inside of your `configure()` method.

```javascript
orm = { injection = { enabled=true } };
```

## Contact.cfc

An object that represents a contact and self-validates using [ColdBox Validation Module](https://github.com/coldbox-modules/cbox-validation/wiki), and is an awesome [ActiveEntity](https://coldbox.ortusbooks.com/the-basics/models/coding-activeentity-style) object. Let's use CommandBox to build it:

```bash
coldbox create orm-entity entityName=contact primaryKey=contactID properties=firstName,lastName,email --activeEntity --open
```

Then spice it up with the validation constraints

```javascript
/**
* A cool Contact entity
*/
component persistent="true" table="contacts" extends="cborm.models.ActiveEntity"{

    // Primary Key
    property name="contactID" fieldtype="id" column="contactID" generator="native" setter="false";

    // Properties
    property name="firstName" ormtype="string";
    property name="lastName" ormtype="string";
    property name="email" ormtype="string";

    // validation
    this.constraints = {
        firstName = {required=true},
        lastName = {required=true},
        email = {required=true, type="email"}
    };

}
```

## Contacts Handler

That's right, go to the handler now, no need of data layers or services, we build them for you! This time, we show you the entire CRUD operations as Active Entity makes life easy!

```bash
coldbox create handler name=contacts actions=index,editor,delete,save
```

Then spice it up

```javascript
/**
* I am a new handler
*/
component{

    function index(event,rc,prc){
        prc.contacts = entityNew("Contact").list(sortOrder="lastName",asQuery=false);
        event.setView("contacts/index");
    }

    function editor(event,rc,prc){
        event.paramValue("id",0);
        prc.contact = entityNew("Contact").get( rc.id );
        event.setView("contacts/editor");
    }

    function delete(event,rc,prc){
        event.paramValue("id",0);
        entityNew("Contact").deleteByID( rc.id );
        flash.put( "notice", "Contact Removed!" );
        setNextEvent("contacts");
    }

    function save(event,rc,prc){
        event.paramValue("id",0);
        var contact = populateModel( entityNew("Contact").get( rc.id ) );
        if( contact.isValid() ){
            contact.save();
            flash.put( "notice", "Contact Saved!" );
            setNextEvent("contacts");
        }
        else{
            flash.put( "errors", contact.getValidationResults().getAllErrors() );
            return editor(event,rc,prc);
        }
    }

}
```

## Views

Here are the views as well, which are pre-created via CommandBox handler creation command.

### index.cfm

```javascript
<cfoutput>
<h1>Contacts</h1>

<cfif flash.exists( "notice" )>
    <div class="alert alert-info">#flash.get( "notice" )#</div>
</cfif>
<cfif flash.exists( "errors" )>
    <div class="alert alert-danger">#flash.get( "errors" ).toString()#</div>
</cfif>

#html.href(href='contacts.editor',text="Create Contact")#
<br><br>
<cfloop array="#prc.contacts#" index="contact">
<div>
    #contact.getLastName()#, #contact.getFirstName()# (#contact.getEmail()#)<br/>
    #html.href(href='contacts.editor.id.#contact.getContactID()#',text="[ Edit ]")#
    #html.href(href='contacts.delete.id.#contact.getContactID()#',text="[ Delete ]",onclick="return confirm('Really Delete?')")#
    <hr>
</div>
</cfloop>
</cfoutput>
```

### editor.cfm

Check out our awesome `html` helper object. It can even build the entire forms according to the Active Entity object and bind the values for you!

```javascript
<cfoutput>
<h1>Contact Editor</h1>

<cfif flash.exists( "notice" )>
    <div class="alert alert-info">#flash.get( "notice" )#</div>
</cfif>
<cfif flash.exists( "errors" )>
    <div class="alert alert-danger">#flash.get( "errors" ).toString()#</div>
</cfif>

#html.startForm(action="contacts.save")#
    #html.entityFields(entity=prc.contact,fieldwrapper="div")#
    #html.submitButton()# or #html.href(href="contacts",text="Cancel")#
#html.endForm()#
</cfoutput>
```

## Coding: Virtual Service Layer

Now let's build the same thing but using ColdFusion ORM and our [Virtual Service Layer](http://wiki.coldbox.org/wiki/ORM:VirtualEntityService.cfm) approach, in which we will use a service layer but virtually built by ColdBox. This will most likely give you 80% of what you would ever need, but in case you need to create your own and customize, then you would build a service object that extends or virtual or base layer.

```javascript
+ handlers 
  + contacts.cfc
+ models
  + Contact.cfc
```

## ORM

You will first make sure your `contacts` datsource exists in the Administrator and then we can declare our ORM settings in our `Application.cfc`

```javascript
// ORM Settings
this.ormEnabled       = true;
this.datasource          = "contacts";
this.ormSettings      = {
    cfclocation = "models",
    dbcreate    = "update",
    logSQL         = true,
    flushAtRequestEnd = false,
    autoManageSession = false,
    eventHandling       =  true,
    eventHandler      = "cborm.models.EventHandler"
};
```

These are the vanilla settings for using the ORM with ColdBox. Make sure that `flushAtRequestEnd` and `autoManageSession` are set to **false** as the ORM extensions will manage that for you.

In this example, we also use `dbcreate="update"` as we want ColdFusion ORM to build the database for us which allows us to concentrate on the domain problem at hand and not persistence. You also see that we add our own eventHandler which points to the extension's event handler so we can make Active Entity become well, Active!

### Activating ORM injections

Now open your `ColdBox.cfc` and add the following to activate ORM injections inside of your `configure()` method.

```javascript
orm = { injection = { enabled=true } };
```

## Contacts.cfc

Let's use CommandBox to build it:

```bash
coldbox create orm-entity entityName=contact primaryKey=contactID properties=firstName,lastName,email --open
```

Then spice it up with the validation constraints

```javascript
/**
* A cool Contact entity
*/
component persistent="true" table="contacts"{

    // Primary Key
    property name="contactID" fieldtype="id" column="contactID" generator="native" setter="false";

    // Properties
    property name="firstName" ormtype="string";
    property name="lastName" ormtype="string";
    property name="email" ormtype="string";

    // validation
    this.constraints = {
        firstName = {required=true},
        lastName = {required=true},
        email = {required=true, type="email"}
    };

}
```

## Contacts Handler

That's right, go to the handler now, no need of data layers or services, we build them for you!

```bash
coldbox create handler name=contacts actions=index,editor,delete,save
```

Now spice it up

```javascript
/**
* I am a new handler
*/
component{

    // Inject a virtual service layer binded to the contact entity
    property name="contactService" inject="entityService:Contact";

    function index(event,rc,prc){
        prc.contacts = contactService.list(sortOrder="lastName",asQuery=false);
        event.setView("contacts/index");
    }

    function editor(event,rc,prc){
        event.paramValue("id",0);
        prc.contact = contactService.get( rc.id );
        event.setView("contacts/editor");
    }

    function delete(event,rc,prc){
        event.paramValue("id",0);
        contactService.deleteByID( rc.id );
        flash.put( "notice", "Contact Removed!" );
        setNextEvent("contacts");
    }

    function save(event,rc,prc){
        event.paramValue("id",0);
        var contact = populateModel( contactService.get( rc.id ) );
        var vResults = validateModel( contact );
        if( !vResults.hasErrors() ){
            contactService.save( contact );
            flash.put( "notice", "Contact Saved!" );
            setNextEvent("contacts");
        }
        else{
            flash.put( "errors", vResults.getAllErrors() );
            return editor(event,rc,prc);
        }
    }

}
```

## Views

Here are the views as well, isn't it awesome that the views stay the same :\), that means we did a good job abstracting our model and controllers.

### index.cfm

```javascript
<cfoutput>
<h1>Contacts</h1>

<cfif flash.exists( "notice" )>
    <div class="alert alert-info">#flash.get( "notice" )#</div>
</cfif>
<cfif flash.exists( "errors" )>
    <div class="alert alert-danger">#flash.get( "errors" ).toString()#</div>
</cfif>

#html.href(href='contacts.editor',text="Create Contact")#
<br><br>
<cfloop array="#prc.contacts#" index="contact">
<div>
    #contact.getLastName()#, #contact.getFirstName()# (#contact.getEmail()#)<br/>
    #html.href(href='contacts.editor.id.#contact.getContactID()#',text="[ Edit ]")#
    #html.href(href='contacts.delete.id.#contact.getContactID()#',text="[ Delete ]",onclick="return confirm('Really Delete?')")#
    <hr>
</div>
</cfloop>
</cfoutput>
```

### editor.cfm

Check out our awesome `html` helper object. It can even build the entire forms according to the Active Entity object and bind the values for you!

```javascript
<cfoutput>
<h1>Contact Editor</h1>

<cfif flash.exists( "notice" )>
    <div class="alert alert-info">#flash.get( "notice" )#</div>
</cfif>
<cfif flash.exists( "errors" )>
    <div class="alert alert-danger">#flash.get( "errors" ).toString()#</div>
</cfif>

#html.startForm(action="contacts.save")#
    #html.entityFields(entity=prc.contact,fieldwrapper="div")#
    #html.submitButton()# or #html.href(href="contacts",text="Cancel")#
#html.endForm()#
</cfoutput>
```

## Coding: ORM Scaffolding

Now let's use the power of ORM and CommandBox to scaffold everything for you :\). The help for this command is here:

```bash
coldbox create orm-crud help
```

It even creates all the integration tests for you.

## ORM

You will first make sure your `contacts` datsource exists in the Administrator and then we can declare our ORM settings in our `Application.cfc`

```javascript
// ORM Settings
this.ormEnabled       = true;
this.datasource          = "contacts";
this.ormSettings      = {
    cfclocation = "models",
    dbcreate    = "update",
    logSQL         = true,
    flushAtRequestEnd = false,
    autoManageSession = false,
    eventHandling       =  true,
    eventHandler      = "cborm.models.EventHandler"
};
```

These are the vanilla settings for using the ORM with ColdBox. Make sure that `flushAtRequestEnd` and `autoManageSession` are set to **false** as the ORM extensions will manage that for you.

In this example, we also use `dbcreate="update"` as we want ColdFusion ORM to build the database for us which allows us to concentrate on the domain problem at hand and not persistence. You also see that we add our own eventHandler which points to the extension's event handler so we can make Active Entity become well, Active!

### Activating ORM injections

Now open your `ColdBox.cfc` and add the following to activate ORM injections inside of your `configure()` method.

```javascript
orm = { injection = { enabled=true } };
```

## Contacts.cfc

Let's use CommandBox to build it:

```bash
coldbox create orm-entity entityName=contacts primaryKey=contactID properties=firstName,lastName,email --open
```

Then spice it up with the validation constraints

```javascript
/**
* A cool Contact entity
*/
component persistent="true" table="contacts"{

    // Primary Key
    property name="contactID" fieldtype="id" column="contactID" generator="native" setter="false";

    // Properties
    property name="firstName" ormtype="string";
    property name="lastName" ormtype="string";
    property name="email" ormtype="string";

    // validation
    this.constraints = {
        firstName = {required=true},
        lastName = {required=true},
        email = {required=true, type="email"}
    };

}
```

## Scaffold

Now let us relish in the power of the `orm-crud` command:

```bash
coldbox create orm-crud entity=models.Contacts pluralName=Contacts
```

That's it.

```text
Generated Handler: /Users/lmajano/tmp/restapp/handlers/Contacts.cfc
Generated View: /Users/lmajano/tmp/restapp/views/Contacts/edit.cfm
Generated View: /Users/lmajano/tmp/restapp/views/Contacts/editor.cfm
Generated View: /Users/lmajano/tmp/restapp/views/Contacts/new.cfm
Generated View: /Users/lmajano/tmp/restapp/views/Contacts/index.cfm
```

The command will create the handler with RESTFul capabilities, the views and all the necessary machinery for you to manage Contacts! Now go get a nice latte in celebration!

