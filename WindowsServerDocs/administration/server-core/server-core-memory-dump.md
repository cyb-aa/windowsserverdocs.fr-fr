---
title: Configurer des fichiers de vidage de la mémoire pour une installation Server Core
description: Découvrez comment configurer des fichiers de vidage mémoire pour une installation Server Core de Windows Server
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 4f1baa52fc9f0ebfe8afae35d86b7a7238d56223
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383388"
---
# <a name="configure-memory-dump-files-for-server-core-installation"></a>Configurer des fichiers de vidage de la mémoire pour une installation Server Core

>S’applique à : Windows Server 2019, Windows Server 2016 et Windows Server (canal semi-annuel)

Procédez comme suit pour configurer un vidage de la mémoire pour votre installation Server Core. 

## <a name="step-1-disable-the-automatic-system-page-file-management"></a>Étape 1 : Désactiver la gestion automatique des fichiers de pagination système

La première étape consiste à configurer manuellement vos options de récupération et d’échec du système. Cela est nécessaire pour effectuer les étapes restantes.

Exécutez la commande suivante : 

```
wmic computersystem set AutomaticManagedPagefile=False
```
 
## <a name="step-2-configure-the-destination-path-for-a-memory-dump"></a>Étape 2 : Configurer le chemin d’accès de destination pour une image mémoire

Vous n’avez pas besoin de disposer du fichier d’échange sur la partition où le système d’exploitation est installé. Pour placer le fichier d’échange sur une autre partition, vous devez créer une nouvelle entrée de Registre nommée **DedicatedDumpFile**. Vous pouvez définir la taille du fichier d’échange à l’aide de l’entrée de Registre **DumpFileSize** . Pour créer les entrées de Registre DedicatedDumpFile et DumpFileSize, procédez comme suit : 

1. À l’invite de commandes, exécutez la commande **regedit** pour ouvrir l’éditeur du Registre.
2. Localisez la sous-clé de Registre suivante, puis cliquez dessus : HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl
3. Cliquez sur **modifier > nouvelle valeur de chaîne de >** .
4. Nommez la nouvelle valeur **DedicatedDumpFile**, puis appuyez sur entrée.
5. Cliquez avec le bouton droit sur **DedicatedDumpFile**, puis cliquez sur **modifier**.
6. Dans **valeur** , tapez **\<Drive @ no__t-3 : @no__t -4\<Dedicateddumpfile.sys @ no__t-6**, puis cliquez sur **OK**.

   >[!NOTE] 
   > Remplacez \<Drive @ no__t-1 par un lecteur disposant d’un espace disque suffisant pour le fichier d’échange et remplacez @no__t -2Dedicateddumpfile. dmp @ no__t-3 par le chemin d’accès complet au fichier dédié.
 
7. Cliquez sur **modifier > nouvelle > Valeur DWORD**.
8. Tapez **DumpFileSize**, puis appuyez sur entrée.
9. Cliquez avec le bouton droit sur **DumpFileSize**, puis cliquez sur **modifier**.
10. Dans **modifier la valeur DWORD**, sous **base**, cliquez sur **décimal**.
11. Dans **données**de la valeur, tapez la valeur appropriée, puis cliquez sur **OK**.
    >[!NOTE]
    > La taille du fichier de vidage est exprimée en mégaoctets (Mo).
12. Quittez l’éditeur du Registre.

Après avoir déterminé l’emplacement de la partition de l’image mémoire, configurez le chemin d’accès de destination du fichier d’échange. Pour afficher le chemin d’accès de destination actuel du fichier d’échange, exécutez la commande suivante :

```
wmic RECOVEROS get DebugFilePath
```

La destination par défaut pour **DebugFilePath** est%SystemRoot%\Memory.dmp. Pour modifier le chemin d’accès de destination actuel, exécutez la commande suivante :

```
wmic RECOVEROS set DebugFilePath = <FilePath>
```

