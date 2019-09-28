---
title: Vérifier l’inscription de serveur d’un certificat de serveur
description: Cette rubrique fait partie du guide déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 X.
manager: brianlic
ms.topic: article
ms.assetid: bd80a018-5a30-47c3-89fc-aacb9f5ad298
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5f87db78d6f07d11c36193b1a56cf66bd44e7160
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356102"
---
# <a name="verify-server-enrollment-of-a-server-certificate"></a>Vérifier l’inscription de serveur d’un certificat de serveur

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour vérifier que vos serveurs NPS (Network Policy Server) ont inscrit un certificat de serveur auprès de l’autorité de certification.   
  
>[!NOTE]  
>L’appartenance au groupe **Admins du domaine** est la condition minimale requise pour effectuer ces procédures.  
  
## <a name="verify-network-policy-server-nps-enrollment-of-a-server-certificate"></a>Vérifier l’inscription du serveur NPS (Network Policy Server) d’un certificat de serveur  
  
Comme NPS est utilisé pour authentifier et autoriser les demandes de connexion réseau, il est important de s’assurer que le certificat de serveur que vous avez délivré à NPSs est valide lorsqu’il est utilisé dans les stratégies réseau.  
  
Pour vérifier qu’un certificat de serveur est correctement configuré et qu’il est inscrit au serveur NPS, vous devez configurer une stratégie de réseau de test et autoriser le serveur NPS à vérifier que le serveur NPS peut utiliser le certificat pour l’authentification.  
  
### <a name="to-verify-nps-enrollment-of-a-server-certificate"></a>Pour vérifier l’inscription NPS d’un certificat de serveur  
  
1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur **serveur NPS (Network Policy Server**). La console MMC (Microsoft Management Console) du serveur NPS (Network Policy Server) s’ouvre.  
  
2.  Double-cliquez sur **stratégies**, cliquez avec le bouton droit sur **stratégies réseau**, puis cliquez sur **nouveau**. L’Assistant Nouvelle stratégie réseau s’ouvre.  
  
3.  Dans **spécifier le nom de la stratégie réseau et le type de connexion**, dans nom de la **stratégie**, tapez **stratégie de test**. Vérifiez que la valeur **du type de serveur d’accès réseau** n’est pas **spécifiée**, puis cliquez sur **suivant**.  
  
4.  Dans **spécifier les conditions**, cliquez sur **Ajouter**. Dans **Sélectionner une condition**, cliquez sur **groupes Windows**, puis sur **Ajouter**.  
  
5.  Dans **groupes**, cliquez sur **Ajouter des groupes**. Dans **Sélectionner un groupe**, tapez **utilisateurs du domaine**, puis appuyez sur entrée. Cliquez sur **OK**, puis cliquez sur **Suivant**.  
  
6.  Dans **spécifier l’autorisation d’accès**, vérifiez que **l’option accès accordé** est sélectionnée, puis cliquez sur **suivant**.  
  
7.  Dans **configurer les méthodes d’authentification**, cliquez sur **Ajouter**. Dans **Ajouter un EAP**, cliquez sur **Microsoft : PEAP (Protected EAP)** , puis cliquez sur **OK**. Dans **types EAP**, sélectionnez **Microsoft : PEAP (Protected EAP)** , puis cliquez sur **modifier**. La boîte de dialogue **modifier les propriétés EAP protégées** s’ouvre.  
  
8.  Dans la boîte de dialogue **modifier les propriétés EAP protégé** , dans **certificat délivré à**, NPS affiche le nom de votre certificat de serveur au format *ComputerName*. *Domaine*. Par exemple, si votre serveur NPS est nommé NPS-01 et que votre domaine est example.com, NPS affiche le certificat **NPS-01.example.com**. En outre, dans l' **émetteur**, le nom de votre autorité de certification s’affiche et, à la **Date d’expiration**, la date d’expiration du certificat de serveur est affichée. Cela démontre que votre NPS a inscrit un certificat de serveur valide qu’il peut utiliser pour prouver son identité aux ordinateurs clients qui essaient d’accéder au réseau via vos serveurs d’accès réseau, tels que les serveurs de réseau privé virtuel (VPN), 802.1 X les points d’accès sans fil, les serveurs de passerelle Bureau à distance et les commutateurs Ethernet 802.1 X.  
  
    > [!IMPORTANT]  
    > Si le serveur NPS n’affiche pas de certificat de serveur valide et s’il indique que le certificat est introuvable sur l’ordinateur local, il existe deux raisons possibles à ce problème. Il est possible que stratégie de groupe ne s’actualise pas correctement et que le serveur NPS n’ait pas inscrit de certificat auprès de l’autorité de certification. Dans ce cas, redémarrez le serveur NPS. Lorsque l’ordinateur redémarre, stratégie de groupe est actualisée et vous pouvez relancer cette procédure pour vérifier que le certificat de serveur est inscrit. Si l’actualisation stratégie de groupe ne résout pas ce problème, soit le modèle de certificat, l’inscription automatique de certificat, soit les deux ne sont pas configurés correctement. Pour résoudre ces problèmes, commencez au début de ce guide et effectuez à nouveau toutes les étapes pour vous assurer que les paramètres que vous avez fournis sont exacts.  
  
9. Une fois que vous avez vérifié la présence d’un certificat de serveur valide, vous pouvez cliquer sur **OK** et sur **Annuler** pour quitter l’Assistant Nouvelle stratégie réseau.  
  
    > [!NOTE]  
    > Étant donné que vous ne terminez pas l’Assistant, la stratégie de test réseau n’est pas créée dans NPS.  
  


