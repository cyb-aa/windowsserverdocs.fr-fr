---
title: "Vue d’ensemble de la délégation contrainte du protocole Kerberos"
description: "Sécurité de Windows Server"
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
ms.openlocfilehash: e90aa5e395fa43a3f94cccb13444fd6991ac1074
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="kerberos-constrained-delegation-overview"></a>Vue d’ensemble de la délégation contrainte du protocole Kerberos

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique de présentation destinée aux professionnels de l’informatique décrit les nouvelles fonctionnalités pour la délégation contrainte Kerberos dans Windows Server2012R2 et Windows Server2012.

## <a name="feature-description"></a>Description de la fonctionnalité
La délégation Kerberos contrainte a été introduite dans Windows Server2003 pour fournir une forme de délégation qui pourrait être utilisée par les services plus sûre. Lorsqu’elle est configurée, la délégation contrainte restreint les services sur lesquels le serveur spécifié peut agir pour le compte d’un utilisateur. Cela nécessite des privilèges d’administrateur de domaine pour configurer un compte de domaine pour un service et cela restreint le compte à un seul domaine. Dans les entreprises, les services frontaux ne sont pas conçus pour être limités à une intégration aux seuls services dans leur domaine.

Dans les systèmes d’exploitation antérieurs où l’administrateur de domaine configuré le service, l’administrateur de service ne possédait aucun moyen de savoir quels services frontaux qui effectuaient pour les services de ressources qu’ils appartenant. Et tout service frontal qui pouvait effectuer une délégation à un service de ressource représenté un point d’attaque potentiel. Si un serveur hébergeant un service frontal était compromis, et il a été configuré pour effectuer une délégation à des services de ressources, les services de ressources peuvent également être compromis.

Dans Windows Server2012R2 et Windows Server2012, possibilité de configurer la délégation contrainte pour le service a été transférée de l’administrateur de domaine à l’administrateur de service. De cette façon, l’administrateur de service principal peut autoriser ou refuser les services frontaux.

