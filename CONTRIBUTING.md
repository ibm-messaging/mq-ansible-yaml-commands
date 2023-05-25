# Contributing to the mq-ansible-yaml-commands repository

Thank you for your interest in wanting to contribute to the open-source **mq-ansible-yaml-commands** repository in the **ibm-messaging** project.

To ensure that the codebase is always healthy and does not result in deployment issues when forked and used, it is important that you pre-check your additions and updates for any potential code conflicts before uploading your changes to the GitHub Repository. 

Perform the following steps to make and submit your contributions: 

1. Fork the mq-ansible-yaml-commands repository.
2. Create and test your changes.
3. Commit/Push changes to your fork.
4. Merge changes.
5. Create a Pull Request.

## 1. Fork the mq-ansible-yaml-commands repository

To fork the repository:
- Get started by clicking on "Fork" from the top-right corner of the main repository page.
- Choose a name and description for your fork.
- Select the option "Copy the main branch only", as in most cases, you will only need the default branch to be copied.
- Click on "Create fork".

Once you have forked the repository, you can clone your fork so that it is local to your computer. In order to do that:

- Click on "Code" (the green button on your forked repository).
- Copy the forked repository URL under HTTPS.
- Type the following on your terminal:

```
git clone <the_forked_repository_url> 
```

You can set up Git to pull updates from the mq-ansible-yaml-commands repository into the local clone of your fork when you fork a project in order to propose changes to the mq-ansible-yaml-commands repository. In order to do that, run the following command:

```
git remote add upstream https://github.com/ibm-messaging/mq-ansible-yaml-commands
```

To verify the new upstream repository you have specified for your fork, run the following command:

```
git remote -v
```

You should see the URL for your fork as **origin**, and the URL for the mq-ansible-yaml-commands repository as **upstream**.

## 2. Create and test your changes**

Now, you can work locally to add and test your changes. Ensure to include any necessary documentation. 

Ensure that they do not break any existing code in the mq-ansible-yaml_commands repository.

## 3. Commit/Push changes to your fork 

If you are looking to add all the files you have modified in a particular directory, you can stage them all with the following command:

```
git add . 
```

If you are looking to recursively add all changes including those in subdirectories, you can type: 

```
git add -A 
```

Alternatively, for all new files to be staged, you can type:

```
git add -all
```

Once you are ready to submit your changes, ensure that you commit them to your fork with a message. The commit message is an important aspect of your code contribution; it helps the maintainers of mq-ansible-yaml_commands repository and other contributors to understand the change you have made, why you made it, and how significant it is. 

You can commit your changes by running: 

```
git commit -m "Brief description of your changes/additions"
```

To push all your changes to the forked repo:

```
git push
```

## 4. Merge changes

Merge any changes that were made in the original repository’s main branch:

```
git merge upstream/main
```

## 5. Create a Pull Request

Before creating a Pull Request (PR), ensure you have read the [IBM Contributor License Agreement](CLA.md). By creating a PR, you certify that your contribution:

1. Is licensed under Apache Licence Version 2.0.
2. Does not result in IBM MQ proprietary code being statically or dynamically linked to Ansible runtime.

Once you have carefully read and agree to the terms mentioned in the [IBM Contributor License Agreement](CLA.md), you are ready to make a pull request to the original repository.

Navigate to your forked repository and press the _**New pull request**_ button. Add a title and a comment in the appropriate fields, and press the _**Create pull request**_ button.

The maintainers of the original repository will review your contribution and decide whether or not to accept your pull request.