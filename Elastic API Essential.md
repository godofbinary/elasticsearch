# API essentielles elasticsearch à connaitre

Ma fiche personnelle qui liste les api essentielles à connaitre pour gérer son cluster et ses index elasticsearch.  

## 1. Cluster API
[Cluster api](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster.html) expose des endpoints pour obtenir des informations générales sur le cluster elasticsearch.

 ` GET /_cluster/settings?include_defaults=true? `. 
 `GET /_cluster/settings?include_defaults=true&flat_settings=true`.
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
  
