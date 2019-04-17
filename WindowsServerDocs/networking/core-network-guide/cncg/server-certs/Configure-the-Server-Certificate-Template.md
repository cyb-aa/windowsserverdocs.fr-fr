---
title: Configurer le modèle de certificat de serveur
description: Cette rubrique fait partie du guide de certificats de serveur de déploiement pour 802.1 X câblé et sans fil déploiements
manager: brianlic
ms.topic: article
ms.assetid: 8ff610e2-43ca-407f-a828-06d9366e02f0
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e747fedd74dfc55c2c4df457d7f107b618423b8e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-server-certificate-template"></a>Configurer le modèle de certificat de serveur

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour configurer le modèle de certificat ActiveDirectory&reg; Services de certificats (ADCS) utilisent comme base pour les certificats de serveur inscrits sur les serveurs de votre réseau.  
  
Lors de la configuration de ce modèle, vous pouvez spécifier les serveurs par groupe d’ActiveDirectory qui doit recevoir automatiquement un certificat de serveur à partir des services ADCS.   
  
La procédure ci-dessous inclut des instructions pour configurer le modèle pour émettre des certificats à tous les types de serveur suivants:  
  
- Les serveurs qui exécutent le service d’accès à distance, y compris les serveurs de passerelle, qui sont membres de la **serveurs RAS et IAS** groupe.  
- Les serveurs qui exécutent le service de serveur NPS (Network Policy) qui sont membres de la **serveurs RAS et IAS** groupe.  
  
L’appartenance dans les deux **administrateurs de l’entreprise** et du domaine racine **Admins du domaine** groupe est la condition minimale requise pour effectuer cette procédure.  
  
### <a name="to-configure-the-certificate-template"></a>Pour configurer le modèle de certificat  
  
1.  Sur CA1, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **autorité de Certification**. La Certification autorité Console MMC (MicrosoftManagement) s’ouvre.  
  
2.  Dans la console MMC, double-cliquez sur le nom de l’autorité de certification, faites un clic droit **modèles de certificats**, puis cliquez sur **gérer**.  
  
3.  La console Modèles de certificats s’ouvre. Tous les modèles de certificats sont affichés dans le volet d’informations.  
  
4.  Dans le volet d’informations, cliquez sur le **serveur RAS et IAS** modèle.  
  
5.  Cliquez sur le **Action** menu, puis cliquez sur **dupliquer le modèle**. Le modèle **propriétés** boîte de dialogue s’ouvre.  
  
6.  Cliquez sur le **sécurité** onglet.   
  
7.  Sur le **sécurité** onglet **noms d’utilisateur ou groupe**, cliquez sur **serveurs RAS et IAS**.  
  
8.  Dans **autorisations pour les serveurs RAS et IAS**, sous **autoriser**, assurez-vous que **inscription** est activée, puis sélectionnez le **inscription automatique** case à cocher. Cliquez sur **OK**, fermez la console Modèles de certificat.  
  
9.  Dans la console MMC Autorité de Certification, cliquez sur **modèles de certificats**. Sur le **Action** menu, pointez sur **New**, puis cliquez sur **modèle de certificat à délivrer**. Le **activer les modèles de certificat** boîte de dialogue s’ouvre.  
  
10. Dans **activer les modèles de certificat**, cliquez sur le nom du modèle de certificat que vous venez de configurer, puis cliquez sur **OK**. Par exemple, si vous n’avez pas modifié le nom de modèle de certificat par défaut, cliquez sur **copie de serveur RAS et IAS**, puis cliquez sur **OK**.  
  


