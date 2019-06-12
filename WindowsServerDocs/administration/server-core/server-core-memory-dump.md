---
title: Configurer les fichiers de vidage de mémoire pour l’installation de Server Core
description: Découvrez comment configurer les fichiers de vidage mémoire pour une installation Server Core de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 235df6f681de51a12f82b9fad019dd2db45fd486
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435550"
---
# <a name="configure-memory-dump-files-for-server-core-installation"></a>Configurer les fichiers de vidage de mémoire pour l’installation de Server Core

>S'applique à : Windows Server (canal semi-annuel) et Windows Server 2016

Utilisez les étapes suivantes pour configurer un vidage de mémoire pour votre installation de Server Core. 

## <a name="step-1-disable-the-automatic-system-page-file-management"></a>Étape 1 : Désactiver la gestion de fichier de page automatique du système

La première étape consiste à configurer manuellement vos options de défaillance et de récupération du système. Cela est nécessaire pour effectuer les étapes restantes.

Exécutez la commande suivante : 

```
wmic computersystem set AutomaticManagedPagefile=False
```
 
## <a name="step-2-configure-the-destination-path-for-a-memory-dump"></a>Étape 2 : Configurer le chemin d’accès de destination pour une image mémoire

Vous n’êtes pas obligé d’avoir le fichier d’échange sur la partition où est installé le système d’exploitation. Pour placer le fichier d’échange sur une autre partition, vous devez créer une nouvelle entrée de Registre nommée **DedicatedDumpFile**. Vous pouvez définir la taille du fichier d’échange à l’aide de la **DumpFileSize** entrée de Registre. Pour créer les entrées de Registre DedicatedDumpFile et DumpFileSize, procédez comme suit : 

1. À l’invite de commandes, exécutez le **regedit** commande pour ouvrir l’Éditeur du Registre.
2. Localisez la sous-clé de Registre suivante, puis cliquez dessus : HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl
3. Cliquez sur **Modifier > Nouveau > valeur de chaîne**.
4. Nommez la nouvelle valeur **DedicatedDumpFile**, puis appuyez sur ENTRÉE.
5. Avec le bouton droit **DedicatedDumpFile**, puis cliquez sur **modifier**.
6. Dans **données de la valeur** type  **\<lecteur\>:\\\<Dedicateddumpfile.sys\>** , puis cliquez sur **OK**.

   >[!NOTE] 
   > Remplacez \<lecteur\> avec un lecteur qui dispose de suffisamment de disque de l’espace du fichier d’échange, puis remplacez \<Dedicateddumpfile.dmp\> avec le chemin d’accès complet au fichier dédié.
 
7. Cliquez sur **Modifier > Nouveau > valeur DWORD**.
8. Type **DumpFileSize**, puis appuyez sur ENTRÉE.
9. Avec le bouton droit **DumpFileSize**, puis cliquez sur **modifier**.
10. Dans **Edit DWORD Value**, sous **Base**, cliquez sur **décimal**.
11. Dans **données de la valeur**, tapez la valeur appropriée, puis cliquez sur **OK**.
    >[!NOTE]
    > La taille du fichier de vidage est en mégaoctets (Mo).
12. Quittez l’Éditeur du Registre.

Après avoir déterminé l’emplacement de la partition de l’image mémoire, configurez le chemin d’accès de destination pour le fichier d’échange. Pour afficher le chemin d’accès de destination actuel pour le fichier d’échange, exécutez la commande suivante :

```
wmic RECOVEROS get DebugFilePath
```

La destination par défaut **DebugFilePath** est % systemroot%\memory.dmp. Pour modifier le chemin d’accès de destination en cours, exécutez la commande suivante :

```
wmic RECOVEROS set DebugFilePath = <FilePath>
```

Définissez \<FilePath\> pour le chemin d’accès de destination. Par exemple, la commande suivante définit le chemin d’accès de destination de vidage mémoire pour C:\WINDOWS\MEMORY. DMP : 

```
wmic RECOVEROS set DebugFilePath = C:\WINDOWS\MEMORY.DMP
```
 
## <a name="step-3-set-the-type-of-memory-dump"></a>Étape 3 : Définissez le type d’image mémoire

Déterminer le type de vidage de mémoire à configurer pour votre serveur. Pour afficher le type de vidage de mémoire actuel, exécutez la commande suivante :

```
wmic RECOVEROS get DebugInfoType
```

Pour modifier le type de vidage de mémoire actuel, exécutez la commande suivante : 

```
wmic RECOVEROS set DebugInfoType = <Value>
```

\<Valeur\> peut être 0, 1, 2 ou 3, tel que défini ci-dessous.

- 0: Désactiver la suppression d’un vidage de mémoire.
- 1 : Vidage mémoire complet. Enregistre tout le contenu de la mémoire système lorsque votre ordinateur après un arrêt inattendu. Une image mémoire complète peut contenir des données à partir de processus en cours d’exécution lorsque le vidage de mémoire a été collecté.
- 2 : Vidage de la mémoire du noyau (par défaut). Enregistre la mémoire du noyau uniquement. Cela accélère le processus d’enregistrement des informations dans un fichier journal lorsque votre ordinateur après un arrêt inattendu.
- 3 : Vidage de mémoire insuffisante. Enregistre le plus petit ensemble d’informations utiles qui peuvent aider à identifier la cause de l’arrêt inattendu.

## <a name="step-4-configure-the-server-to-restart-automatically-after-generating-a-memory-dump"></a>Étape 4 : Configurer le serveur redémarre automatiquement après avoir généré un vidage de mémoire

Par défaut, le serveur redémarre automatiquement une fois qu’il génère un vidage de mémoire. Pour afficher la configuration actuelle, exécutez la commande suivante :

