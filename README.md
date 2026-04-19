# satellite_cleanup

Ansible task file that removes stale RHEL hosts from Satellite that no longer exist in vCenter.

---

## What it does

1. Pulls all hosts from Satellite, filtered to RHEL systems whose names start with `gva`, `ext`, `org`, or `gvt`.
2. Pulls all VMs from vCenter filtered the same way.
3. Diffs the two lists by short hostname.
4. Deletes from Satellite any host that has no matching VM in vCenter (powered on **or** off).

A hard stop prevents execution if more than 20 hosts would be deleted in a single run.

---

## Requirements

- `redhat.satellite` collection
- `community.vmware` collection

The tasks delegate to `localhost`, so the AWX execution node needs network access to both Satellite and vCenter.

---

## Variables

### vars/main.yml

| Variable | Description | Default |
|---|---|---|
| `sat_web_protocol` | Protocol used to reach Satellite (`http`/`https`) | `https` |
| `sat_srv_fqdn` | Satellite server FQDN |
| `sat_organization` | Satellite organization for filtering |
| `validate_certs` | Validate TLS certificates for Satellite API calls | `false` |
| `vcenter_validate_certs` | Validate TLS certificates for vCenter API calls | `true` |

### AWX credentials

| Credential | Variables injected |
|---|---|
| Satellite | `sat_username`, `sat_password` |
| vCenter | `vcenter_hostname`, `vcenter_username`, `vcenter_password` |

### AWX group_vars

| Variable | Description |
|---|---|
| `debugmode` | Enable verbose debug output |
