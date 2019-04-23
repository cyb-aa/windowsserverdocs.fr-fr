# <a name="rename-or-delete-an-article"></a>Renommer ou supprimer un article

Avant de renommer ou supprimer un article, vous devez procédez comme suit pour éviter ou réduire le nombre de liens rompus. Les clients détestent les liens rompus et ne sont pas timide sur consonance off lorsqu’ils il est atteint un. Notez que, renommer ou supprimer un article, est la dernière étape de ce processus, pas la première.


## <a name="step-1-manage-inbound-links"></a>Étape 1 : Gérer les liens entrants

Déterminez s’il existe des liens entrants non-Microsoft vers votre contenu, tels que les blogs, forums et autres contenus sur le web. Contactez les propriétaires des blogs pour modifier ces liens et supprimer ou mettre à jour des liens à partir d’articles de forum. Outils d’analytique Web permet d’identifier les liens entrants à fort trafic que vous devrez peut-être gérer de cette façon.

## <a name="step-2-remove-all-crosslinks-to-the-article-from-the-windowsserverdocs-pr-repository"></a>Étape 2 : Supprimer tous les liens croisés à l’article à partir du référentiel WindowsServerDocs-pr

1. Actualiser votre branche – exécuter `git pull upstream <branch>`, ie pour actualiser à partir de master, exécutez `git pull upstream master`

2.  Analyse le dossier WindowsServerDocs-pr pour rechercher des articles qui ont un lien vers l’article que vous souhaitez renommer ou mettre hors service, puis mettre à jour les articles pour supprimer les liens ou remplacez-les par des liens vers un autre article. Vous pouvez utiliser une recherche et remplacez utilitaire pour rechercher les liens croisés si vous en avez pas installé. Si vous ne le faites, vous pouvez utiliser Windows PowerShell :

 a. Démarrez Windows PowerShell.

 b. À l’invite de PowerShell, accédez dans le dossier WindowsServerDocs-pr :

 `cd WindowsServerDocs-pr\WindowsServerDocs-pr`

 c. Exécutez une commande pour répertorier tous les fichiers qui contiennent un lien vers l’article à renommer ou supprimer :

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  Pour envoyer la liste des noms de fichiers dans un fichier texte (dans ce cas, nommé psoutput.txt), exécutez :

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. Ajouter et validez toutes vos modifications, envoyez-les à votre branchement et ouvrir une demande de tirage. Pour obtenir des instructions, consultez [commandes Git pour créer ou mettre à jour un article](git-steps-create-update-content.md).

## <a name="step-3-update-fwlinks"></a>Étape 3 : Mettre à jour les FWLinks

Recherchez l’outil FWLink les FWLinks qui pointent vers l’article. Pointez les FWLinks au contenu de remplacement ; Si vous n’êtes pas sur l’alias qui possède le lien, la joindre. Si les propriétaires ne sont pas mettre à jour le lien, soumettez un ticket avec MSCOM pour que le lien modifié. Plus d’informations - [wiki interne](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx).

## <a name="step-4-remove-crosslinks-to-the-article-from-table-of-contents"></a>Étape 4 : Supprimer des liens croisés à l’article de table des matières

Travailler avec la personne qui gère le fichier ToC.md. Ce fichier remplit la table de gauche du contenu de la bibliothèque technique. Si vous ne savez pas qui contacter, envoyez un e-mail à wssc-pra@microsoft.com.

## <a name="step-5-add-redirects"></a>Étape 5 : Ajouter des redirections
Si vous êtes changement de nom ou suppression d’un fichier, ajoutez une redirection afin que les liens existants n’enfreignent :

1. Laissez l’ancien fichier à l’emplacement existant avec le nom de fichier existant.
2. Remplacez le contenu dans le fichier avec ce morceau de métadonnées :
   ```
   ---
   redirect_url: <redirection-URL-or-file>
   ---
   ```
   \<URL-ou-fichier de redirection > est l’URL complète vers un autre emplacement, soit le chemin d’accès + nom du fichier à une autre rubrique dans le même dépôt OPS.

   Par exemple, cela serait la totalité du fichier :

   ```
   ---
   redirect_url: ../../failover-clustering/whats-new-in-failover-clustering
   ---
   ```

3. Après avoir créé une demande de tirage pour la redirection, cliquez sur les liens dans les commentaires de la demande de tirage - si la redirection fonctionne vous devraient accéder à la rubrique de la cible.

## <a name="step-6-rename-or-delete-the-article"></a>Étape 6 : Renommer ou supprimer l’article

Si vous n’utilisez pas une redirection, effectuez cette étape une fois que vous avez terminé les étapes précédentes et tous les articles concernés sont publiés. Si vous utilisez une redirection, renommer ou supprimer l’article serait annuler cette opération et provoquer un lien rompu. Pour renommer un fichier, il vous suffit de le renommer dans le système de fichiers, puis ajouter, valider et transmettre la modification, puis ouvrez une demande de tirage.
Pour supprimer un fichier, vous devez d’abord savoir qu’il ne fonctionne pas pour supprimer uniquement un fichier à partir de votre système de fichiers, car il s’agit d’un système de contrôle de code source et il doit connaître prévu pour ce faire. Sinon, vos fichiers supprimés réapparaîtront probablement.
Il existe deux manières de procéder :

- Système de fichiers et de git : Supprimez le fichier du système de fichiers. Puis, à partir de votre outil git, exécutez une des opérations suivantes  ```Git add -A``` | ```Git add --all``` | ```Git add -u```
- Git simplement :   Exécutez ```git rm foo.md```

    Plus d’informations [ http://stackoverflow.com/questions/2047465/how-can-i-delete-a-file-from-git-repo ](http://stackoverflow.com/questions/2047465/how-can-i-delete-a-file-from-git-repo) et [https://git-scm.com/docs/git-rm](https://git-scm.com/docs/git-rm) 

## <a name="step-7-find-and-fix-straggler-broken-links"></a>Étape 7 : Recherchez et corrigez les liens rompus suspecte

Utilisez l’outil d’assurance qualité contenu pour rechercher des liens rompus, que les étapes précédentes n’ont pas intercepter, puis supprimer ou réparer les liens.

## <a name="step-8-remove-cached-pages-from-search-engines"></a>Étape 8 : Supprimer les pages mises en cache à partir de moteurs de recherche

Accédez à ces pages web pour supprimer des pages web mises en cache à partir de moteurs de recherche : [Bing](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### <a name="contributors-guide-links"></a>Liens du guide du contributeur

- [Index des articles d’instructions](./contributor-guide-index.md)

