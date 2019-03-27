---
title: Black Jupyter Notebook
toc: true
toc_label: "Black Jupyter"
---

I was looking to auto format my notebooks with [Black](https://github.com/ambv/black)

I found a couple such as
 - https://github.com/drillan/jupyter-black
 - https://github.com/dnanhkhoa/nb_black
 - https://github.com/csurfer/blackcellmagic
 - https://github.com/tobinjones/jupyterlab_formatblack
 - https://github.com/betatim/joli

However, they were all missing features, were out of date, and had issues.

Writing my own version would have been 2 short files, however, I found that [code_prettify](https://jupyter-contrib-nbextensions.readthedocs.io/en/latest/nbextensions/code_prettify/README_code_prettify.html) is so extensible that you can easily configure it to use Black using the nbextensions configurable parameters. 

### Features (and issues solved)
 - skip jupyter magic lines
   - https://github.com/csurfer/blackcellmagic/issues/5
   - https://github.com/drillan/jupyter-black/issues/5
 - configurable shortcuts
 - adds a toolbar button
 - runs for all selected cells
   - https://github.com/drillan/jupyter-black/issues/2
 - can format whole notebook
 - don't have to run the cells
 - don't have to load ext each notebook
 - don't have to add notebook magics to each cell
 - allowing for the cells to be edited while running
 - deal with Black's changing API and works with the most recent Black version, as well as previous ones
   - https://github.com/csurfer/blackcellmagic/issues/9
   - https://github.com/drillan/jupyter-black/issues/4
   - https://github.com/dnanhkhoa/nb_black/issues/2
 - works with the most recent jupyter version
   - https://github.com/drillan/jupyter-black/issues/6

The python code I've written to do the black formatting is:
```python
def black_reformat(cell_text):
    import black
    import re

    cell_text = re.sub("^%", "#%#", cell_text, flags=re.M)
    try:
        reformated_text = black.format_str(cell_text, 88)
    except TypeError:
        reformated_text = black.format_str(
            cell_text, mode=black.FileMode(line_length=88)
        )
    return re.sub("^#%#", "%", reformated_text, flags=re.M)
```

but code_prettify takes a json configuration file that runs javascript so here's the json you need to paste into the configuration window:
<script src="https://gist.github.com/MarvinT/a072aa992e977496974aaf492287b08c.js"></script>


### To install
1. go to your Nbextensions in your jupyter notebook
   - to install nbextensions see https://github.com/ipython-contrib/jupyter_contrib_nbextensions#1-install-the-python-package
2. enable Code prettify
3. paste above json into text box labeled `json defining library calls required to load the kernel-specific prettifying modules, and the prefix & postfix for the json-format string required to make the prettifying call.`

This also requires black to be installed in the running kernel and it therefore goes without saying this only works in py>=3.6
