# Groupe Lautier Paul et Verrier Paul-Alexis

## TP Kubernetes

### Namespaces

Notre découpage à été le suivant : le namespace devops303 contenant 2 pods devops303 et un redis. Nous avons aussi le namescpace hello qui est constitué de 3 pods hello-world. Le namespace hello a un quota pour limiter son impact sur les ressources du cluster et le namespace devops303 a aussi un quota pour limiter son utilisation des ressources.

### Devops303, Redis et Hello-world

Dans le namespace devops303, 2 pods on étés mis en place pour que le service soit redondant, et un pod Redis que l'on a lié aux pods Devops303 grâce à des variables d'environnement présentes dans le fichier ConfigMap. Ces variables d'environnement sont le port et le host. Dans le namespace hello, on a mis en place 3 pods Hello-World. Les pods Devops303 et hello-world sont respectivement joignables avec devops303.paul suivi du port et hello.paul suivi du port.

### Monitoring

Pour le monitoring, on a réalisé l'installation avec la commande `helm install prometheus prometheus-community/kube-prometheus-stack`.
Puis pour lancer Grafana, nous avons fait la commande suivante `kubectl port-forward deployment/prometheus-grafana 3000` ce qui permet de forward le service Grafana par le port 3000, on peut alors utiliser localhost:3000 sur un navigateur et obtenir une page de connexion pour Grafana.
Sur cette page les identifiants sont admin et prom-operator, ce qui permet d'obtenir un dashbord affichant les métriques récupérées par Prometheus.

Nous avons ensuite relié les alertes Prometheus à un salon textuel Discord grâce à un Webhook. Ce qui nous permet de facilement visualiser s'il y a une alerte liée au cluster.

### RBAC

Nous avons créé 3 rôles qui sont admin-cluster, dev et client. Admin-cluster dispose des privilèges root sur le cluster. Dev n'a que les droits de suppression en moins par rapport à admin-cluster et client n'a que les droits de lecture.

Un label criticity:danger a également été déployé sur le cluster Devops303 afin d'éviter les modifications des ressources du namespace par une personne non autorisée.

### KubeDB

Nous avons installé KubeDB avec la commande suivante `helm install kubedb appscode/kubedb`, mais nous n’avons pas réussi à le configurer de manière satisfaisante.
