---
layout: post
title: "Guide de survie d'une installation Rails"
description: ""
category: blog
french: true
---

J'écris cet article dans le but de réduire le [_bus factor_](https://en.wikipedia.org/wiki/Bus_factor) du [Wagon](https://www.lewagon.com/fr). Depuis plus de deux ans maintenant, j'ai eu l'occasion de cotoyer plus de **500 machines** (et oui, nous avons maintenant plus de 500 alumni !) et d'y installer Ruby on Rails. Alors bien sûr, je n'ai pas personnellement tapé chaque commande pour chaque élève, tout la procédure est [standardisée et documentée](https://github.com/lewagon/setup) précisément, mais vous voyez l'idée.

Quand je parle de _bus factor_, je veux dire qu'il arrive régulièrement qu'un ordinateur, Mac ou PC, soit récalcitrant à la procédure standard. C'est logique après tout, chaque élève amène son ordi, et chaque ordi a un passif. Peut-être que l'élève a suivi un vieux tuto il y a deux ans pour installer Ruby, et que cette version va mettre des battons dans les roues à la nouvelle. Ou alors une vieille base de données empêche PostgreSQL de démarrer car il y a une migration de données à faire. _Sounds familiar?_ Du coup, à chaque fois, le premier jour de chaque nouveau _batch_, je m'y colle. J'ouvre le terminal et à grand coup de `tail`, `rm` et autre commandes de base, je fixe le problème. J'ai aussi fait ce travail dans les différentes villes où nous avons eu la chance d'ouvrir un Wagon (Bruxelles, Bordeaux, Amsterdam, etc.). Comme Le Wagon se porte bien et que d'autres ouvertures sont au programme, il va bien falloir passer la main, et transmettre la **méthodologie** qui me permet de fixer ces problèmes de _setup_. C'est parti.

## Les fondamentaux

