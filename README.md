# Sujet de TP : Introduction à Redis, une Base NoSQL Clé/Valeur

## Objectif
Apprendre les bases de l'utilisation de Redis via sa CLI (Command Line Interface) : installation, manipulation des clés/valeurs, exploration des structures de données, et compréhension des cas d'usage pratiques.

---

## Étape 1 : Installation de Redis avec Docker
**But :** Configurer Redis rapidement avec Docker pour éviter les complications d'installation.

1. Ouvrez votre terminal.
2. Téléchargez l'image officielle Redis :
   ```bash
   docker pull redis
   ```
3. Lancez un conteneur Redis :
   ```bash
   docker run --name redis-tp -d -p 6379:6379 redis
   ```
4. Vérifiez que le conteneur est actif :
   ```bash
   docker ps
   ```
5. Connectez-vous à la CLI Redis :
   ```bash
   docker exec -it redis-tp redis-cli
   ```

---

## Étape 2 : Premières Commandes avec Redis
**But :** Comprendre les commandes de base pour gérer des clés et des valeurs.

1. **Créer une clé :**
   ```bash
   SET nom "Alice"
   ```
   Résultat attendu : `OK`

2. **Lire une valeur :**
   ```bash
   GET nom
   ```
   Résultat attendu : `"Alice"`

3. **Modifier une clé :**
   ```bash
   SET nom "Bob"
   ```
   Réponse : `OK`

4. **Supprimer une clé :**
   ```bash
   DEL nom
   ```
   Résultat attendu : `1` si la clé existait.

5. **Créer une clé temporaire :**
   ```bash
   SET temporaire "Valeur" EX 120
   ```
   La clé expire après 120 secondes.

6. **Vérifier le temps restant d'une clé :**
   ```bash
   TTL temporaire
   ```

---

## Étape 3 : Manipuler les Structures de Données
**But :** Explorer des structures de données avancées et leurs cas d'utilisation.

### 1. **Listes :**
- Ajouter des éléments :
  ```bash
  RPUSH cours "Math" "Physique" "Chimie"
  ```
- Lire la liste :
  ```bash
  LRANGE cours 0 -1
  ```
  Résultat attendu : `["Math", "Physique", "Chimie"]`
- Supprimer un élément :
  ```bash
  LPOP cours
  ```

### 2. **Ensembles (Sets) :**
- Ajouter des éléments uniques :
  ```bash
  SADD utilisateurs "Alice" "Bob" "Charlie"
  ```
- Vérifier les membres :
  ```bash
  SMEMBERS utilisateurs
  ```
- Supprimer un élément :
  ```bash
  SREM utilisateurs "Charlie"
  ```

### 3. **Hachages (Hashes) :**
- Ajouter des données structurées :
  ```bash
  HSET user:1 nom "Alice" age 25 email "alice@example.com"
  ```
- Lire toutes les données :
  ```bash
  HGETALL user:1
  ```
- Incrémenter un champ :
  ```bash
  HINCRBY user:1 age 1
  ```

### 4. **Ensembles Ordonnés (Sorted Sets) :**
- Ajouter des éléments avec un score :
  ```bash
  ZADD scores 100 "Alice" 200 "Bob"
  ```
- Récupérer les éléments par score croissant :
  ```bash
  ZRANGE scores 0 -1
  ```
- Récupérer les éléments par score décroissant :
  ```bash
  ZREVRANGE scores 0 -1
  ```

### 5. **HyperLogLog :**
- Ajouter des éléments uniques :
  ```bash
  PFADD visiteurs "User1" "User2" "User3"
  ```
- Compter les éléments uniques estimés :
  ```bash
  PFCOUNT visiteurs
  ```

---

## Étape 4 : Cas d'Utilisation Pratique

1. **Système de Caching :** Utilisez Redis pour stocker temporairement des résultats de calculs ou de requêtes.
   - Exercice : Créez une clé avec un TTL de 60 secondes et testez sa suppression automatique.

2. **Classements :** Implémentez un tableau de scores pour les utilisateurs avec les ensembles ordonnés.
   - Exercice : Classez des joueurs par leur score et récupérez le top 3.

3. **Système Pub/Sub :** Implémentez une communication en temps réel entre deux terminaux Redis.
   - Terminal 1 :
     ```bash
     SUBSCRIBE notifications
     ```
   - Terminal 2 :
     ```bash
     PUBLISH notifications "Nouvelle mise à jour disponible !"