```
wmic RECOVEROS get AutoReboot
```

Si la valeur de **AutoReboot** a la valeur TRUE, le serveur redémarre automatiquement après avoir généré un vidage de mémoire. Aucune configuration n’est nécessaire et vous pouvez passer à l’étape suivante.

Si la valeur de **AutoReboot** est FALSE, le serveur ne redémarre pas automatiquement. Exécutez la commande suivante pour modifier la valeur :

```
wmic RECOVEROS set AutoReboot = true
```
 
## <a name="step-5-configure-the-server-to-overwrite-the-existing-memory-dump-file"></a>Étape 5 : Configurer le serveur pour remplacer le fichier de vidage de mémoire existant

Par défaut, le serveur remplace le fichier de vidage de mémoire existant lorsqu’un nouveau est créé. Pour déterminer si les fichiers de vidage mémoire existants sont déjà configurés pour être remplacé, exécutez la commande suivante :

```
wmic RECOVEROS get OverwriteExistingDebugFile
```

Si la valeur est 1, le serveur remplace le fichier de vidage de mémoire existant. Aucune configuration n’est nécessaire, et vous pouvez passer à l’étape suivante.

Si la valeur est 0, le serveur ne remplace pas le fichier de vidage de mémoire existant. Exécutez la commande suivante pour modifier la valeur : 

```
wmic RECOVEROS set OverwriteExistingDebugFile = 1
```
 
## <a name="step-6-set-an-administrative-alert"></a>Étape 6 : Définir une alerte d’administration

Déterminer si une alerte d’administration est appropriée et définissez **SendAdminAlert** en conséquence. Pour afficher la valeur actuelle pour SendAdminAlert, exécutez la commande suivante :

```
wmic RECOVEROS get SendAdminAlert
```

Les valeurs possibles pour SendAdminAlert ont la valeur TRUE ou FALSE. Pour modifier la valeur SendAdminAlert existante sur true, exécutez la commande suivante : 

```
wmic RECOVEROS set SendAdminAlert = true
```
 
## <a name="step-7-set-the-memory-dumps-page-file-size"></a>Étape 7 : Définir la taille de fichier d’échange de l’image mémoire

Pour vérifier les paramètres de fichier de page actuels, exécutez une des commandes suivantes :

   ```
   wmic.exe pagefile
   ```

   ou

   ```
   wmic.exe pagefile list /format:list
   ```

Par exemple, exécutez la commande suivante pour configurer les tailles initiales et maximales de votre fichier d’échange :

```
wmic pagefileset where name="c:\\pagefile.sys" set InitialSize=1000,MaximumSize=5000
```

## <a name="step-8-configure-the-server-to-generate-a-manual-memory-dump"></a>Étape 8 : Configurer le serveur pour générer une image mémoire manuelle

Vous pouvez générer manuellement un vidage de mémoire à l’aide d’un clavier PS/2. Cette fonctionnalité est désactivée par défaut, et il n’est pas disponible pour les claviers USB Universal Serial Bus ().

Pour activer la mémoire manuelle fait un dump à l’aide d’un clavier PS/2, exécutez la commande suivante :

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\i8042prt\Parameters /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
```

Pour déterminer si la fonctionnalité a été activée correctement, exécutez la commande suivante :

```
Reg query HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ i8042prt \ Parameters / v CrashOnCtrlScroll
```

Vous devez redémarrer le serveur pour que les modifications entrent en vigueur. Vous pouvez redémarrer le serveur en exécutant la commande suivante :

```
Shutdown / r / t 0
```

Vous pouvez générer des vidages sur incident manuelle de la mémoire avec un clavier PS/2 qui est connecté à votre serveur en maintenant la touche CTRL de droite tout en appuyant sur la touche Arrêt défil deux fois. Cela rend l’ordinateur à cocher avec le code d’erreur 0xE2 de bogues. Après avoir redémarré le serveur, un nouveau fichier de vidage s’affiche dans le chemin d’accès de destination que vous avez établi à l’étape 2.

## <a name="step-9-verify-that-memory-dump-files-are-being-created-correctly"></a>Étape 9 : Vérifiez les fichiers créés sont correctement ce vidage de la mémoire

Vous pouvez utiliser l’utilitaire dumpchk.exe pour vérifier que les fichiers de vidage de mémoire sont créées correctement. L’utilitaire dumpchk.exe n’est pas installé avec l’option d’installation Server Core, vous devez donc l’exécuter à partir d’un serveur avec l’expérience de bureau ou à partir de Windows 10. En outre, les outils de débogage pour les produits Windows doivent être installés.  

L’utilitaire dumpchk.exe vous permet de transférer le fichier de vidage de mémoire à partir de votre installation de Server Core de Windows Server 2008 à l’autre ordinateur à l’aide du support de votre choix.

> [!WARNING]
> Fichiers de page peuvent être très volumineux, soigneusement prendre en compte le transfert de méthode et les ressources qui requiert de méthode.
 

Références supplémentaires

Pour obtenir des informations générales sur l’utilisation de fichiers de vidage de mémoire, consultez [options de vue d’ensemble du fichier de vidage mémoire pour Windows](https://support.microsoft.com/help/254649/overview-of-memory-dump-file-options-for-windows).

Pour plus d’informations sur les fichiers de vidage dédié, consultez [l’utilisation de la valeur de Registre DedicatedDeumpFile pour surmonter les limitations d’espace sur le lecteur système lors de la capture d’une image de la mémoire système](https://blogs.msdn.microsoft.com/ntdebugging/2010/04/02/how-to-use-the-dedicateddumpfile-registry-value-to-overcome-space-limitations-on-the-system-drive-when-capturing-a-system-memory-dump/).



