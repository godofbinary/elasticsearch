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

`DELETE /<index>`
[Supprimer un index](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-delete-index.html).

`GET /<index>`
[Renvoie les informations sur un index](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-get-index.html).

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
