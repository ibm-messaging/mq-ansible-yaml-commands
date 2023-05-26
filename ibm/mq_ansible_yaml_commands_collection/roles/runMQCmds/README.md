# runMQCmds role
The **runMQCmds** role reads **YAML** form IBM MQ commands specified in a cmds input file and invokes the **IBM MQ Administrative POST REST API** to issue JSON formatted MQSC commands against **MQ distributed or z/OS queue managers**.

The input cmds file contains a YAML array named **mq_commands:**, of one or more MQ commands. Each command is defined as **- command:**. This can be seen in the sample cmds files supplied with the **mq_ansible_yaml_commands_collection**.

Commands and their attributes are specified as key/value pairs that are identical to those that can be specified when using the IBM MQ Administrative POST REST API to issue JSON format MQSC commands. Therefore, the YAML representation of **any** JSON formatted MQSC commands supported by the POST REST API can be specified in an input cmds file see: [**POST - JSON formatted command**](https://www.ibm.com/docs/en/ibm-mq/9.3?topic=adminactionqmgrqmgrnamemqsc-post-json-formatted-command) for a list of supported commands.

Since the source code for the runMQCmds role has been made available, users can extend the runMQCmds role to support additional arguments if required.

## Requirements
Users will need to configure and run a **mqweb server** so that the runMQCmds role can issue MQ REST API calls: see: [**Administration using the REST API**](https://www.ibm.com/docs/en/ibm-mq/9.3?topic=administering-administration-using-rest-api).

## Role Variables
The runMQCmds role can be invoked in Ansible playbooks in a simple form with default arguments, or it can be invoked with one or more of the following **argument overrides**:

- **cmds_file_name**: The full directory path for the file that contains the MQ commands to be run. This is a string value.
- **webserver_addr**: The mqweb server address. This is a string value.
- **webserver_port**: The mqweb server port. This is a string value.
- **api_userid**: The user ID to use when issuing the MQ Administrative POST REST API. This is a string value. Refer to the MQ docs for information about using user IDs with the MQ Administrative REST API.
- **api_password**: The password to use when issuing the MQ Administrative POST REST API. This is a string value. Refer to the MQ docs for information about using passwords with the MQ Administrative REST API.
- **qmgr_name**: The name of the queue manager on which commands are to be run. This is a string value.
- **stop_on_error**: The option to indicate if processing of subsequent MQ commands should stop if the current MQ command fails with certain errors. This is a boolean value.

**Argument Overrides** allow users to vary values on a per role invocation basis and hence run commands against multiple MQ queue managers

The runMQCmds role also makes use of a local variable called **overallCompCode** to store the overall completion code resulting from the execution of the list of MQ commands in the input cmds file. 

## Files associated with the runMQCmds role
The following files are assocociated with the **runMQCmds** role:

- [**README.md**](README.md) - this README file.
- [**vars/main.yml**](vars/main.yml) - local variables.
- [**defaults/main.yml**](defaults/main.yml) - default argument values.
- [**meta/argument_specs.yml**](meta/argument_specs.yml) - argument specification.
- [**tasks/main.yml**](tasks/main.yml) - main tasks list.
- [**tasks/prepare_and_run_mq_cmd.yml**](tasks/prepare_and_run_mq_cmd.yml) - include tasks list that prepares and runs MQ commands.

  In some cases, the REST API implementation on IBM MQ on distributed platforms displays more information than the REST API implementation on IBM MQ for z/OS: see [**POST - JSON formatted command**](https://www.ibm.com/docs/en/ibm-mq/9.3?topic=adminactionqmgrqmgrnamemqsc-post-json-formatted-command).

  As the MQ command is run by issuing the **IBM MQ administrative POST REST API**, the [**ansible.builtin.uri**](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/uri_module.html) module is used to issue the REST API call. A **user ID** and **password** are set on the call for basic authentication.

  This file also contains **debug statements**, that are executed when playbooks are run with the **Ansible -vv** or **-vvv** options, to provide extra information.

## Dependencies
See **Requirements** above. 

## Example cmds input file: mq_start_stop_yaml_form_cmds_file.yml
The purpose of the following file is to show how YAML form MQ start and stop commands could be issued.
```
---
# (c) Copyright IBM Corporation 2023
# Apache License, Version 2.0 (see https://opensource.org/licenses/Apache-2.0)

# File: cmds_files/mq_start_stop_yaml_form_cmds_file.yml

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

## Example Playbook: run_mq_start_stop_cmds.yml
```
---
# (c) Copyright IBM Corporation 2023
# Apache License, Version 2.0 (see https://opensource.org/licenses/Apache-2.0)

# Playbook file: playbooks/run_mq_start_stop_cmds.yml

# This sample playbook demonstrates how to invoke the runMQCmds role with arguments, to run
# MQ start and stop commands.

- name: Sample playbook to run YAML form MQ for z/OS CF structure commands specified in a cmds file

  hosts: localhost
  gather_facts: yes
  roles:
  - { role: runMQCmds,
        cmds_file_name: '../cmds_files/mq_start_stop_yaml_form_cmds_file.yml',
        stop_on_error: 'false'
    }
...
```

## Running the example playbook
To run the example playbook, which is available with the **mq_ansible_yaml_commands_collection**, issue the following command:
```
ansible-playbook playbooks/run_mq_start_stop_cmds.yml
```
## License
Licensed under [**Apache License**](https://opensource.org/licenses/Apache-2.0).

## Copyright
© Copyright IBM Corporation 2023

## Author Information
IBM UK Labs Ltd.