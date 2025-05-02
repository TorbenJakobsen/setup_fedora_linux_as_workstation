
.. _Cosmic: https://system76.com/cosmic/

#####################################
  Setup Fedora Linux as Workstation
#####################################

Introduction
------------

Documents my personal setup.

A better way is to use Ansible, and I will get there eventually.

| Currently I am using Fedora 42 with Cosmic_ desktop. 
| **Note**: Cosmic_ is currently (2025-04-29) in alfa 7, but for my use performs fast with only minor hiccups.

If you want to run in a container then 
https://github.com/geerlingguy/docker-fedora42-ansible
is a good place to start.

Initial Housekeeping
--------------------

Package manager
~~~~~~~~~~~~~~~~

Use `dnf5` instead of `dnf` (https://www.tecmint.com/dnf-vs-dnf5/)
as it is a more modern and faster inplementation of :code:`dnf`.

Documentation on `dnf5` commands are available here: https://dnf5.readthedocs.io/en/latest/commands/index.html

Initial update
~~~~~~~~~~~~~~

.. note:: 

  Make sure you have a network connection.

After the build of the installation media many chnages will likely
have been added to your system.
So a full update is in place.
Note: If you are used to :code:`apt` and other package managers; 
:code:`dnf5 update` and :code:`dnf5 upgrade` does the same update.

.. code:: bash

  sudo dnf5 -y update

.. code:: bash

  sudo dnf5 makecache

The `dnf5 makecache` command creates and downloads metadata for enabled repositories.

You can check available update packages beforehand:

.. code:: bash

  dnf5 check-update

Depending on your updates you should restart the system.
Strictly you could probaly get away with restarting some sub-systems,
but it will likely be faster just restarting instead of micro-managing services and daemons.

.. code:: bash

  sudo dnf5 -y dnf-utils

  dnf5 needs-restarting

https://www.mankier.com/1/needs-restarting

install :code:`ansible`
-----------------------


https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html

install the full package:

.. code:: bash

  sudo dnf5 install ansible

It is also possible to install just the core and modules of your choosing.

:code:`ssh`` Keys
-----------------

To access :ode:`git` you will need a public key.

Install :code:`gìt`
-------------------

.. code:: bash

  sudo dnf5 install git

Follow:
https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

.. code:: bash

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"
  git config --global init.defaultBranch "main"

Depending on your preferences. 
Personally I like :code:`code` to open. You may prefer :code:`vim`.

.. code:: bash

  git config --global core.editor "code --wait"

Optionally install public key in GitHub
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

I use GitHub and other services and have other servers that I want to access.

To install public key in GitHub follow ...

Install Visual Studio Code
--------------------------

https://code.visualstudio.com/docs/setup/linux#_rhel-fedora-and-centos-based-distributions

.. code:: bash 

  sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
  echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\nautorefresh=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" | sudo tee /etc/yum.repos.d/vscode.repo > /dev/null

.. code:: bash 

  dnf check-update
  sudo dnf install code

The general guide is here:
https://code.visualstudio.com/docs/setup/linux

Install :code:`code` Extensions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| :code:`ms-python.python`
| :code:`ms-python.vscode-pylance`

Install Docker
--------------

Follow:
https://docs.docker.com/engine/install/fedora/

The general installation:
https://docs.docker.com/engine/install/

Setup `zsh` as default shell
----------------------------

Configure omz

Configure shell prompt
