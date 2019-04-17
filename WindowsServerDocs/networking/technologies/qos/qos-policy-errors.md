---
title: Erreur de stratégie de QoS et les Messages d’événements
description: Cette rubrique fournit une liste des messages d’erreur et les événements de stratégie de qualité de Service (QoS) dans Windows Server2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e2e890a7947d4f7de09159d7de606c0542f45045
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-error-and-event-messages"></a>Erreur de stratégie de QoS et les Messages d’événements

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Voici les messages d’erreur et les événements associés à la stratégie de QoS.  
  
## <a name="informational-messages"></a>Messages d’information  

Voici une liste des messages d’information de stratégie de QoS.

|||  
|-|-|  
|**MessageId**|16500|  
|**Gravité**|Information|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|  
|**Langue**|Anglais|  
|**Message**|Actualisation des stratégies QoS ordinateur. Aucun changement détecté.|  
  
|||  
|-|-|  
|**MessageId**|16501|  
|**Gravité**|Information|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|  
|**Langue**|Anglais|  
|**Message**|Actualisation des stratégies QoS ordinateur. Modifications de stratégie détectées.|  
  
|||  
|-|-|  
|**MessageId**|16502|  
|**Gravité**|Information|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|  
|**Langue**|Anglais|  
|**Message**|Actualisation des stratégies QoS utilisateur. Aucun changement détecté.|  
  
|||  
|-|-|  
|**MessageId**|16503|  
|**Gravité**|Information|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|  
|**Langue**|Anglais|  
|**Message**|Actualisation des stratégies QoS utilisateur. Modifications de stratégie détectées.|  
  
|||  
|-|-|  
|**MessageId**|16504|  
|**Gravité**|Information|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|  
|**Langue**|Anglais|  
|**Message**|Les paramètres QoS avancés pour le niveau du débit TCP entrant a été actualisé. Valeur n’est pas spécifié par une stratégie de qualité de service. Valeur par défaut de l’ordinateur local sera appliquée.|  
  
|||  
|-|-|  
|**MessageId**|16505|  
|**Gravité**|Information|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|  
|**Langue**|Anglais|  
|**Message**|Les paramètres QoS avancés pour le niveau du débit TCP entrant a été actualisé. Valeur de paramètre est le niveau 0 (débit minimal).|  
  
|||  
|-|-|  
|**MessageId**|16506|  
|**Gravité**|Information|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|  
|**Langue**|Anglais|  
|**Message**|Les paramètres QoS avancés pour le niveau du débit TCP entrant a été actualisé. Valeur de paramètre est niveau 1.|  
  
|||  
|-|-|  
|**MessageId**|16507|  
|**Gravité**|Information|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|  
|**Langue**|Anglais|  
|**Message**|Les paramètres QoS avancés pour le niveau du débit TCP entrant a été actualisé. Valeur est de niveau 2.|  
  
|||  
|-|-|  
|**MessageId**|16508|  
|**Gravité**|Information|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|  
|**Langue**|Anglais|  
|**Message**|Les paramètres QoS avancés pour le niveau du débit TCP entrant a été actualisé. Valeur est de niveau 3 (débit maximal).|  
  
|||  
|-|-|  
|**MessageId**|16509|  
|**Gravité**|Information|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|  
|**Langue**|Anglais|  
|**Message**|Les paramètres QoS avancés pour le marquage DSCP remplacements actualisée avec succès. Valeur n’est pas spécifié. Applications peuvent définir des valeurs DSCP indépendamment des stratégies de QoS.|  
  
|||  
|-|-|  
|**MessageId**|16510|  
|**Gravité**|Information|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|  
|**Langue**|Anglais|  
|**Message**|Les paramètres QoS avancés pour le marquage DSCP remplacements actualisée avec succès. Demandes de marquage DSCP de application seront ignorés. Seules les stratégies de qualité de service peuvent définir des valeurs DSCP.|  
  
|||  
|-|-|  
|**MessageId**|16511|  
|**Gravité**|Information|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|  
|**Langue**|Anglais|  
|**Message**|Les paramètres QoS avancés pour le marquage DSCP remplacements actualisée avec succès. Applications peuvent définir des valeurs DSCP indépendamment des stratégies de QoS.|  
  
|||  
|-|-|  
|**MessageId**|16512|  
|**Gravité**|Information|  
|**SymbolicName**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|  
|**Langue**|Anglais|  
|**Message**|Application sélective des stratégies de QoS basée sur la catégorie de réseau de domaine a été désactivée. Stratégies de QoS seront appliqueront à toutes les interfaces réseau.|  
  
