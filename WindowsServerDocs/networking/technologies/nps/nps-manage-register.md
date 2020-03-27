---
title: Inscrire un serveur NPS dans un domaine Active Directory
description: Vous pouvez utiliser cette rubrique pour inscrire un serveur exécutant NPS (Network Policy Server) dans Windows Server 2016 dans le domaine par défaut NPS ou dans un autre domaine.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 63d630250b0b24937a3dfc01bcba7ec63faa3c3e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315966"
---
# <a name="register-an-nps-in-an-active-directory-domain"></a>Inscrire un serveur NPS dans un domaine Active Directory

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour inscrire un serveur exécutant NPS (Network Policy Server) dans Windows Server 2016 dans le domaine par défaut NPS ou dans un autre domaine.

## <a name="register-an-nps-in-its-default-domain"></a>Inscrire un serveur NPS dans son domaine par défaut

Vous pouvez utiliser cette procédure pour inscrire un serveur NPS dans le domaine dans lequel le serveur est membre d’un domaine. 

NPSs doit être inscrit dans Active Directory pour qu’il soit autorisé à lire les propriétés de numérotation des comptes d’utilisateur pendant le processus d’autorisation. L’inscription d’un serveur NPS ajoute le serveur au groupe **serveurs RAS et IAS** dans Active Directory.

Pour effectuer ces procédures, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.

### <a name="to-register-an-nps-in-its-default-domain"></a>Pour inscrire un serveur NPS dans son domaine par défaut


1. Sur le serveur NPS, dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur serveur NPS ( **Network Policy Server**). La console du serveur de stratégie réseau s’ouvre.

2. Cliquez avec le bouton droit sur **NPS (local)** , puis cliquez sur **inscrire le serveur dans Active Directory**. La boîte de dialogue **Serveur NPS (Network Policy Server)** s’ouvre.

3. Dans **serveur de stratégie réseau**, cliquez sur **OK**, puis de nouveau sur **OK** .

## <a name="register-an-nps-in-another-domain"></a>Inscrire un serveur NPS dans un autre domaine

Pour fournir à un serveur NPS l’autorisation de lire les propriétés de numérotation des comptes d’utilisateur dans Active Directory, le serveur NPS doit être enregistré dans le domaine dans lequel se trouvent les comptes.

Vous pouvez utiliser cette procédure pour inscrire un serveur NPS dans un domaine où le serveur NPS n’est pas membre du domaine.

Pour effectuer ces procédures, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.

### <a name="to-register-an-nps-in-another-domain"></a>Pour inscrire un serveur NPS dans un autre domaine

1. Sur le contrôleur de domaine, dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Active Directory les utilisateurs et les ordinateurs**. La console Utilisateurs et ordinateurs Active Directory s'ouvre.

2. Dans l’arborescence de la console, accédez au domaine dans lequel vous souhaitez que le serveur NPS lise les informations du compte d’utilisateur, puis cliquez sur le dossier **utilisateurs** . 

3. Dans le volet d’informations, cliquez avec le bouton droit sur **serveurs RAS et IAS**, puis cliquez sur **Propriétés**. La boîte de dialogue **Propriétés des serveurs RAS et IAS** s’ouvre.

4. Dans la boîte de dialogue **Propriétés des serveurs RAS et IAS** , cliquez sur l’onglet **membres** , ajoutez chacun des NPSs que vous souhaitez inscrire dans le domaine, puis cliquez sur **OK**.


### <a name="to-register-an-nps-in-another-domain-by-using-netsh-commands-for-nps"></a>Pour inscrire un serveur NPS dans un autre domaine à l’aide des commandes netsh pour NPS

1. Ouvrez l’invite de commandes ou Windows PowerShell. 

2. Tapez la commande suivante à l’invite de commandes : **netsh nps add registeredserver** &nbsp;*Domain* &nbsp;*Server*, puis appuyez sur entrée.

>[!NOTE]
>Dans la commande précédente, *domaine* est le nom de domaine DNS du domaine dans lequel vous voulez inscrire le serveur NPS et *Server* est le nom de l’ordinateur NPS.

