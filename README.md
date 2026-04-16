# Ludus - DFIR-IRIS

An Ansible role that installs [DFIR-IRIS](https://github.com/dfir-iris/iris-web) on a Debian/Ubuntu host for [Ludus](https://ludus.cloud).

DFIR-IRIS is a collaborative digital forensics and incident response platform.

## Requirements

- A Debian (bullseye/bookworm) or Ubuntu (focal/jammy) VM in your Ludus range
- Minimum 4 CPU cores and 8 GB RAM recommended

## Role Variables

| Variable | Default | Description |
|---|---|---|
| `ludus_dfir_iris_version` | `v2.4.20` | DFIR-IRIS release tag |
| `ludus_dfir_iris_https_port` | `443` | HTTPS port for the web interface |
| `ludus_dfir_iris_server_name` | `iris.local` | nginx `server_name` (FQDN or IP) |
| `ludus_dfir_iris_key_filename` | `iris_dev_key.pem` | TLS key filename inside the nginx image |
| `ludus_dfir_iris_cert_filename` | `iris_dev_cert.pem` | TLS cert filename inside the nginx image |
| `ludus_dfir_iris_admin_password` | `admin` | Admin password (set on first boot) |
| `ludus_dfir_iris_postgres_password` | `iris_db_password` | PostgreSQL database password |
| `ludus_dfir_iris_postgres_admin_password` | `iris_db_admin_password` | PostgreSQL admin password |
| `ludus_dfir_iris_secret_key` | `iris_secret_key_change_me` | Web app secret key |
| `ludus_dfir_iris_security_password_salt` | `iris_salt_change_me` | Password salt |
| `ludus_dfir_iris_install_path` | `/opt/iris-web` | Installation directory |
| `ludus_dfir_iris_auth_type` | `local` | Authentication type (`local`, `ldap`, or `oidc`) |

## Ludus Setup

```
ludus ansible roles add -d ./ludus_dfir_iris
```

### Range Config Example

```yaml
ludus:
  - vm_name: "{{ range_id }}-DFIR-IRIS"
    hostname: "{{ range_id }}-iris"
    template: debian-12-x64-server-template
    vlan: 10
    ip_last_octet: 50
    ram_gb: 8
    cpus: 4
    linux: true
    roles:
      - ludus_dfir_iris
    role_vars:
      ludus_dfir_iris_admin_password: "MySecurePassword123!"
```

### Deploy

```
ludus range deploy -t user-defined-roles
```

## Access

After deployment, access DFIR-IRIS at:

```
https://<VM_IP>:443
```

Default credentials: `admin` / value of `ludus_dfir_iris_admin_password` (default: `admin`)

## License

MIT

## Author

Whispergate
