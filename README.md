![python version](https://img.shields.io/badge/python-3.6+-blue.svg)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
![Travis (.com)](https://img.shields.io/travis/com/django-elasticsearch/django-elasticsearch)
![PyPI - Django Version](https://img.shields.io/pypi/djversions/django-elasticsearch-ng)
# Django Elasticsearch

Django Elasticsearch is a package that allows indexing of django models in elasticsearch.
It is built as a thin wrapper around [elasticsearch-dsl](https://github.com/elastic/elasticsearch-dsl-py)
so you can use all the features developed by the elasticsearch-dsl-py team.

You can view the full documentation at https://django-elasticsearch-dsl.readthedocs.io

## Features

- Based on [elasticsearch-dsl](https://github.com/elastic/elasticsearch-dsl-py) so you can make queries with the [Search](http://elasticsearch-dsl.readthedocs.io/en/stable/search_dsl.html) class.
- Django signal receivers on save and delete for keeping Elasticsearch in sync.
- Management commands for creating, deleting, rebuilding and populating indices.
- Elasticsearch auto mapping from django models fields.
- Complex field type support (ObjectField, NestedField).
- Index fast using `parallel` indexing.

### Elasticsearch Compatibility
The library is currently **only** compatible with Elasticsearch 7
