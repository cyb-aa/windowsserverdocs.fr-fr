---
ms.date: 01/07/2019
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, installer, configurer
contributor: maertendMSFT
author: maertendMSFT
title: Installation de OpenSSH pour Windows
ms.openlocfilehash: f617b01ee7dabd4897f99e374420f673e209e145
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859560"
---
# <a name="installation-of-openssh-for-windows-server-2019-and-windows-10"></a>Installation de OpenSSH pour Windows Server 2019 et Windows 10 #

Le OpenSSH Client et OpenSSH Server sont des composants installables séparément dans Windows Server 2019 et Windows 10 1809.
Utilisateurs avec ces versions de Windows doivent utiliser les instructions qui suivent pour installer et configurer OpenSSH. 

> [!NOTE] 
> Les utilisateurs qui acquis OpenSSH du référentiel Github de PowerShell (https://github.com/PowerShell/OpenSSH-Portable) doit utiliser les instructions à partir de là, et __ne doivent pas__ Utilisez ces instructions. 


## <a name="installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809"></a>L’installation d’OpenSSH dans les paramètres de l’interface utilisateur sur Windows Server 2019 ou Windows 10 1809

Serveur et client OpenSSH sont des fonctionnalités installables de Windows 10 1809. 

Pour installer OpenSSH, démarrer des paramètres, puis accédez à applications > applications et fonctionnalités > Gérer les fonctionnalités facultatives. 

Analyse de cette liste pour voir si le client OpenSSH est déjà installé. Si ce n’est pas le cas, puis en haut de la page, sélectionnez « Ajouter une fonctionnalité », puis : 

* Pour installer le client OpenSSH, recherchez « OpenSSH Client », puis cliquez sur « Installer ». 
* Pour installer le serveur OpenSSH, recherchez « OpenSSH Server », puis cliquez sur « Installer ». 

Une fois l’installation terminée, revenez au applications > applications et fonctionnalités > Gérer les fonctionnalités facultatives vous devriez voir l’ou les composants OpenSSH répertorié.

> [!NOTE]
> L’installation OpenSSH Server pour créer et activer une règle de pare-feu nommée « OpenSSH-Server-de-TCP ». Ainsi, le trafic SSH entrant sur le port 22. 

## <a name="installing-openssh-with-powershell"></a>L’installation d’OpenSSH avec PowerShell 

Pour installer OpenSSH à l’aide de PowerShell, vous devez tout d’abord lancez PowerShell en tant qu’administrateur.
Pour vous assurer que les fonctionnalités d’OpenSSH sont disponibles pour l’installation :

```powershell
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'

# This should return the following output:

Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent
Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

Ensuite, installez les fonctionnalités de serveur ou client :

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

## <a name="uninstalling-openssh"></a>Désinstallation OpenSSH

Pour désinstaller OpenSSH en utilisant les paramètres de Windows, démarrer des paramètres, puis accédez à applications > applications et fonctionnalités > Gérer les fonctionnalités facultatives. Dans la liste des fonctionnalités installées, sélectionnez le composant OpenSSH Client ou OpenSSH Server, puis sélectionnez Désinstaller.

Pour désinstaller OpenSSH à l’aide de PowerShell, utilisez une des commandes suivantes :

```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

Un redémarrage de Windows peut être nécessaire après que la suppression d’OpenSSH, si le service est en cours d’utilisation au moment où il a été désinstallé.


## <a name="initial-configuration-of-ssh-server"></a>Configuration initiale du serveur SSH

Pour configurer le serveur OpenSSH pour une première utilisation sur Windows, lancez PowerShell en tant qu’administrateur, puis exécutez les commandes suivantes pour démarrer le service SSHD :

```powershell
Start-Service sshd
# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
# Confirm the Firewall rule is configured. It should be created automatically by setup. 
Get-NetFirewallRule -Name *ssh*
# There should be a firewall rule named "OpenSSH-Server-In-TCP", which should be enabled 
```

## <a name="initial-use-of-ssh"></a>Première utilisation de SSH

Une fois que vous avez installé le serveur OpenSSH sur Windows, vous pouvez tester rapidement à l’aide de PowerShell à partir de n’importe quel appareil Windows avec le Client SSH installé. Dans PowerShell, tapez la commande suivante : 

```powershell
Ssh username@servername
```

La première connexion à n’importe quel serveur entraîne un message similaire à ce qui suit :

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

La réponse doit être « yes » ou « non ». Si vous répondez Oui ajoutera ce serveur pour le système local de liste de connue ssh hôtes.

Vous devez le mot de passe à ce stade. Par mesure de sécurité, votre mot de passe affichera pas en cours de frappe. 

Une fois que vous vous connectez, vous verrez un interpréteur de commandes semblable à ce qui suit :

```
domain\username@SERVERNAME C:\Users\username>
```

Le shell par défaut utilisé par Windows OpenSSH server est l’interface de commande Windows. 

