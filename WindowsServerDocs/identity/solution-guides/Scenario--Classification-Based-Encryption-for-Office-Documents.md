---
ms.assetid: 73542e1c-53ef-4ddb-89b1-bc563b2bfb49
title: Chiffrement basé sur la classification des scénarios pour les documents Office
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: fa6de270d7d6acf4bf7c99f9a5f9f9457b80b55d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861132"
---
# <a name="scenario-classification-based-encryption-for-office-documents"></a>Scénario : Chiffrement des documents Office d’après leur classification

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La protection des informations confidentielles a principalement pour but de minimiser les risques pour une organisation. Diverses normes de conformité, telles que la norme HIPAA (Health Insurance Portability and Accountability Act) et la norme PCI-DSS (Payment Card Industry Data Security Standard), imposent le chiffrement des informations et de nombreuses raisons professionnelles motivent le chiffrement des données professionnelles à caractère confidentiel. Cependant, le chiffrement des informations est coûteux et il peut nuire à la productivité de l’entreprise. C’est pourquoi les organisations ont souvent des approches et des priorités différentes en matière de chiffrement des données.  
  
## <a name="scenario-description"></a><a name="BKMK_OVER"></a>Description du scénario  
 Windows Server 2012 offre la possibilité de chiffrer automatiquement les fichiers de Microsoft Office sensibles, en fonction de leur classification. Cette opération se fait par le biais de tâches de gestion des fichiers. Ces tâches entraînent une protection AD RMS (Active Directory Rights Management Services) des documents confidentiels quelques secondes après que le fichier a été identifié comme fichier confidentiel sur le serveur de fichiers. Cette opération est facilitée par des tâches de gestion de fichiers continues sur le serveur de fichiers.  
  
Le chiffrement AD RMS fournit une autre couche de protection pour les fichiers. Même si une personne ayant accès à un fichier confidentiel envoie par inadvertance ce fichier par courrier électronique, le fichier est protégé par le chiffrement AD RMS. Les utilisateurs qui souhaitent accéder au fichier doivent d'abord s'authentifier auprès d'un serveur AD RMS pour recevoir la clé de déchiffrement. Ce processus est représenté dans la figure suivante.  
  
![guides de solutions](media/Scenario--Classification-Based-Encryption-for-Office-Documents/DynamicAccessControl_RevGuide_6.JPG)  
  
**Figure 6** Protection RMS basée sur la classification  
  
La prise en charge des formats de fichiers non-Microsoft est disponible par l'intermédiaire de fournisseurs autres que Microsoft. Une fois qu'un fichier a été protégé par le chiffrement AD RMS, des fonctionnalités de gestion de données telles que la classification basée sur la recherche ou sur le contenu ne sont plus disponibles pour ce fichier.  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Vous trouverez ci-dessous les instructions disponibles pour ce scénario :  
  
-   [Considérations relatives à la planification du chiffrement des documents Office](assetId:///14714ba6-d6a2-45e4-aae5-d3318817e52a)  
  
-   [Étapes de la démonstration déployer &#40;le chiffrement des fichiers Office&#41;](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md)  
  
-   [Access Control dynamique : vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Rôles et fonctionnalités inclus dans ce scénario  
Le tableau qui suit décrit les rôles et les fonctionnalités inclus dans ce scénario et détaille la manière dont ils prennent en charge ce dernier.  
  
|Rôle/fonctionnalité|Prise en charge de ce scénario|  
|-----------------|---------------------------------|  
|Rôle AD DS (Active Directory Domain Services)|Les services AD DS fournissent une base de données distribuée qui stocke et gère des informations sur les ressources réseau et les données spécifiques à des applications provenant d’applications utilisant un annuaire. Dans ce scénario, AD DS dans Windows Server 2012 introduit une plateforme d’autorisation basée sur les revendications qui permet de créer des revendications d’utilisateur et d’appareil, une identité composée (revendications utilisateur et de périphérique), un nouveau modèle de stratégies d’accès centralisées et l’utilisation des informations de classification de fichiers dans les décisions d’autorisation.|  
|Rôle des Services de fichiers et de stockage<p>Outils de gestion de ressources pour serveur de fichiers|Les services de fichiers et de stockage fournissent des technologies qui vous permettent de configurer et de gérer un ou plusieurs serveurs de fichiers qui constituent sur votre réseau des emplacements centralisés où vous pouvez stocker des fichiers et les partager avec d'autres utilisateurs. Si vos utilisateurs réseau doivent accéder aux mêmes fichiers et applications, ou si la sauvegarde et la gestion des fichiers centralisées sont des éléments importants pour votre organisation, configurez un ou plusieurs ordinateurs en tant que serveurs de fichiers en ajoutant le rôle Services de fichiers et de stockage et les services de rôle appropriés aux ordinateurs. Dans ce scénario, les administrateurs du serveur de fichiers peuvent configurer des tâches de gestion de fichiers qui font appel à la protection AD RMS pour les documents confidentiels quelques secondes après que le fichier a été identifié comme confidentiel sur le serveur de fichiers (tâches de gestion de fichiers continues sur le serveur de fichiers).|  
|Rôle des services AD RMS (Active Directory Rights Management Services)|Les services AD RMS permettent par le biais de stratégies de Gestion des droits relatifs à l'information à des utilisateurs et des administrateurs de spécifier des autorisations d'accès à des documents, des classeurs et des présentations. Cela aide à éviter l'impression, le transfert ou la copie d'informations sensibles par des personnes non autorisées. Une fois qu’une autorisation a été restreinte pour un fichier en utilisant la Gestion IRM, les restrictions d’accès et d’utilisation sont appliquées quel que soit l’emplacement des informations, car l’autorisation sur un fichier est stockée dans le fichier de document lui-même. Dans ce scénario, le chiffrement AD RMS fournit une autre couche de protection pour les fichiers. Même si une personne ayant accès à un fichier confidentiel envoie par inadvertance ce fichier par courrier électronique, le fichier est protégé par le chiffrement AD RMS. Les utilisateurs qui souhaitent accéder au fichier doivent d'abord s'authentifier auprès d'un serveur AD RMS pour recevoir la clé de déchiffrement.|  
  


