# Deepseek — Étude d’Architecture Gateway (C4 Model)

Ce document présente une étude complète de l’architecture **Gateway** de Deepseek, réalisée avec la méthode **C4 (Context, Container, Component, Code)** et enrichie par des diagrammes **PlantUML**.

---

## Sommaire
1. [Introduction](#-introduction)  
2. [Analyse de l’Architecture Actuelle](#-analyse-de-larchitecture-actuelle)  
3. [Améliorations Proposées](#-améliorations-proposées)  
4. [Modélisation C4](#-modélisation-c4)  
   - [Niveau 1 — Context](#niveau-1--context)  
   - [Niveau 2 — Container](#niveau-2--container)  
   - [Niveau 3 — Component](#niveau-3--component)  
   - [Niveau 4 — Code](#niveau-4--code)  
5. [Architecture Microservices Parallèle](#-architecture-microservices-parallèle)  
6. [Conclusion](#-conclusion)  

---

##  Introduction

Deepseek est une plateforme reposant sur une architecture orientée microservices.  
Le **Gateway** est le point d’entrée du système et remplit plusieurs rôles critiques :  
- Authentification et autorisation  
- Routage et agrégation  
- Caching  
- Contrôle de débit (rate limiting)  

L’objectif de cette étude :  
1. **Analyser l’architecture actuelle**.  
2. **Proposer une amélioration**.  
3. **Modéliser une architecture microservices parallèle**.  

---

## Analyse de l’Architecture Actuelle

### Rôles du Gateway
- Validation JWT via un fournisseur d’identité externe.  
- Routage vers les services internes (Search, Ranking, Personalization, User, Index).  
- Application des règles de sécurité et monitoring.  

### Limites
- Latence élevée si appels séquentiels.  
- Logique métier trop couplée au Gateway.  
- Manque de résilience (pas de circuit-breaker).  
- Cache limité ou absent côté Gateway.  

---

## Améliorations Proposées

- **Gateway mince** (Thin Gateway) : seulement Auth, cache rapide et routage.  
- **Orchestrator Service** : prend en charge l’agrégation et l’exécution parallèle.  
- **Cache distribué** (Redis + Edge/CDN).  
- **Résilience** : retries, circuit breakers, timeouts.  
- **Scalabilité** : Kubernetes autoscaling, partitionnement.  
- **Observabilité** : traces distribuées (OpenTelemetry).  

---

##  Modélisation C4

### Niveau 1 — Context
![Screenshot_20250929_142420_Chrome](https://github.com/user-attachments/assets/6543609f-e19a-433e-9fc8-090440458893)

### Niveau 2 — Container
![Screenshot_20250929_143304_Chrome](https://github.com/user-attachments/assets/1ccebbe3-e896-4c29-9ab9-b7a5f8ad64de)


### Niveau 3 — Component
![Screenshot_20250929_143708_Chrome](https://github.com/user-attachments/assets/20ff9e4a-c198-41fd-8e9d-11699ee0da85)

### Niveau 4 — Code
![Screenshot_20250929_143857_Chrome](https://github.com/user-attachments/assets/7de4ef78-55fc-46ae-ab67-5f487e9572b5)


## Architecture Microservices Parallèle

L’architecture proposée repose sur une exécution **parallèle** des microservices derrière un Gateway allégé.  
Chaque service est indépendant, scalable et communique via des canaux asynchrones (Kafka, Redis, REST/gRPC).

### Caractéristiques
- **Gateway mince** : se limite à l’authentification, au cache rapide et au routage.
- **Orchestrator** : responsable de la coordination, de l’exécution parallèle et de l’agrégation des résultats.
- **Services métiers** :
  - **Search Service** : interagit avec Elasticsearch pour fournir les résultats.
  - **Personalization Service** : génère un contenu adapté à l’utilisateur.
  - **Ranking Service** : trie les résultats selon les règles métier.
  - **Index Service** : gère la mise à jour des index via Kafka.
- **Cache distribué** : Redis (near-gateway) + CDN/Edge pour réduire la latence.
- **Résilience** : mécanismes de retry, timeout, circuit-breaker.
- **Scalabilité** : autoscaling Kubernetes, partitionnement des données.
- **Observabilité** : logs centralisés, métriques Prometheus, traces distribuées OpenTelemetry.

## Conclusion

L’architecture **Gateway de DeepSeek** fournit une base robuste pour la gestion centralisée des flux et des interactions entre les services.  
Cependant, en intégrant une approche **microservices parallèles**, il est possible d’améliorer significativement :

- ✅ **La scalabilité** : chaque service peut évoluer indépendamment selon la charge.  
- ✅ **La résilience** : un échec localisé n’impacte pas tout le système.  
- ✅ **La performance** : traitement parallèle optimisé, réduction des goulots d’étranglement.  
- ✅ **La flexibilité** : ajout ou retrait de services sans perturbation majeure.  

En conclusion, le passage vers une architecture distribuée et orientée **4C (Context, Container, Component, Code)** avec microservices parallèles permet de préparer DeepSeek à des usages plus intensifs, à une croissance future et à une meilleure fiabilité dans le temps.
