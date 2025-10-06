
# 3. Proposition d’une architecture locale

## Introduction

L’objectif d’une **architecture locale** est de permettre aux développeurs de lancer et tester 
l’intégralité du système sur leur machine sans dépendre d’une infrastructure cloud complexe.  
Cette version locale doit conserver la même structure logique que la version distribuée, 
mais en simplifiant la configuration, le déploiement et les interconnexions.


## 3.1 Principes de conception

L’architecture locale repose sur les principes suivants :

- **Simplicité** : un seul fichier de configuration permettant de démarrer tous les services nécessaires.
- **Reproductibilité** : même environnement pour tous les développeurs.
- **Isolation** : aucun besoin de connexion externe (tout tourne en local).
- **Compatibilité** : même topologie logique que la version cloud pour faciliter les tests de bout en bout.


## 3.2 Composants de l’architecture locale

| Composant | Description | Technologie |
|------------|--------------|-------------|
| **Gateway** | Point d’entrée unique. Gère le routage, l’authentification et le caching basique. | Nginx / Traefik |
| **Orchestrator** | Service d’agrégation parallèle des résultats provenant des microservices. | FastAPI / Node.js |
| **Search Service** | Moteur de recherche local. | Elasticsearch / OpenSearch |
| **Ranking Service** | Classement des résultats. | Python / Go |
| **Personalization Service** | Recommandations et filtrage personnalisé. | FastAPI / TorchServe |
| **Index Service** | Gère la mise à jour des index. | Python / Kafka consumer |
| **Redis** | Cache rapide pour les données temporaires. | Redis 7 |
| **PostgreSQL** | Base de données relationnelle pour les utilisateurs et métadonnées. | Postgres 15 |
| **Kafka / Redpanda** | File de messages pour la communication asynchrone. | Redpanda (plus léger pour local) |
| **Observabilité** | Suivi des métriques et logs. | Prometheus + Grafana + Jaeger |
| **Stockage d’assets** | Stockage d’images ou documents. | MinIO (S3 compatible) |


## 3.3 Workflow de développement local

1. **Cloner le projet**
   - Télécharger le dépôt depuis GitHub et se placer dans le dossier principal du projet.

2. **Lancer les services**
   - Démarrer les différents services (base de données, cache, moteur de recherche, orchestrator, etc.) via une commande unique.

3. **Initialiser la base de données**
   - Exécuter un script d’initialisation pour insérer des données de test ou de configuration.

4. **Tester l’application**
   - Accéder à la Gateway via le navigateur (par exemple sur `http://localhost:8080`).
   - Vérifier le bon fonctionnement de chaque microservice.

5. **Observation et monitoring**
   - Consulter les tableaux de bord de Grafana pour visualiser les métriques.
   - Analyser les traces via Jaeger pour identifier les éventuels ralentissements.


## 3.4 Avantages de l’architecture locale

-  **Environnement de test complet et reproductible**  
-  **Simplifie l’intégration continue (CI)** grâce à une configuration unifiée  
-  **Facilite le débogage** des interactions entre services  
-  **Prépare la transition** vers l’infrastructure cloud sans modifications majeures  
-  **Permet l’expérimentation rapide** de nouvelles fonctionnalités en local  


## 3.5 Limites de la version locale

-  Performances limitées (machines locales vs cluster cloud)
-  Pas de haute disponibilité réelle
-  Pas de scaling automatique
-  Sécurité simplifiée (authentification locale ou désactivée)
-  Monitoring allégé (logs et métriques locaux uniquement)


## Conclusion

L’architecture locale permet de **simuler l’écosystème complet** du système distribué de manière simple et portable.  
Elle favorise la cohérence entre les environnements de développement et de production, 
réduit les erreurs d’intégration et améliore la productivité des équipes.

Elle constitue ainsi **une étape essentielle** avant la mise en place d’un déploiement distribué à grande échelle 
sur le cloud ou dans un cluster Kubernetes.

