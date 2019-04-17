---
title: "Créer la clé racine KDS de Distribution de clés Services"
description: "Sécurité de Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 42e5db8f-1516-4d42-be0a-fa932f5588e9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 30075e56f3ca8e90a0655508efeacfcf2aaa0337
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="create-the-key-distribution-services-kds-root-key"></a>Créer la clé racine KDS de Distribution de clés Services

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique pour les professionnels de l’informatique décrit comment créer une clé de racine de Service de Distribution de clés Microsoft (kdssvc.dll) sur le contrôleur de domaine à l’aide de Windows PowerShell pour générer groupe compte de Service administré mots de passe dans Windows Server2012.

 Contrôleurs de domaine Windows Server2012 (DC) nécessitent une clé racine pour commencer à générer des mots de passe de compte gMSA. Les contrôleurs de domaine attendra jusqu'à 10heures à partir de l’heure de création pour permettre à tous les contrôleurs de domaine à converger leur réplication ActiveDirectory avant d’autoriser la création d’un compte gMSA. Au cours des 10heures est une mesure de sécurité pour empêcher la génération de mot de passe de se produire avant que tous les contrôleurs de domaine dans l’environnement sont capables de répondre aux demandes de service administré de groupe.  Si vous essayez d’utiliser un compte gMSA trop tôt la clé n’a ne peut-être pas été répliquée sur tous les contrôleurs de domaine 2012 de serveur Windows et par conséquent, une récupération de mot de passe peut échouer lorsque l’hôte de service administré de groupe tente de récupérer le mot de passe. échecs de récupération de mot de passe de compte gMSA peuvent également se produire lorsque vous utilisez des contrôleurs de domaine avec des planifications de réplication limitée ou s’il existe un problème de réplication.

L’appartenance au groupe le **Admins du domaine** ou **administrateurs de l’entreprise** groupes, ou équivalent, est la condition minimale requise pour effectuer cette procédure. Pour obtenir des informations détaillées sur les comptes appropriés et les appartenances de groupe, voir [locaux et groupes de domaine par défaut](https://technet.microsoft.com/library/dd728026(WS.10).aspx).

> [!NOTE]
> Une architecture 64bits est requise pour exécuter les commandes Windows PowerShell qui sont utilisées pour administrer les comptes de Service administrés groupe.

#### <a name="to-create-the-kds-root-key-using-the-new-kdsrootkey-cmdlet"></a>Pour créer la clé racine KDS à l’aide de l’applet de commande New-KdsRootKey

1.  Sur le contrôleur de domaine Windows Server2012, exécutez Windows PowerShell à partir de la barre des tâches.

2.  À l’invite de commandes du module ActiveDirectory de Windows PowerShell, tapez les commandes suivantes et appuyez sur ENTRÉE:

    **Add-KdsRootKey - EffectiveImmediately**

    > [!TIP]
    > Le paramètre heure d’effectivité peut être utilisé pour laisser le temps aux clés d’être propagées à tous les contrôleurs de domaine avant de l’utiliser. À l’aide d’Add-KdsRootKey - EffectiveImmediately ajoutera une clé racine à la cible du contrôleur de domaine qui sera utilisé par le service KDS immédiatement. Toutefois, les autres contrôleurs de domaine Windows Server2012 ne sera pas en mesure d’utiliser la clé racine jusqu'à ce que la réplication ait réussi.

Pour les environnements de test avec un seul contrôleur de domaine, vous pouvez créer une clé racine KDS et définir l’heure de début dans le passé pour éviter l’attente de l’intervalle de la génération de clés à l’aide de la procédure suivante. Vérifiez qu’un événement 4004 a été consigné dans le journal des événements kds.

#### <a name="to-create-the-kds-root-key-in-a-test-environment-for-immediate-effectiveness"></a>Pour créer la clé racine KDS dans un environnement de test pour l’efficacité immédiate

1.  Sur le contrôleur de domaine Windows Server2012, exécutez Windows PowerShell à partir de la barre des tâches.

2.  À l’invite de commandes du module ActiveDirectory de Windows PowerShell, tapez les commandes suivantes et appuyez sur ENTRÉE:

    **$un = Get-Date.**

    **$b=$a.AddHours(-10)**

    **Add-KdsRootKey - EffectiveTime $b**

    Ou utilisez une commande unique

    **Add-KdsRootKey - EffectiveTime ((get-date).addhours(-10))**

## <a name="see-also"></a>Voir aussi
[Prise en main de groupe des comptes de Service administrés](getting-started-with-group-managed-service-accounts.md)


