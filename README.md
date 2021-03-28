# How to start a Django Project in 2020 with Fish Shell

[![Imports: isort](https://img.shields.io/badge/%20imports-isort-%231674b1?style=flat&labelColor=ef8336)](https://pycqa.github.io/isort/)
[![Code Style: black](https://img.shields.io/badge/code%20style-black-black)](https://github.com/psf/black)

## Set Python version

```fish
# For example
pyenv install 3.8.6
pyenv shell 3.8.6    # See NOTE 1 if there are any error in this step
```

## Create a Django Project

```fish
set -x PROJECT_NAME the_project
pip install --user Django
django-admin startproject $PROJECT_NAME
cd $PROJECT_NAME
mv $PROJECT_NAME/ config
find . -type f -name '*.py' -exec sed -i "s/$PROJECT_NAME/config/g" '{}' \;
```

## Run the project to test

```fish
python manage.py runserver
```

## Add Version Control

```fish
git init
curl https://raw.githubusercontent.com/github/gitignore/master/Python.gitignore --output .gitignore
# If use vscode add ".vscode/" to .gitignore
echo ".vscode/" >> .gitignore
git add .
git commit -m "codebase"
```

## Python environment for the project

```fish
pyenv local 3.8.6
git add .
git commit -m "set python version for the project"
```

## Manage dependencies with Poetry

```fish
poetry init -n
poetry config virtualenvs.in-project true --local
poetry add Django
```

## Run the project with Poetry

```fish
poetry run python manage.py runserver
git add .
git commit -m "add Poetry"
```

## Add pre-commit hooks and other tools

```fish
poetry add -D pre-commit
poetry run pre-commit install
poetry run pre-commit sample-config > .pre-commit-config.yaml
```

## Complete and Update Basic Python Hooks

```fish
touch .pre-commit-config.yaml

# Append config to yaml file 
echo "repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v3.4.0
    hooks:
      - id: check-builtin-literals
      - id: check-toml
      - id: check-yaml
      - id: debug-statements
  - repo: https://github.com/pycqa/isort
    rev: 5.8.0
    hooks:
      - id: isort
        name: isort (python)
        args: ["--profile", "black", "--filter-files"]
  - repo: https://github.com/ambv/black
    rev: 20.8b1
    hooks:
      - id: black
  - repo: local
    hooks:
      - id: flakehell
        name: flakehell
        entry: flakehell
        args: [lint]
        language: python
        types: [python]
" > .pre-commit-config.yaml

# Update hooks
poetry run pre-commit autoupdated

# Config flakehell, isort and black in vscode
echo '{
  "python.pythonPath": ".venv/bin/python3.8",
  "editor.formatOnSave": true,
  "python.terminal.activateEnvironment": true,
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": false,
  "python.linting.flake8Enabled": true,
  "python.linting.flake8Path": ".venv/bin/flake8helled",
  "python.formatting.provider": "black",
  "python.formatting.blackPath": "black",
  "[python]": {
    /* https://github.com/Microsoft/vscode-python/issues/1883#issuecomment-395216906 */
    "editor.formatOnPaste": false,
  },
  /* https://flakehell.readthedocs.io/ide.html#ide-integration */
  "cornflakes.linter.executablePath": "flake8hell",
  "cornflakes.linter.run": "onType",
}
' >> .vscode/settings.json
git add .
git commit -m "add and config pre-commit"
```

## Test pre-commit hooks

```fish
poetry run pre-commit run --all-files
# Fail?
poetry run pre-commit run --all-files
# Ok!

# commit formated and linted code
# Remember to have the venv activated (. ./venv/bin/activate.fish)
# or run git `poetry run git commit ...` Otherwise Flakehell won't work
poetry run git commit -m "black re-formated"
```
