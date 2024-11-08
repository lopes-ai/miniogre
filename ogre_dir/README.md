```
# miniogre

**miniogre** automates the management of software dependencies with AI, to ensure your Python code runs on any computer. It is a command-line application that analyzes a Python codebase to automatically generate a Dockerfile, requirements.txt file, and SBOM files, expediting the process of packaging any Python application. Additionally, it is able to update the README (documentation) file to comply with what really happens in the source code.

![miniogre_gif_33](https://github.com/ogre-run/miniogre/assets/6482944/0850dbb5-6634-4f08-80a9-6fd8e3ca8e03)

## Why miniogre

Developers waste hours per week managing software dependencies. This is particularly true in AI development where many Python packages lack proper documentation and have outdated configuration files. **Miniogre empowers developers** to automatically identify, update, and install the necessary software dependencies to get code to work. Unlike other tools that need manual setup, **miniogre uses AI to quickly handle Python dependencies setup**, cutting down "dependency hunting" from hours a week to just minutes.

## How it Works

Upon running the application, it carries out the following steps:

- The project directory is scrutinized to identify the primary code language.
- The README file is located and read.
- The source code is crawled to obtain a preliminary list of requirements.
- A large language model (LLM) provider (choices are `openai`, `mistral`, `groq`, `octoai`, `google`, `ollama`, or `local` if you want to skip this step and use an existing `requirements.txt` file) is used to refine the list of requirements and generate the final content for the `requirements.txt` file.
- The `requirements.txt`, `Dockerfile`, and `sbom.json` files are created.
- A Docker image of the application is built.
- An ogre container is spun up.

Two main commands can be run, with the `miniogre/main.py` file serving as the entry point.

- **run**: Executes a series of actions, including configuring directories and files (bashrc, Dockerfile), generating requirements, building a Docker image, and spinning up a container.
- **readme**: Constructs a new `README.md` file that mirrors the operations observed within the source code.

For more in-depth execution details, refer to `miniogre/main.py`,`miniogre/actions.py`, and `miniogre/config.py`.

## Requirements

To use miniogre effectively, ensure the following are installed:

- Python 3.10 or higher.
- Docker.
- pipx (recommended) or pip.
- An API token for at least one of the following LLM providers, if you are generating or rewriting files that use them:
    - OpenAI (`OPENAI_API_KEY`).
    - Mistral (`MISTRAL_API_KEY`).
    - Groq (`GROQ_API_KEY`).
    - OctoAI (`OCTOAI_TOKEN`).
    - Google Gemini (`GOOGLE_API_KEY`).  **Note:** Renamed from GEMINI_API_KEY for consistency and correctness.
    - Ollama (`base_url` for your Ollama instance). Ollama itself needs to be installed separately.
    - Ogre (`OGRE_API_SERVER`, `OGRE_TOKEN`). Ogre itself needs to be installed separately.


## Installation
Miniogre can be installed either by using `pip` or `pipx`:
- `pip install miniogre`
- `pipx install miniogre`

You can also build the wheel from the source and then install it on your system:

```bash
poetry build && pipx install dist/*.whl
```

## Usage

After installation, go inside the project folder and run:

`miniogre run`

This will analyze the project, generate `ogre_dir/Dockerfile`, `ogre_dir/requirements.txt`, and `ogre_dir/sbom.json` and build a Docker image. If you want to skip container build and startup, use the flag `--no-container`.

### Commands
- `run`: Executes a series of actions, including configuring directories and files (bashrc, Dockerfile), generating requirements, building a Docker image, and spinning up a container.
- `readme`: Analyzes the source code to generate a new README.md file that reflects the actual operations in the source code.
- `eval`: Determines the reproducibility score of the repository by evaluating the README quality.
- `spinup`: Spins up a container if an image was previously built with the run command.
- `version`: Displays the current version of miniogre.
- `ask`:  Ask questions about the codebase and request suggestions (e.g., `miniogre ask "<your_question_or_code_issue>" --provider ogre`).
- `cloud`: Move local work to the cloud by packaging the repository into a tarball and sending it to a fileserver.


### Build Ogre base image

Useful to create a Docker image that can be deployed on Google Cloud Run:

`miniogre build-ogre-image --host-platform linux/amd64 --baseimage ogrerun/base:ubuntu22.04-amd64 --verbose --no-cache`

## Contributing
Contributions to improve this resource are more than welcome. For inquiries, contact the maintainers at [contact@ogre.run](contact@ogre.run).
```
