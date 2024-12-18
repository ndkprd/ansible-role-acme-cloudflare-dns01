# ansible-role-acme-cloudflare-dns01

## Description

Ansible role to generate SSL certificate using DNS-01 challenge via Cloudflare.

## Usage

### Installation

```bash
ansible-galaxy install ndkprd.acme_cloudflare_dns01
```

### Hosts Example

```ini
[localhost]
localhost
```

### Playbook Example

```yaml
---

- name: Do DNS-01 challenge.
  hosts: localhost
  become: false
  gather_facts: false
  connection: local
  vars:
    acme_cert_country_id: "XX"
    acme_cert_org_name: "EXAMPLE INC."
    acme_domain: "example.com"
    acme_files_dir: "/tmp"
    acme_server_dir: "https://acme-staging-v02.api.letsencrypt.org/directory"
    acme_remaining_days: 30
    acme_account_email: "contact@example.com"
    acme_cf_api_email: "contact@example.com"
    acme_cf_api_token: "{{lookup('ansible.builtin.env', 'CF_API_TOKEN') }}"
    # if using alias/CNAME delegation
    acme_cf_use_alias: "{{ acme_cf_use_alias | default(false) }}"
    acme_cf_alias_zone: "{{ acme_cf_alias_zone | default(omit) }}"
    acme_cf_alias_record: "{{ acme_cf_alias_record | default(omit) }}"


  roles:
    - ndkprd.acme_cloudflare_dns01

```

The generated files will be located in `/tmp/{{ acme_domain }}` directory.

## TODO

- ~~add support for non-aliases/CNAME delegation;~~
- change inputs from string to list so it can be used for multiple domains at once.

## License

[MIT](./LICENSE)
