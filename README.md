## Welcome to Linux for Health

Linux for Health for healthcare data interoperability

Powered by [Apache Camel](https://camel.apache.org/)

### Build

1. Clone the this repo and `cd docs`.
```
git clone https://github.com/linuxforhealth/docs
```

2. To build the linuxforhealth.github.io site, first install `sphinx` and the `sphinx_rtd_theme` theme.
```
pip install sphinx sphinx_rtd_theme
```
Note: Tested with Python 3.7.7 and `pyenv global 3.7.7`.

3. Ensure that this line in conf.py contains the location of `sphinx_rtd_theme` in your environment.
```
sys.path.insert(0, os.path.abspath('/usr/local/lib/python3.7/site-packages'))
```

4. Build the site
```
sphinx-build -E -b html source docs
```
This builds the site from the files in the 'source' directory and places the built site in a 'docs' subdirectory of the linuxforhealth/docs repo, which the repo is configured to use as the site folder.

5. Checkout a branch, push the new site to the linuxforhealth/docs repo & merge the changes to master.  The changes will automatically appear on [https://linuxforhealth.github.io/docs](https://linuxforhealth.github.io/docs) after a few seconds.  You may need to reload the pages in your browser to see changes.

### References

[reStructuredText markup](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#rst-directives)  
[Sphinx quickstart](https://www.sphinx-doc.org/en/master/usage/quickstart.html)  
[sphinx_rtd_theme config options](https://sphinx-rtd-theme.readthedocs.io/en/latest/configuring.html)

### Connect

View the Linux for Health docs: [https://linuxforhealth.github.io/docs](https://linuxforhealth.github.io/docs)  
Check out the Linux for Health source: [https://github.com/linuxforhealth/linux-for-health](https://github.com/linuxforhealth/linux-for-health)
