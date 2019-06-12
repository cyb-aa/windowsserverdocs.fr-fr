---
title: Installer Windows Admin Center
description: Comment installer Windows Admin Center sur un PC Windows ou sur un serveur afin que plusieurs utilisateurs puissent accéder à Windows Admin Center à l’aide d’un navigateur web.
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a9eb7944cd35dfa68e3c36cdc6c016f483a9f1e1
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811953"
---
# <a name="install-windows-admin-center"></a>Installer Windows Admin Center

> S’applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Cette rubrique décrit comment installer Windows Admin Center sur un PC Windows ou sur un serveur afin que plusieurs utilisateurs puissent accéder à Windows Admin Center à l’aide d’un navigateur web.

> [!Tip]
> Vous débutez dans Windows Admin Center ?
> [Découvrez-en davantage sur Windows Admin Center](../understand/windows-admin-center.md) ou [téléchargez maintenant](https://aka.ms/windowsadmincenter).

## <a name="determine-your-installation-type"></a>Déterminer votre type d’installation

Examinez le [options d’installation](../plan/installation-options.md) qui inclut le [les systèmes d’exploitation pris en charge](../plan/installation-options.md#supported-operating-systems-installation). Pour installer Windows Admin Center sur une machine virtuelle dans Azure, consultez [déployer Windows Admin Center dans Azure](../azure/deploy-wac-in-azure.md).

## <a name="install-on-windows-10"></a>Installation sous Windows 10

Lorsque vous installez Windows Admin Center sur Windows 10, il utilise le port 6516 par défaut, mais vous avez la possibilité de spécifier un autre port. Vous pouvez également créer un raccourci sur le bureau et laisser Windows Admin Center gérer vos TrustedHosts.

> [!NOTE]
> Il est nécessaire de modifier les TrustedHosts dans un environnement de groupe de travail ou lors de l’utilisation d’informations d’identification d’administrateur local dans un domaine. Si vous choisissez de renoncer à ce paramètre, vous devez [configurer TrustedHosts manuellement](../support/troubleshooting.md#configure-trustedhosts).

Lorsque vous démarrez Windows Admin Center à partir du menu **Démarrer** , il s’ouvre dans votre navigateur par défaut.

Lorsque vous démarrerez Windows Admin Center pour la première fois, vous verrez une icône dans la zone de notification de votre bureau. Cliquez avec le bouton droit sur cette icône, choisissez **Ouvrir** pour ouvrir l’outil dans votre navigateur par défaut, ou choisissez **Quitter** pour quitter le processus en arrière-plan.

## <a name="install-on-windows-server-with-desktop-experience"></a>Installation sur Windows Server avec Expérience utilisateur

Sur Windows Server, Windows Admin Center est installé en tant que service réseau. Vous devez spécifier le port sur lequel le service écoute et cela nécessite un certificat pour le protocole HTTPS. Le programme d’installation peut créer un certificat auto-signé aux fins de test, ou vous pouvez fournir l’empreinte numérique d’un certificat déjà installé sur l’ordinateur. Si vous utilisez le certificat généré, il correspond au nom DNS du serveur. Si vous utilisez votre propre certificat, assurez-vous que le nom fourni dans le certificat correspond au nom de l’ordinateur (caractère générique certificats ne sont pas pris en charge). Vous obtenez également la possibilité de laisser Windows Admin Center gérer vos hôtes approuvés.

> [!NOTE]
> Il est nécessaire de modifier les TrustedHosts dans un environnement de groupe de travail ou lors de l’utilisation d’informations d’identification d’administrateur local dans un domaine. Si vous choisissez de renoncer à ce paramètre, vous devez [configurer TrustedHosts manuellement](../support/troubleshooting.md#configure-trustedhosts)

Une fois l’installation terminée, ouvrez un navigateur sur un ordinateur distant et naviguer vers les URL présentée dans la dernière étape de l’installer.

> [!WARNING]
> Les certificats générés automatiquement expirent 60 jours après l’installation.

## <a name="install-on-server-core"></a>Installer sur Server Core

Si vous avez une installation Server Core de Windows Server, vous pouvez installer Windows Admin Center à partir de l’invite de commandes (exécutée en tant qu’administrateur). Spécifiez un port et un certificat SSL à l’aide des arguments `SME_PORT` et `SSL_CERTIFICATE_OPTION` respectivement. Si vous avez l’intention d’utiliser un certificat existant, utilisez le `SME_THUMBPRINT` pour spécifier son empreinte.

> [!WARNING]
> Installation de Windows Admin Center entraîne le redémarrage du service WinRM, ce qui interrompt toutes les sessions PowerShell à distance. Il est recommandé d’installer à partir de Cmd local ou de PowerShell. Si vous installez une solution d’automatisation qui sera interrompue par le redémarrage du service WinRM, vous pouvez ajouter le paramètre ```RESTART_WINRM=0``` pour installer les arguments, mais WinRM doit être redémarré pour Windows Admin Center de la fonction.

Exécutez la commande suivante pour installer Windows Admin Center et générer automatiquement un certificat auto-signé :

```   
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SSL_CERTIFICATE_OPTION=generate
```

Exécutez la commande suivante pour installer Windows Admin Center avec un certificat existant :

```
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SME_THUMBPRINT=<thumbprint> SSL_CERTIFICATE_OPTION=installed
```

> [!WARNING]
> N'appelez pas `msiexec` à partir de PowerShell à l'aide de la notation de chemin d’accès relatif point-barre oblique (par exemple, `.\<WindowsAdminCenterInstallerName>.msi`). Cette notation n’est pas prise en charge et l’installation échouera. Supprimez le préfixe `.\` ou spécifiez le chemin d’accès complet au MSI.

## <a name="updating-windows-admin-center"></a>La mise à jour Windows Admin Center

Vous pouvez mettre à jour des versions non préliminaires de Windows Admin Center à l’aide de Microsoft Update ou en installant manuellement. 

Vos paramètres sont conservés lors de la mise à niveau vers une nouvelle version de Windows Admin Center. Nous ne prennent officiellement en charge la mise à niveau les versions Insider Preview de Windows Admin Center - nous pense qu’il est préférable d’effectuer une nouvelle installation - mais nous ne le bloquer.