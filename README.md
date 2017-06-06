DemTools CiviCRM Tests
======================

This project aims to develop behaviour tests for DemTools CiviCRM using Behat.


Setup
-----

To get started, clone this repository:

    $ git clone --recursive https://github.com/nditech/demtools-civicrm-tests
    $ cd demtools-civicrm-tests

This project uses [Drumkit](http://drumk.it) to simplify setting up a testing
tools. First, let's bootstrap it:

    $ . d

Among other things, that will alter your `PATH` temporarily to add a local
`bin` directory where we'll install Behat, Composer and other components of our
test stack.

Next, let's install [Composer](http://getcomposer.org) and [Behat](http://behat.org) and the [Drupal Extension to Behat and Mink](https://behat-drupal-extension.readthedocs.io):

    $ make composer
    ...
    $ composer install
    ...

That should do it. We can test that these components are properly installed by running:

    $ composer --version
    Composer version 1.4.2 2017-05-17 08:17:52
    $ behat --version
    behat 3.1.0

