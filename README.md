# How to start a Django Project in 2020 with Fish Shell

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
