---
title: Désinscrire un serveur NPS d'un domaine Active Directory
description: Vous pouvez utiliser cette rubrique pour inscrire un serveur exécutant le serveur NPS dans Windows Server 2016 dans le domaine par défaut NPS ou dans un autre domaine.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8fe4773efd89aeb413b3793f874ad6a1b030294a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864350"
---
# <a name="unregister-an-nps-from-an-active-directory-domain"></a>Désinscrire un serveur NPS d'un domaine Active Directory

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

En cours de la gestion de votre déploiement de NPS, vous pouvez s’avérer utile pour déplacer un serveur NPS vers un autre domaine, pour remplacer un serveur NPS, ou mettre hors service un serveur NPS. 

Lorsque vous déplacez ou retirez un serveur NPS, vous pouvez annuler l’inscription du serveur NPS dans les domaines Active Directory où le serveur NPS a l’autorisation de lire les propriétés des comptes d’utilisateur dans Active Directory.

Pour effectuer ces procédures, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.

## <a name="to-unregister-an-nps"></a>Pour annuler l’inscription d’un serveur NPS

1. Sur le contrôleur de domaine, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Active Directory Users and Computers**. La console Utilisateurs et ordinateurs Active Directory s'ouvre.

2. Cliquez sur **utilisateurs**, puis double-cliquez sur **serveurs RAS et IAS**.

3. Cliquez sur le **membres** onglet, puis sélectionnez le serveur NPS que vous souhaitez annuler l’inscription.

4. Cliquez sur **supprimer**, cliquez sur **Oui**, puis cliquez sur **OK**.

