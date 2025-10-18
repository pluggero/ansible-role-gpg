# Ansible Role: GNU Privacy Guard

[![CI](https://github.com/pluggero/ansible-role-gpg/actions/workflows/ci.yml/badge.svg)](https://github.com/pluggero/ansible-role-gpg/actions/workflows/ci.yml) [![Ansible Galaxy downloads](https://img.shields.io/ansible/role/d/pluggero/gpg?label=Galaxy%20downloads&logo=ansible&color=%23096598)](https://galaxy.ansible.com/ui/standalone/roles/pluggero/gpg)

An Ansible Role that installs and configures GNU Privacy Guard with secure defaults.

## Security Posture

This role configures GPG with modern cryptographic best practices:

- **Strong algorithms**: AES256, SHA512 preferred; SHA1 marked as weak
- **Secure keyserver**: HKPS (encrypted) connection to keys.openpgp.org
- **Passphrase hardening**: Maximum S2K iteration count (65M+) with AES256/SHA512
- **Trust model**: TOFU+PGP for enhanced authenticity verification
- **Privacy**: Version and comments stripped from output

## Requirements

None.

## Role Variables

```yaml
gpg_install_method: "package"
```

The method used to install gpg can be defined in the variable `gpg_install_method`.
The following methods are available:

- `package`: Installs gpg from the package manager of the distribution
  - **NOTE**: This method installs the latest version available in the package manager and not the version defined in `gpg_version`.

### Paths

| Variable          | Default             | Description                 |
| ----------------- | ------------------- | --------------------------- |
| `gpg_config_dir`  | `~/.gnupg`          | GPG configuration directory |
| `gpg_config_file` | `~/.gnupg/gpg.conf` | GPG configuration file path |

### Keyserver & Key Discovery

| Variable                | Default                    | Description                             |
| ----------------------- | -------------------------- | --------------------------------------- |
| `gpg_keyserver`         | `hkps://keys.openpgp.org`  | Keyserver URL (use HKPS for encryption) |
| `gpg_keyserver_options` | `[no-honor-keyserver-url]` | Keyserver security options              |
| `gpg_auto_key_retrieve` | `false`                    | Automatically retrieve missing keys     |
| `gpg_auto_key_locate`   | `[local, wkd, keyserver]`  | Key discovery methods in priority order |

### Cryptographic Algorithms

| Variable                          | Default                | Description                         |
| --------------------------------- | ---------------------- | ----------------------------------- |
| `gpg_personal_cipher_preferences` | `AES256 AES192 AES`    | Preferred ciphers (strongest first) |
| `gpg_personal_digest_preferences` | `SHA512 SHA384 SHA256` | Preferred digests (strongest first) |
| `gpg_cert_digest_algo`            | `SHA512`               | Certificate signing digest          |

### Security Hardening

| Variable                          | Default    | Description                                         |
| --------------------------------- | ---------- | --------------------------------------------------- |
| `gpg_weak_digest`                 | `SHA1`     | Mark SHA1 as weak                                   |
| `gpg_disable_cipher`              | `[]`       | Disable specific ciphers (e.g., `["IDEA", "3DES"]`) |
| `gpg_s2k_cipher_algo`             | `AES256`   | Passphrase encryption cipher                        |
| `gpg_s2k_digest_algo`             | `SHA512`   | Passphrase hashing digest                           |
| `gpg_s2k_count`                   | `65011712` | Passphrase iteration count (max)                    |
| `gpg_require_cross_certification` | `true`     | Require subkey cross-certification                  |

### Trust Model

| Variable                  | Default    | Description                          |
| ------------------------- | ---------- | ------------------------------------ |
| `gpg_trust_model`         | `tofu+pgp` | Trust model (`tofu+pgp` recommended) |
| `gpg_tofu_default_policy` | `good`     | Default TOFU policy for new keys     |

### Display & Privacy

| Variable               | Default                          | Description                         |
| ---------------------- | -------------------------------- | ----------------------------------- |
| `gpg_keyid_format`     | `0xlong`                         | Key ID display format               |
| `gpg_with_fingerprint` | `true`                           | Always show fingerprints            |
| `gpg_no_emit_version`  | `true`                           | Don't include GPG version in output |
| `gpg_no_comments`      | `true`                           | Don't include comments in output    |
| `gpg_import_options`   | `[import-clean, repair-keys]`    | Import behavior                     |
| `gpg_export_options`   | `[export-clean, export-minimal]` | Export behavior                     |

### GPG Agent Configuration

| Variable                                   | Default | Description                                       |
| ------------------------------------------ | ------- | ------------------------------------------------- |
| `gpg_agent_default_cache_ttl`              | `300`   | Default cache timeout in seconds (5 minutes)      |
| `gpg_agent_max_cache_ttl`                  | `1800`  | Maximum cache timeout in seconds (30 minutes)     |
| `gpg_agent_default_cache_ttl_ssh`          | `300`   | SSH key cache timeout in seconds (5 minutes)      |
| `gpg_agent_max_cache_ttl_ssh`              | `1800`  | SSH key max cache timeout in seconds (30 minutes) |
| `gpg_agent_enforce_passphrase_constraints` | `true`  | Enforce passphrase rules (prevent bypass)         |
| `gpg_agent_min_passphrase_len`             | `12`    | Minimum passphrase length                         |
| `gpg_agent_min_passphrase_nonalpha`        | `2`     | Minimum non-alphabetic characters required        |
| `gpg_agent_max_passphrase_days`            | `365`   | Force passphrase change after N days              |
| `gpg_agent_no_allow_external_cache`        | `true`  | Prevent external passphrase caching               |
| `gpg_agent_grab`                           | `true`  | Grab keyboard/mouse to prevent X-sniffing attacks |

For complete variable list, see `defaults/main.yml`.

## Example Playbook

### Basic Usage

```yaml
- hosts: all
  roles:
    - pluggero.gpg
```

## Dependencies

None.

## License

MIT / BSD

## Author Information

This role was created in 2025 by Robin Plugge.
