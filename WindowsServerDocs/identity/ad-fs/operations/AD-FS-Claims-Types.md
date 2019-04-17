---
ms.assetid: 
title: "Accès au client de revendication types dans ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1e37aded450555d293806d1ed8903a51e3df9424
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
#<a name="client-access-policy-claim-types-in-ad-fs"></a>Dans ADFS, les Types de revendication de stratégie d’accès client

Pour fournir des informations supplémentaires sur la demande de contexte, les stratégies d’accès Client utilisez les types de revendication suivants, qui génère des services ADFS à partir des informations d’en-tête de demande de traitement.  Pour plus d’informations, voir [le rôle du moteur de revendications](../technical-reference/the-role-of-the-claims-engine.md).

##<a name="x-ms-forwarded-client-ip"></a>X-MS-transmis-Client-IP

Type de revendication: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

Cette revendication ADFS représente un «mieux pour tenter «permettant de vérifier l’adresse IP de l’utilisateur (par exemple, le client Outlook) effectue la demande. Cette revendication peut contenir plusieurs adresses IP, y compris l’adresse de chaque serveur proxy qui a transmis la demande.  Cette revendication est remplie à partir d’un en-tête HTTP qui n’est actuellement défini uniquement à Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification pour ADFS. La valeur de la revendication peut être une des opérations suivantes:


- Une seule adresse IP: adresse IP du client qui est connecté directement à Exchange Online

    >! [Remarque] L’adresse IP d’un client sur le réseau d’entreprise s’affiche comme adresse IP externe d’interface de passerelle ou de proxy sortantes de l’organisation.

- Une ou plusieurs adresses IP
    - Si Exchange Online ne peut pas déterminer l’adresse IP du client qui se connecte, il définit la valeur en fonction de la valeur de l’en-tête x transmis pour un en-tête non standard qui peut être inclus dans le protocole HTTP sur demande et est pris en charge par de nombreux clients, les équilibreurs de charge et les proxys sur le marché.
    - Plusieurs adresses IP qui indique l’adresse IP du client et l’adresse de chaque serveur proxy qui a transmis la demande seront séparées par une virgule.

    >! [Remarque] Les adresses IP liés à l’infrastructure Exchange Online ne sera pas présents dans la liste.


>! [Avertissement] Exchange Online prend actuellement en charge uniquement les adresses IPV4. Il ne prend pas en charge les adresses IPV6. 


## <a name="x-ms-client-application"></a>Application X-MS-Client

Type de revendication: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

Cette revendication ADFS représente le protocole utilisé par le client, qui correspond librement à l’application en cours d’utilisation.  Cette revendication est remplie à partir d’un en-tête HTTP qui n’est actuellement défini uniquement à Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification pour ADFS. En fonction de l’application, la valeur de cette revendication sera une des opérations suivantes:



- Dans le cas des périphériques qui utilisent Exchange Active Sync, la valeur est Microsoft.Exchange.ActiveSync. 
- Utilisation du client MicrosoftOutlook peut provoquer une des valeurs suivantes:
    - Microsoft.Exchange.Autodiscover
    - Microsoft.Exchange.OfflineAddressBook
    - Microsoft.Exchange.RPC
    - Microsoft.Exchange.WebServices
    - Microsoft.Exchange.Mapi
- Les autres valeurs possibles pour cet en-tête sont les suivantes:
    - Microsoft.Exchange.Powershell
    - Microsoft.Exchange.SMTP
    - Microsoft.Exchange.PopImap
    - Microsoft.Exchange.Pop
    - Microsoft.Exchange.Imap

## <a name="x-ms-client-user-agent"></a>X-MS-Client-Agent utilisateur

Type de revendication: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

Cette revendication ADFS fournit une chaîne représentant le type de périphérique que le client utilise pour accéder au service. Cela peut servir lorsque les clients souhaitez empêcher l’accès pour certains appareils (tels que des Smartphones particuliers).  Cette revendication est remplie à partir d’un en-tête HTTP qui n’est actuellement défini uniquement à Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification pour ADFS. Exemples de valeurs pour cette revendication inclure (mais ne sont pas limités à) les valeurs ci-dessous.
>! [Remarque] Le vous trouverez ci-dessous des exemples de la valeur x-ms-user-agent qui peut contenir pour un client dont x-ms-application cliente est «Microsoft.Exchange.ActiveSync»

- Tourbillon/1.0
- Apple-iPad1C1/812.1
- Apple-iPhone3C1/811.2
- Apple-iPhone/704.11
- Moto-DROID2/4.5.1
- SAMSUNGSPHD700/100.202
- Android/0,3

>! [Remarque] Il est également possible que cette valeur est vide.


## <a name="x-ms-proxy"></a>X-MS-Proxy

Type de revendication: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

Cette revendication ADFS indique que la demande a passé sur le serveur proxy de fédération.  Cette revendication est remplie par le serveur proxy de fédération, qui remplit l’en-tête lors du passage de la demande d’authentification pour le Service de fédération principal. ADFS puis le convertit en une revendication. 

La valeur de la revendication est le nom DNS du serveur proxy de fédération qui a transmis la demande.

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-point de terminaison-absolue-chemin d’accès (actif et passif)

Type de revendication: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

Ce type de revendication peut être utilisé pour déterminer les demandes provenant de clients (riches) «actives» et «passifs» (web-clients sur navigateur). Ainsi, les requêtes externes à partir des applications de navigateur comme Outlook Web Access, SharePoint Online ou le portail Office 365 à autoriser tandis que les demandes provenant de clients complexes telles que MicrosoftOutlook sont bloqués.

La valeur de la revendication est le nom du service ADFS qui a reçu la demande.
