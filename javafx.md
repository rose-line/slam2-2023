# Conception d'une interface graphique avec JavaFX

## Prérequis

- Visual Studio Code pour Java
- Installation d'un JDK récent (minimum JDK 13)
- Variable d'environnement `JAVA_HOME` qui pointe vers le répertoire de ce JDK

## Création de projet JavaFX sous Maven (pour **VS Code** uniquement)

- Command Palette (Ctrl+Shift+P) -> "_Create Maven Project_"
- "_More..._"
- Recherchez "javafx" et sélectionnez "javafx-archetype-fxml" de _org.openjfx_
- Choisir la version la plus récente proposée
- **Sélectionnez un dossier en dehors de tout dossier-projet déjà créé** (un nouveau dossier-projet sera créé dans le répertoire que vous indiquez - en général vous vous placerez dans votre dossier "ProjetsJava" local)
- group-id : fr.dampierre (ou un autre nom de domaine fictif inversé, pas de majuscules, pas d'espaces)
- artefact-id : projetjavafx (votre nom de projet, pas de majuscules, pas d'espaces)
- version : (tapez entrée en laissant 1.0)
- package : (tapez entrée si demandé)
- Confirmez par Y
- BUILD SUCCESS ! (Non ? Vérifiez le JDK installé, et la variable d'environnement `JAVA_HOME`)
- Ouvrez le dossier-projet nouvellement créé et lancez l'application

## _Scene Builder_

- **_Scene Builder_** (SB) est une application indépendante de tout IDE qui permet de faciliter la réalisation d'interfaces graphiques (GUI) JavaFX
- Recherchez et installez _Scene Builder_ (site de _Gluon_)
- Dans SB, vous créerez/modifierez des fichiers FXML contenant les « écrans » qui seront ensuite chargés par votre application Java
- Erreur fréquente : SB ne vous rappelle pas de sauvegarder ; du coup il arrive que l'on relance l'application sous VS Code après avoir modifié un FXML sous SB et que l'on se demande pourquoi ça ne fonctionne pas...

## Extensions VS Code

Deux toutes petites extensions VS Code pratiques à installer :

- `JavaFX Support` : permet d'éviter quelques avertissements (_warnings_) issus de bugs
- `SceneBuilder extension for Visual Studio Code` : permet simplement d'ouvrir par clic-droit un fichier FXML directement dans Scene Builder

## Fonctionnement de JavaFX

- Tout tourne autour de l'interaction entre les fichiers .java (logique de l'application, code métier, édités sous VS Code) et les fichiers FXML (qui contiennent les interfaces graphiques, édités sous SB)
- Dans une application JavaFX :
  - on dispose d'un objet `Stage` (c'est la fenêtre principale) fourni par le _runtime_
  - le `Stage` contient un objet `Scene` (contenu de la fenêtre, qu'on va pouvoir changer au cours de l'application)
  - la `Scene` contient une `root` (racine) : c'est un fichier FXML qui décrit l'interface graphique, notamment l'emplacement de tous les éléments graphiques utilisés : les _controls_ (boutons, listes déroulantes, zones de texte...)
- En général, on ne modifie pas les fichiers FXML directement : comme dit plus haut, on utilisera Scene Builder (SB)
- Donc : on décrit une ou plusieurs _root_ (écrans) en FXML (avec SB) et on vient peupler la `Scene` avec la méthode `setRoot()`

## Utilisation de Scene Builder

- Quatre grandes zones à différencier :
  - au centre : l'interface graphique sur laquelle on travaille
  - en haut à gauche (_Library_) : la bibliothèque de *controls* ; on vient peupler l'interface graphique avec les _controls_ par glisser-déposer, soit directement où on veut dans la partie centrale, soit dans la zone hiérarchie (parfois plus facile)
  - en bas à gauche : la hiérarchie, qui retranscrit tous les _controls_ qui sont actuellement inclus, sous forme d'arborescence
  - à droite (_Inspector_) : notamment les propriétés du _control_ actuellement sélectionné ; c'est là qu'on va modifié le texte d'un label, les dimensions d'un bouton, appliquer un effet, définir un gradient, etc. Beaucoup de choses sont possibles pour chaque _control_

## _Layout_ (dispositon des controls)

- Pour concevoir une interface graphique en JavaFX, il faut raisonner en terme de d'éléments _containers_ qui contiendront d'autres éléments (éventuellement d'autres _containers_)
- Il y a donc une hiérarchie depuis la racine (_root) qui est très souvent un \_container_
- Les \_containers peuvent contenir récursivement d'autres containers, et ainsi de suite, sans limite de profondeur ; cela permet de concevoir des écrans potentiellement très complexes
- Par exemple, on pourra avoir :
  - un _container_ `VBox` (layout vertical)
  - qui contiendra un label,
  - un bouton
  - et aussi un autre container \_HBox (layout horizontal) avec d'autres éléments

## Les _containers_

- Le panneau _Library_ contient plusieurs catégories de _controls_, dont les _containers_
- Chaque _container_ disponible est spécialisé pour une certaine disposition des éléments qu'il contient
- En voici quelques-uns :

### StackPane

- Empile les _controls_ les uns sur les autres, au centre
- En général on ne veut pas cela car les éléments sont cachés les uns derrière les autres

### AnchorPane

- Permet de « scotcher » (ancrer) les éléments par rapport au 4 bords du _container_
- Ainsi, quand on redimensionne, l'élément reste « ancré » par rapport aux bords désignés
- Souvent utilisé comme _container_ racine pour contrôler les marges

### GridPane

- Définit une grille nxm dans laquelle on place les _controls_ sur n lignes et m colonnes
- Un _control_ pourra s'étendre sur plusieurs lignes et/ou colonnes, si besoin
- _Container_ très pratique, par exemple pour ranger trois lignes de couples `label+textfield`, on pourra utiliser une `GridPane` de 3x2

### FlowPane et TilePane

- Permet de définir des _controls_ les uns à côté des autres (ou les uns en dessous des autres)
- S'adapter à la taille de la fenêtre, dans le sens où les _controls_ « passeront à la ligne » si la place n'est pas suffisante

### BorderPane

- Permet de créer un layout avec cinq sections : haut, bas, gauche, droite, centre (potentiellement vides)
- Chaque section est elle-même un _container_ pouvant contenir d'autres éléments
- C'est ainsi qu'est définie l'interface de SB, par exemple

### SplitPane

- Permet de diviser l'espace en deux parties redimensionnables horizontalement ou verticalement
- Par exemple, le panneau gauche de VS Code est redimensionnable en glissant-déposant le bord du panneau
- Typiquement, les deux zones d'un `SplitPane` contiennent un autre _container_
- Un `SplitPane` peut contenir un autre `SplitPane`, etc. On peut ainsi construire un layout arbitrairement complexe dont chaque partie est redimensionnable

### HBox, VBox, ButtonBar

- `HBox` : placement horizontal de _controls_
- `VBox` : placement vertical
- `ButtonBar` : spécialisé dans le placement horizontal de boutons

## Les controllers

### FXML

- Le format FXML a une structure de type _markup_ (comme XML, HTML...), c'est une structure hiérarchique qui reprend la hiérarchie des _controls_ JavaFX définis dans SB
- L'architecture est de type MVC : Model-View-Controller, bien connue dans les frameworks Web
  - le _Model_ définit les objets données
  - la _View_ définit l'interface graphique (ici décrite en FXML)
  - le _Controller_ fait office d'intermédiaire entre la _View_ et le _Model_
- Quand vous donnez un fichier FXML comme racine d'une _Scene_, le FXML est analysé et transformé en objets Java qui vont pouvoir être manipulés dans le code (accès en lecture/écriture a telle zone de texte, peupler une liste, réaction à un événement...)
- Pour cela, en FXML sous SB, on donne des noms à chaque élément que l'on va vouloir manipuler (sous-panneau _Code_ de _Inspector_, propriété `fx:id`)
- Dans le _controller_, on aura alors accès à cet élément en le désignant par son nom

### Configurer les actions

- Des événements, auxquels vous pouvez réagir, sont définis sur chaque _control_
  - clic sur un bouton
  - appui sur touches du clavier
  - passer la souris au-dessus d'un élément...
- On peut définir des méthodes, appelées _handlers_, qui définissent ce qui doit être fait lorsque l'événement survient
- On relie un événement à une méthode _handler_ par l'intermédiaire du _controller_ (voir plus loin)

### Exemple de _controller_

- On va reprendre comme base le `PrimaryController` défini dans l'application-demo, comprendre comment il fonctionne et le modifier
- Dans SB, ouvrez `primary.fxml`, situé dans un sous-répertoire de `resources`
- Notez la structure aborescente du code FXML ; cependant, en général, vous n'allez jamais éditer un fichier FXML directement, même si il arrive de les consulter pour chercher à expliquer une erreur
- Utilisez le clic-droit pour l'ouvrir sous SB
- Dans SB, ouvrez le sous-panneau _controller_ (tout en bas à gauche)
  - notez que le champ `Controller class` est défini
  - cela signifie qu'un _controller_ est relié à cette _view_
  - c'est le fichier `PrimaryController` que l'on retrouve dans notre projet
  - il est complètement qualifié, c'est-à-dire qu'il est précédé du nom de son package (`fr.votredomaine`)
  - quand vous définissez une vue, **vous devez toujours définir le _controller_ associé**
- Sélectionnez maintenant le bouton _Switch to Secondary View_
- Dans le panneau _Inspector_, sous-panneau *Code* :
  - localisez `fx:id` (_identity_)
  - notez que le champ est déjà renseigné avec le nom `primaryButton`  : c'est le nom du bouton
  - localiser `On Action`
  - notez que le champ est déjà renseigné avec `switchToSecondary` : c'est le nom de la méthode qui va être associée à l'action principale de cet élément (clic)
- Allez dans le menu _View/Show Sample Controller Skeleton_
  - cela montre un exemple de code _controller_
  - notez qu'il définit une variables pour l'élément auquel on a donné un nom
  - et une méthode vide dont le nom correspond au nom donné plus haut
  - lorsque l'on définit de nouveaux noms/actions, on peut utiliser cet exemple de code pour copier/coller les nouvelles parties dans le _controller_ sous VS Code avant de définir le code des méthodes
- Revenez sous VS Code et observez de nouveau le code de `PrimaryController`
  - notez que le nom du bouton n'a pas été conservé (le code ne l'utilise pas) ; s'il était défini, on pourrait accéder aux propriétés du bouton dans le code en utilisant la notation pointée (par exemple `primaryButton.setTextFill(Color.web("BLUE"))` changera le texte en bleu)
  - l'entête de la méthode change un peu par rapport au code-exemple de SB mais cela revient au même
  - la méthode est implémentée : elle fournit le code associé à l'événement
  - ici on vient, en appelant `setRoot()` dans la classe `App`, changer la racine de la `Scene`, ce qui résulte en l'affichage du second écran (`secondary.fxml`)

### Testez par vous-même

- Dans `secondary.fxml`, faire en sorte que, lorsque l'on clique sur le bouton :
  - la couleur du texte du bouton passe en rouge
  - le texte du label du dessus se change en "COUCOU !"
  - (consultez la section plus bas pour les détails si vous n'y arrivez pas)

### N'oubliez pas

- Quand vous définissez une nouvelle vue (FXML), vous devez :
  - définir un nouveau fichier _controller_ dans VS Code
  - sous SB, associer la vue avec ce _controller_
  - pour chaque élément auquel on voudra accéder, lui donner un nom
  - pour chaque événement auquel on voudra réagir, renseigner le nom de méthode correspondante sur le _control_ associé
  - utiliser SB pour vous indiquer à quoi doit ressembler le code de _controller_
  - utiliser la notation pointée pour accéder aux propriétés des _controls_ (en lecture ou en écriture)

## Détails de l'implémentation d'un nouvel écran

Voici donc les étapes à considérer lorque vous concevez un nouvel écran :

- Partir d'un fichier .fxml existant (en le copiant) ou bien en créant un fichier .fxml vierge sous SB
  - rappel : les fichiers .fxml doivent être enregistrés dans un sous-répertoire (correspondant au nom de votre package) du répertoire `resources` de votre projet pour que Java les trouvent (par exemple `mon-projet\src\main\resources\fr\pgah\Connexion.fxml`)
- Donner un nom expressif à ce fichier .fxml
  - (donner un nom expressif à tout ce qu'on peut nommer est (très) important en programmation)
- Créer l'interface graphique en glissant-déposant les _controls_ dont vous avez besoin
- Utiliser les panneaux _Proprerties_ et _Layout_ de droite pour adapter l'élément à vos besoins (texte affiché, couleur, taille... en fonction de la nature de l'élément)
- Utiliser le troisième panneau _Code_ de droite pour :
  - donner un nom à l'élément grâce à l'attribut `fx:id` (uniquement si vous avez besoin d'y accéder depuis le code Java par la suite, par exemple pour récupérer le contenu d'un _textfield_)
  - indiquer un nom de méthode qui se produira lorsque l'élément sera « actionné » grâce à l'attribut `On Action` ou un autre désignant l'événement (uniquement si vous voulez réagir à une action sur cet élément, le plus souvent pour un clic de bouton)
- Quand vous avez un écran suffisamment rempli pour la mise en place d'au moins une fonctionnalité, il est temps de passer à la logique côté Java sous VS Code, pour coder le _controller_ qui va correspondre à cette vue
- Mais d'abord, pour se faciliter la tâche, on va demander à SB de nous montrer à quoi devra ressembler le code du _controller_ pour qu'il prenne en compte tous les éléments que l'on a nommés et les méthodes que l'on a mises en place
  - menu `View/Show Sample Controller Skeleton`
  - copier le code du _controller_ présenté
  - voici un exemple avec un champ texte nommé `txtPrenom` et un bouton nommé `btnValider` ; on a également indiqué qu'un clic sur ce bouton lance la méthode `btnValiderClic`

```java
package fr.pgah;

import javafx.event.ActionEvent;
import javafx.fxml.FXML;
import javafx.scene.control.TextField;
import javafx.scene.control.Button;

public class TestController {

  @FXML
  private TextField txtPrenom;

  @FXML
  private Button btnValider;

  @FXML
  void btnValiderClic(ActionEvent event) {
  }
}
```

- Notez la façon dont les éléments graphiques sont nommés : un préfixe indique de quel type d'élément il s'agit
  - `txt` pour un champ texte, `btn` pour un bouton, etc.
  - ce n'est pas obligatoire mais c'est une bonne convention
- Sous VS Code, créer un nouveau fichier aux côté du fichier `App.java`, dans le répertoire principal de votre package
  - même nom que pour le fichier .fxml, avec le suffixe `Controller` (ex: `ConnexionController.java`)
  - y coller le code du « squelette » donné par SB
- N'oubliez pas que **chaque variable et chaque méthode déclarée en FXML (depuis SB) doit avoir l'annotation `@FXML`** sinon Java ne considérera pas que les deux sont reliés (mettre plusieurs déclarations de variables sous une seule annotation `FXML` n'est pas suffisant)
- Pour relier effectivement le fichier FXML avec ce _controller_, retourner dans SB
  - panneau _Controller_, en bas à gauche
  - indiquer le nom complet de la classe _controller_ nouvellement définie
  - conseil : utiliser le _dropdown_ pour être sûr de ne pas se tromper
- Il faut maintenant implémenter les méthodes d'action pour effectuer les traitements correspondants
- Souvent, on a besoin de récupérer des informations depuis l'interface graphique pour effectuer le traitement
  - ex : récupérer le contenu d'un champ texte
  - il faut « interroger » l'objet correspondant
  - ex : `txtPrenom.getText()` retourne le contenu du champ texte
  - on peut ensuite utiliser cette donnée dans un traitement quelconque :

```java
@FXML
void btnValiderClic(ActionEvent event) {
  String lePrenom = txtPrenom.getText();
  // Affichage sur la console pour vérification
  System.out.println("Prénom récupéré :" + lePrenom);
  // Puis on peut utiliser lePrenom dans une requête SQL
  // Code de connexion BDD non reproduit...

  // Préparation de la requête
  String req = "SELECT * FROM Users WHERE prenom='" + lePrenom + "'";
  Statement statement = conn.createStatement();
  // Exécution de la requête
  ResultSet result = statement.executeQuery(req);
  // On boucle pour traiter tous les résultats
  while (result.next()){
    // Récup du num et de l'email depuis cet enregistrement
    String nom = result.getString("nom");
    String email = result.getString("email");
    // Simple affichage dans la console de sortie
    System.out.println("Nom : " + nom + " ; email : " + email));
  }
}
```

- Les `println` sont utiles pour suivre l'avancée des opérations et vérifier que les données récupérées sont cohérentes, mais on va aussi vouloir mettre à jour l'écran JavaFX avec ces données
- Il faut de nouveau aller interroger les objets qui correspondent aux éléments à mettre à jour pour savoir comment les modifier
  - une façon d'explorer est de taper `txtPrenom.` dans l'IDE : VS Code va nous montrer toutes les méthodes pouvant être appelées depuis cet objet
  - on peut aussi [se renseigner sur Internet](http://tutorials.jenkov.com/javafx/textfield.html) sur l'usage du _control_ JavaFX en question
  - par exemple, pour afficher dans un `TextField` une informations d'un employé dont a a récupéré l'id :

```java
@FXML
void btnValiderClic(ActionEvent event) {
  // Récupération de l'id depuis un champ texte
  int num = txtNumEmploye.getText();
  // Code de connexion BDD non reproduit...
  // Préparation de la requête
  String req = "SELECT prenom FROM Employes WHERE id='" + num + "'";
  Statement statement = conn.createStatement();
  // Exécution de la requête
  ResultSet result = statement.executeQuery(req);
  // Cette fois on n'a qu'un seul résultat (au plus)
  // puisqu'on a filtré sur la clé primaire
  // On teste pour savoir si on a effectivement un résultat
  if (rs.next())
  {
    // Récup du prénom depuis le résultat de la requête
    String prenom = rs.getString("prenom");
    // Mise à jour du champ texte JavaFX avec setText
    txtPrenom.setText(prenom);
  }
}
```
