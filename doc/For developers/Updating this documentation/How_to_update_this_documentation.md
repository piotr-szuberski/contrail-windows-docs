# How to update this documentation

## How does it work

Documentation is stored on master branch of Juniper/contrail-windows-docs repository. It is stored in Markdown (.md) format. Documentation is served as HTML on github.io. HTML pages are generated by MkDocs tool. HTML pages need to be redeployed manually after modification.

## Documentation update procedure (procedure for developer)

1. Open a pull request to master branch in repository Juniper/contrail-windows-docs with your changes.
2. Once PR is accepted, an administrator is going to deploy new modifications to gh-pages branch.

## How to preview modifications (procedure for developer)

1. Install MkDocs (see below).
2. Apply required modifications to the documentation.
3. Invoke `mkdocs serve` in main repository directory. It's going to start local HTTP server with documentation preview. Follow instructions displayed by mkdocs to see the outcome.

## How to install MkDocs (procedure for administrator and developer)

MkDocs requires Python (both Python 2 and Python 3 are supported) and PIP. Follow instructions on [http://www.mkdocs.org/#installation](http://www.mkdocs.org/#installation).

1. Create a virtualenv
```
mkdir venv
virtualenv --python=python2 venv
source venv/bin/activate
```
1. Install requirements
```
pip install -r requirements.txt
```

## How to deploy modifications (procedure for administrator)

After master branch of Juniper/contrail-windows-docs repository is updated, github.io pages need to be redeployed manually. To do this, follow these steps:

1. Install MkDocs.
2. Checkout master branch of repository Juniper/contrail-windows-docs.
3. Invoke `mkdocs gh-deploy` in main repository directory and follow displayed instructions. Additional details are described in [http://www.mkdocs.org/user-guide/deploying-your-docs/](http://www.mkdocs.org/user-guide/deploying-your-docs/).
