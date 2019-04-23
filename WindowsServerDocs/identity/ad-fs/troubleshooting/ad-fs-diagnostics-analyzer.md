---
title: Analyseur de diagnostics de l'aide AD FS
description: Ce document décrit l’Analyseur de Diagnostics aident à AD FS et comment elle peut effectuer le basic vérifie à l’aide du module PowerShell de diagnostic AD FS.
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: anandyadavMSFT
ms.date: 02/13/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a5b5f895a0575094f8f1af950bde82e1d56325b2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829120"
---
# <a name="ad-fs-help-diagnostics-analyzer"></a>Analyseur de diagnostics de l'aide AD FS

AD FS a un grand nombre de paramètres qui prennent en charge la large gamme de fonctionnalités qu’il propose pour le développement de l’authentification et l’application. Pendant le dépannage, il est recommandé de vous assurer que tous les paramètres AD FS sont configurés correctement. Effectuer une vérification manuelle de ces paramètres peut parfois prendre du temps. Analyseur de Diagnostics aident à AD FS peut aider à effectuer les vérifications de base à l’aide du module PowerShell de ADFSToolbox. Après avoir effectué les vérifications, aide AD FS fournit le [Diagnostics analyseur](https://aka.ms/adfsdiagnosticsanalyzer) pour vous aider à facilement visualiser les résultats et offrent des étapes de correction.

L’opération complète prend 3 étapes simples :

1. Programme d’installation du module ADFSToolbox sur le serveur AD FS principal ou un serveur WAP
2. Exécuter les tests de diagnostic et de charger le fichier à l’aide AD FS
3. Afficher l’analyse de diagnostic et résoudre les problèmes éventuels

Accédez à [Analyseur de Diagnostics aident à AD FS (https://aka.ms/adfsdiagnosticsanalyzer) ](https://aka.ms/adfsdiagnosticsanalyzer) pour démarrer la résolution des problèmes.

![Outil d’analyseur de diagnostic d’AD FS sur l’aide AD FS](media/ad-fs-diagonostics-analyzer/home.png)

## <a name="step-1-setup-the-adfstoolbox-module-on-ad-fs-server"></a>Étape 1 : Programme d’installation du module ADFSToolbox sur le serveur AD FS

Pour exécuter le [Diagnostics analyseur](https://aka.ms/adfsdiagnosticsanalyzer), vous devez installer le module PowerShell de ADFSToolbox. Si le serveur AD FS est connecté à internet, vous pouvez installer le module ADFSToolbox directement à partir de PowerShell gallery. En cas d’aucune connectivité à internet, clonez le référentiel GitHub pour l’installation manuelle. 

![Analyseur de diagnostic AD FS - le programme d’installation](media/ad-fs-diagonostics-analyzer/step1.png)

### <a name="setup-using-powershell-gallery"></a>Le programme d’installation à l’aide de PowerShell gallery

Si le serveur AD FS est connecté à internet, il est recommandé d’installer le module ADFSToolbox directement à partir de la galerie PowerShell en utilisant les commandes PowerShell indiquées ci-dessous.
 
   ```powershell 
    Install-Module -Name ADFSToolbox -force
    Import-Module ADFSToolbox -force
   ```
### <a name="setup-manually-by-cloning-the-repository"></a>Le programme d’installation manuellement en clonant le référentiel

Le module ADFSToolbox peut être installé manuellement à partir de GitHub directement. Suivez les instructions ci-dessous pour le clonage du référentiel et en installant le module ADFSToolbox sur le serveur AD FS.

1. Téléchargez le [référentiel](https://github.com/Microsoft/adfsToolbox/archive/master.zip)
2. Décompressez le fichier téléchargé et copiez le dossier d’adfsToolbox-master sur %SystemDrive%\\Program Files\\WindowsPowerShell\\Modules\\.
3. Importez le Module PowerShell. Dans une fenêtre PowerShell avec élévation de privilèges, exécutez la commande suivante :
 
   ```powershell 
    Import-Module ADFSToolbox -Force
   ```

## <a name="step-2-execute-the-diagnostics-and-upload-the-file-to-ad-fs-help"></a>Étape 2 : Exécuter les tests de diagnostic et de charger le fichier à l’aide AD FS

Une seule commande peut être utilisée pour exécuter facilement les tests de diagnostic sur tous les serveurs AD FS dans la batterie de serveurs. Le module PowerShell utilise les sessions à distance PS pour exécuter les tests de diagnostic sur différents serveurs dans la batterie de serveurs.

```powershell
    Export-AdfsDiagnosticsFile [-adfsServers <list of servers>]
```

Dans une batterie de serveurs ADFS Server 2016, la commande lit la liste des serveurs AD FS à partir de la configuration AD FS. Les tests de diagnostic sont ensuite tentées contre chaque serveur dans la liste. Si la liste des serveurs AD FS n’est pas disponible (exemple 2012R2), puis les tests sont exécutés sur l’ordinateur local. Pour spécifier une liste de serveurs par rapport à laquelle les tests doivent être exécutées, utilisez le **adfsServers** argument pour fournir une liste de serveurs. Vous trouverez ci-dessous un exemple

```powershell
    Export-AdfsDiagnosticsFile -adfsServers @("sts1.contoso.com", "sts2.contoso.com", "sts3.contoso.com")
```

Le résultat est un fichier JSON qui est créé dans le même répertoire où la commande a été exécutée. Le nom du fichier est ADFSDiagnosticsFile -\<timestamp\>. Un exemple de nom de fichier est ADFSDiagnosticsFile-20180716124030.

À l’étape 2 sur [ https://aka.ms/adfsdiagnosticsanalyzer ](https://aka.ms/adfsdiagnosticsanalyzer) utiliser l’Explorateur de fichiers pour sélectionner le fichier de résultat à charger.

![Outil de AD FS diagnostics analyzer - exécuter et chargez les résultats](media/ad-fs-diagonostics-analyzer/step2.png)

Cliquez sur **télécharger** pour terminer le téléchargement et de passer à l’étape suivante.

## <a name="step-3-view-diagnostics-analysis-and-resolve-any-issues"></a>Étape 3 : Afficher l’analyse de diagnostic et résoudre les problèmes éventuels

Il existe quatre sections des résultats des tests :

1. Échec : Cette section contient la liste de tests qui ont échoué. Chaque résultat se compose de :
2. Avertissement : Cette section contient une liste de tests qui ont abouti à un avertissement. Ces problèmes seront pas susceptible de provoquer des problèmes autour de l’authentification sur une plus grande échelle, mais doivent être traités dès que possible.
3. Non applicable : Cette section contient la liste de tests qui n’ont pas été exécutées, car elles n’étaient pas applicables pour ce serveur sur lequel l’exécution de la commande.
4. Passé : Cette section contient la liste de tests qui passé et n’avoir aucun élément d’action pour l’utilisateur.

Chaque résultat de test s’affiche avec les détails qui décrivent le test et la résolution les étapes :

1. Nom du test : Nom du test qui a été exécuté
2. Détails : Description de l’opération globale a été effectuée pendant le test
3. Étapes de résolution : Les étapes suggérées pour résoudre le problème en surbrillance par le test

![Outil de AD FS diagnostics analyzer - liste des résultats de test](media/ad-fs-diagonostics-analyzer/step3a.png)

![Outil Analyseur de diagnostic Microsoft AD FS - résolution des problèmes](media/ad-fs-diagonostics-analyzer/step3b.png)

## <a name="next"></a>Suivant

- [Utiliser AD FS Help troublehshooting guides](https://aka.ms/adfshelp/troubleshooting )
- [Résolution des problèmes de AD FS](ad-fs-tshoot-overview.md)

 
