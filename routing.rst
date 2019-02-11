===========
URL Routing
===========

In a web application, routing is the process of using URLs to drive the user interface (UI). URLs are
a prominent feature in every single web browser, and have several main functions:

* Bookmarking - Users can bookmark URLs in their web browser to save content they want to come back to later.
* Sharing - Users can share content with others by sending a link to a certain page.
* Navigation - URLs are used to drive the web browser’s back/forward functions.

Traditionally, visiting a different URL would cause the page to be refreshed, but in modern web applications
this often isn't necessary, and Kweb handles this for you automatically.

A simple example
----------------

.. code-block:: kotlin

    import io.kweb.Kweb
    import io.kweb.dom.element.creation.tags.a
    import io.kweb.dom.element.new
    import io.kweb.routing.route
    import io.kweb.state.*

    fun main() {
        Kweb(port = 16097) {
            doc.body.new {
                route {
                    path("/users/{userId}") { params ->
                        val userId = params.getValue("userId")
                        h1().text(userId.map { "User id: $it" })
                    }
                    path("/lists/{listId}") { params ->
                        val listId = params.getValue("listId")
                        h1().text(listId.map { "List id: $it" })
                    }
                }
            }
        }
    }

Now, if you visit http://localhost:16097/users/997, you will see:

.. code-block:: html

    <h1>User id: 997</h1>

You can have as many path()s as you need, each with it's own path definition.  The definition can
contain parameters wrapped in {braces}.

The value of these parameters can then be retrieved from the *params* map, but note that the values are
wrapped in a *KVal<String>* object.  This means that you can use all of Kweb's `state management <https://docs.kweb.io/en/latest/state.html>`_
features to render parts of the DOM using this value.

The key advantage here is that if the URL changes the page can be updated without a full page refresh, but
rather only changing the parts of the DOM that need to change - this is much faster and more efficient.

Modifying the URL
-----------------

The *routing* and *path* DSL illustrated above is built on a

You can obtain *and modify* the URL of the current page using *url(simpleUrlParser)* within the Kweb {block}.
This returns a KVar<`URL <http://galimatias.mola.io/>`_ > which you can use to read and *modify* the
page URL:

.. code-block:: kotlin

    import io.kweb.Kweb
    import io.kweb.dom.element.creation.tags.a
    import io.kweb.dom.element.new
    import io.kweb.routing.route
    import io.kweb.state.*

    fun main() {
        Kweb(port = 16097) {
            doc.body.new {
                val path = url(simpleUrlParser).path
                route {
                    path("/") {
                        path.value = "/number/1"
                    }
                    path("/number/{num}") { params ->
                        val num = params.getValue("num").toInt()
                        a().text(num.map {"Number $it"}).on.click {
                            path.value = "/number/${num.value + 1}"
                        }
                    }
                }
            }
        }
    }

If you visit http://localhost:16097/ the URL will immediately update to http://localhost:16097/number/1
without a page refresh, and you'll see a hyperlink with text "Number 1".  If you click on this link
you'll see that the number increments (both in the URL and in the link text).

An even more elegant approach that would also work would be to replace:

.. code-block:: kotlin

    path.value = "/number/${num.value + 1}"

...with...

.. code-block:: kotlin

    num.value++

This works because the KVars always work bidirectionally, so can be used both to read and modify that
part of the page URL, resulting in an automatic re-render of the necessary DOM elements.
