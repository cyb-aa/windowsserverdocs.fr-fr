---
title: Erreur de stratégie de QoS et les Messages d’événement
description: Cette rubrique fournit une liste de messages d’erreur et les événements de stratégie de qualité de Service (QoS) dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 774d9473beed1da861c6827357710133aeaca15e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824050"
---
# <a name="qos-policy-error-and-event-messages"></a>Erreur de stratégie de QoS et les Messages d’événement

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Voici les messages d’erreur et les événements qui sont associés à la stratégie QoS.  
  
## <a name="informational-messages"></a>Messages d’information  

Voici une liste de messages d’information de la stratégie de QoS.

|||  
|-|-|  
|**MessageId**|16500|  
|**Niveau de gravité**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|  
|**Langue**|Anglais|  
|**Message**|Actualisation des stratégies QoS ordinateur. Aucune modification détectée.|  
  
|||  
|-|-|  
|**MessageId**|16501|  
|**Niveau de gravité**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|  
|**Langue**|Anglais|  
|**Message**|Actualisation des stratégies QoS ordinateur. Modifications de stratégie détectées.|  
  
|||  
|-|-|  
|**MessageId**|16502|  
|**Niveau de gravité**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|  
|**Langue**|Anglais|  
|**Message**|Actualisation des stratégies QoS utilisateur. Aucune modification détectée.|  
  
|||  
|-|-|  
|**MessageId**|16503|  
|**Niveau de gravité**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|  
|**Langue**|Anglais|  
|**Message**|Actualisation des stratégies QoS utilisateur. Modifications de stratégie détectées.|  
  
|||  
|-|-|  
|**MessageId**|16504|  
|**Niveau de gravité**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|  
|**Langue**|Anglais|  
|**Message**|Les paramètres avancés de QoS pour le niveau du débit TCP entrant a été actualisé avec succès. Définition de valeur n’est pas spécifiée par une stratégie de QoS. Valeur par défaut de l’ordinateur local sera appliquée.|  
  
|||  
|-|-|  
|**MessageId**|16505|  
|**Niveau de gravité**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|  
|**Langue**|Anglais|  
|**Message**|Les paramètres avancés de QoS pour le niveau du débit TCP entrant a été actualisé avec succès. Valeur de paramètre est le niveau 0 (débit minimal).|  
  
|||  
|-|-|  
|**MessageId**|16506|  
|**Niveau de gravité**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|  
|**Langue**|Anglais|  
|**Message**|Les paramètres avancés de QoS pour le niveau du débit TCP entrant a été actualisé avec succès. Valeur de paramètre est niveau 1.|  
  
|||  
|-|-|  
|**MessageId**|16507|  
|**Niveau de gravité**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|  
|**Langue**|Anglais|  
|**Message**|Les paramètres avancés de QoS pour le niveau du débit TCP entrant a été actualisé avec succès. Valeur de paramètre est de niveau 2.|  
  
|||  
|-|-|  
|**MessageId**|16508|  
|**Niveau de gravité**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|  
|**Langue**|Anglais|  
|**Message**|Les paramètres avancés de QoS pour le niveau du débit TCP entrant a été actualisé avec succès. Valeur de paramètre est le niveau 3 (débit maximal).|  
  
|||  
|-|-|  
|**MessageId**|16509|  
|**Niveau de gravité**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|  
|**Langue**|Anglais|  
|**Message**|Les paramètres avancés de QoS pour le marquage DSCP remplacements ont été actualisées. Définition de valeur n’est pas spécifiée. Applications peuvent définir des valeurs DSCP indépendamment des stratégies QoS.|  
  
|||  
|-|-|  
|**MessageId**|16510|  
|**Niveau de gravité**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|  
|**Langue**|Anglais|  
|**Message**|Les paramètres avancés de QoS pour le marquage DSCP remplacements ont été actualisées. Les demandes de marquage DSCP de application seront ignorés. Seules les stratégies QoS peuvent définir des valeurs DSCP.|  
  
|||  
|-|-|  
|**MessageId**|16511|  
|**Niveau de gravité**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|  
|**Langue**|Anglais|  
|**Message**|Les paramètres avancés de QoS pour le marquage DSCP remplacements ont été actualisées. Applications peuvent définir des valeurs DSCP indépendamment des stratégies QoS.|  
  
|||  
|-|-|  
|**MessageId**|16512|  
|**Niveau de gravité**|Informationnel|  
|**SymbolicName**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|  
|**Langue**|Anglais|  
|**Message**|Application sélective des stratégies de QoS basée sur la catégorie de réseau de domaine a été désactivée. Stratégies de QoS sont appliquées à toutes les interfaces réseau.|  
  
