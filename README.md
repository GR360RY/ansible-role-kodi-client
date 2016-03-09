kodi-client
===========

[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.kodi--client-green.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/3098)

An Ansible role to install and configure Kodi on Ubuntu.

Requirements
------------

This role requires Ansible 2.0 or higher. Platform requirements are listed in the metadata file.
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

Downloads and Media folders layout if used with default variable values:

```
/mnt/media/
├── downloads
│   ├── complete
│   └── incomplete
├── movies
├── music
├── pictures
└── tv
```

Role Variables
--------------

```
# defaults file for kodi-client

# Helper variable. In use by other roles
kodi_client_enabled: yes

# Kodi Ubuntu ppa.
kodi_repo: 'ppa:team-xbmc/ppa'

# Run Kodi on login as part of Ubuntu desktop
# Set to 'kodi' to run Kodi as standalone Xsession.
# See examples.
kodi_xsession: ubuntu            

# Helper variable. In use by other roles
kodi_host: "{{ ansible_default_ipv4.address }}"
```

Optionally configure Kodi to use shared MySQL Database. See Examples.

```
# Use Kodi with MySQL Database. When `kodi-client` installed together with `kodi-mysql` role,
# will be set to `yes` from the defaults scope of `kodi-mysql` role
kodi_mysql_enabled: 

# Helper variable. When installed together with `kodi-mysql` role
# assume that mysql database runs on the same host as `kodi-client`
# See examples.
kodi_mysqldb_host: "{{ ansible_default_ipv4.address }}"

# Kodi Database User. Default set to `kodi` if installed together with `kodi-mysql` role
kodi_mysqldb_user:

# Kodi Database Password. Default set to `kodi` if installed together with `kodi-mysql` role
kodi_mysqldb_password:
```

Dependencies
------------

Role Name | Description
----------|-----------
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--common-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/htpc-common/)| Create htpc user and media folders|

Variables defined in `GR360RY.htpc-common` role:

```
# defaults file for htpc-common

htpc_user_username: htpc
htpc_user_password: htpc
htpc_user_group: htpc
htpc_user_shell: /bin/bash
htpc_user_sudo_access: yes
htpc_ssh_service: yes
htpc_create_media_folders: yes
htpc_zeroconf: yes
htpc_media_path: /mnt/media
htpc_media_movies: movies
htpc_media_tv: tv
htpc_media_music: music
htpc_media_pictures: pictures
htpc_downloads_complete: "{{ htpc_media_path }}/downloads/complete"
htpc_downloads_incomplete: "{{ htpc_media_path }}/downloads/incomplete"
```

Example Playbook
----------------

Create HTPC user `htpc` indentified by `htpc`. Use local storage and sqlite database.
Start Kodi session as part of Ubuntu Desktop. See folders layout in Overview Section.

```
- hosts: htpc
  become: yes

  roles:
    - role: GR360RY.kodi-client
```

Create HTPC user `foo` indentified by `bar`. Use local storage and sqlite database. Create Custom Media Folders.
Start Kodi as standalone Xsession:

```
- hosts: htpc
  become: yes

  vars:

    htpc_user_username: foo
    htpc_user_group: foo
    htpc_user_password: bar
    htpc_media_path: /media/big_disk
    htpc_media_movies: Movies
    htpc_media_tv: TV
    htpc_media_music: Music
    htpc_media_pictures: Pictures
    kodi_xsession: kodi


  roles:
    - role: GR360RY.kodi-client
```

Configure Kodi together with Kodi MySQL Database and NAS for sharing with other kodi clients:

```
- hosts: htpc
  become: yes

  roles:
    - role: GR360RY.kodi-client
    - role: GR360RY.kodi-mysql
    - role: GR360RY.htpc-nas
```

Use External MySQL Database for Kodi Media Library:

```
- hosts: htpc
  become: yes

  vars:
    kodi_mysql_enabled: yes
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
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--common-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/htpc-common)   | Create htpc user and media folders
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--nas-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/htpc-nas)         | Configure NAS ( NFS, CIFS and AFP )
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.kodi--client-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/kodi-client)   | Install Kodi Media Player
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.kodi--mysql-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/kodi-mysql)     | Install MySQL Backend for Kodi
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.deluge-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/deluge)              | Install Deluge Bittornet Client
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.nzbtomedia-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/nzbtomedia)      | Install NZBtoMedia Postprocessing
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.sickrage-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/sickrage)          | Install SickRage
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.couchpotato-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/couchpotato)    | Install CouchPotato
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--manager-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/htpc-manager) | Install htpc-manager
<!--
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.sabnzbd-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/sabnzbd)            | Install Sabnzbd
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.tvheadend-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/tvheadend)        | Install Tvheadend

Additional Info is available at [www.htpc-ansible.org](http://www.htpc-ansible.org)
 -->
License
-------

BSD

Author Information
------------------

Gregory Shulov
