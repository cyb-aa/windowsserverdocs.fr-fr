---
title: Architecture de référence pour l’hébergement de postes de travail
description: Guide architectural pour la création d’un solution avec les services Bureau à distance et Azure d’hébergement de bureau.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/02/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1bac5dd3-8430-46ee-8bef-10cc4b7cc437
author: lizap
manager: dongill
ms.openlocfilehash: 6f235fd89c34c00601c802f4ea71e440af630169
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890240"
---
# <a name="desktop-hosting-reference-architecture"></a>Architecture de référence pour l’hébergement de postes de travail

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cet article définit un ensemble de blocs architecturaux pour l’utilisation des Services Bureau à distance (RDS) et les machines virtuelles Microsoft Azure pour créer multiclient hébergé applications et bureau de Windows services, que nous appelons « hébergement du bureau. » Vous pouvez utiliser cette référence de l’architecture pour créer des solutions pour petites et moyennes pour les organisations avec 5 à 5 000 utilisateurs d’hébergement de bureau hautement sécurisé, évolutif et fiable.    
  
Le principal pour cette architecture de référence sont fournisseurs d’hébergement qui souhaitent exploiter les Services d’Infrastructure Microsoft Azure pour fournir des services d’hébergement de postes de travail et les licences d’accès abonné (SAL) à plusieurs clients via la [ Contrat de licence de fournisseur de services de Microsoft](https://www.microsoft.com/hosting/en/us/licensing/splabenefits.aspx) programme (SPLA). Un deuxième public pour cette architecture de référence sont les clients finaux qui souhaitent créer et gérer des solutions d’hébergement dans les Services d’Infrastructure Microsoft Azure pour leurs employés à l’aide de bureau [RDS CAL d’utilisateur étendu les droits via le logiciel Assurance](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf) (SA).   
  
Pour fournir un hébergement des solutions d’hébergement de bureau partenaires et les clients SA tirer parti de Windows Server pour fournir aux utilisateurs Windows une expérience d’application les utilisateurs professionnels et particuliers connaissent déjà. Basé sur les fondations de Windows 10, Windows Server 2016 fournit prise en charge de l’application familière et utilisateur expérience.    
  
La portée de ce document est limitée à :   
  
* Conseils de conception architecturale pour un service d’hébergement de bureau. Des informations détaillées, telles que les procédures de déploiement, les performances et la planification de la capacité sont expliquées dans des documents distincts. Pour obtenir des informations plus générales sur les Services d’Infrastructure Azure, consultez [Microsoft Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/).   
  
* Bureaux basés sur session, les applications RemoteApp et bureaux personnels basée sur le serveur qui utilisent Windows Server 2016 Remote Desktop Session Host (hôte de Session Bureau à distance). Windows clientes infrastructures de bureau virtuel ne sont pas couverts, car il n’existe aucune licence accord SPLA (Service Provider) pour les systèmes d’exploitation clients Windows. Infrastructures de bureau virtuel basé sur le serveur de Windows sont autorisés sous le SPLA, et Windows clientes infrastructures de bureau virtuel sont autorisées sur du matériel dédié avec des licences de client final dans certains scénarios. Toutefois, les infrastructures de bureau virtuel basé sur le client sont hors de portée pour ce document.   
  
* Les produits Microsoft et des fonctionnalités, principalement Windows Server 2016 et les Services d’Infrastructure Microsoft Azure.   
  
* Hébergement de services pour les locataires allant de 5 à 5 000 utilisateurs du bureau.   Pour les clients plus volumineux, vous devrez peut-être modifier cette architecture pour fournir des performances adéquates. L’interface utilisateur graphique de gestionnaire de serveur RDS (GUI) n'est pas recommandée pour les déploiements de plus de 500 utilisateurs. PowerShell est recommandée pour la gestion des déploiements de services Bureau à distance entre 500 et 5 000 utilisateurs.   
  
* L’ensemble minimal de composants et les services requis pour un service d’hébergement de bureau. Il existe plusieurs composants facultatifs et les services qui peuvent être ajoutés pour améliorer un service d’hébergement de bureau, mais ils sont hors de portée pour ce document.    
  
Après avoir lu ce document, le lecteur doit comprendre :   
- Les indications des sections qui sont nécessaires pour fournir un bureau fiable, sécurisé et mutualisé solution basée dans Microsoft Azure Services d’hébergement.  
- L’objectif de chaque bloc de construction et comment ils s’imbriquent.  
  
Il existe plusieurs façons de créer un solution basée sur cette architecture d’hébergement de bureau. Cette architecture présente des améliorations dans Azure avec Windows Server 2016 et intégration. Autres options de déploiement sont disponibles avec la [Guide d’Architecture de référence d’hébergement Desktop](https://go.microsoft.com/fwlink/p/?LinkId=517389) pour Windows Server 2012 R2.    
  
Les rubriques suivantes sont traitées :  
- [Architecture logique l’hébergement de postes de travail](Desktop-hosting-logical-architecture.md)  
- [Comprendre les rôles des services Bureau à distance](Understanding-RDS-roles.md)
- [Comprendre l’environnement d’hébergement bureau](Understanding-the-desktop-hosting-environment.md)  
- [Considérations pour l’hébergement de postes de travail et des services Azure](Azure-services-and-considerations-for-desktop-hosting.md)
  
 


