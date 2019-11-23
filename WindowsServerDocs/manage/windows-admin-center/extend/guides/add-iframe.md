---
title: Ajouter un élément iFrame à une extension d'outil
description: Développer une extension d’outil Kit de développement logiciel (SDK) du centre d’administration Windows (projet Honolulu)-ajouter un iFrame à une extension d’outil
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 0833b2fd92f2bf4b512120783bb71295a3112745
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406892"
---
# <a name="add-an-iframe-to-a-tool-extension"></a>Ajouter un élément iFrame à une extension d'outil

>S’applique à : Windows Admin Center, Windows Admin Center Preview

Dans cet article, nous allons ajouter un iFrame à une nouvelle extension d’outil vide que nous avons créée à l’aide de l’interface de commande du centre d’administration Windows.

## <a name="prepare-your-environment"></a>Préparer votre environnement ##

Si vous ne l’avez pas déjà fait, suivez les instructions fournies dans [développer une extension d’outil](../develop-tool.md) pour préparer votre environnement et créer une extension d’outil vide.

## <a name="add-a-module-to-your-project"></a>Ajouter un module à votre projet ##

Ajoutez un nouveau [module vide](add-module.md) à votre projet, auquel nous ajouterons un IFRAME à l’étape suivante.  

## <a name="add-an-iframe-to-your-module"></a>Ajouter un iFrame à votre module ##

Nous allons maintenant ajouter un iFrame à ce nouveau module vide que nous venons de créer.

Dans \src\app\, accédez à votre dossier de module, puis ouvrez le ```{!module-name}.component.html```de fichiers, qui se trouve avec la Convention d’affectation de noms suivante :

| Valeur | Explication | Exemple de nom de fichier |
| ----- | ----------- | ------- |
| ```{!module-name}``` | Nom de votre module (minuscules, espaces remplacés par des tirets) | ```manage-foo-works-portal.component.html``` |
    
Ajoutez le contenu suivant au fichier HTML :

``` html
<div>
  <iframe  style="height: 850px;" src="https://www.bing.com"></iframe>
</div>
```

C’est-ce que vous avez ajouté un iFrame à votre extension.  Ensuite, vous pouvez [créer et charger](../develop-tool.md#build-and-side-load-your-extension) votre extension dans le centre d’administration Windows pour voir les résultats.

> [!Note]
> Les paramètres de stratégie de sécurité de contenu (CSP) peuvent empêcher le rendu de certains sites dans un iFrame au sein du centre d’administration Windows. Vous pouvez en savoir plus à ce sujet [ici](https://content-security-policy.com/). 
