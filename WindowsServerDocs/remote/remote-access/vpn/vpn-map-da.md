---
title: Fonctionnalités du VPN toujours actif (AlwaysOn)
description: Dans cette rubrique, vous en savoir plus sur les fonctionnalités de VPN Always On.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.assetid: 8fe1c810-4599-4493-b4b8-73fa9aa18535
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 68d8561eb55b844a80c8b6a38d1255ad44457af6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874810"
---
# <a name="always-on-vpn-features-and-functionalities"></a>Fonctionnalités et les fonctionnalités de VPN always On

>S'applique à : Windows Server \(canal semi-annuel\), Windows Server 2016, Windows 10

&#171;  [**Précédent :** Toujours sur le déploiement de VPN pour Windows Server et Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md)<br>
&#187;[ **Suivant :** En savoir plus sur les améliorations de VPN Always On](../vpn/always-on-vpn/always-on-vpn-enhancements.md)

Dans cette rubrique, vous en savoir plus sur les fonctionnalités de VPN Always On.  Le tableau suivant n’est pas une liste exhaustive, toutefois, il inclut certaines des principales fonctions et fonctionnalités utilisées dans les solutions d’accès à distance. 

>[!TIP]
>Si vous utilisez actuellement DirectAccess, nous vous recommandons de vérifier la fonctionnalité VPN Always On avec soin pour déterminer s’il traite que tous vos besoins d’accès à distance avant de migrer forment DirectAccess pour VPN Always On.  

| Zone fonctionnelle | VPN Toujours actif (AlwaysOn)  |
| ---- | ---- |
| Connectivité transparente au réseau d’entreprise. | Vous pouvez configurer le VPN Always On pour prendre en charge le déclenchement automatique en fonction des demandes de résolution de lancement ou espace de noms application.<p><p>Définir à l’aide de :<br>**VPNv2/ProfileName/AlwaysOn**<br>**VPNv2/ProfileName/AppTriggerList**<br>**VPNv2/ProfileName/DomainNameInformationList/AutoTrigger** |
| Utilisation d’un Tunnel d’Infrastructure dédiée pour fournir une connectivité pour les utilisateurs non connectés au réseau d’entreprise. | Vous pouvez obtenir cette fonctionnalité à l’aide de la fonction Tunnel de périphérique dans le profil VPN.<p><p>_**Remarque.**_<br>Tunnel de l’appareil peut uniquement être configurée sur les appareils joints au domaine à l’aide d’IKEv2 ordinateur avec authentification par certificat.<p><p>Définir à l’aide de :<br>**VPNv2/ProfileName/DeviceTunnel** |
| Utilisation de la gestion sortante pour autoriser la connectivité à distance aux clients des systèmes de gestion situés sur le réseau d’entreprise. | Vous pouvez obtenir cette fonctionnalité en utilisant la fonction Tunnel de périphérique dans le profil VPN associée à la configuration de la connexion VPN pour enregistrer de manière dynamique les adresses IP attribuées à l’interface VPN avec des services DNS internes.<p><p>_**Remarque.**_<br>Si vous activez des filtres de trafic dans le profil de Tunnel de l’appareil, le Tunnel appareil refuse le trafic entrant (du réseau d’entreprise au client).<p><p>Définir à l’aide de :<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/RegisterDNS** |
| Se situent lorsque les clients se trouvent derrière un pare-feu ou serveurs proxy. | Vous pouvez configurer le retour à SSTP (à partir de IKEv2) en utilisant le type/protocole de tunnel automatique dans le profil VPN.<p><p>_**Remarque.**_<br>Tunnel de l’utilisateur prend en charge SSTP et IKEv2 et Tunnel de l’appareil prend en charge IKEv2 uniquement avec aucune prise en charge de secours SSTP.<p>Définir à l’aide de :<br>**VPNv2/ProfileName/NativeProfile/NativeProtocolType** |
| Prise en charge pour le mode d’accès de bout à bord. | VPN Toujours actif (AlwaysOn) fournit une connectivité à des ressources d’entreprise à l’aide de stratégies de tunnel qui nécessitent une authentification et un chiffrement jusqu'à ce qu’elles atteignent la passerelle VPN. Par défaut, les sessions de tunnel s'arrêtent à la passerelle VPN, qui sert également de passerelle IKEv2, afin de fournir une sécurité de bout-à-bord. |
| Prise en charge de l’authentification du certificat d'ordinateur. | Le type de protocole IKEv2 est disponible comme partie de la plateforme VPN Always On prend en charge l’utilisation de certificats d’ordinateur ou d’ordinateur pour l’authentification VPN.<p><p>_**Remarque.**_<br>IKEv2 est le seul protocole pris en charge pour le Tunnel de l’appareil, et aucune option n’est prise en charge de secours SSTP. <p>Définir à l’aide de :<br>**VPNv2/ProfileName/NativeProfile/Authentication/MachineMethod** |
| Utiliser des groupes de sécurité pour limiter les fonctionnalités d’accès à distance à des clients spécifiques. | Vous pouvez configurer VPN Toujours actif (AlwaysOn) pour qu'il prenne en charge l’autorisation granulaire lors de l’utilisation de RADIUS, qui inclut l’utilisation de groupes de sécurité pour contrôler l’accès VPN. |
| Prise en charge des serveurs situés derrière un pare-feu de périmètre ou un périphérique NAT. | VPN Toujours actif (AlwaysOn) vous donne la possibilité d’utiliser des protocoles comme IKEv2 et SSTP qui prennent totalement en charge l’utilisation d’une passerelle VPN qui se trouve derrière un périphérique NAT ou un pare-feu de périmètre.<p><p>_**Remarque.**_<br>Tunnel de l’utilisateur prend en charge SSTP et IKEv2 et Tunnel de l’appareil prend en charge IKEv2 uniquement avec aucune prise en charge de secours SSTP. |
| Possibilité de déterminer la connectivité intranet lorsque connecté au réseau d’entreprise. | Détection des réseaux approuvés offre la possibilité de détecter les connexions de réseau d’entreprise, et il est basé sur une évaluation du suffixe DNS spécifique à la connexion affectée aux interfaces réseau et le profil de réseau.<p><p>Définir à l’aide de :<br>**VPNv2/ProfileName/TrustedNetworkDetection** |
| Conformité à l’aide de la Protection d’accès réseau (NAP). | Le client VPN Toujours actif (AlwaysOn) peut s’intégrer avec un accès conditionnel Azure pour mettre en œuvre MFA, la conformité de l'appareil ou une combinaison des deux. Lorsqu’il est compatible avec les stratégies d’accès conditionnel, Azure AD émet un certificat d’authentification IPsec de courte durée (par défaut, 60 minutes) que le client peut ensuite utiliser pour s’authentifier auprès de la passerelle VPN. La conformité de l'appareil tire parti des stratégies de conformité de System Center Configuration Manager/Intune, qui peuvent inclure l’état d'attestation d'intégrité de l'appareil. À ce stade, l'accès conditionnel VPN Azure fournit le remplacement le plus proche de la solution NAP existante, bien qu’il n’existe aucune forme de service de correction ni de fonctionnalités de réseau de quarantaine. Pour plus d’informations, consultez [VPN et l’accès conditionnel](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-conditional-access).<p>Définir à l’aide de :<br>**VPNv2/ProfileName/DeviceCompliance** |
| Possibilité de définir les serveurs de gestion accessibles avant la connexion de l’utilisateur. | Vous pouvez obtenir cette fonctionnalité dans VPN Always On à l’aide de la fonctionnalité de Tunnel de l’appareil (disponible dans la version 1709 – pour IKEv2 uniquement) dans le profil VPN combiné avec les filtres de trafic pour contrôler les systèmes de gestion sur le réseau d’entreprise qui sont accessibles via le Tunnel de l’appareil.<p><p>_**Remarque.**_<br>Si vous activez des filtres de trafic dans le profil de Tunnel de l’appareil, le Tunnel appareil refuse le trafic entrant (du réseau d’entreprise au client).<p>Définir à l’aide de :<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/TrafficFilterList** |
---

