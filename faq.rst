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

Vaadin comes with it's own set of UI widgets which you have to use, whereas with Kweb you can use your
favorite JavaScript UI framework (`Semantic UI <https://semantic-ui.com/>`_ is supported out of the box).
Also, being designed for Kotlin rather than Java allows Kweb to take full advantage of Kotlin's numerous
benefits, including a much more ergonomic API.

Is there a larger working example?
----------------------------------

Yes, here is a simple `todo list <https://github.com/kwebio/core/tree/master/src/main/kotlin/io/kweb/demos/todo>`_
implementation which demonstrates many of Kweb's features.

I have a question not answered here
-----------------------------------

Please join us in our `Gitter chat room <https://gitter.im/kwebio/Lobby>`_ where we'll do our best to answer
any questions you might have.