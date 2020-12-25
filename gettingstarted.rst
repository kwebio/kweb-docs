===============
Getting Started
===============

What you'll need
----------------

Some familiarity with `Kotlin <https://kotlinlang.org/>`_ is assumed, as is familiarity with
`Gradle <https://gradle.org/>`_.  You should also have some familiarity with HTML.

Adding Kweb to your project
---------------------------

Kweb is distributed via JitPack, so add this to the repositories {block} in your build.gradle:

Groovy DSL
~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: gradle

   repositories {
     maven { url 'https://jitpack.io' }
     jcenter()
   }

Kotlin DSL
~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: kotlin

   repositories {
     maven("https://jitpack.io")
     jcenter()
   }

Then add Kweb to the dependencies block:

Groovy DSL
~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: gradle

   dependencies {
     compile 'com.github.kwebio:kweb-core:LATEST_VERSION'
     
     // This (or another SLF4J binding) is required for Kweb to log errors
     compile group: 'org.slf4j', name: 'slf4j-simple', version: '1.7.30'
   }

Kotlin DSL
~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: kotlin

   dependencies {
     compile("com.github.kwebio:kweb-core:LATEST_VERSION")
     
     // This (or another SLF4J binding) is required for Kweb to log errors
     compile("org.slf4j:slf4j-simple:1.7.30")
   }

Replace LATEST_VERSION with the latest version of Kweb, which you can find on `https://jitpack.io/#kwebio/kweb-core <https://jitpack.io/#kwebio/kweb-core>`_.  

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
