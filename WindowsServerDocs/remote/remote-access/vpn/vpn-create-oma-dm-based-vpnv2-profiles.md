---
title: Créer des profils VPNv2 basés sur OMA-DM sur les appareils Windows 10
description: 'Vous pouvez utiliser une des deux méthodes pour créer l’OMA-DM en fonction des profils de VPNv2. '
services: active-directory
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1ce20d09c304b26e3708429cc45da06d020e5809
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816480"
---
# <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devices"></a>Étape 7.5. Créer OMA-DM en fonction des profils de VPNv2 pour les appareils Windows 10

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#171;  [**Précédent :** Étape 7.4. Déployer des certificats racine de l’accès conditionnel en local AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)<br>
&#187;[ **Suivant :** Découvrez comment l’accès conditionnel pour VPN works](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Dans cette étape, vous pouvez créer OMA-DM profils VPNv2 à l’aide d’Intune pour déployer une stratégie de Configuration de l’appareil VPN. Si vous souhaitez utiliser SCCM ou un script PowerShell pour créer des profils de VPNv2, consultez [paramètres VPNv2 CSP](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) pour plus d’informations. 

## <a name="managed-deployment-using-intune"></a>Déploiement géré à l’aide d’Intune

Tous les sujets traités dans cette section sont le minimum nécessaire pour que les VPN à fonctionne avec l’accès conditionnel. Il ne couvre pas le Tunneling fractionné, à l’aide de travaux en cours, création personnalisées profils de configuration d’appareil Intune pour obtenir AutoVPN utilisation ou l’authentification unique. Intégrer les paramètres ci-dessous dans le profil VPN que vous avez créé précédemment sous [étape 5. Configurer les clients Windows 10 toujours sur les connexions VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md).  Dans cet exemple, nous allons les intégrer dans le [configurer le client VPN à l’aide d’Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune) stratégie. 

**Condition préalable :**<p>
Ordinateur client de Windows 10 a déjà été configuré avec une connexion VPN à l’aide d’Intune.   


**Procédure :**

1. Dans le portail Azure, cliquez sur **Intune** > **Configuration de l’appareil** > **profils** et sélectionnez le profil VPN que vous avez créé précédemment dans [ Configurer le client VPN à l’aide d’Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune).
    
2. Dans l’éditeur de stratégie, sélectionnez **propriétés** > **paramètres** > **VPN de Base**. Étendre existant **Xml EAP** pour inclure un filtre qui donne le client VPN à la logique, il doit récupérer le certificat d’accès conditionnel AAD à partir du magasin de certificats de l’utilisateur au lieu de laisser au hasard ce qui lui permet d’utiliser la première certificat découverts.

    >[!NOTE]
    >Sans cela, le client VPN peut récupérer le certificat utilisateur émis par l’autorité de certificat en local, ce qui entraîne un échec de la connexion VPN.

    ![Portail Intune](../../media/Always-On-Vpn/intune-eap-xml.png)

3. Recherchez la section qui se termine par  **\</AcceptServerName >\</EapType >** et insérer la chaîne suivante entre ces deux valeurs pour fournir le client VPN avec la logique pour sélectionner le conditionnel AAD Certificat d’accès :

    ```XML
    <TLSExtensions xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><EKUName>AAD Conditional Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></ClientAuthEKUList></FilteringInfo></TLSExtensions>
    ```

4. Sélectionnez le **accès conditionnel** panneau et toogle **l’accès conditionnel pour cette connexion VPN** à **activé**.<p>L’activation de cette définition modifie le  **\<DeviceCompliance >\<activé > true\</activé >** définissant dans le XML du profil VPNv2.

    ![Accès conditionnel pour VPN - propriétés Always On](../../media/Always-On-Vpn/vpn-conditional-access-azure-ad.png)

6. Cliquez sur **OK**.

6. Sélectionnez **affectations**, sous inclure, cliquez sur **sélectionner les groupes à inclure**.

7. Sélectionnez le **utilisateurs VPN** groupe qui reçoit cette stratégie et le clic **enregistrer**.

    ![LIMITE pour les utilisateurs VPN automatique - attributions](../../media/Always-On-Vpn/cap-for-auto-vpn-users-assignments.png)

## <a name="force-mdm-policy-sync-on-the-client"></a>Forcer la synchronisation de stratégie de gestion des appareils mobiles sur le Client
Si le profil VPN n’apparaît pas sur l’appareil client, sous paramètres\\réseau & Internet\\VPN, vous pouvez forcer la stratégie de gestion des appareils mobiles pour la synchronisation.

1. Connectez-vous à un ordinateur client joint au domaine en tant que membre de la **utilisateurs VPN** groupe.

2. Dans le menu Démarrer, tapez **compte**, puis appuyez sur ENTRÉE.

3.  Dans le volet de navigation gauche, cliquez sur **accès scolaire ou Professionnel**.

5.  Sous accès Professionnel ou scolaire, cliquez sur **connecté à < \domain > Gestion des appareils mobiles** et cliquez sur **Info**.

6.  Cliquez sur **synchronisation** et vérifiez le profil VPN s’affiche sous les paramètres\\réseau & Internet\\VPN.


## <a name="next-step"></a>Étape suivante
Vous avez terminé configuration du profil VPN pour utiliser l’accès conditionnel Azure AD. 

|Si vous souhaitez...  |Alors consultez...  |
|---------|---------|
|En savoir plus sur l’accès conditionnel avec des réseaux privés virtuels  |[VPN et l’accès conditionnel](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Cette page fournit plus d’informations sur l’accès conditionnel fonctionne avec les réseaux privés virtuels.      |
|En savoir plus sur les fonctionnalités VPN avancées  |[Fonctionnalités VPN avancées](always-on-vpn/deploy/always-on-vpn-adv-options.md#advanced-vpn-features): Cette page fournit des conseils sur l’activation de filtres de trafic VPN, comment configurer des connexions VPN automatique à l’aide de déclencheurs d’application et comment configurer NPS pour autoriser uniquement les connexions VPN à partir de clients à l’aide de certificats émis par Azure AD.        |


---

## <a name="related-topics"></a>Rubriques connexes
- [VPNv2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp):  Cette rubrique vous fournit une vue d’ensemble de VPNv2 CSP. Le fournisseur de service de configuration VPNv2 permet au serveur de gestion (MDM) d’appareil mobile configurer le profil VPN de l’appareil.

- [Configurer les clients Windows 10 toujours sur les connexions VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): Cette rubrique fournit des informations sur les options de ProfileXML et le schéma et comment créer des VPN ProfileXML. Après avoir configuré l’infrastructure de serveur, vous devez configurer les ordinateurs clients Windows 10 pour communiquer avec cette infrastructure avec une connexion VPN. 

- [Configurer le client VPN à l’aide d’Intune](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections#configure-the-vpn-client-by-using-intune): Cette rubrique fournit des informations sur la façon de déployer Windows 10 distant accès profils VPN Always On. Intune utilise désormais les groupes Azure AD. Si Azure AD Connect synchronisé avec le groupe d’utilisateurs VPN en local à Azure AD, puis il est inutile de configuration du client VPN à l’aide d’Intune.

---
