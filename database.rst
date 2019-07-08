========
Database
========

Overview
--------

Kweb integrates nicely with `Shoebox <https://github.com/kwebio/shoebox>`_, a key-value store that supports the
observer pattern, and a sister project to Kweb.  Shoebox has both in-memory and persistent (filesystem) engines.

In the future Shoebox will support back-end cloud services like `AWS Pub/Sub Messaging <https://aws.amazon.com/pub-sub-messaging/>`_
and `Dynamo DB <https://aws.amazon.com/dynamodb/>`_, which would enable unlimited scalability.

We'll assume you've taken a minute or two to review `Shoebox <https://github.com/kwebio/shoebox>`_ and get the
general idea of how it's used.

Shoebox and State
-----------------

This example shows how *toVar* can be used to convert a value in a Shoebox to a `KVar </en/latest/state.html>`_:

.. code-block:: kotlin

    fun main() {
        data class User(val name : String, val email : String)
        val users = Shoebox<User>()
        users["aaa"] = User("Ian", "ian@ian.ian")

        Kweb(port = 16097) {
            doc.body.new {
                val user = toVar(users, "aaa")
                ul().new {
                    li().text(user.map {"Name: ${it.name}"})
                    li().text(user.map {"Email: ${it.email}"})

                }
            }
        }
    }

Other Databases
---------------

Kweb doesn't require you to use Shoebox, you're free to use any database, either directly, or via a database
abstraction layer of your choosing.

Working Example
---------------

For a more complete example of using Shoebox for persistent storage see the `to do demo <https://github.com/kwebio/kweb-core/tree/master/src/main/kotlin/io/kweb/demos/todo>`_.