---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, installer, configurer
contributor: maertendMSFT
author: maertendMSFT
title: Configuration du serveur de OpenSSH pour Windows
ms.product: w10
ms.openlocfilehash: 3e2981cbbdfe34bf28a77d5121bff0b663e0d3bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837150"
---
# <a name="openssh-key-management"></a>Gestion de la clé OpenSSH

La plupart des authentification dans les environnements Windows est effectuée avec une paire nom d’utilisateur-mot de passe.
Cela fonctionne bien pour les systèmes qui partagent le même domaine. Lorsque vous travaillez sur plusieurs domaines, tels qu’entre les systèmes locaux et hébergés dans le cloud, il devient plus difficile.

En comparaison, les environnements Linux utilisent généralement des paires de clé publique-clé privée pour l’authentification d’un lecteur.
OpenSSH inclut des outils pour vous aider à prendre en charge, en particulier :

* __SSH-keygen__ pour la génération de clés sécurisées
* __SSH-agent__ et __ssh-ajouter__ pour stocker en toute sécurité les clés privées
* __SCP__ et __sftp__ à copier en toute sécurité des fichiers de clés publiques au cours de la première utilisation d’un serveur

Ce document fournit une vue d’ensemble de l’utilisation de ces outils sur Windows pour commencer à utiliser l’authentification par clé avec SSH. Si vous n’êtes pas familiarisé avec la gestion de clés SSH, nous recommandons fortement de consulter [document NIST IR 7966](http://nvlpubs.nist.gov/nistpubs/ir/2015/NIST.IR.7966.pdf) intitulée « Sécurité de Interactive et automatisée Access Management Using SSH (Secure Shell) ».

## <a name="about-key-pairs"></a>Sur les paires de clés

Paires de clés font référence aux fichiers de clés publiques et privées qui sont utilisés par certains protocoles d’authentification. 

L’authentification de clé publique SSH utilise des algorithmes de chiffrement asymétriques pour générer deux fichiers de clés : une « privé » et l’autre « public ». Les fichiers de clés privées sont l’équivalent d’un mot de passe et doivent être protégées en toutes circonstances. Si un utilisateur acquiert votre clé privée, ils peuvent connectez-vous en tant que vous à n’importe quel serveur SSH que vous avez accès. La clé publique est ce qui est placé sur le serveur SSH et peut être partagé sans compromettre la clé privée.

Lorsque vous utilisez l’authentification par clé avec un serveur SSH, le serveur SSH et le client comparent la clé publique pour le nom d’utilisateur fourni par rapport à la clé privée. Si la clé publique ne peut pas être validée par rapport à la clé privée du côté client, l’authentification échoue. 

L’authentification multifacteur peut être implémentée avec des paires de clés en exigeant qu’une phrase secrète être fourni lors de la paire de clés est générée (voir ci-dessous la génération de clés). Lors de l’authentification de que l’utilisateur est invité à entrer la phrase secrète, qui est utilisé pour authentifier l’utilisateur, ainsi que la présence de la clé privée sur le client SSH. 

## <a name="host-key-generation"></a>Génération de clés d’hôte

Clés publiques ont ACL des exigences spécifiques, sur Windows, sont équivalentes à autorisant uniquement l’accès aux administrateurs et système. Pour faciliter cette opération, 

* Le module PowerShell de OpenSSHUtils a été créé pour définir la clé ACL correctement et doit être installé sur le serveur
* Sur la première utilisation de sshd, la paire de clés pour l’hôte sera générée automatiquement. Si ssh-agent est en cours d’exécution, les clés seront automatiquement ajoutés au magasin local. 

Pour faciliter l’authentification par clé avec un serveur SSH, exécutez les commandes suivantes à partir d’une invite de PowerShell avec élévation de privilèges :

```powershell

# Install the OpenSSHUtils module to the server. This will be valuable when deploying user keys.
Install-Module -Force OpenSSHUtils -Scope AllUsers

# Start the ssh-agent service to preserve the server keys
Start-Service ssh-agent

# Now start the sshd service
Start-Service sshd
```

Dans la mesure où il n’existe aucun utilisateur associé au service sshd, les clés d’hôte sont stockées sous \ProgramData\ssh.


## <a name="user-key-generation"></a>Génération de la clé utilisateur

Pour utiliser l’authentification basée sur la clé, vous devez tout d’abord générer certaines paires de clés publique/privée pour votre client. Dans PowerShell ou cmd, utilisez ssh-keygen pour générer des fichiers de clés.

```powershell
cd ~\.ssh\
ssh-keygen
```

Ceci devrait afficher quelque chose comme ce qui suit (où « username » est remplacé par votre nom d’utilisateur)

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\username\.ssh\id_ed25519):
```

Vous pouvez appuyer sur ENTRÉE pour accepter la valeur par défaut, ou spécifier un chemin d’accès où vous souhaitez que vos clés à générer. À ce stade, vous allez être invité à utiliser une phrase secrète pour chiffrer vos fichiers de clé privée.
La phrase secrète fonctionne avec le fichier de clé pour fournir l’authentification à 2 facteurs. Pour cet exemple, nous allons laisser vide la phrase secrète. 

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

Maintenant vous disposez d’une paire de ED25519 clée publique/privée (les fichiers .pub sont des clés publiques et les autres sont des clés privées) :

```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/28/2018  11:09 AM           1679 id_ed25519
-a----        9/28/2018  11:09 AM            414 id_ed25519.pub
```

N’oubliez pas que les fichiers de clés privées sont que l’équivalent d’un mot de passe doit être protégée de la même façon que vous protégez votre mot de passe.
Pour vous aider à qui, utilisez ssh-agent pour stocker en toute sécurité les clés privées dans un contexte de sécurité Windows, associées à votre connexion de Windows. Pour ce faire, démarrez le service ssh-agent en tant qu’administrateur et utilisez ssh-add pour stocker la clé privée. 

```powershell
# Make sure you're running as an Administrator
Start-Service ssh-agent

