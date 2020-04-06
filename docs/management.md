# Management Commands

Create the indices and their mapping in Elasticsearch:

```bash
./manage.py search_index --create [--models [app[.model] app[.model] ...]]
```

Populate the Elasticsearch mappings with the django models data (index need to be existing):

```bash
./manage.py search_index --populate [--models [app[.model] app[.model] ...]] [--parallel]
```

Recreate and repopulate the indices:

```bash
./manage.py search_index --rebuild [-f] [--models [app[.model] app[.model] ...]] [--parallel]
```

Delete all indices in Elasticsearch or only the indices associate with a model (`--models`):
```bash
./manage.py search_index --delete [-f] [--models [app[.model] app[.model] ...]]
```



