---
title: Créer la clé racine du service de distribution de clés (KDS, Key Distribution Service)
description: Sécurité de Windows Server
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
ms.openlocfilehash: 3d5f7b46b28e6a2fbfafb664b69aebc8d34886fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867210"
---
# <a name="create-the-key-distribution-services-kds-root-key"></a>Créer la clé racine du service de distribution de clés (KDS, Key Distribution Service)

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique destinée aux professionnels de l’informatique décrit comment créer une clé de racine de Service de Distribution de clés Microsoft (kdssvc.dll) sur le contrôleur de domaine à l’aide de Windows PowerShell pour générer des mots de passe de compte de Service administré groupe dans Windows Server 2012.

 Contrôleurs de domaine Windows Server 2012 (DC) nécessitent une clé racine pour commencer à générer des mots de passe de compte gMSA. Les contrôleurs de domaine n'autoriseront la création d'un compte gMSA que 10 heures après la création d'une clé racine pour permettre à tous les contrôleurs de domaine de converger leur réplication Active Directory. Ces 10 heures sont une mesure de sécurité pour empêcher la génération de mots de passe avant que tous les contrôleurs de domaine de l'environnement ne soient en mesure de répondre aux demandes de compte gMSA.  Si vous essayez d’utiliser un compte gMSA trop tôt la clé n’a ne peut-être pas été répliquée sur tous les contrôleurs de domaine 2012 de serveur Windows et par conséquent la récupération de mot de passe peut échouer lorsque l’hôte de service administré de groupe tente de récupérer le mot de passe. échecs de récupération de mot de passe de compte gMSA peuvent également se produire lorsque vous utilisez des contrôleurs de domaine avec les planifications de réplication limitée ou s’il existe un problème de réplication.

Vous devez appartenir au groupe **Admins du domaine** ou **Administrateurs de l'entreprise**, ou à un groupe équivalent, pour réaliser cette procédure. Pour plus d’informations sur l’utilisation des comptes et appartenances à des groupes appropriés, voir [Groupes locaux et de domaine par défaut](https://technet.microsoft.com/library/dd728026(WS.10).aspx).

> [!NOTE]
> Une architecture 64 bits est requise pour exécuter les commandes Windows PowerShell utilisées pour administrer les comptes de service administrés de groupe.

#### <a name="to-create-the-kds-root-key-using-the-add-kdsrootkey-cmdlet"></a>Pour créer la clé racine KDS à l’aide de l’applet de commande Add-KdsRootKey

1.  Sur le contrôleur de domaine Windows Server 2012, exécutez la commande Windows PowerShell à partir de la barre des tâches.

2.  À l'invite de commandes du module Active Directory pour Windows PowerShell, tapez les commandes suivantes et appuyez sur Entrée :

    **Add-KdsRootKey -EffectiveImmediately**

    > [!TIP]
    > Le paramètre Heure d'effectivité peut être utilisé pour laisser le temps aux clés d'être propagées sur tous les contrôleurs de domaine avant leur utilisation. À l’aide de Add-KdsRootKey EffectiveImmediately - ajouter une clé racine pour la contrôleur de domaine qui sera utilisé par le service KDS immédiatement de la cible. Toutefois, les autres contrôleurs de domaine Windows Server 2012 ne sera pas en mesure d’utiliser la clé racine jusqu'à ce que la réplication ait réussi.

Dans les environnements de test à contrôleur de domaine unique, vous pouvez créer une clé racine KDS et définir l'heure de début sur une heure passée afin d'éviter l'intervalle d'attente avant la génération de clés. Utilisez pour cela procédure suivante. Vérifiez qu'un événement 4004 a été consigné dans le journal des événements kds.

#### <a name="to-create-the-kds-root-key-in-a-test-environment-for-immediate-effectiveness"></a>Pour créer la clé racine KDS dans un environnement de test et faire qu'elle soit immédiatement effective

1.  Sur le contrôleur de domaine Windows Server 2012, exécutez la commande Windows PowerShell à partir de la barre des tâches.

2.  À l'invite de commandes du module Active Directory pour Windows PowerShell, tapez les commandes suivantes et appuyez sur Entrée :

    **$a=Get-Date**

    **$b=$a.AddHours(-10)**

    **Add-KdsRootKey -EffectiveTime $b**

    Ou utilisez une commande unique

    **Add-KdsRootKey -EffectiveTime ((get-date).addhours(-10))**

## <a name="see-also"></a>Voir aussi
[Mise en route avec le groupe de comptes de Service administrés](getting-started-with-group-managed-service-accounts.md)


