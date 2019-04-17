---
ms.assetid: 73542e1c-53ef-4ddb-89b1-bc563b2bfb49
title: "Scénario de chiffrement Classification des Documents Office"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 38e058f36522ba6a2c81694cb883d0946b04adda
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-classification-based-encryption-for-office-documents"></a>Scénario: Chiffrement Classification des Documents Office

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Protection des informations confidentielles a principalement pour minimiser les risques pour l’organisation. Diverses normes de conformité, telles que la d’intégrité Insurance Portability and Accountability Act (HIPAA) et paiement carte Industry Data Security Standard (PCI-DSS), imposent le chiffrement d’informations, et il existe de nombreuses raisons professionnelles motivent. Toutefois, le chiffrement des informations est coûteux et il peut nuire à la productivité de l’entreprise. Par conséquent, les organisations ont tendance à avoir des approches différentes et des priorités pour le chiffrement de leurs informations.  
  
## <a name="BKMK_OVER"></a>Description du scénario  
 Windows Server2012 offre la possibilité de chiffrer automatiquement des fichiers MicrosoftOffice confidentiels, en fonction de leur classification. Cela s’effectue via des tâches de gestion de fichiers qui font appel à la protection ActiveDirectory Rights Management Services (ADRMS) des documents confidentiels quelques secondes après que le fichier a été identifié comme un fichier confidentiel sur le serveur de fichiers. Cette opération est facilitée par les tâches de gestion de fichiers continues sur le serveur de fichiers.  
  
Le chiffrement ADRMS fournit une autre couche de protection pour les fichiers. Même si une personne ayant accès à un fichier confidentiel envoie par inadvertance ce fichier par courrier électronique, le fichier est protégé par le chiffrement ADRMS. Les utilisateurs qui souhaitent accéder au fichier doivent tout d’abord s’authentifier auprès d’un serveur ADRMS pour recevoir la clé de déchiffrement. La figure suivante illustre ce processus.  
  
![guides de solutions](media/Scenario--Classification-Based-Encryption-for-Office-Documents/DynamicAccessControl_RevGuide_6.JPG)  
  
**Figure6** protection RMS basée sur la Classification  
  
Prise en charge des formats de fichier non Microsoft est disponible par le biais de fournisseurs non-Microsoft. Une fois un fichier a été protégé par le chiffrement ADRMS, les fonctionnalités de gestion des données telles que la classification en fonction de recherche ou de contenu ne sont plus disponibles pour ce fichier.  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Voici les instructions disponibles pour ce scénario:  
  
-   [Considérations de planification pour le chiffrement des Documents Office](assetId:///14714ba6-d6a2-45e4-aae5-d3318817e52a)  
  
-   [Déployer le chiffrement des fichiers Office et #40; les étapes de démonstration et #41;](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md)  
  
-   [Contrôle d’accès dynamique: Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Fonctionnalités et rôles inclus dans ce scénario  
Le tableau suivant répertorie les rôles et fonctionnalités qui font partie de ce scénario et décrit la prise en charge.  
  
|Rôle ou une fonctionnalité|Comment il prend en charge ce scénario|  
|-----------------|---------------------------------|  
|Rôle des Services de domaine ActiveDirectory (ADDS)|Les services ADDS fournit une base de données distribuée qui stocke et gère des informations sur les ressources réseau et les données spécifiques à l’application à partir d’applications d’annuaire. Dans ce scénario, les services ADDS dans Windows Server2012 introduit une plateforme d’autorisation basée sur les revendications qui permet de créer des revendications d’utilisateur et revendications de périphérique, une identité composée (utilisateur plus revendications de périphérique), un nouveau modèle de stratégies d’accès centralisée et l’utilisation des informations de classification des fichiers dans les décisions d’autorisation.|  
|Rôle Services de fichiers et stockage<br /><br />Gestionnaire de ressources du serveur de fichiers|Services de fichiers et stockage fournit des technologies pour aider à configurer et de gérer un ou plusieurs serveurs de fichiers qui constituent des emplacements centralisés sur votre réseau où vous pouvez stocker les fichiers et les partager avec des utilisateurs. Si vos utilisateurs réseau doivent accéder aux mêmes fichiers et applications, ou si la gestion centralisée de sauvegarde et de fichier est importante pour votre organisation, vous devez configurer un ou plusieurs ordinateurs en tant qu’un serveur de fichiers en ajoutant le rôle Services de fichiers et stockage et les services de rôle appropriés sur les ordinateurs. Dans ce scénario, les administrateurs de serveur de fichiers peuvent configurer des tâches de gestion de fichiers qui font appel à la protection ADRMS pour les documents confidentiels quelques secondes après que le fichier a été identifié comme un fichier confidentiel sur le serveur de fichiers (tâches de gestion de fichiers continues sur le serveur de fichiers).|  
|Rôle d’ActiveDirectory Rights Management Services ADRMS)|Les services ADRMS permet de personnes et les administrateurs (via des stratégies d’Information Rights Management (IRM)) pour spécifier des autorisations d’accès aux documents, des classeurs et des présentations. Cela aide à éviter des informations sensibles d’être imprimés, le transfert ou copiés par des personnes non autorisées. Une fois que l’autorisation d’un fichier a été restreinte par IRM, les restrictions d’accès et d’utilisation sont appliquées quel que soit l’où les informations sont, car l’autorisation à un fichier est stockée dans le fichier de document lui-même. Dans ce scénario, le chiffrement ADRMS fournit une autre couche de protection pour les fichiers. Même si une personne ayant accès à un fichier confidentiel envoie par inadvertance ce fichier par courrier électronique, le fichier est protégé par le chiffrement ADRMS. Les utilisateurs qui souhaitent accéder au fichier doivent tout d’abord s’authentifier auprès d’un serveur ADRMS pour recevoir la clé de déchiffrement.|  
  


