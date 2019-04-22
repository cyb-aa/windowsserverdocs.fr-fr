---
title: Mise à jour corrective de Server Core
description: Découvrez comment mettre à jour une installation Server Core de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: f51ffae5ed8f91cca386eb209e7a1d8cc664ceeb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817350"
---
# <a name="patch-a-server-core-installation"></a>Corriger une installation Server Core

> S’applique à : Windows Server (canal semi-annuel) et Windows Server 2016

Vous pouvez appliquer un correctif un serveur exécutant l’installation Server Core de plusieurs manières :

- **À l’aide de Windows Update automatiquement ou avec Windows Server Update Services (WSUS)**. À l’aide de Windows Update, automatiquement ou avec des outils de ligne de commande ou Windows Server Update Services (WSUS), vous pouvez service serveurs exécutant une installation Server Core.

- **Manuellement**. Même dans les organisations qui n’utilisent pas Windows update ou WSUS, vous pouvez appliquer des mises à jour manuellement.

## <a name="view-the-updates-installed-on-your-server-core-server"></a>Afficher les mises à jour installées sur votre serveur Server Core
Avant d’ajouter une nouvelle mise à jour de Server Core, il est judicieux de voir quelles mises à jour ont déjà été installés.

Pour afficher les mises à jour à l’aide de Windows PowerShell, exécutez **Get-Hotfix**.

Pour afficher les mises à jour en exécutant une commande, exécutez **systeminfo.exe**. Il peut y avoir un léger retard pendant que l’outil inspecte votre système.

Vous pouvez également exécuter **wmic qfe liste** à partir de la ligne de commande. 

## <a name="patch-server-core-automatically-with-windows-update"></a>Correctif de Server Core automatiquement avec la mise à jour de Windows

Utilisez les étapes suivantes pour appliquer le correctif automatiquement avec la mise à jour de Windows :

1. Vérifier le paramètre actuel de la mise à jour de Windows :
   ```
   %systemroot%\system32\Cscript scregedit.wsf /AU /v 
   ```

2. Pour activer les mises à jour automatiques :

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 4 
   Net start wuauserv
   ```  

3. Pour désactiver les mises à jour automatiques, exécutez :

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 1 
   Net start wuauserv 
   ```

Si le serveur est membre d’un domaine, vous pouvez également configurer Windows Update avec une stratégie de groupe. Pour plus d'informations, consultez https://go.microsoft.com/fwlink/?LinkId=192470. Toutefois, lorsque vous utilisez cette méthode, seule l’option 4 (« téléchargement automatique et planification de l’installation ») concerne les installations Server Core en raison de l’absence d’une interface graphique. Pour mieux contrôler quelles mises à jour doivent être installées, et à quel moment, vous pouvez utiliser un script qui fournit des équivalences de ligne de commande pour la plupart des commandes de l’interface graphique de Windows Update. Pour plus d’informations sur le script, consultez https://go.microsoft.com/fwlink/?LinkId=192471.

Pour forcer Windows Update à détecter et installer immédiatement les mises à jour disponibles, exécutez la commande suivante :

```
Wuauclt /detectnow 
```

Selon les mises à jour installées, vous pouvez être amené à redémarrer l’ordinateur, même si le système ne vous le demande pas. Pour déterminer si le processus d’installation est terminée, utilisez le Gestionnaire des tâches pour vérifier que le **Wuauclt** ou **installateur approuvé** processus ne sont pas en cours d’exécution. Vous pouvez également utiliser les méthodes dans [afficher les mises à jour installées sur votre serveur Server Core](#view-the-updates-installed-on-your-Server-Core-server) pour vérifier la liste des mises à jour installées.

## <a name="patch-the-server-with-wsus"></a>Appliquer le correctif avec WSUS 

Si le serveur en mode d’installation minimale est membre d’un domaine, vous pouvez le configurer pour utiliser un serveur WSUS à l’aide d’une stratégie de groupe. Pour plus d’informations, téléchargez le [les informations de référence de stratégie de groupe](https://www.microsoft.com/download/details.aspx?id=25250). Vous pouvez également consulter [configurer des paramètres de stratégie de groupe pour les mises à jour automatiques](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>Appliquer le correctif manuellement

Télécharger la mise à jour et le rendre disponible à l’installation de Server Core.
À l’invite de commandes, exécutez la commande suivante :

```
Wusa <update>.msu /quiet 
```

Selon les mises à jour installées, vous pouvez être amené à redémarrer l’ordinateur, même si le système ne vous le demande pas.

Pour désinstaller une mise à jour manuellement, exécutez la commande suivante :

```
Wusa /uninstall <update>.msu /quiet 
```