## <a name="warning-messages"></a>Messages d’avertissement

Voici une liste de messages d’avertissement de stratégie de QoS.

|||  
|-|-|  
|**MessageId**|16600|  
|**Gravité**|Avertissement|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_1|  
|**Langue**|Anglais|  
|**Message**|EQOS: *** Testing\ * \ * \ * [, avec une seule chaîne] «%2».|  
  
|||  
|-|-|  
|**MessageId**|16601|  
|**Gravité**|Avertissement|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_2|  
|**Langue**|Anglais|  
|**Message**|EQOS: *** Testing\ * \ * \ * [, avec deux chaînes, chaîne1 est] «%2» [, est chaîne2] «%3».|  
  
|||  
|-|-|  
|**MessageId**|16602|  
|**Gravité**|Avertissement|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|  
|**Langue**|Anglais|  
|**Message**|L’ordinateur stratégie QoS «%2» a un numéro de version non valide. Cette stratégie ne sera pas appliquée.|  
  
|||  
|-|-|  
|**MessageId**|16603|  
|**Gravité**|Avertissement|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|  
|**Langue**|Anglais|  
|**Message**|L’utilisateur stratégie QoS «%2» a un numéro de version non valide. Cette stratégie ne sera pas appliquée.|  
  
|||  
|-|-|  
|**MessageId**|16604|  
|**Gravité**|Avertissement|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|  
|**Langue**|Anglais|  
|**Message**|La stratégie QoS «%2» de l’ordinateur ne spécifie pas un taux de limitation de bande passante ou de valeur DSCP. Cette stratégie ne sera pas appliquée.|  
  
|||  
|-|-|  
|**MessageId**|16605|  
|**Gravité**|Avertissement|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|  
|**Langue**|Anglais|  
|**Message**|La stratégie QoS «%2» de l’utilisateur ne spécifie pas un taux de limitation de bande passante ou de valeur DSCP. Cette stratégie ne sera pas appliquée.|  
  
|||  
|-|-|  
|**MessageId**|16606|  
|**Gravité**|Avertissement|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|  
|**Langue**|Anglais|  
|**Message**|A dépassé le nombre maximal de stratégies de qualité de service l’ordinateur. La stratégie QoS «%2» et les stratégies de QoS ordinateur suivantes ne seront pas appliquées.|  
  
|||  
|-|-|  
|**MessageId**|16607|  
|**Gravité**|Avertissement|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|  
|**Langue**|Anglais|  
|**Message**|A dépassé le nombre maximal de stratégies de QoS utilisateur. La stratégie QoS «%2» et les stratégies de QoS utilisateur suivantes ne seront pas appliquées.|  
  
|||  
|-|-|  
|**MessageId**|16608|  
|**Gravité**|Avertissement|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|  
|**Langue**|Anglais|  
|**Message**|La stratégie QoS «%2» de l’ordinateur est potentiellement en conflit avec d’autres stratégies de qualité de service. Consultez la documentation de règles sur lequel la stratégie sera appliquée.|  
  
|||  
|-|-|  
|**MessageId**|16609|  
|**Gravité**|Avertissement|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|  
|**Langue**|Anglais|  
|**Message**|L’utilisateur stratégie QoS «%2» est potentiellement en conflit avec d’autres stratégies de qualité de service. Consultez la documentation de règles sur lequel la stratégie sera appliquée.|  
  
|||  
|-|-|  
|**MessageId**|16610|  
|**Gravité**|Avertissement|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|  
|**Langue**|Anglais|  
|**Message**|L’ordinateur stratégie QoS «%2» a été ignorée car le chemin d’accès de l’application ne peut pas être traité. Le chemin d’accès de l’application peut être non valide, contiennent une lettre de lecteur non valide ou contient un lecteur réseau mappé.|  
  
|||  
|-|-|  
|**MessageId**|16611|  
|**Gravité**|Avertissement|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|  
|**Langue**|Anglais|  
|**Message**|L’utilisateur stratégie QoS «%2» a été ignorée car le chemin d’accès de l’application ne peut pas être traité. Le chemin d’accès de l’application peut être non valide, contiennent une lettre de lecteur non valide ou contient un lecteur réseau mappé.|  
  
## <a name="error-messages"></a>Messages d’erreur  

Voici une liste des messages d’erreur de stratégie de QoS.

