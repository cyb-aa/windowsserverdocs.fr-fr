---
title: Messages d’erreur et d’événement de stratégie QoS
description: Cette rubrique fournit une liste de messages d’erreur et d’événement pour la stratégie de qualité de service (QoS) dans Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 935bda5ab47f3e9a362c81a8aeb99ebf22095725
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405322"
---
# <a name="qos-policy-error-and-event-messages"></a>Messages d’erreur et d’événement de stratégie QoS

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Voici les messages d’erreur et d’événement associés à la stratégie QoS.  
  
## <a name="informational-messages"></a>Messages d’information  

La liste suivante répertorie les messages d’information sur la stratégie QoS.

|||  
|-|-|  
|**ID**|16500|  
|**Va**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|  
|**Langue**|Anglais|  
|**Message**|Stratégies QoS de l’ordinateur actualisées. Aucune modification détectée.|  
  
|||  
|-|-|  
|**ID**|16501|  
|**Va**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|  
|**Langue**|Anglais|  
|**Message**|Stratégies QoS de l’ordinateur actualisées. Modifications de stratégie détectées.|  
  
|||  
|-|-|  
|**ID**|16502|  
|**Va**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|  
|**Langue**|Anglais|  
|**Message**|Stratégies QoS utilisateur actualisées avec succès. Aucune modification détectée.|  
  
|||  
|-|-|  
|**ID**|16503|  
|**Va**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|  
|**Langue**|Anglais|  
|**Message**|Stratégies QoS utilisateur actualisées avec succès. Modifications de stratégie détectées.|  
  
|||  
|-|-|  
|**ID**|16504|  
|**Va**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|  
|**Langue**|Anglais|  
|**Message**|Le paramètre de QoS avancé pour le niveau de débit TCP entrant a été actualisé avec succès. La valeur du paramètre n’est pas spécifiée par une stratégie QoS. La valeur par défaut de l’ordinateur local sera appliquée.|  
  
|||  
|-|-|  
|**ID**|16505|  
|**Va**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|  
|**Langue**|Anglais|  
|**Message**|Le paramètre de QoS avancé pour le niveau de débit TCP entrant a été actualisé avec succès. La valeur du paramètre est le niveau 0 (débit minimal).|  
  
|||  
|-|-|  
|**ID**|16506|  
|**Va**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|  
|**Langue**|Anglais|  
|**Message**|Le paramètre de QoS avancé pour le niveau de débit TCP entrant a été actualisé avec succès. La valeur du paramètre est de niveau 1.|  
  
|||  
|-|-|  
|**ID**|16507|  
|**Va**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|  
|**Langue**|Anglais|  
|**Message**|Le paramètre de QoS avancé pour le niveau de débit TCP entrant a été actualisé avec succès. La valeur du paramètre est de niveau 2.|  
  
|||  
|-|-|  
|**ID**|16508|  
|**Va**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|  
|**Langue**|Anglais|  
|**Message**|Le paramètre de QoS avancé pour le niveau de débit TCP entrant a été actualisé avec succès. La valeur du paramètre est de niveau 3 (débit maximal).|  
  
|||  
|-|-|  
|**ID**|16509|  
|**Va**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|  
|**Langue**|Anglais|  
|**Message**|Le paramètre de QoS avancé pour les remplacements de marquage DSCP a été actualisé avec succès. La valeur du paramètre n’est pas spécifiée. Les applications peuvent définir des valeurs DSCP indépendamment des stratégies QoS.|  
  
|||  
|-|-|  
|**ID**|16510|  
|**Va**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|  
|**Langue**|Anglais|  
|**Message**|Le paramètre de QoS avancé pour les remplacements de marquage DSCP a été actualisé avec succès. Les demandes de marquage DSCP des applications seront ignorées. Seules les stratégies de QoS peuvent définir des valeurs DSCP.|  
  
|||  
|-|-|  
|**ID**|16511|  
|**Va**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|  
|**Langue**|Anglais|  
|**Message**|Le paramètre de QoS avancé pour les remplacements de marquage DSCP a été actualisé avec succès. Les applications peuvent définir des valeurs DSCP indépendamment des stratégies QoS.|  
  
|||  
|-|-|  
|**ID**|16512|  
|**Va**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|  
|**Langue**|Anglais|  
|**Message**|L’application sélective des stratégies de QoS basée sur la catégorie du réseau de domaine a été désactivée. Les stratégies de qualité de service seront appliquées à toutes les interfaces réseau.|  
  
