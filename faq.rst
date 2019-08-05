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

Of all web frameworks we're aware of, `Vaadin <https://vaadin.com/>`_ is the closest in design and philosophy to Kweb.
In many ways Kweb is a philosophical descendant of Vaadin.  This makes Vaadin one of the most useful frameworks to compare
Kweb to, as there are also very important differences:

- Kweb is *far* more lightweight than Vaadin.  At the time of writing,
  `kwebio/core <https://github.com/kwebio/kweb-core>`_ is about 4,351 lines of code, while
  `vaadin/framework <https://github.com/vaadin/framework>`_ is currently 502,398 lines of code, almost a 100:1 ratio!


- Vaadin doesn't have any equivalent feature to Kweb's `immediate events <https://docs.kweb.io/en/latest/events.html#immediate-events>`_,
  which has led to frequent `complaints <https://stackoverflow.com/a/22848521/16050>`_ of sluggishness from Vaadin users
  because a server round-trip is required to update the DOM.


- Vaadin brought a more desktop-style of user interface to the web browser, but since then we've realized that
  users generally prefer their websites to look like websites.


- This is why Kweb's philosophy is to be a thin interface between server logic and the user's browser, leveraging existing
  tools from the JavaScript ecosystem `when it makes sense <https://docs.kweb.io/en/latest/aesthetics.html>`_.


- Kweb was built natively for Kotlin, and takes advantage of all of its language features like `coroutines <https://kotlinlang.org/docs/reference/coroutines-overview.html>`_ and
  the flexible DSL-like syntax.  Because of this Kweb code can be a lot more concise, without sacrificing readability.


- In Vaadin's favor, it has been a commercial product since 2006, it is extremely mature and has a vast
  developer ecosystem, while Kweb is still pre-1.0.

Is there a larger working example?
----------------------------------

Yes, here is a simple `todo list <https://github.com/kwebio/kweb-core/tree/master/src/main/kotlin/io/kweb/demos/todo>`_
implementation which demonstrates many of Kweb's features.

You can find a copy of this demo running here: http://demo.kweb.io:7659/

It's running on a $50/month EC2 instance.  Try visiting the same list URL in two different browser windows and notice
how they synchronize in realtime.

What about templates?
---------------------

Kweb replaces templates with something better - a typesafe HTML DSL embedded within a powerful programming language.  

If you like you could separate out the code that interfaces directly to the DOM - this would be architecturally closer to a template-based approach, but we view it as a feature that this paradigm isn't forced on the programmer.

Why risk my project on a framework I just heard of?
---------------------------------------------------

Picking a framework is stressful.  Pick the wrong one and perhaps the company behind it goes out of business,
meaning your entire app is now built on something obsolete.  We've been there.

Somewhat unusually for a framework, rather than being tied to a company with paid contributors, Kweb is an individual
passion project, with a growing number of voluntary contributors.

This may mean we lack the resource of salaried contributors, but we also avoid the dependence on the success of any one
company, reducing long-term risk significantly.

Because of the powerful abstractions it's built on, Kweb also has the advantage of simplicity (<5k loc). This makes
it easier for people to contribute, and less code means fewer bugs.

That said, Kweb is still pre-1.0, one of the implications being that we can and will make breaking API changes, and
new releases are quite frequent.

How is "Kweb" pronounced?
-------------------------

One syllable, like "queue" and "web" smashed together.

I can't say "Kweb" to my boss!
-------------------------------

Find a new job.

I have a question not answered here
-----------------------------------

Please join us in our `Gitter chat room <https://gitter.im/kwebio/Lobby>`_ where we'll do our best to answer
any questions you might have.