Définissez \<FilePath @ no__t-1 sur le chemin d’accès de destination. Par exemple, la commande suivante définit le chemin d’accès de destination de l’image mémoire sur C:\WINDOWS\MEMORY. DMP 

```
wmic RECOVEROS set DebugFilePath = C:\WINDOWS\MEMORY.DMP
```
 
## <a name="step-3-set-the-type-of-memory-dump"></a>Étape 3 : Définir le type de vidage de la mémoire

Déterminez le type d’image mémoire à configurer pour votre serveur. Pour afficher le type de vidage de la mémoire actuel, exécutez la commande suivante :

```
wmic RECOVEROS get DebugInfoType
```

Pour modifier le type de vidage de la mémoire actuel, exécutez la commande suivante : 

```
wmic RECOVEROS set DebugInfoType = <Value>
```

\<Value @ no__t-1 peut être 0, 1, 2 ou 3, comme indiqué ci-dessous.

- 0 : Désactive la suppression d’un vidage de la mémoire.
- 1 : Image mémoire complète. Enregistre tout le contenu de la mémoire système lorsque votre ordinateur s’arrête de manière inattendue. Une image mémoire complète peut contenir des données des processus qui étaient en cours d’exécution lors de la collecte de l’image mémoire.
- 2 : Image mémoire du noyau (par défaut). Enregistre la mémoire du noyau uniquement. Cela accélère le processus d’enregistrement des informations dans un fichier journal lorsque votre ordinateur s’arrête de manière inattendue.
- 3 : Image mémoire réduite. Enregistre le plus petit ensemble d’informations utiles qui peuvent aider à identifier la raison pour laquelle votre ordinateur s’est arrêté de manière inattendue.

## <a name="step-4-configure-the-server-to-restart-automatically-after-generating-a-memory-dump"></a>Étape 4 : Configurer le serveur pour qu’il redémarre automatiquement après la génération d’un vidage de la mémoire

Par défaut, le serveur redémarre automatiquement après avoir généré un vidage de la mémoire. Pour afficher la configuration actuelle, exécutez la commande suivante :

```
wmic RECOVEROS get AutoReboot
```

Si la valeur de **redémarrage** automatique est true, le serveur redémarre automatiquement après la génération d’une image mémoire. Aucune configuration n’est nécessaire et vous pouvez passer à l’étape suivante.

Si la valeur de **redémarrage** automatique est false, le serveur ne redémarre pas automatiquement. Exécutez la commande suivante pour modifier la valeur :

```
wmic RECOVEROS set AutoReboot = true
```
 
## <a name="step-5-configure-the-server-to-overwrite-the-existing-memory-dump-file"></a>Étape 5 : Configurer le serveur pour remplacer le fichier de vidage de mémoire existant

Par défaut, le serveur remplace le fichier de vidage de mémoire existant lorsqu’un nouveau fichier est créé. Pour déterminer si des fichiers de vidage de la mémoire existants sont déjà configurés pour être remplacés, exécutez la commande suivante :

```
wmic RECOVEROS get OverwriteExistingDebugFile
```

Si la valeur est 1, le serveur remplacera le fichier de vidage de mémoire existant. Aucune configuration n’est nécessaire et vous pouvez passer à l’étape suivante.

Si la valeur est 0, le serveur ne remplace pas le fichier de vidage de mémoire existant. Exécutez la commande suivante pour modifier la valeur : 

```
wmic RECOVEROS set OverwriteExistingDebugFile = 1
```
 
## <a name="step-6-set-an-administrative-alert"></a>Étape 6 : Définir une alerte administrative

Déterminez si une alerte administrative est appropriée et définissez **SendAdminAlert** en conséquence. Pour afficher la valeur actuelle de SendAdminAlert, exécutez la commande suivante :

```
wmic RECOVEROS get SendAdminAlert
```

