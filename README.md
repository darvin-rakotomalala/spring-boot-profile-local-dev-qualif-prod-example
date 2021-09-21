## Exemple Spring Boot Profil pour les environnements DEV et PROD
Dans ce projet, nous allons créer un exemple profil DEV et un profil PROD pour activer les propriétés de configuration spécifiques à l'environnement.

### Que sont les profils de démarrage Spring ?
---
L'un des principes de conception de base de Spring Boot est qu'il encourage les conventions plutôt que la configuration. 
Cela signifie que la grande majorité des configurations d'applications utilisent des valeurs par défaut sensibles 
qui peuvent être remplacées si nécessaire, mais en général, une application Spring Boot fonctionnera sans configuration personnalisée.
<br/>
Cependant, une certaine personnalisation est généralement nécessaire et nous avons souvent 
besoin d'une personnalisation spécifique à l'environnement. C'est là que les profils sont utiles. 
Un profil dans Spring Boot peut être considéré comme un contexte qui définit un ensemble spécifique 
de paramètres d'application, de variables et de comportements. Chaque fois que l'application est créée, 
le développeur peut spécifier le profil à utiliser. Si aucun profil n'est spécifié, la valeur par défaut sera utilisée.

### Prérequis
---
Pour ce projet, vous auriez besoin des spécifications suivantes :<br/>
- Spring Boot v2.0+
- JDK v1.8+
- Maven 3+ ou Gradle 4+ - outil de construction
- Tout IDE prenant en charge Java et Spring Boot (Spring Tool Suite (STS), IntelliJ, VSC, NetBeans, etc.)

### Dependances Maven
---
Dans ce projet nous allons utiliser les dependances Maven suivants :<br/>
- **Spring Web** - Pour inclure Spring MVC et utilise le tomcat comme conteneur intégré par défaut.
- **Spring Boot DevTools** - dépendance pour les rechargements automatiques ou le rechargement en direct des applications.

### Etapes à faire
---
On peut facilement définir les profils en ajoutant le XML suivant au fichier du projet `pom.xml`
```
<profiles>
	<profile>
		<id>LOCAL</id>
		<properties>
			<activatedProperties>LOCAL</activatedProperties>
		</properties>
		<activation>
			<activeByDefault>true</activeByDefault>
		</activation>
	</profile>
	<profile>
		<id>DEV</id>
		<properties>
			<activatedProperties>DEV</activatedProperties>
		</properties>
	</profile>
	<profile>
		<id>QUALIF</id>
		<properties>
			<activatedProperties>QUALIF</activatedProperties>
		</properties>
	</profile>
	<profile>
		<id>PROD</id>
		<properties>
			<activatedProperties>PROD</activatedProperties>
		</properties>
	</profile>
</profiles>
```
La balise `<activeByDefault>true</activeByDefault>` signifie que le profil de local sera utilisé 
par défaut en supposant qu'aucun profil n'est spécifié au moment de la génération.
<br/>
Les profils fonctionnent conjointement avec les fichiers de propriétés Spring Boot. Par défaut, 
Spring Boot analyse un fichier appelé `application.properties` – situé dans le répertoire `src/main/resources` – 
pour identifier les informations de configuration.
<br/>
Notre première tâche sera d'ajouter un paramètre dans ce fichier qui indiquera à Spring d'utiliser 
un autre fichier de propriétés spécifique à l'environnement correspondant au profil actif 
(c'est-à-dire le profil avec lequel l'application est actuellement exécutée). 
Nous pouvons le faire en ajoutant ce qui suit au fichier `application.properties` : 

`spring.profiles.active=@activatedProperties@` <br/>

On créer aussi deux nouveaux fichiers de propriétés spécifiques à l'environnement 
(dans le même chemin que le fichier `application.properties` existant), l'un à utiliser par le profil **DEV**
et l'autre à être utilisé par le profil **PROD**. Ces fichiers doivent être nommés comme suit :<br/>
* `application-dev.properties`
* `application-prod.properties`

Dans chacun de ces fichiers, des propriétés peuvent être définies qui ne seront appliquées que lorsque le profil correspondant est actif.

### Spécification du profil au moment de la construction
---
* Lors de la création de l'application pour la production en utilisant Maven comme outil de construction - `$ mvn -Pprod clean install`<br/>
`-P` est utilisé pour spécifier le profil à utiliser pour la construction.
* Si nous voulons définir le profil après la construction du code, nous pouvons utiliser un argument Java VM au lancement de l'application. Cela se fait comme suit: `$ java –jar -Dspring.profiles.active=prod app.jar`
* Alternativement, le profil peut être directement spécifié dans le fichier `application.properties` en ajoutant la ligne : `spring.profiles.active=prod`
