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

* python-version:
    - description: 'Version of Python to set up'
    - default: '3.8'
* git-username:
    - description: 'Git username for commits'
    - default: 'yourusername'
* git-email:
    - description: 'Git email for commits'
    - default: 'youremail@users.noreply.github.com'
* project-path:
    - description: 'Path to project source code'
    - default: 'src'
* files-pattern:
    description: 'Pattern to match example files'
    default: 'examples/*example*py'
* commit-message:
    description: 'Commit message for the documentation update'
    default: 'update docs'



## How to use
1. Create a workflow with the following yml code.
```
name: Sphinx Documentation

on:
  push:
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: ahmad88me/sphinx-docs-action@v1
        with:
          python-version: '3.8'
          git-username: 'yourusername'
          git-email: 'youremail@users.noreply.github.com'
          sphinx-project-path: 'src'
          sphinx-files-pattern: 'examples/*example*.py'
          commit-message: 'update docs'

```
2. Change the configurations to fit your needs. (e.g., github name and email).


## Limitations
* Note that this action deletes the docs everytime and regenerated again. So any modification will be deleted. Instead, change the documentation in the code
directly.
* custom `conf.py` is not yet supported. But you are welcome to add this feature to [sphinx_example_includer](https://github.com/ahmad88me/sphinx_example_includer).