## <a name="warning-messages"></a>Messages d’avertissement

Voici une liste de messages d’avertissement de stratégie de QoS.

|||  
|-|-|  
|**MessageId**|16600|  
|**Niveau de gravité**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_1|  
|**Langue**|Anglais|  
|**Message**|EQOS : *** test\*\*\*[, avec une seule chaîne] « %2 ».|  
  
|||  
|-|-|  
|**MessageId**|16601|  
|**Niveau de gravité**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_2|  
|**Langue**|Anglais|  
|**Message**|EQOS : *** test\*\*\*[, avec deux chaînes, string1 est] « %2 » [, string2 est] « %3 ».|  
  
|||  
|-|-|  
|**MessageId**|16602|  
|**Niveau de gravité**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|  
|**Langue**|Anglais|  
|**Message**|L’ordinateur « %2 » de la stratégie QoS a un numéro de version non valide. Cette stratégie ne sera pas appliquée.|  
  
|||  
|-|-|  
|**MessageId**|16603|  
|**Niveau de gravité**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|  
|**Langue**|Anglais|  
|**Message**|L’utilisateur de la stratégie de QoS « %2 » a un numéro de version non valide. Cette stratégie ne sera pas appliquée.|  
  
|||  
|-|-|  
|**MessageId**|16604|  
|**Niveau de gravité**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|  
|**Langue**|Anglais|  
|**Message**|L’ordinateur « %2 » de la stratégie QoS ne spécifie pas un taux de valeur ou de limitation DSCP. Cette stratégie ne sera pas appliquée.|  
  
|||  
|-|-|  
|**MessageId**|16605|  
|**Niveau de gravité**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|  
|**Langue**|Anglais|  
|**Message**|La stratégie de QoS « %2 » de l’utilisateur ne spécifie pas un taux de valeur ou de limitation DSCP. Cette stratégie ne sera pas appliquée.|  
  
|||  
|-|-|  
|**MessageId**|16606|  
|**Niveau de gravité**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|  
|**Langue**|Anglais|  
|**Message**|A dépassé le nombre maximal de stratégies QoS ordinateur. La stratégie QoS de « %2 » et les stratégies QoS ordinateur suivantes ne seront pas appliquées.|  
  
|||  
|-|-|  
|**MessageId**|16607|  
|**Niveau de gravité**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|  
|**Langue**|Anglais|  
|**Message**|A dépassé le nombre maximal de stratégies QoS utilisateur. La stratégie QoS de « %2 » et les stratégies QoS utilisateur suivantes ne seront pas appliquées.|  
  
|||  
|-|-|  
|**MessageId**|16608|  
|**Niveau de gravité**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|  
|**Langue**|Anglais|  
|**Message**|L’ordinateur « %2 » de la stratégie QoS est potentiellement en conflit avec d’autres stratégies de qualité de service. Consultez la documentation sur les règles sur lequel la stratégie est appliquée.|  
  
|||  
|-|-|  
|**MessageId**|16609|  
|**Niveau de gravité**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|  
|**Langue**|Anglais|  
|**Message**|La stratégie de QoS « %2 » de l’utilisateur est potentiellement en conflit avec d’autres stratégies de qualité de service. Consultez la documentation sur les règles sur lequel la stratégie est appliquée.|  
  
|||  
|-|-|  
|**MessageId**|16610|  
|**Niveau de gravité**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|  
|**Langue**|Anglais|  
|**Message**|La stratégie de QoS « %2 » de l’ordinateur a été ignorée, car le chemin d’accès de l’application ne peut pas être traité. Le chemin d’accès de l’application peut être non valide, contenir une lettre de lecteur non valide ou contient un lecteur réseau mappé.|  
  
|||  
|-|-|  
|**MessageId**|16611|  
|**Niveau de gravité**|Warning|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|  
|**Langue**|Anglais|  
|**Message**|L’utilisateur de la stratégie de QoS « %2 » a été ignorée, car le chemin d’accès de l’application ne peut pas être traité. Le chemin d’accès de l’application peut être non valide, contenir une lettre de lecteur non valide ou contient un lecteur réseau mappé.|  
  
## <a name="error-messages"></a>Messages d'erreur  

Voici une liste des messages d’erreur de stratégie de QoS.

|||  
|-|-|  
|**MessageId**|16700|  
|**Niveau de gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|  
|**Langue**|Anglais|  
|**Message**|Actualisation des stratégies QoS ordinateur a échoué. Code d’erreur : « %2 ».|  
  
