<properties pageTitle="Commandes GIT pour créer un nouvel article ou la mise à jour un article existant" description="Étapes pour créer et mettre à jour un article dans WindowsServerDocs-pr." metaKeywords="" services="" solutions="" documentationCenter="" authors="Kathy Davies" videoId="" scriptId="" manager="dongill" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="08/24/16" ms.author="kathydav" />

# <a name="git-commands-to-create-or-update-an-article"></a>Commandes GIT pour créer ou mettre à jour un article

>! REMARQUE : Ces commandes supposent que vous avez configuré Github pour spécifier le référentiel par défaut où vous extrayez des fichiers. Dans Github est la terminologie, où vous extrayez les fichiers à partir de votre *en amont*. Où vous envoyez des fichiers à est votre *origine*. Selon la façon dont notre référentiel et le flux de travail sont conçues, votre en amont doit être défini sur notre référentiel, ce qui se trouve sous l’organisation Microsoft - https://github.com/Microsoft/WindowsServerDocs-pr et votre origine doit être votre duplication de ce dépôt, sous votre propre compte Github. Par exemple, Kathy est https://github.com/KBDAzure/WindowsServerDocs-pr 

>Pour vérifier vos paramètres, tapez ```git config -l```. Examinez les URL pour vous assurer qu’ils font référence à l’emplacement prévu.

## <a name="add-or-update-an-article"></a>Ajouter ou mettre à jour un article

Voici comment créer une branche locale, enregistrez vos modifications et les placer dans votre fourche distante.

1. Démarrez Git Bash (ou l’outil de ligne de commande que vous utilisez pour Git).

2. Modifiez WindowsServerDocs-pr :

        cd WindowsServerDocs-pr

3. Il est préférable de conserver votre maître local branche et le maître distant une branche dans votre fourche synchronisée avec principal du dépôt branche. Cela peut aider à gagner beaucoup de frustration et perdu de temps--vous êtes plus susceptible d’intercepter tôt les problèmes et de simplifier les choses en cours de fonctionnement correctes, connues. Exécutez :

        git checkout master
        git pull upstream master
        git push origin master

4. Maintenant vous êtes prêt à créer une branche pour effectuer votre quotidienne ou en fonction du livrable fonctionne. La meilleure façon de créer une branche de travail locale à partir d’une branche de version consiste à utiliser pour exécuter :

        git checkout upstream/upstream-branch-name -b your-local-branch-name

   Cela crée la branche locale à partir de la branche en amont et vous permet d’éviter la fusion des fichiers erronés dans votre nouvelle branche locale. Par exemple, pour créer une branche de travail en fonction de la branche de seuil de la disponibilité générale, vous pouvez exécuter une commande comme suit :
      
        git checkout upstream/master -b working-11-18

   Si vous travaillez sur le contenu qui doit être fusionné dans la branche qui n’est pas         

5. Ajoutez la branche de travail local vers votre branche :

        git push origin <working branch>

6. Créez un article ou apporter des modifications à un article existant. Utilisez l’Explorateur Windows pour ouvrir des fichiers markdown et votre éditeur markdown pour créer et modifier des fichiers. Pour une aide mise en forme de base, consultez ce [article](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/) dans Github.

7. Ajouter les métadonnées requises et les informations de version, en fonction de [contrôle de version de produit et de métadonnées](metadata-OSversioning-and-trademarks.md).

8. Vérifier l’état, puis ajoutez et validez vos modifications :

        git status

   Passez en revue les résultats s’assurer que les fichiers répertoriés sont ceux que vous avez modifié. Si tous les fichiers semblent exacts, exécutez cette commande pour ajouter tous les fichiers :

        git add .
        git commit –m "<comment>"

   Pour ajouter uniquement les fichiers spécifiques (par exemple, si ```git status``` répertorie les fichiers que vous ne souhaitez pas envoyer), au lieu de cela, vous devez exécuter :

        git add <file path>
        git commit –m "<comment>"

   >[!IMPORTANT]
   >La commande ```git add .``` ajoute toutes les modifications en attente signalées par ```git status```. Cela signifie que si ```git status``` affiche les mises à jour non suivis que vous ne souhaitez pas ajouter, utilisez ```git add <file path>``` à la place.  

9. Mettez à jour votre branche de travail local avec les modifications en amont :

        git pull upstream master

10. Envoyez les modifications à votre branchement sur GitHub :

        git push origin <working branch>

## <a name="submit-your-changes"></a>Soumettez vos modifications

Lorsque vous êtes prêt à envoyer votre contenu pour intermédiaire, de validation ou de publication, utilisez l’UI GitHub pour créer une demande de tirage. 

Lorsque vous ouvrez une demande de tirage (PR), cela déclenche une passe de test, génère le projet et publie sur notre site intermédiaire interne. Il est OK ouvrir une demande de tirage que vous n’êtes pas prêt à les avoir fusionné, car il s’agit comment obtenir une étape de test et vérifier vos mises à jour sur le site intermédiaire. Détails de la build et les liens de préproduction sont publiées sous forme de commentaires dans la demande de tirage. 

Il vous incombe effectuer les opérations suivantes **avant de lui demander d’avoir vos modifications fusionnées**:
  - Révision de build détails pour vous assurer qu’il ne contient aucune erreur. 
  - Passez en revue vos mises à jour sur le site intermédiaire.

Une fois que vous avez fait, indiquer qu’il en soit :
- Ajout de l’étiquette « prêt pour la fusion » à la demande de tirage. \(Cliquez sur **étiquettes** ou l’icône d’engrenage à droite du flux de commentaires dans la demande de tirage.)
- Ajout de prêt pour la fusion en tant que commentaire et envoyer un e-mail à l’alias WSSC extraire réviseurs : wssc-pra

Le réviseur de demande de tirage vérifie vos modifications et accepte la demande de tirage, sauf s’il existe des problèmes ou questions. Des commentaires ou des demandes pour résoudre les problèmes sont ajoutés sous forme de commentaires. Passez en revue [les critères de qualité pour extraction demander une revue](contributor-guide-pr-criteria.md) pour savoir ce qui est attendu.

## <a name="publishing"></a>Publication

- Articles sont publiés à environ 10 h 00 et 3 h 00 heure du Pacifique, du lundi au vendredi. N’oubliez pas qu’un réviseur de demande de tirage a besoin de temps pour consulter et accepter vos modifications avant qu’ils peuvent fusionnées. Modifications doivent être fusionnées pour être récupéré lors du prochain cycle de publication planifié. Si vous avez besoin d’un article publié pour un cycle de publication spécifique, indiquez un réviseur de demande de tirage avance. Lorsque votre demande de tirage est accepté, vos modifications sont fusionnées dans le référentiel et figureront dans la prochaine publication de temps s’exécuter. Il peut prendre jusqu'à 30 minutes pour les articles apparaissent en ligne après la publication. 
