# Testing a stackable multienvironment directory layout for Ansible
[![Buy me a coffee](https://img.shields.io/badge/$-BuyMeACoffee-blue.svg)](https://www.buymeacoffee.com/jobcespedes)
## Quickstart:
1. Check that [Docker](https://docs.docker.com/install/) and [Docker SDK for Python](https://docker-py.readthedocs.io/en/stable/) are installed in your system.
2. Download this repo: ```git clone --recursive https://github.com/jobcespedes/demo-multienv && cd demo-multienv```
3. Stack the environment (production over base): ```ansible-playbook -i localhost, multienv.yml```
4. Test running a _foo_ playbook: ```ansible-playbook foo.yml```
5. Modify files inside ```environments/<environment>```
6. Test repeating step #4
> Refer to [multienv Ansible role](https://github.com/jobcespedes/multienv/blob/master/README.md) for how to configure the stackable environment

> [Read the article about it](https://jobcespedes.dev/2020/02/multiple-environments-in-ansible/)
