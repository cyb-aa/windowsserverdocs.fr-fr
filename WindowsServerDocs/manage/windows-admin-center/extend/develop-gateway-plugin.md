---
title: Développer un plug-in de passerelle
description: Développer un plug-in de passerelle SDK Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93cee5b8e3611a264119947103d22d9aa3b9a56b
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081156"
---
# Développer un plug-in de passerelle

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Un plug-in de passerelle Windows Admin Center permet une communication API à partir de l’interface utilisateur de votre outil ou une solution à un nœud cible.  Windows Admin Center héberge un service de passerelle qui relaie les commandes et les scripts de plug-ins de passerelle à exécuter sur des nœuds de la cible. Le service de passerelle peut être étendu pour inclure des plug-ins de passerelle personnalisé qui prennent en charge les protocoles autres que les styles par défaut.

Ces plug-ins de passerelle sont incluses par défaut avec Windows Admin Center:

* Plug-in de passerelle PowerShell
* Plug-in de passerelle WMI

Si vous souhaitez communiquer avec un protocole autre que PowerShell ou WMI, comme avec REST, vous pouvez créer votre propre plug-in de passerelle.  Plug-ins de passerelle sont chargés dans un AppDomain séparé à partir du processus de passerelle existant, mais utilisent le même niveau d’élévation des droits.

> [!NOTE]
> Pas familiarisé avec les types d’extension différente? En savoir plus sur les [types d’architecture et l’extension extensibilité](understand-extensions.md).

## Préparer votre environnement

Si vous n’avez pas déjà fait, [préparer votre environnement](prepare-development-environment.md) en installant les dépendances et les conditions préalables globales requis pour tous les projets.

## Créer un plug-in de passerelle (bibliothèque c#)

Pour créer un plug-in de passerelle personnalisé, créez une nouvelle classe c# qui implémente le ```IPlugIn``` d’interface à partir de la ```Microsoft.ManagementExperience.FeatureInterfaces``` espace de noms.  

> [!NOTE]
> Le ```IFeature``` interface, disponible dans les versions précédentes du SDK, est désormais signalée comme obsolète.  Tous les développements de plug-in de passerelle doivent utiliser IPlugIn (ou si vous le souhaitez la classe abstraite HttpPlugIn).

### Télécharger l’exemple à partir de GitHub

