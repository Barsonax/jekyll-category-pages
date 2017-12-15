# jekyll-category-pages

[![Gem Version](https://img.shields.io/gem/v/jekyll-category-pages.svg)](https://rubygems.org/gems/jekyll-category-pages)

This plugin adds category index pages with and without pagination.  
Benefits are:
* Easy to setup and use, fully compatible with the default [pagination
    plugin](https://github.com/jekyll/jekyll-paginate) (also cf. the
    [official documenation](https://jekyllrb.com/docs/pagination/)).
* Supports category keys with spaces and other special characters.
* Complete documentation and a usage example.
* Test coverage of key features.
* Category index pages are generated based on a customizable template.

## Usage

Assign one or more categories in the YAML front matter of each page:
```yaml
categories: [Category Pages Plugin, jekyll]
```
Generate the site and the category pages are generated:
```
_site/blog/category/
├── Category%20Pages%20Plugin/
│   └── index.html
├── 好的主意/
│   └── index.html
└── jekyll/
    ├── index.html
    ├── page2.html
    └── page3.html
```
In this example there are three paginated index pages for the `jekyll`
category (apparently, there are many posts for this category), a
single index page for the `好的主意` category and another single index
page for the `Category Pages Plugin` category.

Note that the `categories` entry should always use brackets `[]` to
make it explicit that it is an array!

You can find this example in the `example` directory of the [git
repository](https://github.com/field-theory/jekyll-category-pages).

### The example project

The `example` directory contains a basic example project that
demonstrates the different use cases. In order to run Jekyll on these
examples use:
```shell
bundle install
rake example
```
The result is put in `example/_site`.

## Installation and setup

Installation is straightforward (like other plugins):
1. Add the plugin to the site's `Gemfile` and configuration file:
    ```ruby
    group :jekyll_plugins do
      gem "jekyll-category-pages"
    end
    ```
    and run `bundle install`. (Alternatively you can install the gem
    globally if you do not use bundler.)
2. Add the plugin to your site's `_config.yml`:
    ```ruby
    plugins:
      - jekyll-category-pages
    ```
3. Configure any other options you need (see below).
4. Add template for category pages (see below).
5. Set appropriate `categories` tags on each blog post YAML front
   matter.

### Configuration

The following options can be set in the Jekyll configuration file
`_config.yml`:
* `category_path`: Root directory for category index pages. Defaults
    to `blog/category` if unset.  
    In the example this places the index file for category `jekyll` at
    `example/_site/blog/category/jekyll/index.html`.
* `category_layout`: Basic category index template. Defaults to
    `category_index.html`. The layout **must** reside in the standard
    `_layouts` directory.  
    In the example the layout is in
    `example/_layouts/category_index.html`.
* `paginate`: (Maximum) number of posts on each category index
    page. This is the same for the regular (front page) pagination. If
    absent, pagination is turned off and only single index pages are
    generated.  
    In the example `paginate` is set to 2.

### Template for category pages

The template for a category index page must be placed in the site's
`_layouts` directory. The attribute `category` indicates the current
category for which the page is generated. The page title also defaults
to the current category. The other attributes are similar to the
default Jekyll pagination plugin.

If no pagination is enabled the following attributes are available:

| Attribute     | Description                               |
| ------------- | ----------------------------------------- |
| `category`    | Current page category                     |
| `posts`       | List of posts in current category         |
| `total_posts` | Total number of posts in current category |

If pagination is enabled (i.e., if setting `site.paginate` globally in
`_config.yml`) then a `paginator` attribute is available which returns
an object with the following attributes:

| Attribute            | Description                                                                      |
| -------------------- | -------------------------------------------------------------------------------- |
| `page`               | Current page number                                                              |
| `per_page`           | Number of posts per page                                                         |
| `posts`              | List of posts on the current page                                                |
| `total_posts`        | Total number of posts in current category                                        |
| `total_pages`        | Total number of pagination pages for the current category                        |
| `previous_page`      | Page number of the previous pagination page, or `nil` if no previous page exists |
| `previous_page_path` | Path of previous pagination page, or `nil` if no previous page exists            |
| `next_page`          | Page number of the next pagination page, or `nil` if no subsequent page exists   |
| `next_page_path`     | Path of next pagination page, or `nil` if no subsequent page exists              |

An example can be found in `example/_layouts/category_index.html`
which demonstrates the various attributes and their use.

### Category listing

A category overview with a full listing of all categories can be
created as follows:
```html
<ul>
{% for category in site.categories %}
<li><a href="{{ site.url }}/blog/category/{{ category | first | url_encode }}/index.html">{{ category | first }}</a></li>
{% endfor %}
</ul>
```

Note that the category page paths are URL-encoded when generated by
this plugin. Thus, you have to use `url_encode` when linking to each
category. This saves you from problems with spaces or other special
characters in category names.

An example listing can be found in `example/index.html` which
shows a full listing of categories with corresponding links.

## Development

This project has a set of unit tests that generate category index
pages for a couple of posts.

TODO

## License

The gem is available as open source under the terms of the [MIT
License](https://github.com/field-theory/jekyll-category-pages/blob/master/LICENSE).
