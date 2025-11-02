# TP_910_k8s - F1 Leaderboard Application

Application de classement F1 avec un backend Java Spring Boot et un frontend React, déployée sur Kubernetes avec Minikube.

## Liens des repositories

- [Repository Front-end](https://github.com/mathisfeltrin/info910-f1-leaderboard-frontend)
- [Repository Back-end](https://github.com/AyRron/TP-Back_INFO-910)

## Architecture

- **Backend**: API REST Java Spring Boot (port 8080)
- **Frontend**: Application React (port 80)
- **Kubernetes**: Déploiements, Services et Ingress NGINX

## Prérequis

- [Minikube](https://minikube.sigs.k8s.io/docs/start/) installé
- [kubectl](https://kubernetes.io/docs/tasks/tools/) installé
- Docker ou un hyperviseur (VirtualBox, HyperKit, etc.)

## Installation et lancement avec Minikube

### 1. Démarrer Minikube

```bash
# Démarrer Minikube avec le driver approprié
minikube start

# Vérifier que Minikube est bien démarré
minikube status
```

### 2. Activer l'addon Ingress NGINX (optionnel)

Si vous souhaitez utiliser les Ingress au lieu du port-forward :

```bash
# Activer l'addon ingress nginx
minikube addons enable ingress

# Vérifier que l'addon est activé
minikube addons list | grep ingress
```

### 3. Déployer les applications

```bash
# Appliquer tous les fichiers Kubernetes
kubectl apply -f k8s/

# Vérifier les déploiements
kubectl get deployments
kubectl get pods
kubectl get services
kubectl get ingress
```

### 4. Accéder à l'application avec port-forward

Utiliser le port-forwarding pour accéder aux services :

```bash
# Port-forward pour le frontend (dans un terminal)
kubectl port-forward -n default service/frontend 3000:80

# Port-forward pour le backend (dans un autre terminal)
kubectl port-forward -n default service/backend 8080:8080
```

Accès à l'application :

- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:8080/api/drivers/health

**Note**: Les commandes port-forward doivent rester actives dans leurs terminaux respectifs. Utilisez `Ctrl+C` pour les arrêter.

### 5. Commandes utiles

```bash
# Voir les logs d'un pod
kubectl logs <pod-name>

# Suivre les logs en temps réel
kubectl logs -f <pod-name>

# Décrire une ressource
kubectl describe pod <pod-name>
kubectl describe service <service-name>
kubectl describe ingress <ingress-name>

# Redémarrer un déploiement
kubectl rollout restart deployment/backend
kubectl rollout restart deployment/frontend

# Supprimer toutes les ressources
kubectl delete -f k8s/

# Arrêter Minikube
minikube stop

# Supprimer le cluster Minikube
minikube delete
```

## Alternative : Docker Compose

Pour tester rapidement l'application sans Kubernetes :

```bash
docker compose up -d
```

- **Frontend**: http://localhost
- **Backend**: http://localhost:8080/api/drivers

## Dépannage

### Les pods ne démarrent pas

```bash
# Vérifier les événements
kubectl get events --sort-by=.metadata.creationTimestamp

# Vérifier les logs
kubectl logs <pod-name>
```

### L'ingress ne fonctionne pas

```bash
# Vérifier que l'addon ingress est activé
minikube addons list | grep ingress

# Vérifier les logs du contrôleur ingress
kubectl logs -n ingress-nginx <ingress-controller-pod>

# Vérifier la configuration ingress
kubectl describe ingress backend
kubectl describe ingress frontend
```

## Structure des fichiers Kubernetes

```
k8s/
├── back-dep.yaml       # Déploiement backend
├── back-svc.yaml       # Service backend
├── back-ingress.yaml   # Ingress backend
├── front-dep.yaml      # Déploiement frontend
├── front-svc.yaml      # Service frontend
└── front-ingress.yaml  # Ingress frontend
```
