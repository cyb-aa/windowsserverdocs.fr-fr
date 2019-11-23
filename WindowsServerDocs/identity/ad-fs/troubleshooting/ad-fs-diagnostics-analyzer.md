---
title: Analyseur de diagnostics de l'aide AD FS
description: Ce document décrit AD FS analyseur de diagnostics de l’aide et comment il peut effectuer les vérifications de base à l’aide du module PowerShell de diagnostics AD FS.
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: anandyadavMSFT
ms.date: 03/29/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5d56a84680467359b68ae1ab115801d82a34c822
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407233"
---
# <a name="ad-fs-help-diagnostics-analyzer"></a>Analyseur de diagnostics de l'aide AD FS

AD FS possède de nombreux paramètres qui prennent en charge la grande variété de fonctionnalités offertes par l’authentification et le développement d’applications. Lors de la résolution des problèmes, il est recommandé de s’assurer que tous les paramètres de AD FS sont correctement configurés. La vérification manuelle de ces paramètres peut parfois prendre du temps. AD FS aide de l’analyseur de diagnostics peut vous aider à effectuer les vérifications de base à l’aide du module PowerShell ADFSToolbox. Après avoir effectué les vérifications, AD FS aide fournit l' [Analyseur de diagnostics](https://aka.ms/adfsdiagnosticsanalyzer) pour vous aider à visualiser facilement les résultats et à offrir des étapes de correction.

L’opération complète nécessite 3 étapes simples :

1. Configurer le module ADFSToolbox sur le serveur AD FS principal ou le serveur WAP
2. Exécuter les diagnostics et télécharger le fichier vers AD FS aide
3. Afficher l’analyse des diagnostics et résoudre les problèmes