## <a name="warning-messages"></a>Messages d’avertissement

La liste suivante répertorie les messages d’avertissement de stratégie QoS.

|||  
|-|-|  
|**ID**|16600|  
|**Va**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_1|  
|**Langue**|Anglais|  
|**Message**|EQOS : * * * test de @ no__t-0 @ no__t-1 @ no__t-2 [, avec une chaîne] "% 2".|  
  
|||  
|-|-|  
|**ID**|16601|  
|**Va**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_2|  
|**Langue**|Anglais|  
|**Message**|EQOS : * * * test de @ no__t-0 @ no__t-1 @ no__t-2 [, avec deux chaînes, chaîne1 est] « % 2 » [, chaîne2 est] « % 3 ».|  
  
|||  
|-|-|  
|**ID**|16602|  
|**Va**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|  
|**Langue**|Anglais|  
|**Message**|Le numéro de version de la stratégie QoS de l’ordinateur « % 2 » n’est pas valide. Cette stratégie ne sera pas appliquée.|  
  
|||  
|-|-|  
|**ID**|16603|  
|**Va**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|  
|**Langue**|Anglais|  
|**Message**|Le numéro de version de la stratégie QoS utilisateur « % 2 » n’est pas valide. Cette stratégie ne sera pas appliquée.|  
  
|||  
|-|-|  
|**ID**|16604|  
|**Va**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|  
|**Langue**|Anglais|  
|**Message**|La stratégie QoS de l’ordinateur « % 2 » ne spécifie pas de valeur DSCP ni de taux d’accélération. Cette stratégie ne sera pas appliquée.|  
  
|||  
|-|-|  
|**ID**|16605|  
|**Va**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|  
|**Langue**|Anglais|  
|**Message**|La stratégie de qualité de service utilisateur « % 2 » ne spécifie pas de valeur DSCP ni de taux d’accélération. Cette stratégie ne sera pas appliquée.|  
  
|||  
|-|-|  
|**ID**|16606|  
|**Va**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|  
|**Langue**|Anglais|  
|**Message**|A dépassé le nombre maximal de stratégies QoS ordinateur. La stratégie QoS « % 2 » et les stratégies QoS ordinateur ultérieures ne seront pas appliquées.|  
  
|||  
|-|-|  
|**ID**|16607|  
|**Va**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|  
|**Langue**|Anglais|  
|**Message**|A dépassé le nombre maximal de stratégies QoS utilisateur. La stratégie QoS « % 2 » et les stratégies QoS utilisateur ultérieures ne seront pas appliquées.|  
  
|||  
|-|-|  
|**ID**|16608|  
|**Va**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|  
|**Langue**|Anglais|  
|**Message**|La stratégie QoS de l’ordinateur « % 2 » peut être en conflit avec d’autres stratégies de QoS. Consultez la documentation pour connaître les règles relatives à la stratégie qui sera appliquée.|  
  
|||  
|-|-|  
|**ID**|16609|  
|**Va**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|  
|**Langue**|Anglais|  
|**Message**|La stratégie QoS utilisateur « % 2 » peut être en conflit avec d’autres stratégies de QoS. Consultez la documentation pour connaître les règles relatives à la stratégie qui sera appliquée.|  
  
|||  
|-|-|  
|**ID**|16610|  
|**Va**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|  
|**Langue**|Anglais|  
|**Message**|La stratégie QoS de l’ordinateur « % 2 » a été ignorée, car le chemin d’accès de l’application ne peut pas être traité. Le chemin d’accès de l’application n’est peut-être pas valide, contient une lettre de lecteur non valide ou contient un lecteur mappé sur le réseau.|  
  
|||  
|-|-|  
|**ID**|16611|  
|**Va**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|  
|**Langue**|Anglais|  
|**Message**|La stratégie QoS utilisateur « % 2 » a été ignorée, car le chemin d’accès de l’application ne peut pas être traité. Le chemin d’accès de l’application n’est peut-être pas valide, contient une lettre de lecteur non valide ou contient un lecteur mappé sur le réseau.|  
  
## <a name="error-messages"></a>Messages d'erreur  

Voici une liste des messages d’erreur de stratégie QoS.

|||  
|-|-|  
|**ID**|16700|  
|**Va**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|  
|**Langue**|Anglais|  
|**Message**|L’actualisation des stratégies QoS ordinateur a échoué. Code d’erreur : « % 2 ».|  
  
