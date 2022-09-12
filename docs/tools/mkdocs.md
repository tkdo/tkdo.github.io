


ss@ssdeMacBook-Pro tkdo-site % mkdocs serve
INFO     -  Building documentation...
1) ERROR    -  Config value: 'theme'. Error: Unrecognised theme name: 'material'. The available installed themes are: mkdocs, readthedocs
2) ERROR    -  Config value: 'markdown_extensions'. Error: Failed to load extension 'pymdownx.arithmatex'.
            ModuleNotFoundError: No module named 'pymdownx'
3) ERROR    -  Config value: 'plugins'. Error: The "tags" plugin is not installed


1) pip install  mkdocs-material
2) pin3 install pymdown-extensions
3) pip3 install mkdocs-git-revision-date-plugin  && pip install mkdocs-minify-plugin






