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

        val body : BodyElement = doc.body
        body.new {
            val clickMe = button().text("Click Me!")
        }

As you can see, it's easy to set the text of an element, you can also modify its attributes:

.. code-block:: kotlin

            clickMe.setAttribute("class", "bigbutton")

Or delete it:

.. code-block:: kotlin

    clickMe.delete()

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

Notice that *clickMe.getValue() doesn't return a String, it returns a *CompletableFuture<String>.
This is because retrieving something from the DOM requires some communication with the browser and
will take some time - and we don't want to block while we wait.

This allows us to take advantage of Kotlin's `coroutines <https://kotlinlang.org/docs/reference/coroutines/basics.html>`_
functionality to make this fairly seamless to the programmer (using *GlobalScope.launch* and *await()*).

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

Since the code to respond to events runs on the server, there will be a delay between the event occurring
and any modifications to the DOM caused by the event.

Fortunately Kweb has a solution, but it must be used with caution:

.. code-block:: kotlin

    doc.body.new {
        val label = h1()
        label.text("Click Me")
        label.onImmediate.click {
            label.text("Clicked!")
        }
    }

This is virtually identical to the first event listener example, except instead of using *on* we're using
*onImmediate*.

When Kweb encounters this, it immediately runs the event handler and records the changes it makes to the DOM
(in this case changing the *text* value of *label*).  Kweb then "pre-loads" these instructions to the browser
such that they are executed immediately when the event occurs without any server round-trip.

**Caution**

Due to this pre-loading mechanism, the event handler for an *onImmediate* should limit itself to DOM
changes.  If it attempts to read or modify any server state then this will occur once on page render which almost
certainly isn't what you want.

**Combining on and onImmediate**

A common pattern is to use both types of event handler on a DOM element.  The immediate handler might disable
a clicked button, or temporarily display some form of `spinner <https://loading.io/css/>`_.  The normal handler
would then do what it needs on the server, and then perhaps re-enable the button and remove the spinner.