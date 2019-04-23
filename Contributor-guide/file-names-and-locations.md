<properties title="" pageTitle="Noms de fichiers et emplacements des articles techniques Windows Server 2016" description="Explique la structure de fichiers pour les articles et les conventions d’affectation de noms, que vous devez suivre lorsque vous créez un nouvel article." metaKeywords="" services="" solutions="" documentationCenter="" authors="Kathy-Davies" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="03/14/2016" ms.author="jimpark; tysonn" />

# <a name="file-names-and-locations-for-windows-server-technical-articles"></a>Noms de fichiers et emplacements des articles techniques Windows Server

Connaître et en suivant les règles vous permettent d’obtenir votre demande de tirage accepté plus rapidement.

+ [règles]
+ [Pattern]
+ [Approbation du nom de fichier]

## <a name="rules"></a>Règles

- Tous les fichiers doivent être au format markdown et utiliser l’extension de fichier .md.
- Garder les noms de fichiers, si possible aux mots de 3 à 5 - 10 mots est vraiment trop long pour une meilleure lisibilité et moteurs de recherche.
- Utiliser des minuscules et uniquement des lettres, des chiffres et des traits d’union.
- Aucune espaces ou des caractères de ponctuation. Utilisez un trait d’union pour séparer les mots et les nombres dans le nom de fichier.
- Utiliser la forme abrégée de verbes d’action, pas le '-ing' écran : `deploy-nano-server` pas `Deploying-Nano-Server`
- Laisser de côté petits mots, tels qu’un et, le, en, ou.
- L’orthographe des mots éviter les acronymes inutiles ou non approuvés dans les noms de fichiers
- Les noms de fichier doivent être uniques - au lieu de `overview.md` utiliser `storage-spaces-overview.md`

Acronymes et les abréviations dans les noms de fichier - recommandations spécifiques :

- Suivez les conseils de Microsoft existant pour les abréviations de noms acceptable
- Les abréviations standard du secteur sont acceptables si nécessaire dans les noms de fichiers.

## <a name="pattern"></a>Motif

Passez en revue la liste des articles dans le référentiel pour avoir une idée des noms existants. Voici le modèle général :

 **component-topic-title.md**
 
Par exemple, `storage-spaces-direct-overview.md`.

## <a name="file-name-approval"></a>Approbation du nom de fichier

Lorsque vous envoyez un nouveau fichier, réviseurs des demandes de tirage Vérifiez le nom et fournissent des commentaires via le flux de commentaires de demande de tirage si des modifications sont nécessaires. Le nom de fichier doit être corrigé avant que la demande de tirage est acceptée. Les contributeurs peuvent facilement transmettre la mise à jour à la demande de tirage en attente.

## <a name="folders-in-the-repo"></a>Dossiers dans le référentiel

Utilisez la structure de dossiers existante. Ne créez pas les dossiers sans obtenir l’approbation à partir d’un administrateur de dépôt. Communiquer avec eux si vous pensez que vous avez besoin d’un nouveau dossier.

Le dépôt GitHub doit disposer d’un simple et relativement plates structure de dossiers – \<zone >\\\<technologie > : pour exemple storage\data-la déduplication. Cela présente les avantages suivants :
 - Il est très simple.
 - Il est plus proche de fonctionnement Azure.com
 - Il facilite pour les writers/PMs à ne trouver rapidement leurs rubriques – aucun nécessaire de faire des recherches dans les autres dossiers recherchez où une technologie particulière est imbriquée.
 - Il conserve l’URL court, ce qui convient pour les moteurs de recherche et l’expérience utilisateur, les clients parfois examiner l’URL pour les piles.

## <a name="changing-case-in-file-names"></a>Changement de casse dans les noms de fichiers

Les systèmes d’exploitation Windows respectent la casse. Si vous avez besoin de modifier un nom de fichier pour corriger la casse, il est préférable d’apporter une modification au contenu du fichier, sauf si vous êtes sur Linux ou Mac. Exemple :

  biztalk-administration-and-Development-Task-List-in-BizTalk-Services --> biztalk-services-administration-and-development-task-list

Utilisez le `git mv` commande pour renommer un fichier :
```
  git mv <WindowsServerDocs/tech-area/subarea/current-file-name.md> <WindowsServerDocs/tech-area/subarea/new-file-name.md>
```

### <a name="contributors-guide-links"></a>Liens du Guide du contributeur

- [Index des articles d’instructions](./contributor-guide-index.md)


<!--Anchors-->
[règles]: #rules
[Pattern]: #pattern
[Approbation du nom de fichier]: #file-name-approval
