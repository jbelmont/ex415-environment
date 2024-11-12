## Exam Objectives

#### Use Red Hat Ansible® Engine

* Install Red Hat Ansible Engine on a control node.
* Configure managed nodes.
* Configure simple inventories.
* Perform basic management of systems.
* Run a provided playbook against specified nodes.

#### Configure Intrusion Detection

* Install AIDE.
* Configure AIDE to monitor critical system files.

#### Configure Encrypted Storage

* Encrypt and decrypt block devices using LUKS.
* Configure encrypted storage persistence using NBDE.
* Change encrypted storage passphrases.

#### Restrict USB Devices

* Install USBGuard.
* Write device policy rules with specific criteria to manage devices.
* Manage administrative policy and daemon configuration.

#### Manage System Login Security Using PAM (Pluggable Authentication Modules)

* Configure password quality requirements.
* Configure failed login policy.
* Modify PAM configuration files and parameters.

#### Configure System Auditing

* Write rules to log auditable events.
* Enable prepackaged rules.
* Produce audit reports.

#### Configure SELinux

* Enable SELinux on a host running a simple application.
* Interpret SELinux violations and determine remedial action.
* Restrict user activity with SELinux user mappings.
* Analyze and correct existing SELinux configurations.

#### Enforce Security Compliance

* Install OpenSCAP and OpenSCAP Workbench.
* Use OpenSCAP and Red Hat Insights to scan hosts for security compliance.
* Use OpenSCAP Workbench to tailor policy.
* Use OpenSCAP Workbench to scan an individual host for security compliance.
* Use Red Hat Satellite server to implement an OpenSCAP policy.
* Apply OpenSCAP remediation scripts to hosts.

#### How to install Virtualization

###### macOS

1. Install homebrew
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2. Install VirtualBox
```
brew install --cask virtualbox
```

3. Install Vagrant
```
brew install --cask vagrant
```

4. Check out the repo
```
git clone https://github.com/jbelmont/ex415-environment.git
```

5. Navigate to the repo
```
cd ex415-environment
```

6. Provision
```
vagrant up --provision
``` 

## Commonly used commands
### Accessing the machines
In order to access control-node execute
```
vagrant ssh controller
```

### Stopping the machines
```
vagrant halt
```

### Starting the machines back again
```
vagrant up
```

5. Install ansible
```
sudo yum install -y ansible
```
