# strongswan-ansible-deploy
A playbook, config templates and vars to deploy a Strongswan IKEv2 VPN server

## Prerequisites
+ A target server or several, configured to accept SSH connections as user with privilege escalation rights
+ Python 3.x installed on both localhost and remotes
+ Ansible (core >= 2.9) installed on localhost

## Usage
1. Install Ansible if absent with:
`python3 -m pip install ansible --user`
2. Define your hosts, Ansible remote connection user and `sudo` password in `inventory/hosts.yml`.
The `domain_name` variable is required for certificate generation.
The `generate_certs: true` variable enables certificate generation. Omit only if you plan to generate and deliver certificates yourself. [Ansible Vault](https://docs.ansible.com/ansible/latest/vault_guide/vault_encrypting_content.html) usage for storing passwords and other secrets is strongly recommended.
3. Define Strongswan users' credentials in `vars/ipsec-users.yml`. As before, you can either set passwords in plaintext, use Ansible Vault or, as example file states, use Hashicorp Vault engine.
4. Change your root CA certificate's common name in `vars/pki-config.yml` - `ipsec.cacerts.ca_cert.common_name`
5. Optional - review and change if necessary config file templates in `templates/`

### Run
`ansible-playbook --vault-password-file <path-to-password-file> -i inventory/hosts.yml playbook.yml --private-key <path-to-ansible-user-private-key>`

Example is using Ansible Vault, you can omit that if you decide to store your super sensitive data in plaintext (ew).

## Aftermath
If everything went well, you should end up with no red lines in your terminal, and a generated root CA certificate file(s) should appear in the directory from which you ran the playbook. Install this certificate in your system and set up IKEv2 connection using system-specific methods.
