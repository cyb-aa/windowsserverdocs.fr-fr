---
title: Ajouter un élément iFrame à une extension d'outil
description: 'Développer une extension de l’outil Kit de développement Windows Admin Center (projet Honolulu) : ajout d’un iFrame avec une extension de l’outil'
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: da3850b75a0e069f9153d3c66baef9f00b67d61c
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452589"
---
# <a name="add-an-iframe-to-a-tool-extension"></a>Ajouter un élément iFrame à une extension d'outil

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Dans cet article, nous allons ajouter un iFrame avec une extension de l’outil vide que nous avons créé avec l’interface CLI de Windows Admin Center.

## <a name="prepare-your-environment"></a>Préparer votre environnement ##

Si vous n’avez pas déjà, suivez les instructions de [développer une extension de l’outil](../develop-tool.md) pour préparer votre environnement et créer un nouveau, vide l’extension de l’outil.

## <a name="add-a-module-to-your-project"></a>Ajouter un module à votre projet ##

Ajouter un nouveau [module vide](add-module.md) à votre projet, auquel nous ajouterons un iFrame dans l’étape suivante.  

## <a name="add-an-iframe-to-your-module"></a>Ajouter un iFrame à votre module ##

Nous allons maintenant ajouter un iFrame pour ce module vide que nous venons de créer.

Dans \src\app\, Parcourir dans votre dossier de module, puis ouvrez le fichier ```{!module-name}.component.html```, elle est trouvée avec la convention d’affectation de noms suivante :

| Value | Explication | Exemple de nom de fichier |
| ----- | ----------- | ------- |
| ```{!module-name}``` | Nom de votre module (minuscules, espaces remplacés par des tirets) | ```manage-foo-works-portal.component.html``` |
    
Ajoutez le contenu suivant au fichier html :

``` html
<div>
  <iframe  style="height: 850px;" src="https://www.bing.com"></iframe>
</div>
```

Voilà, vous avez ajouté un iFrame à votre extension.  Ensuite, vous pouvez [générer et côté charge](../develop-tool.md#build-and-side-load-your-extension) votre extension dans Windows Admin Center pour afficher les résultats.

> [!Note]
> Paramètres de stratégie de sécurité (CSP) de contenu peut empêcher certains sites de rendu dans un iFrame dans Windows Admin Center. Vous pouvez en savoir plus à ce sujet [ici](https://content-security-policy.com/). 
