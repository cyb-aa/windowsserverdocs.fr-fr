---
title: Installer Windows Admin Center
description: Guide pratique pour installer Windows Admin Center sur un PC Windows ou sur un serveur afin que plusieurs utilisateurs puissent accéder à Windows Admin Center à l’aide d’un navigateur web.
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 07/17/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: cab128a3da9fa58c598cebcdf188058631c33977
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950006"
---
# <a name="install-windows-admin-center"></a>Installer Windows Admin Center

> S'applique à : Windows Admin Center, Windows Admin Center Preview

Cette rubrique explique comment installer Windows Admin Center sur un PC Windows ou sur un serveur afin que plusieurs utilisateurs puissent accéder à Windows Admin Center à l’aide d’un navigateur web.

> [!Tip]
> Vous débutez dans Windows Admin Center ?
> [Découvrez-en davantage sur Windows Admin Center](../overview.md) ou [téléchargez maintenant](https://aka.ms/windowsadmincenter).

## <a name="determine-your-installation-type"></a>Déterminer votre type d’installation

Passez en revue les [options d’installation](../plan/installation-options.md), qui incluent les [systèmes d’exploitation pris en charge](https://docs.microsoft.com/windows-server/manage/windows-admin-center/plan/installation-options#installation-supported-operating-systems). Pour installer Windows Admin Center sur une machine virtuelle dans Azure, consultez [Déployer Windows Admin Center dans Azure](../azure/deploy-wac-in-azure.md).

## <a name="install-on-windows-10"></a>Installation sur Windows 10

Quand vous installez Windows Admin Center sur Windows 10, il utilise le port 6516 par défaut, mais vous avez la possibilité de spécifier un autre port. Vous pouvez également créer un raccourci sur le bureau et laisser Windows Admin Center gérer vos TrustedHosts.

> [!NOTE]
> Il est nécessaire de modifier les TrustedHosts dans un environnement de groupe de travail ou lors de l’utilisation d’informations d’identification d’administrateur local dans un domaine. Si vous choisissez de renoncer à ce paramètre, vous devez [configurer TrustedHosts manuellement](../support/troubleshooting.md#configure-trustedhosts).

Quand vous démarrez Windows Admin Center à partir du menu **Démarrer**, il s’ouvre dans votre navigateur par défaut.

Quand vous démarrerez Windows Admin Center pour la première fois, vous verrez une icône dans la zone de notification de votre bureau. Cliquez avec le bouton droit sur cette icône, choisissez **Ouvrir** pour ouvrir l’outil dans votre navigateur par défaut, ou choisissez **Quitter** pour quitter le processus en arrière-plan.

## <a name="install-on-windows-server-with-desktop-experience"></a>Installation sur Windows Server avec Expérience utilisateur

Sur Windows Server, Windows Admin Center est installé en tant que service réseau. Vous devez spécifier le port sur lequel le service écoute et cela nécessite un certificat pour le protocole HTTPS. Le programme d’installation peut créer un certificat auto-signé aux fins de test, ou vous pouvez fournir l’empreinte numérique d’un certificat déjà installé sur l’ordinateur. Si vous utilisez le certificat généré, il correspond au nom DNS du serveur. Si vous utilisez votre propre certificat, assurez-vous que le nom fourni dans le certificat correspond au nom de l’ordinateur (les certificats génériques ne sont pas pris en charge.) Vous aurez également la possibilité de laisser Windows Admin Center gérer vos TrustedHosts.

> [!NOTE]
> Il est nécessaire de modifier les TrustedHosts dans un environnement de groupe de travail ou lors de l’utilisation d’informations d’identification d’administrateur local dans un domaine. Si vous choisissez de renoncer à ce paramètre, vous devez [configurer TrustedHosts manuellement](../support/troubleshooting.md#configure-trustedhosts).

Une fois l’installation terminée, ouvrez un navigateur à partir d’un ordinateur distant et accédez à l’URL présentée à la dernière étape du programme d’installation.

> [!WARNING]
> Les certificats générés automatiquement expirent 60 jours après l’installation.

## <a name="install-on-server-core"></a>Installer sur Server Core

Si vous avez une installation Server Core de Windows Server, vous pouvez installer Windows Admin Center à partir de l’invite de commandes (exécutée en tant qu’administrateur). Spécifiez un port et un certificat SSL à l’aide des arguments `SME_PORT` et `SSL_CERTIFICATE_OPTION` respectivement. Si vous avez l’intention d’utiliser un certificat existant, utilisez le `SME_THUMBPRINT` pour spécifier son empreinte.

> [!WARNING]
> L’installation de Windows Admin Center entraîne le redémarrage du service WinRM et, par la même occasion, l’interruption de toutes les sessions PowerShell distantes. Nous vous recommandons d’effectuer l’installation à partir d’une invite Cmd locale ou de PowerShell. Si vous effectuez l’installation avec une solution d’automatisation qui serait interrompue par le redémarrage du service WinRM, vous pouvez ajouter le paramètre ```RESTART_WINRM=0``` aux arguments d’installation, mais WinRM doit être redémarré pour que Windows Admin Center fonctionne.

Exécutez la commande suivante pour installer Windows Admin Center et générer automatiquement un certificat auto-signé :

```   
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SSL_CERTIFICATE_OPTION=generate
```

Exécutez la commande suivante pour installer Windows Admin Center avec un certificat existant :

```
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SME_THUMBPRINT=<thumbprint> SSL_CERTIFICATE_OPTION=installed
```

> [!WARNING]
> N’appelez pas `msiexec` à partir de PowerShell à l’aide de la notation de chemin d’accès relatif point-barre oblique (par exemple, `.\<WindowsAdminCenterInstallerName>.msi`). Cette notation n’est pas prise en charge et l’installation échouera. Supprimez le préfixe `.\` ou spécifiez le chemin d’accès complet au MSI.

## <a name="upgrading-to-a-new-version-of-windows-admin-center"></a>Mise à niveau vers une nouvelle version de Windows Admin Center

Vous pouvez mettre à jour les versions non-Preview de Windows Admin Center à l’aide de Microsoft Update ou en effectuant une installation manuelle.

Vos paramètres sont conservés lors de la mise à niveau vers une nouvelle version de Windows Admin Center. Nous ne prenons officiellement pas en charge la mise à niveau des versions d’Insider Preview de Windows Admin Center (nous pensons qu’il est préférable d’effectuer une nouvelle installation), mais nous ne l’empêchons pas.

## <a name="updating-the-certificate-used-by-windows-admin-center"></a>Mise à jour du certificat utilisé par Windows Admin Center

Une fois Windows Admin Center déployé en tant que service, vous devez fournir un certificat pour le protocole HTTPS. Pour mettre à jour ce certificat ultérieurement, réexécutez le programme d’installation et choisissez ```change```.
