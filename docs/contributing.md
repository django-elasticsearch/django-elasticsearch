# Contributing

Any help is highly appreciated: https://github.com/django-elasticsearch/django-elasticsearch


## Testing

Please make sure the tests pass, like so:
 
```bash
poetry install
poetry run python runtests.py
# and for integration testing with a running Elasticsearch server
poetry run python runtests.py --elasticsearch <localhost:9200>
```
