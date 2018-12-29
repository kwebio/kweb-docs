============
Introduction
============

Motivation
----------

Most websites are two pieces of tightly coupled software, one of which runs in the browser, the client, and the
other runs on one or more web servers.

* Client
    * Runs in the browser
    * Responsible for user interaction
    * Typically written in JavaScript
    * Untrusted execution environment

* Server
    * Runs in a datacenter
    * Responsible for business logic
    * May be written in a wide variety of languages
    * Trusted execution environment

Coders work hard to try to make the website appear like a single piece of software to the website visitor, but this
requires a lot of hidden complexity.

Kweb allows you to write your entire app as a single piece of software.  It does this by moving as much of the
business logic to the server as possible, leaving a simple but powerful interface to the web browser.

It's written in `Kotlin <https://kotlinlang.org/>`_, a
powerful programming language that is rapidly growing in popularity, now being promoted by Google as a replacement
for Java in `Android development <https://developer.android.com/kotlin/>`_.

How does it work?
-----------------

Kweb is a self-contained Kotlin library that can be added easily to new or existing projects.  When Kweb receives
a HTTP request it responds with a small HTML file including optimized instructions for building the page, and a
client which connects back to the web server via a WebSocket.  The client then waits and listens for instructions
from the server.

To minimize latency, Kweb can `preload <https://docs.kweb.io/en/latest/dom.html#immediate-events>`_ instructions to
the browser to modify the DOM instantly in response to browser events, perhaps to disable a button or temporarily
display a "spinner".  This solves one of the most serious problems affecting some of Kweb's philosophical ancestors,
Wicket and Vaadin (which shared Kweb's "server driven" approach).

Kweb is designed to be efficient.  All operations are handled asynchronously, thread and memory usage are minimized.
Kweb runs on the JVM, which is 5-10 times `faster <https://benchmarksgame-team.pages.debian.net/benchmarksgame/faster/javascript.html>`_
than Node.js.

Kweb also takes and end-to-end approach to state.  You can bind the value of a DOM element to a field in your
database, and have it update in realtime `automatically <https://docs.kweb.io/en/latest/state.html>`_.

Features
--------

* A single unified codebase for your webapp
* Code in Kotlin (`Why? <https://steve-yegge.blogspot.com/2017/05/why-kotlin-is-better-than-whatever-dumb.html?m=1>`_)
* Bind DOM values directly to a value in your database and have them update in realtime
* Makes efficient use of WebSockets and "preloading" of instructions to the browser
* A smooth upgrade path to WebAssembly
