---
title: Kerberos Constrained Delegation Overview
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e6e62effcb875c0e3a1cdd6c886f3d74923e1b94
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403412"
---
# <a name="kerberos-constrained-delegation-overview"></a>Kerberos Constrained Delegation Overview

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique de présentation pour les professionnels de l’informatique décrit les nouvelles fonctionnalités pour la délégation Kerberos avec restriction dans Windows Server 2012 R2 et Windows Server 2012.

**Description de la fonctionnalité**

La délégation Kerberos contrainte a été introduite pour la première fois dans Windows Server 2003 pour fournir une forme de délégation plus sûre utilisable par les services. Lorsqu’elle est configurée, la délégation contrainte restreint les services sur lesquels le serveur spécifié peut agir pour le compte d’un utilisateur. Des droits d’administrateur de domaine sont requis pour configurer un compte de domaine pour un service et cela restreint le compte à un domaine unique. Dans l’entreprise d’aujourd’hui, les services frontaux ne sont pas conçus pour être limités à l’intégration aux seuls services de leur domaine.

Dans les systèmes d’exploitation antérieurs où l’administrateur de domaine configurait le service, l’administrateur de service ne possédait aucun moyen de savoir quels étaient les services frontaux qui effectuaient des délégations à leurs services de ressources. En outre, tout service frontal qui pouvait effectuer une délégation à un service de ressources représentait un point d’attaque potentiel. Si un serveur hébergeant un service frontal était compromis et qu’il était configuré pour effectuer une délégation à des services de ressources, ces derniers pouvaient également être compromis.

Dans Windows Server 2012 R2 et Windows Server 2012, la capacité à configurer la délégation restreinte pour le service a été transférée de l’administrateur de domaine à l’administrateur de service. De cette manière, l’administrateur de service principal peut autoriser ou refuser les services frontaux.

