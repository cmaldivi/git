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
 
Bon, rappelons nous que l'on doit se simplifier la vie, donc : ```git config --global alias.di "diff --ignore-all-space --word-diff --word-diff-regex=."``` pour créer un alias. On pourra donc maintenant utiliser ```git di``` tout simplement.

## log tout beau
 - SHA1 complets, ça ne sert à rien, donc : ```git config --global log.abbrevCommit true```
 - ```git log --graph --date=relative --pretty=tformat:'%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%an %ad)%Creset'```
 
 - ```git config --global alias.lg "log --graph --date=relative --pretty=tformat:'%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%an %ad)%Creset'"```