|||  
|-|-|  
|**MessageId**|16700|  
|**Gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|  
|**Langue**|Anglais|  
|**Message**|Actualisation des stratégies QoS ordinateur a échoué. Code d’erreur: «%2».|  
  
|||  
|-|-|  
|**MessageId**|16701|  
|**Gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|  
|**Langue**|Anglais|  
|**Message**|Actualisation des stratégies QoS utilisateur a échoué. Code d’erreur: «%2».|  
  
|||  
|-|-|  
|**MessageId**|16702|  
|**Gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|  
|**Langue**|Anglais|  
|**Message**|Qualité de service n’a pas pu ouvrir la clé racine au niveau de l’ordinateur pour les stratégies de qualité de service. Code d’erreur: «%2».|  
  
|||  
|-|-|  
|**MessageId**|16703|  
|**Gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|  
|**Langue**|Anglais|  
|**Message**|Qualité de service n’a pas pu ouvrir la clé racine au niveau de l’utilisateur pour les stratégies de qualité de service. Code d’erreur: «%2».|  
  
|||  
|-|-|  
|**MessageId**|16704|  
|**Gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|  
|**Langue**|Anglais|  
|**Message**|Une stratégie QoS ordinateur dépasse la longueur maximale autorisée pour le nom. La stratégie incriminée est répertoriée sous la clé de racine de la stratégie de QoS au niveau de l’ordinateur, avec l’index «%2».|  
  
|||  
|-|-|  
|**MessageId**|16705|  
|**Gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|  
|**Langue**|Anglais|  
|**Message**|Une stratégie QoS utilisateur dépasse la longueur maximale autorisée pour le nom. La stratégie incriminée est répertoriée sous la clé racine de la stratégie QoS utilisateur, avec l’index «%2».|  
  
|||  
|-|-|  
|**MessageId**|16706|  
|**Gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|  
|**Langue**|Anglais|  
|**Message**|Une stratégie QoS ordinateur a un nom de longueur nulle. La stratégie incriminée est répertoriée sous la clé de racine de la stratégie de QoS au niveau de l’ordinateur, avec l’index «%2».|  
  
|||  
|-|-|  
|**MessageId**|16707|  
|**Gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|  
|**Langue**|Anglais|  
|**Message**|Une stratégie QoS utilisateur a un nom de longueur nulle. La stratégie incriminée est répertoriée sous la clé racine de la stratégie QoS utilisateur, avec l’index «%2».|  
  
|||  
|-|-|  
|**MessageId**|16708|  
|**Gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|  
|**Langue**|Anglais|  
|**Message**|Qualité de service n’a pas pu ouvrir la sous-clé de Registre pour une stratégie de QoS de l’ordinateur. La stratégie est répertoriée sous la clé de racine de la stratégie de QoS au niveau de l’ordinateur, avec l’index «%2».|  
  
|||  
|-|-|  
|**MessageId**|16709|  
|**Gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|  
|**Langue**|Anglais|  
|**Message**|Qualité de service n’a pas pu ouvrir la sous-clé de Registre pour une stratégie de QoS de l’utilisateur. La stratégie est répertoriée sous la clé racine de la stratégie QoS utilisateur, avec l’index «%2».|  
  
|||  
|-|-|  
|**MessageId**|16710|  
|**Gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|  
|**Langue**|Anglais|  
|**Message**|Qualité de service n’a pas pu lire ou valider le champ «%2» pour l’ordinateur de la stratégie de QoS «%3».|  
  
|||  
|-|-|  
|**MessageId**|16711|  
|**Gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|  
|**Langue**|Anglais|  
|**Message**|Qualité de service n’a pas pu lire ou valider le champ «%2» pour l’utilisateur de la stratégie de QoS «%3».|  
  
|||  
|-|-|  
|**MessageId**|16712|  
|**Gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|  
|**Langue**|Anglais|  
|**Message**|Qualité de service n’a pas pu lire ou de définir le code d’erreur niveau entrant TCP débit: «%2».|  
  
|||  
|-|-|  
|**MessageId**|16713|  
|**Gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|  
|**Langue**|Anglais|  
|**Message**|Qualité de service n’a pas pu lire ou de définir le marquage DSCP remplace le paramètre, le code d’erreur: «%2».|  

Pour la rubrique suivante dans ce guide, voir [Forum aux Questions de la qualité de service stratégie](qos-policy-faq.md).

Pour la première rubrique de ce guide, voir [stratégie de qualité de Service (QoS)](qos-policy-top.md).