Pour obtenir des informations détaillées sur la délégation contrainte telle qu’elle a été introduite dans Windows Server 2003, voir [Transition du protocole Kerberos et délégation contrainte](https://technet.microsoft.com/library/cc739587(v=ws.10)).

L’implémentation Windows Server 2012 R2 et Windows Server 2012 du protocole Kerberos comprend des extensions spécifiques pour la délégation restreinte.  Service for User to Proxy (S4U2Proxy) permet à un service d’utiliser son ticket de service Kerberos pour qu’un utilisateur obtienne un ticket de service à partir du centre de distribution de clés (KDC) pour un service principal. Ces extensions permettent de configurer la délégation restreinte sur le compte du service principal, qui peut se trouver dans un autre domaine. Pour plus d’informations sur ces extensions, consultez [ @ no__t-1 ms-SFU-no__t-2 : extensions du protocole Kerberos : Spécification du protocole de service pour l’utilisateur et de délégation confrontée @ no__t-0 dans MSDN Library.

**Applications pratiques**

La délégation contrainte permet aux administrateurs de service de spécifier et d’appliquer les limites d’approbation d’application en limitant l’étendue dans laquelle les services d’application peuvent agir pour le compte d’un utilisateur. Les administrateurs de service peuvent spécifier les comptes de services frontaux qui peuvent effectuer une délégation sur leurs services principaux.

En prenant en charge la délégation avec restriction entre les domaines dans Windows Server 2012 R2 et Windows Server 2012, les services frontaux tels que Microsoft Internet Security and Acceleration (ISA) Server, Microsoft Forefront Threat Management Gateway, Microsoft Exchange Outlook Accès web (OWA) et Microsoft SharePoint Server peuvent être configurés pour utiliser la délégation avec restriction pour s’authentifier auprès des serveurs dans d’autres domaines. Cela permet la prise en charge de solutions de service sur plusieurs domaines via une infrastructure Kerberos existante. La délégation Kerberos contrainte peut être gérée par les administrateurs de domaine ou les administrateurs de service.

## <a name="resource-based-constrained-delegation-across-domains"></a>Délégation contrainte basée sur les ressources sur plusieurs domaines

La délégation Kerberos contrainte peut être utilisée pour fournir une délégation contrainte lorsque les services frontaux et les services de ressources ne sont pas dans le même domaine. Les administrateurs de service sont en mesure de configurer la nouvelle délégation en spécifiant les comptes de domaine des services frontaux qui peuvent emprunter l’identité des utilisateurs sur les objets de compte des services de ressources.

**Quels avantages cette modification procure-t-elle ?**

En prenant en charge la délégation contrainte sur plusieurs domaines, les services peuvent être configurés pour utiliser la délégation contrainte pour s’authentifier auprès des serveurs dans d’autres domaines plutôt qu’utiliser une délégation non contrainte. Cela permet la prise en charge de l’authentification pour des solutions de service sur plusieurs domaines via une infrastructure Kerberos existante sans qu’il soit nécessaire d’approuver les services frontaux pour effectuer une délégation à un service quelconque.

Cela décale également la décision de déterminer si un serveur doit approuver la source d’une identité déléguée de l’administrateur de domaine déléguer au propriétaire de la ressource.

**En quoi le fonctionnement est-il différent ?**

Une modification dans le protocole sous-jacent permet la délégation contrainte sur plusieurs domaines. L’implémentation de Windows Server 2012 R2 et Windows Server 2012 du protocole Kerberos comprend les extensions du protocole S4U2Proxy (service for user to proxy). Il s’agit d’un ensemble d’extensions au protocole Kerberos qui permettent à un service d’utiliser son ticket de service Kerberos pour qu’un utilisateur obtienne un ticket de service à partir du centre de distribution de clés (KDC) pour un service principal.

Pour plus d’informations sur l’implémentation de ces extensions, voir [ @ no__t-1 ms-SFU-SFU @ no__t-2 : extensions du protocole Kerberos : Spécification du protocole de service pour l’utilisateur et de délégation confrontée @ no__t-0 dans MSDN.

Pour plus d’informations sur la séquence de message de base pour la délégation Kerberos avec un ticket TGT (Ticket-Granting Ticket) transmis par rapport aux extensions S4U (Service for User), reportez-vous à la section [1.3.3 Vue d’ensemble du protocole](https://msdn.microsoft.com/library/cc246080(v=prot.10).aspx) dans le document [MS-SFU] : extensions du protocole Kerberos : spécification du protocole de service pour l’utilisateur et de délégation contrainte.

**Implications en matière de sécurité de la délégation avec restriction basée sur les ressources**

La délégation avec restriction basée sur les ressources permet de contrôler la délégation des mains de l’administrateur détenant la ressource faisant l’objet de l’accès. Elle dépend des attributs du service de ressources plutôt que du service approuvé pour le déléguer. Par conséquent, la délégation avec restriction basée sur les ressources ne peut pas utiliser le bit de confiance à l’authentification pour la délégation qui contrôlait précédemment la transition de protocole. Le KDC autorise toujours la transition de protocole lors de l’exécution d’une délégation contrainée basée sur les ressources comme si le bit était défini.

Étant donné que le KDC ne limite pas la transition de protocole, deux nouveaux sid connus ont été introduits pour accorder ce contrôle à l’administrateur des ressources.  Ces sid identifient si la transition de protocole s’est produite et peuvent être utilisées avec les listes de contrôle d’accès standard pour accorder ou limiter l’accès en fonction des besoins.

|SID|Description|
|-------|--------|
|AUTHENTICATION_AUTHORITY_ASSERTED_IDENTITY<br />S-1-18-1|Un SID qui signifie que l’identité du client est déclarée par une autorité d’authentification basée sur la preuve de possession des informations d’identification du client.|
|SERVICE_ASSERTED_IDENTITY<br />S-1-18-2|SID qui signifie que l’identité du client est déclarée par un service.|

Un service principal peut utiliser des expressions ACL standard pour déterminer comment l’utilisateur a été authentifié.

**Comment configurer la délégation avec restriction basée sur les ressources ?**

Pour configurer un service de ressources pour autoriser l’accès à un service frontal pour le compte d’utilisateurs, utilisez les applets de commande Windows PowerShell.

-   Pour récupérer une liste de principaux, utilisez les applets de commande **obtenir-ADComputer**, **obtenir-ADServiceAccount**et **obtenir-ADUser** avec le paramètre **PrincipalsAllowedToDelegateToAccount des propriétés** .

-   Pour configurer le service de ressources, utilisez les applets de commande **New-ADComputer**, **New-ADServiceAccount**, **New-ADUser**, **Set-ADComputer**, **Set-ADServiceAccount**et **Set-ADUser** avec l' **applet de commande Paramètre PrincipalsAllowedToDelegateToAccount** .

## <a name="BKMK_SOFT"></a>Configuration logicielle requise
La délégation avec restriction basée sur les ressources peut uniquement être configurée sur un contrôleur de domaine exécutant Windows Server 2012 R2 et Windows Server 2012, mais elle peut être appliquée dans une forêt en mode mixte.

Vous devez appliquer le correctif logiciel suivant à tous les contrôleurs de domaine exécutant Windows Server 2012 dans les domaines de compte d’utilisateur sur le chemin d’accès de référence entre les domaines frontaux et principaux qui exécutent des systèmes d’exploitation antérieurs à Windows Server :  Échec KDC_ERR_POLICY de la délégation avec restriction basée sur les ressources dans les environnements disposant de contrôleurs de domaine Windows Server 2008 R2 (https://support.microsoft.com/en-gb/help/2665790/resource-based-constrained-delegation-kdc-err-policy-failure-in-enviro).
