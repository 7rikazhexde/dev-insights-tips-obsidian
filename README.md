# dev-insights-tips-obsidian

A repository that gathers insights and tips for development.\
It aims to share information related to programming and development.

## URL

Content(website) is managed by GitHub Pages.

[https://7rikazhexde-dev-insight-tips-obsidian.netlify.app/](https://7rikazhexde-dev-insight-tips-obsidian.netlify.app/)

## Usage

### For local

If you want to launch website in a local repository, do the following.

```bash
git clone https://github.com/7rikazhexde/dev-insights-tips-obsidian.git
```

> 🚨 **Note:**<br />
> **This project assumes that you have installed Poetry.**\
> **For Poetry installation, [check the official website](https://python-poetry.org/docs/#installing-with-the-official-installer).**

```bash
poetry install
```

> 🚨 **Note:**<br />
> **When using the pip command, please create a virtual environment in the [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv#pyenv-virtualenv) environment and retrieve the package.**
> **The pip command should also retrieve packages for development (requirements-dev.txt).**

```bash
pyenv virtualenv [version] [virtual environment name]
pyenv local [virtual environment name]
pip install -r requirements.txt
pip install -r requirements-dev.txt
```

create `.git/hooks/pre-commit`

> ℹ️ **Note:**<br />
> **Install pre-commit if you want to run static analysis before committing.**\
> **For pre-commit installation, [check the official website](https://pre-commit.com/)**

```bash
poetry run pre-commit install
```

> ℹ️ **Note:**<br />
> **When I set [pre-commit-hooks](https://pre-commit.com/#pre-commit-configyaml---hooks) and checked the operation, I confirmed that the files under the site folder were modified multiple times.**
> **Considering the commit process, I determined that there was no need to include files under the site folder in the hook process.**
> **Therefore, I decided to add a Python script that excludes files under the generated site folder and set them to be excluded.**
>
> **The following script can be executed to include them in the [exclude](https://pre-commit.com/#config-exclude) list.**

```bash
poetry run python ci/set_pre-commit-hooks_exclude.py
```

#### Launch local website

Execute the following command to access the URL displayed.
Check mkdocs for details.

- <https://www.mkdocs.org/>
- <https://squidfunk.github.io/mkdocs-material/>

```bash
poetry run mkdocs serve
```

#### Update dpcuments

Add or update content (.md) under docs. Then execute the following command.

```bash
poetry run mkdocs build -c
poetry run mkdocs serve
```

### For remote (Netify)

Content build and deployment is performed by setting up Netlify.\
See [netlify.toml](./netlify.toml) for build configuration.
