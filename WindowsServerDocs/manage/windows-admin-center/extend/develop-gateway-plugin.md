---
title: Développer un plug-in de passerelle
description: Développer un plug-in de passerelle Windows Admin Center SDK (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93cee5b8e3611a264119947103d22d9aa3b9a56b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834390"
---
# <a name="develop-a-gateway-plugin"></a>Développer un plug-in de passerelle

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Un plug-in de la passerelle Windows Admin Center permet la communication de API à partir de l’interface utilisateur de votre outil ou une solution à un nœud cible.  Windows Admin Center héberge un service de passerelle qui relaie les commandes et des scripts à partir de plug-ins de la passerelle doit être exécuté sur les nœuds cibles. Le service de passerelle peut être étendu pour inclure les plug-ins de la passerelle personnalisée qui prennent en charge les protocoles autres que celles par défaut.

Ces plug-ins de passerelle sont inclus par défaut avec Windows Admin Center :

* Plug-in de la passerelle PowerShell
* Plug-in de la passerelle WMI

Si vous souhaitez communiquer avec un protocole autre que PowerShell ou WMI, comme avec REST, vous pouvez créer votre propre plug-in de la passerelle.  Plug-ins de la passerelle sont chargés dans un AppDomain séparé à partir du processus de passerelle existante, mais utilisent le même niveau d’élévation pour les droits.

> [!NOTE]
> Ne connaissent pas les types d’extension différents ? En savoir plus sur la [types d’architecture et l’extension d’extensibilité](understand-extensions.md).

## <a name="prepare-your-environment"></a>Préparer votre environnement

Si vous n’avez pas déjà fait, [préparer votre environnement](prepare-development-environment.md) en installant des dépendances et globales conditions préalables requises pour tous les projets.

## <a name="create-a-gateway-plugin-c-library"></a>Créer un plug-in de la passerelle (C# bibliothèque)

Pour créer un plug-in passerelle personnalisée, créez un nouveau C# classe qui implémente le ```IPlugIn``` de l’interface à partir de la ```Microsoft.ManagementExperience.FeatureInterfaces``` espace de noms.  

> [!NOTE]
> Le ```IFeature``` interface, disponible dans les versions antérieures du kit SDK, est désormais marqué comme obsolète.  Tout développement de plug-in de passerelle doit utiliser IPlugIn (ou éventuellement à la classe abstraite HttpPlugIn).

### <a name="download-sample-from-github"></a>Télécharger l’exemple à partir de GitHub

Pour commencer rapidement à utiliser un plug-in passerelle personnalisée, vous pouvez cloner ou télécharger une copie de notre [exemple C# plug-in project](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) à partir de notre kit de développement logiciel Windows Admin Center [site GitHub](https://aka.ms/wacsdk).

### <a name="add-content"></a>Ajoutez du contenu

Ajouter un nouveau contenu de votre copie clonée de la [exemple C# plug-in project](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) du fichier projet (ou votre propre projet) pour contenir vos API personnalisées, puis générez votre plug-in passerelle personnalisée DLL pour être utilisé dans les étapes suivantes.

### <a name="deploy-plugin-for-testing"></a>Déployer le plug-in de test

Tester votre plug-in passerelle personnalisée DLL en la chargeant dans le processus de passerelle Windows Admin Center.

Windows Admin Center recherche tous les plug-ins dans un ```plugins``` dossier dans le dossier Application Data de l’ordinateur actuel (à l’aide de la valeur CommonApplicationData de l’énumération Environment.SpecialFolder). Sur Windows 10, cet emplacement est ```C:\ProgramData\Server Management Experience```.  Si le ```plugins``` dossier n’existe pas encore, vous pouvez créer le dossier vous-même.

> [!NOTE]
> Vous pouvez remplacer l’emplacement du plug-in dans une version debug en mettant à jour la valeur de configuration « StaticsFolder ». Si vous déboguez localement, ce paramètre est dans le fichier App.Config de la solution de bureau. 

Dans le dossier de plug-ins (dans cet exemple, ```C:\ProgramData\Server Management Experience\plugins```)

* Créez un dossier portant le même nom que le ```Name``` valeur de propriété de la ```Feature``` dans votre plug-in passerelle personnalisée DLL (dans notre exemple de projet, le ```Name``` est « Exemple Uno »)
* Copiez votre fichier DLL de plug-in de passerelle personnalisée vers ce nouveau dossier
* Redémarrez le processus Windows Admin Center

Une fois que le processus d’administration de Windows redémarre, vous pourrez tester les API dans votre plug-in passerelle personnalisée DLL en émettant une commande GET, PUT, PATCH, DELETE ou publier sur ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```

### <a name="optional-attach-to-plugin-for-debugging"></a>Facultatif : Attacher au plug-in pour le débogage

Dans Visual Studio 2017, dans le menu Déboguer, sélectionnez « Attacher au processus ». Dans la fenêtre suivante, faites défiler la liste processus disponibles et sélectionnez SMEDesktop.exe, puis cliquez sur « Joindre ». Une fois le débogueur démarre, vous pouvez placer un point d’arrêt dans votre code de fonctionnalité et d’un exercice puis par le biais du format d’URL ci-dessus. Pour notre exemple de projet (nom de la fonctionnalité : « Exemple Uno ») l’URL est : « http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno»

## <a name="create-a-tool-extension-with-the-windows-admin-center-cli"></a>Créer une extension de l’outil avec l’interface CLI de Windows Admin Center ##

Nous devons maintenant créer une extension de l’outil à partir de laquelle vous pouvez appeler votre plug-in de passerelle personnalisée.  Créez ou accédez au dossier où vous souhaitez stocker vos fichiers projet, ouvrez une invite de commandes et configurez ce dossier comme répertoire de travail.  À l’aide de l’interface Windows Admin Center CLI qui a été précédemment installé, créez une nouvelle extension avec la syntaxe suivante :

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Value | Explication | Exemple |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nom de votre société (avec des espaces) | ```Contoso Inc``` |
| ```{!Tool Name}``` | Votre nom de l’outil (avec des espaces) | ```Manage Foo Works``` |

Voici un exemple d’utilisation :

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

Cette opération crée un nouveau dossier dans le répertoire de travail actuel dans le nom spécifié pour votre outil de copie tous les fichiers de modèle nécessaire dans votre projet et configure les fichiers avec le nom de votre entreprise et l’outil.  

Ensuite, changez de répertoire dans le dossier venez de créer, puis installer les dépendances requises locales en exécutant la commande suivante :

```
npm install
```

Une fois cette opération terminée, vous avez configuré tous les éléments que nécessaires pour charger votre nouvelle extension dans Windows Admin Center. 

## <a name="connect-your-tool-extension-to-your-custom-gateway-plugin"></a>Connecter votre extension de l’outil à votre plug-in passerelle personnalisée

Maintenant que vous avez créé une extension avec l’interface CLI de Windows Admin Center, vous êtes prêt à vous connecter votre extension de l’outil pour votre plug-in passerelle personnalisée, procédez comme suit :

- Ajouter un [module vide](guides\add-module.md)
- Utilisez votre [plug-in de la passerelle personnalisée](guides\use-custom-gateway-plugin.md) dans votre extension de l’outil
 
## <a name="build-and-side-load-your-extension"></a>Génération et côté chargent votre extension

Ensuite, build et côté chargent votre extension dans Windows Admin Center.  Ouvrez une fenêtre de commande, accédez à votre répertoire source, puis vous êtes prêt à créer.

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

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Cibler une version différente du Kit de développement Windows Admin Center

Il est facile de garder votre extension à jour avec les modifications apportées au SDK et les modifications de la plateforme.  En savoir plus sur comment [cibler une version différente](target-sdk-version.md) du Kit de développement Windows Admin Center.