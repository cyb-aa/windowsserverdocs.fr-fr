---
title: Résoudre les problèmes liés aux stratégies de restriction logicielle
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8dff3e1542afcc3cba3645b6834981bd6ed33f58
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407164"
---
# <a name="troubleshoot-software-restriction-policies"></a>Résoudre les problèmes liés aux stratégies de restriction logicielle

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit les problèmes courants et leurs solutions lors du dépannage des stratégies de restriction logicielle (SRP) à partir de Windows Server 2008 et Windows Vista.

## <a name="introduction"></a>Présentation
La fonctionnalité Stratégies de restriction logicielle est une fonctionnalité fondée sur les stratégies de groupe qui identifie les programmes logiciels s’exécutant sur les ordinateurs d’un domaine et qui contrôle la capacité de ces programmes à s’exécuter. Les stratégies de restriction logicielle peuvent aussi contribuer à créer une configuration fortement restreinte pour vos ordinateurs, dans laquelle seule l’exécution d’applications clairement identifiées est autorisée. Celles-ci sont intégrées à Microsoft Active Directory Domain Services et stratégie de groupe mais peuvent également être configurées sur des ordinateurs autonomes. Pour plus d’informations sur les SRP, consultez [stratégies de restriction logicielle](software-restriction-policies.md).

À compter de Windows Server 2008 R2 et Windows 7, il est possible d’utiliser Windows AppLocker à la place ou en collaboration avec les stratégies de restriction logicielle pour une partie de votre stratégie de contrôle des applications.

### <a name="windows-cannot-open-a-program"></a>Windows ne peut pas ouvrir un programme
Les utilisateurs reçoivent un message indiquant que «Windows ne peut pas ouvrir ce programme, car il a été empêché par une stratégie de restriction logicielle. Pour plus d’informations, ouvrez observateur d’événements ou contactez votre administrateur système.» Ou, sur la ligne de commande, un message indique « le système ne peut pas exécuter le programme spécifié ».

**Cause** : Le niveau de sécurité par défaut (ou une règle) a été créé afin que le programme logiciel soit défini sur non **autorisé**, et par conséquent, il ne démarrera pas.

**Solution :** Recherchez dans le journal des événements une description détaillée du message. Le message du journal des événements indique le programme logiciel défini sur non **autorisé** et la règle appliquée au programme.

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>Les stratégies de restriction logicielle modifiées ne sont pas prises en compte
**Cause** : Les stratégies de restriction logicielle qui sont spécifiées dans un domaine via stratégie de groupe remplacent les paramètres de stratégie configurés localement. Cela peut impliquer un paramètre de stratégie du domaine qui remplace votre paramètre de stratégie.

**Cause** : Stratégie de groupe n’avez peut-être pas actualisé ses paramètres de stratégie. Stratégie de groupe applique périodiquement les modifications apportées aux paramètres de stratégie. par conséquent, il est probable que les modifications apportées à la stratégie dans le répertoire n’ont pas encore été actualisées.

**Elles**

1.  L’ordinateur sur lequel vous modifiez les stratégies de restriction logicielle pour le réseau doit être en mesure de contacter un contrôleur de domaine. Vérifiez que l’ordinateur peut contacter un contrôleur de domaine.

2.  Actualisez la stratégie en vous déconnectant du réseau, puis en vous reconnectant au réseau. Si une stratégie est appliquée via stratégie de groupe, la reconnexion à permet d’actualiser ces stratégies.

3.  Vous pouvez actualiser les paramètres de stratégie à l’aide de l’utilitaire de ligne de commande gpupdate ou en vous déconnectant, puis en vous reconnectant à votre ordinateur. Pour de meilleurs résultats, exécutez gpupdate, puis déconnectez-vous et reconnectez-vous à votre ordinateur. En règle générale, les paramètres de sécurité sont actualisés toutes les 90 minutes sur une station de travail ou un serveur et toutes les 5 minutes sur un contrôleur de domaine. Les paramètres sont également actualisés toutes les 16 heures, qu'ils aient été modifiés ou non. Il s’agit de paramètres configurables. les intervalles d’actualisation peuvent donc être différents dans chaque domaine.

