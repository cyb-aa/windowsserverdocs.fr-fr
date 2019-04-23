---
title: Configurer le modèle de certificat de serveur
description: Cette rubrique fait partie du guide des certificats de serveur de déploiement pour les déploiements de sans fil et câblé à 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 8ff610e2-43ca-407f-a828-06d9366e02f0
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 238579c945821d19e45dad7e623450d598830596
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886120"
---
# <a name="configure-the-server-certificate-template"></a>Configurer le modèle de certificat de serveur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour configurer le modèle de certificat qu’Active Directory&reg; Services de certificats (AD CS) utilisent comme base pour les certificats de serveur qui sont inscrits sur les serveurs sur votre réseau.  
  
Lors de la configuration de ce modèle, vous pouvez spécifier les serveurs par groupe Active Directory qui doit recevoir automatiquement un certificat de serveur à partir des services AD CS.   
  
La procédure ci-dessous inclut des instructions pour configurer le modèle pour émettre des certificats pour tous les types de serveur suivants :  
  
- Serveurs qui exécutent le service d’accès à distance, y compris les serveurs de passerelle RAS, qui sont membres de la **serveurs RAS et IAS** groupe.  
- Les serveurs qui exécutent le service de serveur NPS (Network Policy Server) qui sont membres de la **serveurs RAS et IAS** groupe.  
  
Pour être autorisé à effectuer cette procédure, vous devez faire partie du groupe **Administrateurs de l'entreprise** et du groupe **Admins du domaine** du domaine racine.  
  
### <a name="to-configure-the-certificate-template"></a>Pour configurer le modèle de certificat  
  
1.  Dans CA1, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **autorité de Certification**. Le MMC Autorité de Certification Microsoft Management Console () s’ouvre.  
  
2.  Dans la console MMC, double-cliquez sur le nom de l’autorité de certification, cliquez sur **modèles de certificats**, puis cliquez sur **gérer**.  
  
3.  La console Modèles de certificats s’ouvre. Tous les modèles de certificats sont affichés dans le volet détails.  
  
4.  Dans le volet détails, cliquez sur le **serveur RAS et IAS** modèle.  
  
5.  Cliquez sur le **Action** menu, puis sur **dupliquer le modèle**. Le modèle **propriétés** boîte de dialogue s’ouvre.  
  
6.  Cliquez sur l’onglet **Sécurité** .   
  
7.  Sur le **sécurité** sous l’onglet **noms d’utilisateur ou groupe**, cliquez sur **serveurs RAS et IAS**.  
  
8.  Dans **autorisations pour les serveurs RAS et IAS**, sous **autoriser**, vérifiez que **inscription** est sélectionné, puis sélectionnez le **inscription automatique** vérifier zone. Cliquez sur **OK**, puis fermez la console Modèles de certificat.  
  
9.  Dans la console MMC Autorité de Certification, cliquez sur **modèles de certificats**. Sur le **Action** menu, pointez sur **New**, puis cliquez sur **modèle de certificat à délivrer**. La boîte de dialogue **Activer les modèles de certificat** s'ouvre.  
  
10. Dans **activer les modèles de certificat**, cliquez sur le nom du modèle de certificat que vous venez de configurer, puis cliquez sur **OK**. Par exemple, si vous n’avez pas modifié le nom de modèle de certificat par défaut, cliquez sur **copie de serveur RAS et IAS**, puis cliquez sur **OK**.  
  


