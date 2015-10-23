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

### Snippet Examples

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




