---
ms.date: 09/27/2018
ms.topic: conceptual
contributor: maertendMSFT
ms.product: windows-server
author: maertendmsft
title: Configuration du serveur OpenSSH pour Windows
ms.openlocfilehash: 61f176f7f73495a6b9dbbcb1a25f2337a44ab99b
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80852022"
---
# <a name="openssh-server-configuration-for-windows-10-1809-and-server-2019"></a>Configuration du serveur OpenSSH pour Windows 10 1809 et Server 2019

Cette rubrique porte sur la configuration spécifique à Windows pour le serveur OpenSSH (sshd). 

OpenSSH gère la documentation détaillée des options de configuration en ligne sur [OpenSSH.com](https://www.openssh.com/manual.html), qui n’est pas dupliquée dans cet ensemble de documentation. 

## <a name="configuring-the-default-shell-for-openssh-in-windows"></a>Configuration du shell par défaut pour OpenSSH dans Windows

L’interface shell par défaut offre une expérience utilisateur à toute personne se connectant au serveur à l’aide de SSH. Par défaut, le Windows initial est l’interface shell Windows (cmd.exe). Windows comprend également PowerShell et Bash. Les interfaces shell tierces sont également disponibles pour Windows et peuvent être configurées comme shell par défaut pour un serveur.

Pour définir l’interface shell par défaut, vérifiez d’abord que le dossier d’installation de OpenSSH se trouve sur le chemin d’accès système. Pour Windows, le dossier d’installation par défaut est SystemDrive:WindowsDirectory\System32\openssh. Les commandes suivantes affichent le paramètre du chemin d’accès actuel et y ajoutent le dossier d’installation de OpenSSH par défaut. 

Interface shell | Commande à utiliser
------------- | -------------- 
Commande | path
PowerShell | $env:path

La configuration du shell ssh par défaut s’effectue dans le Registre Windows en ajoutant le chemin d’accès complet de l’exécutable du shell sur Computer\HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH dans la valeur de chaîne DefaultShell. 

Par exemple, la commande PowerShell suivante définit le shell par défaut comme étant PowerShell. exe :

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## <a name="windows-configurations-in-sshd_config"></a>Configurations de Windows dans sshd_config 

Dans Windows, sshd lit les données de configuration à partir de %programdata%\ssh\sshd_config par défaut. Un autre fichier de configuration peut être spécifié en lançant sshd. exe avec le paramètre -f.
En cas d’absence du fichier, sshd en génère un avec la configuration par défaut lorsque le service est démarré.

Les éléments listés ci-dessous fournissent une configuration spécifique à Windows possible par le biais d’entrées dans sshd_config. Il existe d’autres paramètres de configuration possibles qui ne sont pas répertoriés ici, car ils sont abordés en détail dans la [documentation en ligne Win32 OpenSSH](https://github.com/powershell/win32-openssh/wiki). 


### <a name="allowgroups-allowusers-denygroups-denyusers"></a>AllowGroups, AllowUsers, DenyGroups, DenyUsers 

Le contrôle des utilisateurs et des groupes qui peuvent se connecter au serveur se fait à l’aide des directives AllowGroups, AllowUsers, DenyGroups et DenyUsers. Les directives d’autorisation/de refus sont traitées dans l’ordre suivant : DenyUsers, AllowUsers, DenyGroups et AllowGroups. Tous les noms de compte doivent être indiqués en minuscules. Pour plus d’informations sur les modèles de caractères génériques, voir MODÈLES dans ssh_config.

Lorsque vous configurez des règles basées sur des utilisateurs ou des groupes avec un utilisateur ou un groupe de domaines, utilisez le format suivant : ``` user?domain* ```.
Windows autorise l’utilisation de plusieurs formats afin d’indiquer les principes du domaine, mais beaucoup d’entre eux entrent en conflit avec les modèles Linux standard. Pour cette raison, * est ajouté pour inclure les noms de domaine complets. En outre, cette approche utilise « ? », au lieu de @, pour éviter les conflits avec le format username@host. 

Les utilisateurs de groupes de travail/groupes de travail et les comptes connectés à Internet sont toujours résolus avec leur nom de compte local (sans composant de domaine, comme pour les noms UNIX standard). Les utilisateurs et les groupes de domaine sont strictement résolus au format [NameSamCompatible](https://docs.microsoft.com/windows/desktop/api/secext/ne-secext-extended_name_format) – domain_short_name\user_name. Toutes les règles de configuration basées sur les utilisateurs et les groupes doivent respecter ce format.

Exemples pour les utilisateurs et les groupes de domaine 

```
DenyUsers contoso\admin@192.168.2.23 : blocks contoso\admin from 192.168.2.23
DenyUsers contoso\* : blocks all users from contoso domain
AllowGroups contoso\sshusers : only allow users from contoso\sshusers group
```

Exemples pour les utilisateurs et les groupes locaux 

```
AllowUsers localuser@192.168.2.23
AllowGroups sshusers
```

### <a name="authenticationmethods"></a>AuthenticationMethods 

Pour Windows OpenSSH, les seules méthodes d’authentification disponibles sont « mot de passe » et « clé publique ».

### <a name="authorizedkeysfile"></a>AuthorizedKeysFile 

La valeur par défaut est « .ssh/authorized_keys .ssh/authorized_keys2 ». Si le chemin d’accès n’est pas absolu, il est défini par rapport au répertoire de base de l’utilisateur (ou au chemin d’accès de l’image de profil). Exemple : c:\users\user. Notez que si l’utilisateur appartient au groupe d’administrateurs, % ProgramData%/SSH/administrators_authorized_keys est utilisé à la place.

### <a name="chrootdirectory-support-added-in-v7700"></a>ChrootDirectory (prise en charge ajoutée dans v7.7.0.0)

Cette directive est uniquement prise en charge avec les sessions SFTP. Une session à distance dans cmd.exe ne respecterait pas cela. Pour configurer un serveur chroot avec SFTP uniquement, définissez ForceCommand SFTP interne. Vous pouvez également configurer SCP avec chroot, en implémentant un shell personnalisé qui autorise uniquement SCP et SFTP.

### <a name="hostkey"></a>HostKey

Par défaut, il s’agit de %programdata%/ssh/ssh_host_ecdsa_key, %programdata%/ssh/ssh_host_ed25519_key, %programdata%/ssh/ssh_host_dsa_key et de %programdata%/ssh/ssh_host_rsa_key. Si les valeurs par défaut ne sont pas présentes, sshd les génère automatiquement au démarrage du service.

### <a name="match"></a>Faire correspondre

Notez les règles de modèle dans cette section. Les noms d’utilisateur et de groupe doivent être en minuscules.

### <a name="permitrootlogin"></a>PermitRootLogin

Non applicable dans Windows. Pour empêcher la connexion de l’administrateur, utilisez les administrateurs avec la directive DenyGroups.

### <a name="syslogfacility"></a>SyslogFacility

Si vous avez besoin d’une journalisation basée sur des fichiers, utilisez LOCAL0. Les journaux sont générés sous %programdata%\ssh\logs.
Toute autre valeur, y compris l’authentification par défaut de la valeur, dirige la journalisation vers ETW. Pour plus d’informations, consultez la fonction de journalisation dans Windows.

### <a name="not-supported"></a>Non prise en charge 

Les options de configuration suivantes ne sont pas disponibles dans la version de OpenSSH fournie avec Windows Server 2019 et Windows 10 1809 :

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

