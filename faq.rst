==========================
Frequently Asked Questions
==========================

Won't Kweb be slow relative to client-side web frameworks?
----------------------------------------------------------

No, Kweb's `immediate events <https://docs.kweb.io/en/latest/dom.html#immediate-events>`_ allow you to avoid
any server communication delay by responding immediately to DOM-modifying events.  This should address the majority
of scenarios where a server-driven approach might otherwise be sluggish.

What's the difference between Kweb and Vaadin?
----------------------------------------------

Kweb is *far* more lightweight than Vaadin.  At the time of writing, `kwebio/core <https://github.com/kwebio/core>`_ is about 4,351 lines of code, while
`vaadin/framework <https://github.com/vaadin/framework>`_ is currently 502,398 lines of code, almost a 100:1 ratio.

Vaadin doesn't have anything like Kweb's `immediate events <https://docs.kweb.io/en/latest/dom.html#immediate-events>`_,
which address the primary shortcoming of "server driven" web frameworks which was the lag from unnecessary server
round-trips.

Kweb was built natively for Kotlin, and takes advantage of all of its language features like `coroutines <https://kotlinlang.org/docs/reference/coroutines-overview.html>`_ and
the flexible DSL-like syntax.

That said, in its favor Vaadin is extremely mature, while Kweb is still pre-1.0.0.

Is there a larger working example?
----------------------------------

Yes, here is a simple `todo list <https://github.com/kwebio/core/tree/master/src/main/kotlin/io/kweb/demos/todo>`_
implementation which demonstrates many of Kweb's features.

I have a question not answered here
-----------------------------------

Please join us in our `Gitter chat room <https://gitter.im/kwebio/Lobby>`_ where we'll do our best to answer
any questions you might have.