# Settings

## ELASTICSEARCH_DSL
Default: `None`

`ELASTICSEARCH_DSL` is passed to [`elasticsearch-dsl-py.connections.configure`](http://elasticsearch-dsl.readthedocs.io/en/stable/configuration.html#multiple-clusters).<br>
A good starting point is:
```python
ELASTICSEARCH_DSL = {"default": {"hosts": "localhost:9200"}}
``` 


## ELASTICSEARCH_DSL_AUTOSYNC

Default: ``True``

Set to ``False`` to globally disable auto-syncing.

## ELASTICSEARCH_DSL_INDEX_SETTINGS

Default: ``{}``

These settings will be passed through to the ElasticSearch index. Common options are 
`number_of_replicas` and `number_of_shards`.<br>
See [elastic.co](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules.html) for more information.



## ELASTICSEARCH_DSL_AUTO_REFRESH

Default: ``True``

Set to ``False`` not force an [index refresh](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-refresh.html) with every save.

## ELASTICSEARCH_DSL_SIGNAL_PROCESSOR

This (optional) setting controls what SignalProcessor class is used to handle
Django's signals and keep the search index up-to-date.

An example:

```python
ELASTICSEARCH_DSL_SIGNAL_PROCESSOR = 'django_elasticsearch.signals.RealTimeSignalProcessor'
```

Defaults to ``django_elasticsearch.signals.RealTimeSignalProcessor``.

You could, for instance, make a ``CelerySignalProcessor`` which would add
update jobs to the queue to for delayed processing.

## ELASTICSEARCH_DSL_PARALLEL

Default: ``False``

Run indexing (populate and rebuild) in parallel using ES' parallel_bulk() method.
Note that some databases (e.g. sqlite) do not play well with this option.
