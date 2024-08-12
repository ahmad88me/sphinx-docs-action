# sphinx-docs-action
Action to generate sphinx docs using sphinx-example-includer.


## Overview
This GitHub Action automates the generation of Sphinx documentation using [sphinx_example_includer](https://github.com/ahmad88me/sphinx_example_includer). It streamlines the process by building the documentation structure, fetching project metadata, extracting documentation from source code, and generating HTML files. The final output is cleaned and prepared for deployment via GitHub Pages.


## Features
*  *Automated Documentation Generation:* Generates Sphinx documentation effortlessly without specifying the configurations (but you can if you want).
* *Flexible Configuration:* Easily customizable to suit various project needs.
* *Metadata Extraction:* Automatically fetches metadata from pyproject.toml.
* *Extract Source Code Documentation:* Extracts and includes documentation from your source code using `sphinx-apidoc`.
* *Ready for Deployment:* Cleans and prepares the documentation folder for GitHub Pages.


## Prerequisites
Ensure your project has the following files configured:

* `pyproject.toml` for project metadata.
* Properly documented source code compatible with Sphinx.
* Your source code is located in`src`. If not, make sure to change the configuration to specify the directory of your code.


## Outputs
* *HTML Documentation:* The action generates and prepares HTML files for your project documentation.
* Cleaned Docs Folder:*  Ensures the docs/ directory is ready for deployment to GitHub Pages.



## Configuration
You can customize the action to fit your project needs. Here are some configuration options:

* *GitHub Username:* You can specify the username of the git push.
* *GitHub Email:* Specify the email of the user that does the push.
* *Commit Message:* You can change the commit message of the git push.
* *[sphinx_example_includer](https://github.com/ahmad88me/sphinx_example_includer) Configurations:* You can modify sphinx example includer configurations as well.



## How to use
1. Create a workflow with the following yml code.
```
name: Sphinx Docs
author: Ahmad Alobaid
description: Automates Sphinx docs generation: builds structure, extracts metadata, creates HTML, cleans up, and preps for GitHub Pages.

on:
  push:
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Set up Python 3.8
      uses: actions/setup-python@v3
      with:
        python-version: "3.8"
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip sphinx-autodoc-typehints sphinx_example_includer sphinx docutils
        if [ -f requirements.txt ]; then pip install -r requirements.txt; fi
    - name: Remove docs folder
      run: |
        rm -Rf docs
    - name: Generate documentation
      run: |
        python -m sphinx_example_includer --info --build --project src --files examples/*example*.py --overwrite
        cd docs; ls;find . -maxdepth 1 ! -name '_build' ! -name '.' ! -name '..' -exec rm -rf {} +
        mv _build/html/* ./
        touch .nojekyll
        cd ..
    - name: Configure Git
      run: |
        git config --global user.name "yourusername"
        git config --global user.email "youremail@users.noreply.github.com"
    - name: Commit changes
      run: |
        git status -u
        git add docs
        git commit -m "update docs"
    - name: Push changes
      run: |
        git push
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
2. Change the configurations to fit your needs. (e.g., github name and email).


## Limitations
* Note that this action deletes the docs everytime and regenerated again. So any modification will be deleted. Instead, change the documentation in the code
directly.
* custom `conf.py` is not yet supported. But you are welcome to add this feature to [sphinx_example_includer](https://github.com/ahmad88me/sphinx_example_includer).


