---
title: Désinscrire un serveur NPS d'un domaine Active Directory
description: Vous pouvez utiliser cette rubrique pour inscrire un serveur exécutant NPS (Network Policy Server) dans Windows Server 2016 dans le domaine par défaut NPS ou dans un autre domaine.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 366e3e7eef6ac1e8682dd3064e0d133f21d1a8da
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315895"
---
# <a name="unregister-an-nps-from-an-active-directory-domain"></a>Désinscrire un serveur NPS d'un domaine Active Directory

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans le processus de gestion de votre déploiement NPS, il peut s’avérer utile de déplacer un serveur NPS vers un autre domaine, de remplacer un serveur NPS ou de retirer un serveur NPS. 

Lorsque vous déplacez ou désactivation d’un serveur NPS, vous pouvez annuler l’inscription du serveur NPS dans le Active Directory domaines où le serveur NPS est autorisé à lire les propriétés des comptes d’utilisateur dans Active Directory.

Pour effectuer ces procédures, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.

## <a name="to-unregister-an-nps"></a>Pour annuler l’inscription d’un serveur NPS

1. Sur le contrôleur de domaine, dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Active Directory les utilisateurs et les ordinateurs**. La console Utilisateurs et ordinateurs Active Directory s'ouvre.

2. Cliquez sur **utilisateurs**, puis double-cliquez sur **serveurs RAS et IAS**.

3. Cliquez sur l’onglet **membres** , puis sélectionnez le serveur NPS dont vous souhaitez annuler l’inscription.

4. Cliquez sur **supprimer**, sur **Oui**, puis sur **OK**.

