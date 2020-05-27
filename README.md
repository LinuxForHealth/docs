## Welcome to iDAAS-Connect

iDAAS Connectors for Inbound Data Processing

Powered by [Apache Camel](https://camel.apache.org/)

### Build

1. To build the idaas-connect.github.io site, first install `sphinx` and the `sphinx_rtd_theme` theme.
```
pip install sphinx sphinx_rtd_theme
```
Note: Tested with Python 3.7.7 and `pyenv global 3.7.7`.

2. Ensure that this line in conf.py contains the location of `sphinx_rtd_theme` in your environment.
```
sys.path.insert(0, os.path.abspath('/usr/local/lib/python3.7/site-packages'))
```

3. Build the site
```
sphinx-build -b html source docs
```
This builds the site from the files in the 'source' directory and places the built site in a 'docs' subdirectory of the idaas-connect/docs repo, which the repo is configured to use as the site folder.

4. Checkout a branch, push the new site to the idaas-connect/docs repo & merge the changes to master.  The changes will automatically appear on `https://idaas-connect.github.io/docs` after a few seconds.  You may need to reload the pages in your browser to see changes.

### References

[reStructuredText markup](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#rst-directives)  
[Sphinx quickstart](https://www.sphinx-doc.org/en/master/usage/quickstart.html)  
[sphinx_rtd_theme config options](https://sphinx-rtd-theme.readthedocs.io/en/latest/configuring.html). 

### Connect

View the iDAAS-Connect docs: `https://idaas-connect.github.io/docs`.
