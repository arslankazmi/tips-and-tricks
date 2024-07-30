# Quick and Dirty Dashboards from Notebooks

Voila can be combined with jupyter-flex to create a dashboard from a jupyter notebook.

The tools/python libraries used were:

1. jupyter-flex
2. voila

Voila takes any notebook and displays any outputs sequentially in an html page while hiding the code cells.

Jupyter-flex allows me to arrange these outputs in a dashboard-like grid format by just annotating certain cells and using different level headings.

Combining voila with jupyter-flex requires running voila with a special runtime parameter: --template=flex

```bash
voila --template=flex notebooks/<notebook-filename>.ipynb --no-browser --VoilaConfiguration.file_allowlist="['.*']" --classic-tree
```

Using jupyter-flex is simple too:

- mark cells which produce outputs you want to display with the tag "body"
- mark a code cell with the tag "parameters" and use ```flex_title = "<title of dashboard>"``` to set dashboard title. Otherwise it will use the notebook filename as the title.
- use second-level headings to denote rows or sections: ```## Row 1```
- use third-level headings to denote individual widgets or cards: ```### Cell 1```