|||  
|-|-|  
|**ID**|16701|  
|**Va**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|  
|**Langue**|Anglais|  
|**Message**|L’actualisation des stratégies QoS utilisateur a échoué. Code d’erreur : « % 2 ».|  
  
|||  
|-|-|  
|**ID**|16702|  
|**Va**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|  
|**Langue**|Anglais|  
|**Message**|QoS n’a pas pu ouvrir la clé racine au niveau de l’ordinateur pour les stratégies QoS. Code d’erreur : « % 2 ».|  
  
|||  
|-|-|  
|**ID**|16703|  
|**Va**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|  
|**Langue**|Anglais|  
|**Message**|QoS n’a pas pu ouvrir la clé racine au niveau de l’utilisateur pour les stratégies QoS. Code d’erreur : « % 2 ».|  
  
|||  
|-|-|  
|**ID**|16704|  
|**Va**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|  
|**Langue**|Anglais|  
|**Message**|Une stratégie QoS ordinateur dépasse la longueur maximale autorisée pour le nom. La stratégie incriminée est indiquée sous la clé racine de la stratégie QoS au niveau de l’ordinateur, avec l’index « % 2 ».|  
  
|||  
|-|-|  
|**ID**|16705|  
|**Va**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|  
|**Langue**|Anglais|  
|**Message**|Une stratégie QoS utilisateur dépasse la longueur maximale autorisée pour le nom. La stratégie incriminée est indiquée sous la clé racine de la stratégie QoS au niveau de l’utilisateur, avec l’index « % 2 ».|  
  
|||  
|-|-|  
|**ID**|16706|  
|**Va**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|  
|**Langue**|Anglais|  
|**Message**|Une stratégie QoS de l’ordinateur a un nom de longueur zéro. La stratégie incriminée est indiquée sous la clé racine de la stratégie QoS au niveau de l’ordinateur, avec l’index « % 2 ».|  
  
|||  
|-|-|  
|**ID**|16707|  
|**Va**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|  
|**Langue**|Anglais|  
|**Message**|Une stratégie QoS utilisateur a un nom de longueur zéro. La stratégie incriminée est indiquée sous la clé racine de la stratégie QoS au niveau de l’utilisateur, avec l’index « % 2 ».|  
  
|||  
|-|-|  
|**ID**|16708|  
|**Va**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|  
|**Langue**|Anglais|  
|**Message**|QoS n’a pas pu ouvrir la sous-clé de Registre pour une stratégie QoS ordinateur. La stratégie est indiquée sous la clé racine de la stratégie QoS au niveau de l’ordinateur, avec l’index « % 2 ».|  
  
|||  
|-|-|  
|**ID**|16709|  
|**Va**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|  
|**Langue**|Anglais|  
|**Message**|QoS n’a pas pu ouvrir la sous-clé de Registre pour une stratégie QoS utilisateur. La stratégie est indiquée sous la clé racine de la stratégie QoS au niveau de l’utilisateur, avec l’index « % 2 ».|  
  
|||  
|-|-|  
|**ID**|16710|  
|**Va**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|  
|**Langue**|Anglais|  
|**Message**|QoS n’a pas pu lire ou valider le champ « % 2 » pour la stratégie QoS ordinateur « % 3 ».|  
  
|||  
|-|-|  
|**ID**|16711|  
|**Va**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|  
|**Langue**|Anglais|  
|**Message**|QoS n’a pas pu lire ou valider le champ « % 2 » pour la stratégie QoS utilisateur « % 3 ».|  
  
|||  
|-|-|  
|**ID**|16712|  
|**Va**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|  
|**Langue**|Anglais|  
|**Message**|QoS n’a pas pu lire ou définir le niveau de débit TCP entrant, code d’erreur : « % 2 ».|  
  
|||  
|-|-|  
|**ID**|16713|  
|**Va**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|  
|**Langue**|Anglais|  
|**Message**|QoS n’a pas pu lire ou définir le paramètre de remplacement de marquage DSCP, code d’erreur : « % 2 ».|  

Pour la rubrique suivante de ce guide, consultez [Forum aux questions](qos-policy-faq.md)sur la stratégie de QoS.

Pour obtenir la première rubrique de ce guide, consultez [stratégie de qualité de service (QoS)](qos-policy-top.md).
