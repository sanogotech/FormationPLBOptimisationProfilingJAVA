
Voici un TP détaillé avec un titre et la liste des prérequis pour l'intégration d'un service d'envoi d'emails avec Spring Boot en utilisant **FakeSMTP**.

---

## TP : Intégration d'un service d'envoi d'emails dans une application Spring Boot avec FakeSMTP

### Objectif du TP :
L'objectif de ce TP est de configurer une application Spring Boot pour envoyer des emails via un serveur SMTP simulé (FakeSMTP). Cette solution permet de tester l'envoi d'emails sans qu'ils soient réellement envoyés, en capturant les messages dans une interface graphique. Ce TP est utile pour le développement local avant de passer à un service de messagerie réel.

---

### Prérequis

Avant de commencer, assurez-vous que vous avez les éléments suivants :

1. **Java Development Kit (JDK)** :
   - JDK 8 ou version supérieure doit être installé sur votre machine.
   - Vérifiez l'installation de Java avec la commande suivante :
     ```bash
     java -version
     ```

2. **Spring Boot** :
   - Avoir une application Spring Boot fonctionnelle.
   - Si vous n'avez pas encore créé de projet Spring Boot, vous pouvez le faire via [Spring Initializr](https://start.spring.io/) ou en utilisant Maven/Gradle.

3. **Maven ou Gradle** :
   - Ce TP utilise Maven pour la gestion des dépendances, donc assurez-vous que Maven est installé sur votre machine. Vous pouvez vérifier cela avec :
     ```bash
     mvn -version
     ```

4. **FakeSMTP** :
   - Téléchargez FakeSMTP depuis [GitHub - FakeSMTP](https://github.com/Nilhcem/FakeSMTP) et assurez-vous qu'il est prêt à être utilisé en local.
   - FakeSMTP fonctionne en tant qu'application Java autonome et peut être démarré via un fichier `.jar`.

5. **IDE pour développement Java** :
   - Un IDE tel que **IntelliJ IDEA**, **Eclipse**, ou **Visual Studio Code** avec support Java doit être installé.

---

### Étapes du TP

#### 1. **Télécharger et démarrer FakeSMTP**

- Téléchargez la dernière version de **FakeSMTP** depuis [ici](https://github.com/Nilhcem/FakeSMTP/releases).
- Une fois téléchargé, exécutez le fichier JAR en utilisant la commande suivante dans le terminal :
  ```bash
  java -jar fakeSMTP-x.x.x.jar
  ```

  Par défaut, FakeSMTP démarrera un serveur SMTP sur le port `2525`. Vous pouvez également personnaliser le port si nécessaire via les options de ligne de commande.

- Lorsque FakeSMTP est démarré, vous devriez voir une interface graphique où les emails envoyés seront capturés.

#### 2. **Configurer Spring Boot pour utiliser FakeSMTP**

Dans votre application Spring Boot, configurez le serveur SMTP pour qu'il pointe vers **FakeSMTP**.

1. **Ajouter la dépendance Spring Mail à votre `pom.xml` (si vous utilisez Maven)**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

2. **Configurer `application.properties`**

Ajoutez les propriétés suivantes dans le fichier `src/main/resources/application.properties` :

```properties
# Configuration du serveur SMTP local (FakeSMTP)
spring.mail.host=localhost  # Adresse de FakeSMTP
spring.mail.port=2525       # Port par défaut de FakeSMTP
spring.mail.username=       # Aucun nom d'utilisateur requis
spring.mail.password=       # Aucun mot de passe requis
spring.mail.properties.mail.smtp.auth=false  # Pas d'authentification
spring.mail.properties.mail.smtp.starttls.enable=false  # Pas de STARTTLS requis
spring.mail.properties.mail.smtp.starttls.required=false  # Pas de STARTTLS requis
```

#### 3. **Créer un service pour envoyer des emails**

Dans votre application Spring Boot, créez un service pour envoyer des emails en utilisant le `JavaMailSender` de Spring.

1. **Créer le service `EmailService.java`**

```java
import javax.mail.MessagingException;
import javax.mail.internet.MimeMessage;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.mail.javamail.MimeMessageHelper;
import org.springframework.stereotype.Service;

@Service
public class EmailService {

    @Autowired
    private JavaMailSender mailSender;

    public void send(String from, String to, String subject, String content) {
        try {
            MimeMessage mimeMessage = mailSender.createMimeMessage();
            MimeMessageHelper helper = new MimeMessageHelper(mimeMessage, false, "utf-8");

            helper.setFrom(from);
            helper.setTo(to);
            helper.setSubject(subject);
            mimeMessage.setContent(content, "text/html");

            mailSender.send(mimeMessage);
            System.out.println("Email envoyé à " + to);
        } catch (MessagingException e) {
            System.err.println("Erreur lors de l'envoi de l'email : " + e.getMessage());
        }
    }
}
```

#### 4. **Tester l'envoi d'un email**

- Lancez votre application Spring Boot et vérifiez que l'email est capturé par **FakeSMTP**.
- Dans **FakeSMTP**, vous pourrez voir l'email envoyé apparaître dans l'interface, sans qu'il soit réellement envoyé à une adresse email.

#### 5. **Conclusion**

Une fois ces étapes réalisées, vous avez réussi à configurer un serveur SMTP local avec **FakeSMTP** pour tester l'envoi d'emails dans une application Spring Boot. Cela vous permet de tester votre fonctionnalité d'email sans envoyer réellement les emails, ce qui est idéal pour les environnements de développement.

---

### Remarques
- **FakeSMTP** est conçu pour un usage local et de développement uniquement. Lors du passage en production, vous devrez remplacer **FakeSMTP** par un vrai serveur SMTP tel qu'**Amazon SES**, **SendGrid**, ou tout autre fournisseur d'email.
- Assurez-vous de ne pas utiliser les mêmes configurations en production, notamment pour la sécurité et l'authentification.

---

Avec ce TP, vous avez couvert l'intégration des services d'email dans une application Spring Boot en utilisant un serveur SMTP simulé. Ce processus vous aidera à tester la fonctionnalité avant de l'intégrer à un véritable service de messagerie.
