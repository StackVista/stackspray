![Stackstate Logo](https://www.stackstate.com/wp-content/uploads/2019/05/StackState-Logo-Vector-RGB.svg)

Deploy a Stackstate Environemnt
============================================

Quick Start
-----------

To deploy Stackstate you can use :

### Ansible

#### Usage

    # Copy ``inventory/sample`` as ``inventory/myinstance``
    cp -rfp inventory/sample inventory/myinstance

    # Update Ansible inventory file with inventory builder
    # Use your editor of choice to update the inventory file
    vi inventory/myinstance/inventory.ini
    
    # When building a single node setup, only fill in node1, node2 can be left commented out.

    # Review and change parameters under ``inventory/myinstance/group_vars``
    cat inventory/myinstance/group_vars/all/all.yml

    # Deploy Stackstray with Ansible Playbook
    ansible-playbook -i inventory/myinstance/hosts.yml install-stackstate.yml

Note: By default this playbook will set your Python interpeter to python3 (/usr/bin/python3) inside the `inventory.ini` file. If you don't have python3 running, please update the `ansible_python_interpreter` variable.

Documents
---------

-   [Single vs multi node](#single-vs-multi-node)
-   [Variables](#variables)
-   [Stackstate docs](#stackstate-docs)
-   [Tested on](#tested-on)
-   [Todo](#todo)

Single vs multi node
------------

This playbook supports both single node as mutlti nodes setup. The `inventory.ini` contains two nodes, `node1` and `node2`. `node1` lives in the `stackstate` group, while `node2` lives in the `stackgraph` group.

`node1` is required. If no IP is set for that node, the playbook will fail. However, `node2` is optional. The following rules apply:

* If `node2` is empty, the playbook will create a single node cluster and install both Stackstate as well as Stackgraph on the supplied machine.
* If `node2` is set, the playbook will create a multi node cluster and install Stackstate and Stackgraph on two separate machines.

Variables
------------
* *tmp_location* - A temporary download location for Stackstate
* *stackstate_receiver_base* -  Public dns resolvable hostname external services can use to connect to the installed StackState instance
* *stackstate_setup* - Choose whether to install in production or development mode. Valid options are `PRODUCTION` or `DEVELOPEMT`
* *stackgraph_server* - The hostname of your StackGraph server
* *stackstate_license* - Your license key provided by Stackstate
* *use_artifactory* - If you want to use a custom artifactory instead of the public download page, set this to true
* *artifactory_url* - Your artifactory url, f.e. https://artifactory.mycompany.com/artifactory/release-candidates/com/company/company-distr
* *artifactory_user* -  Only required when use_artifactory is true
* *artifactory_password* - Only required when use_artifactory is true

Stackstate docs
------------
[Stackstate docs](https://docs.stackstate.com)

Tested on
------------
| OS            | Version       |
| ------------- |:-------------:|
| Ubuntu        | Bionic        |
| Ubuntu        | Xenial        |
| CentOS        | 8             |
| CentOS        | 7             |


Todo
------------

- Add upgrade path
- Improve support for package location (artifactory / downloads page)