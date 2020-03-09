============
Introduction
============

Motivation
----------

Modern websites consist of at least two `tightly coupled <https://en.wikipedia.org/wiki/Coupling_(computer_programming)>`_ components, one runs in the browser, the other on the server.  These are often written in different languages and communicate with each other over a HTTP connection.

Kweb makes your life easier by eliminating this server/browser separation. 

We do this by moving as much of the logic to the server as possible, leaving a simple but powerful interface to the web browser where server-browser communications are handled automatically.

Kweb includes a typesafe `domain-specific language <https://en.wikipedia.org/wiki/Domain-specific_language>`_
for building and manipulating the `DOM <https://en.wikipedia.org/wiki/Document_Object_Model>`_ in a remote web browser.

Kweb runs on `Kotlin <https://kotlinlang.org/>`_, an excellent modern programming language that is rapidly growing in
popularity.  Kotlin is now Google's recommended language for `Android development <https://developer.android.com/kotlin/>`_.

According to Github, Kotlin was the `fastest growing <https://octoverse.github.com/projects#languages>`_ programming language
of 2018.

How does it work?
-----------------

Kweb is a self-contained Kotlin library that can be added easily to new or existing projects.  When Kweb receives
a HTTP request it responds with a small HTML file including optimized instructions for building the page, and a
client which connects back to the web server via a WebSocket.  The client then waits and listens for instructions
from the server.

To minimize latency, Kweb can `preload <https://docs.kweb.io/en/latest/dom.html#immediate-events>`_ instructions to
the browser to modify the DOM instantly in response to browser events, perhaps to disable a button or temporarily
display a "spinner".  This allows Kweb to respond instantly to user behavior without a server round-trip.

Kweb is designed to be efficient.  All operations are handled asynchronously, thread and memory usage are minimized.
Kweb runs on the JVM, which is 5-10 times `faster <https://benchmarksgame-team.pages.debian.net/benchmarksgame/faster/javascript.html>`_
than Node.js.

Kweb also takes an end-to-end approach to state.  You can bind the value of a DOM element to a field in your
database, and have it update in realtime `automatically <https://docs.kweb.io/en/latest/state.html>`_.

Features
--------

* Automatically manages communication between web browser and server

* End-to-end Kotlin (`Why Kotlin? <https://steve-yegge.blogspot.com/2017/05/why-kotlin-is-better-than-whatever-dumb.html?m=1>`_)

* Changes to data in your database automatically update websites in realtime

* Server-side HTML rendering with `rehydration <https://developers.google.com/web/updates/2019/02/rendering-on-the-web>`_

* Efficient instruction preloading to avoid unnecessary server communication

* Very lightweight, Kweb is less than 5,000 lines of code

Relevant Links
--------------

* Website: https://kweb.io/

* Source code: https://github.com/kwebio/kweb-core

* Demo: http://demo.kweb.io:7659/

* Questions/Feedback/Bugs: https://github.com/kwebio/kweb-core/issues

* API Docs: https://javadoc.jitpack.io/com/github/kwebio/kweb-core/latest/javadoc/index.html
