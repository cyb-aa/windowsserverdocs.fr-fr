---
ms.date: 09/27/2018
ms.topic: conceptual
contributor: maertendMSFT
author: maertendmsft
title: Configuration du serveur OpenSSH pour Windows
ms.product: windows-server
ms.openlocfilehash: defb8875ca73c0d08fb0fa0764ed3ddf9003e09c
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80852042"
---
# <a name="openssh-key-management"></a>Gestion des clés OpenSSH

La plupart des authentifications dans les environnements Windows s’effectuent à l’aide d’une paire nom d’utilisateur/mot de passe.
Cela convient bien aux systèmes qui partagent un domaine commun. Si vous travaillez sur plusieurs domaines, par exemple sur un système local et un système hébergé dans le cloud, cela devient plus difficile.

À titre de comparaison, les environnements Linux utilisent généralement des paires de clés publiques/privées pour gérer l’authentification.
OpenSSH intègre des outils permettant de faciliter la prise en charge de ce service, notamment :

* __ssh-keygen__ pour la génération de clés sécurisées
* __ssh-agent__ et __ssh-add__ pour le stockage sécurisé des clés privées
* __scp__ et __sftp__ pour copier en toute sécurité des fichiers de clé publique lors de l’utilisation initiale d’un serveur

Ce document fournit une vue d’ensemble de l’utilisation de ces outils sur Windows pour commencer à utiliser l’authentification par clé avec SSH. Si vous n’êtes pas familier avec la gestion des clés SSH, nous vous recommandons vivement de consulter le [document NIST IR 7966](http://nvlpubs.nist.gov/nistpubs/ir/2015/NIST.IR.7966.pdf) intitulé « Sécurité de la gestion des accès interactifs et automatisés à l’aide de Secure Shell (SSH) ».

## <a name="about-key-pairs"></a>À propos des paires de clés

Les paires de clés font référence aux fichiers de clé publique et privée utilisés par certains protocoles d’authentification. 

L’authentification par clé publique SSH utilise des algorithmes de chiffrement asymétriques pour générer deux fichiers de clé : un « privé » et l’autre « public ». Les fichiers de clé privée sont l’équivalent d’un mot de passe et doivent être protégés dans en toutes circonstances. Si quelqu’un détient votre clé privée, il peut se connecter en votre nom à n’importe quel serveur SSH auquel vous avez accès. La clé publique est placée sur le serveur SSH et peut être partagée sans compromettre la clé privée.

Lorsque vous utilisez l’authentification par clé avec un serveur SSH, le serveur et le client SSH comparent la clé publique du nom d’utilisateur fourni avec la clé privée. Si la clé publique ne peut pas être validée par rapport à la clé privée côté client, l’authentification échoue. 

L’authentification multifacteur peut être implémentée avec des paires de clés en exigeant qu’une phrase secrète soit fournie lors de la génération de la paire de clés (voir la génération de clés ci-dessous). Pendant l’authentification, l’utilisateur est invité à entrer la phrase secrète, qui est utilisée en plus de la clé privée sur le client SSH pour authentifier l’utilisateur. 

## <a name="host-key-generation"></a>Génération d’une clé d’hôte

Les clés publiques ont des exigences de liste de contrôle d’accès spécifiques qui, sur Windows, équivalent à ne permettre l’accès qu’aux administrateurs et au système. Pour faciliter cette opération, 

* Le module OpenSSHUtils PowerShell a été créé pour paramétrer correctement les listes de contrôle d’accès des clés et doit être installé sur le serveur
* Lors de la première utilisation de sshd, la paire de clés de l’hôte sera générée automatiquement. Si ssh-agent est en cours d’exécution, les clés sont automatiquement ajoutées au magasin local. 

Pour faciliter l’authentification par clé avec un serveur SSH, exécutez les commandes suivantes depuis une invite PowerShell avec élévation de privilèges :

```powershell

# Install the OpenSSHUtils module to the server. This will be valuable when deploying user keys.
Install-Module -Force OpenSSHUtils -Scope AllUsers

# Start the ssh-agent service to preserve the server keys
Start-Service ssh-agent

# Now start the sshd service
Start-Service sshd
```

Étant donné qu’aucun utilisateur n’est associé au service sshd, les clés de l’hôte sont stockées sous \ProgramData\ssh.


## <a name="user-key-generation"></a>Génération d’une clé utilisateur

Pour utiliser l’authentification par clé, vous devez d’abord générer des paires de clés publiques/privées pour votre client. À partir de PowerShell ou de cmd, utilisez ssh-keygen pour générer des fichiers de clé.

```powershell
cd ~\.ssh\
ssh-keygen
```

Ceci devrait afficher ce qui suit ou y ressembler (le « nom d’utilisateur » remplacé par votre nom d’utilisateur)

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\username\.ssh\id_ed25519):
```

Vous pouvez appuyer sur Entrée pour accepter la valeur par défaut ou indiquer un chemin d’accès où vous souhaitez que vos clés soient générées. À ce stade, vous serez invité à utiliser une phrase secrète pour chiffrer vos fichiers de clé privée.
La phrase secrète fonctionne avec le fichier de clé pour fournir une authentification à 2 facteurs. Pour cet exemple, nous laissons la phrase secrète vide. 

```
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in C:\Users\username\.ssh\id_ed25519.
Your public key has been saved in C:\Users\username\.ssh\id_ed25519.pub.
The key fingerprint is: 
SHA256:OIzc1yE7joL2Bzy8!gS0j8eGK7bYaH1FmF3sDuMeSj8 username@server@LOCAL-HOSTNAME

