---
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: Annexe A Dynamic Access Control Glossaire
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5508c3397039a1a70c07f1dc5f29e06bd02234a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357611"
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>Annexe A : Glossaire de la Access Control dynamique

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Voici la liste des termes et définitions inclus dans le scénario de Access Control dynamique.  
  
|Terme|Définition|  
|--------|--------------|  
|Classification automatique|Classification qui se produit en fonction des propriétés de classification qui sont déterminées par les règles de classification configurées par un administrateur.|  
|CAPID|ID de la stratégie d’accès centralisée. Cet ID fait référence à une stratégie d’accès centralisée spécifique et est utilisé pour référencer la stratégie à partir du descripteur de sécurité des fichiers et des dossiers.|  
|Règle d’accès central|Règle qui comprend une condition et une expression d’accès.|  
|Stratégie d’accès centralisée|Stratégies créées et hébergées dans Active Directory.|  
|Contrôle d’accès basé sur les revendications|Paradigme qui utilise des revendications pour prendre des décisions de contrôle d’accès aux ressources.|  
|Classification|Processus qui consiste à déterminer les propriétés de classification des ressources et à assigner ces propriétés aux métadonnées associées aux ressources. Voir aussi REF AutomaticClassification \h \\ * MERGEFORMAT classification automatique, REF InheritedClassification \h \\ @ no__t-2 MERGEFORMAT classification héritée et REF ManualClassification \h \\ @ no__t-4 MERGEFORMAT manuel classification.|  
|Revendication d’appareil|Revendication associée au système.  Avec les revendications d’utilisateur, il est inclus dans le jeton d’un utilisateur qui tente d’accéder à une ressource.|  
|Liste de contrôle d’accès discrétionnaire (DACL)|Liste de contrôle d’accès qui identifie les destinataires autorisés ou non à accéder à une ressource sécurisable. Elle peut être modifiée à la discrétion du propriétaire de la ressource.|  
|Propriété de ressource|Propriétés (telles que des étiquettes) qui décrivent un fichier et qui sont affectées à des fichiers à l’aide d’une classification automatique ou d’une classification manuelle. Par exemple : Sensibilité, projet et période de rétention.|  
|Outils de gestion de ressources pour serveur de fichiers|Fonctionnalité du système d’exploitation Windows Server qui assure la gestion des quotas de dossiers, le filtrage des fichiers, les rapports de stockage, la classification des fichiers et les travaux de gestion des fichiers sur un serveur de fichiers.|  
|Propriétés et étiquettes du dossier|Les propriétés et les étiquettes qui décrivent un dossier et sont affectées manuellement par les administrateurs et les propriétaires de dossiers. Ces propriétés attribuent des valeurs de propriété par défaut aux fichiers de ces dossiers, par exemple, le secret ou le service.|  
|Stratégie de groupe|Ensemble de règles et de stratégies qui contrôlent l’environnement de travail des utilisateurs et des ordinateurs dans un environnement de Active Directory.|  
|Classification en temps quasi réel|Classification automatique qui est exécutée peu après la création ou la modification d’un fichier.|  
|Tâches de gestion de fichiers en temps quasi réel|Tâches de gestion de fichiers qui sont effectuées peu après (un fichier a été créé ou modifié. Ces tâches sont déclenchées par la classification presque en temps réel.|  
|Unité d’organisation (UO)|Conteneur Active Directory qui représente les structures hiérarchiques et logiques au sein d’une organisation. Il s’agit de la plus petite étendue à laquelle les paramètres de stratégie de groupe sont appliqués.|  
|Propriété sécurisée|Une propriété de classification que le runtime d’autorisation peut approuver comme une assertion valide sur la ressource à un certain point dans le temps. Dans le contrôle d’accès basé sur les revendications, une propriété sécurisée qui est affectée à une ressource est traitée comme une revendication de ressource.|  
|Descripteur de sécurité|Structure de données qui contient des informations de sécurité associées à une ressource sécurisable, telles que des listes de contrôle d’accès.|  
|Langage de définition du descripteur de sécurité|Spécification qui décrit les informations contenues dans un descripteur de sécurité sous la forme d’une chaîne de texte.|  
|Stratégie de préproduction|Stratégie d’accès centralisée qui n’est pas encore en vigueur.|  
|Liste de contrôle d’accès système (SACL)|Liste de contrôle d’accès qui spécifie les types de tentatives d’accès par des approuvés particuliers pour lesquels des enregistrements d’audit doivent être générés.|  
|Revendication de l’utilisateur|Attributs d’un utilisateur qui sont fournis dans le jeton de sécurité de l’utilisateur. Par exemple : Service, société, projet et habilitation de sécurité.  Les informations du jeton utilisateur provenant des systèmes antérieurs à Windows Server 2012, comme les groupes de sécurité dont l’utilisateur fait partie, peuvent également être considérées comme des revendications d’utilisateur. Certaines revendications utilisateur sont fournies par le biais de Active Directory et d’autres sont calculées de manière dynamique, par exemple si l’utilisateur s’est connecté avec une carte à puce.|  
|Jeton de l’utilisateur|Objet de données qui identifie un utilisateur et les revendications de l’utilisateur et de l’appareil qui sont associées à cet utilisateur. Il est utilisé pour autoriser l’accès de l’utilisateur aux ressources.|  
  
## <a name="see-also"></a>Voir aussi  
[Contrôle d’accès dynamique : Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  


