---
ms.assetid: 69ec592a-5499-4249-8ba0-afa356a8ff75
title: "Référence technique de l’inscription de périphérique"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fac6437e9b6c3893064769a8279c2cf96cbc47d6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
>S’applique à: Windows Server2016, Windows Server2012R2

# <a name="device-registration-technical-reference"></a>Référence technique de l’inscription de périphérique
Le Service d’inscription de périphérique \(DRS\) est un nouveau service de Windows qui est inclus avec le rôle de Service de fédération ActiveDirectory sur Windows Server2012R2.  Le service DRS doit être installé et configuré sur tous les serveurs de fédération dans votre batterie ADFS.  Pour plus d’informations sur le déploiement DRS, voir [configurer un serveur de fédération avec Device Registration Service](https://technet.microsoft.com/library/dn486831.aspx).  
  
## <a name="active-directory-objects-created-when-a-device-is-registered"></a>Les objets ActiveDirectory créés lorsqu’un appareil est inscrit.  
Les objets ActiveDirectory suivants sont créés dans le cadre du Service DRS.  
  
### <a name="device-registration-configuration"></a>Configuration de l’inscription de périphérique  
La Configuration de l’inscription de périphérique est stockée dans le contexte d’appellation de Configuration de la forêt ActiveDirectory. \ (Par exemple, **CN\ = Device Registration Configuration CN\ = Services, < configuration\-naming\-context >**\). Cet objet est créé lors de la forêt ActiveDirectory est paraphée pour l’inscription de l’appareil.  
  
La Configuration de l’inscription de périphérique inclut les éléments suivants:  
  
-   **Clés de l’émetteur**  
  
    Les clés publiques et privées utilisées pour émettre le certificat X.509 associé à un appareil enregistré.  Les clés privées sont protégées par DKM.  
  
-   **Configuration Device Registration Service**  
  
    Stratégies concernant Device Registration Service.  
  
### <a name="registered-devices-container"></a>Conteneur d’appareils inscrits  
Le conteneur d’objet périphérique est créé sous un des domaines dans la forêt ActiveDirectory.  Ce conteneur d’objet contient tous les objets de périphérique pour la forêt ActiveDirectory.  
  
Par défaut, le conteneur est créé dans le même domaine que les services ADFS.  \ (Par exemple, **CN\ = RegisteredDevices, DC\ = < default\-naming\-context >**\). Cet objet est créé lors de la forêt ActiveDirectory est paraphée pour l’inscription de l’appareil.  
  
### <a name="registered-devices"></a>Appareils inscrits  
Les objets périphériques sont des objets de nouveau légers dans ActiveDirectory.  Ils sont utilisés pour représenter la relation entre: un utilisateur, d’un appareil et la société.  Les objets périphériques utilisent un certificat signé par ADFS pour ancrer l’appareil physique à l’objet périphérique logique dans ActiveDirectory.  
  
Les appareils inscrits incluent les éléments suivants:  
  
-   **Nom d’affichage**  
  
    Nom convivial de l’appareil.  Pour les appareils windows, il s’agit du nom d’hôte de l’ordinateur.  
  
-   **Id de périphérique**  
  
    GUID généré par le serveur d’inscription de l’appareil.  
  
-   **Empreinte numérique du certificat**  
  
    L’empreinte numérique du certificat X.509 utilisé avec l’appareil inscrit.  
  
-   **Type de système d’exploitation**  
  
    Le type de système d’exploitation sur l’appareil.  
  
-   **Version du système d’exploitation**  
  
    La version du système d’exploitation sur l’appareil.  
  
-   **Est activé**  
  
    Une valeur booléenne qui indique si le périphérique est activé dans ActiveDirectory.  Seuls les appareils activés sont autorisés à accéder aux services.  
  
-   **Utiliser l’heure dernier approximative**  
  
    Heure approximative à laquelle que le périphérique a été utilisé pour accéder à une ressource.  Pour limiter le trafic de réplication, il est uniquement mise à jour tous les 14jours.  
  
-   **Propriétaire enregistré**  
  
    \(SID\) l’identité de sécurité de l’utilisateur qui joint cet appareil à l’espace de travail.  
  
## <a name="ad-fsdrs-server-ssl-certificate-revocation-checking"></a>Vérification de révocation de certificats SSL du serveur FS\/DRS AD  
Le client de jonction vérifie la validité du certificat SSL du serveur ADFS.  Si le certificat SSL de serveur ADFS inclut un point de terminaison \(CRL\) liste de révocation de certificats, le client doit être en mesure d’atteindre le point de terminaison spécifié pour valider le certificat.  
  
Si vous utilisez un environnement de test et une autorité de certification de test \(CA\) pour émettre des certificats SSL de votre serveur que vous pouvez choisir de ne pas inclure le point de terminaison CRL dans les certificats de serveur émis par votre autorité de certification.  Cela permettra au client de jonction d’ignorer la vérification de révocation de certificats.  
  
> [!CAUTION]  
> Cela n’est jamais recommandé pour les systèmes de production  
  

