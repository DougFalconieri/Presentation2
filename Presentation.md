#Presentation 2: Lift

##Information Sources

* [Dallaway, Ricard. *Lift Cookbook.* O'Reilly, 2013](http://chimera.labs.oreilly.com/books/1234000000030/index.html)
* [Lift Website](http://liftweb.net/)

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
* 




