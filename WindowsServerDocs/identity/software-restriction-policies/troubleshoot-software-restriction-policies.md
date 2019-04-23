---
title: Résoudre les problèmes liés aux stratégies de restriction logicielle
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fd53736-03e7-4bf9-ba90-d1212d93e19a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: abf592930f5347fa10e83c644671f68f9dc1a3f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872380"
---
# <a name="troubleshoot-software-restriction-policies"></a>Résoudre les problèmes liés aux stratégies de restriction logicielle

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit les problèmes courants et leurs solutions lors de la résolution des problèmes de stratégies de Restriction logicielle (SRP) à compter de Windows Server 2008 et Windows Vista.

## <a name="introduction"></a>Introduction
La fonctionnalité Stratégies de restriction logicielle est une fonctionnalité fondée sur les stratégies de groupe qui identifie les programmes logiciels s’exécutant sur les ordinateurs d’un domaine et qui contrôle la capacité de ces programmes à s’exécuter. Les stratégies de restriction logicielle peuvent aussi contribuer à créer une configuration fortement restreinte pour vos ordinateurs, dans laquelle seule l’exécution d’applications clairement identifiées est autorisée. Elles sont intégrées aux Services de domaine Microsoft Active Directory et stratégie de groupe, mais peuvent également être configurés sur des ordinateurs autonomes. Pour plus d’informations sur les stratégies de restriction logicielle, consultez le [stratégies de Restriction logicielle](software-restriction-policies.md).

Depuis Windows Server 2008 R2 et Windows 7, Windows AppLocker peut servir à la place d’ou de concert avec les stratégies de restriction logicielle pour une partie de votre stratégie de contrôle d’application.

### <a name="windows-cannot-open-a-program"></a>Windows ne peut pas ouvrir un programme
Les utilisateurs reçoivent un message indiquant que « Windows ne peut pas ouvrir ce programme, car elle a été empêchée par une stratégie de restriction logicielle. Pour plus d’informations, ouvrez l’Observateur d’événements ou contactez votre administrateur système. » Ou, sur la ligne de commande, un message indique que « le système ne peut pas exécuter le programme spécifié. »

**Cause :** Le niveau de sécurité par défaut (ou une règle) a été créée afin que le programme logiciel est défini en tant que **rejeté**, et par conséquent, il ne démarre pas.

**Solution :** Recherchez dans le journal des événements pour une description détaillée du message. Le message de journal des événements indique quel programme logiciel est défini en tant que **rejeté** et quelle règle est appliquée au programme.

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>Stratégies de restriction logicielle modifié ne prennent pas effet
**Cause :** Les stratégies de restriction logicielle qui sont spécifiés dans un domaine via la stratégie de groupe remplacent les paramètres de stratégie configurés localement. Cela peut impliquer qu’un paramètre de stratégie à partir du domaine qui substitue le paramètre de stratégie.

**Cause :** Stratégie de groupe n’ont ne peut-être pas actualisées de ses paramètres de stratégie. Stratégie de groupe applique les modifications apportées aux paramètres de stratégie régulièrement ; Par conséquent, il est probable que les modifications de stratégie qui ont été apportées dans le répertoire n’ont pas encore été actualisées.

**Solutions :**

1.  L’ordinateur sur lequel vous modifiez des stratégies de restriction logicielle pour le réseau doit être en mesure de contacter un contrôleur de domaine. Vérifiez que l’ordinateur peut contacter un contrôleur de domaine.

2.  Actualiser la stratégie en vous connectant sur le réseau et puis reconnecter au réseau. Si une stratégie est appliquée via la stratégie de groupe, se reconnectent actualisera ces stratégies.

