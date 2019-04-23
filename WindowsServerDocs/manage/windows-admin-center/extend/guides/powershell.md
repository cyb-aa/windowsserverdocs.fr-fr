---
title: Utilisation de PowerShell dans votre extension
description: À l’aide de PowerShell dans votre extension pour le Kit de développement Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ae5004104150c510a56c06161c9280e029968298
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867600"
---
# <a name="using-powershell-in-your-extension"></a>Utilisation de PowerShell dans votre extension #

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Revenons plus approfondie dans le Kit de développement logiciel Windows Admin Center Extensions - parlons à présent ajouter des commandes PowerShell à votre extension.

## <a name="powershell-in-typescript"></a>PowerShell dans TypeScript ##

Le processus de génération gulp a une étape de génération qui prendra toute « .ps1 » qui est placé dans le dossier « / src/ressources/scripts » et les intégrer à la classe « scripts powershell » sous le dossier « / src/généré ».

>[!NOTE] 
> Ne pas mettre à jour manuellement, les fichiers « strings.ts » ni « powershell-scripts.ts ». Toute modification apportée est remplacée lors de la génération suivante.

### <a name="adding-your-own-powershell-script"></a>Ajout de votre propre script PowerShell ##

Nous allons ajouter plus d’informations sur l’ajout de votre propre script PowerShell bientôt.

### <a name="managing-powershell-sessions"></a>La gestion des sessions de PowerShell ###

Lorsque vous travaillez avec PowerShell dans Windows Admin Center, les sessions sont un composant obligatoire du processus de l’exécution de script. Pour exécuter des scripts sur les serveurs gérés à distance, PowerShell utilise des instances d’exécution. Windows Admin Center encapsule la création de l’instance d’exécution et la gestion dans un objet PowerShellSession pour gérer la durée de vie et permettre la réutilisation de l’instance d’exécution pour l’exécution du script séquentiel.

Chaque composant doit créer une référence à un objet de session est créée par la classe AppContextService à l’aide de trois options différentes :
<!-- I don't 100% get this part - it looks like you're adding 3 arguments - nodeName, <session key>, and <PowerShellSessionRequestOptions>. I got that from looking at the examples, not the text. We need to rework those paras explaining. -->
``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName);
```

En fournissant le nom du nœud à la méthode createSession, une nouvelle session PowerShell est créée, utilisée, puis immédiatement détruite à l’achèvement de l’appel de PowerShell. Cette fonctionnalité est valable pour les appels uniques, mais une utilisation répétée peut affecter les performances. Une session prend environ 1 seconde, donc en permanence recyclage sessions peut entraîner des ralentissements.

``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName, '<session key>');
```

Le premier paramètre facultatif est la \<clé de session\> paramètre. Cette opération crée une session à clé qui peut être recherchée et réutilisée, même à travers les composants (ce qui signifie que composant1 peut créer une session avec la clé « SME ROCKS » et Component2 peut utiliser cette même session).  

``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName, '<session key>', <PowerShellSessionRequestOptions>);
```
<!-- The second optional parameter is \<PowerShellSessionRequestOptions\> that does ... ? -->
### <a name="common-patterns"></a>Modèles courants ###

Dans la plupart des cas, créez une session à clé dans le **ngOnInit** (méthode) et puis supprimez-la dans un **ngOnDestroy**. Suivez ce modèle lorsqu’il y a plusieurs scripts PowerShell dans un composant, mais la session sous-jacente n’est pas partagé entre plusieurs composants.

Pour de meilleurs résultats, vérifiez que la création de session est gérée à l’intérieur des composants plutôt que des services - ce vous permet de vous assurer que durée de vie et le nettoyage peut être géré correctement.