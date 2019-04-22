---
title: Kerberos Constrained Delegation Overview
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51923b0a-0c1a-47b2-93a0-d36f8e295589
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: d77dd6f6f310ae71a4d9e2d52b44cc9b1bef6401
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821930"
---
# <a name="kerberos-constrained-delegation-overview"></a>Kerberos Constrained Delegation Overview

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique de présentation destinée aux professionnels de l’informatique décrit les nouvelles fonctionnalités pour la délégation contrainte Kerberos dans Windows Server 2012 R2 et Windows Server 2012.

**Description de la fonctionnalité**

La délégation Kerberos contrainte a été introduite pour la première fois dans Windows Server 2003 pour fournir une forme de délégation plus sûre utilisable par les services. Lorsqu’elle est configurée, la délégation contrainte restreint les services sur lesquels le serveur spécifié peut agir pour le compte d’un utilisateur. Des droits d’administrateur de domaine sont requis pour configurer un compte de domaine pour un service et cela restreint le compte à un domaine unique. De nos jours, les services frontaux ne sont pas conçus pour être limités à une intégration aux seuls services dans leur domaine.

Dans les systèmes d’exploitation antérieurs où l’administrateur de domaine configurait le service, l’administrateur de service ne possédait aucun moyen de savoir quels étaient les services frontaux qui effectuaient des délégations à leurs services de ressources. En outre, tout service frontal qui pouvait effectuer une délégation à un service de ressources représentait un point d’attaque potentiel. Si un serveur hébergeant un service frontal était compromis et qu’il était configuré pour effectuer une délégation à des services de ressources, ces derniers pouvaient également être compromis.

Dans Windows Server 2012 R2 et Windows Server 2012, possibilité de configurer la délégation contrainte pour le service a été transférée à partir de l’administrateur de domaine à l’administrateur de service. De cette manière, l’administrateur de service principal peut autoriser ou refuser les services frontaux.

