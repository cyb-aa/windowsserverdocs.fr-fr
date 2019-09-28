---
ms.assetid: 53ee93e2-09ea-4f8b-adb7-c24c59f055ea
title: Utilisation des URI dans AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2ae91b16448c5acd61712332310544c1fd66789f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385491"
---
# <a name="how-uris-are-used-in-ad-fs"></a>Utilisation des URI dans AD FS
Un URI \(\) Uniform Resource Identifier est une chaîne de caractères utilisée comme identificateur unique.  Dans ADFS, les URI sont utilisés pour identifier des adresses de réseau partenaire et des objets de configuration.  Lorsqu’il est utilisé pour identifier les adresses de réseaux partenaires, l’URI est toujours une URL.  Lorsqu’il est utilisé pour identifier des objets de configuration, l’URI peut être un URN ou une URL.  Pour en savoir plus sur les URI, voir [RFC 2396](https://go.microsoft.com/fwlink/?LinkId=48289) et [RFC 3986](https://go.microsoft.com/fwlink/?LinkId=90453).  
  
## <a name="uris-as-partner-network-addresses"></a>URI en tant qu’adresses de réseaux partenaires  
Voici les URL d’adresses de réseaux qui sont plus souvent gérées par les administrateurs dans AD FS.  
  
-   URL de la service FS (Federation Service), y compris WS\-Federation, SAML, WS\-Trust, les métadonnées de\-Fédération, WS MetadataExchange, les URL de confidentialité et d’organisation  
  
-   URL d’une approbation de partie de confiance, y compris\-les URL de métadonnées WS Federation, SAML et de Fédération  
  
-   URL d’une approbation de fournisseur de revendications, y\-compris les URL de métadonnées WS Federation, SAML et de Fédération  
  
## <a name="uris-as-object-identifiers"></a>URI en tant qu’identificateurs d’objet  
Le tableau suivant décrit les identificateurs qui sont plus souvent gérés par les administrateurs dans AD FS.  
  
|Nom de l’identificateur|Description|Comparaisons|  
|-------------------|---------------|---------------|  
|Identificateur du service de fédération|Cet identificateur permet d’identifier le service de fédération.  Il est utilisé par les parties de confiance qui utilisent des revendications issues de ce service de fédération, ainsi que par les fournisseurs de revendications qui émettent des revendications destinées à ce service de fédération.|Lorsqu’un utilisateur demande des revendications auprès d’un fournisseur de revendications pour ce service de fédération, l’identificateur du service de fédération est utilisé pour identifier la cible des revendications.<br /><br />Lorsque ce service de fédération reçoit les revendications de la part d’un fournisseur de revendications, il vérifie que ces dernières s’y rapportent en recherchant son identificateur de service de fédération.<br /><br />Lorsqu’une partie de confiance reçoit des revendications de la part de ce service de fédération, celle-ci vérifie que l’émetteur des revendications correspond à l’identificateur du service de fédération.|  
|Identificateur de partie de confiance|Cet identificateur permet d’identifier la partie de confiance pour ce service de fédération.  Il est utilisé lors de l’émission de revendications destinées à la partie de confiance.|Lorsqu’un utilisateur demande des revendications auprès de ce service de fédération pour la partie de confiance, l’identificateur de cette dernière est utilisé pour identifier la partie de confiance pour laquelle les revendications doivent être ciblées.  Cette comparaison est effectuée à l’aide \(de la\)correspondance de préfixe, voir ci-dessous.<br /><br />Lorsque la partie de confiance reçoit les revendications, elle recherche son identificateur dans le jeton de sécurité pour garantir que les revendications lui sont bien destinées.|  
|Identificateur du fournisseur de revendications|Cet identificateur permet d’identifier le fournisseur des revendications pour ce service de fédération.  Il est utilisé lors de la réception de revendications issues du fournisseur de revendications.|Lorsque ce service de fédération reçoit des revendications de la part du fournisseur de revendications, celui-ci vérifie que l’émetteur des revendications correspond à l’identificateur du fournisseur de revendications.|  
|Type de revendication|Cet identificateur permet de définir le type de revendication.  Il est utilisé par ce service de fédération, les fournisseurs de revendications et les parties de confiance lors de l’envoi et de la réception des revendications.|Lorsque le service de fédération reçoit des revendications de la part d’un fournisseur de revendications, les règles de revendication associées à l’approbation du fournisseur de revendications correspondante permettent à l’administrateur de traiter les revendications et d’en comparer les types.  Les règles de revendication associées à une approbation de partie de confiance permettent également à l’administrateur de comparer les types des revendications issues des règles d’approbation du fournisseur de revendications et de choisir les revendications à émettre.|  
  
## <a name="uri-prefix-matching-for-relying-party-identifiers"></a>Correspondance du préfixe URI pour les identificateurs de parties de confiance  
La syntaxe de chemin d’accès d’un URI est organisée hiérarchiquement et est délimitée par\/tous les caractères « » ou « : ».  Par conséquent, le chemin d’accès peut être fractionné en sections basées sur le caractère de délimitation.  En cas de correspondance de préfixe, chaque section doit être une correspondance complète en \(fonction des règles de correspondance. ces\)règles régissent la casse des correspondances. Pour plus d’informations sur les règles de correspondance, consultez le document RFC mentionné ci-dessus.  
  
Lorsqu’une partie de confiance est identifiée dans une demande faite au service de fédération, AD FS utilise une logique de correspondance de préfixe pour déterminer s’il existe une approbation de partie de confiance correspondante dans la base de données de configuration AD FS.  
  
Par exemple, si l’identificateur de la partie de confiance dans la base de \(données\) de configuration AD FS uri1 est un préfixe de l’identificateur de la partie \(de\)confiance dans la demande entrante uri2, les conditions suivantes doivent être remplies :  
  
-   Les délimiteurs \(\) de fin et les signes deux-points des sections ou des autorités de chemin d’accès doivent être ignorés  
  
-   Les parties relatives à l’autorité et au schéma d’URI1 et URI2 doivent être une correspondance exacte qui n’est pas sensible à la casse.  
  
-   Chaque section de chemin d’accès de uri1 doit être \(une correspondance exacte en fonction du\) respect de la casse choisi pour la section de chemin d’accès correspondante de uri2  
  
-   URI2 peut comporter plus de sections de chemin d’accès que URI1, mais l’inverse est impossible.  
  
-   URI1 ne peut pas comporter plus de sections de chemin d’accès qu’URI2  
  
-   Si URI1 comporte une chaîne de requête, celle-ci doit correspondre exactement à une chaîne de requête d’URI2.  
  
-   Si URI1 comporte un fragment, celui-ci doit correspondre exactement à un fragment d’URI2.  
  
Le tableau suivant fournit des exemples supplémentaires.  
  
|Identificateur de partie de confiance de la base de données de configuration AD FS|Identificateur de partie de confiance du message de demande|L’identificateur de demande correspond-il à l’identificateur de configuration ?|Reason|  
|------------------------------------------------------------|-----------------------------------------------|------------------------------------------------------------|----------|  
|http :\/\/contoso.com|http :\/\/contoso.com|TRUE|Correspondance exacte|  
|http :\/\/contoso.com\/|http :\/\/contoso.com|TRUE|Les barres obliques finales sont ignorées.|  
|http :\/\/contoso.com|http :\/\/contoso.com\/|TRUE|Les barres obliques finales sont ignorées.|  
|http :\/\/contoso.com|http :\/\/contoso.comhr\/|TRUE|URI1 ne comporte aucun chemin d’accès et correspond au schéma et à l’autorité d’URI2|  
|http :\/\/contoso.comhr\/|http :\/\/contoso.comRH\/Web\/|TRUE|Les premières sections de chemin d’accès correspondent, URI1 ne comporte pas de deuxième section de chemin d’accès|  
|http :\/\/contoso.comhr\/|http :\/\/contoso.comhr\/Web\/?m\=t\/|TRUE|Les mêmes raisons que ci-dessus, la chaîne de requête ne change rien|  
|http :\/\/contoso.comhr\/\/|http :\/\/contoso.comrunbookWorkerhybride\/main\/|FALSE|La section de chemin d’accès 1 d’URI1 ne correspond pas à la section de chemin d’accès 1 d’URI2|  
|http :\/\/contoso.comhr\/|http :\/\/contoso.com|FALSE|Le nombre de sections de chemin d’accès d’URI1 est supérieur à celui d’URI2|  
|http :\/\/contoso.comhr\/|http :\/\/contoso.comHRWeb\/|FALSE|Les premières sections de chemin d’accès ne correspondent pas|  
|http :\/\/contoso.com?\/mt\=|http :\/\/contoso.com?\/mf\=|FALSE|Certaines parties de la chaîne de requête ne correspondent pas|  
|https :\/\/contoso.com|http :\/\/contoso.com|FALSE|Certaines parties de schéma ne correspondent pas|  
|http :\/\/STS.contoso.com|http :\/\/contoso.com|FALSE|Certaines parties d’autorité ne correspondent pas|  
|http :\/\/contoso.com|http :\/\/STS.contoso.com|FALSE|Certaines parties d’autorité ne correspondent pas|  
  

