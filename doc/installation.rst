.. _installation:

Installation
############

The common way of installing ``espyta`` is by installing from PyPI. I suggest
you to use a virtual environment, these prevent accidentally updating libraries
that your other projects or even your operating system depend on.

For a Debian based distribution installation would look like this:

  .. code:

  python3 -m venv virtual-environment
  virtual-environment/bin/pip install espyta

After this you can run ``espyta`` in the following way:

  .. code:

  virtual-environment/bin/espyta http

This will start the built-in HTTP server on localhost, port 8000.

Running on boot
***************
If you wish to run ``espyta`` as an actual service there's a few more things
we will need to take care of. This bit of the documentation has strong ideas
about how to host a website and doesn't provide commands but general guidance.

This guide only applies to Linux and BSD systems though I am sure it can be
applied to Windows systems as well if you read synonymous terms where
necessary.

Prerequisites
=============

* A HTTP server or proxy such as nginx, haproxy, apache2, etc.

Setup
=====
Start by creating a separate user for your ``espyta`` this way we can make
use of the filesystem.

After you've created this user it's now time to setup an environment where
this users code will live. Let's get into their homedir and then perform the
initial section of this document as the user you've just created.

From now on I'll assume you have ``/home/youruser/virtual-environment`` with
``espyta`` installed to it.

We'll create a configuration file ``/home/youruser/espyta.toml`` with the
following content::

  database_uri = "mysql+pymysql://user:password@host/database"

If you want to configure more then read the :ref:`configuration` section.

Now that we have all of this setup it's time to test out ``espyta`` real
quick::

  /home/youruser/virtual-environment/bin/espyta --configuration-path /home/youruser/espyta.toml http

This should start ``espyta`` listening on localhost port 8000. Verify
that this is the case.

I will use systemd for this example since it comes pre-installed on most of
our systems nowadays.

Take the example systemd unit file from the repository_ and place it in
``/etc/systemd/system/espyta.service``. Then open the file and adjust
the paths to the paths you've created.

After you've done this you can ``systemctl daemon-reload`` and 
``systemctl enable espyta.service``. Check its status, if it has come up
verify that you can connect to localhost port 8000 as well and get served
with the ``espyta`` homepage.

Now it's time to configure our webserver to forward requests to ``espyta``.
I'll use ``nginx`` in this example but the ideas carry over to anything you
might be using.

Here's an example nginx configuration file::

  server {
          listen 443 ssl ;
          listen [::]:443 ssl ;
  
          root /var/www/empty;
  
          server_name myespyta.net; # managed by Certbot
  
          add_header X-Xss-Protection "1; mode=block" always;
          add_header X-Content-Type-Options "nosniff" always;
          add_header X-Frame-Options "SAMEORIGIN" always;
          add_header Content-Security-Policy "default-src 'self'" always;
          add_header Strict-Transport-Security "max-age=31536000; includeSubdomains; preload" always;
          add_header Referrer-Policy "no-referrer" always;
          add_header Feature-Policy "accelerometer 'none'; camera 'none'; geolocation 'none'; gyroscope 'none'; magnetometer 'none'; microphone 'none'; payment 'none'; usb 'none'" always;
   
          location / {
                  limit_req zone=myespyta burst=100;
                  proxy_pass http://127.0.0.1:8000;
                  proxy_set_header Host $host;
                  proxy_set_header X-Forwarded-Proto https;
          }
  
          access_log /home/youruser/myespyta.net_access.log;
  
          ssl_certificate /etc/letsencrypt/live/myespyta.net/fullchain.pem; # managed by Certbot
          ssl_certificate_key /etc/letsencrypt/live/myespyta.net/privkey.pem; # managed by Certbot
  }

Place that file in ``/etc/nginx/sites-enabled/myespyta.net`` and reload your
nginx. It is important that you pass the Host header and protocol as ``espyta``
will use these to build its URLs.


.. _repository: https://github.com/supakeen/espyta