Je ne vais parler que des sytèmes Unix, donc OSX ou Linux. Au Wagon, nous ne développons pas en Ruby on Rails sur Windows, on fait un _dual boot_, une machine virtuelle ou encore un [Nitrous](https://www.nitrous.io)

### La variable d'environnement `$PATH`

Dans votre terminal, vous tapez des commandes, vous faîtes `Enter` et pouf, vous avez le résultat. Prenons l'exemple de la commande **`cat`**. Rien à voir avec les chats, son but est d'afficher dans le terminal le contenu d'un fichier.

Supposons que vous vous trouvez dans un dossier contenant un fichier `fichier.txt`. Si vous tapez la commande suivante,

```bash
cat fichier.txt
# => le contenu
# => du fichier
# => etc...
```

le contenu du fichier `fichier.txt` va être affiché.

Vous venez de demander à l'ordinateur d'exécuter le programme `cat` avec comme argument `fichier.txt`. Ce programme `cat`, c'est lui-même un **fichier sur votre ordinateur**, tout simplement. Fichier qui est là parce que OSX ou Linux l'ont de base, pas besoin de l'installer (encore heureux !). Mais du coup, où est-il ?

Pour le savoir, on peut utiliser la commande **`type`** :

```bash
type cat
# => cat is /bin/cat
```

On apprend donc que lorsqu'on tape `cat` dans le terminal, en réalité l'ordinateur va chercher ce programme dans le dossier `/bin` et va l'éxecuter. `/bin/cat` n'est donc qu'un simple fichier, qui a la particularité d'être **exécutable**. On peut s'en convaincre avec la commande suivante `ls` à nouveau :

```bash
ls -l /bin/cat
# => -rwxr-xr-x  1 root  wheel  23520 Dec  3 07:36 /bin/cat
```

On voit que que le fichier `/bin/cat` pèse 23520 octets (23 Ko), a été modifié pour la dernière fois le 3 décembre (le jour de l'installation d'OSX sur mon ordinateur). Il appartient à l'utilisateur `root` (on y reviendra), et il est executable par tous les utilisateurs (il y a trois `x` dans les permissions, on y reviendra aussi).

Je vous invite à ouvrir le terminal et tentez de localiser des programmes courants :

```bash
type ls
# => ls is an alias for ls -G
#    (J'ai zsh comme shell)

type -a ls
# => ls is an alias for ls -G
# => ls is /bin/ls
```

Un alias, c'est comme un raccourci de commande si vous voulez.

Alors pourquoi je parle de tout ça ? Quel est le rapport avec le titre de cette section, `$PATH` ? J'y viens.

Je vous ai dit que la commande `type` sert à révéler l'emplacement d'un programme sur le disque dur. Quand je tape `cat`, en fait j'éxecute le fichier `/bin/cat`. Très bien. Mais comment le terminal **sait-il où chercher ?** Comment sait-il que le fichier `cat` se trouve dans le dossier `/bin` ? Est-ce qu'il a parcouru tous les fichiers sur l'ordinateur ? Non ce serait un peu long...

En fait, le terminal utilise **la variable d'environnement `$PATH`** qui contient une **liste des dossiers** contenant des fichiers exécutables. Rien de magique donc. Affichons ensemble ce `$PATH` :

```bash
echo $PATH
# =>./bin:/usr/local/opt/rbenv/shims:/usr/local/opt/rbenv/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin

# Et de manière plus "jolie" :

echo $PATH | tr -s ':' '\n'
# => ./bin
# => /usr/local/opt/rbenv/shims
# => /usr/local/opt/rbenv/bin
# => /usr/local/bin
# => /usr/bin
# => /bin
# => /usr/sbin
# => /sbin
```

Cette liste se lit de gauche à droite (ou de haut en bas dans la version "jolie"). Ce qui est le plus à gauche (ou le plus haut) est **prioritaire**. En gros, quand vous tapez la commande `cat`, dans le terminal, l'ordinateur regarde dans le dossier `./bin` (un dossier `bin` dans le dossier courant, celui où vous vous trouvez actuellement dans le terminal, le `pwd` si vous préférez). S'il ne trouve pas de fichier `cat`, il passe au dossier suivant du `$PATH`, dans mon cas `/usr/local/opt/rbenv/shims`. Toujours rien ? Bien, dossier suivant, etc. ainsi de suite, jusqu'à arriver au dossier `/bin`, et là boum il trouve un fichier `cat`. Donc la recherche **s'arrête** et l'ordinateur va exécuter le programme contenu dans ce fameux fichier.

Ce `$PATH` est donc fondamental pour comprendre comment fonctionne votre terminal, comment il réagit et surtout pour **debugger les problèmes du type `command not found`**, où les problèmes où la **mauvaise version** d'un programme s'exécute (`git`, ou au hasard `ruby` !)

Ce `$PATH`, vous pouvez le modifier pour y ajouter des dossiers à vous. Par défaut, il va contenir les dossiers définis à [plusieurs endroits](http://superuser.com/a/69190). Dans le _setup_ du Wagon, on utilise **zsh** (plutôt que le traditionnel **bash**) en combinaison avec [Oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh). À chaque fois que vous lancez un terminal (une nouvelle fenêtre, un nouvel onglet), le fichier [`~/.zshrc`](https://github.com/lewagon/dotfiles/blob/master/zshrc) va être lu et exécuté. Dans ce fichier vous allez par exemple pouvoir y inclure une **modification** du `$PATH` en concaténant votre propre liste de dossiers et celle existante :

```bash
export PATH="./bin:${PATH}"
```

En faisant ça, j'étend le `$PATH`, en ajoutant un dossier où chercher. C'est très pratique ici pour Rails car tout projet Rails utilise a un dossier `bin` à la racine contenant un fichier `rails`. Du coup, la documentation de Rails explique qu'il faut toujours écrire `bin/rails` dès qu'on éxecute une commande (comme lancer le `server`, la `console`, ou un `generate`...). En ajoutant `./bin` au `$PATH`, plus besoin de préfixer la commande `rails` par `bin/`. Et vous avez la garantie que **la bonne version de rails** va être utilisée. De plus, rbenv vous garantit également que vous allez utiliser la bonne version de Ruby.

Si vous avez un problème de commande non reconnue, ou de mauvaise version, explorez votre `$PATH` et **dans quel ordre** les dossiers y sont. Vous devriez ainsi résoudre la plupart de vos soucis.





