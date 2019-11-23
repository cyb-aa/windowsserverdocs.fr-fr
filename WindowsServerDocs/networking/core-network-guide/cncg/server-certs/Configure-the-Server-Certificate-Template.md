---
title: Configurer le modèle de certificat de serveur
description: Cette rubrique fait partie du guide déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 X.
manager: brianlic
ms.topic: article
ms.assetid: 8ff610e2-43ca-407f-a828-06d9366e02f0
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 35d5875c78dcd92f3b40b919568dabcf0d45d673
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356758"
---
# <a name="configure-the-server-certificate-template"></a>Configurer le modèle de certificat de serveur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour configurer le modèle de certificat que Active Directory les services de certificats&reg; (AD CS) utilisent comme base pour les certificats de serveur inscrits sur les serveurs de votre réseau.  
  
Lors de la configuration de ce modèle, vous pouvez spécifier les serveurs en Active Directory groupe qui doit recevoir automatiquement un certificat de serveur à partir des services AD CS.   
  
La procédure suivante contient des instructions pour configurer le modèle afin d’émettre des certificats pour tous les types de serveur suivants :  
  
- Les serveurs qui exécutent le service d’accès à distance, y compris les serveurs de passerelle RAS, qui sont membres du groupe de **serveurs RAS et IAS** .  
- Les serveurs qui exécutent le service NPS (Network Policy Server) qui sont membres du groupe de **serveurs RAS et IAS** .  
  
Pour être autorisé à effectuer cette procédure, vous devez faire partie du groupe **Administrateurs de l'entreprise** et du groupe **Admins du domaine** du domaine racine.  
  
### <a name="to-configure-the-certificate-template"></a>Pour configurer le modèle de certificat  
  
1.  Sur CA1, dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur **autorité de certification**. La console MMC (Microsoft Management Console) de l’autorité de certification s’ouvre.  
  
2.  Dans la console MMC, double-cliquez sur le nom de l’autorité de certification, cliquez avec le bouton droit sur **modèles de certificats**, puis cliquez sur **gérer**.  
  
3.  La console modèles de certificats s’ouvre. Tous les modèles de certificat s’affichent dans le volet d’informations.  
  
4.  Dans le volet d’informations, cliquez sur le modèle **serveur RAS et IAS** .  
  
5.  Cliquez sur le menu **action** , puis sur **dupliquer le modèle**. La boîte de dialogue **Propriétés** du modèle s’ouvre.  
  
6.  Cliquez sur l’onglet **Sécurité** .   
  
7.  Sous l’onglet **sécurité** , sous **noms de groupes ou d’utilisateurs**, cliquez sur **serveurs RAS et IAS**.  
  
8.  Dans **autorisations pour les serveurs RAS et IAS**, sous **autoriser**, vérifiez que **inscrire** est sélectionné, puis activez la case à cocher **inscription automatique** . Cliquez sur **OK**, puis fermez la console MMC modèles de certificats.  
  
9.  Dans la console MMC Autorité de certification, cliquez sur **modèles de certificats**. Dans le menu **action** , pointez sur **nouveau**, puis cliquez sur **modèle de certificat à délivrer**. La boîte de dialogue **Activer les modèles de certificat** s'ouvre.  
  
10. Dans **activer les modèles de certificats**, cliquez sur le nom du modèle de certificat que vous venez de configurer, puis cliquez sur **OK**. Par exemple, si vous n’avez pas modifié le nom du modèle de certificat par défaut, cliquez sur **copie de Ras et serveur IAS**, puis cliquez sur **OK**.  
  


