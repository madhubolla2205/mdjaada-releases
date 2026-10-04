# MDJaada agent releases

Downloads of the **MDJaada agent**: the small program that checks your databases, proxies, queues and services
from inside your network and reports to your MDJaada server. This repository holds release files only.

- What MDJaada is, and how to get a pilot: https://madhubolla2205.github.io/mdjaada-website/
- Latest release: https://github.com/madhubolla2205/mdjaada-releases/releases/latest

## Install as a service (Linux with systemd)

```bash
curl -fsSLO https://github.com/madhubolla2205/mdjaada-releases/releases/latest/download/install.sh
sudo bash install.sh --api https://<your-mdjaada-server>:8080/api --agent-id dc1-agent --assigned-only
```

The installer asks for the API key from your MDJaada server, downloads the binary that matches the installer's
release and this server's CPU, verifies its checksum (and the Sigstore signature when `cosign` is installed; add
`--require-signature` to insist on it), and runs it as the hardened `mdjaada-agent` service.

```bash
journalctl -u mdjaada-agent -f                # logs
sudo bash install.sh --upgrade                # newer release (download the newer install.sh first)
sudo bash install.sh --uninstall --purge      # remove everything
```

Each binary is a single file with the Postgres, MySQL/MariaDB, MongoDB, Kafka and YAML drivers inside, built on a
glibc 2.28 base: RHEL/Alma/Rocky 8+, Ubuntu 20.04+, Debian 10+, on x86-64 and ARM64. For AWS, Google Cloud, Azure or
Snowflake checks, use the Python install from the MDJaada package (`--drivers aws,...`) or the Docker image.

Every release lists `SHA256SUMS` signed with [Sigstore](https://www.sigstore.dev/) by the MDJaada release workflow;
the release notes show the exact `cosign verify-blob` command.
