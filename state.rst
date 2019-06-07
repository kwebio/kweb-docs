============================
State & The Observer Pattern
============================

Building Blocks
---------------

Kweb makes use of the `observer pattern <https://en.wikipedia.org/wiki/Observer_pattern>`_, through the
`KVar <https://jitpack.io/com/github/kwebio/core/0.3.15/javadoc/io.kweb.state/-k-var/index.html>`_ class.

A KVar contains a single typed object, which can change over time.  For example:

.. code-block:: kotlin

   val counter = KVar(0)

Here we create a counter of type *KVar<Int>* initialized with the value 0.

We can also read and modify the value of a KVar:

.. code-block:: kotlin

    println("Counter value ${counter.value}")
    counter.value = 1
    println("Counter value ${counter.value}")
    counter.value++
    println("Counter value ${counter.value}")

Will print:

.. code-block:: text

    Counter value 0
    Counter value 1
    Counter value 2

KVars support powerful mapping semantics to create new KVars:

.. code-block:: kotlin

    val counterDoubled = counter.map { it * 2 }
    counter.value = 5
    println("counter: ${counter.value}, doubled: ${counterDoubled.value}")
    counter.value = 6
    println("counter: ${counter.value}, doubled: ${counterDoubled.value}")

Will print:

.. code-block:: text

    counter: 5, doubled: 10
    counter: 6, doubled: 12

Note that counterDoubled updates automatically.

.. note:: KVars should only be used to store values that are themselves immutable, such as an Int, String, or
    a Kotlin `data class <https://kotlinlang.org/docs/reference/data-classes.html>`_ with immutable parameters.

KVars and the DOM
-----------------

You can use a KVar (or KVal) to set the text of a DOM element:

.. code-block:: kotlin

    val name = KVar("John")
    li().text(name)

The neat part is that if the value of *name* changes, the DOM element text will update automatically.  It may
help to think of this as a way of "unwrapping" a KVar.

Numerous other functions on `Elements <https://jitpack.io/com/github/kwebio/core/0.3.15/javadoc/io.kweb.dom.element/-element/index.html>`_
support KVars in a similar manner, including `innerHtml() <https://jitpack.io/com/github/kwebio/core/0.3.15/javadoc/io.kweb.dom.element/-element/inner-h-t-m-l.html>`_
and `setAttribute() <https://jitpack.io/com/github/kwebio/core/0.3.15/javadoc/io.kweb.dom.element/-element/set-attribute.html>`_.

Binding a KVar to an input element's value
--------------------------------------------

For <input> elements you can set the value to a KVar, note that this connection is bidirectional, so any changes
to the KVar will be reflected in realtime in the browser, and similarly any changes in the browser by the user
will be reflected immediately in the KVar:

.. code-block:: kotlin

    Kweb(port = 2395) {
        doc.body.new {
             p().text("What is your name?")
            val clickMe = input(type = text)
            val nameKVar = KVar("Peter Pan")
            clickMe.value = nameKVar
            p().text(nameKVar.map { n -> "Hi $n!" })
        }
    }

This will also work for <option> and <textarea> elements which also have values.

Source: `ValueElement.value <https://github.com/kwebio/core/blob/master/src/main/kotlin/io/kweb/dom/element/creation/tags/form.kt#L85>`_

Rendering state to a DOM fragment
---------------------------------

But what if you want to do more than just modify a single element based on a KVar, what if you want to modify
a whole tree of elements?

This is where the `render <https://jitpack.io/com/github/kwebio/core/0.3.15/javadoc/io.kweb.state.persistent/render.html>`_
function comes in:

.. code-block:: kotlin

    val list = KVar(listOf("one", "two", "three"))

    Kweb(port = 16097) {
        doc.body.new {
            render(list) { rList ->
                ul().new {
                    for (item in rList) {
                        li().text(item)
                    }
                }
            }
        }
    }

Here, if we were to change the list:

.. code-block:: kotlin

    list.value = listOf("four", "five", "six")

Then the relevant part of the DOM will be redrawn instantly.

The simplicity of this mechanism may disguise how powerful it is, since render {} blocks can be nested, it's
possible to be very selective about what parts of the DOM must be modified in response to changes in state.

.. note:: Kweb will only re-render a DOM fragment if the value of the KVar actually changes.  Because of this
    it is most efficient to avoid "unwrapping" KVars with a *render()* or *.text()* call before you need to.  The
    `KVal.map {} <https://javadoc.jitpack.io/com/github/kwebio/core/0.3.15/javadoc/io.kweb.state/-k-val/map.html>`_
    function is a powerful tool for manipulating KVals and KVars without unwrapping them.

Reversible mappings
-------------------

If you check the type of *counterDoubled*, you'll notice that it's a *KVal* rather than a *KVar*.
`KVal <https://jitpack.io/com/github/kwebio/core/0.3.15/javadoc/io.kweb.state/-k-val/index.html>`_'s values may not be
modified directly, so this won't be permitted:

.. code-block:: kotlin

    counterDoubled.value = 20 // <--- This won't compile

The *KVar* class has a second
`map() <https://jitpack.io/com/github/kwebio/core/0.3.15/javadoc/io.kweb.state/-k-var/map.html>`_ function which takes
a *ReversableFunction* implementation.  This version of *map* will produce a KVar which can be modified, as follows:

.. code-block:: kotlin

    val counterDoubled = counter.map(object : ReversableFunction<Int, Int>("doubledCounter") {
        override fun invoke(from: Int) = from * 2
        override fun reverse(original: Int, change: Int) = change / 2
    })
    counter.value = 5
    println("counter: ${counter.value}, doubled: ${counterDoubled.value}")

    counterDoubled.value = 12 // <--- This wouldn't have worked before
    println("counter: ${counter.value}, doubled: ${counterDoubled.value}")

Will print:

.. code-block:: text

    counter: 5, doubled: 10
    counter: 6, doubled: 12

Data classes
------------

If your KVar contains a `data class <https://kotlinlang.org/docs/reference/data-classes.html>`_ then you can use
Kvar.property() to create a KVar from one of its properties which will update the original KVar if changed:

.. code-block:: kotlin

    data class User(val name : String)
    val user = KVar(User("Ian"))
    val name = user.property(User::name)
    name.value = "John"
    println(user) // Will print: KVar(User(name = "John"))