|||  
|-|-|  
|**MessageId**|16701|  
|**Niveau de gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|  
|**Langue**|Anglais|  
|**Message**|Actualisation des stratégies QoS utilisateur a échoué. Code d’erreur : « %2 ».|  
  
|||  
|-|-|  
|**MessageId**|16702|  
|**Niveau de gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|  
|**Langue**|Anglais|  
|**Message**|Échec de la qualité de service ouvrir la clé racine de niveau de l’ordinateur pour les stratégies de QoS. Code d’erreur : « %2 ».|  
  
|||  
|-|-|  
|**MessageId**|16703|  
|**Niveau de gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|  
|**Langue**|Anglais|  
|**Message**|Échec de la qualité de service ouvrir la clé racine de niveau utilisateur pour les stratégies de QoS. Code d’erreur : « %2 ».|  
  
|||  
|-|-|  
|**MessageId**|16704|  
|**Niveau de gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|  
|**Langue**|Anglais|  
|**Message**|Une stratégie QoS ordinateur dépasse la longueur maximale autorisée pour le nom. La stratégie incriminée est répertoriée sous la clé de racine de la stratégie de QoS au niveau de l’ordinateur, avec l’index « %2 ».|  
  
|||  
|-|-|  
|**MessageId**|16705|  
|**Niveau de gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|  
|**Langue**|Anglais|  
|**Message**|Une stratégie QoS utilisateur dépasse la longueur maximale autorisée pour le nom. La stratégie incriminée est répertoriée sous la clé de racine de la stratégie de QoS au niveau de l’utilisateur, avec l’index « %2 ».|  
  
|||  
|-|-|  
|**MessageId**|16706|  
|**Niveau de gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|  
|**Langue**|Anglais|  
|**Message**|Une stratégie QoS ordinateur a un nom de longueur nulle. La stratégie incriminée est répertoriée sous la clé de racine de la stratégie de QoS au niveau de l’ordinateur, avec l’index « %2 ».|  
  
|||  
|-|-|  
|**MessageId**|16707|  
|**Niveau de gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|  
|**Langue**|Anglais|  
|**Message**|Une stratégie QoS utilisateur a un nom de longueur nulle. La stratégie incriminée est répertoriée sous la clé de racine de la stratégie de QoS au niveau de l’utilisateur, avec l’index « %2 ».|  
  
|||  
|-|-|  
|**MessageId**|16708|  
|**Niveau de gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|  
|**Langue**|Anglais|  
|**Message**|Échec de la qualité de service ouvrir la sous-clé de Registre pour une stratégie QoS de l’ordinateur. La stratégie est répertoriée sous la clé de racine de la stratégie de QoS au niveau de l’ordinateur, avec l’index « %2 ».|  
  
|||  
|-|-|  
|**MessageId**|16709|  
|**Niveau de gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|  
|**Langue**|Anglais|  
|**Message**|Échec de la qualité de service ouvrir la sous-clé de Registre pour une stratégie QoS de l’utilisateur. La stratégie est répertoriée sous la clé de racine de la stratégie de QoS au niveau de l’utilisateur, avec l’index « %2 ».|  
  
|||  
|-|-|  
|**MessageId**|16710|  
|**Niveau de gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|  
|**Langue**|Anglais|  
|**Message**|Échec de la qualité de service lire ou de valider le champ « %2 » pour l’ordinateur « %3 » de la stratégie QoS.|  
  
|||  
|-|-|  
|**MessageId**|16711|  
|**Niveau de gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|  
|**Langue**|Anglais|  
|**Message**|Échec de la qualité de service lire ou de valider le champ « %2 » pour l’utilisateur « %3 » de la stratégie QoS.|  
  
|||  
|-|-|  
|**MessageId**|16712|  
|**Niveau de gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|  
|**Langue**|Anglais|  
|**Message**|Échec de la qualité de service lire ou définir le code d’erreur au niveau, entrant TCP débit : « %2 ».|  
  
|||  
|-|-|  
|**MessageId**|16713|  
|**Niveau de gravité**|Erreur|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|  
|**Langue**|Anglais|  
|**Message**|Qualité de service n’a pas pu lire ou définir le marquage DSCP remplacer le paramétrage, code d’erreur : « %2 ».|  

Pour la rubrique suivante de ce guide, consultez [Forum aux Questions de qualité de service de stratégie](qos-policy-faq.md).

Pour la première rubrique de ce guide, consultez [stratégie de qualité de Service (QoS)](qos-policy-top.md).
