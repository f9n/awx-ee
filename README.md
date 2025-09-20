# AWX EE

The default Execution Environment for AWX.

## Version 0.1.0 - Technical Specifications

| Component | Version/Requirement |
|-----------|-------------------|
| **Operating System** | Fedora 42 |
| **Python Version** | Python 3.13 |
| **Ansible Version** | ansible-core 2.18.9 |
| **Target Host Min Python Version** | Python >= 3.8 |

### Included Collections

- awx.awx
- azure.azcollection
- amazon.aws
- theforeman.foreman
- google.cloud
- openstack.cloud
- community.vmware
- ovirt.ovirt
- kubernetes.core
- ansible.posix
- ansible.windows
- redhatinsights.insights
- kubevirt.core

### System Dependencies

- git-core, git-lfs
- python3.13-devel
- Kerberos support (krb5-devel, krb5-workstation)
- Development tools (gcc, gcc-c++, make, cmake)
- Network tools (libcurl-devel, openssl-devel)
- Container tools (podman-remote)
- Essential utilities (rsync, unzip, sshpass, subversion)

### Python Dependencies

- ansible-sign, ncclient, paramiko
- Kerberos support (pykerberos)
- SSL/TLS (pyOpenSSL)
- Windows management (pypsrp, pywinrm with kerberos & credssp)
- Configuration management (pyyaml, toml)
- Process management (pexpect, python-daemon)
- AWX integration (receptorctl)

## Build the image locally

### Prerequisites

1. **Install ansible-builder**
   ```bash
   pip install ansible-builder
   ```
   
2. **Container runtime** (one of the following):
   - Podman (default, recommended)
   - Docker

### Build Commands

**Basic build with Podman (default):**
```bash
ansible-builder build -v3 -t awx-ee:0.1.0 .
```

**Build with Docker:**
```bash
ansible-builder build -v3 -t awx-ee:0.1.0 --container-runtime=docker .
```

**Build with custom tag for registry:**
```bash
ansible-builder build -v3 -t quay.io/your-org/awx-ee:0.1.0 .
```

**Build with build outputs (useful for debugging):**
```bash
ansible-builder build -v3 -t awx-ee:0.1.0 --build-outputs-dir ./build-outputs .
```

### Verify the build

After successful build, verify the image:

```bash
# Check image size and details
podman images awx-ee:0.1.0

# Test Ansible installation
podman run --rm awx-ee:0.1.0 ansible --version

# Test Python version
podman run --rm awx-ee:0.1.0 python --version

# List installed collections
podman run --rm awx-ee:0.1.0 ansible-galaxy collection list
```

### Troubleshooting

- **Build fails with permission errors:** Ensure your user is in the `docker`/`podman` group
- **Network timeouts:** Use `--build-arg HTTP_PROXY=<your-proxy>` if behind a proxy
- **Disk space issues:** Clean up unused containers/images: `podman system prune -a`
