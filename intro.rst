Motivation
##########

Most websites are two pieces of software.  The first runs on the web server, the second
runs in the web browser.  Coders work hard to try to make the website appear like a single piece
of software to the website visitor, but this requires a lot of hidden complexity.

Kweb allows you to write your entire app as a single piece of software, as
if it was running on a single computer.  It's written in [Kotlin](https://kotlinlang.org/), a
powerful new programming language that is rapidly growing in popularity.

Kweb does this by moving as much of the business logic to the server as possible, and then allowing
a programmer to interact directly with the browser DOM and JavaScript engine as if was running
locally to the server.

Through a clever set of optimizations, Kweb avoids unnecessary delays waiting for server roundtrips,
and minimizes network latency through use of WebSockets.

Key Features
############

* A single unified codebase for your webapp, no server/browser split
* Code in [Kotlin](https://steve-yegge.blogspot.com/2017/05/why-kotlin-is-better-than-whatever-dumb.html?m=1)
