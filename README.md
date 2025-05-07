# get-versions

`get-versions` is a Bash utility that generates pinned package installation commands for Debian/Ubuntu-based Dockerfiles or flat lists. It resolves a package's full dependency tree, determines each dependency's candidate version and architecture, and outputs either:

* A **single** Dockerfile `RUN` command that installs all packages with pinned versions and architectures.
* A **list** mode that prints each `package:arch=version` on its own line for use in scripts or documentation.

## Features

* **Recursive dependency resolution** using `apt-cache depends --recurse`.
* **Batch version lookup** via a single `apt-cache policy` call for speed.
* **Architecture-aware** pins, e.g. `openssl:amd64=3.0.13-0ubuntu3.5`.
* **`--list` mode** for flat file output: one `package:arch=version` per line.
* **No external dependencies** beyond standard `apt-cache` and `apt-get` commands.
* **Stable, sorted output** for reproducible builds.

## Installation

Clone this repository and make the script executable:

```bash
git clone https://github.com/your-org/get-versions.git
cd get-versions
chmod +x get-versions
```

## Usage

```bash
# Generate a Dockerfile RUN line for curl and jq
echo "$(./get-versions curl jq)" > Dockerfile

# Generate a flat list for a file
./get-versions --list openssl ca-certificates jq curl gnupg lsb-release > packages.txt
```

### Options

* `--list` – Outputs each `package:arch=version` on its own line rather than a single `RUN` command.
* `<package1> [package2 ...]` – One or more package names to resolve and pin.

### Examples

**Dockerfile mode (default):**

```bash
$ ./get-versions curl gnupg lsb-release
RUN apt-get update && apt-get install -y \
  curl:amd64=8.5.0-2ubuntu10.6 \
  gnupg:amd64=2.4.4-2ubuntu17.2 \
  gpg:amd64=2.4.4-2ubuntu17.2 \
  gpg-agent:amd64=2.4.4-2ubuntu17.2 \
  gpgconf:amd64=2.4.4-2ubuntu17.2 \
  lsb-release:all=12.0-2
```

**List mode:**

```bash
$ ./get-versions --list curl gnupg lsb-release
curl:amd64=8.5.0-2ubuntu10.6
gnupg:amd64=2.4.4-2ubuntu17.2
gpg:amd64=2.4.4-2ubuntu17.2
gpg-agent:amd64=2.4.4-2ubuntu17.2
gpgconf:amd64=2.4.4-2ubuntu17.2
lsb-release:all=12.0-2
```

## Contributing

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m 'Add your feature'`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a Pull Request

Please ensure code style consistency and add tests for new behavior where applicable.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
