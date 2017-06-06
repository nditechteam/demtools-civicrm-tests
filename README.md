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


Local Development & Testing
---------------------------

*Note that the instruction below can take an hour or longer to complete. So
prepare accordingly.*

The setup instructions above are sufficient to get up and running with the
testing tools themselves. However, you'll still need to build a DemTools
CiviCRM platform and install a site to run tests on. That's there
[Valkyrie](http://www.getvalkyrie.com) comes into play. It allows you to run a
full [Aegir](http://www.aegirproject.org) server inside of a
[Virtualbox](https://www.virtualbox.org) virtual machine built with
[Vagrant](https://www.vagrantup.com/).

To get started, you'll need to [install
Vagrant](https://www.vagrantup.com/intro/getting-started/install.html) and
[Virtualbox](https://www.virtualbox.org/manual/ch02.html#idm929). Once you've
done that, you can verify that they are properly installed by running the
following:

    $ vagrant --version
    Vagrant 1.8.5
    $ virtualbox --help
    Oracle VM VirtualBox Manager 5.1.14
    [...]

Now we're ready to install [Drush](http://drush.org) and Valkyrie:

    $ make drush
    [...]
    $ vagrant up
    [...]

Finally, you should see a welcome message that includes a login link to your
newly installed Aegir server. The easiest way to access it is to type:

    $ drush @v uli

This ought open a new browser window and log you into the Aegir site
automatically. This is a fully functional Aegir system, so it might be
worthwhile to check out the [Aegir documentation](http://docs.aegirproject.org)
before proceeding.

