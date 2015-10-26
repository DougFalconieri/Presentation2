#Presentation 2: Lift

##Information Sources

* [Dallaway, Ricard. *Lift Cookbook.* O'Reilly, 2013](http://chimera.labs.oreilly.com/books/1234000000030/index.html)
* [Lift Website](http://liftweb.net/)
* Perrett, Timothy. *Lift in Action.* Manning, November 27, 2011.
* It was surprisingly hard to find good resources about Lift.
    * Not many books available and most are old and have poor reviews on Amazon.com.
    * Lift website points to the *Lift Cookbook*.
        * I find tutorial format better for learning a new technology.
        * Cookbook format is good for learning more details once you have a basic understanding.

##What is Lift?

* A web framework using Scala and running on the Java Virtual Machine.

##Why is it Interesting?

* Newer programming languages that support functional programming features get a lot of buzz.
* I want to get a sense of what it would be like to use one for an everyday software development task like building a web application.
* Dynamically typed languages like have some disadvantages when used on large-scale projects.
    * Performance can be worse.
    * Development environments for dynamic languages typically lack features like automatic refactoring, accurate code completion and jumping to variable and method definitions.
        * These features are useful when working with large codebases.
* Scala is statically-typed, but includes many advanced languages like support for functional programming.
* It also runs on the Java Virtual Machine, an environment already supported in many enterprises.
* Could potentially be a replacement for Java web development.

##Installing Lift

* Highly automated installation process on all operating systems.
* Need to have Java 1.7 or later installed.
* Download Lift as a zip file from the [Lift Website]( http://liftweb.net/download and download the most recent Lift 2.6 ZIP file.
Unzip the file.)
* Package includes several example projects.
* Each project contains a copy of Scala's Simple Build Tool (SBT).
* From within a project, run SBT from the command line and type `container:start` to start the project in a Jetty server.
* By default, it will run on `localhost:8080`.
* `container.stop` will stop the project.
* `~; container:start; container:reload /` will start the container and automatically pick up file changes.

##Using Eclipse

* Since IDE support is a major potential benefit of Scala, I wanted to use Eclipse to work with my Lift projects.
* Need to install the Scala plugin for Eclipse from the [Scala IDE Website](http://scala-ide.org).
   * Available as a bundle with Eclipse or from an update server for existing Eclipse installations.
* SBT instances in the Lift sample projects come with a plugin to generate Eclipse projects files.
    * Type `eclipse` into SBT to generate the project files.
* Once the project files have been generated, the project can be imported into an Eclipse workspace using `File -> Import... -> Existing Projects Into Workspace`.

##Exploring a Lift Project

* Basic project properties (project name, version number, etc.) are stored in the build.sbt file at the project root.
* Like most web frameworks, Lift uses HTML templates to define the user interface.
    * When you hit the root URL of a Lift application, the view you see is defined in a file called `index.html`.
    * Located at "<project root>/src/main/webapp".
* Lift templates are plain, valid HTML files with .html extensions.
* No Scala code is embedded in the HTML.
    * Instead HTML is embedded in your Scala code!
* Tags in the HTML that you want to replace with dynamic content are marked with `lift` or `data-lift` attributes.
   * `data-lift` option is more compliant with HTML standards.
   * e.g. `<span data-lift="HelloWorld.howdy">Placeholder</span>`
* The value of the data-lift attribute point to a Scala class and function.
* These functions are known as `snippets`.
* Snippets are located at “<project root>/src/main/scala/code/snippet” directory.
* Lift also has a number of other alternative ways to associate an HTML tag with a snippet.
    * But the one above seems to be the most "standard".

##Snippets

* A snippet takes in a NodeSeq and returns another NodeSeq.
    * `NodeSeq` is part of Scala’s built-in XML support and represents a list of XML nodes.
    * So basically a snippet replaces a piece of the HTML template with dynamic content.
* Most snippets use Lift’s support for CSS-style selectors to indicate what part of the HTML to replace.
    * These snippets take the form `<selector> #> <replacement>`.
* Since Cascading Style Sheets (CSS) language is already familiar to all web developers, this provides powerful search capability without a big learning curve.
    * Similar approach to the jQuery JavaScript library: applies existing CSS language to an alternate use.
* Selectors are basically a domain-specific language for tranforming HTML.

###Snippet Examples

* `”span” #> “<a></a>”
    * Using the name of a tag selects all the tags of that type within the current tag and completely replaces them.
    * This is mainly useful when combined with other selectors.
* `”span *” #> “Hello!”`
    * This example replaces the contents of all the span tags within the current tag with the string “Hello!”.
    * The asterisk selects the contents of the selection.
* `”#mySpan *” #> “Hello!”`
    * This example replaces the contents of the tag with an id attribute of `mySpan` with the string “Hello!”.
* `”span#mySpan * #> “Hello!”`
    * This example shows how multiple selectors can be combined together.
    * The result selects the contents of a span tag with an id of “mySpan”.
* `”[href]” #> “http://www.google.com”`
    * This example selects the value of the href attribute on the current tag.
    * The bracket notation is used to access attributes of tags.
* `”@foo *” #> “Hello!”
    * This example uses the `@` selector to select elements with their `name` attributes set to “foo”.
* `”[class+]” #> “selected”`
    * Adding a plus-sign to the attribute selector causes the replacement to be appended to the existing value rather than replacing it.
* `”[class!]” #> “selected”`
    * Similarly, adding an exclamation point to the attribute selector causes the replacement to be appended to the existing value rather than replacing it.
* `”ul” #> myItems.map(item => ".name" #> item.name)`
    * CSS selectors can even be used to create repeating HTML sections when combined with the map function.
    
###Advanced Snippets

* As an alternative to standard snippets, snippets can extend `DispatchSnippet` to provide customized dispatching.
* Another variation of snippet is `StatefulSnippet` which preserves data across multiple requests.
* A snippet can be nested within the built-in `LazyLoad` snippet causing the page to be rendered with out it while the snippet is executed in the background.
    * Once the snippet execution is finished, the result is injected into the page replacing the placeholder.
    * This is useful for snippets that do some long-running task like calling a slow web service.

##Reusing Content

* Web applications often have boilerplate content that appears on every screen of the application.
    * This can includes a page header, global menu and footer.
* Lift supports this use case without duplicating the HTML using the built in `surround` template.
    * One of many useful built-in snippets that are included with Lift.
* A template can use the surround template to embed itself within another template.
* Example: `<div id="main" data-lift="surround?with=default;at=content">`.
* In this example, `default` is the name of the template to wrap around the current template.
    * This corresponds to a template called `default.html`.
    * This file would typically be located in the `templates-hidden` directory of the project so it cannot be accessed directly.
* `content` indicates where in the wrapping template to add the current template's content.
* Corresponds to a tag with its `id` attribute set to `content`.
* Lift also comes with an `embed` snippet to include content from another template.
    * e.g. `<div id="main" data-lift="embed?what={path to the file}">`.
* Both `surround` and `embed` will merge the head sections of the two files being combined.
* Optionally, the `tail` snippet can merge JavaScript includes immediately before the end of the `body` section.
* The `RequestVar` class can be used to return variables from a request in the response.
* Similarly `SessionVar` can be used to store data accross multiple requests.
* The `S` object provides the snippet access to a lot of useful functionality.
    * Snippets can work with cookies using the `S.setCookie()`, `S.findCookie()`, and `S.deleteCookie()` methods.
    * `S.error()`, `S.message()` and `S.notice()` display messages to the user by injecting them into the template.
    * `S.redirectTo()` allows the snippet to redirect to a different URL.

##Routing

* An important feature of web development frameworks is mapping URLs to templates and code.
* Lift supports routing through the `SiteMap` object.
* `SiteMaps` are configured in the `Boot.scala` file located in `<project directory>/src/main/scala/bootstrap/liftweb`.
* The `SiteMap` consists of `Menu` objects.
* Like selectors, `SiteMaps` use a custom DSL to concisely define `Menu` objects.
* Example: `Menu("Home") / "test"`
    * This example defines a `Menu` object named "Home".
    * "test" is a path to template for the `Menu`.
* `Menu` items can contain nested sub-menus.
* The built-in `menu.builder` snippet can automatically generate an HTML menu from a `SiteMap`.
* You can use the `Hidden` attribute to keep a screen in the site map out of the generated menu.
    * e.g. `Menu("Home") / "test" >> Hidden`
* `SiteMaps` can also be used to assign security rules to each screen in the application.
* Finally, it can define filter-like functionality that can run before each request is processed.

##Forms and AJAX

* Lift includes lots of support for the common task of working with forms.
* Lift snippets can associate form controls in the HTML templates with Lift helpers.
    * Examples:
        * `"name=email" #> SHtml.onSubmit(email = _)`
            * This snippet associates a form control with a `name` attribute of "email" to a local variable named "email".
            * `onSubmit` also supports adding validation rules.
        * `"type=submit" #> SHtml.onSubmitUnit(processForm)`
            * This snippet tells Lift to call a function called `proccessForm` when a HTML submit button is pressed.
* Lift also includes a class called `SHtml` that can generate HTML.
    * For example, `SHtml.text` generates a text box that can be prepopulated with data from a variable or object.
* Snippets that extend the `LiftScreen` class can automatically generate a whole form from a data object.
* Lift has a lot of support for working with Ajax.
    * Includes library for serializing and deserializing JSON.
    * `SHtml.ajaxSubmit` can inject Ajax submission of a form in a single line of code.
    * The `JsonCmd` class can generate JavaScript code for many common operations.
        * Allows developer to work mostly in Scala without having to write JavaScript.
        
##Data Management

* Because it runs on the JVM, Lift is compatible with any Java database framework.
    * This includes the standard Java Persistence API (JPA).
* However, Lift does ship with a built-in persistence framework called "Mapper".
* Mapper follows the Active Record pattern popularized by Ruby on Rails.
* Entity classes are created that represent a table in a database.
    * These classes extend the `Mapper[T]` trait or one of its sub-traits.
* Database columns are represented by properties of the mapper class.
* Relationships between entity classes can also be defined.
* Once the entity classes are set up, they can be used to execute CRUD operations against the database.
    * This is done with Scala methods rather than directly writing SQL queries.
    * However, it is also possible to directly execute SQL when required.
* Mapper comes with a tool called `Schemifier` that can generate a database from Mapper entity classes.

##Final Thoughts

* Scala is a very powerful and flexible programming language and as a result Lift has a lot of potential.
* Scala is good for creating domain specific languages and Lift takes advantage of this to create very concise syntax for many tasks.
    * But this can result in obscure code that takes some work to understand.
* Lift is very flexible offering lots of alternatives for marking up HTML and different ways to create snippets and handle forms.
    * It supports a wide variety and frameworks for working with data.
    * It even supports multiple templating engines.
    * But the fact that there are many ways to do the same task can make it difficult to learn.
* I liked that Lift templates are pure HTML files.
    * But this design results in some HTML and CSS fragments being written in strings in Scala code.
    * It is a matter of taste which design is better.
* When working with Ajax, Lift tries to provide Scala helper functions that reduce the need to write JavaScript.
    * I have worked with other frameworks like ASP.NET web forms and Google Web Toolkit and am not a fan of the approach.
    * Can be inflexible, hard to debug and hard to integrate with external JavaScript libraries.
    * JavaScript is so important for modern web development that no web developer should try to avoid learning it deeply.
* Lift could really use a good introductory book or documentation. 
    * The few books on the topics are old and poorly-rated.
    * Online documentation exists, but I didn't find it very friendly to newcomers.








