==========
DOM Basics
==========

Creating DOM Elements
---------------------

The DOM is built starting with an element, typically the BodyElement which is obtained easily as follows:

.. code-block:: kotlin

   import io.kweb.*
   import io.kweb.dom.element.*

   fun main() {
     Kweb(port = 16097) {
        val body : BodyElement = doc.body
    }
   }

Let's create a button element as a child of the body element, we do this using the *.new* function (which is
supported by all Element types):

.. code-block:: kotlin

   import io.kweb.*
   import io.kweb.dom.element.*

   fun main() {
     Kweb(port = 16097) {
        doc.body.new {
            val button = button().text("Click Me!")
        }
      }
    }

As you can see, it's easy to set the text of an element, you can also modify its attributes:

.. code-block:: kotlin

    button.setAttribute("class", "bigbutton")

Or delete it:

.. code-block:: kotlin

    button.delete()

Supported HTML tags
-------------------

Kweb supports a significant subset of HTML tags like *button()*, *p()*, *a()*, *table()*, and so on.  You can find a
more complete list in the `API documentation <https://jitpack.io/com/github/kwebio/core/0.3.15/javadoc/io.kweb.dom.element.creation.tags/index.html>`_
(scroll down to the *Functions* section).  This provides a nice statically-typed HTML DSL, fully integrated
with the Kotlin language.

If a tag doesn't have explicit support in Kweb that's not a problem.  For example, here is how you might use the
infamous and now-obsolete <blink> tag:

.. code-block:: kotlin

    doc.body.new {
        val blink = element("blink").text("I am annoying!")
    }

Extending Kweb to support new HTML tags
---------------------------------------

Adding support for new tags to Kweb is easy, take a look at `the source <https://github.com/kwebio/core/blob/master/src/main/kotlin/io/kweb/dom/element/creation/tags/other.kt>`_.
If you add some useful functionality please submit a pull request `via Github <https://github.com/kwebio/core>`_, or just `ask us <https://github.com/kwebio/core/issues>`_
and we'll do our best to add support.

Reading the DOM
---------------

You can read values from the DOM too:

.. code-block:: kotlin

    doc.body.new {
        val label = h1().text("What is your name?")
        val clickMe = input(type = text)
        clickMe.setValue("Foo Bar")
        GlobalScope.launch {
            val v : String = clickMe.getValue().await()
            label.text("Value: $v")
        }
    }

Notice that *clickMe.getValue()* doesn't return a String, it returns a `CompletableFuture\<String\> <https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/CompletableFuture.html>`_.
This is because retrieving something from the DOM requires some communication with the browser and
will take some time - and we don't want to block while we wait.

This allows us to take advantage of Kotlin's `coroutines <https://kotlinlang.org/docs/reference/coroutines/basics.html>`_
functionality to make this fairly seamless to the programmer (using `GlobalScope.launch and await <https://github.com/Kotlin/kotlinx.coroutines/blob/master/docs/basics.md>`_).

Yes, this example is a little pointless since we're just setting the value and then immediately reading it, more
realistic use cases will follow.

Further Reading
---------------

The `Element <https://github.com/kwebio/core/blob/master/src/main/kotlin/io/kweb/dom/element/Element.kt>`_ class
provides many other useful ways to interact with DOM elements.