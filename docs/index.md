# Django-Elasticsearch

Django-Elasticsearch is a package that allows indexing of django models in elasticsearch.
It utilizes [elasticsearch-dsl](https://github.com/elastic/elasticsearch-dsl-py)
so you can use all the features developed by the elasticsearch-dsl-py team.

## Getting started

First, add `django_elasticsearch` to your project:

```bash
pip install django_elasticsearch
# or, you know... 
poetry add django_elasticsearch
```

Next, configure your `settings.py`:

```python
# add it to your INSTALLED_APPS
INSTALLED_APPS = [
    "...",
    "django_elasticsearch",
]

# and define some settings:
ELASTICSEARCH_DSL={
    'default': {
        'hosts': 'localhost:9200'
    },
}
```
See [settings](/settings) for all options.

## Indexing your first document

Let's use some exemplary Django models:
```python
# models.py
from django.db import models

class Musician(models.Model):
    name = models.CharField(max_length=50)
    instrument = models.CharField(max_length=100)
    gender = models.IntegerField(choices=[
        (0, "Female"),
        (1, "Male"),
        (2, "Other"),
    ])


class Album(models.Model):
    artist = models.ForeignKey(Musician, on_delete=models.CASCADE)
    name = models.CharField(max_length=100)
    release_date = models.DateField()
    num_stars = models.IntegerField()
```

Then, within the same django app, create a file `documents.py`:

```python
# documents.py
from django_elasticsearch import Document
from django_elasticsearch.registries import registry
from .models import Musician, Album


@registry.register_document
class MusicianDocument(Document):
    class Index:
        name = 'musicians'

    class Django:
        model = Musician


@registry.register_document
class AlbumDocument(Document):
    class Index:
        name = 'albums'

    class Django:
        model = Album
    
    def prepare_artist(self, instance: Album):
        artist = instance.artist
        if not artist:
            return None
        return {
            "id": artist.id,
            "name": artist.name,
            "instrument": artist.instrument,
            "gender": artist.gender
        }
```

Now as you can see, for `Album` we declared an extra method `prepare_artist()`.<br>
At the moment this is necessary to tell specify which fields of this ForeignKey you want mapped.

All the other fields should be mapped automagically and for the Musician's `gender` attribute two fields will be created:
`gender` and `gender_display` that will have the integer and the string value respectively.

For further information about specifying which attributes to index, see [Documents](/documents)


## Generate Index

If you already have existing data in your models, you can trigger an index rebuild:
```bash
./manage.py search_index --rebuild
```

Whenever you save a model now,
```python
django = Musician(
    name="Django Reinhardt",
    instrument="Triangle",
    gender=1,
)
django.save()
```

`.save()` will emit a signal that creates or updates the document in the index accordingly.


## Search


Search is pretty much vanilla `elasticsearch-dsl` [Search](https://elasticsearch-dsl.readthedocs.io/en/latest/search_dsl.html) -
to instanciate a search instance you just go from the relevant `Document`, i.e.
```python
s = MusicianDocument.search().filter("term", instrument="Triangle")

for hit in s:
    print(f"Musician {hit.name} plays {hit.instrument}")
```


The previous example returns a result specific to 
[elasticsearch_dsl](http://elasticsearch-dsl.readthedocs.io/en/latest/search_dsl.html#response),
but it is also possible to convert the elastisearch result into a real Django queryset.
**Be aware** that this costs a sql request to retrieve the model instances
with the ids returned by the elastisearch query.

```python
s = MusicianDocument.search().filter("term", instrument="Triangle")[:30]
qs = s.to_queryset()
# qs is just a django queryset and it is called with order_by to keep
# the same order as the elasticsearch result.
for hit in qs:  # type: Musician
    print(hit.name)
```
