---
title: "Résoudre les problèmes de stratégies de Restriction logicielle"
description: "Sécurité de Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-software-restriction-policies"></a>Résoudre les problèmes de stratégies de Restriction logicielle

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique décrit les problèmes courants et leurs solutions lors de la résolution des problèmes de stratégies de Restriction logicielle (SRP) à compter de Windows Server2008 et WindowsVista.

## <a name="introduction"></a>Introduction
Stratégies de Restriction logicielle (SRP) est la fonctionnalité de stratégie de groupe qui identifie les programmes logiciels s’exécutant sur des ordinateurs dans un domaine et contrôle la capacité de ces programmes à exécuter. Vous utilisez des stratégies de restriction logicielle pour créer une configuration fortement restreinte pour vos ordinateurs, dans laquelle vous uniquement spécifiquement les applications identifiées est autorisée à s’exécuter. Elles sont intégrées aux Services de domaine Microsoft Active Directory et la stratégie de groupe, mais peuvent également être configurés sur des ordinateurs autonomes. Pour plus d’informations sur les stratégies de restriction logicielle, consultez le [stratégies de Restriction logicielle](software-restriction-policies.md).

À partir de Windows Server 2008 R2 et Windows 7, Windows AppLocker peut servir à la place ou conjointement avec les stratégies de restriction logicielle pour une partie de votre stratégie de contrôle d’application.

### <a name="windows-cannot-open-a-program"></a>Windows ne peut pas ouvrir un programme
Les utilisateurs reçoivent un message indiquant «Impossible d’ouvrir ce programme car il a été empêché par une stratégie de restriction logicielle. Pour plus d’informations, ouvrez l’Observateur d’événements ou contactez votre administrateur système.» Ou, sur la ligne de commande, un message indiquant «le système ne peut pas exécuter le programme spécifié.»

**Cause:** le niveau de sécurité par défaut (ou une règle) a été créée afin que le programme logiciel est défini comme **non autorisé**, et par conséquent, il ne démarre pas.

**Solution:**, consultez le journal des événements pour une description détaillée du message. Le message du journal des événements indique quel programme logiciel est défini comme **non autorisé** et quelle règle est appliquée au programme.

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>Stratégies de restriction logicielle modifiée ne prennent pas effet
**Cause:** des stratégies de restriction logicielle qui sont spécifiés dans un domaine via la stratégie de groupe remplacent les paramètres de stratégie configurés localement. Cela implique qu’il existe un paramètre de stratégie du domaine qui remplace le paramètre de stratégie.

**Cause:** stratégie de groupe ne peut pas avoir actualisée ses paramètres de stratégie. Stratégie de groupe applique les modifications apportées aux paramètres de stratégie périodiquement; par conséquent, il est probable que les modifications de stratégie qui ont été apportées dans le répertoire n’ont pas encore été actualisées.

**Solutions:**

1.  L’ordinateur sur lequel vous modifiez les stratégies de restriction logicielle pour le réseau doit être en mesure de contacter un contrôleur de domaine. Vérifiez que l’ordinateur peut contacter un contrôleur de domaine.

2.  Actualiser la stratégie en vous connectant désactivés sur le réseau et puis reconnecter au réseau. Si une stratégie est appliquée par le biais de stratégie de groupe, la journalisation dans actualisera ces stratégies.

3.  Vous pouvez actualiser les paramètres de stratégie avec l’utilitaire de ligne de commande gpupdate ou fermer la session, puis connectez-vous à votre ordinateur. Pour de meilleurs résultats, exécutez gpupdate et puis déconnectez-vous et reconnectez-vous à votre ordinateur. En règle générale, les paramètres de sécurité sont actualisés toutes les 90minutes sur une station de travail ou un serveur et toutes les 5minutes sur un contrôleur de domaine. Les paramètres sont également actualisés toutes les 16heures, il existe des modifications ou non. Ce sont des paramètres configurables pour les intervalles d’actualisation peuvent être différents dans chaque domaine.

4.  Vérifiez quelles stratégies s’appliquent. Vérifiez les stratégies au niveau du domaine pour **aucun remplacement** paramètres.

5.  Les stratégies de restriction logicielle qui sont spécifiés dans un domaine via la stratégie de groupe remplacent les stratégies sont configurées localement. Utiliser l’outil de ligne de commande Gpresult pour déterminer l’effet net de la stratégie. Cela implique qu’il existe une stratégie sur le domaine qui remplace votre paramètre local.

6.  Si les paramètres de stratégie de restriction logicielle et AppLocker sont dans le même objet de stratégie de groupe, les paramètres d’AppLocker sont priorité sur Windows7, Windows Server2008R2 et versions ultérieures. Il est recommandé de placer les paramètres de stratégie de restriction logicielle et AppLocker dans différents objets de stratégie de groupe.

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>Après avoir ajouté une règle par le biais de stratégies de restriction logicielle, vous ne peut pas ouvrir une session sur votre ordinateur
**Cause:** votre ordinateur accède à plusieurs programmes et fichiers lorsqu’il démarre. Vous avez peut-être par inadvertance défini un de ces programmes ou les fichiers **non autorisé**. Étant donné que le programme ou le fichier ne peut pas accéder à l’ordinateur, il ne peut pas démarrer correctement.

**Solution:** démarrer l’ordinateur en Mode sans échec, connectez-vous en tant qu’administrateur local et modifiez les stratégies de restriction logicielle pour autoriser le programme ou le fichier à exécuter.

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>Un nouveau paramètre de stratégie s’applique pas à une extension de nom de fichier spécifique
**Cause:** l’extension de nom de fichier n’est pas dans la liste des types de fichiers pris en charge.

**Solution:** ajouter l’extension de nom de fichier à la liste des types de fichiers pris en charge par les stratégies de restriction logicielle.

Stratégies de restriction logicielle résoudre le problème du code inconnu ou non approuvé. Stratégies de restriction logicielle sont des paramètres de sécurité pour identifier les logiciels et de contrôler sa capacité à s’exécuter sur un ordinateur local, d’un site, le domaine ou l’unité d’organisation et peuvent être implémentées par le biais d’un objet de stratégie de groupe.

### <a name="a-default-rule-is-not-restricting-as-expected"></a>Une règle par défaut n’est pas limitation comme prévu
**Cause:** des règles qui sont appliquées dans un ordre particulier qui peut provoquer des règles par défaut être substituée par des règles spécifiques. Stratégies de restriction logicielle s’applique les règles dans l’ordre suivant (plus spécifique au général):

1.  Règles de hachage

2.  Règles de certificat

3.  Règles de chemin d’accès

4.  Règles de Zone Internet

5.  Règles par défaut

**Solution:** évaluer les règles de restreindre l’application et, le cas échéant, supprimez toutes sauf la règle par défaut.

### <a name="unable-to-discover-which-restrictions-are-applied"></a>Impossible de détecter les restrictions sont appliquées.
**Cause:** il n’existe aucune cause apparente pour le comportement inattendu et l’actualisation de la stratégie de groupe n’a pas résolu le problème enquête supplémentaire est nécessaire.

**Solutions:**

1.  Examiner le journal des événements système, le filtrage sur la source de "Stratégie de Restriction logicielle". Les entrées d’état explicitement la règle est implémentée pour chaque application.

2.  Activer la journalisation avancée. Voir [déterminer la liste autorisation / exclusion et l’inventaire des applications pour les stratégies de Restriction logicielle](software-restriction-policies.md) pour plus d’informations.


