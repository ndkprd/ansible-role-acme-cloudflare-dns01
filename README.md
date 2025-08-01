# ansible-role-acme-cf

## Description

Ansible role to generate wildcard SSL certificates using DNS-01 challenge via [Cloudflare](https://cloudflare.com).

## Usage

### Installation

```bash
ansible-galaxy install ndkprd.acme_cf
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
    acme_cf_certcountry_id: "XX"
    acme_cf_certorg_name: "Example Inc."
    acme_cf_domains:
      # without CNAME delegation
      - domain: "example.com"
      # if using CNAME delegation
      - domain: "example.com"
        alias_enabled: true
        alias_zone: "example.xyz"
        alias_record: "_acme-challenge.acme"
    acme_cf_server_dir: "https://acme-staging-v02.api.letsencrypt.org/directory"
    acme_cf_remaining_days: 30
    acme_cf_account_email: "contact@example.com"
    acme_cf_api_token: "{{lookup('ansible.builtin.env', 'CF_API_TOKEN') }}"

  roles:
    - ndkprd.acme_cf

```

The generated files will be located in the dir you set in `acme_cf_files_dir` var, with the default being `/tmp/acme_cf`.

## Notes

There's some tasks that is tagged with `debug` tag that is basically just print out variable vars for debugging purpose. You can skip those using `--skip-tags=debug` flag.

## TODO

- [x] add support for non-aliases/CNAME delegation;
- [x] change input from string to list so it can be used for multiple domains at once;
- [ ] Add checksums.

## License

[MIT](./LICENSE)