Accédez à [AD FS l’analyseur de diagnostics de l’aide (https://aka.ms/adfsdiagnosticsanalyzer)](https://aka.ms/adfsdiagnosticsanalyzer) pour commencer la résolution des problèmes.

![AD FS l’outil Analyseur de diagnostics sur AD FS aide](media/ad-fs-diagonostics-analyzer/home.png)

## <a name="step-1-setup-the-adfstoolbox-module-on-ad-fs-server"></a>Étape 1 : configurer le module ADFSToolbox sur AD FS Server

Pour exécuter l' [Analyseur de diagnostics](https://aka.ms/adfsdiagnosticsanalyzer), vous devez installer le module PowerShell ADFSToolbox. Si le serveur AD FS dispose d’une connectivité à Internet, vous pouvez installer le module ADFSToolbox directement à partir de PowerShell Gallery. En cas d’absence de connectivité à Internet, vous pouvez l’installer manuellement. 

[!WARNING]
Si vous utilisez AD FS 2,1 ou une version antérieure, vous devez installer la version 1.0.13 de ADFSToolbox. ADFSToolbox ne prend plus en charge AD FS 2,1 ou une version antérieure sur les versions les plus récentes.

![Analyseur de diagnostics de AD FS-installation](media/ad-fs-diagonostics-analyzer/step1_v2.png)

### <a name="setup-using-powershell-gallery"></a>Installation à l’aide de PowerShell Gallery

Si le serveur AD FS est connecté à Internet, il est recommandé d’installer le module ADFSToolbox directement à partir de PowerShell Gallery à l’aide des commandes PowerShell ci-dessous.

   ```powershell
    Install-Module -Name ADFSToolbox -force
    Import-Module ADFSToolbox -force
   ```

### <a name="setup-manually"></a>Installation manuelle

Le module ADFSToolbox doit être copié manuellement sur les serveurs AD FS ou WAP. Suivez les instructions ci-dessous.

1. Lancez une fenêtre PowerShell avec élévation de privilèges sur un ordinateur qui a accès à Internet.
2. Installez le module AD FS boîte à outils.

    ```powershell
    Install-Module -Name ADFSToolbox -Force
    ```
3. Copiez le dossier ADFSToolbox `%SYSTEMDRIVE%\Program Files\WindowsPowerShell\Modules\` situé sur votre ordinateur local dans le même emplacement sur votre AD FS ou sur votre ordinateur WAP.

4. Lancez une fenêtre PowerShell avec élévation de privilèges sur votre machine AD FS et exécutez l’applet de commande suivante pour importer le module.

    ```powershell
    Import-Module ADFSToolbox -Force
    ```

## <a name="step-2-execute-the-diagnostics-cmdlet"></a>Étape 2 : exécuter l’applet de commande Diagnostics

![Outil Analyseur de diagnostics de AD FS-exécuter et charger les résultats](media/ad-fs-diagonostics-analyzer/step2_v2.png)

Une seule commande peut être utilisée pour exécuter facilement les tests de diagnostic sur tous les serveurs AD FS de la batterie. Le module PowerShell utilise les sessions PS distantes pour exécuter les tests de diagnostic sur les différents serveurs de la batterie.

```powershell
    Export-AdfsDiagnosticsFile [-ServerNames <list of servers>]
```

Dans un serveur 2016 ou supérieur AD FS batterie de serveurs, la commande lira la liste des serveurs AD FS à partir de AD FS configuration. Les tests de diagnostic sont ensuite tentés sur chaque serveur de la liste. Si la liste des serveurs de AD FS n’est pas disponible (par exemple 2012 R2), les tests sont exécutés sur l’ordinateur local. Pour spécifier la liste des serveurs sur lesquels les tests doivent être exécutés, utilisez l’argument **serverNames** pour fournir une liste de serveurs. Vous trouverez un exemple ci-dessous

```powershell
    Export-AdfsDiagnosticsFile -ServerNames @("adfs1.contoso.com", "adfs2.contoso.com")
```

Le résultat est un fichier JSON qui est créé dans le même répertoire que celui où la commande a été exécutée. Le nom du fichier est AdfsDiagnosticsFile-\<timestamp\>. Un exemple de nom de fichier est AdfsDiagnosticsFile-07312019-184201. JSON.

## <a name="step-3-upload-the-diagnostics-file"></a>Étape 3 : charger le fichier de diagnostic

À l’étape 3 sur [https://aka.ms/adfsdiagnosticsanalyzer](https://aka.ms/adfsdiagnosticsanalyzer) utilisez l’Explorateur de fichiers pour sélectionner le fichier de résultats à télécharger.

Cliquez sur **Télécharger** pour terminer le téléchargement.

En vous connectant avec un compte Microsoft, vous pouvez enregistrer les résultats des diagnostics pour un point d’affichage ultérieur et les envoyer au support Microsoft. Si vous ouvrez un dossier de support à tout moment, Microsoft sera en mesure d’afficher les résultats de l’analyseur de diagnostics et de vous aider à résoudre plus rapidement votre problème.

![Outil Analyseur de diagnostics de AD FS-connexion](media/ad-fs-diagonostics-analyzer/sign_in_step.png)

## <a name="step-4-view-diagnostics-analysis-and-resolve-any-issues"></a>Étape 4 : afficher l’analyse des diagnostics et résoudre les problèmes

Il y a cinq sections des résultats des tests :

1. Échec : cette section contient la liste des tests qui ont échoué. Chaque résultat se compose des éléments suivants :
2. AVERTISSEMENT : cette section contient une liste de tests qui ont généré un avertissement. Ces problèmes n’entraînent probablement pas de problèmes d’authentification sur une plus grande échelle, mais doivent être traités au plus tôt.
3. Réussite : cette section contient la liste des tests qui ont réussi et qui n’ont aucun élément d’action pour l’utilisateur.
4. Non exécuté : cette section contient la liste des tests qui n’ont pas pu être exécutés en raison d’informations manquantes.
5. Non applicable : cette section contient la liste des tests qui n’ont pas été exécutés, car ils n’étaient pas applicables au serveur sur lequel la commande s’exécutait.

![AD FS outil Analyseur de diagnostics-liste des résultats des tests](media/ad-fs-diagonostics-analyzer/step3a_v3.png) chaque résultat de test s’affiche avec des détails qui décrivent le test et la résolution des étapes :

1. Nom du test : nom du test qui a été exécuté
2. Description : description du test.
3. Détails : description de l’opération globale effectuée pendant le test
4. Procédure de résolution : étapes suggérées pour résoudre le problème mis en surbrillance par le test

![Outil Analyseur de diagnostics de AD FS-résolution d’échec](media/ad-fs-diagonostics-analyzer/step3b_v3.png)

## <a name="next"></a>Suivant

- [Utiliser AD FS guides troublehshooting Help](https://aka.ms/adfshelp/troubleshooting )
- [Résolution des problèmes AD FS](ad-fs-tshoot-overview.md)
