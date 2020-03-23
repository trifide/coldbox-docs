---
description: Model View Controller
---

# Architecture Concepts

## What is MVC

### Intro to MVC

> "A developer often wishes to separate data \(model\) and user interface \(view\) concerns, so that changes to the user interface will not affect data handling, and that the data can be reorganized without changing the user interface. The model-view-controller solves this problem by decoupling data access and business logic from data presentation and user interaction, by introducing an intermediate component: the controller." [Wikipedia](http://en.wikipedia.org/wiki/Model-view-controller)​

MVC is a popular design pattern called [Model View Controller](http://en.wikipedia.org/wiki/Model–view–controller) which seeks to promote good maintainable software design by separating your code into 3 main tiers:

* **Model**  - Business Logic, Data, Queries, Etc
* **View** - Representation of your models, queries, data.
* **Controller** - Orchestrator of client request to the appropriate models and views

Let's go a little deeper.

#### Model

The Model is the heart of your application. Your business logic should mostly live here in the form of services, beans, entities and DAOs. A dependency injection framework becomes invaluable when dealing with object oriented model layers: [**WireBox**](https://wirebox.ortusbooks.com) \(Dependency Injection Framework\) is the framework of choice for dependency injection and aspect oriented programming.

#### Views

The Views are what the users see and interact with. They are the templates used to render your application out for the web browser. Typically this means cfm/HTML, but it can also be JSON, XML, data views, etc.

In modern times, your views can even be pure HTML with a combination of a JavaScript MVC framework. The major players in the MVC front-end world that we would recommend in order of personal preference:

* **VueJS** - [https://vuejs.org/](https://vuejs.org/)
* **Angular** - [https://angular.io/](https://angular.io/)
* **ReactJS** - [https://reactjs.org/](https://reactjs.org/)
* **EmberJS** - [https://www.emberjs.com/](https://www.emberjs.com/)

#### Controllers

Controllers are the traffic cops of your application. They direct flow control, and interface directly without incoming parameters from FORM and URL scopes. It is the controller’s job to communicate with the appropriate models for processing, and set up either a view to display results or return serialized data like JSON, XML, PDF, etc.

### Benefits of MVC

By implementing an MVC Framework to your applications you will gain several benefits that come inherent to the MVC design pattern. The most important benefit of MVC is that you will be **separating the presentation** from the model. This is a very important heuristic of software development as [**separation of concerns**](https://en.wikipedia.org/wiki/Separation_of_concerns) is applied and responsibilities are delegated upon the layers.

#### Separation of Concerns

The model and the view layers have different concerns about their implementations. A view layer is concerned with how to render the data, the type of browser, or remote rendering, etc. While the model is more concerned with the business rules of the application, how to store data and even database operations. You use different development approaches to each layer.

#### Multiple GUI’s

Due to this separation, you can easily create multiple views for the same model data without affecting how the model works or is coded. The view layers can adapt to the model by coding their own implementations. This makes it really easy to create multiple GUI’s for applications.

#### Unit and Behavioral Testing

Non-visual objects are easier to test than visual objects, in theory. With the introduction of Selenium, integration and visual UI testing has become rather simple. However, the key benefit here is that testing can be done separately. Frameworks like ColdBox even give you the ability to do UI and integration testing within its domain.

#### Dependency

The most important benefit that we can arise out of the MVC pattern, is the direction of the dependencies. A view depends on its model data and controller, but the model itself does not depend on the view or controllers. This is how you want to build your business logic, encapsulated and providing a good API.

### Evolution of MVC Architecture <a id="coldbox-mvc"></a>

There are many types of MVC architectures and hopefully the following diagrams can help you in the progression from spaghetti hell to the most complex MVC architecture using an [`ORM`](https://en.wikipedia.org/wiki/Object-relational_mapping) or Object Relational Mapper.

#### Spaghetti Hell

As you can see from the spaghetti hell diagram above, everything is linear and can become extremely convoluted. Tracking bugs are difficult, maintenance suffers and reusability is not efficient. Everything is in the same bowl of soup.

#### MVC

With the introduction of MVC we can hack away our spaghetti hell and at least have three distinct and separate layers of logic. Ahh much better. However, we can get even more complex.

#### MVC Plus

MVC Plus shows us how you can further partition your model layer into more layers. We can identify now a layer of service CFCs and data access object CFCs. The main transportation of data between these layers by default is implied to be ColdFusion Query objects.

#### MVC Plus Objects

In this architecture approach, we have replaced \(mostly\) queries as our data structure of preference and converted to the usage of business objects. We are approaching a more object oriented architectural style. Remember that data is just data, objects are data plus behavior. We can encapsulate more features and abstract more behavior into actual objects now, which we could not do with queries.

#### MVC Plus ORM

In this architecture approach we have replaced business objects for ORM entities and replaced our data access layer to be controlled now by the ORM. This takes us very deep into object oriented land where the majority of our model is now modeled vi relational objects.

{% hint style="danger" %}
**Stern Warning:** ORMs are NOT silver bullets. They are an incredible tool that must be used for the right reasons and at the right time. Do not be confused in that you must ONLY use the ORM. No, you can still use DAOs and queries for certain things that matter. You do not need to retrieve entire object graph collections if NOT needed.

We have even build a companion package for ColdBox called [**cborm**](https://github.com/coldbox-modules/cbox-cborm) that will help you build more pragmatic and enjoyable ORM applications.
{% endhint %}

### More Resources <a id="resources"></a>

* ​[http://en.wikipedia.org/wiki/Domain\_model](http://en.wikipedia.org/wiki/Domain_model)​
* ​[http://domaindrivendesign.org/](http://domaindrivendesign.org/)​
* ​[http://martinfowler.com/eaaCatalog/domainModel.html](http://martinfowler.com/eaaCatalog/domainModel.html)​

## What is ColdBox

ColdBox is a conventions-based MVC framework for ColdFusion \(CFML\). It is fast, scalable, and runs on CFML engines such as Adobe ColdFusion and the open source CFML engine [Lucee](http://www.lucee.org), for a completely free and open source development stack. ColdBox itself is Professional Open Source Software, backed by [Ortus Solutions](http://www.ortussolutions.com) which provides support, training, architecture, consulting and commercial additions.

ColdBox was the first ColdFusion \(CFML\) framework to embrace Conventions Over Configuration and comes with a modular architecture to enhance developer and team productivity. It integrates seamlessly with [CommandBox](http://www.ortussolutions.com/products/commandbox) CLI to provide you with REPL interfaces, package management, console development, testing, and automation.

Here’s a look at some of the core components of the ColdBox platform. Each of these come bundled with the framework, but are also available as separate stand-alone libraries that can be used in ANY ColdFusion application or framework:

### WireBox

Managing your domain objects has never been easier with this Dependency Injection and Inversion of Control \(IOC\) framework. WireBox supports all forms of injection as well as maintaining persistence for your domain objects. WireBox can also interface and provide Java classes, web services, and aspects of the ColdBox framework itself. WireBox also has built-in Aspect Oriented Programming \(AOP\) support; you’ll never need another Dependency Injection engine again.

### LogBox

This is a highly-configurable logging library which can be set up to relay messages of different types from any portion of your application to any number of predefined logging appenders.

### CacheBox

A highly-versatile caching aggregator and enterprise cache that allows for multiple named caching stores as well as granular control over cache behaviors and eviction policies. CacheBox can interface out of the box with Ehcache, Adobe ColdFusion cache, and any Lucee cache.

## How ColdBox Works

ColdBox uses both implicit and explicit invocation methods to execute events and render content back to a user. You have a single configuration CFC: `config/Coldbox.cfc`, from where you can configure your entire application and a set of folder/file conventions. This configuration file activates certain aspects of your application and configures all the implicit events that mostly reflect the events in the `Application.cfc` that ColdFusion exposes to you.

> Remember that this framework will not solve all your problems. It is a standard and a foundation on which to develop on due to the software programming aspects that it provides. However, it is up to you to create GOOD code. This is not a magical framework that will make your code better. It will help you, but at the end of the day, it is your responsibility.

### Front Controller

ColdBox is loaded by the `Application.cfc` and makes use of the Front Controller design pattern as its means of operation. This means that every request comes in through a single template, usually `index.cfm`. Once a request is received by the framework through this front controller, it will parse the request and redirect appropriately to the correct event handler controller by looking for an `event` variable in the URL or FORM scopes.

### Request Context

The incoming URL, FORM, and REMOTE variables are merged into a single structure that we call the request collection and, since we love objects, that collection is stored in an object called Request Context. We also create a secondary collection called the private request collection that cannot be affected by the outside world as nothing is merged into it. You can use it for private request variables and the like.

The request context object has tons of methods to help you in setting and getting variables from one MVC layer to another, to getting request metadata, rendering RESTful content, setting HTTP headers, and more. It is your information super highway for specific requests. Remember that the API Docs are your best friend!

### ColdBox Major Classes

Below you can see a UML diagram of the ColdBox Major Classes eco-system.

### Request Lifecycle

A typical basic request to a ColdBox application looks like this:

* HTTP\(S\) request is sent from browser to server `http://www.example.com/index.cfm?event=home.about`
* The request context is created for this request and FORM/URL scopes are populated into the request collection
* The Main ColdBox Event is determined from the `event` variable \(`home.about`\). \(handler is **home** and action is **about**, notice the period separator
* Event handler controller action is run \(`about()` method in `/handlers/home.cfc`\)
* The event handler might call a model for business logic
* The view set in the event is rendered \(`/views/home/about.cfm`\)
* The view’s HTML is wrapped in the rendered layout \(`/layouts/main.cfm`\)
* Page is returned to the browser

Below you can see the full life-cycle for MVC requests:

#### Proxy Lifecycle

ColdBox also has a proxy feature for building SOAP webservices or Flex/Air integration called [ColdBox Proxy](../digging-deeper/coldbox-proxy/). Below you can see the life-cycle for that process:

## Testing Concepts

> In computer programming, unit testing is a procedure used to validate that individual units of source code are working properly. A unit is the smallest testable part of an application. In procedural programming a unit may be an individual program, function, procedure etc, while in object-oriented programming, the smallest unit is always a Class; which may be a base/super class, abstract class or derived/child class. Wikipedia

The testing process will take us through a journey of exploration of our software in order to determine its quality, if it met the gathered requirements, work as expected, and provide the required functionality. However, all the testing that we do is mostly controlled and under certain conditions. It is extremely difficult to determine all factors and all software conditions. Therefore attaining a testing nirvana is unrealistic.

## Functional Testing

[Functional testing](http://en.wikipedia.org/wiki/Functional_testing) relies on the action of verification of specific requirements in our software. This can usually be tracked via use cases or Agile stories. "Can a user log in?", "Does the logout work", "Can I retrieve my password", "Can I purchase a book?". Our main focus of functional testing is that all these use cases need to be met.

## Non-Functional Testing

[Non Functional](http://en.wikipedia.org/wiki/Non-functional_testing) Testing are testing aspects that are not related to our specific requirements but more of the quality of our software.

* Can our software scale? 
* Is our software performant?
* Can it sustain load? What is the maximum load it can sustain? 
* What do we do to extend our security? Are our servers secure?

As developers, we sometimes forget the importance of non-functional testing, but what good is an application that meets the requirements but cannot be online for more than an hour? Both are extremely important and we must dedicate time to each area of testing so our software not only meets the requirements, but has great quality!

## Bugs Cost Money

> Kaner, Cem; James Bach, Bret Pettichord \(2001\). Lessons Learned in Software Testing: A Context-Driven Approach. Wiley. p. 4. ISBN 0-471-08112-4.

It is believed that the earlier a bug is found the cheaper it is to fix it. This is something to remember as when things affect our income is when we become babies and really want to fix things then. So always take into consideration that bugs do cost money and we can be proactive about it. So what are some things apart from unit testing that we can do to improve the quality of our software? Well, apart from applying our functional and non-functional testing approaches we can also do some static and dynamic testing of our software.

## Static Testing

Static testing relies on human interaction not only with our code but with our developers and stakeholders. Some aspects can be code reviews, code/ui walkthroughs, design and modeling sessions, etc. Through experience, we can also attest that static testing is sometimes omitted or considered to be a waste of time. Especially when you need to hold up on production releases to review somebody else’s work. Not only that, but sometimes code reviews can spark animosity between developers and architects as most of the time the idea is to find holes in code, so careful tact must be in place and also maturity across development environments. In true fun we can say: `“Come on, take the criticism, it is for the common good!”`

## Dynamic Testing

On the other side of the spectrum, dynamic testing relies on the fact of executing test cases to verify software. Most of the time it can be the developers themselves or a QA team than can direct testing cases on executed code.

## Developer Focus

This section is writtent for developers, so what is our focus? Our focus is to get awareness about the different aspects of testing and what are some of the testing focuses for the development phase:

* **Unit Testing** - We should be testing all the CFCs we create in our applications. Especially when dealing with Object Oriented systems, we need to have mocking and stubbing capabilities as well, as that might be the only way to test some CFCs.
* **Integration Testing** - Unit tests can only expose issues at the CFC level, but what happens when we put everything together? Integration testing in ColdBox allows you to test requests just like if they where executed from the browser. So all your application loads, interceptions, caching, dependency injection, ORM, database, etc, will fire in unison.
* **UI Integration Testing** - Testing verification via the UI using tools like Selenium is another great idea!
* **Load Testing** - There are tons of free load testing tools and they are easy to test how our code behaves under load. Don't open a browser with 5 tabs on it and start hitting enter! We have been there and it doesn't work that great :\)

## Testing Vocabulary

Here are some terms that we need to start getting familiar with:

* **Test Plan**: A test plan documents the strategy that will be used to verify and ensures that a product or system meets its design specifications and other requirements.
* **Test Case**: A set of conditions or variables under which a tester will determine whether an application or software system is working correctly or not. In our ColdFusion space, it might be represented by an MXUnit test case CFC.
* **Test Script**: A set of instructions that will be performed on the system under test to test that the system functions as expected. This can be manual or automated. Sometimes they can be called runners.
* **Test Suite**: A collection of test cases that are intended to be used to test a software program to show that it has some specified set of behaviors or met expectations.
* **Test Data**: A collection of pre-defined data or mocked data used to run against our test cases.
* **Test Harness**: A collection of software and test data configured to test a program unit by running it under varying conditions and monitoring its behavior and outputs.
* **TDD**: Test Driven Development - A software development process that relies on the repetition of a very short development cycle: first the developer writes a failing automated test case that defines a desired improvement or new function, then produces code to pass that test and finally refactors the new code to acceptable standards.
* **BDD**: BDD stands for behavior driven development and is highly based on creating specifications and expectations of results in a readable DSL \(Domain Specific Language\).
* **Mock Object**: Simulated objects that mimic the behavior of real objects in controlled ways.
* **Stub Object**: Simulated objects that have no behavior or implementations and can be controlled and injected with functionality.
* **Assertions**: The function to test if a predicate is true or false: X&gt;1, 4=5
* **Expectations** : Much like assertions, it is closely linked to BDD to verify behavior.

