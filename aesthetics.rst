==========
Aesthetics
==========

Kweb has out-of-the-box support for the excellent `Semantic UI <https://semantic-ui.com/>`_
framework, which helps create beautiful, responsive layouts using human-friendly HTML.

Kweb's Semantic UI plugin provides a convenient DSL to use Semantic UI.

Getting started
---------------

First tell Kweb to use the Semantic UI plugin:

.. code-block:: kotlin

    import io.kweb.plugins.semanticUI.*

    fun main() {
        Kweb(port = 4736, plugins = listOf(semanticUIPlugin)) {
            // ...
        }
    }

Now the plugin will add the Semantic UI CSS and JavaScript code to your website automatically.

Now, let's look at one of the simple examples from the `Semantic UI <https://semantic-ui.com/elements/input.html>`_
documentation:

.. code-block:: html

    <div class="ui icon input">
      <input type="text" placeholder="Search...">
      <i class="search icon"></i>
    </div>

We can translate this to Kweb fairly directly:

.. code-block:: kotlin

    import io.kweb.plugins.semanticUI.*

    fun main() {
        Kweb(port = 4736, plugins = listOf(semanticUIPlugin)) {
            div(semantic.ui.icon.input).new {
                input(type = text, placeholder = "Search...")
                i(semantic.search.icon)
            }
        }
    }

Other UI Frameworks
-------------------

It's easy to create Kweb plugins for many JavaScript tools and frameworks, taking full advantage of Kotlin's DSL
capabilities.

The `Semantic UI plugin implementation <https://github.com/kwebio/core/tree/master/src/main/kotlin/io/kweb/plugins/semanticUI>`_
itself can serve as an example.
