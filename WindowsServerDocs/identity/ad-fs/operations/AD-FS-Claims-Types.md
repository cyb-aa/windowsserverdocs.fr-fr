---
ms.assetid: ''
title: Accès client types de revendications dans ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1e37aded450555d293806d1ed8903a51e3df9424
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839140"
---
#<a name="client-access-policy-claim-types-in-ad-fs"></a>Stratégie d’accès client Types de revendications dans ADFS

Pour fournir des informations de contexte de requête supplémentaires, les stratégies d’accès Client utiliser les types de revendication suivants, AD FS génère à partir des informations d’en-tête de demande pour le traitement.  Pour plus d’informations, consultez [le rôle du moteur de revendications](../technical-reference/the-role-of-the-claims-engine.md).

##<a name="x-ms-forwarded-client-ip"></a>X-MS-Forwarded-Client-IP

Type de revendication : `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

Cette revendication AD FS représente une « meilleure tentative » permettant de vérifier l’adresse IP de l’utilisateur (par exemple, le client Outlook) qui effectue la requête. Cette revendication peut contenir plusieurs adresses IP, y compris l’adresse de chaque serveur proxy qui a transféré la demande.  Cette revendication est remplie à partir d’un en-tête HTTP qui est actuellement définies uniquement par Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification pour AD FS. La valeur de la revendication peut être une des opérations suivantes :


- Une seule adresse IP - adresse IP du client qui est directement connecté à Exchange Online

    >! [Remarque] L’adresse IP d’un client sur le réseau d’entreprise s’affiche comme adresse IP externe d’interface de passerelle ou proxy sortant de l’organisation.

- Une ou plusieurs adresses IP
    - Si Exchange Online ne peut pas déterminer l’adresse IP du client qui se connecte, il définit la valeur basée sur la valeur de l’en-tête x-forwarded-for, un en-tête non standard qui peut être inclus dans basées sur demande et est pris en charge par nombreux clients, les équilibreurs de charge, et proxys sur le marché.
    - Plusieurs adresses IP indiquant l’adresse IP du client et l’adresse de chaque proxy qui a transmis la demande doivent être séparés par une virgule.

    >! [Remarque] Les adresses IP liées à l’infrastructure Exchange Online ne seront pas présents dans la liste.


>! [Avertissement] Exchange Online prend actuellement en charge uniquement les adresses IPV4. Il ne prend pas en charge les adresses IPV6. 


## <a name="x-ms-client-application"></a>X-MS-Client-Application

Type de revendication : `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

Cette revendication AD FS représente le protocole utilisé par le client final, qui correspond étroitement à l’application utilisée.  Cette revendication est remplie à partir d’un en-tête HTTP qui est actuellement définies uniquement par Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification pour AD FS. Selon l’application, la valeur de cette revendication sera une des opérations suivantes :



- Dans le cas des appareils qui utilisent Exchange Active Sync, la valeur est Microsoft.Exchange.ActiveSync. 
- Utilisation du client Microsoft Outlook peut entraîner une des valeurs suivantes :
    - Microsoft.Exchange.Autodiscover
    - Microsoft.Exchange.OfflineAddressBook
    - Microsoft.Exchange.RPC
    - Microsoft.Exchange.WebServices
    - Microsoft.Exchange.Mapi
- Les autres valeurs possibles pour cet en-tête sont les suivantes :
    - Microsoft.Exchange.Powershell
    - Microsoft.Exchange.SMTP
    - Microsoft.Exchange.PopImap
    - Microsoft.Exchange.Pop
    - Microsoft.Exchange.Imap

## <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent

Type de revendication : `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

Cette revendication AD FS fournit une chaîne représentant le type d’appareil que le client utilise pour accéder au service. Cela peut servir lorsque les clients souhaitent empêcher l’accès pour certains périphériques (tels que les types particuliers de téléphones intelligents).  Cette revendication est remplie à partir d’un en-tête HTTP qui est actuellement définies uniquement par Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification pour AD FS. Exemples de valeurs pour cette revendication incluent (mais ne sont pas limitées à) les valeurs ci-dessous.
>! [Remarque] Le Voici des exemples de ce que peut contenir la valeur de x-ms-user-agent pour un client dont x-ms-application cliente est « Microsoft.Exchange.ActiveSync »

- Tourbillon/1.0
- Apple-iPad1C1/812.1
- Apple-iPhone3C1/811.2
- Apple-iPhone/704.11
- Moto-DROID2/4.5.1
- SAMSUNGSPHD700/100.202
- Android/0.3

>! [Remarque] Il est également possible que cette valeur est vide.


## <a name="x-ms-proxy"></a>X-MS-Proxy

Type de revendication : `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

Cette revendication AD FS indique que la demande est passée par le serveur proxy de fédération.  Cette revendication est remplie par le serveur proxy de fédération, qui remplit l’en-tête lors du passage de la demande d’authentification au serveur principal de Service de fédération. AD FS puis la convertit en une revendication. 

La valeur de la revendication est le nom DNS du serveur proxy de fédération qui a transmis la demande.

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-point de terminaison-absolue-chemin d’accès (actif ou passif)

Type de revendication : `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

Ce type de revendication peut être utilisé pour déterminer les demandes provenant des clients (complets) « actifs » et « passifs » (web-clients sur navigateur). Ainsi, les requêtes externes à partir des applications de navigateur telles que Outlook Web Access, SharePoint Online ou le portail Office 365 pour être autorisée tandis que les demandes provenant des clients riches tels que Microsoft Outlook sont bloquées.

La valeur de la revendication est le nom du service AD FS qui a reçu la demande.
