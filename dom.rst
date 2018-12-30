====================
Working with the DOM
====================

Modifying the DOM
-----------------

The DOM is built starting with an element, typically the BodyElement which is obtained easily as follows:

.. code-block:: kotlin

   import io.kweb.*
   import io.kweb.dom.element.*

   fun main() {
     Kweb(port = 8091) {
        val body : BodyElement = doc.body
    }
   }

Let's create a button element as a child of the body element, we do this using the *.new* function (which is
supported by all Element types):

.. code-block:: kotlin

        doc.body.new {
            val clickMe = button().text("Click Me!")
        }

As you can see, it's easy to set the text of an element, you can also modify its attributes:

.. code-block:: kotlin

            clickMe.setAttribute("class", "bigbutton")

Or delete it:

.. code-block:: kotlin

    clickMe.delete()

Supported HTML tags
-------------------

Kweb supports a significant subset of HTML tags like *button()*, *p()*, *a()*, *table()*, and so on.  You can find a
more complete list in the `API documentation <https://jitpack.io/com/github/kwebio/core/0.3.14/javadoc/io.kweb.dom.element.creation.tags/index.html>`_
(scroll down to the *Functions* section).

If an tag doesn't have explicit support in Kweb that's not a problem.  For example, here is how you might use the
famous <blink> tag:

.. code-block:: kotlin

    doc.body.new {
        val blink = element("blink").text("I am annoying!")
    }

Extending Kweb to support new HTML tags
---------------------------------------

Adding support for new tags to Kweb is fairly simple.  You can see how the existing functions are `implemented <https://github.com/kwebio/core/blob/master/src/main/kotlin/io/kweb/dom/element/creation/tags/other.kt>`_.
Feel free to submit a pull request `via Github <https://github.com/kwebio/core>`_, or just `submit an issue <https://github.com/kwebio/core/issues>`_
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

Listening for events
--------------------

You can attach event handlers to DOM elements:

.. code-block:: kotlin

    doc.body.new {
        val label = h1()
        label.text("Click Me")
        label.on.click {
            label.text("Clicked!")
        }
    }

Most if not all JavaScript event types are supported, and you can read event data like which key was pressed:

.. code-block:: kotlin

    doc.body.new {
        val input = input(type = text)
        input.on.keypress { keypressEvent ->
            println("Key Pressed: ${keypressEvent.key}")
        }
    }

Immediate events
----------------

Since the code to respond to events runs on the server, there may be a short lag between the action causing the
event and any changes to the DOM caused by the event handler.  This was a common complaint about server-driven
web frameworks like Vaadin, inhibiting their adoption.

Kweb has a solution - `onImmediate <https://jitpack.io/com/github/kwebio/core/0.3.14/javadoc/io.kweb.dom.element.events/on-immediate.html>`_:

.. code-block:: kotlin

    doc.body.new {
        val label = h1()
        label.text("Click Me")
        label.onImmediate.click {
            label.text("Clicked!")
        }
    }

This is identical to the first event listener example, except *on* has been replaced by *onImmediate*.

Kweb executes this event handler *on page render* and records the changes it makes to the DOM.  It then "pre-loads"
these instructions to the browser such that they are executed immediately when the event occurs without any server
round-trip.

.. warning:: Due to this pre-loading mechanism, the event handler for an *onImmediate* must limit itself to simple DOM modifications.  Kweb includes some runtime safeguards against this but they can't catch every problem so please use with caution.

Combination event handlers
--------------------------

A common pattern is to use both types of event handler on a DOM element.  The immediate handler might disable a clicked
button, or temporarily display some form of `spinner <https://loading.io/css/>`_.  The normal handler would then do
what it needs on the server, and then perhaps re-enable the button and remove the spinner.