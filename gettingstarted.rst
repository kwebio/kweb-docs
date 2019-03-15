===============
Getting Started
===============

What you'll need
----------------

Familiarity with `Kotlin <https://kotlinlang.org/>`_ and `Gradle <https://gradle.org/>`_ is assumed, as is working knowledge of HTML.

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

Replace "LATEST_VERSION" with the name of the latest version of Kweb, which you can find on `JitPack <https://jitpack.io/#kwebio/core>`_.

Hello world!
-----------

Create a new Kotlin file and type this:

.. code-block:: kotlin

   import io.kweb.*
   import io.kweb.dom.element.*

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

This simple example illustrates some important features of Kweb; including, 1) Kwebsites are a breeze to set up because there's no need to mess around with servlets or third party webservers, and 2) Kweb code will loosely mirror the structure of the HTML it generates.

Hello world²
------------

One way to think of Kweb is as a
`domain-specific language (DSL) <https://en.wikipedia.org/wiki/Domain-specific_language>`_ for building and manipulating
a `DOM <https://en.wikipedia.org/wiki/Document_Object_Model>`_ in a remote web browser.

Kweb takes full advantage of Kotlin's ability to embed DSLs, allowing full use of
`control flow <https://kotlinlang.org/docs/reference/control-flow.html>`_ features such as *for* loops.


Here is a simple example using a *for* loop:

.. code-block:: kotlin

   import io.kweb.*
   import io.kweb.dom.element.*

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

The above code will produce the following:

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
