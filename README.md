# Automated Homework Evaluation - concept documentation

This repository is the source of the documentation website at <https://akosdudas.github.io/automated-homework-evaluation>.

## How to build the website locally

### Using Visual Studio Code Remote Containers

Pre-requisites: Docker, Visual Studio Code, [Remote Development extension](https://aka.ms/vscode-remote/download/extension)

1. Start VS Code open the repository inside a container:
   - Run command (Ctrl-Shift-P or F1) `Remote-Containers: Open Folder in Container...` and browse the repository folder.
   - Or open the repository root folder and follow popup instructions to "Re-open folder in container."
1. Open a new Terminal and execute command `mkdocs serve`.

### Using Docker

Pre-requisites: Docker

1. Open a Powershell console to the repository root
1. `docker run -it --rm -p 8000:8000 -v ${PWD}:/docs squidfunk/mkdocs-material:7.3.0`
1. Open <http://localhost:8000> in a browser to view the live content.

### Using Python and pip

Pre-requisites: Python 3

1. Install package using `pip install mkdocs-material==7.3.0`
1. Start `mkdocs serve` in the repository root
