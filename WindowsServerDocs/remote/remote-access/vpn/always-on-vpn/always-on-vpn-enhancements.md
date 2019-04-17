---
title: Améliorations de VPN Toujours actif (AlwaysOn)
description: Toujours actif (AlwaysOn) présente de nombreux avantages par rapport aux solutions VPN Windows des versions précédentes. Améliorations de la clé dans l’intégration, sécurité, connectivité, contrôle de mise en réseau et la compatibilité alignent toujours actif (AlwaysOn) vision axé sur le cloud, mobile priorité de Microsoft.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d7844d93ac898316580123c82e08bb6f02d51a2b
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067139"
---
# Améliorations de VPN Toujours actif (AlwaysOn)

>S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, Windows10

& #171; [ **Précédent:** obtenir des informations sur les fonctionnalités toujours actif (AlwaysOn)](../vpn-map-da.md)<br>
& #187; [ **Suivant:** obtenir des informations sur la technologie toujours actif (AlwaysOn)](always-on-vpn-technology-overview.md)

Toujours actif (AlwaysOn) présente de nombreux avantages par rapport aux solutions VPN Windows des versions précédentes. Les améliorations clées suivantes s’aligner sur toujours actif (AlwaysOn) vision axé sur le cloud, mobile priorité de Microsoft:

- **Intégration à la plateforme:** Toujours actif (AlwaysOn) a une meilleure intégration avec le système d’exploitation Windows et des solutions tierces pour fournir une plateforme robuste pour créait de nombreux scénarios de connexion avancées.

- **Sécurité:** Toujours actif (AlwaysOn) présente des fonctionnalités de nouveau la sécurité avancée pour restreindre le type de trafic, les applications peuvent utiliser la connexion VPN, et les méthodes d’authentification que vous pouvez utiliser pour lancer la connexion. Lorsque la connexion est active, la plupart du temps, il est particulièrement important sécuriser la connexion. Pour plus d’informations, voir les [options d’authentification VPN](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-authentication).

- **Connectivité VPN:** Toujours actif (AlwaysOn), avec ou sans Tunnel de périphérique offre la possibilité de déclenchement automatique. Avant de toujours actif (AlwaysOn), la possibilité de déclencher une connexion automatique par le biais de l’utilisateur ou de l’authentification d’appareil n’a pas possible.  

- **Contrôle de mise en réseau:** Toujours actif (AlwaysOn) permet aux administrateurs de spécifier les stratégies de routage à un niveau plus précis, même jusqu'à l’application individuelle, ce qui est parfait pour les applications cœur de métier (LOB) qui nécessitent un accès à distance spécial.  Toujours actif (AlwaysOn) est également entièrement compatible avec les deux Internet Protocol version 4 (IPv4) et version 6 (IPv6). Contrairement à DirectAccess, il n’existe aucune dépendance spécifique sur IPv6.

  >[!Note]
  >Avant de commencer, veillez à activer IPv6 sur le serveur VPN. Sinon, une connexion ne peut pas être établie et affiche un message d’erreur.
  
- **Compatibilité et configuration:** Toujours actif (AlwaysOn) peuvent être déployé et géré de plusieurs façons, ce qui permet à plusieurs avantages par rapport à l’autre logiciel de client VPN toujours actif (AlwaysOn).

## Intégration de plate-forme

Microsoft a introduit ou améliorer les fonctions d’intégration suivantes dans toujours actif (AlwaysOn):

