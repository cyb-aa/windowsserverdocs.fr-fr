---
title: Créer des profils VPNv2 basés sur OMA-DM sur les appareils Windows10
description: 'Vous pouvez utiliser une des deux méthodes pour créer des OMA-DM en fonction des profils de VPNv2. '
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
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031293"
---
# Étape7.5. Créer des OMA-DM en fonction des profils de VPNv2 pour les appareils Windows 10

>S’applique à: Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Précédente:** étape 7.4. Déployer des certificats racine de l’accès conditionnel à local AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)<br>
& #187; [ **Suivant:** Découvrez l’accès conditionnel pour VPN fonctionne](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Dans cette étape, vous pouvez créer des OMA-DM profils VPNv2 à l’aide d’Intune pour déployer une stratégie de Configuration de l’appareil VPN. Si vous souhaitez utiliser script SCCM ou PowerShell pour créer des profils de VPNv2, consultez les [paramètres CSP VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) pour plus d’informations. 

## Déploiement géré à l’aide d’Intune

Tous les éléments présentés dans cette section est au minimum nécessaire pour améliorer le fonctionnement avec un accès conditionnel de VPN. Elle ne traite pas le Tunneling fractionné, à l’aide de la fonctionnalité WIP, créez les profils de configuration de périphérique personnalisés Intune pour obtenir AutoVPN fonctionne, ou à l’authentification unique. Intégrer les paramètres ci-dessous dans le profil VPN que vous avez créé précédemment sous [étape 5. Configurer le Client Windows 10 toujours sur des connexions VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md).Dans cet exemple, nous cherchons à les intégrer dans la stratégie de [configurer le client VPN à l’aide de Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune) . 

**Condition préalable requise:**<p>
Ordinateur du client Windows 10 a déjà été configuré avec une connexion VPN à l’aide d’Intune.   


**Procédure:**

1. Dans le portail Azure, cliquez sur **Intune** > **Configuration du périphérique** > **profils** et sélectionnez le profil VPN que vous avez créé précédemment dans [configurer le client VPN à l’aide de Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune).
    
2. Dans l’éditeur de stratégie, sélectionnez **Propriétés** > **paramètres** > **VPN de Base**. Étendre l' existant **EAP Xml** pour inclure un filtre qui donne le client VPN la logique qu’il a besoin de récupérer le certificat de l’accès conditionnel AAD à partir du magasin de certificats de l’utilisateur au lieu de laisser au hasard lui permettant d’utiliser le premier certificat découverts.

    >[!NOTE]
    >Sans cela, le client VPN peut récupérer le certificat de l’utilisateur émis par l’autorité de certification sur site, ce qui entraîne un échec de la connexion VPN.

    ![Portail Intune](../../media/Always-On-Vpn/intune-eap-xml.png)

3. Recherchez la section qui se termine par **\</AcceptServerName>\</EapType>** , l’insérer la chaîne suivante entre ces deux valeurs pour fournir le client VPN avec la logique pour sélectionner le certificat d’accès conditionnel AAD:

    ```XML
    <TLSExtensions xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><EKUName>AAD Conditional Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></ClientAuthEKUList></FilteringInfo></TLSExtensions>
    ```

4. Sélectionnez le panneau **d’Accès conditionnel** et toogle **l’accès conditionnel pour cette connexion VPN** sur **activé**.<p>L’activation de ce paramètre est modifié le **\<DeviceCompliance>\<Enabled>true\</Enabled>** affichée dans le XML de profil VPNv2.

    ![Accès conditionnel pour toujours sur VPN - propriétés](../../media/Always-On-Vpn/vpn-conditional-access-azure-ad.png)

6. Cliquez sur **OK**.

6. **Affectations**, sous inclure, cliquez sur **Sélectionner des groupes à inclure**.

7. Sélectionnez le groupe **d’Utilisateurs VPN** qui reçoit cette stratégie, puis cliquez sur **Enregistrer**.

    ![LIMITÉ pour les utilisateurs VPN Auto - affectations](../../media/Always-On-Vpn/cap-for-auto-vpn-users-assignments.png)

## Forcer la synchronisation de la stratégie GPM sur le Client
Si le profil VPN ne s’affiche pas sur l’appareil du client, sous Settings\\Network & Internet\\VPN, vous pouvez forcer la stratégie GPM d’effectuer la synchronisation.

1. Connectez-vous à un ordinateur client joint au domaine en tant que membre du groupe **d’Utilisateurs VPN** .

2. Dans le menu Démarrer, tapez le **compte**et appuyez sur ENTRÉE.

3.  Dans le volet de navigation de gauche, cliquez sur **accès Professionnel ou scolaire**.

5.  Sous accès Professionnel ou scolaire, cliquez sur **connecté à <\domain> GPM** et **informations**.

6.  Cliquez sur **la synchronisation** et vérifiez que le profil VPN s’affiche sous Settings\\Network & Internet\\VPN.


## Étape suivante
Vous avez fini de configurer le profil VPN pour utiliser l’accès conditionnel Azure AD. 

|Si vous souhaitez...  |Alors consultez...  |
|---------|---------|
|En savoir plus sur le fonctionnement d’un accès conditionnel avec VPN  |[VPN et accès conditionnel](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): cette page fournit plus d’informations sur l’accès conditionnel comment fonctionne avec les réseaux privés virtuels.      |
|En savoir plus sur les fonctionnalités avancées de VPN  |[Fonctionnalités avancées de VPN](always-on-vpn/deploy/always-on-vpn-adv-options.md#advanced-vpn-features): cette page fournit des conseils sur l’activation des filtres de trafic VPN, la configuration de connexions VPN automatiques à l’aide de déclencheurs d’application et comment configurer un serveur NPS pour autoriser uniquement les connexions VPN à partir de clients utilisant des certificats émis par Azure AD.        |


---

## Rubriques associées
- [Fournisseur CSP VPNv2](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp): cette rubrique vous offre une vue d’ensemble de CSP VPNv2. Le fournisseur de services de configuration VPNv2 permet le serveur de gestion des périphériques mobiles configurer le profil VPN de l’appareil.

- [Configurer Windows 10 toujours sur les connexions VPN Client](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): cette rubrique fournit des informations sur le schéma et les options de ProfileXML et comment créer le VPN ProfileXML. Après avoir configuré l’infrastructure de serveur, vous devez configurer les ordinateurs clients de Windows 10 pour communiquer avec cette infrastructure avec une connexion VPN. 

- [Configurer le client VPN à l’aide de Intune](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections#configure-the-vpn-client-by-using-intune): cette rubrique fournit des informations sur le déploiement de Windows 10 distant accès toujours sur les profils VPN. Intune utilise désormais des groupes Azure AD. Si Azure AD Connect synchronisé avec le groupe d’utilisateurs VPN local à Azure Active Directory, puis il est inutile de configuration du client VPN à l’aide d’Intune.

---
