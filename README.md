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

To make sure you have the Valkyrie branch of the project, run:

    $ git checkout valkyrie

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

If this isn't working, another way to log in is by typing:

    $ vagrant ssh

You should now be logged in and see something like ubuntu@valkyrie:~$ 
To leave the vagrant box, type:

    $ exit


Deleting and creating a new Vagrant Box
---------------------------------------

If for some reason you need to get rid of your vagrant box, it can be deleted by typing the following when outside of the box (Warning: this will delete the box and any changes you made to it as well as deleting any files being stored in it. Do not do this unless you are willing to lose everything you have done inside of it.)

    $ vagrant destroy

You can then create a new blank box with the original configuration using:

    $ vagrant up


Inside the Vagrant Box
----------------------

Once inside the box, you can do the following to change users. To move to being the root user:

    $ sudo -s

From root user, you can become the aegir user with:

    $ su aegir

To move out a level from aegir to root, or root to ubuntu, press ctrl+d

If you’re ever unsure of what user you are, type:

    $ whoami


Most relevant files for Behat testing will be found in the Vagrant file. To go there and set up Behat, type:

    $ cd /vagrant
    $ make behat
    $ . d

The '. d' command will set up the correct path as before. Everytime you become the aegir user, you should do the '. d' command while in the vagrant folder. If at any point something isn't working, a good first thing to try is '. d'

To get to the typical Aegir page for managing sites and platforms, open your browser and go to the following url: [http://valkyrie.local](http://valkyrie.local)
If the vagrant box is running, you should now be on the standard aegir webpage, similar to aegir0.demcloud.org


Creating a new Drupal site:
---------------------------

From valkyrie.local, you can create a new site the same way you would normally.
As a refresher, to create a Civi site, go to the Sites tab on the top right, and then click the add sites button. Choose a domain name (if you don’t know which one to pick, use example.local, as that is the address Behat runs its tests on by default). If you have choices, choose NDI Pushbutton CiviCRM as the install profile. Don’t worry about aliases or redirects.

To get the url for the new page you created, go to terminal, make sure you are the aegir user, and then type: (Replace example.local with whichever domain name you gave your site)

    $ drush @example.local uli

Take the url it sends back and copy and paste it into your browser. You should now be on the Civi site.

You can check the civi page for the system status by going to example.local/civicrm/a/#/status , again replacing example.local with the domain for your site.


Using Behat
-----------

Now that you have a site running, you will want to configure Behat to run on your site. To do so, go to the vagrant folder and edit the behat.yml file. This can be done by typing:

    $ cd /vagrant
    $ vim behat.yml

(If vim is installed, the second command will open up the file using the vim text editor. If you don’t know how to use vim, take some time to go [https://www.linux.com/learn/vim-101-beginners-guide-vim](here) to familiarize yourself with it. Basic commands are press i to go into normal text editing mode, and ESC to leave that and return to the default command mode. From command mode, type :w and press enter to save a file. Typing :q and pressing enter exits vim.)

Inside of Behat.yml, find the field called base_url. By default, it will be followed by http://example.local . If this is the domain name you chose when creating the site you want to test, you can leave it as is. Otherwise, remove the url and replace it with the domain name for whichever site you are testing.

Next, find the field drupal_root. In that field, you will want to put something that looks like this:  '/var/aegir/platforms/demtools-civicrm-20170609' , where the last section should be the name of the platform your site was built with. To make sure you get the right address, in your browser go to the local aegir site (valkyrie.local) and click on the site you are trying to test. In the middle of the page next to 'Platform', click on the name of the platform. On the page this takes you to, you should see 'Publish path' followed by text starting /var/ . Copy this text after 'Publish path' and paste it into the Behat.yml file in the drupal_root field, making sure to put quotes around it like in the example above.

Behat.yml should now be configured properly, and you can exit the text editor.


To see a list of the pre-defined behat steps, as the aegir user, make sure you are in /vagrant and then type:

    $ behat -dl

To run Behat (again make sure you’re in /vagrant as the aegir user), just type: 

    $ Behat

All Behat tests should be stored in files within the features folder (/vagrant/features)
Remember that everytime when you become the Aegir user, in the vagrant folder you will need to type:

    $ . d


Fixing a Frozen Task Queue
--------------------------

If the Aegir task queue is ever frozen, here are two things to try:

1. As the aegir user, run:

    $ drush @hm sqlc

You will now be in the Aegir mySQL database. Now run:

    > DELETE FROM semaphore WHERE name='hosting_queue_tasks_running'

If this works, the task queue should start up momentarily. To exit mySQL, use ctrl+d.


2. To force an item in the task queue to go, click on the view button next to the item that is frozen. The address for the page you are now in should look something like   http://… /task/###   where ### is some number. Run the following lines of code, filling in ### with that number:

    $ drush @hm cc drush
    $ drush @hm hosting-task ### --force
