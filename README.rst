========================
django-phonenumber-field
========================

.. image:: https://github.com/stefanfoulis/django-phonenumber-field/workflows/Test/badge.svg
    :target: https://github.com/stefanfoulis/django-phonenumber-field/workflows/Test/badge.svg
.. image:: https://img.shields.io/coveralls/stefanfoulis/django-phonenumber-field/develop.svg
    :target: https://coveralls.io/github/stefanfoulis/django-phonenumber-field?branch=main

A Django library which interfaces with `python-phonenumbers`_ to validate, pretty print and convert
phone numbers. ``python-phonenumbers`` is a port of Google's `libphonenumber`_ library, which
powers Android's phone number handling.

.. _`python-phonenumbers`: https://github.com/daviddrysdale/python-phonenumbers
.. _`libphonenumber`: https://code.google.com/p/libphonenumber/

Included are:

* ``PhoneNumber``, a pythonic wrapper around ``python-phonenumbers``' ``PhoneNumber`` class
* ``PhoneNumberField``, a model field
* ``PhoneNumberField``, a form field
* ``PhoneNumberField``, a serializer field
* ``PhoneNumberPrefixWidget``, a form widget for selecting a region code and
  entering a national number. Requires the `Babel`_ package be installed.
* ``PhoneNumberInternationalFallbackWidget``, a form widget that uses national numbers unless an
  international number is entered.  A ``PHONENUMBER_DEFAULT_REGION`` setting needs to be added
  to your Django settings in order to know which national number format to recognize.

.. _`Babel`: https://pypi.org/project/Babel/

Installation
============

::

    pip install django-phonenumber-field[phonenumbers]

As an alternative to the ``phonenumbers`` package, it is possible to install
the ``phonenumberslite`` package which has `a lower memory footprint
<https://github.com/daviddrysdale/python-phonenumbers#memory-usage>`_.

::

    pip install django-phonenumber-field[phonenumberslite]

Basic usage
===========

First, add ``phonenumber_field`` to the list of the installed apps in
your ``settings.py`` file::

    INSTALLED_APPS = [
        ...
        'phonenumber_field',
        ...
    ]

Then, you can use it like any regular model field::

    from phonenumber_field.modelfields import PhoneNumberField

    class MyModel(models.Model):
        name = models.CharField(max_length=255)
        phone_number = PhoneNumberField()
        fax_number = PhoneNumberField(blank=True)

Internally, PhoneNumberField is based upon ``CharField`` and by default
represents the number as a string of an international phonenumber in the database (e.g
``'+41524204242'``).

The object returned is a PhoneNumber instance, not a string. If strings are used to initialize it,
e.g. via ``MyModel(phone_number='+41524204242')`` or form handling, it has to be a phone number
with country code.

Settings
========

``PHONENUMBER_DB_FORMAT``
-------------------------

Store phone numbers strings in the specified format.

Default: ``"E164"``.

Choices:

- ``"E164"``,
- ``"INTERNATIONAL"``,
- ``"NATIONAL"`` (requires ``PHONENUMBER_DEFAULT_REGION``),
- ``"RFC3966"`` (requires ``PHONENUMBER_DEFAULT_REGION``).

``PHONENUMBER_DEFAULT_REGION``
------------------------------

`ISO-3166-1 <https://en.wikipedia.org/wiki/ISO_3166-1#Current_codes>`_
two-letter country code indicating how to interpret regional phone numbers.

Default: ``None``.

``PHONENUMBER_DEFAULT_FORMAT``
------------------------------

String formatting of phone numbers.

Default: ``"E164"``.

Choices:

- ``"E164"``,
- ``"INTERNATIONAL"``,
- ``"NATIONAL"``,
- ``"RFC3966"``.

Running tests
=============

tox needs to be installed. To run the whole test matrix with the locally
available Python interpreters and generate a combined coverage report::

    tox

run a specific combination::

    tox -e py36-djmain,py39-djmain
