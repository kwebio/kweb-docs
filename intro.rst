============
Introduction
============

A new paradigm for webapps
--------------------------

Most websites are two pieces of tightly coupled software:

 * Client
   * Runs in the browser
   * Responsible for user interaction
   * Typically written in JavaScript
   * Runs in an "untrusted" environment

 * Server
   * Runs in a datacenter
   * Responsible for

The first runs on the web server, the second
runs in the web browser.  Coders work hard to try to make the website appear like a single piece
of software to the website visitor, but this requires a lot of hidden complexity.

Kweb allows you to write your entire app as a single piece of software, as
if it was running on a single computer.  It's written in `Kotlin <https://kotlinlang.org/>`_, a
powerful new programming language that is rapidly growing in popularity.

Kweb does this by moving as much of the business logic to the server as possible, and then allowing
a programmer to interact directly with the browser DOM and JavaScript engine as if was running
locally to the server.

Through a clever set of optimizations, Kweb avoids unnecessary delays waiting for server roundtrips,
and minimizes network latency through use of WebSockets.

Build Powerful, fast, beautiful websites, painlessly and quickly
----------------------------------------------------------------

* A single unified codebase for your webapp, no artificial separation between browser and server
* Code in Kotlin (`Why? <https://steve-yegge.blogspot.com/2017/05/why-kotlin-is-better-than-whatever-dumb.html?m=1>`_)
* Efficient