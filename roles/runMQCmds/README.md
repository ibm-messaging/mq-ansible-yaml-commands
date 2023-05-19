runMQCmds
=========
The **runMQCmds** role reads **YAML** form IBM MQ commands in an input cmds file and invokes the **IBM MQ Administrative POST REST API** to issue JSON formatted MQSC commands against MQ queue managers.

The input cmds file contains a YAML array named **mq_commands:**, of one or more MQ commands. Each command is defined as **- command:**.

Commands and their attributes are specified as key/value pairs that are identical to those that can be specified when using the IBM MQ administrative POST REST API to issue JSON format MQSC commands. Therefore, the YAML representation of **any** JSON formatted MQSC commands supported by the POST REST API can be specified in an input cmds file (see: https://www.ibm.com/docs/en/ibm-mq/9.3?topic=adminactionqmgrqmgrnamemqsc-post-json-formatted-command for a list of supported commands).

Since the source code for the runMQCmds role has been made available, users can extend the runMQCmds role to support additional arguments if required. And, any such extensions can be contributed back into the collection so that other users can benefit from making use of them.

Requirements
------------
Users will need to configure and run a **mqweb server** (see https://www.ibm.com/docs/en/ibm-mq/9.3?topic=administering-administration-using-rest-api for further information about this).

Role Variables
--------------
The runMQCmds role can be invoked in Ansible playbooks in a simple form with default arguments, or it can be invoked with one or more of the following **overriding arguments**:

- **cmds_file_name**: The full directory path for the file that contains the MQ commands to be run. This is a string value.
- **webserver_addr**: The mqweb server address. This is a string value.
- **webserver_port**: The mqweb server port. This is a string value.
- **api_userid**: The user ID to use when issuing the MQ Administrative POST REST API. This is a string value. Refer to the MQ docs for further information about using user IDs with the MQ Administrative REST API.
- **api_password**: The password to use when issuing the MQ Administrative POST REST API. This is a string value. Refer to the MQ docs for further information about using passwords with the MQ Administrative REST API.
- **qmgr_name**: The name of the queue manager on which commands are to be run. This is a string value.
- **stop_on_error**: The option to indicate if processing of subsequent MQ commands should stop if the current MQ command fails with certain errors. This is a boolean value.

**Overriding arguments** allow users to vary values on a per role invocation basis and hence run commands against multiple MQ queue managers

The runMQCmds role also makes use of a local variable called **overallCompCode** to store the overall completion code resulting from the execution of the MQ commands in the input cmds file. 

Files associated with the runMQCmds role
----------------------------------------
The following files are assocociated with the **runMQCmds** role:

- [**roles/runMQCmds/README.md**](roles/runMQCmds/README.md) - this README file.
- [**roles/runMQCmds/vars/main.yml**](roles/runMQCmds/vars/main.yml) - local variables.
- [**roles/runMQCmds/defaults/main.yml**](roles/runMQCmds/defaults/main.yml) - default argument values.
- [**roles/runMQCmds/meta/argument_specs.yml**](roles/runMQCmds/meta/argument_specs.yml) - argument specification.
- [**roles/runMQCmds/tasks/main.yml**](roles/runMQCmds/tasks/main.yml) - main tasks list.
- [**roles/runMQCmds/tasks/prepare_and_run_mq_cmd.yml**](roles/runMQCmds/tasks/prepare_and_run_mq_cmd.yml) - include tasks list that prepares and runs MQ commands.

  In some cases, the REST API implementation on IBM MQ on distributed platforms displays more information than the REST API implementation on IBM MQ for z/OS. Refer to the section on **POST - JSON formatted command** in the **IBM MQ Docs** for specific information about this (see: https://www.ibm.com/docs/en/ibm-mq/9.3?topic=adminactionqmgrqmgrnamemqsc-post-json-formatted-command).

  As the MQ command is run by issuing the **IBM MQ administrative POST REST API**, the **ansible.builtin.uri** module is used to issue the REST API call. A **user ID** and **password** are set on the call for basic authentication.

  This file also contains **debug statements** that are executed when playbooks are run with the **Ansible -vv** or **-vvv** options to provide extra information.

Dependencies
------------
A mqweb server needs to be configured and running so that the runMQCmds role can issue MQ REST API calls. 

Example cmds input file
-----------------------
```
---
# (c) Copyright IBM Corporation 2023
# Apache License, Version 2.0 (see https://opensource.org/licenses/Apache-2.0)

# Playbook file: cmds_files/mq_sample_stop_start_cmds_file.yml

# Sample file that contains YAML form MQ start and stop commands to be run by the runMQCmds role.
#
# Commands and their attributes are specified as key/value pairs. The key/value pairs are 
# identical to those that can be specified when using the MQ administrative POST REST API 
# to issue JSON format MQSC commands. Any key/value pairs not specified will be set to
# default values as defined for the queue manager on which commands are run.

# Array of MQ commands.
mq_commands:
#
# Commands to start and stop a MQ TCP listener and channel.
#
# Start a TCP listener.
  - command: start
    parameters:
      port: 1407
    qualifier: listener
# Start a channel.
  - command: start
    name: MQ07.TO.MQ08
    qualifier: channel
# Stop a TCP listener.
  - command: stop
    qualifier: listener
# Stop a channel.
  - command: stop
    name: MQ07.TO.MQ08
    parameters:
      mode: quiesce
      status: stopped
    qualifier: channel
...
```
Example Playbook
----------------
```
---
# (c) Copyright IBM Corporation 2023
# Apache License, Version 2.0 (see https://opensource.org/licenses/Apache-2.0)

# Playbook file: playbooks/run-yaml-mq-start-stop-qmgr-cmds.yml

# This sample playbook demonstrates how to invoke the runMQCmds role with arguments, to run MQ for z/OS CF structure commands.

- name: Sample playbook to run YAML form MQ for z/OS CF structure commands specified in a cmds file

  hosts: localhost
  gather_facts: yes
  roles:
  - { role: runMQCmds,
        cmds_file_name: '~/mq/cmds_files/mq_sample_stop_start_cmds_file.yml',
        qmgr_name: MQ08
    }
...
```
License
-------
Licensed under [Apache License](https://opensource.org/licenses/Apache-2.0).

Copyright
---------
© Copyright IBM Corporation 2023

Author Information
------------------
IBM UK Labs Ltd.
