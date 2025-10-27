# ansible-webserver-setup

Goal
Set up a Linux server (Ubuntu recommended) with a static website, Nginx, SSH key access, and basic hardening using a modular Ansible playbook and roles. Demonstrates infrastructure automation and reusable configuration.

Key roles
- base: system updates, utilities, UFW, Fail2Ban
- nginx: install and configure Nginx
- app: deploy static website from tarball
- ssh: add public SSH key for passwordless access

Prerequisites
- Local machine with Ansible installed
- Target Linux server reachable over SSH
- Private SSH key locally (e.g., ~/.ssh/id_rsa)
- Public SSH key for the ssh role (roles/ssh/files/id_rsa.pub)
- Website tarball at roles/app/files/website.tar.gz

Directory (example)
ansible-webserver-setup/
├── inventory.ini
├── setup.yml
└── roles/
    ├── base/
    ├── nginx/
    ├── app/
    └── ssh/

Common commands
- Run full playbook:
  ansible-playbook -i inventory.ini setup.yml

- Run a specific role (by tag):
  ansible-playbook -i inventory.ini setup.yml --tags "app"

- Dry-run (simulate changes):
  ansible-playbook -i inventory.ini setup.yml --check --diff

- Check Nginx on remote host:
  ssh -i ~/.ssh/id_rsa ubuntu@YOUR_SERVER_IP 'systemctl status nginx'

- Copy website tarball to control repo (if needed):
  scp website.tar.gz user@controlmachine:roles/app/files/

- Add local public key to server (alternative to role):
  ssh-copy-id -i ~/.ssh/id_rsa.pub ubuntu@YOUR_SERVER_IP

- Inspect UFW rules remotely:
  ssh -i ~/.ssh/id_rsa ubuntu@YOUR_SERVER_IP 'sudo ufw status'

Usage summary
1. Edit inventory.ini with target IP and ansible_user/ansible_ssh_private_key_file.
2. Place website.tar.gz into roles/app/files/ and public key into roles/ssh/files/ (or use ssh-copy-id).
3. Run the playbook (without --check) for first deployment:
   ansible-playbook -i inventory.ini setup.yml
4. Re-deploy only the website after updates:
   ansible-playbook -i inventory.ini setup.yml --tags "app"

Notes
- Use --check for safe testing; run without it for real changes.
- Roles are independent and extendable (e.g., add SSL, DB roles).
- Ensure inventory SSH user has sudo privileges.

License
MIT

https://roadmap.sh/projects/configuration-management
