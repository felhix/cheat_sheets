# Premiers pas avec Git
## Qu'est-ce que versionner son code ?
Quand on écrit un projet, on se retrouve avec bcp de code et on peut s'y perdre. Versionner son code, c'est garder une trace de toutes les versions de son code. Cette trace, c'est le commit. Chaque commit créé une version du code.
Les commit sont aussi pratiques pour collaborer.

En gros, c'est la même chose quand vous faites une grosse présentation PPT. Vous faites tellement de modifications dessus que vous vous retrouvez à la fin avec le nom "VF_avec_retours_Jean01_final.ppt". Le versionning vous permet d'avoir toutes les versions sauvegardées, et de revenir à celles que vous voulez à tout moment, et de nous éviter ces tracas.

## Git par rapport aux autres solutions de versioning
Git a été créé par Linus Torvalds. Grosse communauté derrière. Différents modèles de gestion de version :

- centralisé : tout le code est centralisé
- distribué : en mode peer-to-peer. tout le monde a toutes les informations partout. Git est distribué

Git est distribué cela permet de ne pas tout perdre, c'est plus rapide, et pas besoin d'internet.

## Installer Git
Toutéla : https://openclassrooms.com/courses/gerer-son-code-avec-git-et-github/installer-git

## Faire son premier commit
Voici un récap des bases du commit :

