---
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: Glossaire de contrôle d’accès dynamique annexe A
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2044355812e95b9a5bfe90e33257f11ce78cf93
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856700"
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>Annexe A : Glossaire de contrôle d’accès dynamique

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Voici la liste des termes et définitions qui sont incluses dans le scénario de contrôle d’accès dynamique.  
  
|Terme|Définition|  
|--------|--------------|  
|Classification automatique|Classification qui se produit en fonction des propriétés de classification qui sont déterminées par les règles de classification configurées par un administrateur.|  
|CAPID|ID de stratégie d’accès centralisée. Cet ID fait référence à une stratégie d’accès centralisée, et il est utilisé pour faire référence à la stratégie du descripteur de sécurité des fichiers et dossiers.|  
|Règle d’accès centralisée|Une règle qui inclut une condition et une expression d’accès.|  
|Stratégie d’accès centralisée|Stratégies sont créées et hébergées dans Active Directory.|  
|Contrôle d’accès basée sur les revendications|Un paradigme qui utilise les revendications pour prendre des décisions de contrôle d’accès aux ressources.|  
|Classification|Le processus de détermination des propriétés de classification des ressources et d’affectation de ces propriétés pour les métadonnées qui est associé aux ressources. Voir aussi REF AutomaticClassification \h \\* la classification automatique MERGEFORMAT, REF InheritedClassification \h \\ \* classification MERGEFORMAT héritée et REF ManualClassification \h \\ \* Classification de MERGEFORMAT manuel.|  
|Revendication de périphérique|Une revendication qui est associée au système.  Avec les revendications d’utilisateur, il est inclus dans le jeton d’un utilisateur tente d’accéder à une ressource.|  
|Liste de contrôle d’accès discrétionnaire (DACL)|Une liste de contrôle d’accès qui identifie le tiers de confiance qui sont accordés ou refusés l’accès à une ressource sécurisable. Elle peut être modifiée à la discrétion du propriétaire de la ressource.|  
|Propriété de ressource|Propriétés (telles que les étiquettes) qui décrivent un fichier et sont attribuées aux fichiers à l’aide de la classification automatique ou la classification manuelle. Par exemple : Période de sensibilité, projet et la rétention.|  
|Outils de gestion de ressources pour serveur de fichiers|Une fonctionnalité du système d’exploitation Windows Server qui assure la gestion des quotas de dossier, filtrage des fichiers, des rapports de stockage, la classification des fichiers et tâches de gestion de fichier sur un serveur de fichiers.|  
|Étiquettes et les propriétés du dossier|Les propriétés et les étiquettes qui décrivent un dossier et sont attribuées manuellement par les administrateurs et les propriétaires des dossiers. Ces propriétés attribuer des valeurs de propriété par défaut pour les fichiers dans ces dossiers, par exemple, le secret ou département.|  
|Stratégie de groupe|Un ensemble de règles et stratégies qui contrôle l’environnement de travail des utilisateurs et ordinateurs dans un environnement Active Directory.|  
|Près de classification de temps réel|La classification automatique qui est effectuée dès qu’un fichier est créée ou modifiée.|  
|Près de tâches de gestion de fichiers en temps réel|Les tâches de gestion sont effectuées peu après de fichiers (un fichier est créé ou modifié. Ces tâches sont déclenchés par la classification en temps réel proche.|  
|Unité d’organisation (UO)|Un conteneur Active Directory qui représente les structures logiques, hiérarchiques dans une organisation. Il est la plus petite étendue quelle stratégie de groupe de paramètres sont appliqués.|  
|Propriété sécurisée|Une propriété de classification, qui le runtime d’autorisation peut pour être une assertion valide sur la ressource à un certain point dans le temps. Dans le contrôle d’accès basée sur les revendications, une propriété sécurisée qui est affectée à une ressource est traitée comme une revendication de ressources.|  
|descripteur de sécurité|Une structure de données qui contient les informations de sécurité associées à une ressource sécurisable, telles que les listes de contrôle d’accès.|  
|Langage de définition de descripteur de sécurité|Une spécification qui décrit les informations contenues dans un descripteur de sécurité comme une chaîne de texte.|  
|Stratégie de mise en lots|Une stratégie d’accès centralisée qui n’est pas encore en vigueur.|  
|Liste de contrôle d’accès système (SACL)|Une liste de contrôle d’accès qui spécifie les types de tentatives d’accès par des tiers de confiance particulières pour laquelle les enregistrements d’audit doivent être générés.|  
|Revendication d’utilisateur|Attributs d’un utilisateur qui sont fournis dans le jeton de sécurité utilisateur. Par exemple : Département, entreprise, projet et habilitation.  Informations contenues dans le jeton d’utilisateur à partir des systèmes antérieurs à Windows Server 2012, tels que les groupes de sécurité dont l’utilisateur est la partie, peut également être considéré comme revendications d’utilisateur. Certaines revendications d’utilisateur sont fournies via Active Directory et d’autres sont calculées dynamiquement, comme si l’utilisateur connecté avec une carte à puce.|  
|Jeton d’utilisateur|Un objet de données qui identifie un utilisateur et les revendications d’utilisateur et les revendications de périphérique qui sont associées à cet utilisateur. Il est utilisé pour autoriser l’accès utilisateur aux ressources.|  
  
## <a name="see-also"></a>Voir aussi  
[Contrôle d’accès dynamique : Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  


