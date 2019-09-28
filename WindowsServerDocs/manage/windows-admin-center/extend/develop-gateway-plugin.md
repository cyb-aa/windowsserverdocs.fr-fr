---
title: Développer un plug-in de passerelle
description: Développer un plug-in de passerelle Kit de développement logiciel (SDK) Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 2b096b226190ad1ca3fd07c38b7b939d019ee30f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406931"
---
# <a name="develop-a-gateway-plugin"></a>Développer un plug-in de passerelle

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Un plug-in de passerelle du centre d’administration Windows active la communication d’API à partir de l’interface utilisateur de votre outil ou solution vers un nœud cible.  Le centre d’administration Windows héberge un service de passerelle qui relaie les commandes et les scripts des plug-ins de passerelle à exécuter sur les nœuds cibles. Le service de passerelle peut être étendu pour inclure des plug-ins de passerelle personnalisés qui prennent en charge des protocoles autres que ceux par défaut.

Ces plug-ins de passerelle sont inclus par défaut dans le centre d’administration Windows :

* Plug-in PowerShell Gateway
* Plug-in de passerelle WMI

Si vous souhaitez communiquer avec un autre protocole que PowerShell ou WMI, comme avec REST, vous pouvez créer votre propre plug-in de passerelle.  Les plug-ins de passerelle sont chargés dans un AppDomain distinct du processus de passerelle existant, mais utilisent le même niveau d’élévation pour les droits.

> [!NOTE]
> Vous n’êtes pas familiarisé avec les différents types d’extensions ? En savoir plus sur l' [architecture et les types d’extension d’extensibilité](understand-extensions.md).

## <a name="prepare-your-environment"></a>Préparer votre environnement

Si vous ne l’avez pas déjà fait, [Préparez votre environnement](prepare-development-environment.md) en installant des dépendances et des prérequis globaux requis pour tous les projets.

## <a name="create-a-gateway-plugin-c-library"></a>Créer un plug-inC# de passerelle (bibliothèque)

Pour créer un plug-in de passerelle personnalisé, C# créez une nouvelle classe qui implémente l’interface ```IPlugIn``` à partir de l’espace de noms ```Microsoft.ManagementExperience.FeatureInterfaces```.  

> [!NOTE]
> L’interface ```IFeature```, disponible dans les versions antérieures du kit de développement logiciel (SDK), est désormais marquée comme obsolète.  Tout le développement de plug-ins de passerelle doit utiliser IPlugIn (ou éventuellement la classe abstraite HttpPlugIn).

### <a name="download-sample-from-github"></a>Télécharger l’exemple à partir de GitHub

