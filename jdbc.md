# Accès à une base de données via JDBC

## Prérequis

- Visual Studio Code pour Java
- Installation d'un JDK récent (minimum JDK 13)
- Variable d'environnement `JAVA_HOME` qui pointe vers le répertoire de ce JDK

## JDBC

- JDBC (_Java Database Connectivity_) est une interface de programmation Java qui permet aux applications d'accéder à une base de données relationnelle
- Des pilotes JDBC sont disponibles pour tous les systèmes connus de bases de données relationnelles, comme MySQL
- Dans votre projet généré par Maven :
  - ouvrez la palette de commande
  - tapez `maven`
  - sélectionnez _Add a dependency_
  - trouvez `mysql-connector-java`
  - la dépendance devrait être ajoutée au fichier `pom.xml` :

```
<dependencies>
    ...
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>X.Y.Z</version>
    </dependency>
    ...
</dependencies>
```

- Si votre projet dispose d'un fichier `module-info.java` (s'il s'agit d'un projet javafx notamment), modifiez-le pour ajouter `java.sql` (en ne touchant surtout pas au reste du fichier) :

```java
module fr... {
  // requires...
  requires java.sql;
	// opens...
	// exports...
}
```

- Votre projet est maintenant configuré pour reconnaître du code JDBC d'accès à une BDD
- Consultez le lien suivant pour ensuite accéder à MySQL à partir de code Java : [Tuto indicatif pour l'utilisation de classes JDBC](https://www.codejava.net/java-se/jdbc/jdbc-tutorial-sql-insert-select-update-and-delete-examples)
- Attention, dans la suite, lorsque vous résolvez les imports d'objets SQL manquants (avec la petite ampoule ou bien `Ctrl+.`), sélectionnez le package `java.sql`
- **Partie 1** : à ignorer, vous devez déjà avoir une installation Java et MySQL correcte. Assurez-vous de bien relever les informations suivantes concernant la configuration du SGBRD (elles seront bien sûr nécessaires pour assurer la connexion Java/MySQL) : port, identifiant utilisateur, mot de passe, nom de la BDD
- **Partie 2** : dans l'idéal, utilisez immédiatement la BDD correspondante à ce que vous essayez de faire. Sinon, n'importe quelle petite table, comme celle qui est présentée, fera l'affaire pour tester
- **Partie 3** : Lisez (même si vous ne comprenez pas bien) cette partie décrivant les classes qui vont être utilisées. Essayez par la suite de vous y rapporter régulièrement pour comprendre ce qui se passe au fur et à mesure. Repérez les appels de méthodes statiques (directement sur une classe - Majuscule) et les appels de méthodes d'instances (sur des variables objets - minuscule)
- **Partie 4** : premier objectif => une connexion réussie à la BDD via Java (le message doit s'afficher dans la console). Notez que la connexion à la BDD est encapsulée dans un block `try...catch` : c'est une construction Java qui permet de capturer les erreurs éventuelles qui peuvent se produire lors de la connexion et permettent de récupérer le contrôle du flux d'exécution (au lieu d'avoir un programme qui se termine de manière abrupte par un message d'erreur)
- **Parties 5/6/7/8** : CRUD que vous adapterez à votre application. Chaque fonctionnalité de l'application sera modularisée dans sa (ses) propre(s) méthode(s). La connexion à la BDD, répétée à chaque requête, sera également encapsulée dans une méthode dédiée pour éviter la duplication de code
- Une fois le CRUD opérationnel en ligne de commande, on pourra s'occuper de construire une interface graphique avec JavaFX conjugué à *Scene Builder* ; les méthodes réagissant aux événements s'occuperont de la logique métier et appelleront les méthodes d'accès à la BDD quand elles en auront besoin
