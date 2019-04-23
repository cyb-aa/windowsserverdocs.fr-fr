---
title: Inscrire un serveur NPS dans un domaine Active Directory
description: Vous pouvez utiliser cette rubrique pour inscrire un serveur exécutant le serveur NPS dans Windows Server 2016 dans le domaine par défaut NPS ou dans un autre domaine.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4a289ec519e5107576becf2905cd881cf9def190
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877650"
---
# <a name="register-an-nps-in-an-active-directory-domain"></a>Inscrire un serveur NPS dans un domaine Active Directory

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour inscrire un serveur exécutant le serveur NPS dans Windows Server 2016 dans le domaine par défaut NPS ou dans un autre domaine.

## <a name="register-an-nps-in-its-default-domain"></a>Inscrire un serveur NPS dans son domaine par défaut

Vous pouvez utiliser cette procédure pour inscrire un serveur NPS dans le domaine où le serveur est membre d’un domaine. 

NPSs doit être inscrit dans Active Directory afin qu’ils aient l’autorisation de lire les propriétés de numérotation des comptes d’utilisateur pendant le processus d’autorisation. L’inscription d’un serveur NPS ajoute le serveur à la **serveurs RAS et IAS** groupe dans Active Directory.

Pour effectuer ces procédures, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.

### <a name="to-register-an-nps-in-its-default-domain"></a>Pour inscrire un serveur NPS dans son domaine par défaut


1. Sur le serveur NPS, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Network Policy Server**. La console du serveur NPS s’ouvre.

2. Avec le bouton droit **NPS (Local)**, puis cliquez sur **inscrire le serveur dans Active Directory**. La boîte de dialogue **Serveur NPS (Network Policy Server)** s’ouvre.

3. Dans **Network Policy Server**, cliquez sur **OK**, puis cliquez sur **OK** à nouveau.

## <a name="register-an-nps-in-another-domain"></a>Inscrire un serveur NPS dans un autre domaine

Pour fournir un serveur NPS avec l’autorisation de lire les propriétés de numérotation des comptes d’utilisateur dans Active Directory, le serveur NPS doit être inscrit dans le domaine où se trouvent les comptes.

Vous pouvez utiliser cette procédure pour inscrire un serveur NPS dans un domaine où le serveur NPS n’est pas membre du domaine.

Pour effectuer ces procédures, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.

### <a name="to-register-an-nps-in-another-domain"></a>Pour inscrire un serveur NPS dans un autre domaine

1. Sur le contrôleur de domaine, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Active Directory Users and Computers**. La console Utilisateurs et ordinateurs Active Directory s'ouvre.

2. Dans l’arborescence de la console, accédez au domaine où vous souhaitez le serveur NPS pour lire les informations de compte d’utilisateur, puis cliquez sur le **utilisateurs** dossier. 

3. Dans le volet détails, cliquez sur **serveurs RAS et IAS**, puis cliquez sur **propriétés**. Le **propriétés RAS et IAS serveurs** boîte de dialogue s’ouvre.

4. Dans le **propriétés RAS et IAS serveurs** boîte de dialogue, cliquez sur le **membres** chacun des NPSs que vous souhaitez inscrire dans le domaine, puis cliquez sur Ajouter, onglet **OK**.


### <a name="to-register-an-nps-in-another-domain-by-using-netsh-commands-for-nps"></a>Pour inscrire un serveur NPS dans un autre domaine à l’aide des commandes Netsh pour NPS

1. Ouvrez une invite de commandes ou windows PowerShell. 

2. Tapez la commande suivante à l’invite de commandes : **netsh nps ajouter registeredserver** &nbsp; *domaine* &nbsp; *server*, puis appuyez sur ENTRÉE.

>[!NOTE]
>Dans la commande précédente, *domaine* est le nom de domaine DNS du domaine où vous voulez inscrire le serveur NPS, et *serveur* est le nom de l’ordinateur du serveur NPS.

