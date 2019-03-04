============
Introduction
============

Motivation
----------

Most websites consist of at least two tightly coupled process components.  One runs in the browser, and the other runs on a
web server.

* Client
    * Runs in the browser
    * Responsible for user interaction
    * Typically written in JavaScript
    * Untrusted execution environment
    * Unreliable persistent local state***

* Server
    * Runs in a data center
    * Responsible for business logic
    * May be written in a wide variety of languages
    * Trusted execution environment
    * Reliable persistent global state***

The fact that often these two tightly coupled processes are written in different languages and must communicate
with each other over an HTTP connection adds significant complexity to the overall system.

This is the problem Kweb was designed to solve.  We do this by moving as much of the logic to the server as possible,
leaving a simple but powerful interface to the web browser where server-browser communications are handled automatically.

Kweb includes a typesafe `domain-specific language <https://en.wikipedia.org/wiki/Domain-specific_language>`_
for building and manipulating the `DOM <https://en.wikipedia.org/wiki/Document_Object_Model>`_ in a remote web browser.

Kweb runs on `Kotlin <https://kotlinlang.org/>`_, an excellent modern programming language that is rapidly growing in
popularity.  Kotlin is now Google's recommended language for `Android development <https://developer.android.com/kotlin/>`_.
According to Github, Kotlin was the `fastest growing <https://octoverse.github.com/projects#languages>`_ programming language
of 2018.

How does it work?
-----------------

Kweb is a self-contained Kotlin library that can be accessed easily for new or existing projects.  When Kweb receives
an HTTP request it responds with a small HTML file, including optimized instructions for building the page and a
client which connects back to the web server via a WebSocket.  The client then waits and listens for instructions
from the server.

To minimize latency, Kweb can `preload <https://docs.kweb.io/en/latest/dom.html#immediate-events>`_ instructions to
the browser to modify the DOM instantly in response to browser events, perhaps to disable a button or temporarily
display a "spinner."  This allows Kweb to respond instantly to user behavior without data making its way to the server and back.

Kweb is designed to be efficient.  All operations are handled asynchronously, and thread and memory usage are minimized.
Kweb runs on the JVM, which is 5-10 times `faster <https://benchmarksgame-team.pages.debian.net/benchmarksgame/faster/javascript.html>`_
than Node.js.

Kweb also takes an end-to-end approach to state.  You can bind the value of a DOM element to a field in your
database, and have it update on the server in realtime `automatically <https://docs.kweb.io/en/latest/state.html>`_.

Features
--------

* Free as in speech, licensed under `LGPL v3.0 <https://opensource.org/licenses/lgpl-3.0.html>`_

* Unifying codebase for your webapp, from database to DOM

* End-to-end Kotlin (`Why Kotlin? <https://steve-yegge.blogspot.com/2017/05/why-kotlin-is-better-than-whatever-dumb.html?m=1>`_)

* Easy to update server in realtime, as DOM values bind directly to a value in your database

* Statically typed HTML DSL; IDE catches bugs so you don't have to

* Efficient server-side rendering, DOM caching, and instruction preloading

* Ridiculously lightweight at less than 5,000 lines of code

Relevant Links
--------------

* Website: http://kweb.io/

* Source code: https://github.com/kwebio/core

* Feedback: https://github.com/kwebio/core/issues

* Help: https://gitter.im/kwebio/Lobby

* API Docs: https://jitpack.io/com/github/kwebio/core/0.3.15/javadoc/
