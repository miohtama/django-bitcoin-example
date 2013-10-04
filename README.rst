django-bitcoin example project
================================

This proejct shows how to use `django-bitcoin to receive and send money in your Python + Django application <https://github.com/kangasbros/django-bitcoin>`_.

Prerequisitements
-------------------

* Python 2.7

* MySQL

* memcached

* `bitcoind <http://bitcoin.org/en/download>`_

Intermediate Django experience needed: how to configure Django project, MySQL, models, South migrations, using interactive Python shell.

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

Configuring bitcoind
========================

* *django-bitcoin* communites with bitcoind over JSON-RPC protocol. You set the bitcoind address in ``settings.py``

* ``bitcoind`` transaction handling is done as polling, using Django management commands

Edit settings.py and let's put in::

    BITCOIND_CONNECTION_STRING = "http://miguel:passwor@example.com:8332"

    # How many bitcoin network confirmations are required until django-bitcoin considers the transaction
    # as received
    BITCOIN_MINIMUM_CONFIRMATIONS = 1

    # Use Django signals to tell the system when new money has arrived to your wallets
    BITCOIN_TRANSACTION_SIGNALING = True


Let's start interactive IPython prompt:




