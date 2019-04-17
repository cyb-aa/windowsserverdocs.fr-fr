---
ms.assetid: 53ee93e2-09ea-4f8b-adb7-c24c59f055ea
title: Utilisation des URI dans AD FS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 305bf0cece742c961604dacda7e27b8eac8065e5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
>S’applique à: Windows Server2016, Windows Server2012R2

# <a name="how-uris-are-used-in-ad-fs"></a>Utilisation des URI dans AD FS
Un \(URI\) Uniform Resource Identifier est une chaîne de caractères qui est utilisée comme un identificateur unique.  Dans AD FS, les URI sont utilisés pour identifier les adresses de réseaux partenaires et les objets de configuration.  Lorsqu’il est utilisé pour identifier les adresses de réseaux partenaires, l’URI est toujours une URL.  Lorsqu’il est utilisé pour identifier les objets de configuration, l’URI peut être un URN ou une URL.  Pour plus d’informations sur les URI, voir [RFC 2396](https://go.microsoft.com/fwlink/?LinkId=48289) et [RFC 3986](https://go.microsoft.com/fwlink/?LinkId=90453).  
  
## <a name="uris-as-partner-network-addresses"></a>URI en tant qu’adresses de réseaux partenaires  
Voici les URL d’adresse de réseau qui sont plus souvent gérées par les administrateurs dans AD FS.  
  
-   Les URL du Service de fédération, y compris WS-Federation, SAML, WS-Trust, les métadonnées de fédération, WS-MetadataExchange, confidentialité et URL de l’organisation  
  
-   Les URL d’une relation de confiance, notamment WS-Federation, SAML et les URL des métadonnées de fédération  
  
-   Les URL d’une approbation de fournisseur de revendications, notamment WS-Federation, SAML et les URL des métadonnées de fédération  
  
## <a name="uris-as-object-identifiers"></a>URI en tant qu’identificateurs d’objet  
Le tableau suivant décrit les identificateurs qui sont plus souvent gérées par les administrateurs dans AD FS.  
  
|Nom de l’identificateur|Description|Comparaisons|  
|-------------------|---------------|---------------|  
|Identificateur du Service de fédération|Cet identificateur est utilisé pour identifier le Service de fédération.  Il est utilisé par les parties de confiance qui utilisent des revendications à partir de ce Service de fédération, ainsi que les fournisseurs de revendications que problème de revendications pour ce Service de fédération.|Lorsqu’un utilisateur demande des revendications à partir d’un fournisseur de revendications pour ce Service de fédération, l’identificateur du Service de fédération est utilisé pour identifier la cible des revendications.<br /><br />Lorsque ce Service de fédération reçoit les revendications à partir d’un fournisseur de revendications, il sera vérifier que les revendications sont étendues pour qu’il en recherchant son identificateur de Service de fédération.<br /><br />Lorsqu’une partie de confiance reçoit des revendications à partir de ce Service de fédération, la partie de confiance vérifie que l’émetteur des revendications correspond à l’identificateur du Service de fédération.|  
|Identificateur de partie de confiance|Cet identificateur est utilisé pour identifier la partie de confiance pour ce Service de fédération.  Il est utilisé lors de l’émission des revendications à la partie de confiance.|Lorsqu’un utilisateur demande des revendications à partir de ce Service de fédération pour la partie de confiance, l’identificateur de partie de confiance sera utilisé pour identifier la partie de confiance pour lequel les revendications doivent être ciblées.  Cette comparaison est effectuée à l’aide du préfixe \(see below\) correspondante.<br /><br />Lorsque la partie de confiance reçoit les revendications, il vérifie son identificateur dans le jeton de sécurité pour vous assurer que les revendications sont destinées.|  
|Identificateur du fournisseur de revendications|Cet identificateur est utilisé pour identifier le fournisseur de revendications pour ce Service de fédération.  Il est utilisé lors de la réception des revendications à partir du fournisseur de revendications.|Lorsque ce Service de fédération reçoit des revendications à partir du fournisseur de revendications, ce Service de fédération vérifie que l’émetteur des revendications correspond à l’identificateur du fournisseur de revendications.|  
|Type de revendication|Cet identificateur est utilisé pour définir le type de revendication.  Il est utilisé par ce Service de fédération, les fournisseurs de revendications et les parties de confiance lors de l’envoi et la réception des revendications.|Lorsque le Service de fédération reçoit des revendications à partir d’un fournisseur de revendications, les règles de revendication associés à l’approbation de fournisseur de revendications correspondante permettent à l’administrateur comparer les types de revendications et de traiter les revendications.  Les règles de revendication associés à une partie de confiance permettent à l’administrateur de comparer les types de revendications issues les règles d’approbation de fournisseur de revendications et choisir les revendications à émettre.|  
  
## <a name="uri-prefix-matching-for-relying-party-identifiers"></a>Préfixe URI pour les identificateurs de parties partie de confiance  
La syntaxe de chemin d’accès d’un URI est organisée hiérarchiquement et est délimitée par l’option tout «\ / «caractères ou tous les»:» caractères.  Par conséquent, le chemin d’accès peut être fractionné en sections de chemin d’accès basées sur le caractère de délimitation.  Correspondance du préfixe lorsque, chaque section doit être complètement correspondre aux règles de correspondance \ (ces règles régissent la casse des matches\). Pour plus d’informations sur les règles de correspondance, consultez le document RFC mentionné ci-dessus.  
  
Lorsqu’une partie de confiance est identifiée dans une demande au Service de fédération, AD FS utilise une logique de correspondance de préfixe pour déterminer s’il existe une confiance correspondante dans la base de données de configuration AD FS.  
  
Par exemple, si l’identificateur de partie de confiance dans la base de données de configuration AD FS \(URI1\) est un préfixe pour l’identificateur de partie de confiance en entrant demander \(URI2\), puis les éléments suivants doivent être remplis:  
  
-   Les délimiteurs de fin \(slashes and colons\) de chemin d’accès sections ou des autorités doivent être ignorés.  
  
-   L’autorité et schéma d’URI1 et URI2 doivent se trouver une correspondance exacte de la casse  
  
-   Chaque section de chemin d’accès de URI1 doit être une correspondance exacte \ (basé sur le respect de la casse chosen\) à la section correspondante de chemin d’accès de URI2  
  
-   Uri2 peut comporter plus de sections de chemin d’accès que URI1, mais URI1 ne doit pas comporter plus de sections de chemin d’accès qu’URI2  
  
-   Uri1 ne peut pas comporter plus de sections de chemin d’accès qu’URI2  
  
-   Si URI1 comporte une chaîne de requête, il doit correspondre exactement à une chaîne de requête d’URI2  
  
-   Si URI1 comporte un fragment, il doit correspondre exactement à un fragment d’URI2.  
  
Le tableau suivant fournit des exemples supplémentaires.  
  
|Dans la base de données de configuration AD FS identificateur de partie de confiance|Identificateur de message de demande de partie de confiance|Demande de correspondances identificateur l’identificateur de configuration?|Raison|  
|------------------------------------------------------------|-----------------------------------------------|------------------------------------------------------------|----------|  
|http:///\/contoso.com|http:///\/contoso.com|TRUE|Correspondance exacte|  
|http:///\/contoso.com\/|http:///\/contoso.com|TRUE|Les barres obliques finales sont ignorées.|  
|http:///\/contoso.com|http:///\/contoso.com\/|TRUE|Les barres obliques finales sont ignorées.|  
|http:///\/contoso.com|http:///\/contoso.com\/hr|TRUE|Uri1 ne comporte aucun chemin d’accès et correspond au schéma et une autorité d’URI2|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hr\/Web|TRUE|Premières sections de chemin d’accès correspond à, URI1 ne comporte aucune seconde section de chemin d’accès|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hr\/Web\/?m\=t|TRUE|Mêmes raisons que ci-dessus, la chaîne de requête ne change rien|  
|http:///\/contoso.com\/hr\/|http:///\/contoso.com\/hrw\/main|FALSE (FAUX)|Chemin d’accès de uri1 section 1 ne correspond pas à la section de chemin d’accès de URI2 1|  
|http:///\/contoso.com\/hr|http:///\/contoso.com|FALSE (FAUX)|Uri1 comporte plus de sections de chemin d’accès qu’URI2|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hrweb|FALSE (FAUX)|Premières sections de chemin d’accès ne correspondent pas|  
|http:///\/contoso.com\/?m\=t|http:///\/contoso.com\/?m\=f|FALSE (FAUX)|Parties de chaîne de requête ne correspondent pas|  
|https:\/\/contoso.com|http:///\/contoso.com|FALSE (FAUX)|Certaines parties de schéma ne correspondent pas|  
|http:///\/STS.contoso.com|http:///\/contoso.com|FALSE (FAUX)|Certaines parties d’autorité ne correspondent pas|  
|http:///\/contoso.com|http:///\/STS.contoso.com|FALSE (FAUX)|Certaines parties d’autorité ne correspondent pas|  
  

