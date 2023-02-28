# pygetfacl

Retrieves Access Control List (ACL) information provided by the Unix / Linux [`getfacl`](https://manpages.ubuntu.com/manpages/trusty/man1/getfacl.1.html) command and store that information as a Python object.

## System Requirements

* Unix or a Linux distribution with the `acl` package installed.
* Python version 3.7 or higher

## Installation
From the command line:
```shell
pip install git+https://github.com/duanegoodner/pygetfacl
```

## Basic Usage

```python
from pygetfacl import ACLInfoRetriever

info_retriever = ACLInfoRetriever("/some/path/")
getfacl_result = info_retriever.getfacl()
my_acl_data = getfacl_result.acl_data
```

## Try *aclpath* in a Docker container
If you want to try aclpath without installing it in a local environment and/or you're not on a Linux system, you can test it out in a Docker container. From the aclpath project root, run the following shell commands to build a Docker image, start a container, and `docker exec` into the container.
```shell
$ docker build ./demo_docker -t pygetfacl_demo
$ docker run -it -d --rm --name="pygetfacl_demo" pygetfacl_demo
$ docker exec -it -w /home/user_a aclpath_demo /bin/bash
```

Once you're in the container, start an interactive Pyton console.
```shell
user_a@container-id:~$ python
```

Then try the following Python commands:
```pycon
>>> from pygetfacl import ACLInfoRetriever
>>> info_retriever = ACLInfoRetriever("/home/user_a/test_dir")
>>> getfacl_result = info_retriever.getfacl()
>>> my_acl_data = getfacl_result.acl_data
>>> import pprint
>>> pprint.pprint(my_acl_data)
ACLData(owning_user='user_a',
        owning_group='group_x',
        flags=None,
        user_permissions='rwx',
        group_permissions='r--',
        other_permissions='r--',
        mask='rwx',
        special_users_permissions={'user_b': 'rwx'},
        special_groups_permissions={'group_x': 'rwx'},
        default_user_permissions='rwx',
        default_group_permissions='r--',
        default_other_permissions='---')
```
The console output shows the attributes and values of the ACLData object that stores information obtained by calling the getfacl() method.

