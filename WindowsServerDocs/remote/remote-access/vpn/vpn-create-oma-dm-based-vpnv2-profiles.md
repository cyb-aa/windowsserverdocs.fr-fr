---
title: Créer des profils VPNv2 basés sur OMA-DM sur les appareils Windows 10
description: 'Vous pouvez utiliser l’une des deux méthodes suivantes pour créer des profils VPNv2 basés sur OMA-DM. '
services: active-directory
ms.prod: windows-server
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 016d9d2dcc26572f8d248ef2f4a922da2e456b83
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949897"
---
# <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devices"></a>Étape 7.5. Créer des profils VPNv2 basés sur OMA-DM sur des appareils Windows 10

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Précédent :** Étape 7,4. Déployer des certificats racine d’accès conditionnel vers AD local](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)
- [**Ensuite :** En savoir plus sur le fonctionnement de l’accès conditionnel pour VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Au cours de cette étape, vous pouvez créer des profils VPNv2 basés sur OMA-DM à l’aide d’Intune pour déployer une stratégie de configuration d’appareil VPN. Si vous souhaitez utiliser SCCM ou un script PowerShell pour créer des profils VPNv2, consultez [paramètres CSP VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) pour plus d’informations. 

## <a name="managed-deployment-using-intune"></a>Déploiement géré à l’aide d’Intune

Tout ce qui est abordé dans cette section est le minimum nécessaire pour que le VPN fonctionne avec l’accès conditionnel. Il ne couvre pas le tunneling fractionné, à l’aide de WIP, en créant des profils de configuration d’appareil Intune personnalisés pour faire fonctionner le AutoVPN ou l’authentification unique. Intégrez les paramètres ci-dessous dans le profil VPN que vous avez créé précédemment à l' [étape 5. Configurez le client Windows 10 Always On les connexions VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md).  Dans cet exemple, nous les intégrons à la [configuration du client VPN à l’aide](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune) de la stratégie Intune. 

**Prérequis :**

L’ordinateur client Windows 10 a déjà été configuré avec une connexion VPN à l’aide d’Intune.   


**Procédures**

1. Dans la Portail Azure, sélectionnez **intune** > **configuration** de l’appareil > **profils** et sélectionnez le profil VPN que vous avez créé précédemment dans [configurer le client VPN à l’aide d’Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune).
    
2. Dans l’éditeur de stratégie, sélectionnez **propriétés** > **paramètres** > **VPN de base**. Étendez le **code XML EAP** existant pour inclure un filtre qui donne au client VPN la logique dont il a besoin pour récupérer le certificat d’accès conditionnel AAD à partir du magasin de certificats de l’utilisateur au lieu de le laisser à l’occasion de l’autoriser à utiliser le premier certificat détecté.

    >[!NOTE]
    >Sans cela, le client VPN peut récupérer le certificat utilisateur émis par l’autorité de certification locale, ce qui entraîne l’échec d’une connexion VPN.

    ![Portail Intune](../../media/Always-On-Vpn/intune-eap-xml.png)

3. Recherchez la section qui se termine par **\</AcceptServerName >\</EapType >** et insérez la chaîne suivante entre ces deux valeurs pour fournir au client VPN la logique de sélectionner le certificat d’accès conditionnel AAD :

    ```XML
    <TLSExtensions xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><EKUName>AAD Conditional Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></ClientAuthEKUList></FilteringInfo></TLSExtensions>
    ```

4. Sélectionnez le panneau **accès conditionnel** et l' **accès conditionnel MTD pour que cette connexion VPN** soit **activée**.
   
   L’activation de ce paramètre modifie la **\<DeviceCompliance >\<activée > valeur true\</Enabled >** dans le XML de profil VPNv2.

    ![Accès conditionnel pour Always On VPN-Properties](../../media/Always-On-Vpn/vpn-conditional-access-azure-ad.png)

5. Sélectionnez **OK**.

6. Sélectionnez **affectations**, sous inclure, sélectionnez les **groupes à inclure**.

7. Sélectionnez le groupe **utilisateurs VPN** qui reçoit cette stratégie, puis sélectionnez **Enregistrer**.

    ![CAP pour les utilisateurs de VPN automatique-affectations](../../media/Always-On-Vpn/cap-for-auto-vpn-users-assignments.png)

## <a name="force-mdm-policy-sync-on-the-client"></a>Forcer la synchronisation de la stratégie MDM sur le client

Si le profil VPN n’apparaît pas sur le périphérique client, sous paramètres\\réseau & Internet\\VPN, vous pouvez forcer la synchronisation de la stratégie MDM.

1. Connectez-vous à un ordinateur client joint à un domaine en tant que membre du groupe **utilisateurs VPN** .

2. Dans le menu Démarrer, entrez **Account**, puis appuyez sur entrée.

3. Dans le volet de navigation de gauche, sélectionnez **accès professionnel ou scolaire**.

4. Sous accès professionnel ou scolaire, sélectionnez **connecté à < \domain > MDM**, puis sélectionnez **informations**.

5. Sélectionnez **synchronisation** et vérifiez que le profil VPN apparaît sous paramètres\\réseau & Internet\\VPN.


## <a name="next-steps"></a>Étapes suivantes

Vous avez terminé la configuration du profil VPN pour utiliser Azure AD accès conditionnel. 

|Si vous souhaitez...  |Alors consultez...  |
|---------|---------|
|En savoir plus sur le fonctionnement de l’accès conditionnel avec les VPN  |[VPN et accès conditionnel](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): cette page fournit des informations supplémentaires sur le fonctionnement de l’accès conditionnel avec les VPN.      |
|En savoir plus sur les fonctionnalités VPN avancées  |[Fonctionnalités VPN avancées](always-on-vpn/deploy/always-on-vpn-adv-options.md#advanced-vpn-features): cette page fournit des conseils sur l’activation des filtres de trafic VPN, sur la configuration des connexions VPN automatiques à l’aide de déclencheurs d’application et sur la configuration de NPS pour autoriser uniquement les connexions VPN à partir de clients utilisant des certificats émis par Azure ad.        |


## <a name="related-topics"></a>Rubriques connexes

- [CSP VPNv2](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp): cette rubrique fournit une vue d’ensemble du fournisseur de services de chiffrement VPNv2. Le fournisseur de services de configuration VPNv2 permet au serveur de gestion des appareils mobiles (MDM) de configurer le profil VPN de l’appareil.

- [Configurer le client Windows 10 Always on les connexions VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): cette rubrique fournit des informations sur les options et le schéma ProfileXML, et explique comment créer le VPN ProfileXML. Après la configuration de l’infrastructure de serveur, vous devez configurer les ordinateurs clients Windows 10 pour qu’ils communiquent avec cette infrastructure avec une connexion VPN. 

- [Configurer le client VPN à l’aide d’Intune](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections#configure-the-vpn-client-by-using-intune): cette rubrique fournit des informations sur la façon de déployer l’accès à distance Windows 10 Always on les profils VPN. Intune utilise désormais des groupes de Azure AD. Si Azure AD Connect synchronisé le groupe d’utilisateurs VPN de l’emplacement local vers Azure AD, il n’est pas nécessaire de configurer le client VPN à l’aide d’Intune.