- `git init` : il faut TOUJOURS commencer par initialiser git avec cette commande. Avec cette commande, le répertoire courant est considéré comme un repository git
- `git add [fichier]` : ajoute aux sauvegardes le fichier mentionné. **Protip** : si vous avez plusieurs fichiers à ajouter, vous pouvez utiliser `git add .` qui ajoute au repository tous les fichiers du dossier
- `git commit -m [commentaire]` : créé un commit (commit = sauvegarde suivie d'un commentaire).

Les commandes suivantes sont pratiques :

- `git status` : vous dit le status actuel de git.


## Lire l'historique
`git log` : permet de voir l'historique et de voir tous les commits. Les commits sont rangés avec : 

- SHA : liste de chiffres et lettres qui indentifient de façon unique le commit. 
- Auteur
- Date
- Message donné durant le commit : avec ce message, tu vas comprendre ce que faisait le commit. C'est pour cela qu'il est important d'avoir un bon nom.

Pour quitter le log, il faut appuyer sur `Q`. 

## Se positionner sur un commit donné
Imaginons que l'on doit revenir en arrière.
On va utiliser la commande `git checkout`

- `git checkout 45581cebdd2cae494f80f44010af9e4a86c9b8fa` : on dit à git de se positionner sur ce sha précis.
- `git checkout master` : revient sur le dernier commit

⚠️ `git checkout` ne marche que si vous n'avez pas de modifications non sauvegardées. Si vous êtes entre deux commits, git checkout ne marchera pas. Du coup il vous faudra soit faire une sauvegarde (== faire un commit), soit effacer tout pour revenir au commit d'avant.

## Effacer pour revenir au commit d'avant
La fonction `git reset --hard` permet de revenir au commit précédent, en effaçant tout. C'est une commande pratique quand on veut essayer de nouvelles choses, qui nous demandent de virer une partie du code.

# Apprendre à utiliser GitHub
## Comprendre les remotes
C'est cool d'avoir le code sur son ordi, mais c'est mieux s'il est en ligne et que d'autres peuvent y contribuer. Il est important de pouvoir définir un endroit distant pour y envoyer son code. On peut le faire avec la commande `git remote`.

## GitHub, késako ?
Il existe de nombreux services pour héberger les repositery en ligne. Github est le n°1 mondial, et c'est celui que l'on va utiliser.

## Comment lier son code à Github
Sur Github, vous pouvez créer des dossiers pour y héberger votre code, puis travailler dessus sur votre machine locale. Une fois bien relié au repository Github, il vous sera très aisé de de travailler dessus en local, et pousser en ligne votre code.

Pour relier un fichier GitHub à un dossier git, il y a plusieurs techniques

### Git clone
Cette technique est pratique si le repository sur GitHub existe déjà, et que vous voulez tout télécharger sur votre ordinateur pour effectuer les modifications. Voici la marche à suivre :

- aller sur le repository sur Github et cliquer sur "Clone or Download à droite. Copier, coller le lien donné par Github
- dans le dossier de votre choix, entrer `git clone [lien_donné_par_Github]` le dossier du repository Github devrait être téléchargé sur votre ordinateur
- aller dans le dossier du repository via `cd nomdurepo`
- étant donné que ce fichier est déjà sur git, pas besoin de faire `git init`
- faire les modifications
- `git add .`
- `git commit -m "commentaire_du_commit"`
- et pour pousser le contenu en ligne, il suffit de faire `git push origin master` (`git push` : on dit à git de pousser le dossier : `origin` : nom de mon remote, nom par défaut dans la plupart des cas ; `master` : c'est le nom de la branche)

### Git add origin
Cette technique sert si vous avez déjà le code sur votre ordinateur, et que vous voulez le lier à un repository Github. Voici la marche à suivre :

- faire du beau code dans un dossier local (`git init` est activé bien sûr)
- créér un repository sur Github, sans readme.md
- copier coller le `git remote add origin [url]` qu'ils donnent à l'acran juste après
- coller ceci dans le dossier de votre code
- tadam ! Vous pouvez l'uploader vos commit sur Github avec `git push origin master`

## Récupérer des modifications
Si une deuxième personne veut bosser avec nous sur le projet, on va récupérer les dernières modifications via git pull.

- `git pull origin master` : me dit les modifications. J'ai synchronisé mon remote avec ma machine locale. 

# Collaborer et maitriser son historique
Imaginons un moment que vous bossez sur un projet. Vous vous dîtes, mon code est bien, mais j'ai envie d'ajouter une feature à mon site. Les branches sont un super outil pour cela. Une branche est une divergence de la base de code principale, sur laquelle on peut travailler sans crainte. Git nous permet de naviguer facilement entre les branches.

## Créer une branche
On peut créér facilement une branche avec la commande `git branch nomdelabranche`. À partir de là, on peut bouger sur la branche avec `git checkout nomdelabranche`.

**Protip** : on peut créér une branche et bouger dessus direct avec `git checkout -b nomdelabranche`


## Fusionner des branches
Une fois que l'on est satisfait de notre travail on peut fusionner notre branche avec le tronc principal (après avoir fait un commit bien sûr 😇).

- `git checkout master` : revient sur le tronc principal
- `git merge nomdelabranche` : la branche sur laquelle nous nous trouvons est remplacée par la branche qui s'appelle _nomdelabranche_

### Résoudre les conflits
Quand on travaille sur le même fichier, Git ne saura pas ce qu'il faut garder ou enlever. Ainsi, on va taper `git merge master`, et il va nous sortir ceci  :

```shell
Auto-merging app/views/static_pages/home.html.erb
CONFLICT (content): Merge conflict in app/views/static_pages/home.html.erb
Automatic merge failed; fix conflicts and then commit the result.```

Pour résoudre ceci, on va devoir aller dans notre fichier en conflit, qui devrait ressembler à ceci :
```ruby
<<<<<<<<< HEAD
p "Kikou !"
============
p "Hello !"
>>>>>>>>> la_branche
```
Il faut donc garder que ce qui nous intéresse. Ensuite, si l'on fait `git status`, il va nous dire que l'on a des unmerged path. On va devoir lui dire que tout a été réglé.

Pour ceci, on va faire `git add nom_du_fichier`, `git commit` (sans rien). Au git commit, il devrait envoyer un message chelou. Il faut juste faire `:x` pour sauvegarder.

## Ignorer des fichiers
On peut ignorer les fichiers inutiles avec gitignore. On créé un fichier .gitgnore dans lequel je mets les fichiers que je veux ignorer. 


# Best practices
Voici quelques best practices quand on travaille sur git :

- toujours ouvrir par `git init`
- linker ensuite notre dossier avec `git remote add origin urldurepositorygithub`
- dès que vous voulez sauvegarder, faire un commit. Pour faire un commit, il faut faire `git add .`, puis `git commit "commentaireducommit"`
- dès que vous voulez avancer sur votre programme, faîtes un `git checkout nomdelabranche`, puis quand vous êtes satisfait de votre travail, faîtes un commit, `git checkout master`, `git merge nomdelabranche`
- quand vous voulez tester rapidement des trucs, au lieu de mettre votre code actuel en commentaire comme un vilain, faîtes un commit, supprimez-le, testez. Si vous êtes content : nouveau commit. Sinon `git reset --hard` pour tout effacer et revenir au commit précédent
- ne pas hésiter à faire des `git reset --hard` si vous n'êtes pas contents de votre travail


# Aller plus loin
[Le cours de OpenClassrooms est excellent](https://openclassrooms.com/courses/gerer-son-code-avec-git-et-github/creer-son-premier-repository).