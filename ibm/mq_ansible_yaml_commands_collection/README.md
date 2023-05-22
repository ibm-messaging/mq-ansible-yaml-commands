# IBM MQ Ansible Collection to issue YAML form commands 
The **IBM MQ Ansible Collection to issue YAML form commands** can be used to **administer IBM MQ Queue Managers** and **provision and administer IBM MQ resources** on **distributed** or **z/OS** platforms.

Numerous sample **Ansible Playbooks** are provided as part of the collection which invoke the **runMQCmds** role to run **IBM MQ commands** that are specified in **YAML** form in an input cmds file. 

The runMQCmds role invokes the **IBM MQ Administrative POST REST API** to issue **JSON formatted MQSC commands** against MQ queue managers. Commands and their attributes are specified as key/value pairs in the input file. The key/value pairs are identical to those that can be specified when using the **IBM MQ Administrative POST REST API** to issue JSON format MQSC commands. The following diagram shows the flow:

<img src="./docs/mq_cmds_deployment_flow.png"  width="720" height="405">

To be able to represent IBM MQ commands in a YAML form provides for a **modern approach, with simple and existing documented IBM MQ attribute name/value pairs**. Users can define the cmds file in their favourite YAML editor, which may include user-defined templates or schemas to limit typographical errors and perform pre command execution validation.

As mentioned in the **Getting Started** section below, users will need to perform some customization to run the playbooks in their own environments.

## Playbook Summary
Ansible Playbooks are typically run on an **Ansible Control node**. Code in a playbook typically acts on resources in an **Ansible Managed node**. For example, Ansible may be installed on a laptop so that the laptop can act as an Ansible control node. Playbooks can then be run on the control node to manage resources on a managed node.

The use of playbooks provides for **automation**, and for the possibility to run playbooks as part of a **Continuous Integration and Continuous Delivery/Continuous Deployment (CI/CD) pipeline**.

The following sample MQ playbooks run on a control node to issue YAML form MQ commands against MQ queue managers on managed nodes. The playbooks demonstrate:

1. [**run-mq-crud-cmds.yml**](run-mq-crud-cmds.yml) - # the simplest form of invocation of the runMQCmds role to run various YAML form MQ CRUD commands specified in a default cmds input file.
2. [**run-mq-cfstruct-cmds.yml**](run-mq-cfstruct-cmds.yml) - # an invocation of the runMQCmds role with arguments, to run YAML form MQ CF structure commands specified in a cmds input file.
3. [**run-mq-comms-cmds.yml**](run-mq-comms-cmds.yml) - # multiple invocations of the runMQCmds role with varying arguments, to run YAML form MQ commands specified in multiple cmds input files at sending and receiving queue managers.
4. [**run-mq-chlauth-cmds.yml**](run-mq-chlauth-cmds.yml) - # an invocation of the runMQCmds to run YAML form MQ channel authentication rule commands specified in a cmds input file. As described in comments in this playbook, argument overrides are specified with the '-e' (extra variables) option when running the playbook.
5. [**run-mq-start-stop-cmds.yml**](run-mq-start-stop-cmds.yml) - # an invocation of the runMQCmds role with arguments, to run YAML form MQ start and stop commands specified in a cmds input file.
6. [**run-mq-clus-qmgr-cmds.yml**](run-mq-clus-qmgr-cmds.yml) - # an invocation of the runMQCmds role with arguments, to run YAML form MQ cluster queue manager commands specified in a cmds input file.
7. [**run-mq-display-qmgr-cmds.yml**](run-mq-display-qmgr-cmds.yml) - # an invocation of the runMQCmds role with argument overrides specified in a variable file (vars_file) included in the playbook, to run YAML form MQ cluster queue manager commands specified in a cmds input file.

The following sample cmds files are also provided, and are processed by the respective playbooks listed above:

1. [**mq_crud_yaml_form_cmds_file.yml**](mq_crud_yaml_form_cmds_file.yml) - # This is defined as the default file to use.
2. [**mq_cfstruct_yaml_form_cmds_file.yml**](mq_cfstruct_yaml_form_cmds_file.yml)
3. (a) [**mq_sending_end_yaml_form_cmds_file.yml**](mq_sending_end_yaml_form_cmds_file.yml) - # Processed by run-mq-comms-cmds.yml.

   (b) [**mq_receiving_end_yaml_form_cmds_file.yml**](mq_receiving_end_yaml_form_cmds_file.yml) - # Processed by run-mq-comms-cmds.yml.
4. [**mq_chlauth_yaml_form_cmds_file.yml**](mq_chlauth_yaml_form_cmds_file.yml)
5. [**mq_start_stop_yaml_form_cmds_file.yml**](mq_start_stop_yaml_form_cmds_file.yml)
6. [**mq_clus_qmgr_yaml_form_cmds_file.yml**](mq_clus_qmgr_yaml_form_cmds_file.yml)
7. [**mq_display_qmgr_yaml_form_cmds_file.yml**](mq_display_qmgr_yaml_form_cmds_file.yml)

## cmds file summary
A cmds file contains a YAML array named **mq_commands:**, of one or more MQ commands. Each command is defined as **- command:**.

