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

.. code-block:: gradle

   repositories {
     maven { url 'https://jitpack.io' }
   }

Then add Kweb to the dependencies block:

.. code-block:: gradle

   dependencies {
     compile 'com.github.kwebio:kweb-core:LATEST_VERSION'
   }

Replace LATEST_VERSION with the latest version of Kweb, which you can find on `https://jitpack.io/#kwebio/core <https://jitpack.io/#kwebio/core>`_,
along with instructions for other dependency management tools like Maven and Ivy.

Hello world
-----------

Create a new Kotlin file and type this:

.. code-block:: kotlin

   import io.kweb.*
   import io.kweb.dom.element.*
   import io.kweb.dom.element.creation.tags.*

   fun main() {
     Kweb(port = 16097) {
       doc.body.new {
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
a `DOM <https://en.wikipedia.org/wiki/Document_Object_Model>`_ in a remote web browser.

But this DSL can also do anything Kotlin can do, including `control flow <https://kotlinlang.org/docs/reference/control-flow.html>`_ features such as *for loops*:

Here is a simple example using a *for* loop:

.. code-block:: kotlin

   import io.kweb.*
   import io.kweb.dom.element.*
   import io.kweb.dom.element.creation.tags.*

   fun main() {
     Kweb(port = 16097) {
       doc.body.new {
         ul().new {
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
    <ul>
  </body>

You can use functions for modularization and reuse:

.. code-block:: kotlin

    fun main() {
        Kweb(port = 16097) {
            doc.body.new {
                ul().new {
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
