# Python development container

A reusable Docker-based development environment for Python coursework, including the separate [py-fun project](https://github.com/blastudeh/py-fun). This repository supplies environment configuration rather than an application.

## What is included

| File | Purpose |
|---|---|
| `Dockerfile` | Builds from `python:3.12-slim`, installs Git and Make, upgrades pip, and sets `/workspace` as the working directory. |
| `devcontainer.json` | Configures the VS Code workspace mount, Python extensions, editor preferences and optional requirements installation. |
| `.termrc` | Terminal initialization file referenced by the container configuration. |
| `.bashrc`, `.profile` | Supporting shell configuration files. |

## Use with a Python project on Ubuntu

You need Docker running, VS Code and its Dev Containers extension. In your Ubuntu VM, run these tools inside Ubuntu so they use the same filesystem and Docker engine.

1. Download this repository and copy `Dockerfile`, `devcontainer.json`, `.termrc`, `.bashrc` and `.profile` into a `.devcontainer/` folder inside your Python project.
2. Keep your project source and any `requirements.txt` at the project root, outside `.devcontainer/`.
3. Open that project folder in VS Code and run **Dev Containers: Reopen in Container** from the command palette.
4. After the build, open a terminal and check `python --version`, `git --version` and `make --version`.
5. Run the application's own setup, tests and build commands. For py-fun, see its README.

The `.devcontainer/` layout matters: the terminal profile explicitly reads `/workspace/.devcontainer/.termrc`. The configuration installs `requirements.txt` only when it exists. The host project is bind-mounted at `/workspace`, so edits there affect your project files.

## Build the image directly

From this repository's root, with Docker available:

```bash
docker build -t python-coursework-dev .
docker run --rm -it -v "$PWD:/workspace" python-coursework-dev bash
```

This direct Docker route does not apply VS Code settings, extensions or the `postCreateCommand`; install project dependencies separately when needed.

## Troubleshooting and status

- `make: command not found`: rebuild the image after pulling the Dockerfile changes.
- Container name conflict: the VS Code configuration requests `dev-container-python312`. Stop the conflicting workspace or choose a different name for a second workspace.
- Missing terminal initialization: check that `.termrc` was copied into the project's `.devcontainer/` folder.
- Docker permission errors: verify Docker works from the same Ubuntu user account before reopening the container.

Documentation was checked against the committed Dockerfile and configuration on 8 November 2026. This documentation update did not rebuild the image or verify every editor setting. Base image tags and pip upgrades are not pinned to immutable versions.
