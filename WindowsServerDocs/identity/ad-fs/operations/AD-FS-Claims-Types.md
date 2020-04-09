---
title: Types de revendications d’accès client dans AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d73995b118ec41ffc892700858d20798f637d83b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857482"
---
# <a name="client-access-policy-claim-types-in-ad-fs"></a>Types de revendications de stratégie d’accès client dans AD FS

Pour fournir des informations de contexte de requête supplémentaires, les stratégies d’accès client utilisent les types de revendication suivants, que AD FS génère à partir d’informations d’en-tête de demande pour le traitement.  Pour plus d’informations, consultez [le rôle du moteur de revendications](../technical-reference/the-role-of-the-claims-engine.md).

## <a name="x-ms-forwarded-client-ip"></a>X-MS-forwarded-client-IP

Type de revendication : `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

Cette revendication de AD FS représente une « meilleure tentative » pour déterminer l’adresse IP de l’utilisateur (par exemple, le client Outlook) à l’origine de la demande. Cette revendication peut contenir plusieurs adresses IP, y compris l’adresse de chaque proxy qui a transféré la demande.  Cette revendication est remplie à partir d’un en-tête HTTP qui est actuellement défini uniquement par Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification à AD FS. La valeur de la revendication peut être l’une des suivantes :


- Une seule adresse IP : l’adresse IP du client qui est directement connecté à Exchange Online

    >! Observe L’adresse IP d’un client sur le réseau d’entreprise s’affiche comme l’adresse IP de l’interface externe du proxy ou de la passerelle sortants de l’organisation.

- Une ou plusieurs adresses IP
  - Si Exchange Online ne peut pas déterminer l’adresse IP du client qui se connecte, il définit la valeur en fonction de la valeur de l’en-tête x-forwarded-for, d’un en-tête non standard qui peut être inclus dans les demandes HTTP et est pris en charge par de nombreux clients, des équilibrages de charge et des proxys sur le marché.
  - Plusieurs adresses IP indiquant l’adresse IP du client et l’adresse de chaque proxy qui a transmis la demande sont séparées par une virgule.

    >! Observe Les adresses IP associées à l’infrastructure Exchange Online ne seront pas présentes dans la liste.


>! Tres Exchange Online ne prend actuellement en charge que les adresses IPV4. il ne prend pas en charge les adresses IPV6. 


## <a name="x-ms-client-application"></a>X-MS-client-application

Type de revendication : `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

Cette revendication de AD FS représente le protocole utilisé par le client final, qui correspond vaguement à l’application utilisée.  Cette revendication est remplie à partir d’un en-tête HTTP qui est actuellement défini uniquement par Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification à AD FS. Selon l’application, la valeur de cette revendication sera l’une des suivantes :



- Dans le cas des appareils qui utilisent Exchange Active Sync, la valeur est Microsoft. Exchange. ActiveSync. 
- L’utilisation du client Microsoft Outlook peut avoir l’une des valeurs suivantes :
    - Découverte automatique Microsoft. Exchange.
    - Microsoft. Exchange. OfflineAddressBook
    - Microsoft. Exchange. RPC
    - Microsoft. Exchange. WebServices
    - Microsoft. Exchange. MAPI
- Les autres valeurs possibles pour cet en-tête sont les suivantes :
    - Microsoft. Exchange. PowerShell
    - Microsoft. Exchange. SMTP
    - Microsoft. Exchange. PopImap
    - Microsoft. Exchange. pop
    - Microsoft. Exchange. IMAP

## <a name="x-ms-client-user-agent"></a>X-MS-client-user-agent

Type de revendication : `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

Cette revendication de AD FS fournit une chaîne représentant le type d’appareil utilisé par le client pour accéder au service. Cela peut être utilisé lorsque les clients souhaitent empêcher l’accès à certains appareils (tels que des types particuliers de téléphones intelligents).  Cette revendication est remplie à partir d’un en-tête HTTP qui est actuellement défini uniquement par Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification à AD FS. Voici des exemples de valeurs pour cette revendication : les valeurs ci-dessous (sans s’y limiter).
>! Observe Voici des exemples de ce que la valeur x-ms-user-agent peut contenir pour un client dont la valeur x-ms-client-application est « Microsoft. Exchange. ActiveSync »

- Vortex/1.0
- Apple-iPad1C1/812.1
- Apple-iPhone3C1/811.2
- Apple-iPhone/704.11
- Moto-DROID2/4.5.1
- SAMSUNGSPHD700/100.202
- Android/0.3

>! Observe Il est également possible que cette valeur soit vide.


## <a name="x-ms-proxy"></a>X-MS-proxy

Type de revendication : `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

Cette revendication de AD FS indique que la demande est passée par le serveur proxy de Fédération.  Cette revendication est renseignée par le serveur proxy de Fédération, qui remplit l’en-tête lors du passage de la demande d’authentification au service FS (Federation Service) de back end. AD FS ensuite le convertit en revendication. 

La valeur de la revendication est le nom DNS du serveur proxy de Fédération qui a transmis la demande.

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-Endpoint-Absolute-Path (actif/passif)

Type de revendication : `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

Ce type de revendication peut être utilisé pour déterminer les demandes provenant de clients « actifs » (riches) par rapport aux clients « passifs » (basés sur un navigateur Web). Cela permet aux requêtes externes provenant d’applications basées sur un navigateur, telles que le Accès web Outlook, SharePoint Online ou le portail Office 365, d’être autorisées pendant que les demandes provenant de clients enrichis, telles que Microsoft Outlook, sont bloquées.

La valeur de la revendication est le nom du service AD FS qui a reçu la demande.
