# liens utiles

 - [Le livre de référence, en Français](https://git-scm.com/book/fr/v2)
 - [Rappel des principales commandes](http://files.zeroturnaround.com/pdf/zt_git_cheat_sheet.pdf)
 - [Visuel intéractif des zones de travail de git](http://ndpsoftware.com/git-cheatsheet.html)
 - [Une configuration Git aux petits oignons](http://www.git-attitude.fr/2013/04/03/configuration-git/)
 - [30 options de commande Git qui gagnent à être connues](http://www.git-attitude.fr/2014/09/15/30-options-git-qui-gagnent-a-etre-connues)
 - [Bien utiliser Git merge et rebase](http://www.git-attitude.fr/2014/05/04/bien-utiliser-git-merge-et-rebase/)
 - [Git reset : rien ne se perd, tout se transforme](http://www.git-attitude.fr/2016/05/11/git-reset/)

 
# à l'aide

Si la page "man" du terminal ne vous plaît pas, vous pouvez accéder à la doc html en utilisant l'option '-w'; par exemple :

```git help status -w```

Pour accèder aux guides : ```git help -g```. Par exemple ```git help everyday``` est un bon moyen d'apprendre, en fonction de votre utilisation de git.

# Bien configurer votre environnement

## diff tout beau

Par défaut, ```git diff``` n'est pas terrible, et l'on a souvent recourt à un outil plus sexy. Or on peut grandement améliorer le rendu du diff avec un peu de configuration.

Essayons ```git diff --ignore-all-space --word-diff --word-diff-regex=.```
 - ```--ignore-all-space````ou ```-w``` pour ignorer les changements liés aux espaces
 - ```--word-diff``` pour éviter le diff sur les lignes, et lui préférer celui sur les mots
 - ```--word-diff-regex``` pour indiquer comment séparer les mots
 
Bon, rappelons nous que l'on doit se simplifier la vie, donc : ```git config --global alias.di "diff --ignore-all-space --word-diff --word-diff-regex=."``` pour créer un alias. 

On pourra donc maintenant **utiliser ```git di``` tout simplement**.

## log tout beau
 - SHA1 complets, ça ne sert à rien, donc : ```git config --global log.abbrevCommit true```
 - ```git log --graph --date=relative --pretty=tformat:'%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%an %ad)%Creset'```
 
 - ```git config --global alias.lg "log --graph --date=relative --pretty=tformat:'%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%an %ad)%Creset'"```

On pourra donc maintenant **utiliser ```git lg``` tout simplement**.

NB: ```git lg -10 -Gfilename -- FileLoader.java``` pour chercher les 10 derniers commits qui ont touché à la variable filename...

## Entrer dans les répertoires untracked lors du status
Important pour ne pas oublier d'ajouter un fichier, avec le risque de le supprimer lors d'un clean : ```git config --global status.showUntrackedFiles all```

# Stash, ou comment mettre d'en un coin pour un moment
Par exemple : ```git stash save -u "Met de côté l'ajout de file2 et file3"``` qui va :
  - créer une nouvelle "remise" pour sauver toutes les modifications locales, puis remettre au propre la branche courante (```git reset --hard```)
  - utiliser l'option ```save``` pour bien fournir un message : dans une semaine vous ne vous souviendrez pas à quoi correspond cette "remise" !
  - utiliser l'option ```-u``` pour ajouter les fichiers "untracked" afind d'être sûr de ne rien oublier
  
Puis utiliser ```git stash pop``` pour restaurer vos modifications. Utiliser ```pop``` plutôt que ```apply``` pour bien supprimer le stash en même temps.

Stash est utile pour une sauvegarde temporaire ! Si vous n'êtes pas dans ce cas, créez une branche locale avec vos modifs (```git checkout -b save_unfinished_work```).

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
 
 