Les valeurs possibles pour SendAdminAlert sont TRUE ou FALSe. Pour remplacer la valeur SendAdminAlert existante par true, exécutez la commande suivante : 

```
wmic RECOVEROS set SendAdminAlert = true
```
 
## <a name="step-7-set-the-memory-dumps-page-file-size"></a>Étape 7 : Définir la taille du fichier d’échange de l’image mémoire

Pour vérifier les paramètres actuels du fichier d’échange, exécutez l’une des commandes suivantes :

   ```
   wmic.exe pagefile
   ```

   ou

   ```
   wmic.exe pagefile list /format:list
   ```

Par exemple, exécutez la commande suivante pour configurer la taille initiale et la taille maximale de votre fichier d’échange :

```
wmic pagefileset where name="c:\\pagefile.sys" set InitialSize=1000,MaximumSize=5000
```

## <a name="step-8-configure-the-server-to-generate-a-manual-memory-dump"></a>Étape 8 : Configurer le serveur pour générer une image mémoire manuelle

Vous pouvez générer manuellement un vidage de la mémoire à l’aide d’un clavier PS/2. Cette fonctionnalité est désactivée par défaut et n’est pas disponible pour les claviers USB (Universal Serial Bus).

Pour activer les images mémoire manuelles à l’aide d’un clavier PS/2, exécutez la commande suivante :

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\i8042prt\Parameters /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
```

Pour déterminer si la fonctionnalité a été activée correctement, exécutez la commande suivante :

```
Reg query HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ i8042prt \ Parameters / v CrashOnCtrlScroll
```

Vous devez redémarrer le serveur pour que les modifications prennent effet. Vous pouvez redémarrer le serveur en exécutant la commande suivante :

```
Shutdown / r / t 0
```

Vous pouvez générer des images mémoire manuelles à l’aide d’un clavier PS/2 connecté à votre serveur en maintenant la touche CTRL enfoncée tout en appuyant deux fois sur la touche Arrêt défil. Cela rend le contrôle de bogue de l’ordinateur avec le code d’erreur 0xE2. Après le redémarrage du serveur, un nouveau fichier de vidage apparaît dans le chemin d’accès de destination que vous avez établi à l’étape 2.

## <a name="step-9-verify-that-memory-dump-files-are-being-created-correctly"></a>Étape 9 : Vérifier que les fichiers de vidage de la mémoire sont correctement créés

Vous pouvez utiliser le utlity Dumpchk. exe pour vérifier que les fichiers de vidage de la mémoire sont correctement créés. L’utilitaire Dumpchk. exe n’est pas installé avec l’option d’installation Server Core. vous devez donc l’exécuter à partir d’un serveur doté de l’expérience utilisateur ou de Windows 10. En outre, les outils de débogage pour les produits Windows doivent être installés.  

L’utilitaire Dumpchk. exe vous permet de transférer le fichier de vidage de la mémoire de votre installation Server Core de Windows Server 2008 vers l’autre ordinateur à l’aide du support de votre choix.

> [!WARNING]
> Les fichiers d’échange peuvent être très volumineux. par conséquent, réfléchissez soigneusement à la méthode de transfert et aux ressources requises par la méthode.
 

Références supplémentaires

Pour obtenir des informations générales sur l’utilisation des fichiers de vidage de mémoire, consultez [vue d’ensemble des options de fichier d’image mémoire pour Windows](https://support.microsoft.com/help/254649/overview-of-memory-dump-file-options-for-windows).

Pour plus d’informations sur les fichiers de vidage dédiés, consultez la page [utilisation de la valeur de Registre DedicatedDeumpFile pour surmonter les limitations d’espace sur le lecteur système lors de la capture d’un vidage de la mémoire système](https://blogs.msdn.microsoft.com/ntdebugging/2010/04/02/how-to-use-the-dedicateddumpfile-registry-value-to-overcome-space-limitations-on-the-system-drive-when-capturing-a-system-memory-dump/).



