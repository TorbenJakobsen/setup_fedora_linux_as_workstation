.. _Cosmic: https://system76.com/cosmic/

######################################
  Set up Fedora Linux as Workstation
######################################

****************
  Introduction
****************

Documents my *personal* setup.

A better way is to use Ansible, and I will get there eventually.

| Currently I am using Fedora 42 with Cosmic_ desktop. 
| **Note**: Cosmic_ is currently (2025-04-29) in alfa 7, but for my use performs fast with only minor hiccups.

If you want to run in a container then 
https://github.com/geerlingguy/docker-fedora42-ansible
is a good place to start.

Initial Housekeeping
====================

Package Manager
---------------

Use `dnf5` instead of `dnf` (https://www.tecmint.com/dnf-vs-dnf5/)
as it is a more modern and faster inplementation of :code:`dnf`.

Documentation on `dnf5` commands are available here: https://dnf5.readthedocs.io/en/latest/commands/index.html

Configure dnf
-------------

Improve download speed.

.. code:: bash

  sudo nano /etc/dnf/dnf.conf

Do note that you will potentionally affect other downloads and users on the same network.

.. code:: text

  [main]

  max_parallel_downloads=10
  fastestmirror=true

Initial Update
--------------

.. note:: 

  Make sure you have a network connection.

After the build of the installation media many changes will likely
have been added to your system.
So a full update is in order.

.. note::

  If you are used to :code:`apt` and other package managers; 
  :code:`dnf5 update` and :code:`dnf5 upgrade` does the same.

.. code:: bash

  sudo dnf5 -y update

.. code:: bash

  sudo dnf5 makecache

The `dnf5 makecache` command creates and downloads metadata for enabled repositories.

You can check available update packages beforehand:

.. code:: bash

  dnf5 check-update

Depending on your updates you should restart the system.
Strictly you could probably get away with restarting some sub-systems,
but it will likely be faster just restarting instead of micro-managing services and daemons.

.. code:: bash

  sudo dnf5 install -y dnf-utils

.. code:: bash

  dnf5 needs-restarting

https://www.mankier.com/1/needs-restarting

Third-party repositories
------------------------

Open Software Center and *optionally* add extra repositories.

EPEL (Extra Packages for Enterprise Linux) - NO
-----------------------------------------------

See https://idroot.us/install-epel-repository-fedora-42/

  A common misconception among Linux users new to Fedora is that EPEL repositories are necessary or beneficial for Fedora systems. 
  In reality, Fedora already contains virtually all packages found in EPEL — and often newer versions. 
  This situation exists because EPEL packages originate from Fedora before being adapted for Enterprise Linux distributions.

  Installing EPEL on Fedora 42 is generally unnecessary and potentially problematic. 
  Since Fedora serves as the upstream source for EPEL packages, 
  adding EPEL to Fedora creates a circular relationship that could lead to package conflicts or dependency issues. 
  Most software needs are already met through Fedora’s extensive default repositories.

https://docs.fedoraproject.org/en-US/epel/

COPR (Cool Other Package Repo) - YES
------------------------------------

See https://idroot.us/install-packages-copr-repositories-fedora/

.. code:: bash

  sudo dnf5 install dnf-plugins-core

.. code:: bash

  dnf5 copr --help

Example: COPR is used to install ghostty.

.. code:: bash

  sudo dnf5 copr enable pgdev/ghostty

RPMFusion
---------

Enable RPMFusion repositories for Fedora.

  RPM Fusion provides software that the Fedora Project or Red Hat doesn't want to ship. 
  That software is provided as precompiled RPMs for all current Fedora versions and current
  Red Hat Enterprise Linux or clones versions; 
  you can use the RPM Fusion repositories with tools like yum and PackageKit. 

  RPM Fusion is a merger of Dribble, Freshrpms, and Livna; our goal is to simplify end-user experience by grouping as much add-on software as possible in a single location. Also see our FoundingPrinciples. 

An example is Nvidia drivers.

Free

.. code:: bash

  sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm

Non-free.

.. code:: bash

  sudo dnf install https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm

.. code:: bash

  sudo dnf upgrade --refresh

Install Firmware Updates
------------------------

.. code:: bash

  sudo fwupdmgr refresh --force

.. code:: bash

  sudo fwupdmgr get-updates

.. code:: bash

  sudo fwupmgr update


Install prefered Terminal and Shell
===================================

This topic has its own page:
https://github.com/TorbenJakobsen/setup_terminal_and_shell.

install :code:`ansible`
-----------------------

https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html

install the full package:

.. code:: bash

  sudo dnf5 install ansible

It is also possible to install just the core and modules of your choosing.

:code:`ssh` Keys
-----------------

To access :code:`git` you will need a public key.

Install :code:`gìt`
-------------------

.. code:: bash

  sudo dnf5 install git

Follow: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

.. code:: bash

  git config --global user.email "TorbenJakobsen@users.noreply.github.com"
  git config --global user.name "Torben Jakobsen"
  git config --global init.defaultBranch "main"

Of course you should use **your** name and and mail address.

*Depending on your preferences*. 
Personally I like :code:`code` to open. 
You may prefer :code:`vi`, :code:`vim`, :code:`neovim`, or the default.

.. code:: bash

  git config --global core.editor "code --wait"

Recommended: Optionally install public key in GitHub
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

I use GitHub and other services and have other servers that I want to access.

To install public key in GitHub follow:
https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account?tool=webuing-a-new-ssh-key-to-your-github-account?tool=webui

Install Visual Studio Code
--------------------------

https://code.visualstudio.com/docs/setup/linux#_rhel-fedora-and-centos-based-distributions

.. code:: bash 

  sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc

.. code:: bash 

  echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\nautorefresh=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" | sudo tee /etc/yum.repos.d/vscode.repo > /dev/null

Now install :code:`code`:

.. code:: bash 

  sudo dnf5 check-update

.. code:: bash 

  sudo dnf5 install code

The general guide is here:
<https://code.visualstudio.com/docs/setup/linux>

Install :code:`code` Extensions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can use the command line to list and install/uninstall extensions.

Examples:

.. code:: bash 

  code --list-extensions
  code --install-extension redhat.vscode-yaml
  code --uninstall-extension timonwong.shellcheck

My personal choice of themeis:

.. code:: bash

  code --install-extension catppuccin.catppuccin-vsc        \
  code --install-extension catppuccin.catppuccin-vsc-icons

My personal preferences are:

| :code:`ms-python.python`
| :code:`ms-python.vscode-pylance`

.. code:: text

  aaron-bond.better-comments
  davidanson.vscode-markdownlint
  docker.docker
  donjayamanne.python-environment-manager
  dracula-theme.theme-dracula
  github.codespaces
  github.vscode-github-actions
  ibm.ibm-developer
  ibmconsulting.ica
  inferrinizzard.prettier-sql-vscode
  jakebecker.elixir-ls
  lextudio.iis
  lextudio.restructuredtext-pack
  mechatroner.rainbow-csv
  ms-azuretools.vscode-docker
  ms-python.black-formatter
  ms-python.debugpy
  ms-python.isort
  ms-python.python
  ms-python.vscode-pylance
  ms-toolsai.jupyter
  ms-toolsai.jupyter-keymap
  ms-toolsai.jupyter-renderers
  ms-toolsai.vscode-jupyter-cell-tags
  ms-toolsai.vscode-jupyter-slideshow
  ms-vscode-remote.remote-containers
  ms-vscode-remote.remote-ssh
  ms-vscode-remote.remote-ssh-edit
  ms-vscode.makefile-tools
  ms-vscode.remote-explorer
  njpwerner.autodocstring
  quarto.quarto
  redhat.ansible
  redhat.vscode-yaml
  sapos.yeoman-ui
  saposs.app-studio-remote-access
  saposs.app-studio-toolkit
  saposs.sap-guided-answers-extension
  saposs.vscode-ui5-language-assistant
  saposs.xml-toolkit
  sapse.sap-ux-annotation-modeler-extension
  sapse.sap-ux-application-modeler-extension
  sapse.sap-ux-fiori-tools-extension-pack
  sapse.sap-ux-help-extension
  sapse.sap-ux-service-modeler-extension
  shuworks.vscode-table-formatter
  sonarsource.sonarlint-vscode
  swyddfa.esbonio
  tamasfe.even-better-toml
  trond-snekvik.simple-rst
  wesbos.theme-cobalt2
  wholroyd.jinja

Install Docker
--------------

Follow:
https://docs.docker.com/engine/install/fedora/

The general installation:
https://docs.docker.com/engine/install/


A CLI alternative to Docker Desktop is :code:`lazydocker`.

.. note::

  To have docker running you need the engine running...

Install :code:`podman`  
----------------------

Install and configure default shell and Terminal
------------------------------------------------

See
<https://github.com/TorbenJakobsen/setup_terminal_and_shell>
for how to configure :code:`zsh` as default shell and more.

Other packages to consider
--------------------------

* draw.io
* tldr (tealdeer)

.. code:: bash 

  sudo dnf5 install tealdeer

duf

.. code:: bash 

  sudo dnf5 install duf


https://github.com/Canop/dysk

dysk

https://ostechnix.com/get-linux-filesystems-information-using-dysk/

install rust

https://ostechnix.com/install-rust-programming-language-in-linux/

zig

Boot Manager
============

.. code:: bash
  
  grub2-mkconfig -o /boot/grub2/grub.cfg
