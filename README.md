# r3edge-engine-iac

## Introduction

Ce projet implémente une infrastructure cloud multi-fournisseurs, entièrement gérée via **Pulumi**, avec des services déployés sur **Oracle Cloud**, **Google Cloud**, et **AWS**. Il s'agit d'un environnement hybride qui combine des instances de machines virtuelles, des services PaaS (PostgreSQL, Kafka, Prometheus, Grafana), et un cluster Kubernetes léger pour orchestrer plusieurs microservices.

L'objectif de cette architecture est de fournir une solution modulaire, scalable et résiliente tout en optimisant les coûts via des offres cloud gratuites ou à faible coût.

### Composants principaux :
- **Cluster Kubernetes léger (k3s/MicroK8s)** : Déployé sur des VMs Oracle gratuites pour orchestrer les microservices.
- **Services PaaS** :
  - **PostgreSQL** via **Supabase/Heroku**.
  - **Kafka** via **Confluent Cloud**.
  - **Prometheus et Grafana** via Google Cloud/AWS pour le monitoring et l'observabilité.
- **Microservices** : 4 microservices déployés sous Kubernetes, gérés via Traefik pour l'ingress.
- **Spring Cloud Config** : Gestion centralisée de la configuration des microservices.

## Arborescence du projet

L'infrastructure du projet est organisée de manière modulaire sous le répertoire `iac/` pour une gestion propre des configurations cloud, tandis que la documentation associée est regroupée sous `docs/`.

    project/
    │
    ├── iac/                         # Infrastructure as Code
    │   ├── aws/
    │   ├── gcp/
    │   ├── oracle/
    │   └── Pulumi.yaml
    │
    ├── docs/                        # Documentation
    │   ├── architecture.md          # Schémas et explication de l'architecture globale
    │   ├── deployment_guide.md      # Instructions pour le déploiement
    │   └── monitoring_guide.md      # Guide sur la configuration de Prometheus et Grafana
    │
    └── README.md                    # Présentation globale du projet

## Objectifs et cas d'usage

Cette architecture vise à fournir une solution cloud hybride avec les objectifs suivants :
1. **Optimisation des coûts** : Utilisation des VMs gratuites d'Oracle Cloud et des services PaaS gratuits pour des services critiques tels que PostgreSQL et Kafka.
2. **Modularité et scalabilité** : L'infrastructure est déployée via **Pulumi** en plusieurs couches (réseau, infrastructure, orchestration), permettant un déploiement flexible sur différents fournisseurs cloud.
3. **Observabilité** : Monitoring complet via **Prometheus** et **Grafana**, déployés en tant que services PaaS, pour surveiller l'état des microservices et du cluster Kubernetes.

## Installation et configuration

### Prérequis
- **Pulumi** doit être installé localement : [Installation guide](https://www.pulumi.com/docs/get-started/install/)
- Accès aux comptes cloud (**Oracle**, **Google Cloud**, **AWS**) avec les configurations nécessaires.

### Étapes de déploiement

1. **Initialisation des environnements Pulumi**
    
        cd iac/
        pulumi stack init <stack-name>   # ex: dev-us-west
    
2. **Déploiement du cluster Kubernetes sur Oracle Cloud**
    
        cd iac/oracle
        pulumi up
    
3. **Déploiement des services PaaS sur Google Cloud et AWS**
    
        cd iac/gcp
        pulumi up
        cd iac/aws
        pulumi up
    
4. **Monitoring avec Prometheus et Grafana**
   - Consultez la documentation dans `docs/monitoring_guide.md` pour la configuration complète de l'intégration avec Prometheus et Grafana.

## Structure modulaire de l'infrastructure

### Réseau (Network)
- Déploiement des VPCs, subnets, et des ressources réseaux pour chaque cloud provider.

### Infrastructure
- Machines virtuelles sur Oracle Cloud pour exécuter Kubernetes et les microservices.
- Kafka et PostgreSQL sont externalisés via des services PaaS.

### Orchestration
- **Kubernetes (k3s)** pour orchestrer les microservices et gérer le scaling automatique.
- **Traefik** comme Ingress Controller pour le routage HTTP.

## Contribution

Toute contribution à ce projet est la bienvenue. Les améliorations autour de l'intégration des services PaaS, l'optimisation des coûts, et l'automatisation des workflows DevOps sont particulièrement appréciées.

---

## Liens utiles
- [Pulumi Documentation](https://www.pulumi.com/docs/)
- [Oracle Cloud Free Tier](https://www.oracle.com/cloud/free/)
- [Grafana Cloud Free](https://grafana.com/products/cloud/)
- [Confluent Cloud Free Kafka](https://www.confluent.io/confluent-cloud/tryfree/)

