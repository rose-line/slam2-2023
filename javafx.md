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
- Sélectionnez le bouton _Switch to Secondary View_
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

### N'oubliez pas

- Quand vous définissez une nouvelle vue (FXML), vous devez :
  - définir un nouveau fichier _controller_ dans VS Code
  - sous SB, associer la vue avec ce _controller_
  - pour chaque élément auquel on voudra accéder, lui donner un nom
  - pour chaque événement auquel on voudra réagir, renseigner le nom de méthode correspondante sur le _control_ associé
  - utiliser SB pour vous indiquer à quoi doit ressembler le code de _controller_
  - utiliser la notation pointée pour accéder aux propriétés des _controls_ (en lecture ou en écriture)
