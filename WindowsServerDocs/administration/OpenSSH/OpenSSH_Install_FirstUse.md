---
ms.date: 01/07/2019
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, installation, installation
contributor: maertendMSFT
author: maertendMSFT
title: Installation de OpenSSH pour Windows
ms.openlocfilehash: 6a5d4d47fbb3f962c2a19582eb0a72810145a28c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866874"
---
# <a name="installation-of-openssh-for-windows-server-2019-and-windows-10"></a>Installation de OpenSSH pour Windows Server 2019 et Windows 10 #

Le client OpenSSH et le serveur OpenSSH sont des composants installables séparément dans Windows Server 2019 et Windows 10 1809.
Les utilisateurs disposant de ces versions de Windows doivent suivre les instructions qui suivent pour installer et configurer OpenSSH. 

> [!NOTE] 
> Les utilisateurs qui ont acquis OpenSSH à partir du référentiel https://github.com/PowerShell/OpenSSH-Portable) PowerShell GitHub (doivent suivre les instructions de cet emplacement et __ne doivent pas__ utiliser ces instructions. 


## <a name="installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809"></a>Installation d’OpenSSH à partir de l’interface utilisateur des paramètres sur Windows Server 2019 ou Windows 10 1809

Le client et le serveur OpenSSH sont des fonctionnalités installables de Windows 10 1809. 

Pour installer OpenSSH, démarrez paramètres, puis accédez à applications > applications et fonctionnalités > gérer les fonctionnalités facultatives. 

Parcourez cette liste pour voir si OpenSSH client est déjà installé. Si ce n’est pas le cas, en haut de la page, sélectionnez « Ajouter une fonctionnalité », puis : 

* Pour installer le client OpenSSH, recherchez « client OpenSSH », puis cliquez sur « installer ». 
* Pour installer le serveur OpenSSH, recherchez « serveur OpenSSH », puis cliquez sur « installer ». 

Une fois l’installation terminée, revenez aux applications > applications et fonctionnalités > gérer les fonctionnalités facultatives. le ou les composants OpenSSH doivent apparaître.

> [!NOTE]
> L’installation du serveur OpenSSH entraîne la création et l’activation d’une règle de pare-feu nommée « OpenSSH-Server-in-TCP ». Cela autorise le trafic SSH entrant sur le port 22. 

## <a name="installing-openssh-with-powershell"></a>Installation d’OpenSSH avec PowerShell 

Pour installer OpenSSH à l’aide de PowerShell, commencez par lancer PowerShell en tant qu’administrateur.
Pour vous assurer que les fonctionnalités OpenSSH sont disponibles pour l’installation :

```powershell
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'

# This should return the following output:

Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent
Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

Ensuite, installez les fonctionnalités serveur et/ou client :

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

Pour désinstaller OpenSSH à l’aide des paramètres Windows, démarrez paramètres, puis accédez à applications > applications et fonctionnalités > gérer les fonctionnalités facultatives. Dans la liste des fonctionnalités installées, sélectionnez le composant client OpenSSH ou serveur OpenSSH, puis sélectionnez Désinstaller.

Pour désinstaller OpenSSH à l’aide de PowerShell, utilisez l’une des commandes suivantes :

```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

Un redémarrage de Windows peut être nécessaire après la suppression d’OpenSSH, si le service est en cours d’utilisation au moment où il a été désinstallé.


## <a name="initial-configuration-of-ssh-server"></a>Configuration initiale du serveur SSH

Pour configurer le serveur OpenSSH pour une utilisation initiale sur Windows, lancez PowerShell en tant qu’administrateur, puis exécutez les commandes suivantes pour démarrer le service SSHD :

```powershell
Start-Service sshd
# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
# Confirm the Firewall rule is configured. It should be created automatically by setup. 
Get-NetFirewallRule -Name *ssh*
# There should be a firewall rule named "OpenSSH-Server-In-TCP", which should be enabled 
```

## <a name="initial-use-of-ssh"></a>Utilisation initiale de SSH

Une fois que vous avez installé le serveur OpenSSH sur Windows, vous pouvez le tester rapidement à l’aide de PowerShell à partir de n’importe quel appareil Windows sur lequel le client SSH est installé. Dans PowerShell, tapez la commande suivante : 

```powershell
Ssh username@servername
```

La première connexion à un serveur se traduira par un message similaire à ce qui suit :

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

La réponse doit être « oui » ou « non ». Si vous répondez Oui, ce serveur sera ajouté à la liste des hôtes SSH connus du système local.

Vous serez invité à entrer le mot de passe à ce stade. Par mesure de sécurité, votre mot de passe ne sera pas affiché au fur et à mesure que vous tapez. 

Une fois que vous êtes connecté, une invite de commande de l’interface de commande semblable à la suivante s’affiche :

```
domain\username@SERVERNAME C:\Users\username>
```

L’interpréteur de commandes par défaut utilisé par le serveur OpenSSH Windows est l’interface de commande Windows. 

