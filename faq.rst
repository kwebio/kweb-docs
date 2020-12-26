==========================
Frequently Asked Questions
==========================

Won't Kweb be slow relative to client-side web frameworks?
----------------------------------------------------------

No, Kweb's `immediate events <https://docs.kweb.io/en/latest/events.html#immediate-events>`_ allows you to avoid
any server communication delay by responding immediately to DOM-modifying events.

Kweb is designed to be efficient by default, minimizing both browser and server CPU/memory.

If you encounter a situation in which Kweb is slow please `submit a bug <https://github.com/kwebio/kweb-core/issues>`_.

What's the difference between Kweb and Vaadin?
----------------------------------------------

Of all web frameworks we're aware of, `Vaadin <https://vaadin.com/>`_ is the closest in design and philosophy to Kweb,
but there are also important differences:

- Kweb is *far* more lightweight than Vaadin.  At the time of writing,
  `kweb-core <https://github.com/kwebio/kweb-core>`_ is about 4,351 lines of code, while
  `vaadin/framework <https://github.com/vaadin/framework>`_ is currently 502,398 lines of code, almost a 100:1 ratio!


- Vaadin doesn't have any equivalent feature to Kweb's `immediate events <https://docs.kweb.io/en/latest/events.html#immediate-events>`_,
  which has led to frequent `complaints <https://stackoverflow.com/a/22848521/16050>`_ of sluggishness from Vaadin users
  because a server round-trip is required to update the DOM.


- Vaadin brought a more desktop-style of user interface to the web browser, but since then we've realized that
  users generally prefer their websites to look like websites.


- This is why Kweb's philosophy is to be a thin interface between server logic and the user's browser, leveraging existing
  tools from the JavaScript ecosystem `when it makes sense <https://docs.kweb.io/en/latest/style.html>`_.


- Kweb was built natively for Kotlin, and takes advantage of all of its language features like `coroutines <https://kotlinlang.org/docs/reference/coroutines-overview.html>`_ and
  the flexible DSL-like syntax.  Because of this Kweb code can be a lot more concise, without sacrificing readability.


- In Vaadin's favor, it has been a commercial product since 2006, it is extremely mature and has a vast
  developer ecosystem, while Kweb is still pre-1.0.

Is there a larger working example?
----------------------------------

Yes, here is a simple `todo list <https://github.com/kwebio/kweb-demos/tree/master/todoList>`_
implementation which demonstrates many of Kweb's features.

You can find a copy of this demo running here: http://demo.kweb.io:7659/

It's running on a $50/month EC2 instance.  Try visiting the same list URL in two different browser windows and notice
how they synchronize in realtime.

You can see a number of other example Kweb projects here: `kwebio/kweb-demos <https://github.com/kwebio/kweb-demos/tree/master/>`_

How do I enable HTTPS?
----------------------

Very easily, please see `this example <https://github.com/kwebio/kweb-demos/blob/master/https/src/HttpsApp.kt>`_.

Can I embed Kweb within an Android app?
---------------------------------------

Yes!  Please see `kweb-demos/tree/master/android <https://github.com/kwebio/kweb-demos/tree/master/android>`_ for an example.

What about templates?
---------------------

Kweb replaces templates with something better - a typesafe HTML DSL embedded within a powerful programming language.  

If you like you could separate out the code that interfaces directly to the DOM - this would be architecturally closer to a template-based approach, but we view it as a feature that this paradigm isn't forced on the programmer.

Why risk my project on a framework I just heard of?
---------------------------------------------------

Picking a framework is stressful.  Pick the wrong one and perhaps the company behind it goes out of business,
meaning your entire app is now built on something obsolete.  We've been there.

Kweb's development is driven by a community of volunteers.  We welcome contributions from anyone, but Kweb doesn't depend on any sponsoring company.

Because of the powerful abstractions it's built on, Kweb also has the advantage of simplicity (<5k loc). This makes
it easier for people to contribute, and less code means fewer bugs.

That said, Kweb is still pre-1.0, one of the implications being that we can and will make breaking API changes, and
new releases are quite frequent.

How is "Kweb" pronounced?
-------------------------

"Kay-web".

Can Kweb be embedded in an existing Ktor app?
---------------------------------------------

Yes!  Please see `this example <https://github.com/kwebio/kweb-demos/blob/master/ktorFeature/src/FeatureApp.kt>`_.

I have a question not answered here
-----------------------------------

Feel free to `ask us a question <https://github.com/kwebio/core/issues/new>`_ on Github Issues, but please search first to see whether it has
already been answered.
