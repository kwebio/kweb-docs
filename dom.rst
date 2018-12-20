====================
Working with the DOM
====================

Modifying the DOM
-------------------

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

        clickMe.setAttribute("class", "nicebutton")

Reading the DOM
---------------

(TODO) Document reading values from the DOM tree

Listening for events
--------------------

(TODO) Explain how to attach DOM listeners and handle them.

Immediate events
----------------

(TODO) Explain how to respond to DOM events without a server round trip