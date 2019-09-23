# Testing a stackable multienvironment directory layout for Ansible
[![Buy me a coffee](https://img.shields.io/badge/$-BuyMeACoffee-blue.svg)](https://www.buymeacoffee.com/jobcespedes)
## Quickstart:
1. Check that [Docker](https://docs.docker.com/install/) and [Docker SDK for Python](https://docker-py.readthedocs.io/en/stable/) are installed in your system.
2. Download this repo: ```git clone --recursive https://github.com/jobcespedes/demo-multienv && cd demo-multienv```
3. Stack the environment (production, over stage and dev): ```ansible-playbook -i localhost, multienv.yml```
4. Test running a _foo_ playbook: ```ansible-playbook foo.yml```
5. Modify files inside ```environments/<environment>```
6. Test repeating step #4

## Description
This project is for testing a stackable multienvironment directory layout for Ansible. The main goal is to have little data and file duplication in a multienvironment Ansible project while maintaining Ansible groups and host granularity. The environments are separated. However, they are based over the others in a hierarchical way. So each one only has the necessary files and artifacts. The current approach to achieve this is to use [unionfs](http://unionfs.filesystems.org/) and a container for stacking the environments.

## Problem
Many systems are deployed in a multienvironment context, for example: development, stage and production.  These environments often share variables and artifacts. In Ansible, there are different methods to work in this context. For example, [separate directory layout](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html#alternative-directory-layout) and [soft links](https://www.digitalocean.com/community/tutorials/how-to-manage-multistage-environments-with-ansible). However, it can end with a considerable amount of data and duplicate files between environments, exposing variables to all hosts or adding much more complexity to playbooks.

## Approach
A [separate directory layout](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html#alternative-directory-layout) is used as recomended by Ansible Best Practices. Then they are unified as one using [unionfs](http://unionfs.filesystems.org/). The resulting directory is used as the current Ansible environment (inventory, vars, artifacts, etc)

This way, one can modify each environment in its respective directory: envionments/dev for example. And instruct Ansible to use the unified directory, `union_environment`, as a stacked environment: production+stage+dev

Refer to [multienv Ansible role](https://github.com/jobcespedes/multienv/blob/master/README.md) for how to configure the stackable environment

## Other options considered
* **Ansible plugins**: the logic could be added using Ansible plugins. It has to handled inventory, vars and other artifacts (files, templates, among others). [Unionfs](http://unionfs.filesystems.org/) approach could handled those without addional plugins
* **Overlayfs**: It is another union filesystem included in the current linux kernel. However, it cannot handle online modification of the lower layers. If one wants to modify the development environment (lower) while on stage (upper), an operation like remount is needed to reflect changes in the union mount.
* **Rsync and Inotify**. Using inotify to monitor modifications and rsync to sync into one environment. One consideration about this is how to handle deleted files in the destination. They are replace again every time the command runs.

## Issues
- Centos: if using Centos and getting a message like **'is mounted on / but it is not a shared mount'**, you may need to make ```multienv_host_mountpoint``` a shared mount point with ```mount --make-rshared <multienv_host_mountpoint>```. Replace ```<multienv_host_mountpoint>``` with the respective value
