---
title: Ajouter un iFrame pour une extension d’outil
description: Développer une extension d’outil SDK Windows Admin Center (projet Honolulu) - ajouter un iFrame pour une extension d’outil
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 7b4a7b688e4b2d9f52e44395c19211b91b964578
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080926"
---
# Ajouter un iFrame pour une extension d’outil

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Dans cet article, nous allons ajouter un iFrame pour une extension d’outil vide que nous avons créé avec l’interface CLI de Windows Admin Center.

## Préparer votre environnement ##

Si vous n’avez pas déjà, suivez les instructions de [développer une extension d’outil](..\develop-tool.md) pour préparer votre environnement et de créer une extension d’outil vide.

## Ajouter un module à votre projet ##

Ajoutez un nouveau [module vide](add-module.md) à votre projet, à laquelle nous allons ajouter un iFrame dans l’étape suivante.  

## Ajouter un iFrame à votre module ##

Maintenant, nous allons ajouter un iFrame à ce module vide que nous venons de créer.

Dans \src\app\, recherchez dans votre dossier de module, puis ouvrez le fichier ```{!module-name}.component.html```, qui respecte la convention d’affectation de noms suivante:

| Valeur | Explication | Exemple de nom de fichier |
| ----- | ----------- | ------- |
| ```{!module-name}``` | Nom de votre module (minuscules, espaces remplacés par des tirets) | ```manage-foo-works-portal.component.html``` |
    
Dans le fichier html, ajoutez le contenu suivant:

``` html
<div>
  <iframe  style="height: 850px;" src="https://www.bing.com"></iframe>
</div>
```

Par conséquent, vous avez ajouté un iFrame à votre extension.  Ensuite, vous pouvez [build et côté charge](..\develop-tool.md#build-and-side-load-your-extension) votre extension dans Windows Admin Center pour afficher les résultats.