| Amélioration de clé | Description |
| ---- | ---- |
| **[Protection des informations Windows (WIP)](https://docs.microsoft.com/windows/threat-protection/windows-information-protection/protect-enterprise-data-using-wip)** | Intégration avec la fonctionnalité WIP permet à l’application des stratégies réseau déterminer si le trafic est autorisé à emprunter le VPN. Si le profil utilisateur est actif et stratégies WIP sont appliqués, toujours actif (AlwaysOn) est déclenchée automatiquement pour vous connecter. En outre, lorsque vous utilisez la fonctionnalité WIP, il est inutile de spécifier les règles apptriggerlist et TrafficFilterList séparément dans le profil VPN (sauf si vous souhaitez configuration plus avancée), car les stratégies WIP et les listes d’applications automatiquement appliquées. |
| **[WindowsHello Entreprise](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-overview)** | Toujours actif (AlwaysOn) en mode natif prend en charge Windows Hello entreprise (en mode d’authentification par certificat) pour fournir une seule ouverture de session expérience transparente pour les deux se connecter à l’ordinateur et la connexion au réseau VPN. Par conséquent, aucune authentification secondaire (informations d’identification de l’utilisateur) n’est nécessaire pour la connexion VPN, ce qui permet d’utiliser une connexion toujours active avec Windows Hello entreprise pour l’authentification. |
| **[Accès conditionnel Microsoft Azure](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls)** | Le client de toujours actif (AlwaysOn) peut être intégré à la plateforme d’accès conditionnel Azure pour mettre en œuvre l’authentification multifacteur (MFA), conformité de l’appareil ou une combinaison des deux. Lorsqu’il est compatible avec les stratégies d’accès conditionnel, Azure Active Directory (AD Azure) émet un certificat d’authentification IPsec (IP Security) courte durée (par défaut, 60 minutes) qui peut ensuite être utilisé pour s’authentifier auprès de la passerelle VPN. Conformité de l’appareil utilise des stratégies de conformité de System Center Configuration Manager/Intune, qui peuvent inclure l’état de d’attestation d’intégrité de l’appareil dans le cadre de la vérification de la conformité de connexion. |
| **Azure MFA** | Lorsqu’il est combiné avec les services de RADIUS Remote Authentication Dial-In User Service () et l’extension de serveur NPS (Network Policy Server) pour Azure MFA, l’authentification VPN peut utiliser MFA fort. |
| **Plug-in VPN de tiers** | Avec la plateforme de Windows universelle (UWP), les fournisseurs de VPN tiers peuvent créer une application unique pour la plage complète de Windows 10 périphériques. La plateforme UWP fournit une couche API système garantie sur tous les appareils, en éliminant la complexité et les problèmes souvent associés à l’écriture de pilotes au niveau du noyau. Actuellement, les plug-in VPN UWP Windows 10 existe pour [Pulse Secure](https://www.microsoft.com/p/pulse-secure/9nblggh3b0bp), [F5 Access](https://www.microsoft.com/p/f5-access/9wzdncrdsfn0), [Check Point Capsule VPN](https://www.microsoft.com/p/check-point-capsule-vpn/9wzdncrdjxtj), [FortiClient](https://www.microsoft.com/p/forticlient/9wzdncrdh6mc), [SonicWall Mobile Connect](https://www.microsoft.com/p/sonicwall-mobile-connect/9wzdncrdsfkz)et [GlobalProtect](https://www.microsoft.com/p/globalprotect/9nblggh6bzl3); sans aucun doute, d’autres seront affiche à l’avenir. |
---

## Sécurité

Les principales améliorations en sécurité sont dans les zones suivantes:

| Amélioration de clé | Description |
| ---- | ---- |
| **Filtres de trafic** | Par le biais de filtres de trafic, vous pouvez spécifier les stratégies de côté client qui déterminent le trafic qui est autorisé dans le réseau d’entreprise. De cette façon, les administrateurs peuvent appliquer les restrictions d’application ou le trafic sur l’interface VPN, limitant ainsi son utilisation pour des sources spécifiques, ports de destination et les adresses IP. Deux types de règles de filtrage sont disponibles:<ul><li>**Règles basées sur l’application.** Règles de pare-feu basé sur l’application sont basées sur une liste des applications spécifiées afin que seul le trafic provenant de ces applications sont autorisées à emprunter l’interface VPN.</li><li>**Règles basées sur le trafic.** Règles de pare-feu basées sur le trafic sont basés sur les exigences de réseau comme ports, adresses et protocoles. Utilisez ces règles uniquement pour le trafic correspondant à ces conditions spécifiques sont autorisées à emprunter l’interface VPN.<p><p>_**Remarque:**_<br>Ces règles s’appliquent uniquement au trafic sortant à partir de l’appareil. Utilisation du trafic filtre bloque le trafic entrant à partir du réseau d’entreprise vers le client. </li></ul> |
| **VPN par application** | VPN par application est comme si vous aviez un filtre de trafic basé sur l’application, mais il accède plus à combiner les déclencheurs d’application avec un filtre de trafic basé sur l’application afin qu’une connectivité VPN est limitée à une application spécifique, par opposition à toutes les applications sur le client VPN. La fonctionnalité lance automatiquement au démarrage de l’application. |
| **Prise en charge des algorithmes de chiffrement IPsec personnalisées** | Toujours actif (AlwaysOn) prend en charge l’utilisation du chiffrement RSA et courbe elliptique basés sur le chiffrement personnalisé des algorithmes de chiffrement pour répondre aux stricte gouvernement ou des stratégies de sécurité de l’organisation. |
| **Prise en charge du protocole EAP (Extensible Authentication) native** | Toujours actif (AlwaysOn) prend en charge en mode natif les EAP, ce qui vous permet d’utiliser un ensemble varié de Microsoft et les types de protocoles EAP tiers dans le cadre du flux de travail d’authentification. EAP fournit une authentification sécurisée basée sur les types d’authentification suivants:<ul><li>Nom d’utilisateur et mot de passe</li><li>Carte à puce (physique et virtuel)</li><li>Certificats d’utilisateur</li><li>WindowsHello Entreprise</li><li>Prise en charge MFA par le biais intégration EAP RADIUS</li></ul>Le fournisseur de l’application contrôle méthodes d’authentification du plug-in tiers VPN UWP bien qu’ils aient un tableau des options disponibles, y compris les types d’informations d’identification personnalisées et prise en charge OTP. |
---

## Connectivité VPN

Les principales améliorations de la connectivité toujours actif (AlwaysOn) sont les suivantes:

| Amélioration de clé | Description |
| ---- | ---- |
| **Toujours actif (AlwaysOn)** | AlwaysOn est une fonctionnalité de Windows 10 qui permet au profil VPN actif pour vous connecter automatiquement et rester connecté en fonction de déclencheurs, à savoir, connexion de l’utilisateur, changement d’état réseau ou écran de l’appareil actif. AlwaysOn est également intégré à l’expérience de veille connectée pour optimiser l’autonomie de la batterie. |
| **Déclenchement de l’application** | Vous pouvez configurer les profils VPN dans Windows 10 pour se connecter automatiquement au lancement d’un ensemble d’applications spécifié. Vous pouvez configurer les applications de bureau et UWP pour déclencher une connexion VPN. |
| **Basé sur le nom de déclenchement** | Toujours actif, vous pouvez définir des règles afin que les requêtes de nom de domaine spécifique déclenchent la connexion VPN. Windows 10 prend désormais en charge basée sur le nom de déclenchement pour les ordinateurs joints au domaine et joints à un domaine (auparavant, seuls les ordinateurs joints à un domaine étaient compatibles). |
| **Détection de réseaux approuvés** | Toujours actif (AlwaysOn) inclut cette fonctionnalité pour vous assurer qu’une connectivité VPN ne soit pas déclenchée si un utilisateur est connecté à un réseau approuvé dans la limite d’entreprise. Vous pouvez combiner cette fonctionnalité avec une des méthodes déclenchement mentionnées précédemment pour fournir une expérience utilisateur de «se connecter seulement quand nécessaire» fluide. |
| **[Tunnel de périphérique](../vpn-device-tunnel-config.md)** | Toujours actif (AlwaysOn) vous donne la possibilité de créer un profil VPN dédié pour appareil ou un ordinateur. Contrairement aux _Tunnel utilisateur_, ce qui se connecte uniquement après un utilisateur se connecte à l’appareil ou l’ordinateur, _Tunnel de périphérique_ permettant la connexion VPN établir la connectivité avant connexion de l’utilisateur. À la fois Tunnel de périphérique et utilisateur Tunnel fonctionnent indépendamment avec leurs profils VPN, peuvent être connectés en même temps et peuvent utiliser différentes méthodes d’authentification et autres paramètres de configuration VPN selon le cas. |
---

## Mise en réseau

Voici quelques-uns des améliorations mise en réseau de toujours actif (AlwaysOn):

| Amélioration de clé | Description |
| ---- | ---- |
| **Prise en charge de pile double pour IPv4 et IPv6** | Toujours actif (AlwaysOn) en mode natif prend en charge que l’utilisation de IPv4 et IPv6 dans une approche de pile double. Il n’est aucune dépendance spécifique sur un protocole sur l’autre, ce qui permet la compatibilité des applications IPv4/IPv6 maximale combinée avec prise en charge d’IPv6 futures aux besoins de communications.<p>**_Remarque:_** Avant de commencer, veillez à activer IPv6 sur le serveur VPN. Sinon, une connexion ne peut pas être établie et affiche un message d’erreur.|
| **Stratégies de routage spécifiques à l’application** | Outre la définition globales stratégies de routage de connexion VPN pour la séparation du trafic internet et intranet, il est possible d’ajouter des stratégies de routage pour contrôler l’utilisation de tunneling fractionné ou forcer les configurations de tunnel sur une base par application. Cette option vous donne un contrôle plus précis sur lesquels les applications sont autorisées pour interagir avec les ressources via le tunnel VPN. |
| **Itinéraires d’exclusion** | Toujours actif (AlwaysOn) prend en charge la possibilité de spécifier des itinéraires d’exclusion qui contrôlent spécifiquement le comportement de routage pour définir le trafic qui doit parcourir le réseau privé virtuel uniquement et ne pas emprunter l’interface réseau physique.<p><p>_**Notes.**_<br>-Des itinéraires d’exclusion fonctionnent actuellement pour le trafic au sein du même sous-réseau que le client, par exemple, LinkLocal.<br>-Des itinéraires d’exclusion fonctionnent uniquement dans une configuration de tunneling fractionné. |
---

## Configuration et compatibilité 

Voici quelques-uns des améliorations configuration et la compatibilité de toujours actif (AlwaysOn):

| Amélioration de clé | Description |
| ---- | ---- |
| **Compatibilité de passerelle VPN tiers** | Le client toujours actif (AlwaysOn) ne nécessite pas l’utilisation d’une passerelle VPN basée sur Microsoft pour fonctionner. Par le biais de la prise en charge du protocole IKEv2, le client facilite l’interopérabilité avec des passerelles VPN tiers qui prennent en charge ce type de tunnel standard au secteur d’activité. Vous pouvez également obtenir l’interopérabilité avec des passerelles VPN tiers à l’aide d’un plug-in de VPN UWP combinées avec un type de tunnel personnalisé sans pour autant sacrifier les avantages et les fonctionnalités de la plateforme toujours actif (AlwaysOn).<p><p>_**Remarque:**_<br>Consultez votre fournisseur de matériel principal passerelle ou tierces sur les configurations et compatibilité avec toujours actif (AlwaysOn) et Tunnel de périphérique à l’aide de IKEv2. |
| **Prise en charge du protocole standard de l’industrie VPN IKEv2** | Le client toujours actif (AlwaysOn) prend en charge IKEv2, un des aujourd'hui plus largement utilisé protocoles de tunneling standard. Cette compatibilité optimise l’interopérabilité avec des passerelles VPN tiers. |
| **Prise en charge de la plateforme** | Prend en charge AAlways sur VPN joints au domaine, joints à un domaine (groupe de travail) ou appareils Azure AD – joint pour les deux entreprises et apportez votre propre appareil scénarios (BYOD). En outre, toujours actif (AlwaysOn) est disponible dans toutes les éditions de Windows. |
| **Divers mécanismes de gestion et de déploiement** | Vous pouvez utiliser des nombreux mécanismes de déploiement et de gestion pour gérer les paramètres de VPN (appelés un _profil VPN_), y compris Windows PowerShell, System Center Configuration Manager, Intune (ou outil de gestion [GPM] tiers des périphériques mobiles) et Windows Concepteur de configuration. Ces options simplifient la configuration de toujours actif (AlwaysOn), les outils de gestion client que vous utilisez. |
| **Définition de profil VPN standardisée** | Toujours actif (AlwaysOn) prend en charge la configuration à l’aide d’un profil XML standard (ProfileXML), en fournissant un modèle de format configuration standard qui utilisent la plupart des ensembles d’outils de déploiement et de gestion des. |
---


## Étapes suivantes

- [En savoir plus sur certaines des fonctionnalités avancées toujours actif (AlwaysOn)](deploy/always-on-vpn-adv-options.md)

- [En savoir plus sur la technologie VPN Toujours actif (AlwaysOn)](always-on-vpn-technology-overview.md)

- [Commencer à planifier votre déploiement de VPN Toujours actif (AlwaysOn)](deploy/always-on-vpn-deploy-deployment.md)



---
