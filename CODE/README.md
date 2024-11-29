
### **Configuration d'un Pool de Threads Asynchrone avec Spring**

```java
package org.springframework.samples.petclinic.system;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.EnableAsync;
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;

@Configuration // Indique que cette classe est une classe de configuration Spring.
@EnableAsync // Active le traitement asynchrone dans l'application.
public class AsyncConfig {

    @Bean(name = "taskExecutor") // Déclare un bean nommé "taskExecutor".
    public ThreadPoolTaskExecutor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor(); // Crée une instance de ThreadPoolTaskExecutor.

        executor.setCorePoolSize(5); // Définit le nombre de threads de base (toujours actifs).
        executor.setMaxPoolSize(10); // Définit le nombre maximal de threads en cas de forte charge.
        executor.setQueueCapacity(25); // Définit la taille maximale de la file d'attente pour les tâches.
        executor.setThreadNamePrefix("Async-"); // Ajoute un préfixe aux noms des threads pour faciliter le suivi.

        executor.initialize(); // Initialise le pool de threads.
        return executor; // Retourne l'instance configurée comme bean Spring.
    }
}
```

------------------------------------

### **Configuration Asynchrone avec Tuning dans `application.properties`**

---

#### **Fichier `application.properties`**

Définissez les paramètres du pool de threads directement dans le fichier de configuration :

```properties
# Configuration du pool de threads pour l'exécution asynchrone
spring.task.execution.pool.core-size=5          # Nombre de threads de base
spring.task.execution.pool.max-size=10          # Nombre maximal de threads
spring.task.execution.pool.queue-capacity=25    # Capacité de la file d'attente
spring.task.execution.thread-name-prefix=Async- # Préfixe des noms de threads
```

---

#### **Classe principale**

Dans la classe principale, activez simplement l'exécution asynchrone avec l'annotation **`@EnableAsync`** :

```java
package org.springframework.samples.petclinic;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableAsync;

@SpringBootApplication // Indique que c'est une application Spring Boot.
@EnableAsync // Active l'exécution asynchrone dans l'application.
public class PetClinicApplication {

    public static void main(String[] args) {
        SpringApplication.run(PetClinicApplication.class, args); // Démarre l'application.
    }
}
```

---

### **Explications**

1. **Configuration dans `application.properties` :**  
   - Les paramètres comme `core-size`, `max-size`, et `queue-capacity` sont externalisés dans le fichier de configuration. Cela permet de centraliser et de modifier facilement les réglages sans toucher au code source.

2. **Classe principale simplifiée :**  
   - La classe Java principale utilise seulement **`@EnableAsync`** pour activer l'exécution asynchrone.  
   - Aucun autre bean n'est nécessaire, car Spring Boot configure automatiquement l'exécuteur basé sur les propriétés définies.

---

### **Avantages**
| **Avantage**               | **Description**                                               |
|-----------------------------|-------------------------------------------------------------|
| **Simplicité**              | Le code Java reste léger et facile à lire.                  |
| **Modularité**              | Les paramètres sont configurés indépendamment du code.      |
| **Facilité de modification**| Les réglages peuvent être ajustés sans recompilation.       |
| **Convention Spring Boot**  | Profite des mécanismes de configuration automatique.        |
