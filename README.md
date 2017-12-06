# fortifly

Fortifly: Basic FreeBSD/Ubuntu hardening for cloud servers (ansible playbooks).

## Expects:

- Python 3.6 on your machine (if using pipenv, else run `pip install ansible` in your virtualenv)
- `~/.ssh/id_rsa.pub` exists, or edit `group_vars/all` for correct path.

## Defaults:

- `ansible` as ssh user, change in `group_vars/all`.
- `22` as ssh port, change in `group_vars/all`.

## Performs tasks:

- Bootstrap host
  - FreeBSD:
    - Bootstrap `pkg`
    - Install `sudo`    
  - Ubuntu:
    - No os-specific tasks.
  - Common:
    - Install `python 2.7`
    - Create user: `group_vars/all -> ssh_user`
    - Add public key: `group_vars/all -> ssh_public_key`
    - Allow wheel and user to sudo (NOPASSWD)
    - Install `mosh`
    - Disable root login on ssh.
    - Update sshd port: `group_vars/all -> ssh_port`.

## Usage:

> Replace `$EDITOR` with your editor of choice.

- `$ git clone https://github.com/hiway/fortifly.git`
- `$ cd fortifly`
- `$ $EDITOR group_vars/all`
  - Change sshd port if preferred.
  - Change ssh user, default is `ansible`.
  - Change ssh public key path if required.
- `$ cp hosts.example hosts`
- `$ $EDITOR hosts`
  - Copy the example line for FreeBSD or Linux
  - Uncomment and change the hostname or IP address
  - Change `ansible_user` if required.
- `$ pipenv run 'ansible-playbook -i hosts --ask-pass site.yml --tags=linux'`
  - If you changed `ansible_user`, add `-k` to the `ansible-playbook` command to use `sudo`.
  - Use appropriate tags, `linux` or `freebsd` to specify the OS of host.

## Tested on:

- FreeBSD 11 (infra: Vultr.com)
- Ubuntu Linux 17.10 (infra: Vultr.com) 


## References:

- https://serversforhackers.com/c/an-ansible-tutorial
- http://abregman.com/2015/12/25/ansible-write-and-run-your-first-playbook/
- https://docs.ansible.com/ansible/latest/playbooks_reuse.html
- https://github.com/ansible/ansible-examples/
- https://everythingshouldbevirtual.com/automation/ansible-clean-formatted-playbooks/
- https://gist.github.com/marktheunissen/2979474 (Well documented example playbook)
- https://gist.github.com/tomster/7585211 (FreeBSD bootstrap, check comment.)
- https://stackoverflow.com/questions/37333305/ansible-create-a-user-with-sudo-privileges 