Pour commencer rapidement avec un plug-in de passerelle personnalisé, vous pouvez dupliquer ou télécharger une copie de notre [exemple c# plug-in de projet](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) à partir de notre [site GitHub](https://aka.ms/wacsdk)du SDK Windows Admin Center.

### Ajouter du contenu

Ajouter un nouveau contenu sur votre copie clonée du projet [exemple c# plug-in de projet](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) (ou de votre projet) pour contenir vos API personnalisé, puis créer votre fichier DLL de plug-in de passerelle personnalisées à utiliser dans les étapes suivantes.

### Déployer des plug-in de test

Testez votre plug-in de passerelle personnalisée DLL en chargeant dans le processus de passerelle Windows Admin Center.

Windows Admin Center recherche tous les plug-ins dans un ```plugins``` dossier dans le dossier de données d’Application de l’ordinateur en cours (à l’aide de la valeur de CommonApplicationData de l’énumération Environment.SpecialFolder). Sur Windows 10 cet emplacement est ```C:\ProgramData\Server Management Experience```.  Si le ```plugins``` dossier n’existe pas encore, vous pouvez créer le dossier vous-même.

> [!NOTE]
> Vous pouvez remplacer l’emplacement de plug-in dans une version de débogage en mettant à jour la valeur de configuration «StaticsFolder». Si vous déboguez localement, ce paramètre est dans le fichier App.Config de la solution de bureau. 

À l’intérieur du dossier Plug-ins (dans cet exemple, ```C:\ProgramData\Server Management Experience\plugins```)

* Créez un dossier portant le même nom que le ```Name``` valeur de propriété de la ```Feature``` dans votre plug-in de passerelle personnalisée DLL (dans notre exemple de projet, le ```Name``` est «Exemple Uno»)
* Copiez votre fichier DLL de plug-in de passerelle personnalisées dans ce nouveau dossier
* Redémarrer le processus de Windows Admin Center

Après le redémarrage du processus d’administration de Windows, vous serez en mesure d’exercer les API dans votre plug-in de passerelle personnalisée DLL en émettant une GET, placer, corriger, supprimer ou publier sur ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```

### Facultatif: Attacher au plug-in pour le débogage

Dans Visual Studio 2017, dans le menu Déboguer, sélectionnez «Attacher au processus». Dans la fenêtre suivante, faites défiler la liste des processus disponibles et sélectionnez SMEDesktop.exe, puis cliquez sur «Attacher». Une fois le démarrage du débogueur, vous pouvez placer un point d’arrêt dans votre code de la fonctionnalité et l’exercice puis par le biais du format d’URL ci-dessus. Pour notre exemple de projet (nom de la fonctionnalité: «Exemple Uno») est l’URL: «http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno»

## Créer une extension d’outil avec l’interface CLI de Windows Admin Center ##

Nous devons maintenant créer une extension d’outil à partir duquel vous pouvez appeler votre plug-in de passerelle personnalisée.  Créez ou accédez à un dossier où vous souhaitez stocker vos fichiers de projet, ouvrez une invite de commandes et la définition de ce dossier comme répertoire de travail.  À l’aide de Windows Admin Center CLI qui a été installé précédemment, créez une nouvelle extension avec la syntaxe suivante:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Valeur | Explication | Exemple |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nom de votre société (avec des espaces) | ```Contoso Inc``` |
| ```{!Tool Name}``` | Nom de votre outil (avec des espaces) | ```Manage Foo Works``` |

Voici un exemple d’utilisation:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

Cela crée un nouveau dossier à l’intérieur du répertoire de travail actuel en utilisant le nom que vous spécifiée pour votre outil de copie tous les fichiers de modèle nécessaires dans votre projet et configure les fichiers avec le nom de votre société et l’outil.  

Ensuite, modifiez le répertoire dans le dossier venez de créer et installer des dépendances locales nécessaires en exécutant la commande suivante:

```
npm install
```

Une fois cette opération terminée, vous avez configuré tout ce dont vous avez besoin pour charger votre nouvelle extension dans Windows Admin Center. 

## Connecter votre extension d’outil à votre plug-in de passerelle personnalisé

Maintenant que vous avez créé une extension avec l’interface CLI de Windows Admin Center, vous êtes prêt à connecter votre extension d’outil pour votre plug-in de passerelle personnalisé, en procédant comme suit:

- Ajouter un [module vide](guides\add-module.md)
- Utilisez votre [plug-in de passerelle personnalisés](guides\use-custom-gateway-plugin.md) dans votre extension d’outil
 
## Génération et côté chargent votre extension

Ensuite, build et côté chargent votre extension dans Windows Admin Center.  Ouvrez une fenêtre de commande, modifiez le répertoire à votre répertoire source, puis vous êtes prêt à générer.

* Créez et proposez avec gulp:

    ```
    gulp build
    gulp serve -p 4201
    ```

Notez que vous devez choisir un port actuellement disponible. Veillez à ne pas essayer d’utiliser le port sur lequel Windows Admin Center est en cours d’exécution.

Votre projet peut être chargé de manière indépendante dans une instance locale de Windows Admin Center pour les tests en s’attachant au projet servi localement dans Windows Admin Center.

* Lancez Windows Admin Center dans un navigateur web
* Ouvrez le débogueur (F12)
* Ouvrez la Console, puis tapez la commande suivante:

    ```
    MsftSme.sideLoad("http://localhost:4201")
    ```

*   Actualisez le navigateur web

Votre projet sera désormais visible dans la liste d'outils avec (side loaded) en regard du nom.

## Cibler une autre version du SDK Windows Admin Center

Il est facile de garder votre extension à jour avec les modifications du Kit de développement logiciel et les modifications de la plateforme.  En savoir plus sur la [cible une autre version](target-sdk-version.md) du SDK Windows Admin Center.