## <a name="additional-functionalities"></a>Fonctionnalités supplémentaires

Chaque élément dans cette section est un scénario d’utilisation ou les fonctionnalités d’accès à distance couramment utilisés pour lequel VPN Always On a amélioré les fonctionnalités, soit via une extension de fonctionnalité, voire l’élimination d’une limitation précédente.


| Zone fonctionnelle | VPN Toujours actif (AlwaysOn)  |
| ---- | ---- |
| Appareils avec exigence de référence (SKU) Entreprise joints à un domaine. | VPN Toujours actif (AlwaysOn) prend en charge les appareils joints à un domaine, non joints à un domaine (groupe de travail) ou joints à Azure AD pour autoriser les scénarios entreprise et BYOD. VPN Toujours actif (AlwaysOn) est disponible dans toutes les éditions de Windows et les fonctionnalités de la plateforme sont disponibles pour des tiers par le biais de la prise en charge du plug-in VPN UWP.<p><p>_**Remarque.**_<br>Tunnel de l’appareil peut uniquement être configurée sur les appareils joints au domaine exécutant Windows 10 entreprise ou l’éducation version 1709 ou ultérieure. Il n’existe aucune prise en charge pour le contrôle tiers du Tunnel de périphérique. |
| Prise en charge pour IPv4 et IPv6. | Avec VPN Toujours actif (AlwaysOn), les utilisateurs peuvent accéder à la fois aux ressources IPv4 et IPv6 sur le réseau d’entreprise. Le client VPN Toujours actif (AlwaysOn) utilise une approche de pile double qui ne dépend pas spécifiquement d'IPv6 ni de la passerelle VPN pour fournir des services de traduction NAT64 ou DNS64. |
| Prise en charge de l'authentification à 2 facteurs ou de l'authentification par mot de passe à usage unique. |La plateforme VPN Toujours actif (AlwaysOn) prend en charge le protocole EAP en natif, ce qui permet l’utilisation de divers types de protocoles EAP Microsoft et tiers dans le cadre du flux de travail d’authentification. VPN Toujours actif (AlwaysOn) prend en charge spécifiquement les cartes à puce (physiques et virtuelles) et les certificats Windows Hello Entreprise pour répondre aux exigences d’authentification à 2 facteurs. En outre, VPN Always On prend en charge OTP via MFA (non pris en charge en mode natif, uniquement pris en charge sur les plug-ins tiers) par le biais de l’intégration EAP RADIUS.<p><p>Définir à l’aide de :<br>**VPNv2/ProfileName/NativeProfile/Authentication** |
| Prise en charge de plusieurs domaines et forêts. | La plateforme VPN Toujours actif (AlwaysOn) n'a pas de dépendance vis-à-vis d'une topologie de forêts ou de domaine Active Directory Domain Services (AD DS) (ni de niveaux de schéma/fonctionnels associés) car elle ne requiert pas que le client VPN soit joint à un domaine pour fonctionner. La stratégie de groupe n’est donc pas une dépendance pour définir les paramètres du profil VPN, car vous ne l’utilisez pas lors de la configuration du client. Lorsque l’intégration de l’autorisation Active Directory est nécessaire, vous pouvez l’obtenir via RADIUS dans le cadre du processus d’authentification et d’autorisation EAP. |
| Prise en charge des fonctions fractionner et forcer le tunnel pour la séparation du trafic internet/intranet. | Vous pouvez configurer VPN Toujours actif (AlwaysOn) pour prendre en charge à la fois forcer le tunnel (mode de fonctionnement par défaut) et fractionner le tunnel en natif. VPN Toujours actif (AlwaysOn) fournit une granularité supplémentaire pour les stratégies de routage spécifiques à l’application.<p><p>_**Remarque.**_<br>Prise en charge par Tunnel utilisateur uniquement.<p><p>Définir à l’aide de :<p> **VPNv2/ProfileName/NativeProfile/RoutingPolicyType**<br>**VPNv2/ProfileName/TrafficFilterList/App/RoutingPolicyType** |
| Prise en charge de plusieurs protocoles. | VPN Always On peut être configuré pour prendre en charge en mode natif SSTP si (Secure Sockets Layer) secours à partir de IKEv2 est requis.<p><p>_**Remarque.**_<br>Tunnel de l’utilisateur prend en charge SSTP et IKEv2 et Tunnel de l’appareil prend en charge IKEv2 uniquement avec aucune prise en charge de secours SSTP.  |
| Assistant de connectivité pour fournir l’état de la connectivité d’entreprise. | VPN Toujours actif (AlwaysOn) est entièrement intégré avec l’Assistant Network Connectivity en natif et indique l'état de connectivité à partir de l’interface d’affichage de tous les réseaux. Avec l’arrivée de Windows 10 Creators Update (version 1703), un état de la connexion VPN et un contrôle de la connexion VPN pour le Tunnel de l’utilisateur sont désormais disponibles via le menu volant de réseau (pour le Windows client VPN intégré), également. |
| Résolution de noms de ressources d’entreprise utilisant un nom court, un nom de domaine complet (FQDN) et un suffixe DNS. | VPN Toujours actif (AlwaysOn) peut définir en mode natif un ou plusieurs suffixes DNS dans le cadre de la connexion VPN et du processus d’affectation d'adresse IP, y compris la résolution de noms de ressources d’entreprise pour les noms courts, les noms de domaines complets ou des espaces de noms DNS entiers. VPN Always On prend également en charge l’utilisation des Tables de stratégie de résolution de nom pour fournir la granularité de la résolution de l’espace de noms spécifique.<p><p>_**Remarque.**_<br>Évitez d’utiliser des Suffixes Global comme elles interfèrent avec la résolution de nom court lors de l’utilisation de tables de la stratégie de résolution de nom.<p><p>Définir à l’aide de :<br>**VPNv2/ProfileName/DnsSuffix**<br>**VPNv2/ProfileName/DomainNameInformationList** |
---



## <a name="next-steps"></a>Étapes suivantes

- [En savoir plus sur les améliorations de VPN Always On](always-on-vpn/always-on-vpn-enhancements.md)

- [En savoir plus sur certaines des fonctionnalités avancées VPN Always On](always-on-vpn/deploy/always-on-vpn-adv-options.md)

- [En savoir plus sur la technologie VPN Always On](always-on-vpn/always-on-vpn-technology-overview.md)

- [Commencer à planifier votre déploiement VPN Always On](always-on-vpn/deploy/always-on-vpn-deploy-deployment.md)

---
