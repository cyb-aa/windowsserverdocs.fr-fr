---
title: Vérifier l’inscription de serveur d’un certificat de serveur
description: Cette rubrique fait partie du guide des certificats de serveur de déploiement pour les déploiements de sans fil et câblé à 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: bd80a018-5a30-47c3-89fc-aacb9f5ad298
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 45ba7a9a7fc5b9622ab1b9a94f38f4bf4de13192
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850900"
---
# <a name="verify-server-enrollment-of-a-server-certificate"></a>Vérifier l’inscription de serveur d’un certificat de serveur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour vérifier que vos serveurs de serveur NPS (Network Policy Server) ont inscrit un certificat de serveur à partir de l’autorité de certification (CA).   
  
>[!NOTE]  
>L’appartenance à la **Admins du domaine** groupe est le minimum requis pour réaliser ces procédures.  
  
## <a name="verify-network-policy-server-nps-enrollment-of-a-server-certificate"></a>Vérifier l’inscription de serveur NPS (Network Policy Server) d’un certificat de serveur  
  
Étant donné que le serveur NPS est utilisé pour authentifier et autoriser les demandes de connexion réseau, il est important de s’assurer que le certificat de serveur que vous avez émis à NPSs est valide lorsqu’il est utilisé dans les stratégies de réseau.  
  
Pour vérifier qu’un certificat de serveur est correctement configuré et qu’il est inscrit pour le serveur NPS, vous devez configurer une stratégie de réseau de test et autoriser le serveur NPS vérifier que le serveur NPS peut utiliser le certificat pour l’authentification.  
  
### <a name="to-verify-nps-enrollment-of-a-server-certificate"></a>Pour vérifier l’inscription d’un certificat de serveur NPS  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Network Policy Server**. Le réseau stratégie serveur Console MMC (Microsoft Management) s’ouvre.  
  
2.  Double-cliquez sur **stratégies**, avec le bouton droit **stratégies réseau**, puis cliquez sur **New**. L’Assistant Nouvelle stratégie de réseau s’ouvre.  
  
3.  Dans **spécifier un nom de stratégie de réseau et le Type de connexion**, dans **nom de la stratégie**, type **tester la stratégie**. Vérifiez que **Type de serveur d’accès réseau** a la valeur **Unspecified**, puis cliquez sur **suivant**.  
  
4.  Dans **spécifier des Conditions**, cliquez sur **ajouter**. Dans **sélectionner condition**, cliquez sur **les groupes Windows**, puis cliquez sur **ajouter**.  
  
5.  Dans **groupes**, cliquez sur **ajouter des groupes**. Dans **sélectionner un groupe**, type **utilisateurs du domaine**, puis appuyez sur ENTRÉE. Cliquez sur **OK**, puis cliquez sur **Suivant**.  
  
6.  Dans **spécifier l’autorisation d’accès**, vérifiez que **accès accordé** est sélectionnée, puis cliquez sur **suivant**.  
  
7.  Dans **configurer des méthodes d’authentification**, cliquez sur **ajouter**. Dans **EAP ajouter**, cliquez sur **Microsoft : PEAP (Protected EAP)**, puis cliquez sur **OK**. Dans **Types EAP**, sélectionnez **Microsoft : PEAP (Protected EAP)**, puis cliquez sur **modifier**. Le **modifier les propriétés EAP protégées** boîte de dialogue s’ouvre.  
  
8.  Dans le **modifier les propriétés EAP protégées** boîte de dialogue **certificat délivré à**, NPS affiche le nom de votre certificat de serveur dans le format *ComputerName*. *Domaine*. Par exemple, si votre serveur NPS est nommé NPS-01 et votre domaine est exemple.com, NPS affiche le certificat **NPS-01.example.com**. En outre, dans **émetteur**, le nom de votre autorité de certification s’affiche, puis, dans **date d’Expiration**, la date d’expiration du certificat du serveur s’affiche. Cet exemple montre que le serveur NPS a inscrit un certificat de serveur valide qu’il peut utiliser pour prouver son identité aux ordinateurs clients qui tentent d’accéder au réseau via vos serveurs d’accès réseau, tels que les serveurs de réseau privé virtuel (VPN), 802.1 compatibles X points d’accès sans fil, les serveurs de passerelle Bureau à distance et 802. 1 X compatible Ethernet bascule.  
  
    > [!IMPORTANT]  
    > Si le serveur NPS n’affiche pas d’un certificat de serveur valide et si elle fournit le message de ce type de certificat est introuvable sur l’ordinateur local, il existe deux raisons possibles pour résoudre ce problème. Il est possible que la stratégie de groupe n’a pas actualisé correctement, et le serveur NPS n’a pas inscrit un certificat à partir de l’autorité de certification. Dans ce cas, redémarrez le serveur NPS. Lorsque l’ordinateur redémarre, la stratégie de groupe est actualisée, et vous pouvez effectuer cette procédure pour vérifier que le certificat de serveur est inscrit. Si l’actualisation de stratégie de groupe ne résout pas ce problème, le modèle de certificat, l’inscription automatique de certificat ou les deux ne sont pas configurés correctement. Pour résoudre ces problèmes, depuis le début de ce guide et effectuer toutes les étapes à nouveau pour vous assurer que les paramètres que vous avez fournies sont précises.  
  
9. Lorsque vous avez vérifié la présence d’un certificat de serveur valide, vous pouvez cliquer sur **OK** et **Annuler** pour quitter l’Assistant Nouvelle stratégie de réseau.  
  
    > [!NOTE]  
    > Étant donné que l’Assistant ne s’effectuent pas, la stratégie de réseau de test n’est pas créée dans NPS.  
  


