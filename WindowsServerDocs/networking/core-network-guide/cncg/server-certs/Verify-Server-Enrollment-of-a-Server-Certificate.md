---
title: Vérifiez le serveur d’inscription d’un certificat de serveur
description: Cette rubrique fait partie du guide de certificats de serveur de déploiement pour 802.1 X câblé et sans fil déploiements
manager: brianlic
ms.topic: article
ms.assetid: bd80a018-5a30-47c3-89fc-aacb9f5ad298
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d8ff51fa83972e2fc73ee54628eeb89e2927046d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="verify-server-enrollment-of-a-server-certificate"></a>Vérifiez le serveur d’inscription d’un certificat de serveur

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour vérifier que vos serveurs de serveur NPS (Network Policy) ont inscrit un certificat de serveur à partir de l’autorité de certification (CA).   
  
>[!NOTE]  
>L’appartenance au groupe le **Admins du domaine** groupe est la condition minimale requise pour effectuer ces procédures.  
  
## <a name="verify-network-policy-server-nps-enrollment-of-a-server-certificate"></a>Vérifier l’inscription de serveur NPS (Network Policy) d’un certificat de serveur  
  
Étant donné que le serveur NPS est utilisé pour authentifier et autoriser les demandes de connexion réseau, il est important de s’assurer que le certificat de serveur que vous avez émis aux serveurs NPS est valide lorsqu’il est utilisé dans les stratégies de réseau.  
  
Pour vérifier qu’un certificat de serveur est correctement configuré et qu’il est inscrit sur le serveur NPS, vous devez configurer une stratégie de réseau de test et autoriser NPS pour vérifier que le serveur NPS peut utiliser le certificat pour l’authentification.  
  
### <a name="to-verify-nps-server-enrollment-of-a-server-certificate"></a>Pour vérifier l’inscription de serveur NPS d’un certificat de serveur  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **serveur NPS**. Le réseau stratégie serveur Console MMC (Microsoft Management) s’ouvre.  
  
2.  Double-cliquez sur **stratégies**, avec le bouton droit **stratégies réseau**, puis cliquez sur **New**. L’Assistant de nouvelle stratégie de réseau s’ouvre.  
  
3.  Dans **spécifier le nom de stratégie du réseau et le Type de connexion**, dans **nom de la stratégie**, type **tester stratégie**. Vérifiez que **Type de serveur d’accès réseau** a la valeur **non spécifié**, puis cliquez sur **suivant**.  
  
4.  Dans **spécifier les Conditions**, cliquez sur **ajouter**. Dans **sélectionnez condition**, cliquez sur **groupes Windows**, puis cliquez sur **ajouter**.  
  
5.  Dans **groupes**, cliquez sur **ajouter des groupes**. Dans **sélectionner un groupe**, type **utilisateurs du domaine**, puis appuyez sur ENTRÉE. Cliquez sur **OK**, puis cliquez sur **suivant**.  
  
6.  Dans **spécifier l’autorisation d’accès**, assurez-vous que **accès accordé** est sélectionné, puis cliquez sur **suivant**.  
  
7.  Dans **configurer des méthodes d’authentification**, cliquez sur **ajouter**. Dans **EAP ajouter**, cliquez sur **Microsoft: PEAP (Protected EAP)**, puis cliquez sur **OK**. Dans **Types de protocoles EAP**, sélectionnez **Microsoft: PEAP (Protected EAP)**, puis cliquez sur **modifier**. Le **modifier les propriétés EAP protégées** boîte de dialogue s’ouvre.  
  
8.  Dans le **modifier les propriétés EAP protégées** la boîte de dialogue dans **certificat délivré à**, NPS affiche le nom de votre certificat de serveur dans le format *Nom_Ordinateur*. *Domaine*. Par exemple, si votre serveur NPS est nommé NPS-01 et votre domaine est example.com, NPS affiche le certificat **NPS-01.example.com**. En outre, dans **émetteur**, le nom de votre autorité de certification s’affiche et dans **date d’Expiration**, la date d’expiration du certificat du serveur s’affiche. Cela permet de démontrer que votre serveur NPS a inscrit un certificat de serveur valide qu’il peut utiliser pour prouver son identité aux ordinateurs clients qui tentent d’accéder au réseau par le biais de vos serveurs d’accès réseau, tels que les serveurs de réseau privé virtuel (VPN), 802.1 X sans fil points d’accès, les serveurs de passerelle Bureau à distance et 802. 1 des commutateurs Ethernet compatibles X.  
  
    > [!IMPORTANT]  
    > Si le serveur NPS n’affiche pas un certificat de serveur valide et qu’elle fournit le message ce certificat est introuvable sur l’ordinateur local, il existe deux raisons possibles pour ce problème. Il est possible que la stratégie de groupe n’a pas actualisé correctement, et le serveur NPS n’ayant pas inscrit un certificat à partir de l’autorité de certification. Dans ce cas, redémarrez le serveur NPS. Lorsque l’ordinateur redémarre, la stratégie de groupe est actualisée, et vous pouvez effectuer cette procédure pour vérifier que le certificat de serveur est inscrit. Si l’actualisation de stratégie de groupe ne résout pas ce problème, le modèle de certificat, l’inscription automatique de certificat ou les deux ne sont pas configurés correctement. Pour résoudre ces problèmes, depuis le début de ce guide et effectuer toutes les étapes pour vous assurer que les paramètres que vous avez fournies sont précises.  
  
9. Lorsque vous avez vérifié la présence d’un certificat de serveur valide, vous pouvez cliquer sur **OK** et **Annuler** pour quitter l’Assistant Nouvelle stratégie de réseau.  
  
    > [!NOTE]  
    > Étant donné que l’Assistant ne sont pas terminé, la stratégie de réseau de test n’est pas créée au serveur NPS.  
  


