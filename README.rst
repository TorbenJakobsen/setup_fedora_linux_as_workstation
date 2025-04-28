#####################################
  Setup Fedora Linux as Workstation
#####################################

Documents my personal setup.

A better way is to use Ansible, and I will get there eventually.

| Currently I am using Fedora 42 with Cosmic desktop. 
| Note: Cosmic is in alfa, but performs fast with only minor hiccups.

If you want to run in a container then 
https://github.com/geerlingguy/docker-fedora42-ansible
is a good place to start.


Intial Housekeeping
-------------------

Use `dnf5` instead of `dnf` (https://www.tecmint.com/dnf-vs-dnf5/)

.. code:: bash

  dnf5 -y update && dnf5 clean all

.. code:: bash

  dnf5 makecache \
  && dnf5 -y install \
    python3-pip \
    sudo \
    which \
    python3-libdnf5 \
  && dnf5 clean all


The `dnf5 makecache` command creates and downloads metadata for enabled repositories.