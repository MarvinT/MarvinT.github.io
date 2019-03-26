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

but they were all, missing features, were out of date and had issues.

Writing my own version would have been 2 short files, however, I found that [code_prettify](https://jupyter-contrib-nbextensions.readthedocs.io/en/latest/nbextensions/code_prettify/README_code_prettify.html) is so extensible that you can easily configure it to use Black using the nbextensions configurable parameters. It also automatically deals with things like running for all selected cells, not having to run the cells, allowing for the cells to be edited while running, and I've updated it to deal with Black's changing API.

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
```json
{
  "python": {
    "library": "import json\ndef black_reformat(cell_text):\n    import black\n    import re\n    cell_text = re.sub('^%', '#%#', cell_text, flags=re.M)\n    try:\n        reformated_text = black.format_str(cell_text, 88)\n    except TypeError:\n        reformated_text = black.format_str(cell_text, mode=black.FileMode(line_length=88))\n    return re.sub('^#%#', '%', reformated_text, flags=re.M)",
    "prefix": "print(json.dumps(black_reformat(u",
    "postfix": ")))"
  },
  "r": {
    "library": "library(formatR)\nlibrary(jsonlite)",
    "prefix": "cat(toJSON(paste(tidy_source(text=",
    "postfix": ", output=FALSE)[['text.tidy']], collapse='\n')))"
  },
  "javascript": {
    "library": "jsbeautify = require('js-beautify')",
    "prefix": "console.log(JSON.stringify(jsbeautify.js_beautify(",
    "postfix": ")));"
  }
}
```
Here's the same code in a linkable gist
<script src="https://gist.github.com/MarvinT/a072aa992e977496974aaf492287b08c.js"></script>


### To install:
1. go to your Nbextensions in your jupyter notebook
   - to install nbextensions see https://github.com/ipython-contrib/jupyter_contrib_nbextensions#1-install-the-python-package
2. enable Code prettify
3. paste above code into text box labeled `json defining library calls required to load the kernel-specific prettifying modules, and the prefix & postfix for the json-format string required to make the prettifying call.`