Pour obtenir des informations détaillées sur la délégation contrainte telle qu’elle a été introduite dans Windows Server 2003, voir [Transition du protocole Kerberos et délégation contrainte](https://technet.microsoft.com/library/cc739587(v=ws.10)).

L’implémentation de Windows Server 2012 R2 et Windows Server 2012 du protocole Kerberos inclut des extensions spécifiquement pour la délégation.  Service for User to Proxy (S4U2Proxy) permet à un service d’utiliser son ticket de service Kerberos pour qu’un utilisateur obtienne un ticket de service à partir du centre de distribution de clés (KDC) pour un service principal. Ces extensions permettent la délégation contrainte à être configuré sur le serveur principal compte du service, qui peut être dans un autre domaine. Pour plus d’informations sur ces extensions, consultez [ \[MS-SFU\]: extensions du protocole Kerberos : Service pour l’utilisateur et la spécification du protocole de la délégation contrainte](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx) dans MSDN Library.

**Applications pratiques**

La délégation contrainte permet aux administrateurs de service pour spécifier et appliquer des limites d’approbation application en limitant l’étendue dans laquelle les services d’application peuvent agir au nom d’un utilisateur. Les administrateurs de service peuvent spécifier les comptes de services frontaux qui peuvent effectuer une délégation sur leurs services principaux.

Par prise en charge la délégation contrainte sur plusieurs domaines dans Windows Server 2012 R2 et Windows Server 2012, les services frontaux tels que Microsoft Internet Security and Acceleration (ISA) Server, Microsoft Forefront Threat Management Gateway, Microsoft Exchange Outlook Web Access (OWA) et Microsoft SharePoint Server peuvent être configurés pour utiliser la délégation contrainte pour s’authentifier auprès des serveurs dans d’autres domaines. Cela permet la prise en charge de solutions de service sur plusieurs domaines via une infrastructure Kerberos existante. La délégation Kerberos contrainte peut être gérée par les administrateurs de domaine ou les administrateurs de service.

## <a name="resource-based-constrained-delegation-across-domains"></a>Délégation contrainte basée sur les ressources sur plusieurs domaines

La délégation Kerberos contrainte peut être utilisée pour fournir une délégation contrainte lorsque les services frontaux et les services de ressources ne sont pas dans le même domaine. Les administrateurs de service sont en mesure de configurer la nouvelle délégation en spécifiant les comptes de domaine des services frontaux qui peuvent emprunter l’identité des utilisateurs sur les objets de compte des services de ressources.

**Quels avantages cette modification procure-t-elle ?**

En prenant en charge la délégation contrainte sur plusieurs domaines, les services peuvent être configurés pour utiliser la délégation contrainte pour s’authentifier auprès des serveurs dans d’autres domaines plutôt qu’utiliser une délégation non contrainte. Cela permet la prise en charge de l’authentification pour des solutions de service sur plusieurs domaines via une infrastructure Kerberos existante sans qu’il soit nécessaire d’approuver les services frontaux pour effectuer une délégation à un service quelconque.

Cela déplace également la décision de si un serveur doit approuver la source d’une identité déléguée à partir de l’administrateur de domaine de délégation pour le propriétaire de la ressource.

**En quoi le fonctionnement est-il différent ?**

Une modification dans le protocole sous-jacent permet la délégation contrainte sur plusieurs domaines. L’implémentation de Windows Server 2012 R2 et Windows Server 2012 du protocole Kerberos inclut des extensions du Service pour l’utilisateur au protocole de Proxy (S4U2Proxy). Il s’agit d’un ensemble d’extensions au protocole Kerberos qui permettent à un service d’utiliser son ticket de service Kerberos pour qu’un utilisateur obtienne un ticket de service à partir du centre de distribution de clés (KDC) pour un service principal.

Pour plus d’informations sur ces extensions, consultez [ \[MS-SFU\]: extensions du protocole Kerberos : Service pour l’utilisateur et la spécification du protocole de la délégation contrainte](https://msdn.microsoft.com/library/cc246071(PROT.10).aspx) dans MSDN.

Pour plus d’informations sur la séquence de message de base pour la délégation Kerberos avec un ticket TGT (Ticket-Granting Ticket) transmis par rapport aux extensions S4U (Service for User), reportez-vous à la section [1.3.3 Vue d’ensemble du protocole](https://msdn.microsoft.com/library/cc246080(v=prot.10).aspx) dans le document [MS-SFU] : extensions du protocole Kerberos : spécification du protocole de service pour l’utilisateur et de délégation contrainte.

**Implications en matière de sécurité de la délégation contrainte basée sur les ressources**

Contrainte basée sur Délégation de contrôle de place de la délégation entre les mains de l’administrateur qui possède la ressource sollicitée. Il dépend des attributs du service des ressources, plutôt que le service soit approuvé pour déléguer. Par conséquent, la délégation contrainte basée sur les ressources ne peut pas utiliser le bit de confiance-à-authentifier-for-Delegation qui précédemment contrôlés transition de protocole. Le KDC permet toujours de transition de protocole lors de l’exécution basée sur les ressources de la délégation contrainte comme si le bit ont été définies.

Étant donné que le KDC ne limite pas la transition de protocole, deux nouveaux SID bien connu ont été introduites pour donner ce contrôle à l’administrateur de la ressource.  Ces identificateurs SID identifier si la transition de protocole s’est produite et peut être utilisée avec les listes de contrôle d’accès standard pour autoriser ou limiter l’accès en fonction des besoins.

|SID|Description|
|-------|--------|
|AUTHENTICATION_AUTHORITY_ASSERTED_IDENTITY<br />S-1-18-1|Un SID qui signifie que l’identité du client est déclaré par une autorité d’authentification basée sur la preuve de possession des informations d’identification du client.|
|SERVICE_ASSERTED_IDENTITY<br />S-1-18-2|Un SID qui signifie que l’identité du client est déclaré par un service.|

Un service principal peut utiliser des expressions ACL standard pour déterminer comment l’utilisateur a été authentifié.

**Comment devez-vous configurer les ressources de la délégation ?**

Pour configurer un service de ressources pour autoriser l’accès à un service frontal pour le compte d’utilisateurs, utilisez les applets de commande Windows PowerShell.

-   Pour récupérer une liste de principaux, utilisez le **Get-ADComputer**, **Get-ADServiceAccount**, et **Get-ADUser** applets de commande avec le **propriétés PrincipalsAllowedToDelegateToAccount** paramètre.

-   Pour configurer le service de ressources, utilisez le **New-ADComputer**, **New-ADServiceAccount**, **New-ADUser**, **Set-ADComputer**,  **Set-ADServiceAccount**, et **Set-ADUser** applets de commande avec le **PrincipalsAllowedToDelegateToAccount** paramètre.

## <a name="BKMK_SOFT"></a>Configuration logicielle requise
La délégation contrainte basée sur les ressources peut uniquement être configurée sur un contrôleur de domaine exécutant Windows Server 2012 R2 et Windows Server 2012, mais peut être appliquée au sein d’une forêt mixte.

Vous devez appliquer le correctif logiciel suivant à tous les contrôleurs de domaine exécutant Windows Server 2012 dans utilisateur domaines de comptes sur le chemin d’accès de référence entre les domaines frontaux et principaux qui exécutent les systèmes d’exploitation antérieurs à Windows Server :  échec KDC_ERR_POLICY de délégation contrainte basée sur les ressources dans des environnements dotés de contrôleurs de domaine basés sur Windows Server 2008 R2.
