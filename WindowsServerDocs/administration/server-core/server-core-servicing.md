---
title: Mise à jour corrective Server Core
description: Découvrez comment mettre à jour une installation Server Core de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: f51ffae5ed8f91cca386eb209e7a1d8cc664ceeb
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1448037"
---
# <a name="patch-a-server-core-installation"></a>Une installation Server Core de correctif

> S’applique à: Windows Server (Channel annuel séparées) et Windows Server 2016

Vous pouvez appliquer un correctif un serveur exécutant l’installation Server Core de la manière suivante:

- **Utilisation de Windows mise à jour automatiquement ou Windows Server Update Services (WSUS)**. À l’aide de Windows Update, soit automatiquement ou dans les outils de ligne de commande ou Windows Server Update Services (WSUS), vous pouvez service les serveurs exécutant une installation Server Core.

- **Manuellement**. Même dans les organisations qui n’utilisent pas Windows update ou WSUS, vous pouvez appliquer des mises à jour manuellement.

## <a name="view-the-updates-installed-on-your-server-core-server"></a>Afficher les mises à jour installés sur votre serveur Server Core
Avant d’ajouter une nouvelle mise à jour pour le serveur principal, il est préférable de voir les mises à jour ont déjà été installées.

Pour afficher les mises à jour à l’aide de Windows PowerShell, exécutez **Get-correctif**.

Pour afficher les mises à jour en exécutant une commande, exécutez **systeminfo.exe**. Il peut y avoir un bref délai pendant que l’outil inspecte votre système.

Vous pouvez également exécuter la **ligne de commande WMI qfe liste** à partir de la ligne de commande. 

## <a name="patch-server-core-automatically-with-windows-update"></a>Correctif Server Core automatiquement avec Windows Update

Pour appliquer le correctif automatiquement avec Windows Update, procédez comme suit:

1. Vérifiez la valeur actuelle de la mise à jour Windows:
   ```
   %systemroot%\system32\Cscript scregedit.wsf /AU /v 
   ```

2. Pour activer les mises à jour automatiques:

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 4 
   Net start wuauserv
   ```  

3. Pour désactiver les mises à jour automatiques, exécutez:

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 1 
   Net start wuauserv 
   ```

Si le serveur est un membre d’un domaine, vous pouvez également configurer Windows Update à l’aide de la stratégie de groupe. Pour plus d’informations, voir https://go.microsoft.com/fwlink/?LinkId=192470. Toutefois, lorsque vous utilisez cette méthode, seule option 4 («téléchargement automatique et planification des installations») concerne les installations Server Core en raison de l’absence d’une interface graphique. Pour plus de contrôle sur les mises à jour sont installées et quand, vous pouvez utiliser un script qui fournit un équivalent de ligne de commande de la plupart de l’interface graphique Windows Update. Pour plus d’informations sur le script, voir https://go.microsoft.com/fwlink/?LinkId=192471.

Pour forcer la mise à jour de Windows pour détecter et installer les mises à jour disponibles immédiatement, exécutez la commande suivante:

```
Wuauclt /detectnow 
```

Selon les mises à jour sont installés, vous devrez peut-être redémarrer l’ordinateur, bien que le système ne vous informera pas. Pour déterminer si le processus d’installation est terminée, utilisez le Gestionnaire des tâches pour vérifier que les processus **Wuauclt** ou **Approuvé le programme d’installation** ne sont pas en cours d’exécution. Vous pouvez également utiliser les méthodes en [mode les mises à jour installé sur votre serveur Server Core](#view-the-updates-installed-on-your-Server-Core-server) pour vérifier la liste des mises à jour installées.

## <a name="patch-the-server-with-wsus"></a>Correctif le serveur WSUS 

Si le serveur de base de serveur est un membre d’un domaine, vous pouvez le configurer pour utiliser un serveur WSUS avec la stratégie de groupe. Pour plus d’informations, téléchargez les [informations de référence de la stratégie de groupe](https://www.microsoft.com/download/details.aspx?id=25250). Vous pouvez également consulter la [Configuration des paramètres de stratégie de groupe pour les mises à jour automatiques](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>Appliquer le correctif manuellement

Télécharger la mise à jour et le rendre disponible à l’installation Server Core.
À l’invite de commandes, exécutez la commande suivante:

```
Wusa <update>.msu /quiet 
```

Selon les mises à jour sont installés, vous devrez peut-être redémarrer l’ordinateur, bien que le système ne vous informera pas.

Pour désinstaller une mise à jour manuellement, exécutez la commande suivante:

```
Wusa /uninstall <update>.msu /quiet 
```