The key's randomart image is:
+--[ED25519 256]--+
|        .        |
|         o       |
|    . + + .      |
|   o B * = .     |
|   o= B S .      |
|   .=B O o       |
|  + =+% o        |
| *oo.O.E         |
|+.o+=o. .        |
+----[SHA256]-----+
```

Vous disposez maintenant d’une paire de clés publique/privée ED25519 (les fichiers. pub sont des clés publiques et les autres sont des clés privées) :

```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/28/2018  11:09 AM           1679 id_ed25519
-a----        9/28/2018  11:09 AM            414 id_ed25519.pub
```

Rappelez-vous que les fichiers de clés privées sont l’équivalent d’un mot de passe et que vous devez les protéger de la même manière que votre mot de passe.
Pour vous aider, utilisez ssh-agent pour stocker en toute sécurité les clés privées dans un contexte de sécurité Windows associé à votre connexion Windows. Pour ce faire, démarrez le service ssh-agent en tant qu’administrateur et utilisez ssh-add pour stocker la clé privée. 

```powershell
# Make sure you're running as an Administrator
Start-Service ssh-agent

# This should return a status of Running
Get-Service ssh-agent

# Now load your key files into ssh-agent
ssh-add ~\.ssh\id_ed25519

```

Après avoir terminé ces étapes, chaque fois qu’une clé privée est nécessaire pour l’authentification de ce client, ssh-agent va automatiquement récupérer la clé privée locale et la transmettre à votre client SSH.

> [!NOTE]
> Il est fortement recommandé de sauvegarder votre clé privée dans un emplacement sécurisé, puis de la supprimer du système local *après* l’avoir ajoutée à ssh-agent.
> La clé privée ne peut pas être récupérée à partir de l’agent.
> Si vous perdez l’accès à la clé privée, vous devrez créer une nouvelle paire de clés et mettre à jour la clé publique sur tous les systèmes avec lesquels vous interagissez.

## <a name="deploying-the-public-key"></a>Déploiement de la clé publique

Pour utiliser la clé utilisateur créée ci-dessus, la clé publique doit être placée sur le serveur dans un fichier texte appelé *authorized_keys* sous users\username\.ssh\. Les outils OpenSSH incluent le point de connexion de service, un utilitaire de transfert de fichiers sécurisé conçu pour vous aider.

Pour déplacer le contenu de votre clé publique (~\.ssh\id_ed25519.pub) dans un fichier texte appelé authorized_keys dans ~\.SSH \ sur votre serveur/hôte.

Cet exemple utilise la fonction Repair-AuthorizedKeyPermissions du module OpenSSHUtils ayant été précédemment installé sur l’hôte à l’aide des instructions ci-dessus.

```powershell
# Make sure that the .ssh directory exists in your server's home folder
ssh user1@domain1@contoso.com mkdir C:\users\user1\.ssh\

# Use scp to copy the public key file generated previously to authorized_keys on your server
scp C:\Users\user1\.ssh\id_ed25519.pub user1@domain1@contoso.com:C:\Users\user1\.ssh\authorized_keys

# Appropriately ACL the authorized_keys file on your server  
ssh --% user1@domain1@contoso.com powershell -c $ConfirmPreference = 'None'; Repair-AuthorizedKeyPermission C:\Users\user1\.ssh\authorized_keys
```

Ces étapes permettent de terminer la configuration requise pour utiliser l’authentification par clés avec SSH sur Windows.
L’utilisateur peut ensuite se connecter à l’hôte sshd à partir de n’importe quel client disposant de la clé privée.

