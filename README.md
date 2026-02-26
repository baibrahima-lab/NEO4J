# 🕸️ Neo4j Yelp Graph Analytics - DU SDA 2025/2026

**Auteur** : Ibrahima BA  
**Dataset** : [Yelp Open Dataset](https://business.yelp.com/data/resources/open-dataset/)  
**Volume** : 200K+ Businesses, 2M+ Users, 8M+ Reviews

---

## 🎯 Objectifs du Projet

Modéliser et analyser le dataset Yelp dans Neo4j pour construire un système de recommandation hybride combinant :
- **Content-Based Filtering** (similarité catégories)
- **Collaborative Filtering** (interactions user-business)
- **Social Recommendation** (graphe d'amitiés)
- **Location-Based** (proximité géographique)

---

## 🏗️ Architecture du Graphe

### Nœuds
| Label | Propriétés Clés | Description |
|-------|-----------------|-------------|
| `Business` | `id`, `name`, `stars`, `review_count`, `location` (point), `city`, `state` | Établissements Yelp |
| `User` | `id`, `name`, `review_count`, `average_stars`, `embedding`, `pagerank`, `community` | Utilisateurs avec métriques GDS |
| `Review` | `id`, `stars`, `text`, `date`, `sentiment_score` | Avis avec analyse de sentiment |
| `Category` | `name` | Catégories métier (Restaurant, Bar, etc.) |
| `Tip` | `text`, `date`, `compliment_count` | Conseils courts |

### Relations
| Type | Connecte | Propriétés | Usage |
|------|----------|------------|-------|
| `INTERACTED_WITH` | User → Business | `weight`, `avg_stars`, `review_count`, `last_review_date` | Interaction pondérée pour ML |
| `FRIENDS` | User ↔ User | - | Réseau social |
| `WROTE` | User → Review | `date` | Auteur de l'avis |
| `REVIEWS` | Review → Business | `stars` | Cible de l'avis |
| `IN_CATEGORY` | Business → Category | - | Classification |
| `SIMILAR_CONTENT` | Business ↔ Business | `score`, `algorithm` | Similarité Jaccard |
| `POTENTIAL_FRIEND` | User → User | `score`, `predicted`, `common_neighbors` | Prédiction liens |

---

## 🚀 Pipeline d'Import

### 1. Prérequis
```bash
# Plugins nécessaires dans neo4j.conf
dbms.security.procedures.unrestricted=apoc.*,gds.*
dbms.memory.heap.max_size=4G


Fichiers JSON Yelp requis

yelp_academic_dataset_business.json
yelp_academic_dataset_user.json
yelp_academic_dataset_review.json
yelp_academic_dataset_tip.json (optionnel)
yelp_academic_dataset_checkin.json (optionnel)
3. Exécution

Exécuter les requêtes Cypher par étapes (0 → 7) dans Neo4j Browser ou via cypher-shell.

Algorithmes GDS Implémentés


| Algorithme                 | Graphe            | Objectif                | Output                       |
| -------------------------- | ----------------- | ----------------------- | ---------------------------- |
| **Jaccard Similarity**     | Business-Category | Similarité contenu      | `SIMILAR_CONTENT`            |
| **FastRP**                 | User-Business     | Embeddings latents      | `embedding` property         |
| **PageRank**               | Social            | Influence utilisateurs  | `pagerank`, `influence_tier` |
| **Betweenness**            | Social            | Ponts entre communautés | `betweenness`                |
| **Louvain**                | Social            | Détection communautés   | `community`                  |
| **Adamic-Adar**            | Social            | Prédiction amitiés      | `POTENTIAL_FRIEND`           |
| **Node2Vec**               | Complet           | Embeddings contextuels  | `node2vec_embedding`         |
| **Clustering Coefficient** | Social            | Cohésion locale         | `clustering_coefficient`     |


🎓 Apprentissages

Modélisation graphe vs relationnel pour recommandation
Algorithmes de graphe pour systèmes de recommandation
Optimisation import massif dans Neo4j
Hybridation des approches (content + collaborative + social + geo)



