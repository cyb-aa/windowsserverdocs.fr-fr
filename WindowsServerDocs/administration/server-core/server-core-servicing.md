---
title: Mise à jour corrective de Server Core
description: Découvrez comment mettre à jour une installation Server Core de Windows Server
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: d670add6e4b4fc7369c48905bb297642ae07ff20
ms.sourcegitcommit: b7f55949f166554614f581c9ddcef5a82fa00625
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2019
ms.locfileid: "72588067"
---
# <a name="patch-a-server-core-installation"></a>Corriger une installation Server Core

> S’applique à : Windows Server 2019, Windows Server 2016 et Windows Server (canal semi-annuel)

Vous pouvez appliquer un correctif à un serveur exécutant une installation Server Core de l’une des manières suivantes :

- **Utilisation de Windows Update automatiquement ou avec Windows Server Update Services (WSUS)** . En utilisant Windows Update, soit automatiquement, soit à l’aide d’outils en ligne de commande ou Windows Server Update Services (WSUS), vous pouvez traiter des serveurs exécutant une installation Server Core.

- **Manuellement**. Même dans les organisations qui n’utilisent pas Windows Update ou WSUS, vous pouvez appliquer les mises à jour manuellement.

## <a name="view-the-updates-installed-on-your-server-core-server"></a>Afficher les mises à jour installées sur votre serveur Server Core
Avant d’ajouter une nouvelle mise à jour à Server Core, il est judicieux de voir quelles mises à jour ont déjà été installées.

Pour afficher les mises à jour à l’aide de Windows PowerShell, exécutez la fenêtre **obtenir un correctif**.

Pour afficher les mises à jour en exécutant une commande, exécutez **systeminfo. exe**. Il peut y avoir un bref délai pendant que l’outil inspecte votre système.

Vous pouvez également exécuter la **liste de correctifs WMI** à partir de la ligne de commande. 

## <a name="patch-server-core-automatically-with-windows-update"></a>Patch Server Core automatiquement avec Windows Update

Procédez comme suit pour corriger automatiquement le serveur avec Windows Update :

1. Vérifiez le paramètre de Windows Update actuel :
   ```
   %systemroot%\system32\Cscript %systemroot%\system32\scregedit.wsf /AU /v 
   ```

2. Pour activer les mises à jour automatiques :

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript %systemroot%\system32\scregedit.wsf /AU 4 
   Net start wuauserv
   ```  

3. Pour désactiver les mises à jour automatiques, exécutez :

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript %systemroot%\system32\scregedit.wsf /AU 1 
   Net start wuauserv 
   ```

Si le serveur est membre d’un domaine, vous pouvez également configurer Windows Update avec une stratégie de groupe. Pour plus d'informations, voir https://go.microsoft.com/fwlink/?LinkId=192470. Toutefois, lorsque vous utilisez cette méthode, seule l’option 4 (« téléchargement automatique et planification de l’installation ») s’applique aux installations Server Core en raison de l’absence d’interface graphique. Pour mieux contrôler quelles mises à jour doivent être installées, et à quel moment, vous pouvez utiliser un script qui fournit des équivalences de ligne de commande pour la plupart des commandes de l’interface graphique de Windows Update. Pour plus d’informations sur le script, consultez https://go.microsoft.com/fwlink/?LinkId=192471.

Pour forcer Windows Update à détecter et installer immédiatement les mises à jour disponibles, exécutez la commande suivante :

```
Wuauclt /detectnow 
```

Selon les mises à jour installées, vous pouvez être amené à redémarrer l’ordinateur, même si le système ne vous le demande pas. Pour déterminer si le processus d’installation est terminé, utilisez le gestionnaire des tâches pour vérifier que les processus du **programme d’installation approuvé** ou **wuauclt** ne sont pas en cours d’exécution. Vous pouvez également utiliser les méthodes dans [afficher les mises à jour installées sur votre serveur Server Core](#view-the-updates-installed-on-your-server-core-server) pour vérifier la liste des mises à jour installées.

## <a name="patch-the-server-with-wsus"></a>Corriger le serveur avec WSUS 

Si le serveur en mode d’installation minimale est membre d’un domaine, vous pouvez le configurer pour utiliser un serveur WSUS à l’aide d’une stratégie de groupe. Pour plus d’informations, téléchargez le [stratégie de groupe informations de référence](https://www.microsoft.com/download/details.aspx?id=25250). Vous pouvez également consulter [configurer les paramètres de stratégie de groupe pour mises à jour automatiques](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>Corriger manuellement le serveur

Téléchargez la mise à jour et rendez-la disponible pour l’installation Server Core.
À l’invite de commandes, exécutez la commande suivante :

```
Wusa <update>.msu /quiet 
```

Selon les mises à jour installées, vous pouvez être amené à redémarrer l’ordinateur, même si le système ne vous le demande pas.

Pour désinstaller une mise à jour manuellement, exécutez la commande suivante :

```
Wusa /uninstall <update>.msu /quiet 
```

