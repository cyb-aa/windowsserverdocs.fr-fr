---
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: "Annexe A glossaire de contrôle d’accès dynamique"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2044355812e95b9a5bfe90e33257f11ce78cf93
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>Annexe a: glossaire de contrôle d’accès dynamique

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Voici la liste des termes et définitions qui sont incluses dans le scénario de contrôle d’accès dynamique.  
  
|Terme|Définition|  
|--------|--------------|  
|Classification automatique|Classification qui se produit en fonction des propriétés de classification sont déterminées par les règles de classification configurés par un administrateur.|  
|CAPID|ID de stratégie d’accès centralisée. Cet ID fait référence à une stratégie d’accès centralisée spécifique, et il est utilisé pour faire référence à la stratégie à partir du descripteur de sécurité des fichiers et dossiers.|  
|Règle d’accès central|Une règle qui inclut une condition et une expression d’accès.|  
|Stratégie d’accès centralisée|Stratégies qui sont créées et hébergées dans Active Directory.|  
|Contrôle d’accès basé sur les revendications|Un modèle qui utilise des revendications pour prendre des décisions de contrôle d’accès aux ressources.|  
|Classification|Le processus de détermination des propriétés de classification des ressources et affecter ces propriétés pour les métadonnées associées aux ressources. Voir aussi AutomaticClassification REF \h \\ * classification automatique MERGEFORMAT, REF InheritedClassification \h \\\ * classification MERGEFORMAT hérité et REF ManualClassification \h \\\ * classification MERGEFORMAT manuel.|  
|Revendications de périphérique|Une revendication qui est associée avec le système.  Avec les revendications d’utilisateur, il est inclus dans le jeton d’un utilisateur tente d’accéder à une ressource.|  
|Liste de contrôle d’accès discrétionnaire (DACL)|Une liste de contrôle d’accès qui identifie les tiers de confiance qui sont autorisés ou refusés l’accès à une ressource sécurisable. Elle peut être modifiée à l’appréciation du propriétaire de la ressource.|  
|Propriété de ressource|Propriétés (tels que des étiquettes) qui décrivent un fichier et sont attribuées aux fichiers à l’aide de la classification automatique ou manuelle classification. Exemples: durée de rétention, du projet et la sensibilité à la période.|  
|Gestionnaire de ressources du serveur de fichiers|Une fonctionnalité du système d’exploitation Windows Server qui assure la gestion des quotas de dossier, filtrage de fichiers, des rapports de stockage, classification des fichiers et les tâches de gestion de fichiers sur un serveur de fichiers.|  
|Étiquettes et les propriétés du dossier|Les propriétés et les étiquettes qui décrivent un dossier et sont attribuées manuellement par les administrateurs et les propriétaires des dossiers. Ces propriétés affecter des valeurs de propriété par défaut pour les fichiers au sein de ces dossiers, par exemple, le secret ou service.|  
|Stratégie de groupe|Un ensemble de règles et les stratégies qui contrôle l’environnement de travail des utilisateurs et ordinateurs dans un environnement Active Directory.|  
|Près de classification en temps réel|Classification automatique est exécutée peu après qu’un fichier est créé ou modifié.|  
|Près de tâches de gestion en temps réel|Tâches de gestion qui sont effectuées juste après de fichiers (un fichier est créé ou modifié. Ces tâches sont déclenchées par la classification en temps réel proche.|  
|Unité d’organisation (UO)|Un conteneur Active Directory qui représente les structures logiques, hiérarchiques au sein d’une organisation. Il est la plus petite étendue à la stratégie de groupe de paramètres sont appliqués.|  
|Propriété sécurisée|Une propriété de classification que vous pouvez faire confiance à l’exécution d’autorisation pour être une assertion valide sur la ressource à un certain point-à-temps. Dans le contrôle d’accès basé sur les revendications, une propriété sécurisée qui est affectée à une ressource est traitée comme une revendication de ressource.|  
|Descripteur de sécurité|Structure de données qui contient des informations de sécurité associées à une ressource sécurisable, comme les listes de contrôle d’accès.|  
|Langage de définition de descripteur de sécurité|Spécification qui décrit les informations contenues dans un descripteur de sécurité comme une chaîne de texte.|  
|Stratégie de mise en attente|Une stratégie d’accès centralisée qui n’est pas encore en vigueur.|  
|Liste de contrôle d’accès système (SACL)|Une liste de contrôle d’accès qui spécifie les types de tentatives d’accès par des tiers de confiance particuliers pour lequel les enregistrements d’audit doivent être générés.|  
|Revendications d’utilisateur|Attributs d’un utilisateur qui sont fournies dans le jeton de sécurité utilisateur. Exemples: service, entreprise, projet et habilitation.  Les informations dans le jeton d’utilisateur à partir des systèmes antérieurs à Windows Server 2012, tels que les groupes de sécurité dont l’utilisateur fait partie, peuvent également être considérées comme des revendications d’utilisateur. Certaines revendications d’utilisateur sont fournies via Active Directory et d’autres sont calculées dynamiquement, tel que l’utilisateur connecté avec une carte à puce.|  
|Jeton de l’utilisateur|Un objet de données qui identifie un utilisateur et les revendications d’utilisateur et les revendications de périphérique qui sont associées à cet utilisateur. Il est utilisé pour autoriser l’accès aux ressources de l’utilisateur.|  
  
## <a name="see-also"></a>Voir aussi  
[Contrôle d’accès dynamique: Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  


