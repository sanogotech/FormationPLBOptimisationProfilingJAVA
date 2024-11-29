
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
