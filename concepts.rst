========
Concepts
========

State Management: KVars and KVals
---------------------------------

Kweb makes use of the `observer pattern <https://en.wikipedia.org/wiki/Observer_pattern>`_, through the *KVar* class.
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

Reversible KVar mappings
-------------------

If you check the type of *counterDoubled*, you'll notice that it's a *KVal* rather than a *KVar*.  The difference is
that *KVal*'s values may not be modified directly, so this won't be permitted:

.. code-block:: kotlin

    counterDoubled.value = 20 // <--- This won't compile

The *KVar* class has a second *.map()* function that takes a *ReversableFunction* implementation which will produce
a KVar which can be modified, and these modifications will propagate back to the original KVar.

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

