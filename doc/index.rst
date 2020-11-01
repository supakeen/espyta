espyta
######

Welcome to the documentation of the ``espyta`` ESP OTA webhook software.

Introduction
============
``espyta`` is a straightforward web application written for Python 3.6 and up
using the Tornado_ web framework.

Usage
=====
You can run the built in ``espyta`` HTTP server through the ``espyta
http`` command. This will serve up a HTTP server listening on localhost
on port 8000.

Installation
============
You can install ``espyta`` from PyPI_ by running pip_ as follows:

  .. code:

  pip install espyta

I suggest you use a virtual environment for installation. There are extended
:ref:`installation` instructions available which explain how to do so.

Contributing
============
``espyta`` is a place that will accept your first contribution to an open
source project. The preferred place to start out is to visit our GitHub_ page
and look at the issues_ there. If you can solve any of them then you can send
a pull request. I will make sure to review your code.

If you are thinking about contributing a new feature then keep in mind that
``espyta`` is trying to stay as small and lean of a project as possible. Open
a ticket first if you have a specific feature in mind.

Table of Contents
=================
.. toctree::

   installation
   configuration
   autodoc
   changelog

.. _GitHub: https://github.com/supakeen/espyta
.. _issues: https://github.com/supakeen/espyta/issues
.. _PyPI: https://pypi.org
.. _pip: https://pip.pypa.org
.. _tornado: https://www.tornadoweb.org
.. _sqlalchemy: https://www.sqlalchemy.org
.. _pygments: http://pygments.org
.. _sqlite: https://sqlite.org
