django-bitcoin example project
================================

This proejct shows how to use `django-bitcoin to receive and send money in your Python + Django application <https://github.com/kangasbros/django-bitcoin>`_.

It shows how to create a web wallet, accept incoming bitcoins and then spend them.
This all is done interactively from a command line prompt.

Why to accept bitcoin in your online service
-----------------------------------------------

* Bitcoin is money with API: very easy to handle programmatically. You can do it even from UNIX command line.

* No upfront contracts or paperwork needed.

* `Bitcoin market liquity has risen to a level, so that it is easy for everyone to obtain bitcoins from LocalBitcoins.com <https://localbitcoins.com>`_. You can also convert bitcoins easily back to fiat currency, when you need to pay taxes.

* It is free from chargeback fraud. The system is based on mathematics instead of trust.

Prerequisitements
-------------------

* Python 2.7

* MySQL recommended and well tested, example runs on SQLite

* memcached

* `bitcoind <http://bitcoin.org/en/download>`_

Intermediate Django experience needed: how to configure Django project, MySQL, models, South migrations, using interactive Python shell.

How to consume piles of open source code from Github.

Installation
----------------

Installation is (`virtualenv based <http://opensourcehacker.com/2012/09/16/recommended-way-for-sudo-free-installation-of-python-software-with-virtualenv/>`_)::

    git clone xxx
    cd xxx
    virtualenv venv   # Create virtualenv folder caller venv
    . venv/bin/activate  # Active virtualenv

Install Python dependencies using *pip*::

    pip install -r requirements.txt

Tutorial
---------

`django-bitcoin <https://github.com/kangasbros/django-bitcoin>`_ creates a so caleld web wallet in your Django application.

* You need to have a `bitcoind <http://bitcoin.org/en/download>`_ installed on your desktop / server

* ``django-bitcoin`` reads transactions from ``bitcoind`` and duplicates them as Django models for easy handling

* ``django-bitcoin`` can send bitcoins from the system

* Bitcoin addresses are created and used on demand. The sending and receiving addresses do not have
  relationship between them. When you receive bitcoins to your system they do not leave out from
  the same address. This is who most bitcoin web wallets behave.

Configuring bitcoind
========================

* *django-bitcoin* communites with bitcoind over JSON-RPC protocol. You set the bitcoind address in ``settings.py``

* ``bitcoind`` transaction handling is done as polling, using Django management commands

Configure your bitcoind to accept connection with username and password.

Create file ``example/localsettings.py` and let's put in confidential settings::

    BITCOIND_CONNECTION_STRING = "http://miguel:passwor@example.com:8332"

    # How many bitcoin network confirmations are required until django-bitcoin considers the transaction
    # as received
    BITCOIN_MINIMUM_CONFIRMATIONS = 1

    # Use Django signals to tell the system when new money has arrived to your wallets
    BITCOIN_TRANSACTION_SIGNALING = True

Initializing database
==========================

``django-bitcoin`` uses South for its schema management.
Create a database::

    python manage.py syncdb
    python manage.pt migrate django_bitcoin

Do a test run
=================

Let's open the development web server and see that the Django admin is up with ``django-bitcoin``::

    python manage.py runserver_plus

Visit ``http://localhost:8000/admin`` to see the interface:

.. img:: https://raw.github.com/miohtama/django-bitcoin-example/master/images/admin.png
    :width: 800

Creating a wallet
====================

A wallet stores bitcoin value (actual bitcoins are stored in different bitcoin addresses managed by bitcoind).
A wallet has sending and receiving bitcoin addresses.

We need to first create a wallet.

Let's start interactive IPython prompt::

    python manage.py shell_plus

Then we create a wallet with an label. Usually if the application has only one wallet (not per user wallets)
you call this wallet instance to *master wallet*::

    master_wallet, created = Wallet.objects.get_or_create(label="master_wallet")

Then we need to have an receiving bitcoin address where this wallet can receive bitcoins::

    recv_address = master_wallet.receiving_address(fresh_addr=False)
    print recv_address

.. img:: https://raw.github.com/miohtama/django-bitcoin-example/master/images/wallet.png
    :width: 800


Get some bitcoins
=======================================

Accepting incoming transaction
====================================

