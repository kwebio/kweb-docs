==========
DOM Basics
==========

Creating DOM Elements and Fragments
-----------------------------------

The DOM is built starting with an `Element <https://github.com/kwebio/kweb-core/blob/master/src/main/kotlin/kweb/Element.kt>`_, normally the `BodyElement <https://github.com/kwebio/kweb-core/blob/master/src/main/kotlin/kweb/html/BodyElement.kt>`_, which is obtained easily as follows:

.. code-block:: kotlin

   import kweb.*
   import kweb.dom.element.*

   fun main() {
     Kweb(port = 16097) {
        val body : BodyElement = doc.body
    }
   }

Let's create a <button> as a child of the <body>:

.. code-block:: kotlin

   import kweb.*
   import kweb.dom.element.*

   fun main() {
     Kweb(port = 16097) {
        doc.body {
            button().text("Click Me!")
        }
      }
    }

If you assign the button element to a val then you can also modify its attributes:

.. code-block:: kotlin

    val button = button()
    button.text("Click Me!")
    button.classes("bigbutton")
    button.setAttributeRaw("autofocus", true)

Or equivalently using Kotlin's apply `scope function <https://kotlinlang.org/docs/reference/scope-functions.html>`_:

.. code-block:: kotlin

    button().apply {
      text("Click Me!")
      classes("bigbutton")
      setAttributeRaw("autofocus", true)
    }

Or delete it:

.. code-block:: kotlin

    button.delete()

Adding children to an existing element
--------------------------------------

The DSL syntax makes it very easy to create elements and their children together:

.. code-block:: kotlin

    ul {
      li().text("One")
      li().text("Two")
    }

However we can also use the `new {}` function on `Element`s to add children to a pre-existing element:

.. code-block:: kotlin

    val unorderedList :  = ul()
    unorderedList.new {
      li().text("One")
      li().text("Two")
    }


Reading from the DOM
--------------------

Kweb can also read from the DOM, in this case the value of an <input> element:

.. code-block:: kotlin

   import kweb.*
   import kotlinx.coroutines.GlobalScope
   import kotlinx.coroutines.future.await
   import kotlinx.coroutines.launch

   fun main() {
       Kweb(port = 2395) {
           doc.body {
               val input: InputElement = input()
               input.on.submit {
                   GlobalScope.launch {
                       val value = input.getValue().await()
                       println("Value: $value")
                   }
               }
           }
       }
   }

Note that input.getValue() returns a `CompletableFuture<String> <https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html>`_.
This is because it can take up to several hundred milliseconds to retrieve from the browser, and we don't want the application
to block if it can be avoided.  Here we use Kotlin's very powerful `coroutines <https://kotlinlang.org/docs/reference/coroutines-overview.html>`_
features to avoid any unnecessary blocking.

.. note:: We discuss an even better way to read <input> values in the `Observer Pattern & State <https://docs.kweb.io/en/latest/state.html#binding-a-kvar-to-an-input-element-s-value>`_ section.

Supported HTML tags
-------------------

Kweb supports a significant subset of HTML tags like *button()*, *p()*, *a()*, *table()*, and so on.  You can find a
more complete list in `prelude.kt <https://github.com/kwebio/kweb-core/blob/master/src/main/kotlin/kweb/prelude.kt>`_
(scroll down to the *Functions* section).  This provides a nice statically-typed HTML DSL, fully integrated
with the Kotlin language.

If a tag doesn't have explicit support in Kweb that's not a problem.  For example, here is how you might use the
infamous and now-obsolete <blink> tag:

.. code-block:: kotlin

    doc.body {
        val blink = element("blink").text("I am annoying!")
    }

Further Reading
---------------

The `Element <https://github.com/kwebio/kweb-core/blob/master/src/main/kotlin/kweb/Element.kt>`_ class
provides many other useful ways to interact with DOM elements.
