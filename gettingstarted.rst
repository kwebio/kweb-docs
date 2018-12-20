===============
Getting Started
===============

What you'll need
----------------

Some familiarity with `Kotlin <https://kotlinlang.org/>`_ is assumed, as is familiarity with
`Gradle <https://kotlinlang.org/>`_.  You should also have some familiarity with HTML.

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
     compile 'com.github.kwebio:core:LATEST_VERSION'
   }

You can find the LATEST_VERSION of Kweb on `JitPack <https://jitpack.io/#kwebio/core>`_.

Hello world
-----------

Type this and run it:

.. code-block:: kotlin

   import io.kweb.*
   import io.kweb.dom.element.*

   fun main() {
     Kweb(port = 8091) {
       doc.body.new {
         h1().text("Hello World!")
       }
    }
   }

Visit http://localhost:8091/ in your web browser and you should see the traditional greeting, translating to the
following HTML body:

.. code-block:: html

  <body>
    <h1>Hello World!</h1>
  </body>

This simple example already illustrates some important features of Kweb:

* Getting a kwebsite up and running is a breeze, no messing around with servlets, or third party webservers

* Your Kweb code will loosely mirror the structure of your page HTML

Hello world²
------------

This isn't some templating language, we have the full power of Kotlin at our disposal.  Witness the power of the
'for' loop:

.. code-block:: kotlin

   import io.kweb.*
   import io.kweb.dom.element.*

   fun main() {
     Kweb(port = 8091) {
       doc.body.new {
         for (x in 1..5) {
            h1().text("Hello World $x!")
         }
       }
    }
   }

To produce...

.. code-block:: html

  <body>
    <h1>Hello World 1!</h1>
    <h1>Hello World 2!</h1>
    <h1>Hello World 3!</h1>
    <h1>Hello World 4!</h1>
    <h1>Hello World 5!</h1>
  </body>
