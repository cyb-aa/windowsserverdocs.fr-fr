---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, installation, installation
contributor: maertendMSFT
ms.product: w10
author: maertendMSFT
title: Configuration du serveur OpenSSH pour Windows
ms.openlocfilehash: ed424c33c4cd2c19a9b5e985ab6083bcbcb9fbdc
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546261"
---
# <a name="openssh-server-configuration-for-windows-10-1809-and-server-2019"></a>Configuration du serveur OpenSSH pour Windows 10 1809 et le serveur 2019

Cette rubrique décrit la configuration spécifique à Windows pour OpenSSH Server (sshd). 

OpenSSH gère la documentation détaillée des options de configuration en ligne sur [OpenSSH.com](https://www.openssh.com/manual.html), qui n’est pas dupliquée dans cet ensemble de documentation. 

## <a name="configuring-the-default-shell-for-openssh-in-windows"></a>Configuration de l’interpréteur de commandes par défaut pour OpenSSH dans Windows

L’interface de commande par défaut permet à l’utilisateur de voir quand il se connecte au serveur à l’aide de SSH. La fenêtre par défaut initiale est l’interface de commande Windows (cmd. exe). Windows comprend également PowerShell et bash, et les interpréteurs de commandes tiers sont également disponibles pour Windows et peuvent être configurés en tant qu’interpréteur de commandes par défaut pour un serveur.

Pour définir l’interface de commande par défaut, vérifiez d’abord que le dossier d’installation de OpenSSH se trouve sur le chemin d’accès système. Pour Windows, le dossier d’installation par défaut est SystemDrive: WindowsDirectory\System32\openssh. Les commandes suivantes affichent le paramètre du chemin d’accès actuel et y ajoutent le dossier d’installation de OpenSSH par défaut. 

Interface de commande | Commande à utiliser
------------- | -------------- 
Command | path
PowerShell | $env:p hemin

La configuration de l’interpréteur de commandes ssh par défaut est effectuée dans le Registre Windows en ajoutant le chemin d’accès complet de l’exécutable de l’interpréteur de commandes à Computer\HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH dans la valeur de chaîne DefaultShell. 

Par exemple, la commande PowerShell suivante définit l’interpréteur de commandes par défaut comme étant PowerShell. exe:

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## <a name="windows-configurations-in-sshd_config"></a>Configurations Windows dans sshd_config 

Dans Windows, sshd lit les données de configuration à partir de%ProgramData%\ssh\sshd_config par défaut, ou un autre fichier de configuration peut être spécifié en lançant sshd. exe avec le paramètre-f.
Si le fichier est absent, sshd en génère un avec la configuration par défaut lorsque le service est démarré.

Les éléments listés ci-dessous fournissent une configuration spécifique à Windows possible par le biais d’entrées dans sshd_config. D’autres paramètres de configuration possibles dans ne sont pas répertoriés ici, car ils sont traités en détail dans la documentation en ligne de [Win32 OpenSSH](https://github.com/powershell/win32-openssh/wiki). 


### <a name="allowgroups-allowusers-denygroups-denyusers"></a>AllowGroups, AllowUsers, DenyGroups, DenyUsers 

Le contrôle des utilisateurs et des groupes pouvant se connecter au serveur s’effectue à l’aide des directives AllowGroups, AllowUsers, DenyGroups et DenyUsers. Les directives d’autorisation/de refus sont traitées dans l’ordre suivant: DenyUsers, AllowUsers, DenyGroups et enfin AllowGroups. Tous les noms de comptes doivent être spécifiés en minuscules. Pour plus d’informations sur les séquences de caractères génériques, consultez PATTERNs in ssh_config.

Quand vous configurez des règles basées sur l’utilisateur ou le groupe avec un groupe ou un utilisateur ``` user?domain* ```de domaine, utilisez le format suivant:.
Windows autorise un grand nombre de formats pour la spécification de principaux de domaine, mais de nombreux conflits avec les modèles Linux standard. Pour cette raison, * est ajouté pour couvrir les noms de domaine complets. En outre, cette approche utilise «?», au lieu de @, pour éviter les username@host conflits avec le format. 

Les utilisateurs/groupes de travail et les comptes connectés à Internet sont toujours résolus en leur nom de compte local (sans partie de domaine, comme pour les noms UNIX standard). Les utilisateurs et les groupes de domaine sont strictement résolus au format [NameSamCompatible](https://docs.microsoft.com/windows/desktop/api/secext/ne-secext-extended_name_format) -domain_short_name\user_name. Toutes les règles de configuration basées sur les utilisateurs et les groupes doivent respecter ce format.

Exemples pour les utilisateurs et les groupes de domaine 

```
DenyUsers contoso\admin@192.168.2.23 : blocks contoso\admin from 192.168.2.23
DenyUsers contoso\* : blocks all users from contoso domain
AllowGroups contoso\sshusers : only allow users from contoso\sshusers group
```

Exemples pour les utilisateurs et groupes locaux 

```
AllowUsers localuser@192.168.2.23
AllowGroups sshusers
```

### <a name="authenticationmethods"></a>AuthenticationMethods 

Pour Windows OpenSSH, les seules méthodes d’authentification disponibles sont «Password» et «PublicKey».

### <a name="authorizedkeysfile"></a>AuthorizedKeysFile 

La valeur par défaut est «. ssh/authorized_keys. ssh/authorized_keys2». Si le chemin d’accès n’est pas absolu, il est pris par rapport au répertoire de démarrage de l’utilisateur (ou au chemin d’accès de l’image de profil). Ex. c:\users\user.

### <a name="chrootdirectory-support-added-in-v7700"></a>ChrootDirectory (prise en charge ajoutée dans v 7.7.0.0)

Cette directive est uniquement prise en charge avec les sessions SFTP. Une session à distance dans cmd. exe ne respecterait pas cela. Pour configurer un serveur mécanisme chroot SFTP uniquement, définissez ForceCommand sur internal-sftp. Vous pouvez également configurer SCP avec mécanisme chroot, en implémentant un shell personnalisé qui autorise uniquement SCP et SFTP.

### <a name="hostkey"></a>HostKey

Les valeurs par défaut sont% ProgramData%/SSH/ssh_host_ecdsa_key,% ProgramData%/SSH/ssh_host_ed25519_key et% ProgramData%/SSH/ssh_host_rsa_key. Si les valeurs par défaut ne sont pas présentes, sshd les génère automatiquement au démarrage du service.

### <a name="match"></a>Faire correspondre

Notez que les règles de modèle dans cette section. Les noms d’utilisateurs et de groupes doivent être en minuscules.

### <a name="permitrootlogin"></a>PermitRootLogin

Non applicable dans Windows. Pour empêcher la connexion de l’administrateur, utilisez les administrateurs avec la directive DenyGroups.

### <a name="syslogfacility"></a>SyslogFacility

Si vous avez besoin d’une journalisation basée sur des fichiers, utilisez LOCAL0. Les journaux sont générés sous%ProgramData%\ssh\logs.
Toute autre valeur, y compris l’authentification par défaut de la valeur, dirige la journalisation vers ETW. Pour plus d’informations, consultez journalisation des fonctionnalités dans Windows.

### <a name="not-supported"></a>Non pris en charge 

Les options de configuration suivantes ne sont pas disponibles dans la version de OpenSSH fournie avec Windows Server 2019 et Windows 10 1809:

* AcceptEnv
* AllowStreamLocalForwarding
* AuthorizedKeysCommand
* AuthorizedKeysCommandUser
* AuthorizedPrincipalsCommand
* AuthorizedPrincipalsCommandUser
* compression ;
* ExposeAuthInfo
* GSSAPIAuthentication
* GSSAPICleanupCredentials
* GSSAPIStrictAcceptorCheck
* HostbasedAcceptedKeyTypes
* HostbasedAuthentication
* HostbasedUsesNameFromPacketOnly
* IgnoreRhosts
* IgnoreUserKnownHosts
* KbdInteractiveAuthentication
* KerberosAuthentication
* KerberosGetAFSToken
* KerberosOrLocalPasswd
* KerberosTicketCleanup
* PermitTunnel
* PermitUserEnvironment
* PermitUserRC
* PidFile
* PrintLastLog
* RDomain
* StreamLocalBindMask
* StreamLocalBindUnlink
* StrictModes
* X11DisplayOffset
* X11Forwarding
* X11UseLocalhost
* XAuthLocation

