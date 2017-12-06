# fortifly

Fortifly: Basic FreeBSD/Ubuntu hardening for cloud servers (ansible playbooks).

## Performs tasks:

- Bootstrap host
  - FreeBSD:
    - Bootstrap `pkg`
    - Install `sudo`    
    - Link installed python to `/usr/bin/python`, allowing easier quick-run.
  - Ubuntu:
    - No os-specific tasks.
  - Common:
    - Install `python 2.7`
    - Install `mosh`
    - Create user: `group_vars/all -> ssh_user`
    - Add public key: `group_vars/all -> ssh_public_key`
    - Allow wheel and user to sudo (NOPASSWD)
    - Update sshd port: `group_vars/all -> ssh_port`.
    - Disable root login on ssh.

## Install:

- `$ git clone https://github.com/hiway/fortifly.git`
- `$ cd fortifly`
- `$ pip install pipenv` (if you do not already have `pipenv`)
- `$ pipenv shell` 

## Quick Run:

Harden a host without changing the `hosts` or `group_vars/all` files. Run inside pipenv shell: 

> WARNING: Run only on newly-created instances. 

```bash
$ ansible-playbook site.yml \
  -i "10.0.0.1," \
  --ask-pass  \
  --tags linux \ 
  --extra-vars="ssh_port=222 ssh_user=$USER"
```

## Expects:

- Python 3.6 on your machine (if using pipenv, else run `pip install ansible` in your virtualenv)
- `~/.ssh/id_rsa.pub` exists, or edit `group_vars/all` for correct path.

## Defaults:

- `ansible` as ssh user, change in `group_vars/all`.
- `22` as ssh port, change in `group_vars/all`.


## Customize:
> Replace `$EDITOR` with your editor of choice.

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
- https://docs.ansible.com/ansible/latest/playbooks_debugger.html
- https://docs.ansible.com/ansible/latest/playbooks_conditionals.html 
- http://www.caphrim.net/ansible/2015/05/24/understanding-ansible-tags.html
- https://stackoverflow.com/questions/33222641/override-hosts-variable-of-ansible-playbook-from-the-command-line
- https://stackoverflow.com/a/18199029/996800 (Arbitrary hosts from command line)