Pour commencer rapidement avec un plug-in de passerelle personnalisé, vous pouvez cloner ou télécharger une copie de notre [ C# exemple de projet de plug-in](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) à partir de notre [site github](https://aka.ms/wacsdk)du centre d’administration Windows.

### <a name="add-content"></a>Ajoutez du contenu

Ajoutez un nouveau contenu à votre copie clonée de [l' C# exemple](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) de projet de projet de plug-in (ou votre propre projet) pour contenir vos API personnalisées, puis générez votre fichier dll de plug-in de passerelle personnalisé à utiliser dans les étapes suivantes.

### <a name="deploy-plugin-for-testing"></a>Déployer le plug-in pour le test

Testez votre DLL de plug-in de passerelle personnalisée en la chargeant dans le processus de passerelle du centre d’administration Windows.

Le centre d’administration Windows recherche tous les plug-ins dans un dossier ```plugins``` dans le dossier Application Data de l’ordinateur actuel (à l’aide de la valeur CommonApplicationData de l’énumération Environment. SpecialFolder). Sur Windows 10, cet emplacement est ```C:\ProgramData\Server Management Experience```.  Si le dossier ```plugins``` n’existe pas encore, vous pouvez créer le dossier vous-même.

> [!NOTE]
> Vous pouvez remplacer l’emplacement du plug-in dans une version Debug en mettant à jour la valeur de configuration « StaticsFolder ». Si vous effectuez un débogage localement, ce paramètre se trouve dans le fichier app. config de la solution Desktop. 

Dans le dossier plugins (dans cet exemple, ```C:\ProgramData\Server Management Experience\plugins```)

* Créez un nouveau dossier portant le même nom que la valeur de propriété ```Name``` de la ```Feature``` dans votre DLL de plug-in de passerelle personnalisée (dans notre exemple de projet, le ```Name``` est « Sample Uno »)
* Copiez votre fichier DLL de plug-in de passerelle personnalisé dans ce nouveau dossier
* Redémarrer le processus du centre d’administration Windows

Après le redémarrage du processus d’administration de Windows, vous pouvez tester les API dans votre DLL de plug-in de passerelle personnalisée en émettant une opération d’extraction, de mise en place, de correction, de suppression ou de publication à ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```

### <a name="optional-attach-to-plugin-for-debugging"></a>Facultatif : Attacher au plug-in pour le débogage

Dans Visual Studio 2017, dans le menu Déboguer, sélectionnez « Attacher au processus ». Dans la fenêtre suivante, faites défiler la liste processus disponibles et sélectionnez SMEDesktop. exe, puis cliquez sur « attacher ». Une fois le débogueur démarré, vous pouvez placer un point d’arrêt dans le code de votre fonctionnalité, puis utiliser le format d’URL ci-dessus. Pour notre exemple de projet (nom de la fonctionnalité : « Sample Uno ») l’URL est : « <http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno> »

## <a name="create-a-tool-extension-with-the-windows-admin-center-cli"></a>Créer une extension d’outil avec l’interface de commande du centre d’administration Windows ##

Nous devons maintenant créer une extension d’outil à partir de laquelle vous pouvez appeler votre plug-in de passerelle personnalisé.  Créez ou accédez à un dossier dans lequel vous souhaitez stocker vos fichiers de projet, ouvrez une invite de commandes, puis définissez ce dossier comme répertoire de travail.  À l’aide de l’interface de commande du centre d’administration Windows qui a été installée précédemment, créez une nouvelle extension avec la syntaxe suivante :

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Value | Explication | Exemple |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nom de votre société (avec des espaces) | ```Contoso Inc``` |
| ```{!Tool Name}``` | Votre nom d’outil (avec des espaces) | ```Manage Foo Works``` |

Voici un exemple d’utilisation :

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

Cela crée un nouveau dossier dans le répertoire de travail actuel en utilisant le nom que vous avez spécifié pour votre outil, copie tous les fichiers de modèle nécessaires dans votre projet et configure les fichiers avec le nom de votre entreprise et de votre outil.  

Ensuite, accédez au dossier que vous venez de créer, puis installez les dépendances locales requises en exécutant la commande suivante :

```
npm install
```

Une fois cette opération terminée, vous avez configuré tout ce dont vous avez besoin pour charger votre nouvelle extension dans le centre d’administration Windows. 

## <a name="connect-your-tool-extension-to-your-custom-gateway-plugin"></a>Connecter votre extension d’outil à votre plug-in de passerelle personnalisé

Maintenant que vous avez créé une extension avec l’interface de commande du centre d’administration Windows, vous êtes prêt à connecter votre extension d’outil à votre plug-in de passerelle personnalisé, en procédant comme suit :

- Ajouter un [module vide](guides/add-module.md)
- Utiliser le [plug-in de votre passerelle personnalisée](guides/use-custom-gateway-plugin.md) dans votre extension d’outil
 
## <a name="build-and-side-load-your-extension"></a>Créez et chargez votre extension

Ensuite, créez et chargez votre extension dans le centre d’administration Windows.  Ouvrez une fenêtre de commande, accédez au répertoire source, puis vous êtes prêt à générer.

* Créez et proposez avec gulp :

    ```
    gulp build
    gulp serve -p 4201
    ```

Notez que vous devez choisir un port actuellement disponible. Veillez à ne pas essayer d’utiliser le port sur lequel Windows Admin Center est en cours d’exécution.

Votre projet peut être chargé de manière indépendante dans une instance locale de Windows Admin Center pour les tests en s’attachant au projet servi localement dans Windows Admin Center.

* Lancez Windows Admin Center dans un navigateur web
* Ouvrez le débogueur (F12)
* Ouvrez la Console, puis tapez la commande suivante :

    ```
    MsftSme.sideLoad("http://localhost:4201")
    ```

*   Actualisez le navigateur web

Votre projet sera désormais visible dans la liste d'outils avec (side loaded) en regard du nom.

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Cibler une version différente du kit de développement logiciel (SDK) du centre d’administration Windows

Il est facile de tenir à jour votre extension avec les modifications du SDK et les modifications de la plateforme.  Découvrez comment [cibler une version différente](target-sdk-version.md) du kit de développement logiciel (SDK) du centre d’administration Windows.