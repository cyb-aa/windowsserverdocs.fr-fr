---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, installer, configurer
contributor: maertendMSFT
ms.product: w10
author: maertendMSFT
title: Configuration du serveur de OpenSSH pour Windows
ms.openlocfilehash: 7eff3d3e1af67c9daf7a68c67c3609c0ee89fc93
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280034"
---
# <a name="openssh-server-configuration-for-windows-10-1809-and-server-2019"></a>Configuration d’OpenSSH Server pour Windows 10 1809 et Server # 2019

Cette rubrique traite de la configuration de Windows spécifiques pour OpenSSH Server (sshd). 

OpenSSH maintient une documentation détaillée des options de configuration en ligne à [OpenSSH.com](https://www.openssh.com/manual.html), c'est-à-dire ne pas figurer dans cette documentation. 

## <a name="configuring-the-default-shell-for-openssh-in-windows"></a>Configuration de l’interpréteur de commandes par défaut pour OpenSSH dans Windows

L’interface de commande par défaut fournit l’expérience qu'utilisateur voit lors de la connexion au serveur à l’aide de SSH. La valeur par défaut initiale Windows est l’interface de commande de Windows (cmd.exe). Windows inclut également PowerShell et Bash, et les interfaces de commande de tiers sont également disponibles pour Windows et peuvent être configurés en tant que le shell par défaut pour un serveur.

Pour définir la valeur par défaut de shell de commande, vérifiez que le dossier d’installation OpenSSH est sur le chemin d’accès système. Pour Windows, le dossier d’installation par défaut est SystemDrive:WindowsDirectory\System32\openssh. Les commandes suivantes affiche le paramètre de chemin d’accès actuel et ajouter le dossier d’installation par défaut OpenSSH. 

Interface de commande | Commande à utiliser
------------- | -------------- 
Command | path
PowerShell | $env:path

Configuration de la valeur par défaut ssh shell est effectué dans le Registre Windows en ajoutant le chemin d’accès complet à un fichier exécutable à Computer\HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH DefaultShell de la valeur de chaîne de l’interpréteur de commandes. 

Par exemple, la commande Powershell suivante définit le shell par défaut pour être PowerShell.exe :

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## <a name="windows-configurations-in-sshdconfig"></a>Configurations de Windows dans sshd_config 

Dans Windows, sshd lit les données de configuration à partir de %programdata%\ssh\sshd_config par défaut, ou un autre fichier de configuration peut-être être spécifié en lançant sshd.exe avec le paramètre -f.
Si le fichier est absent, sshd génère un avec la configuration par défaut, lorsque le service est démarré.

Les éléments répertoriés ci-dessous fournissent possible de configuration spécifiques à Windows via des entrées dans sshd_config. Il existe des autres paramètres de configuration possibles dans qui ne sont pas répertoriées ici, car elles sont traitées en détail dans l’en ligne [OpenSSH Win32 documentation](https://github.com/powershell/win32-openssh/wiki). 


### <a name="allowgroups-allowusers-denygroups-denyusers"></a>AllowGroups, AllowUsers, DenyGroups, DenyUsers 

Contrôler quels utilisateurs et groupes peuvent se connecter au serveur s’effectue à l’aide de directives AllowGroups, AllowUsers, DenyGroups et DenyUsers. Les directives d’autorisation/refus sont traitées dans l’ordre suivant : DenyUsers, AllowUsers, DenyGroups et enfin AllowGroups. Tous les noms de compte doivent être spécifiés en minuscules. Voir les modèles dans ssh_config pour plus d’informations sur des modèles pour des caractères génériques.

Lors de la configuration d’utilisateur/groupe en fonction des règles avec un utilisateur de domaine ou un groupe, utilisez le format suivant : ``` user?domain* ```.
Windows autorise plusieurs formats de pour spécifier les entités de domaine, mais nombreuses en conflit avec les modèles de Linux standard. Pour cette raison, * est ajouté pour couvrir les noms de domaine complets. En outre, cette approche utilise « ? », au lieu de @, pour éviter les conflits avec les username@host format. 

Groupe de travail utilisateurs/groupes et comptes connectés à internet sont toujours résolus à leur nom de compte local (sans domaine la partie, semblable à ceux des Unix standard). Les utilisateurs de domaine et les groupes sont strictement résolues en [NameSamCompatible](https://docs.microsoft.com/windows/desktop/api/secext/ne-secext-extended_name_format) format - domain_short_name\user_name. Tous les utilisateurs/groupes en fonction des règles doivent se conformer à ce format de configuration.

Exemples pour les utilisateurs de domaine et les groupes 

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

Pour Windows OpenSSH, les méthodes d’authentification uniquement disponibles sont « password » et « clé publique ».

### <a name="authorizedkeysfile"></a>AuthorizedKeysFile 

La valeur par défaut est « .ssh/authorized_keys .ssh/authorized_keys2 ». Si le chemin d’accès n’est pas absolu, il est utilisé par rapport au répertoire de base de l’utilisateur (ou le chemin d’accès de profil image). Exemple : c:\Users\User.

### <a name="chrootdirectory-support-added-in-v7700"></a>ChrootDirectory (prise en charge ajoutée v7.7.0.0)

Cette directive est uniquement pris en charge avec les sessions de sftp. Une session à distance dans cmd.exe n’aurait pas honorer cette. Pour configurer un serveur sftp seule chroot, la valeur ForceCommand interne sftp. Vous pouvez également définir le scp avec chroot, en implémentant un interpréteur de commandes personnalisé qui permet uniquement scp et sftp.

### <a name="hostkey"></a>HostKey

Les valeurs par défaut sont les programdata%/ssh/ssh_host_rsa_key programdata%/ssh/ssh_host_ecdsa_key, %programdata%/ssh/ssh_host_ed25519_key et % %. Si les valeurs par défaut ne sont pas présents, sshd génère automatiquement ces sur un démarrage du service.

### <a name="match"></a>Faire correspondre

Notez que le modèle de règles dans cette section. Noms d’utilisateur et de groupe doivent être en minuscules.

### <a name="permitrootlogin"></a>PermitRootLogin

Non applicable dans Windows. Pour empêcher la connexion de l’administrateur, utiliser administrateurs avec la directive DenyGroups.

### <a name="syslogfacility"></a>SyslogFacility

Si vous avez besoin de fichier en fonction de journalisation, utilisez LOCAL0. Journaux sont générés sous % programdata%\ssh\logs.
Toute autre valeur, y compris la valeur par défaut AUTH dirigeant la journalisation ETW. Pour plus d’informations, consultez les fonctions de journalisation dans Windows.

### <a name="not-supported"></a>Non pris en charge 

Les options de configuration suivantes ne sont pas disponibles dans la version d’OpenSSH fourni avec Windows Server 2019 et Windows 10 1809 :

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

