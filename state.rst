========
State Management
========

The Building Blocks: KVars and KVals
---------------

Kweb makes use of the `observer pattern <https://en.wikipedia.org/wiki/Observer_pattern>`_, through the
`KVar <https://jitpack.io/com/github/kwebio/core/0.3.14/javadoc/io.kweb.state/-k-var/index.html>`_ class.
A KVar can contain a value of any type, which can change over time.  For example:

.. code-block:: kotlin

   val counter : KVar<Int> = KVar(0)

We create a counter KVar initialized with the value 0.  I've specified the type of *counter* for explanatory reasons
but this can also be inferred by Kotlin.

We can read and modify the value of a KVar:

.. code-block:: kotlin

    println("Counter value ${counter.value}")
    counter.value = 1
    println("Counter value ${counter.value}")

Will print:

.. code-block:: text

    Counter value 0
    counter value 1

KVars support powerful mapping semantics:

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

Note how counterDoubled updates automatically.

KVars, meet the DOM
-------------------

You can use a KVar (or KVal) to set the text of a DOM element:

.. code-block:: kotlin

    val name = KVar("John")
    li().text(name)

The neat part is that if the value of *name* changes, the DOM element text will update automatically.  Numerous
other functions on `Elements <https://jitpack.io/com/github/kwebio/core/0.3.14/javadoc/io.kweb.dom.element/-element/index.html>`_
support KVars in a similar manner, including `innerHtml() <https://jitpack.io/com/github/kwebio/core/0.3.14/javadoc/io.kweb.dom.element/-element/inner-h-t-m-l.html>`_
and `setAttribute() <https://jitpack.io/com/github/kwebio/core/0.3.14/javadoc/io.kweb.dom.element/-element/set-attribute.html>`_.

But what if you want to do more than just modify a single element based on a KVar, what if you want to modify
a whole tree of elements?  This is where the `render <https://jitpack.io/com/github/kwebio/core/0.3.14/javadoc/io.kweb.state.persistent/render.html>`_
function comes in.  This is a *core building block* of Kweb.

.. code-block:: kotlin

    val list = KVar(listOf("one", "two", "three"))

    Kweb(port = 1234) {
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

The simplicity of this mechanism may disguise how powerful it is, since render {} blocks can be nested, it is possible
to be much more selective about what parts of the DOM must be modified.

KVars, meet Persistent Storage
------------------------------

(TODO) Explain how KVars integrate with `Shoebox <https://github.com/kwebio/shoebox>`_.

DOM, meet Persistent Storage
----------------------------

(TODO) Explain how the previous two things tie together into being able to synchronize state in realtime between
your visitor's web browsers and your back-end database, all with very little effort.

Reversible KVar mappings
------------------------

If you check the type of *counterDoubled*, you'll notice that it's a *KVal* rather than a *KVar*.  The difference is
that `KVal <https://jitpack.io/com/github/kwebio/core/0.3.14/javadoc/io.kweb.state/-k-val/index.html>`_'s values may
not be modified directly, so this won't be permitted:

.. code-block:: kotlin

    counterDoubled.value = 20 // <--- This won't compile

The *KVar* class has a second
`map() <https://jitpack.io/com/github/kwebio/core/0.3.14/javadoc/io.kweb.state/-k-var/map.html>`_ function that takes
a *ReversableFunction* implementation which will produce a KVar which can be modified, and these modifications will
propagate back to the original KVar.

You shouldn't need this very often, but here is an example:

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

