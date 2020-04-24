---
title: Architecture de référence pour l’hébergement de postes de travail
description: Recommandations architecturales pour la création d’une solution d’hébergement de bureaux avec Services Bureau à distance et Azure.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/02/2016
ms.topic: article
ms.assetid: 1bac5dd3-8430-46ee-8bef-10cc4b7cc437
author: lizap
manager: dongill
ms.openlocfilehash: c2bc0c2ba3d12ea1caf8737369ba882f69b111e4
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80818452"
---
# <a name="desktop-hosting-reference-architecture"></a>Architecture de référence pour l’hébergement de postes de travail

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Cet article définit un ensemble de blocs architecturaux qui permettent d’utiliser Services Bureau à distance et des machines virtuelles Microsoft Azure pour créer des services d’applications et de bureaux Windows hébergés multilocataires, appelés « hébergement de bureaux ». Vous pouvez utiliser cette référence d’architecture pour créer des solutions d’hébergement de bureaux extrêmement sûres, évolutives et fiables pour les petites et moyennes organisations comptant entre 5 et 5 000 utilisateurs.    
  
Cette architecture de référence est principalement destinée aux fournisseurs d’hébergement qui souhaitent exploiter les services d’infrastructure Microsoft Azure pour proposer des services d’hébergement de bureaux et des licences d’accès abonné à plusieurs clients via le programme [Microsoft SPLA](https://www.microsoft.com/hosting/en/us/licensing/splabenefits.aspx) (Service Provider Licensing Agreement). Cette architecture de référence est aussi destinée aux clients finaux qui souhaitent créer et gérer des solutions d’hébergement de bureaux dans les services d’infrastructure Microsoft Azure pour leurs employés à l’aide de [droits étendus de licence d’accès client d’utilisateur Bureau à distance via Software Assurance](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf).   
  
Pour fournir une solution d’hébergement de bureaux, les clients Software Assurance et partenaires d’hébergement tirent parti de Windows Server pour proposer aux utilisateurs Windows une expérience utilisateur d’application familière aux utilisateurs professionnels et consommateurs. Basé sur les fondations de Windows 10, Windows Server 2016 offre une expérience utilisateur et une prise en charge d’application familière.    
  
Ce document traite uniquement des points suivants :   
  
* Recommandations en matière de conception architecturale pour un service d’hébergement de bureaux. Les informations détaillées, telles que des procédures de déploiement, les performances et la planification de la capacité sont expliquées dans des documents distincts. Pour obtenir des informations plus générales sur les services d’infrastructure Azure, consultez [Virtual Machines Documentation](https://azure.microsoft.com/documentation/services/virtual-machines/) (Documentation sur les machines virtuelles).   
  
* Bureaux basés sur une session, applications RemoteApp et bureaux personnels basés sur un serveur qui utilisent un hôte de session Bureau à distance Windows Server 2016. Les infrastructures de bureau virtuel basées sur un client Windows ne sont pas couvertes étant donné qu’il n’y a pas de programme SPLA pour les systèmes d’exploitation clients Windows. Les infrastructures de bureau virtuel basées sur Windows Server sont autorisées dans le programme SPLA, et les infrastructures de bureau virtuel basées sur un client Windows sont autorisées sur du matériel dédié avec des licences de clients finaux dans certains scénarios. Toutefois, les infrastructures de bureau virtuel basées sur un client ne sont pas couvertes dans ce document.   
  
* Fonctionnalités et produits Microsoft, principalement Windows Server 2016 et les services d’infrastructure Microsoft Azure.   
  
* Services d’hébergement de bureaux pour les locataires allant de 5 à 5 000 utilisateurs.   Pour les locataires plus volumineux, vous devrez peut-être modifier cette architecture pour fournir des performances adéquates. L’interface utilisateur graphique Bureau à distance du Gestionnaire de serveur n’est pas recommandée pour les déploiements de plus de 500 utilisateurs. PowerShell est recommandé pour la gestion des déploiements Services Bureau à distance compris entre 500 et 5 000 utilisateurs.   
  
* L’ensemble minimal de composants et de services requis pour un service d’hébergement de bureaux. Il existe plusieurs composants et services facultatifs qui peuvent être ajoutés pour améliorer un service d’hébergement de bureaux, mais ils ne sont pas traités dans ce document.    
  
Après avoir lu ce document, le lecteur doit comprendre :   
- Les blocs de conception qui sont nécessaires pour fournir une solution d’hébergement de bureaux multilocataire, fiable et sécurisée basée sur des services Microsoft Azure.  
- L’objectif de chaque bloc de conception et la façon dont ils s’imbriquent entre eux.  
  
Il existe plusieurs façons de créer une solution d’hébergement de bureaux basée sur cette architecture. Cette architecture présente l’intégration et les améliorations dans Azure avec Windows Server 2016. D’autres options de déploiement sont disponibles avec le [Desktop Hosting Reference Architecture Guide](https://go.microsoft.com/fwlink/p/?LinkId=517389) (Guide d’architecture de référence pour l’hébergement de bureaux) pour Windows Server 2012 R2.    
  
Les rubriques suivantes sont traitées :  
- [Architecture logique pour l’hébergement de bureaux](Desktop-hosting-logical-architecture.md)  
- [Présentation des rôles Services Bureau à distance](Understanding-RDS-roles.md)
- [Présentation de l’environnement d’hébergement de bureaux](Understanding-the-desktop-hosting-environment.md)  
- [Services et considérations Azure pour l’hébergement de postes de travail](Azure-services-and-considerations-for-desktop-hosting.md)
  
 


