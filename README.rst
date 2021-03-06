=================================
Getting Started to Sphinx-Gallery
=================================

.. image:: https://travis-ci.org/sphinx-gallery/sphinx-gallery.svg?branch=master
    :target: https://travis-ci.org/sphinx-gallery/sphinx-gallery

.. image:: https://readthedocs.org/projects/sphinx-gallery/badge/?version=latest
    :target: https://readthedocs.org/projects/sphinx-gallery/?badge=latest
    :alt: Documentation Status


A Sphinx extension that builds an HTML version of any Python
script and puts it into an examples gallery.

It is extracted from the scikit-learn project and aims to be an
independent general purpose extension.

Getting the package
===================

You can do a direct install via pip by using::

    $ pip install sphinx-gallery


Install as developer
--------------------

You can get the latest development source from our `Github repository
<http://https://github.com/sphinx-gallery/sphinx-gallery>`. You need
``setuptools`` installed in your system to install Sphinx-Gallery.

You will also need have installed:

* Sphinx
* matplotlib
* pillow

To install everything do::

    $ git clone https://github.com/sphinx-gallery/sphinx-gallery
    $ cd sphinx-gallery
    $ pip install -r requirements.txt
    $ python setup.py develop


.. _set_up_your_project:

Set up your project
===================

Let's say your Python project looks like this::

    .
    ├── doc
    │   ├── conf.py
    │   ├── index.rst
    │   └── Makefile
    ├── py_module
    │   ├── __init__.py
    │   └── mod.py
    └── examples
	├── plot_example.py
	├── example.py
	└── README.txt

Your Python module is on ``py_module``, examples on how to use it are
in ``examples`` and the ``doc`` folder hold the base documentation
structure you get from executing ``sphinx-quickstart``.


To get Sphinx-Gallery into your project we have to extend the Sphinx
``doc/conf.py`` file with:

.. code-block:: python

    import sphinx_gallery
    extensions = [
        ...
        'sphinx_gallery.gen_gallery',
        ]

This is to load Sphinx-Gallery as one of your extensions, the ellipsis
``...`` is to represent your other loaded extensions.

Now to declare your project structure, we add a configuration
dictionary for Sphinx-Gallery. The examples directory ``../examples``
is declared with a relative path from the ``conf.py`` file location.

.. code-block:: python

    sphinx_gallery_conf = {
	# path to your examples scripts
	'examples_dirs' : '../examples',
	# path where to save gallery generated examples
	'gallery_dirs'  : 'auto_examples'}

The ``gallery_dirs`` is the folder where Sphinx-Gallery will store the
converted Python scripts into rst files that Sphinx will process into
HTML.

The structure of the examples folder
------------------------------------

There are some extra instructions on how to present your examples to Sphinx-Gallery.

* A mandatory ``README.txt`` file with rst syntax to introduce your gallery
* ``plot_examples.py`` files. Python scripts that have to be executed
  and output a plot that will be presented in your gallery
* ``examples.py`` files. Python scripts that will not be executed but will be presented
  in the gallery

All the Python scripts in the examples folder need to have a docstring. Written
in rst syntax as it is used in the generated file for the example gallery.

You can have sub-folders in your ``examples`` directory, those will be
recursively scanned by the gallery extension and presented in the
gallery, as long as they also have a ``README.txt`` file. Sub-folders
have to respect the same structure examples folder.

If these instructions are not clear enough, this package uses itself, to generated
its own example gallery. So check the directory structure and the contents of the
files.

Building the documentation locally
----------------------------------

In your sphinx documentation directory, ``doc`` execute::

    $ make html

This will start the build of your complete documentation including the examples
gallery. Once documentation is build, our extension will have generated an ``auto_examples``
directory and populated it with rst files containing the gallery and each example.
Sphinx gives this files its regular processing and you can enjoy your
generated gallery under the same path. That means you will find the gallery in the path::

    _build/html/auto_examples/index.html

that you can open under your favorite browser.

Extending your Makefile
-----------------------
Once your gallery is working you might need remove completely all generated files by
sphinx-gallery to have a clean build, or you might want to build the gallery without
running the examples files. For this you need to extend your ``Makefile`` with::

    clean:
            rm -rf $(BUILDDIR)/*
            rm -rf auto_examples/
            rm -rf modules/generated/*

    html-noplot:
            $(SPHINXBUILD) -D plot_gallery=0 -b html $(ALLSPHINXOPTS) $(BUILDDIR)/html
            @echo
            @echo "Build finished. The HTML pages are in $(BUILDDIR)/html."

Remember that for in ``Makefile`` white space is significant and the indentation are tabs
and not spaces