3.  Vous pouvez actualiser les paramètres de stratégie avec l’utilitaire de ligne de commande gpupdate ou fermer la session, puis connectez-vous à votre ordinateur. Pour de meilleurs résultats, exécutez gpupdate et puis fermer la session à partir d’et reconnectez-vous à votre ordinateur. En règle générale, les paramètres de sécurité sont actualisés toutes les 90 minutes sur un serveur ou station de travail et toutes les 5 minutes sur un contrôleur de domaine. Les paramètres sont également actualisés toutes les 16 heures, qu'ils aient été modifiés ou non. Il s’agit de paramètres configurables pour les intervalles d’actualisation peuvent être différents dans chaque domaine.

4.  Vérifiez les stratégies qui s’appliquent. Vérifiez les stratégies de niveau domaine pour **aucun remplacement** paramètres.

5.  Les stratégies de restriction logicielle qui sont spécifiés dans un domaine via la stratégie de groupe remplacent toutes les stratégies qui sont configurés localement. Utiliser l’outil de ligne de commande Gpresult pour déterminer l’effet net de la stratégie. Cela peut impliquer qu’une stratégie à partir du domaine qui substitue le paramètre local.

6.  Si les paramètres de stratégie de restriction logicielle et AppLocker sont dans le même objet de stratégie de groupe, des paramètres d’AppLocker seront prioritaire sur Windows 7, Windows Server 2008 R2 et versions ultérieures. Il est recommandé de placer les paramètres de stratégie de restriction logicielle et AppLocker dans différents objets de stratégie de groupe.

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>Après avoir ajouté une règle via des stratégies de restriction logicielle, vous ne peut pas vous connecter à votre ordinateur
**Cause :** Votre ordinateur accède à de nombreux programmes et fichiers lorsqu’il démarre. Vous avez peut-être par inadvertance défini un de ces programmes ou les fichiers à **rejeté**. Étant donné que l’ordinateur ne peut pas accéder au programme ou un fichier, il ne peut pas démarrer correctement.

**Solution :** Démarrez l’ordinateur en Mode sans échec, connectez-vous en tant qu’administrateur local, puis modifiez les stratégies de restriction logicielle pour permettre au programme ou un fichier à exécuter.

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>Un nouveau paramètre de stratégie ne s’applique pas à une extension de nom de fichier spécifique
**Cause :** L’extension de nom de fichier n’est pas dans la liste des types de fichiers pris en charge.

**Solution :** Ajoutez l’extension de nom de fichier à la liste des types de fichiers pris en charge par les stratégies de restriction logicielle.

Stratégies de restriction logicielle résoudre le problème du code inconnu ou non approuvé. Stratégies de restriction logicielle sont des paramètres de sécurité pour identifier les logiciels et de contrôler sa capacité à exécuter sur un ordinateur local, dans un site, un domaine ou une unité d’organisation et peuvent être implémentées via un objet de stratégie de groupe.

### <a name="a-default-rule-is-not-restricting-as-expected"></a>Une règle par défaut n’est pas restriction comme prévu
**Cause :** Règles qui sont appliquées dans un ordre particulier, ce qui peut entraîner des règles par défaut être substituée par des règles spécifiques. Stratégies de restriction logicielle applique des règles dans l’ordre suivant (plus spécifique à général) :

1.  Règles de hachage

2.  Règles de certificat

3.  Règles de chemins d’accès

4.  Règles de Zone Internet

5.  Règles par défaut

**Solution :** Évaluer les règles de restreindre l’application et, si nécessaire, supprimez toutes les valeurs sauf la règle par défaut.

### <a name="unable-to-discover-which-restrictions-are-applied"></a>Impossible de découvrir les restrictions sont appliquées
**Cause :** Il n’existe aucune cause apparente pour un comportement inattendu et actualisation de stratégie de groupe n’a pas résolu le problème pour un examen supplémentaire est nécessaire.

**Solutions :**

1.  Examiner le journal des événements système, le filtrage sur la source de la « Stratégie de Restriction logicielle ». Les entrées de déclarer explicitement la règle qui est implémentée pour chaque application.

2.  Activer la journalisation avancée. Consultez [déterminer la liste Autoriser-Deny et inventaire des applications pour les stratégies de Restriction logicielle](software-restriction-policies.md) pour plus d’informations.


