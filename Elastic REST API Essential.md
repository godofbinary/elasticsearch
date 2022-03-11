# API essentielles elasticsearch à connaitre

Ma fiche personnelle qui liste les api essentielles à connaitre pour gérer son cluster et ses index elasticsearch.  

## 1. Cluster API
[Cluster api](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster.html) expose des endpoints pour obtenir des informations générales sur le cluster elasticsearch.

 ` GET /_cluster/settings?include_defaults=true? `
 `GET /_cluster/settings?include_defaults=true&flat_settings=true`
 [Renvoie la configuration générale du cluster au format json éclaté/plat.](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-get-settings.html).
 
 `GET /_cluster/health/`
 `GET /_cluster/health?level=indices`
 [Renvoie l'état de santé du cluster (par défaut), ou des index, ou des shards](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-health.html).
 
 `GET /_cluster/stats`
 [Renvoie l'état global du cluster ainsi que des statistiques sur l'utilisation du cluster](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-stats.html).
 
 `GET /_nodes`
 [Renvoie des informations sur un plusieurs ou tous les noeuds du cluster](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-nodes-info.html).
 
 `GET /_nodes/stats`
 [Renvoie des statistiques d'usages sur les noeuds du cluster](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-nodes-stats.html).
 
 `GET /_cluster/pending_tasks`
 [Renvoie toutes les tâches en attente d'exécution sur le cluster](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-pending.html).
 
 `GET /_tasks`
 [Renvoie toutes les tâches encours d'exécution sur un ou plusieurs noeuds du cluster](https://www.elastic.co/guide/en/elasticsearch/reference/current/tasks.html).
  
## 2. CAT (Compact and aligned text) API
[CAT API](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat.html#cat) renvoie des réponses humainement lisible pour toutes les requêtes qu'elle reçoit.  
Les paramètres commun pour toutes les requête de cette API sont:
>v=true pour une réponse affichant les entêtes des valeurs retournées.Ex: `GET _cat/master?v=true`.
>help pour connaitre l'ensemble des colonnes disponible pour une requête données. Ex: `GET _cat/master?help`.

`GET _cat/aliases?v=true`
[Renvoie l'ensemble des alias d'index du cluster](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat-alias.html).

`GET /_cat/count?v=true`
[Renvoie le total des documents enrégistrés dans un index ou sur le cluster](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat-count.html).

`GET /_cat/health?v=true`
[Renvoie des informations sur l'état de santé du cluster](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat-health.html).

`GET /_cat/indices?v=true`
[Renvoie des informations basiques sur les index du cluster](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat-indices.html).

`GET /_cat/nodes?v=true`
[Renvoie des informations métriques sur les différents noeuds du culster](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat-nodes.html)

`GET /_cat/pending_tasks?v=true`
[Renvoie des informations sur les tâches en attente d'exécution sur le cluster](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat-pending-tasks.html).

`GET /_cat/tasks`
[Renvoie des informations sur les tâches en cours d'exécution sur le cluster](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat-tasks.html).

`GET /_cat/shards?v=true`
[Renvoie des informations sur les différents shards du cluster](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat-shards.html).

## 3. Index API
[Index API](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices.html) permet d'éffectuer des opérations sur les index à  savoir:
* création et configuration des index
* alias
* mappings
* templates

### Gestion des index
`PUT /<index>`
[Créer un nouvel index](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html). On peut passer en paramètres la configuration requise pour la création de l'index: ***aliases***, ***mappings***, ***settings***
Pour une documentation complète sur les index, consulter cette [page](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules.html).

`PUT /my-index-000001`
```json
{
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 2
  },
  "mappings": {
    "properties": {
      "field1": { "type": "text" }
    }
  },
  "aliases": {
    "alias_1": {},
    "alias_2": {
      "routing": "shard-1",
      "is_write_index": true
    }
  }
}
```

`HEAD <target>`
[Verifier si un index existe ou non](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-exists.html).
`DELETE /<index>`
[Supprimer un index](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-delete-index.html).

`GET /<index>`
[Renvoie les informations sur un index](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-get-index.html).

`PUT /<target>/_settings`
```json
{
  "index" : {
    "number_of_replicas" : 2
  }
}
```
[Modifier la configuration d'un index](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-update-settings.html).

``GET /<target>/_settings``
[Récuperer la configuration d'un index](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-get-settings.html).

`GET /<target>/_stats`
[Récupérer les stats d'un index](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-stats.html).

`POST /_refresh`
`POST <target>/_refresh`
[Raffraichir un ou plusieurs index](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-refresh.html).

### Gestion du mapping

`PUT /<index>/_mapping`
[Modifier le mapping d'un index existant](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-put-mapping.html). Prends en paramètre la configuration du mapping.

`PUT /<index>/_mapping`
```jsonn
{
  "properties": {
    "comment":  { "type": "text"}
  }
}
```

`GET /<index>/_mapping`
[Renvoie le mapping d'un index](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-get-mapping.html).

`GET /<index>/_mapping/field/<field>`
[Renvoie le mapping d'un ou plusieurs champs d'un index](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-get-field-mapping.html).

### Gestion des alias
Cett API permet de gérer les [alias](https://www.elastic.co/guide/en/elasticsearch/reference/current/aliases.html) d'un index. L'utilisation de cette API se fait comme décrit [ici](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-aliases.html).  
`POST _aliases`
```json
{
  "actions": [
    {
      "add": {
        "index": "my-index",
        "alias": "my-alias",
        "is_write_index": true
      }
    }
  ]
}
```
> Le code précédent permet de realiser une action d'ajout d'alias sur un index.  
 
`POST _aliases`
```json
{
  "actions": [
    {
      "remove": {
        "index": "logs-nginx.access-prod",
        "alias": "logs"
      }
    }
  ]
}
```
> Le code précédent permet de realiser une action de suppression d'alias sur un index.  

`POST _aliases`
```json
{
  "actions": [
    {
      "remove": {
        "index": "logs-nginx.access-prod",
        "alias": "logs"
      }
    },
    {
      "add": {
        "index": "logs-my_app-default",
        "alias": "logs"
      }
    }
  ]
}
```
> Le code précédent permet de realiser 2 actions (ajout et suppression [swap]) d'alias sur deux index. En pratique cela consite à retirer l'alias d'un index pour l'assigner à un autre index et le tout sans downtime. 

`GET _alias/<alias>`
`GET _alias`
`GET <target>/_alias/<alias>`
[Récuperer les informations sur un ou tous les alias](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-get-alias.html).

`HEAD _alias/<alias>`
[Vérifier l'existance d'un alias](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-alias-exists.html).

`DELETE <target>/_alias/<alias>`
`DELETE <target>/_aliases/<alias>`
[Supprimer un ou plusieurs alias](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-delete-alias.html).

## 4. Document API
[Document API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs.html) permet d'effectuer les classiques opérations CRUD sur un index. Ces opérations peuvent s'effectuer sur un document unique ou sur un ensemble de documents (bulk).

##### [Indexer un document](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html)
`POST /<target>/_doc/<_id>`
Ajouter un document json (passé en paramètre) en spécifiant son ID. Si un document avec cet ID existe déjà, il sera remplacé.

`POST /<target>/_doc/`
Ajouter un document json (passé en paramètre) sans spécifier son ID. un ID sera généré automatiquement par elasticsearch.

`POST /<target>/_create/<_id>`
Ajouter un document json (passé en paramètre) en spécifiant un ID. Si un document avec cet ID existe déjà, la requête échouera.

##### [Récuperer un document json](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-get.html)
`GET <index>/_doc/<_id>`
Récuperer un document et ses métadonnées, via son ID.

`GET <index>/_source/<_id>`
Récuperer un document sans ses métadonnées, via son ID.

##### [Supprimer un document](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-delete.html)
`DELETE /<index>/_doc/<_id>`
Supprimer le document dont l'ID est passé en paramètre.

##### [Mettre à jour un document](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-update.html)
`POST /<index>/_update/<_id>`
```json
{
  "doc": {
    "comment": "This is a modified comment of document 4"
  }
}
```
Mettre à jour document en lui passant en paramètre son nouveau contenu des champs.
La mise à jour peut se faire aussi via un script painless comme suit:
``POST /<index>/_update/<_id>``
```json
{
  "script" : {
    "source": "ctx._source.comment = params.comment",
    "lang": "painless",
    "params" : {
      "comment" : "This is a comment of document 4"
    }
  }
}
```
##### [Mettre à jour/Supprimer un document qui match une requête](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-update-by-query.html)
Quand on a besoin de mettre à jour plusieurs document qui respectent des critères, on utilise l'API update_by_query, auqel on passe en paramètre la requête elasticsearch qui de selectionner ces documents.
`POST /<target>/_update_by_query`
```json
{
  "query": { 
    "term": {
      "user.id": "kimchy"
    }
  }
}
```
Son équivalent pour supprimer les documents est [delete_by_query](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-delete-by-query.html).
`POST /<target>/_delete_by_query`
```json
{
  "query": {
    "match": {
      "user.id": "elkbee"
    }
  }
}
```

## 5. Bulk API
[Bulk API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-bulk.html) permet de réaliser puliseurs opérations CRUD en une seule requête.


## 6. Reindex API
[Reindex API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html) permet de copier des documents directement d'un index source vers un index de destination.
