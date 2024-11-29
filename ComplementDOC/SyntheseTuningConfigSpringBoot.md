
# **Optimisation de la configuration pour tester la performance sous charge élevée dans une application Spring Boot**

L'optimisation de la configuration d'une application Spring Boot est essentielle pour garantir qu'elle peut gérer efficacement des charges de travail importantes, telles que des milliers de requêtes simultanées. Dans le cadre d'un test de performance, comme un test de charge avec 1000 requêtes simultanées, une configuration adéquate de votre serveur Tomcat, des connexions à la base de données et de la gestion des ressources statiques est cruciale. Cette section présente les principales configurations `application.properties` pour maximiser la capacité de traitement de votre application Spring Boot et permettre de supporter une charge importante tout en maintenant une performance optimale.

### **1. Configuration du serveur Tomcat**

Dans cette configuration, nous augmentons la capacité de gestion des connexions et des threads du serveur Tomcat pour traiter plus de requêtes simultanées.

```properties
# Nombre minimum de threads inactifs dans le pool de threads pour accepter les nouvelles requêtes
server.tomcat.threads.min-spare=50

# Nombre maximum de threads pour gérer les requêtes simultanées
server.tomcat.threads.max=1000

# Délai d'attente pour les connexions inactives (en millisecondes)
server.connection-timeout=20000

# Nombre maximum de connexions à accepter (par défaut 10000, ici réglé pour une charge élevée)
server.tomcat.max-connections=2000

# Taille du buffer pour les requêtes HTTP
server.tomcat.max-http-header-size=8192

# Taille maximale du corps des requêtes POST (10 MB par exemple)
server.tomcat.max-post-size=10485760
```

### **2. Compression des ressources statiques**
Activer la compression GZIP des ressources statiques (CSS, JS, HTML) permet de réduire la taille des fichiers transférés, ce qui améliore la vitesse de réponse pour les utilisateurs et réduit la charge du réseau.

```properties
# Activer la compression GZIP des ressources statiques
spring.web.resources.gzip.enabled=true

# Activer la compression des fichiers statiques via les en-têtes HTTP Cache-Control
spring.web.resources.cache.cachecontrol.compress=true

# Ajouter un délai de mise en cache pour les ressources statiques (par exemple, 1 heure)
spring.web.resources.cache.cachecontrol.max-age=3600
spring.web.resources.cache.cachecontrol.public=true
```

### **3. Configuration du pool de connexions à la base de données (si utilisé)**
Si votre application utilise une base de données, ajustez le pool de connexions pour gérer efficacement les requêtes en simultané.

```properties
# Configuration du pool de connexions HikariCP (le pool de connexions par défaut dans Spring Boot)
spring.datasource.hikari.maximum-pool-size=100
spring.datasource.hikari.minimum-idle=20
spring.datasource.hikari.idle-timeout=30000
spring.datasource.hikari.max-lifetime=600000
spring.datasource.hikari.connection-timeout=30000
```

### **4. Paramètres JVM**
Pour optimiser les performances, vous pouvez également ajuster les paramètres JVM utilisés pour exécuter l'application. Ces paramètres peuvent être spécifiés lors du lancement de l'application Spring Boot.

Voici un exemple de paramètres JVM à inclure dans la commande de lancement :

```bash
# Exemple de paramètres JVM pour améliorer les performances sous une forte charge
-Xms4g                    # Taille initiale de la mémoire heap (4 Go)
-Xmx4g                    # Taille maximale de la mémoire heap (4 Go)
-XX:+UseG1GC              # Utilisation du ramasse-miettes G1 pour une meilleure gestion de la mémoire
-XX:+UseStringDeduplication  # Réduction de l'empreinte mémoire pour les chaînes de caractères
-XX:MaxGCPauseMillis=200  # Limiter la pause maximale du Garbage Collector (GC) à 200 ms
```

### **5. Autres optimisations**
Si vous avez des ressources statiques très utilisées, comme des images, des scripts, ou des fichiers CSS, vous pouvez activer la mise en cache des ressources pour éviter de traiter à chaque fois la même requête.

```properties
# Activer le cache des ressources statiques pour améliorer les performances
spring.web.resources.cache.cachecontrol.max-age=86400  # Cache les ressources pendant 1 jour
spring.web.resources.cache.cachecontrol.public=true
```

### Exemple complet de **`application.properties`** :

```properties
# Paramètres du serveur Tomcat
server.tomcat.threads.min-spare=50
server.tomcat.threads.max=1000
server.connection-timeout=20000
server.tomcat.max-connections=2000
server.tomcat.max-http-header-size=8192
server.tomcat.max-post-size=10485760

# Compression GZIP des ressources statiques
spring.web.resources.gzip.enabled=true
spring.web.resources.cache.cachecontrol.compress=true
spring.web.resources.cache.cachecontrol.max-age=3600
spring.web.resources.cache.cachecontrol.public=true

# Configuration du pool de connexions HikariCP
spring.datasource.hikari.maximum-pool-size=100
spring.datasource.hikari.minimum-idle=20
spring.datasource.hikari.idle-timeout=30000
spring.datasource.hikari.max-lifetime=600000
spring.datasource.hikari.connection-timeout=30000

# Cache des ressources statiques
spring.web.resources.cache.cachecontrol.max-age=86400
spring.web.resources.cache.cachecontrol.public=true
```

### Conclusion
Cette configuration permet de préparer votre application Spring Boot pour gérer un nombre élevé de requêtes simultanées tout en maintenant une bonne performance grâce à une gestion optimisée des connexions, des threads et de la compression des ressources statiques. Testez ces paramètres en utilisant des outils comme **Apache AB** ou **JMeter** pour valider que votre application peut supporter un test de charge de 1000 requêtes simultanées efficacement.
