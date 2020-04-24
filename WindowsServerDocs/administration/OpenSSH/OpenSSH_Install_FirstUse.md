---
ms.date: 09/27/2019
ms.topic: conceptual
contributor: maertendMSFT
author: maertendmsft
title: Installation de OpenSSH pour Windows
ms.openlocfilehash: b9889a9057a1ddd5181f4ea4aab35680d524eabf
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80852052"
---
# <a name="installation-of-openssh-for-windows-server-2019-and-windows-10"></a>Installation de OpenSSH pour Windows Server 2019 et Windows 10 #

Le client OpenSSH et le serveur OpenSSH sont des composants pouvant être installés séparément dans Windows Server 2019 et Windows 10 1809.
Les utilisateurs qui disposent de ces versions de Windows doivent utiliser les instructions suivantes pour installer et configurer OpenSSH. 

> [!NOTE] 
> Les utilisateurs qui ont obtenu OpenSSH à partir du référentiel GitHub de PowerShell (https://github.com/PowerShell/OpenSSH-Portable) ) doivent utiliser les instructions fournies et __ne doivent pas__ suivre celles-ci. 


## <a name="installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809"></a>Installation de OpenSSH à partir de l’interface utilisateur Paramètres sur Windows Server 2019 ou Windows 10 1809

Le client et le serveur OpenSSH sont des fonctionnalités installables de Windows 10 1809. 

Pour installer OpenSSH, à partir de Paramètres, accédez à Applications > Applications et fonctionnalités > Gérer les fonctionnalités facultatives. 

Parcourez cette liste pour voir si le client OpenSSH est déjà installé. Si ce n’est pas le cas, sélectionnez « Ajouter une fonctionnalité » en haut de la page, puis : 

* Pour installer le client OpenSSH, recherchez « Client OpenSSH », puis cliquez sur « Installer ». 
* Pour installer le serveur OpenSSH, recherchez « Serveur OpenSSH », puis cliquez sur « Installer ». 

Une fois l’installation terminée, revenez à Applications > Applications et fonctionnalités > Gérer les fonctionnalités facultatives pour afficher la liste des composants OpenSSH.

> [!NOTE]
> L’installation du serveur OpenSSH crée et active une règle de pare-feu appelée « OpenSSH-Server-In-TCP ». Cela autorise le trafic SSH entrant sur le port 22. 

## <a name="installing-openssh-with-powershell"></a>Installation de OpenSSH avec PowerShell 

Pour installer OpenSSH à l’aide de PowerShell, lancez d’abord PowerShell en tant qu’administrateur.
Pour être sûr que les fonctionnalités OpenSSH sont disponibles pour l’installation :

```powershell
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'

# This should return the following output:

Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent
Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

Installez ensuite les fonctionnalités du serveur et/ou client :

```powershell
# Install the OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Install the OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0

# Both of these should return the following output:

Path          :
Online        : True
RestartNeeded : False
```

## <a name="uninstalling-openssh"></a>Désinstallation de OpenSSH

Pour désinstaller OpenSSH à l’aide des Paramètres Windows, à partir de Paramètres, accédez à Applications > Applications et fonctionnalités > Gérer les fonctionnalités facultatives. Dans la liste des fonctionnalités installées, sélectionnez le composant Client OpenSSH ou Serveur OpenSSH, puis l’option Désinstaller.

Pour désinstaller OpenSSH à l’aide de PowerShell, utilisez l’une des commandes suivantes :

```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

Un redémarrage de Windows peut être nécessaire après la suppression de OpenSSH, si le service est en cours d’utilisation au moment de la désinstallation.


## <a name="initial-configuration-of-ssh-server"></a>Configuration initiale du serveur SSH

Pour configurer le serveur OpenSSH pour une utilisation initiale sur Windows, lancez PowerShell en tant qu’administrateur, puis exécutez les commandes suivantes pour démarrer le service SSHD :

```powershell
Start-Service sshd
# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
# Confirm the Firewall rule is configured. It should be created automatically by setup. 
Get-NetFirewallRule -Name *ssh*
# There should be a firewall rule named "OpenSSH-Server-In-TCP", which should be enabled
# If the firewall does not exist, create one
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
```

## <a name="initial-use-of-ssh"></a>Utilisation initiale de SSH

Une fois que vous avez installé le serveur OpenSSH sur Windows, vous pouvez le tester rapidement à l’aide de PowerShell à partir de n’importe que appareil Windows sur lequel est installé le client SSH. Dans PowerShell, entrez la commande suivante : 

```powershell
Ssh username@servername
```

La première connexion à un serveur entraîne l’apparition d’un message similaire à ceci :

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

La réponse doit être « oui » ou « non ». Le fait de répondre Oui ajoute ce serveur à la liste des hôtes SSH connus du système local.

Vous êtes alors invité à entrer le mot de passe. Par mesure de sécurité, votre mot de passe n’est pas affiché lorsque vous l’entrez. 

Une fois connecté, vous allez voir une invite de commande semblable à celle-ci :

```
domain\username@SERVERNAME C:\Users\username>
```

L’interpréteur de commandes utilisé par le serveur OpenSSH Windows est l’interface de commande Windows. 

