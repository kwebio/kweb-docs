===============
Getting Started
===============

What you'll need
----------------

Some familiarity with `Kotlin <https://kotlinlang.org/>`_ is assumed, as is familiarity with
`Gradle <https://gradle.org/>`_.  You should also have some familiarity with HTML.

Adding Kweb to your Gradle project
---------------------------

Add these to your repositories and dependencies {blocks} in your `build.gradle` or `build.gradle.kt` files. 

**NOTE:** Replace LATEST_VERSION with the latest version of Kweb, which you can find `here <https://github.com/kwebio/kweb-core/packages/1663696>`_.

Gradle (Groovy)
~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: gradle

   repositories {
    maven { url "https://maven.pkg.github.com/kwebio/kweb-core" }
    jcenter()
   }

.. code-block:: gradle

   dependencies {
     implementation 'io.kweb:kweb-core:LATEST_VERSION'
     
     // This (or another SLF4J binding) is required for Kweb to log errors
     implementation group: 'org.slf4j', name: 'slf4j-simple', version: '1.7.30'
   }

Gradle (Kotlin)
~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: kotlin

   repositories {
     maven("https://maven.pkg.github.com/kwebio/kweb-core")
     jcenter()
   }

.. code-block:: kotlin

   dependencies {
     implementation("io.kweb:kweb-core:LATEST_VERSION")
     
     // This (or another SLF4J binding) is required for Kweb to log errors
     implementation("org.slf4j:slf4j-simple:2.0.3")
   }

Hello world
-----------

Create a new Kotlin file and type this:

.. code-block:: kotlin

   import kweb.*

   fun main() {
     Kweb(port = 16097) {
       doc.body {
         h1().text("Hello World!")
       }
    }
   }

Run it, and then visit http://localhost:16097/ in your web browser to see the traditional greeting, translating to the
following HTML body:

.. code-block:: html

  <body>
    <h1>Hello World!</h1>
  </body>

This simple example already illustrates some important features of Kweb:

* Getting a kwebsite up and running is a breeze, no messing around with servlets, or third party webservers

* Your Kweb code will loosely mirror the structure of the HTML it generates

Hello world²
------------

One way to think of Kweb is as a
`domain-specific language (DSL) <https://en.wikipedia.org/wiki/Domain-specific_language>`_ for building and manipulating
a `DOM <https://en.wikipedia.org/wiki/Document_Object_Model>`_ in a remote web browser, while also listening for and handing DOM events.

Importantly, this DSL can also do anything Kotlin can do, including features like for loops, functions, coroutines, and classes.

Here is a simple example using an ordinary Kotlin *for loop*:

.. code-block:: kotlin

   import kweb.*

   fun main() {
     Kweb(port = 16097) {
       doc.body {
         ul {
             for (x in 1..5) {
                li().text("Hello World $x!")
             }
         }
       }
    }
   }

To produce...

.. code-block:: html

  <body>
    <ul>
        <li>Hello World 1!</li>
        <li>Hello World 2!</li>
        <li>Hello World 3!</li>
        <li>Hello World 4!</li>
        <li>Hello World 5!</li>
    </ul>
  </body>

You can use functions for modularization and reuse:

.. code-block:: kotlin

    fun main() {
        Kweb(port = 16097) {
            doc.body {
                ul {
                    for (x in 1..5) {
                        createMessage(x)
                    }
                }
            }
        }
    }

    private fun ElementCreator<ULElement>.createMessage(x: Int) {
        li().text("Hello World $x!")
    }

As you can see this is an extension function, which allows you to use the Kweb DSL within the newly created function.

Don't worry if you're unsure about this because you can use IntelliJ's `extract function <https://www.jetbrains.com/help/idea/extract-method.html>`_
refactoring to create these functions automatically.

Template Repository
-------------------

You can find a simple template Kweb project in `kwebio/kweb-template <https://github.com/kwebio/kweb-template>`_.