Pour plus d’informations sur la délégation contrainte, introduite dans Windows Server2003, voir [Transition du protocole Kerberos et délégation contrainte](https://technet.microsoft.com/library/cc739587(v=ws.10)).

L’implémentation de Windows Server2012R2 et Windows Server2012 du protocole Kerberos inclut des extensions spécifiquement pour la délégation contrainte.  Service pour l’utilisateur de Proxy (S4U2Proxy) permet à un service à utiliser son ticket de service Kerberos pour un utilisateur d’obtenir un ticket de service à partir du centre de Distribution de clés (KDC) à un service principal. Ces extensions permettent la délégation contrainte pour être configuré sur le compte du service principal, qui peut être dans un autre domaine. Pour plus d’informations sur ces extensions, voir [\[MS-SFU\]: Extensions du protocole Kerberos: Service pour l’utilisateur et la spécification du protocole de délégation contrainte](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx) dans la bibliothèque MSDN.

## <a name="practical-applications"></a>Applications pratiques
La délégation contrainte permet aux administrateurs de service pour spécifier et appliquer des délimitations d’approbation d’application en limitant l’étendue dans laquelle les services d’application peuvent agir sur le nom d’un utilisateur. Les administrateurs peuvent configurer ce qui les comptes de service frontaux peuvent déléguer à leurs services principaux.

En prenant en charge la délégation contrainte sur plusieurs domaines dans Windows Server2012R2 et Windows Server2012, les services frontaux tels que MicrosoftInternet Security and Acceleration (ISA) Server, MicrosoftForefront Threat Management Gateway, MicrosoftExchange Outlook Web Access (OWA) et MicrosoftSharePoint Server peuvent être configurés pour utiliser la délégation contrainte pour s’authentifier auprès des serveurs dans d’autres domaines. Cela prend en charge sur plusieurs domaines de solutions de service à l’aide d’une infrastructure Kerberos existante. La délégation contrainte Kerberos peut être gérée par les administrateurs de domaine ou administrateurs de service.

## <a name="new-and-changed-functionality"></a>Fonctionnalités nouvelles et modifiées
**Délégation contrainte basée sur les ressources sur plusieurs domaines**

Délégation Kerberos contrainte peut être utilisée pour fournir une délégation contrainte lorsque les services frontaux et les services de ressources ne sont pas dans le même domaine. Les administrateurs de services sont en mesure de configurer la nouvelle délégation en spécifiant les comptes de domaine des services frontaux qui peuvent emprunter l’identité des utilisateurs sur les objets de compte de services de ressources.

**La valeur ajouter par ce changement?**

Prise en charge de la délégation contrainte sur plusieurs domaines, les services peuvent être configurés pour utiliser la délégation contrainte pour s’authentifier auprès des serveurs dans d’autres domaines plutôt qu’à l’aide de la délégation non contrainte. Cette fonctionnalité permet d’authentification pour entre les solutions de service de domaine à l’aide d’une infrastructure Kerberos existante sans avoir besoin d’approuver les services frontaux pour déléguer à n’importe quel service.

**Ce qui fonctionne différemment?**

Une modification dans le protocole sous-jacent permet la délégation contrainte sur plusieurs domaines. L’implémentation de Windows Server2012R2 et Windows Server2012 du protocole Kerberos inclut des extensions de Service pour l’utilisateur au protocole de Proxy (S4U2Proxy). Il s’agit d’un ensemble d’extensions du protocole Kerberos qui permet à un service à utiliser son ticket de service Kerberos pour un utilisateur d’obtenir un ticket de service à partir du centre de Distribution de clés (KDC) à un service principal.

Pour plus d’informations sur ces extensions, voir [\[MS-SFU\]: Extensions du protocole Kerberos: Service pour l’utilisateur et la spécification du protocole de délégation contrainte](https://msdn.microsoft.com/library/cc246071(PROT.10).aspx) dans MSDN.

Pour plus d’informations sur la séquence de message de base pour la délégation Kerberos avec un ticket-granting ticket (TGT ticket) par rapport au Service pour les extensions de l’utilisateur (S4U), voir la section [1.3.3 vue d’ensemble du protocole](https://msdn.microsoft.com/library/cc246080(v=prot.10).aspx) dans le document [MS-SFU]: Extensions du protocole Kerberos: Service pour l’utilisateur et la spécification du protocole de la délégation contrainte.

Pour configurer un service de ressource afin d’autoriser l’accès à un service frontal pour le compte d’utilisateurs, utilisez les applets de commande Windows PowerShell.

-   Pour récupérer une liste de principaux, utilisez le **Get-ADComputer**, **Get-ADServiceAccount**, et **Get-ADUser** applets de commande avec le **Properties PrincipalsAllowedToDelegateToAccount** paramètre.

-   Pour configurer le service de ressources, utilisez le **New-ADComputer**, **New-ADServiceAccount**, **New-ADUser**, **Set-ADComputer**, **Set-ADServiceAccount**, et **Set-ADUser** applets de commande avec le **PrincipalsAllowedToDelegateToAccount** paramètre.

## <a name="BKMK_SOFT"></a>Configuration logicielle requise
Délégation contrainte basée sur les ressources peut uniquement être configurée sur un contrôleur de domaine exécutant Windows Server2012R2 et Windows Server2012, mais peut être appliquée au sein d’une forêt mixte.

Vous devez appliquer le correctif logiciel suivant à tous les contrôleurs de domaine exécutant Windows Server2012dans utilisateur domaines de comptes sur le chemin d’accès de référence entre les domaines frontaux et principaux qui exécutent les systèmes d’exploitation antérieurs à Windows Server: délégation échec KDC_ERR_POLICY dans les environnements dotés de contrôleurs de domaine Windows Server2008R2 contrainte basée sur une ressource.
