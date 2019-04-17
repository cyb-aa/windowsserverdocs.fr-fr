---
title: Installer Windows Admin Center
description: Découvrez comment installer Windows Admin Center sur un PC Windows ou un serveur.
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 496a67cfb93bf9b42b202f6a61211a085a9253b6
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296691"
---
# Installer Windows Admin Center

>S’applique à: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Vous débutez dans Windows Admin Center?
> [Découvrez-en davantage sur Windows Admin Center](../understand/windows-admin-center.md) ou [téléchargez maintenant](https://aka.ms/windowsadmincenter).

## Déterminer le type d’installation

Passez en revue les [options d’installation](..\plan\installation-options.md) qui inclut la [prise en charge des systèmes d’exploitation](..\plan\installation-options.md#supported-operating-systems-installation). Pour installer Windows Admin Center sur un ordinateur virtuel dans Azure, voir [Déployer Windows Admin Center dans Azure](../azure/deploy-wac-in-azure.md).

## Installation sous Windows10

Lorsque vous installez Windows Admin Center sur Windows10, il utilise le port6516 par défaut, mais vous avez la possibilité de spécifier un autre port. Vous pouvez également créer un raccourci sur le bureau et laisser Windows Admin Center gérer vos TrustedHosts.

> [!NOTE]
> Il est nécessaire de modifier les TrustedHosts dans un environnement de groupe de travail ou lors de l’utilisation d’informations d’identification d’administrateur local dans un domaine. Si vous choisissez de renoncer à ce paramètre, vous devez [configurer TrustedHosts manuellement](../support/troubleshooting.md#configure-trustedhosts).

Lorsque vous démarrez Windows Admin Center à partir du menu **Démarrer** , il s’ouvre dans votre navigateur par défaut.

Lorsque vous démarrerez Windows Admin Center pour la première fois, vous verrez une icône dans la zone de notification de votre bureau. Cliquez avec le bouton droit sur cette icône, choisissez **Ouvrir** pour ouvrir l’outil dans votre navigateur par défaut, ou choisissez **Quitter** pour quitter le processus en arrière-plan.

## Installation sur Windows Server avec Expérience utilisateur

Sur Windows Server, Windows Admin Center est installé en tant que service réseau. Vous devez spécifier le port sur lequel le service écoute et cela nécessite un certificat pour le protocole HTTPS. Le programme d’installation peut créer un certificat auto-signé aux fins de test, ou vous pouvez fournir l’empreinte numérique d’un certificat déjà installé sur l’ordinateur. Si vous utilisez le certificat généré, il correspond au nom DNS du serveur. Si vous utilisez votre propre certificat, assurez-vous que le nom indiqué dans le certificat correspond au nom d’ordinateur (caractère générique certificats ne sont pas pris en charge). Vous avez également la possibilité de laisser Windows Admin Center gérer vos TrustedHosts.

> [!NOTE]
> Il est nécessaire de modifier les TrustedHosts dans un environnement de groupe de travail ou lors de l’utilisation d’informations d’identification d’administrateur local dans un domaine. Si vous choisissez de renoncer à ce paramètre, vous devez [configurer TrustedHosts manuellement](../support/troubleshooting.md#configure-trustedhosts)

Une fois que le programme d’installation est terminée, ouvrez un navigateur à partir d’un ordinateur distant et accédez à URL présentée dans la dernière étape du programme d’installation.

> [!WARNING]
> Les certificats générés automatiquement expirent 60jours après l’installation.

## Installer sur Server Core

Si vous avez une installation Server Core de Windows Server, vous pouvez installer Windows Admin Center à partir de l’invite de commandes (exécutée en tant qu’administrateur). Spécifiez un port et un certificat SSL à l’aide des arguments `SME_PORT` et `SSL_CERTIFICATE_OPTION` respectivement. Si vous avez l’intention d’utiliser un certificat existant, utilisez le `SME_THUMBPRINT` pour spécifier son empreinte.

> [!WARNING]
> Installez Windows Admin Center opération va redémarrer le service WinRM, qui sera serveur toutes les sessions PowerShells à distance. Il est recommandé d’installer à partir d’un Cmd local ou PowerShell. Si vous installez avec une solution d’automatisation qui doivent être fractionnée par le service WinRM le redémarrage, vous pouvez ajouter le paramètre ```RESTART_WINRM=0``` pour le programme d’installation arguments, mais WinRM doit être redémarré pour Windows Admin Center pour fonctionner.

Exécutez la commande suivante pour installer Windows Admin Center et générer automatiquement un certificat auto-signé:

```   
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SSL_CERTIFICATE_OPTION=generate
```

Exécutez la commande suivante pour installer Windows Admin Center avec un certificat existant:

```
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SME_THUMBPRINT=<thumbprint> SSL_CERTIFICATE_OPTION=installed
```

> [!WARNING]
> N'appelez pas `msiexec` à partir de PowerShell à l'aide de la notation de chemin d’accès relatif point-barre oblique (par exemple, `.\<WindowsAdminCenterInstallerName>.msi`). Cette notation n’est pas prise en charge et l’installation échouera. Supprimez le préfixe `.\` ou spécifiez le chemin d’accès complet au MSI.

## Mise à jour de Windows Admin Center

Vous pouvez mettre à jour les versions non-version d’évaluation de Windows Admin Center à l’aide de Microsoft Update ou installer manuellement. 

Vos paramètres sont conservés lors de la mise à niveau vers une nouvelle version de Windows Admin Center. Nous ne pas en charge officiellement les versions Insider Preview la mise à niveau de Windows Admin Center: nous pense qu’il est préférable d’effectuer une nouvelle installation -, mais nous ne le bloquer.