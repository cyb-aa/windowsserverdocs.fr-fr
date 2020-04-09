---
ms.assetid: 69ec592a-5499-4249-8ba0-afa356a8ff75
title: Informations techniques de référence sur l’inscription de l’appareil
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0b921e4a88940477ec7d0d4b2fa165880bd41150
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860222"
---
# <a name="device-registration-technical-reference"></a>Informations techniques de référence sur l’inscription de l’appareil
Device Registration service \(DRS\) est un nouveau service Windows inclus avec le rôle Active Directory service FS (Federation Service) sur Windows Server 2012 R2.  Le service DRS doit être installé et configuré sur tous les serveurs de fédération dans votre batterie AD FS.  Pour plus d’informations sur le déploiement DRS, voir [Configurer un serveur de fédération avec Device Registration Service](https://technet.microsoft.com/library/dn486831.aspx).  
  
## <a name="active-directory-objects-created-when-a-device-is-registered"></a>Objets Active Directory créés lorsqu’un appareil est inscrit  
Les objets Active Directory suivants sont créés dans le cadre du service DRS.  
  
### <a name="device-registration-configuration"></a>Configuration de l’inscription de l’appareil  
La configuration de l’inscription de l’appareil est stockée dans le contexte d’appellation de configuration de la forêt Active Directory. \(par exemple, **cn\=Device Registration configuration, cn\=services, < configuration\-nommage\-Context** >\). Cet objet est créé lorsque la forêt Active Directory est paraphée pour l’inscription de l’appareil.  
  
La configuration de l’inscription de l’appareil inclut les éléments suivants :  
  
-   **Clés de l’émetteur**  
  
    Clés publiques et privées utilisées pour émettre le certificat X.509 associé à un appareil enregistré.  Les clés privées sont protégées par DKM.  
  
-   **Configuration du service Device Registration**  
  
    Stratégies concernant Device Registration Service.  
  
### <a name="registered-devices-container"></a>Conteneur d’appareils inscrits  
Le conteneur d’objet périphérique est créé sous l’un des domaines de la forêt Active Directory.  Ce conteneur d’objet contient tous les objets périphériques pour la forêt Active Directory.  
  
Par défaut, le conteneur est créé dans le même domaine que les services AD FS.  \(par exemple, **CN\=RegisteredDevices, DC\=<\-de nommage par défaut\-contexte** >. Cet objet est créé lorsque la forêt Active Directory est initialisée pour l’inscription de l’appareil.\)  
  
### <a name="registered-devices"></a>Appareils inscrits  
Les objets périphériques sont de nouveaux objets légers dans Active Directory.  Ils sont utilisés pour représenter la relation entre un utilisateur, un périphérique et la société.  Les objets périphériques utilisent un certificat signé par AD FS pour ancrer l’appareil physique à l’objet périphérique logique dans Active Directory.  
  
Les appareils inscrits incluent les éléments suivants :  
  
-   **Nom complet**  
  
    Nom convivial de l’appareil.  Pour les appareils Windows, il s’agit du nom d’hôte de l’ordinateur.  
  
-   **ID de l’appareil**  
  
    GUID généré par le serveur d’inscription de l’appareil.  
  
-   **Empreinte numérique du certificat**  
  
    Empreinte numérique du certificat X.509 utilisé avec l’appareil inscrit.  
  
-   **Type de système d’exploitation**  
  
    Type de système d’exploitation de l’appareil.  
  
-   **Version du système d’exploitation**  
  
    Version du système d’exploitation de l’appareil.  
  
-   **Est activé**  
  
    Valeur booléenne qui indique si l’appareil est activé dans Active Directory.  Seuls les appareils activés sont autorisés à accéder aux services.  
  
-   **Heure de la dernière utilisation approximative**  
  
    Heure approximative à laquelle l’appareil a été utilisé pour accéder à une ressource.  Pour limiter le trafic de réplication, cette valeur est uniquement mise à jour tous les 14 jours.  
  
-   **Propriétaire inscrit**  
  
    Identité de sécurité \(SID\) de l’utilisateur qui a joint cet appareil à l’espace de travail.  
  
## <a name="ad-fsdrs-server-ssl-certificate-revocation-checking"></a>AD FS\/la vérification de la révocation des certificats SSL du serveur DRS  
Le client de jonction à l’espace de travail vérifie la validité du certificat SSL du serveur AD FS.  Si le certificat SSL AD FS Server comprend une liste de révocation de certificats \(point de terminaison de\) de liste de révocation de certificats, le client doit être en mesure d’atteindre le point de terminaison spécifié pour valider le certificat.  
  
Si vous utilisez un environnement de test et une autorité de certification de test \(\) de l’autorité de certification pour émettre vos certificats SSL de serveur, vous pouvez choisir de ne pas inclure le point de terminaison de la liste de révocation de certificats dans les certificats de serveur émis par votre autorité de certification.  Cela permettra au client de jonction à l’espace de travail de contourner la vérification de la liste de révocation de certificats.  
  
> [!CAUTION]  
> Cela n’est jamais recommandé pour les systèmes de production  
  

