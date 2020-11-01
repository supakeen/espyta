.. _configuration:

Configuration
#############
``espyta`` is configured in two ways, one is by arguments and the other is
through a configuration file.

The options available are dependent on the command you're running. You can
always pass the ``--configuration-path`` argument to ``espyta``.

Here is a quick example::

  espyta --configuration-path /tmp/foo.toml http --port 9000

The ``http`` subcommand takes a separate argument ``--port`` to override
the default listening port (8000).

File
****
The configuration file has a bunch more properties to configure ``espyta``
with. Here's an example file.

Options
*******