4.  Vérifiez les stratégies qui s’appliquent. Vérifiez les stratégies au niveau du domaine pour aucun paramètre de **remplacement** .

5.  Les stratégies de restriction logicielle qui sont spécifiées dans un domaine via stratégie de groupe remplacent les stratégies configurées localement. Utilisez l’outil en ligne de commande Gpresult pour déterminer l’effet net de la stratégie. Cela peut impliquer la présence d’une stratégie du domaine qui remplace votre paramètre local.

6.  Si les paramètres de stratégie SRP et AppLocker se trouvent dans le même objet de stratégie de groupe, les paramètres AppLocker sont prioritaires sur Windows 7, Windows Server 2008 R2 et versions ultérieures. Il est recommandé de placer les paramètres de stratégie SRP et AppLocker dans différents objets de stratégie de groupe.

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>Après l’ajout d’une règle via un SRP, vous ne pouvez pas vous connecter à votre ordinateur
**Cause** : Votre ordinateur accède à de nombreux programmes et fichiers lorsqu’il démarre. Vous avez peut-être défini par inadvertance l’un de ces programmes ou fichiers sur non **autorisé**. Étant donné que l’ordinateur ne peut pas accéder au programme ou au fichier, il ne peut pas démarrer correctement.

**Solution :** Démarrez l’ordinateur en mode sans échec, connectez-vous en tant qu’administrateur local, puis modifiez les stratégies de restriction logicielle pour autoriser l’exécution du programme ou du fichier.

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>Un nouveau paramètre de stratégie ne s’applique pas à une extension de nom de fichier spécifique
**Cause** : L’extension de nom de fichier ne figure pas dans la liste des types de fichiers pris en charge.

**Solution :** Ajoutez l’extension de nom de fichier à la liste des types de fichiers pris en charge par les stratégies de restriction logicielle.

Les stratégies de restriction logicielle résolvent le problème de régulation du code inconnu ou non approuvé. Les stratégies de restriction logicielle sont des paramètres de sécurité permettant d’identifier les logiciels et de contrôler leur capacité à s’exécuter sur un ordinateur local, dans un site, un domaine ou une unité d’organisation et peuvent être implémentés via un objet de stratégie de groupe.

### <a name="a-default-rule-is-not-restricting-as-expected"></a>Une règle par défaut n’est pas restreinte comme prévu
**Cause** : Règles qui sont appliquées dans un ordre particulier, ce qui peut entraîner le remplacement des règles par défaut par des règles spécifiques. Le SRP applique les règles dans l’ordre suivant (le plus spécifique au niveau général) :

1.  Règles de hachage

2.  Règles de certificat

3.  Règles de chemins d’accès

4.  Règles de la zone Internet

5.  Règles par défaut

**Solution :** Évaluez les règles limitant l’application et, le cas échéant, supprimez toutes les règles sauf la règle par défaut.

### <a name="unable-to-discover-which-restrictions-are-applied"></a>Impossible de détecter les restrictions appliquées
**Cause** : Il n’existe aucune cause apparente pour le comportement inattendu, et l’actualisation de l’objet de stratégie de groupe n’a pas résolu le problème, si bien que des investigations supplémentaires sont nécessaires.

**Elles**

1.  Examinez le journal des événements système, en filtrant sur la source de la « stratégie de restriction logicielle ». Les entrées précisent explicitement la règle qui est implémentée pour chaque application.

2.  Activez la journalisation avancée. Pour plus d’informations, consultez [déterminer la liste d’autorisation et l’inventaire des applications pour les stratégies de restriction logicielle](software-restriction-policies.md) .


