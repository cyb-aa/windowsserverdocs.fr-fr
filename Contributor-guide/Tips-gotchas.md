# <a name="tips-and-gotchas"></a>Conseils et pièges

Si vous débutez avec Git, il peut être frustrant de formation travailler efficacement. Informations collectées ici par conséquent, il est plus facile à trouver que si celle-ci est incluse dans les autres informations dans ce guide.

## <a name="basics"></a>Principes de base :

Commandes de Guide de ce collaborateur supposent que vous avez configuré Github pour spécifier le référentiel par défaut où vous extrayez des fichiers. Dans Github est la terminologie, où vous extrayez les fichiers à partir de votre *en amont*. Où vous envoyez des fichiers à est votre *origine*. 

Raison de sur la façon dont notre référentiel et le flux de travail sont conçues, votre en amont doit être défini sur notre référentiel, ce qui se trouve sous l’organisation Microsoft - https://github.com/Microsoft/WindowsServerDocs-pr et votre origine doit être votre duplication de ce dépôt, ce qui se trouve sous votre propre compte Github. Par exemple, https://github.com/MY-GITHUB-ALIAS/WindowsServerDocs-pr. 

>Pour vérifier vos paramètres, tapez ```git config -l```. Examinez les URL pour vous assurer qu’ils font référence à l’emplacement prévu.

Conseils de branchement de base :

-  Travailler dans les branches, pas dans votre copie locale du serveur principal. Cela rend plus facile à résoudre des problèmes dans vos branches et revenir en arrière à un point correcte, connu.

-  Baser vos branches locales sur une branche dans le référentiel partagé, pas sur votre fourche distante. Ensuite, vous allez transmettre vos branches locales à votre fourche distante. À partir de votre fourche distante, vous devez demander aux mises à jour de votre fourche fusionnée dans le référentiel partagé. Les commandes dans cet article vous montrent comment effectuer cette opération.

-  Lorsque vous créez une branche locale, votre commande indique à Git quels branche que vous souhaitez baser votre nouvelle branche sur. Il s’agit quand et comment vous spécifiez master ou une branche de jalon ou une fonctionnalité. Par exemple, cette commande crée une nouvelle branche à partir d’une branche en amont et puis extrait la nouvelle branche, vous êtes donc prêt à l’utiliser :

        git checkout upstream/upstream-branch-name -b your-local-branch-name

Pour l’aide de markdown, commencez par cela [article](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/) dans Github. Pour les tables, consultez ce [un](https://help.github.com/articles/organizing-information-with-tables/).

## <a name="gotchas"></a>Pièges

### <a name="deleting-files"></a>Suppression de fichiers
Il ne fonctionne pas pour supprimer uniquement un fichier à partir de votre système de fichiers. Utilisez les commandes Git pour informer le système vous conçue pour ce faire, sinon vos fichiers supprimés deviennent des fichiers zombie qui réapparaissent. Et jamais au bon moment. Après tout, il s’agit d’un système de contrôle de code source, et si vous ne lui que sûr de vouloir supprimer ces fichiers, il permet les ajoute pour vous. Merci, Git ! Pour obtenir des instructions, consultez [renommer ou mettre hors service un article](rename-r-retire.md).

### <a name="merge-conflicts"></a>Conflits de fusion

Si vous obtenez un conflit de fusion pendant que vous travaillez localement, vous devez soit corriger ou vers l’arrière en tirer. La plupart du temps il est préférable de simplement les corriger, sauf si elles ne sont pas vos fichiers. Gardez à l’esprit si vous ne faites rien, vous pouvez ne pas envoyer vos modifications à votre origine, afin d’avoir à faire à quelque chose.

**Comment la corriger** --Guide Azure propose plus d’informations à ce sujet et des instructions pour résoudre le conflit. **Une demande de tirage avec conflits de fusion ne doit jamais être accepté**. Veillez à indiquer le nom de notre référentiel et les branches dans tous les exemples lorsque vous examinez le [instructions](https://microsoft.sharepoint.com/teams/azurecontentguidance/wiki/Pages/Resolve%20a%20local%20merge%20conflict%20yourself.aspx). 

**Comment annuler** : si les conflits sont dans des fichiers vous n’êtes pas propriétaire ou il existe une raison que vous ne peut pas les corriger à l’heure, ici comment revenir en arrière. Il réinitialise votre jeu de fichiers local à comment elles étaient avant le conflit de fusion s’est produite. Exécutez la commande suivante :

```git merge --abort```

Si vous n’avez pas intermédiaire et validé votre travail en local, vous pouvez le faire maintenant. Toutefois, si vous envoyez ces modifications et que vous ouvrez une demande de tirage, le conflit réapparaîtront probablement si elle était dans les fichiers avec les modifications. Vous devrez résoudre le problème ou obtenir de l’aide de quelqu'un d’autre à résoudre le problème, avant de pouvoir accepter la demande. Pour cette raison, il est préférable d’utiliser uniquement la méthode pour vous débloquer si vous avez un travail urgent, par exemple une mise à jour critique.

