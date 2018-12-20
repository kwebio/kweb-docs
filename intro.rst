============
Introduction
============

A new paradigm for website architecture
---------------------------------------

Most websites are two pieces of tightly coupled software, *client* and *server*.

 * Client
    * Runs in the browser
    * Responsible for user interaction
    * Typically written in JavaScript
    * Runs in an untrusted  environment

 * Server
    * Runs in a datacenter
    * Responsible for business logic
    * May be written in a wide variety of languages
    * Runs in a trusted environment

The first runs on the web server, the second runs in the web browser.  Coders work hard to try to make the website
appear like a single piece of software to the website visitor, but this requires a lot of hidden complexity.

Kweb allows you to write your entire app as a single piece of software.  Kweb does this by moving as much of the
business logic to the server as possible, leaving a simple but powerful interface to the web browser.

It's written in `Kotlin <https://kotlinlang.org/>`_, a
powerful programming language that is rapidly growing in popularity, now being promoted by Google as a replacement
for Java in `Android development <https://developer.android.com/kotlin/>`_.

How does it work?
-----------------

Kweb is a self-contained Kotlin library that can be added easily to new or existing projects.  When Kweb receives
a HTTP request it responds with a small HTML file including instructions for building the page, and a
client which connects back to the web server via a WebSocket.  The client then waits and listens for instructions
from the server.

To minimize latency, Kweb can "preload" instructions to the browser to modify the DOM instantly in response to browser
events, perhaps to disable a button or temporarily display a "spinner".

Kweb is designed to be efficient.  All operations are handled asynchronously, thread and memory usage are minimized.
Kweb runs on the JVM, which is 5-10 times `faster <https://benchmarksgame-team.pages.debian.net/benchmarksgame/faster/javascript.html>`_
than Node.js.

Features
--------

* A single unified codebase for your webapp
* Code in Kotlin (`Why? <https://steve-yegge.blogspot.com/2017/05/why-kotlin-is-better-than-whatever-dumb.html?m=1>`_)
* Bind DOM values directly to a value in your database and have them update in realtime
* Makes efficient use of WebSockets and "preloading" of instructions to the browser
* A smooth upgrade path to WebAssembly
