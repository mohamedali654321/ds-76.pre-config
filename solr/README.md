# Dspace 7 solr

## Build dspace 7 solr docker image:

```
docker build -t <image name> .
```

EX:

```
docker build -t dspace-v7.2-solr .
```

## Run dspace 7 UI development docker container:

```
  docker run -d -p <dspace 7 solr port>:8983 -v <dspace files path on the server>:/opt/solr/server/solr/configsets --name <container name> <dspace solr image name>
```

- NOTE: <dspace 7 UI port> must be "synced" with the 'dspace.solr.url' setting in your backend's local.cfg.

EX:

```
  docker run -d -p 9083:8983 -v /home/ubuntu/attia-testing/dspace7-last-update/7.2/solr/dspace_solr_core:/dspace_solr_core --name dspace-v7.2-solr dspace-v7.2-solr
```
