# Contributing to balance
We want to make contributing to this project as easy and transparent as
possible.

## Pull Requests
We actively welcome your pull requests.

1. Fork the repo and create your branch from `main`.
2. If you've added code that should be tested, add tests.
3. If you've changed APIs, update the documentation.
4. Ensure the test suite passes.
5. Make sure your code lints.
6. If you haven't already, complete the Contributor License Agreement ("CLA").

## Contributor License Agreement ("CLA")
In order to accept your pull request, we need you to submit a CLA. You only need
to do this once to work on any of Meta's open source projects.

Complete your CLA here: <https://code.facebook.com/cla>

## Issues
We use GitHub issues to track public bugs. Please ensure your description is
clear and has sufficient instructions to be able to reproduce the issue.

Meta has a [bounty program](https://www.facebook.com/whitehat/) for the safe
disclosure of security bugs. In those cases, please go through the process
outlined on that page and do not file a public issue.

## Code Requirements

### Coding Style
* 4 spaces for indentation rather than tabs
* 80 character line length

### Linting
Run the linter via `flake8` (`pip install flake8`) from the root of the Ax repository. Note that we have a [custom flake8 configuration](https://github.com/facebookresearch/balance/blob/main/.flake8).

### Static Type Checking
We use [Pyre](https://pyre-check.org/) for static type checking and require code to be fully type annotated.

### Unit testing
We strongly recommend adding unit testing when introducing new code. To run all unit tests, we recommend installing pytest using `pip install pytest` and running `pytest -ra` from the root of the balance repo.

### Documentation
* We require docstrings on all public functions and classes (those not prepended with `_`).
* We use the [Google docstring style](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html) & use Sphinx to compile API reference documentation.
* Our [website](https://import-balance.org) leverages Docusaurus 2.0 + Sphinx + Jupyter notebook for generating our documentation content.
* To rule out parsing errors, we suggesting [installing sphinx](https://www.sphinx-doc.org/en/master/usage/installation.html) and running `make html` from the balance/sphinx folder.

## Website Development

### Overview
balance's website is also open source and part of this repository. balance leverages several open source frameworks for website development.
* [Docusaurus 2](https://docusaurus.io/): The main site is generated using Docusaurus, with the code living under the [website](https://github.com/facebookresearch/balance/tree/main/website) folder). This includes the website template (navbar, footer, sidebars), landing page, and main sections (Blog, Docs, Tutorials, API Reference).
  * Markdown is used for the content of several sections, particularly the "Docs" section. Files are under the [docs/](https://github.com/facebookresearch/balance/tree/main/website/docs/docs) folder
* [Jupyter notebook](https://fburl.com/55p6vvxo) is used to generate the notebook tutorials under the "Tutorials" section, based on our ipynb tutorials in our [tutorials](https://github.com/facebookresearch/balance/tree/main/tutorials) folder.
* [Sphinx](https://www.sphinx-doc.org/en/master/index.html) is used for Python documentation generation, populated under the "API Reference" section. Files are under the [sphinx](https://github.com/facebookresearch/balance/tree/main/sphinx) folder.


### Setup

To install the necessary dependencies for website development, run the following from the repo root:
```
python -m pip install git+https://github.com/bbalasub1/glmnet_python.git@1.0
python -m pip install .[dev]
```

### Adding Notebook Tutorials
All our notebook tutorials are housed under the [tutorials](https://github.com/facebookresearch/balance/tree/main/tutorials) folder at the root of the repo. We use these notebooks as the source of truth for the "Tutorials" section of the website, executing & generating HTML pages for each notebook.

To add a new tutorial:
1. Check in your notebook (.ipynb) to our [tutorials](https://github.com/facebookresearch/balance/tree/main/tutorials) folder. We strongly suggest clearing notebook output cells.
2. Extend the "Building tutorial HTML" section of [`scripts/make_docs.sh`](https://github.com/facebookresearch/balance/blob/main/scripts/make_docs.sh) to execute & generate HTML for the new tutorial e.g. `jupyter nbconvert tutorials/my_tutorial.ipynb --execute --to html --output-dir website/static/html/tutorials`.
3. Introduce a new .mdx page under the [website/docs/tutorials](https://github.com/facebookresearch/balance/tree/main/website/docs/tutorials) folder for the new tutorial. Use HTMLLoader to load the generated HTML e.g. `<HTMLLoader docFile={useBaseUrl('html/tutorials/my_tutorial.html')}/>`. [quickstart.mdx](https://github.com/facebookresearch/balance/blob/main/website/docs/tutorials/quickstart.mdx) is a good reference for the setup

To test the setup, see the [`Building & Testing Website Changes`](#building--testing-website-changes) section below.

Note: The generated HTML should not be checked into the main repo.

### Building & Testing Website Changes
We've developed a helper script for running the full website build process:

```
./scripts/make_docs.sh

# To start up the local webserver
cd website
yarn serve
```
Once the local webserver is up, you'll get a link you can follow to visit the newly-built site. See [Docusaurus docs](https://docusaurus.io/docs/deployment#testing-build-locally) for more info.

## Deployment
We rely on Github Actions to run our CI/CD. The workflow files can be found [here](https://fburl.com/5kwhksbu).

In summary
* On every pull request, we run our "Build & Test" workflow, which includes PyTest tests, Wheels package builds, flake8 linting, and website build.
* We also run the same "Build & Test" suite nightly.
* On every push, we deploy a new version of the website. The `make_docs.sh` script is run from the main branch and the build artifacts are published to the `gh-pages` branch, which is linked to our repo's Github Page's deployment.

### Releasing a new version
To create a new release, simply navigate to the ["Release" page](https://github.com/facebookresearch/balance/releases) of the repo, draft a new release, and publish. The Github Action workflow should be triggered on publish and you should see a new version of the package live on PyPi in ~10 mins. You can check the status of the job via the [GH Actions tab](https://github.com/facebookresearch/balance/actions).

Guidelines when drafting a new release:
* Follow semantic versioning conventions when chosing the next version.
* The release's tag should only be the version itself (e.g. "0.1.0"). Do not add any prefixes like "v" or "version". The build process relies on proper formatting of this tag.

The Github Actions job is configured at [release.yml](https://github.com/facebookresearch/balance/blob/main/.github/workflows/release.yml).

## License
By contributing to balance, you agree that your contributions will be licensed
under the LICENSE file in the root directory of this source tree.