Commands and their attributes are specified as key/value pairs that are identical to those that can be specified when using the IBM MQ administrative POST REST API to issue JSON format MQSC commands. Therefore, the YAML representation of **any** JSON formatted MQSC commands supported by the POST REST API can be specified in an input cmds file (see: https://www.ibm.com/docs/en/ibm-mq/9.3?topic=adminactionqmgrqmgrnamemqsc-post-json-formatted-command for a list of supported commands).

The sample cmds files listed above provide examples of the required YAML syntax.

## runMQCmds role Summary
The **runMQCmds** role reads YAML form IBM MQ commands and invokes the **IBM MQ Administrative POST REST API** to issue JSON formatted MQSC commands against MQ queue managers. 

The runMQCmds role can be invoked in Ansible playbooks in a simple form with default arguments, or it can be invoked with one or more **overriding arguments** supported by the runMQCmds role. Refer to the README.md file for the runMQCmds role for further information.

The sample playbooks listed above demonstrate some different ways in which arguments can be specified/passed. Ansible supports a hierarchy of twenty-two variable precedence rules (see: https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html#understanding-variable-precedence for further information) so, there are other ways of specifying arguments. Users will need to decide on what works best for their environment. 

## Inventory file
- [**inventories/localhost.ini**](inventories/localhost.ini) - local host file to specify the **Ansible connection** and **Python interpreter** settings, and to prevent **Ansible warning messages** about **implicit localhost** and/or **Python interpreter**.

  The default inventory file as defined in the supplied **ansible.cfg** file is used. Users can specify an alternate inventory file by changing the setting in **ansible.cfg** or by using the '-i' option when running a playbook. For example, to use the myhosts.yml file in the **inventories** folder in the **current user directory**, issue:

  ```
  ansible-playbook ./playbooks/run-mq-cluster-cmds.yml -i ~/inventories/myhosts.yml
  ```

  Ansible will automatically detect and use Python 3 on many platforms that ship with it. To explicitly configure a Python 3 interpreter, set the ansible_python_interpreter inventory variable at a **group** or **host** level (as shown in **localhost.ini** above) to the location of a Python 3 interpreter, such as /usr/bin/python3. The default interpreter path may also be set in file **ansible.cfg**, the **Ansible Configuration File**. As stated below, this collection includes an **ansible.cfg** file.

  For more information on Ansible Configuration Settings, refer to https://docs.ansible.com/ansible/latest/reference_appendices/config.html. Note the search order for configuration files.

  For more information on python configuration requirements on z/OS, see [Ansible FAQ: Running on z/OS](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html).

## ansible.cfg file
- An **ansible.cfg** file with the following contents is shipped with this collection:
```
[defaults]
# Python Interpreter
python_interpreter=/opt/homebrew/bin/python3.11
# Local host inventory file
inventory = ./inventories/localhost.ini
# Roles path
roles_path = ./roles
# Format any output produced in yaml form
stdout_callback=yaml
# Limit output when running an Ansible playbook by not displaying "Skipping [host]" messages
display_skipped_hosts=False
```
## Ansible Collection Requirements
- Standard **ansible.builtin.*** modules.

## Getting Started
- Ansible will need to be configured on a control node so that playbooks can be run on the control node to administer resources on a managed node (see: https://docs.ansible.com/ansible/latest/getting_started/index.html).
- If users are unfamiliar with Ansible playbooks, users may want to refer to the Ansible documentation about playbooks first (see: https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html).

- Users will need to perform some customization before running the playbooks: 

  1. For the MQ collections to function correctly in user environments, it is important that users store the files associated with the role using the collections (see: https://docs.ansible.com/ansible/latest/dev_guide/developing_collections_structure.html#collection-directories-and-files) and roles (see: https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html) directory structure associated with this collection.

  2. Set any overriding arguments, as required, on the invocation of role runMQCmds. The sample playbooks show numerous ways of setting overriding arguments.

  3. Update the REST API call invocation in [**roles/runMQCmds/tasks/prepare_and_run_mq_cmd.yml**] if a different type of authentication is to be used (see: **IBM MQ Console and REST API security** at https://www.ibm.com/docs/en/ibm-mq/9.3?topic=securing-mq-console-rest-api-security for further information).

  4. Update or create new cmds and playbook files to specify and run MQ commands specific for their MQ environments.

      Where applicable, remember to replace hostnames and port numbers with values that are specific to user environments.

## Running the playbooks
To run a sample playbook, issue the respective command (as stated below) from the directory in which this MQ collection has been installed

1. MQ CRUD commands sample playbook, issue:
    ```
    ansible-playbook ./playbooks/run-mq-crud-cmds.yml/clipboard-copy-element
    ```
2. MQ CF structure commands sample playbook, issue:
    ```
    ansible-playbook ./playbooks/run-mq-cfstruct-cmds.yml
    ```
3. MQ communications commands sample playbook to define MQ resources at sending and receiving queue managers, issue:
    ```
    ansible-playbook ./playbooks/run-mq-comms-cmds.yml
    ```
4. MQ channel authentication rules commands sample playbook, issue:
    ```
    ansible-playbook ./playbooks/run-mq-chlauth-cmds.yml -e @./playbooks/vars/overrides_for_chlauth_cmds.yml
    ```
    In this case, the overriding arguments are specified when running the playbook with the '-e' (extra-vars) option. It is important to prepend the YAML form overrides file with the '@' symbol as shown in the above example.

5. MQ start and stop commands sample playbook, issue:
    ```
    ansible-playbook ./playbooks/run-mq-startup-cmds.yml
    ```
6. MQ cluster queue manager commands sample playbook, issue:
    ```
    ansible-playbook ./playbooks/run-mq-cluster-cmds.yml
    ```
7. MQ display queue manager attributes sample playbook, issue:
    ```
    ansible-playbook ./playbook/run-mq-display-qmgr-cmds.yml
    ```
## Copyright
© Copyright IBM Corporation 2023.

## License
Licensed under [Apache License](https://opensource.org/licenses/Apache-2.0).

## Author Information
IBM UK Labs Ltd.