# This should return a status of Running
Get-Service ssh-agent

# Now load your key files into ssh-agent
ssh-add ~\.ssh\id_ed25519

```

Après avoir effectué ces étapes, chaque fois qu’une clé privée est nécessaire pour l’authentification à partir de ce client, ssh-agent automatiquement récupérer la clé privée locale et passez-le à votre client SSH.

> [!NOTE]
> Il est fortement recommandé de sauvegarder votre clé privée dans un emplacement sécurisé, puis de supprimer le système local, *après* ajoutant à ssh-agent.
> La clé privée ne peut pas être récupérée à partir de l’agent.
> Si vous perdez l’accès à la clé privée, vous seriez obligé de créer une nouvelle paire de clés et de mettre à jour de la clé publique sur tous les systèmes, avec que vous pouvez interagir.

## <a name="deploying-the-public-key"></a>Déploiement de la clé publique

Pour utiliser la clé de l’utilisateur qui a été créée ci-dessus, la clé publique doit être placée sur le serveur dans un fichier texte appelé *authorized_keys* sous users\username\ssh. Les outils de OpenSSH incluent scp, qui est un utilitaire de transfert de fichiers sécurisé, pour aider à ce sujet.

Pour déplacer le contenu de votre clé publique (~\.ssh\id_ed25519.pub) dans un fichier texte fichier nommé authorized_keys ~\.ssh\ sur votre serveur/hôte.

Cet exemple utilise la fonction de réparation-AuthorizedKeyPermissions dans le module OpenSSHUtils qui a été précédemment installé sur l’ordinateur hôte dans les instructions ci-dessus.

```powershell
# Make sure that the .ssh directory exists in your server's home folder
ssh user1@domain1@contoso.com mkdir C:\users\user1\.ssh\

# Use scp to opy the public key file generated previously to authorized_keys on your server
scp C:\Users\user1\.ssh\id_ed25519.pub user1@domain1@contoso.com:C:\Users\user1\.ssh\authorized_keys

# Appropriately ACL the authorized_keys file on your server  
ssh --% user1@domain1@contoso.com powershell -c $ConfirmPreference = 'None'; Repair-AuthorizedKeyPermission C:\Users\user1\.ssh\authorized_keys
```

Ces étapes sont terminées à la configuration requise pour utiliser l’authentification basée sur les clés avec SSH sur Windows.
Après cela, l’utilisateur peut se connecter à l’hôte sshd à partir de n’importe quel client qui possède la clé privée.

