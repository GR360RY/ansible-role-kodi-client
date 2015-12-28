kodi-client
===========

[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.kodi--client-green.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/3098)

An ansible role to install and configure Kodi on Ubuntu Desktop.

Requirements
------------

This role requires Ansible 1.6 or higher. Platform requirements are listed in the metadata file.
Make sure to download roles specified in **Dependencies** section if installed not with Ansible Galaxy.

Overview
--------

List of tasks that will be performed under kodi-client role:

1. Install Kodi Media Player.
2. Configure LightDM to autlogin HTPC user.
3. Run Kodi on login as part of Ubuntu desktop or as standalone Xsession.
4. Disable Screen Saver.
5. Configure Movies, TV and Pictures Sources in Kodi.
6. Enable Kodi Internal Web Server.
7. Optionally configure Kodi to use MySQL Backend ( see examples ).


Role Variables
--------------

 name          | default             | comment
---------------|---------------------|-------------
 kodi_repo     | 'ppa:team-xbmc/ppa' | 
 kodi_xsession | ubuntu              | Use 'kodi' to run Kodi in standalone mode.


Optional variables also defined in `GR360RY.kodi-mysql`. Kodi MySQL Database can be configured as a Backend ( see examples ). 

 name                   | comment
------------------------|-------------
 kodi_mysqldb_host      | Kodi MySQL Database Host
 kodi_mysqldb_user      | Database User
 kodi_mysqldb_password  | Database Password


Dependencies
------------

 Role Name| Description
----------|-----------
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--user-blue.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/4645) | Create htpc user on Linux.
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--media-blue.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/4926)| Create HTPC Media Folders.

Variables defined in `GR360RY.htpc-user` role:

 GR360RY.htpc-user        | Default       | Comment          
--------------------------|---------------|---------
 htpc_user_username       | htpc          | Username
 htpc_user_password       | htpc          | Password
 htpc_user_shell          | /bin/bash     | Shell
 htpc_user_ssh_service    | yes           | Install sshd service
 htpc_user_sudo_access    | yes           | Sudo Access

Variables defined in `GR360RY.htpc-media` role:

 GR360RY.htpc-media       | Default       | Comment          
--------------------------|---------------|---------
 htpc_media_path          | /mnt/media    |
 htpc_media_movies        | movies        |
 htpc_media_tv            | tv            |
 htpc_media_music         | music         |
 htpc_media_pictures      | pictures      |


Example Playbook
----------------

Create HTPC user `foo` indentified by `bar`. Use local storage and sqlite database. Create Custom Media Folders.
Start Kodi session without Ubuntu Desktop:

```
    - hosts: htpc

      vars:
        htpc_user_username: foo
        htpc_user_password: bar
        kodi_enable_ubuntu_desktop: False
        htpc_media_path: /media/big_disk
        htpc_media_movies: Movies
        htpc_media_tv: TV
        htpc_media_music: Music
        htpc_media_pictures: Pictures

      roles:
        - role: GR360RY.kodi-client
```

Configure Kodi together with Kodi Mysql Database ( download GR360RY.kodi-mysql role ):

```
    - hosts: htpc
      sudo: True

      roles:
        - role: GR360RY.kodi-client
        - role: GR360RY.kodi-mysql
```

Use External MySQL Database for Kodi Media Library:

```
    - hosts: htpc

      vars:
        kodi_mysqldb_host: 10.0.0.1
        kodi_mysqldb_user: kodi
        kodi_mysqldb_password: kodi

      roles:
        - role: GR360RY.kodi-client
```

HTPC-Ansible Project
--------------------

This role is part of HTPC-Ansible project that includes additional roles for building Ubuntu Based HTPC Server.

 Role name               | Comment
-------------------------|-----------------------------
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--user-blue.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/4645) |  Create htpc user on Linux
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--media-blue.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/4645)      | Create htpc media folders
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--nas-blue.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/4645)    | Configure NAS ( NFS, CIFS and AFP )
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.kodi--client-blue.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/3098)    | Install Kodi Media Player
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.kodi--mysql-blue.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/4645)    | Install MySQL Backend for Kodi
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.deluge-blue.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/4645)    | Install Deluged Bittornet Client
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.sabnzbd-blue.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/4645)    | Install Sabnzbd
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.nzbtomedia-blue.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/4645)    | Install NZBtoMedia Postprocessing
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.sickbeard-blue.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/4645)    | Install SickRage
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.couchpotato-blue.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/4645)    | Install CouchPotato
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--manager-blue.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/4645)    | Install htpc-manager
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.tvheadend-blue.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/4645)    | Install Tvheadend


Additional Info is available at [www.htpc-ansible.org](http://www.htpc-ansible.org)

License
-------

BSD

Author Information
------------------

Gregory Shulov