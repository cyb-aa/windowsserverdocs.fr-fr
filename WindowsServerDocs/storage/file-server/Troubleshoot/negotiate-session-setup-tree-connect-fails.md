---
title: Négociation, configuration de session et échecs de connexion d’arborescence
description: Explique comment résoudre les problèmes liés à la négociation, à la configuration de session et aux échecs de connexion d’arborescence.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 13124176e530aa7b74d18a38c906bf5297be511e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815382"
---
# <a name="negotiate-session-setup-and-tree-connect-failures"></a>Négociation, configuration de session et échecs de connexion d’arborescence

Cet article explique comment résoudre les problèmes qui se produisent pendant une demande de négociation SMB, de configuration de session et de connexion d’arborescence.

## <a name="negotiate-fails"></a>Échec de Negotiate

Le serveur SMB reçoit une demande de négociation SMB d’un client SMB. La connexion expire et est réinitialisée après 60 secondes. Il peut y avoir un message d’accusé de réception après environ 200 microsecondes.

Ce problème est le plus souvent dû à un programme antivirus.

Si vous utilisez Windows Server 2008 R2, il existe des correctifs pour résoudre ce problème. Assurez-vous que le client SMB et le serveur SMB sont à jour.

## <a name="session-setup-fails"></a>Échec de la configuration de session

Le serveur SMB reçoit une SESSION SMB\_demande d’installation à partir d’un client SMB, mais n’a pas pu répondre.

Si le nom de domaine complet (FQDN) ou le nom NetBIOS (Network Basic Input/Output System) du serveur est’sed dans le chemin d’accès UNC (Universal Naming Convention), Windows utilise Kerberos pour l’authentification.

Après la réponse Negotiate, vous tenterez d’obtenir un ticket Kerberos pour le nom de principal du service (SPN) Common Internet File System (CIFS) du serveur. Examinez le trafic Kerberos sur le port TCP 88 pour vous assurer qu’il n’y a pas d’erreurs Kerberos lorsque le client SMB obtient le jeton.

> [!NOTE]
> Les erreurs qui se produisent pendant la pré-authentification Kerberos sont OK. Les erreurs qui se produisent après l’authentification préalable Kerberos (instances dans lesquelles l’authentification ne fonctionne pas) sont les erreurs qui ont provoqué le problème SMB.

En outre, effectuez les vérifications suivantes :

- Examinez l’objet blob de sécurité de la SESSION SMB\_demande d’installation pour vous assurer que les informations d’identification correctes sont envoyées.

- Essayez de désactiver le renforcement de nom de serveur SMB (**SmbServerNameHardeningLevel = 0**).

- Assurez-vous que le serveur SMB dispose d’un SPN lorsqu’il est accessible par le biais d’un enregistrement DNS CNAMe.

- Assurez-vous que la signature SMB fonctionne. (Ceci est particulièrement important pour les appareils tiers plus anciens.)

## <a name="tree-connect-fails"></a>Échec de la connexion de l’arborescence

Assurez-vous que les informations d’identification du compte d’utilisateur disposent d’autorisations de partage et de système de fichiers NT (NTFS) sur le dossier.

La cause des erreurs de connexion de l’arborescence commune se trouve dans [3.3.5.7 recevant une arborescence SMB2\_demande Connect](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/652e0c14-5014-4470-999d-b174d7b2da87). Voici les solutions pour deux codes d’État courants.

\[État\_nom de\_réseau\_incorrect\]

Assurez-vous que le partage existe sur le serveur et qu’il est correctement orthographié dans la demande du client SMB.

État de \[\_accès refusé\_\]

Vérifiez que le disque et le dossier utilisés par le partage existent et sont accessibles.

Si vous utilisez SMBv3 ou une version ultérieure, vérifiez si le serveur et le partage nécessitent un chiffrement, mais que le client ne prend pas en charge le chiffrement. Pour ce faire, effectuez les actions suivantes :

- Vérifiez le serveur en exécutant la commande suivante.

  ```PowerShell
  Get-SmbServerConfiguration | select Encrypt*
  ```

  Si EncryptData et RejectUnencryptedAccess ont la valeur true, le serveur nécessite un chiffrement.

- Vérifiez le partage en exécutant la commande suivante :

  ```PowerShell
  Get-SmbShare | select name, EncryptData  
  ```

  Si EncryptData a la valeur true sur le partage et que RejectUnencryptedAccess a la valeur true sur le serveur, le chiffrement est requis par le partage

Suivez ces instructions lors de la résolution des problèmes :

- Windows 8, Windows Server 2012 et les versions ultérieures de Windows prennent en charge le chiffrement côté client (SMBv3 et versions ultérieures).

- Windows 7, Windows Server 2008 R2 et les versions antérieures de Windows ne prennent pas en charge le chiffrement côté client.

- Samba et les appareils tiers peuvent ne pas prendre en charge le chiffrement. Pour plus d’informations, vous devrez peut-être consulter la documentation du produit.

## <a name="references"></a>Références

Pour plus d’informations, consultez les articles suivants.

[3.3.5.4 recevant une demande de négociation SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/b39f253e-4963-40df-8dff-2f9040ebbeb1)

[3.3.5.5 recevant une SESSION SMB2\_demande d’installation](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/e545352b-9f2b-4c5e-9350-db46e4f6755e)

[3.3.5.7 recevant une arborescence SMB2\_demande CONNECT](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/652e0c14-5014-4470-999d-b174d7b2da87?redirectedfrom=MSDN)
