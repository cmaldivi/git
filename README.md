# liens utiles

 - [Le livre de référence, en Français](https://git-scm.com/book/fr/v2)
 - [Rappel des principales commandes](http://files.zeroturnaround.com/pdf/zt_git_cheat_sheet.pdf)
 - [Visuel intéractif des zones de travail de git](http://ndpsoftware.com/git-cheatsheet.html)
 - [Une configuration Git aux petits oignons](http://www.git-attitude.fr/2013/04/03/configuration-git/)
 - [30 options de commande Git qui gagnent à être connues](http://www.git-attitude.fr/2014/09/15/30-options-git-qui-gagnent-a-etre-connues)
 - [Bien utiliser Git merge et rebase](http://www.git-attitude.fr/2014/05/04/bien-utiliser-git-merge-et-rebase/)
 - [Git reset : rien ne se perd, tout se transforme](http://www.git-attitude.fr/2016/05/11/git-reset/)
 - [Créer le changelog automatiquement à partir de vos commits](https://github.com/conventional-changelog/conventional-changelog)
 
# à l'aide

Si la page "man" du terminal ne vous plaît pas, vous pouvez accéder à la doc html en utilisant l'option '-w'; par exemple :

```git help status -w```

Pour accèder aux guides : ```git help -g```. Par exemple ```git help everyday``` est un bon moyen d'apprendre, en fonction de votre utilisation de git.

# Bien configurer votre environnement

## diff tout beau

Par défaut, ```git diff``` n'est pas terrible, et l'on a souvent recours à un outil plus sexy. Or on peut grandement améliorer le rendu du diff avec un peu de configuration.

Essayons ```git diff --ignore-all-space --color-words```
 - ```--ignore-all-space````ou ```-w``` pour ignorer les changements liés aux espaces
 - ```--color-words``` pour éviter les accolades et le diff sur les lignes, et lui préférer celui sur les mots
 
Bon, rappelons nous que l'on doit se simplifier la vie, donc : ```git config --global alias.di "diff --ignore-all-space --color-words"``` pour créer un alias. 

On pourra donc maintenant **utiliser ```git di``` tout simplement**.

## log tout beau
 - SHA1 complets, ça ne sert à rien, donc : ```git config --global log.abbrevCommit true```
 - ```git log --graph --date=relative --pretty=tformat:'%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%an %ad)%Creset'```
 
 - ```git config --global alias.lg "log --graph --date=relative --pretty=tformat:'%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%an %ad)%Creset'"```

On pourra donc maintenant **utiliser ```git lg``` tout simplement**.

NB: ```git lg -10 -Gfilename -- FileLoader.java``` pour chercher les 10 derniers commits qui ont touché à la variable filename...

## Entrer dans les répertoires untracked lors du status
Important pour ne pas oublier d'ajouter un fichier, avec le risque de le supprimer lors d'un clean : ```git config --global status.showUntrackedFiles all```

# Staged ou pas staged ?
Voir le [Visuel intéractif des zones de travail de git](http://ndpsoftware.com/git-cheatsheet.html).

Regarder le statut suivant:
```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   file2

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   file2

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	file6
```

Il est important de comprendre la différence entre "staged", "not staged" et "untracked". Voir les commandes suivantes :
 - ```git diff --staged```
 - ```git diff```
 
Si vous souhaitez utiliser la séparation "staged" / "not staged", attention à ne pas tout committer d'un coup avec par exemple ```git commit -a -m "feat(user): add login"```.

# Stash, ou comment mettre sous le tapis pour un moment
Voir le [Visuel intéractif des zones de travail de git](http://ndpsoftware.com/git-cheatsheet.html).

Par exemple : ```git stash save -u "Met de côté l'ajout de file2 et file3"``` qui va :
  - créer une nouvelle "remise" pour sauver toutes les modifications locales, puis remettre au propre la branche courante (```git reset --hard```)
  - utiliser l'option ```save``` pour bien fournir un message : dans une semaine vous ne vous souviendrez pas à quoi correspond cette "remise" !
  - utiliser l'option ```-u``` pour ajouter les fichiers "untracked" afind d'être sûr de ne rien oublier
  
Puis utiliser ```git stash pop``` pour restaurer vos modifications. Utiliser ```pop``` plutôt que ```apply``` pour bien supprimer le stash en même temps.

Stash est utile pour une sauvegarde temporaire ! Si vous n'êtes pas dans ce cas, créez une branche locale avec vos modifs (```git checkout -b save_unfinished_work```).

# Mr propre
 - ```git clean -f -d``` pour supprimer tous les "untracked" (dangereux...)
 - ```git reset --hard``` pour supprimer toutes les modifications "staged" / "unstaged" (dangereux...)

# Branches
Visualiser vos branches locales avec rappel du remote et indication du dernier commit : ```git branch -vv```

Pour voir les branches remotes ajouter ```-a```

Pour créer une branche locale avec les modifications en cours, et passer dessus : ```git checkout -b save_unfinished_work```

Raccourci pour revenir à la branche précédente : ```git checkout -```

Pousser une branche sur le remote, et mettre en place le tracking du remote par la branche locale : ```git push -u origin topic```

# Rebase

Quelques principes... :
 - on souhaite garder un historique de commit bien propre. Donc avant de pousser ma branche sur le remote pour les copains, il faut que je puisse nettoyer mes commits : messages, unicité par fonctionnalité, etc
 - les copains ont bossé et committé sur master pendant que je travaillais sur ma branche. J'aimerais réappliquer mes modifications sur le master le plus récent pour régler les éventuels conflits qui seraient apparus avec mes modifications
 
## Oups !

J'ai commité (pas encore pushé...), mais j'ai oublié un truc. Pas de panique, je fais ma modif et je committe de la façon suivante : ```git commit --amend --no-edit```

Ah oui : ```git config --global alias.oops "commit --amend --no-edit"```

## Me remettre à jour par rapport à la branche distante, et réappliquer mes modifications

On devrait (presque) toujours faire ```git pull --rebase``` au lieu de ```git pull```. Cela éviterait des merges parasites et inutiles (problème technique qui n'est en rien lié à la fusion d'une fonctionnalité).

## Sur ma branche dérivée du master, réappliquer mes modifications sur le master le plus récent
A faire systématiquement avant de soumettre un pull/merge request, ou d'essayer de faire une fusion :
```git rebase master topic-branch```. Ainsi on règle les conflits de suite (et cela facilite la vie de git qui pourra faire un fast forward lors de la fusion...).

## Réécrire l'historique des modifications de ma branche avant fusion
```git rebase -i```. NB: pour fusionner les commits sans changer de message, utiliser 'fixup' plutôt que 'squash'


# Conventional Changelog

## Suivez un template pour vos commits
C'est une bonne pratique qui vous aidera en plus à générer le changelog automatiquement : ```git config --global commit.template .git-commit-template.txt```

Par exemple :
```
# Type(<scope>): <subject>

# <body>

# <footer>

# Type should be one of the following:
# * feat (new feature)
# * fix (bug fix)
# * docs (changes to documentation)
# * style (formatting, missing semi colons, etc; no code change)
# * refactor (refactoring production code)
# * test (adding missing tests, refactoring tests; no production code change)
# * chore (updating grunt tasks etc; no production code change)
# Scope is just the scope of the change. Something like (admin) or (teacher).
# Subject should use impertivite tone and say what you did.
# The body should go into detail about changes made.
# The footer should contain any JIRA (or other tool) issue references or actions.
```

## Générer automatiquement le changelog
```conventional-changelog -p angular -r 5 -o ChangeLog.md```. Ensuite pour passer au html : ```pandoc ChangeLog.md -o ChangeLog.html```
