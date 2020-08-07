============
Introduction
============

Motivation
----------

Modern websites consist of at least two `tightly coupled <https://en.wikipedia.org/wiki/Coupling_(computer_programming)>`_ components, one runs in the browser, the other on the server.  These are often written in different programming languages and must communicate with each other over an HTTP connection.

Kweb's goal is to eliminate this server/browser separation so that your webapp's architecture is determined by the problem you're solving, rather than the limitations of today's tools.

How does it work?
-----------------

Kweb is a self-contained Kotlin library that can be added easily to new or existing projects.  When Kweb receives
a HTTP request it responds with the initial HTML page, and some JavaScript that connects back to the web server via a WebSocket.  The page then waits and listens for instructions from the server, while notifying the server of relevant browser events.

A common concern about this approach is that the user interface might feel sluggish if it is server driven. Kweb solves this problem by `preloading <https://docs.kweb.io/en/latest/events.html#immediate-events>`_ instructions to
the browser to be executed immediately on browser events without a server round-trip.

We've designed Kweb to be efficient in both the browser and server, and makes effective use of Kotlin's concurrency features, particularly `coroutines <https://kotlinlang.org/docs/reference/coroutines-overview.html>`_.

Features
--------

* Allows the problem to determine your architecture, not the server/browser divide

* End-to-end Kotlin (`Why Kotlin? <https://steve-yegge.blogspot.com/2017/05/why-kotlin-is-better-than-whatever-dumb.html?m=1>`_)

* Keep the web page in sync with your back-end data in realtime, Kweb does all the plumbing for you

* Server-side HTML rendering with `rehydration <https://developers.google.com/web/updates/2019/02/rendering-on-the-web>`_

* Efficient instruction preloading to avoid unnecessary server communication

* Very lightweight, Kweb is less than 5,000 lines of code

Relevant Links
--------------

* Website: https://kweb.io/

* Source code: https://github.com/kwebio/kweb-core

* API documentation: http://dokka.kweb.io/kweb-core/index.html

* Example projects: https://github.com/kwebio/kweb-demos

* Live Demo: http://demo.kweb.io:7659/

* Questions/Feedback/Bugs: https://github.com/kwebio/kweb-core/issues

* API Docs: https://javadoc.jitpack.io/com/github/kwebio/kweb-core/latest/javadoc/index.html
