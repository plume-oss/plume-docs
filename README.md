# Plume Documentation

Source code for the Plume documentation. The documentation is run 
with [MkDocs Material](https://squidfunk.github.io/mkdocs-material/).

## Getting Started

Install `mkdocs` with `pip3`:

```bash
pip3 install mkdocs          # to install mkdocs
pip3 install mkdocs-material # to install mkdocs-material
```

or with `conda` (ARM friendly):

```bash
conda install mkdocs          # to install mkdocs
conda install mkdocs-material # to install mkdocs-material
```

To run development server, use:

```bash
mkdocs serve
```

## Contributing

The only thing to note is that lines are wrapped at 100. This can be configured in VSCode by 
installing the Rewrap plugin and adding the following configuration:

```json
{
    "[markdown]": {
        "editor.rulers": [100]
    },
    "rewrap.autoWrap.enabled": true,
}
```