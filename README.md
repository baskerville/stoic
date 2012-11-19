![logo](https://github.com/baskerville/stoic/raw/master/logo/stoic-logo.png)

## Usage

    SYNOPSIS
        stoic [OPTIONS]

    OPTIONS
        -h, --help
            Show this help message.

        -c, --copy-mode
            Copy the unknown files to the output directory.
        

## Description

**stoic** is a tiny static site generator.

It builds a tree of items by walking in the *site directory* and render that tree in the *output directory*.

## Configuration

The settings are read from `./settings.py`.

## Settings

- `title`: site title
- `home`: site url
- `site_dir`: directory to build the tree of items from
- `output_dir`: directory to render the tree to
- `layouts_dir`: directory to read the layouts from
- `exclude`: list of directories not to be included in the tree
- `items_per_page`: maximum number of items per page
- `custom_filters`: a dictionary which will be merged to the Jinja2 environment filters
- `default_layout`: default layout (path is relative to `layouts_dir`)
- `source_extension`: file extension of the markdown files (default: 'md')
- `source_index`: file name of the markdown *index* (default: 'index.md')
- `destination_index`: file name of the output *index* (default: 'index.html')

## Details

**stoic** scans `site_dir` for files having a `source_extension` extension.

Unless `--copy-mode` is passed, **stoic** ignores any other files.

The output paths are computed as so:

- `foo/bar.md` becomes `foo/bar/index.html`
- `foo/index.md` becomes `foo/index.html`

The layouts will be given the following elements:

- item: the node being processed, carries the following attributes:
    - url
    - parent
    - children: the list of children
    - prev, next (only if paginated)
- trail: (of type *list*), the path from item to root
- root: the root item
- title: the site title
- home: the site url

Extra attributes are set through the YAML front matter of the markdown files.

You might want to set the `layout` attribute to indicate the layout of the related item.

To get the url of item `a` relative to item `b`, use the following:

    a.relurl(b)

Pagination will occur on item `a` if `items_per_page > 0`, `a.paginate` is true (and if `len(a.children) > items_per_page`).

The `ignore` attribute can be used to prevent rendering on the related item.

## Requirements

Python 3 and the following python libraries:

- Jinja2
- Misaka
- PyYAML
- Beautiful Soup 4 
