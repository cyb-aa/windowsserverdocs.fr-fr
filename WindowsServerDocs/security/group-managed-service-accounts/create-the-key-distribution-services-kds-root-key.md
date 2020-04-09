---
title: Créer la clé racine du service de distribution de clés (KDS, Key Distribution Service)
description: Sécurité de Windows Server
ms.prod: windows-server
ms.technology: security-gmsa
ms.topic: article
ms.assetid: 42e5db8f-1516-4d42-be0a-fa932f5588e9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: d26cd32f021e8b00c6c9c6d3949a00f71096a3c9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857012"
---
# <a name="create-the-key-distribution-services-kds-root-key"></a>Créer la clé racine du service de distribution de clés (KDS, Key Distribution Service)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique destinée aux professionnels de l’informatique explique comment créer une clé racine de service de distribution de clés Microsoft (kdssvc. dll) sur le contrôleur de domaine à l’aide de Windows PowerShell pour générer des mots de passe de compte de service administré de groupe dans Windows Server 2012 ou version ultérieure.

Les contrôleurs de domaine (DC) requièrent une clé racine pour commencer à générer des mots de passe gMSA. Les contrôleurs de domaine n'autoriseront la création d'un compte gMSA que 10 heures après la création d'une clé racine pour permettre à tous les contrôleurs de domaine de converger leur réplication Active Directory. Ces 10 heures sont une mesure de sécurité pour empêcher la génération de mots de passe avant que tous les contrôleurs de domaine de l'environnement ne soient en mesure de répondre aux demandes de compte gMSA.  Si vous essayez d’utiliser un gMSA trop tôt, la clé n’a peut-être pas été répliquée sur tous les contrôleurs de domaine et, par conséquent, la récupération du mot de passe peut échouer lorsque l’hôte gMSA tente de récupérer le mot de passe. les échecs de récupération de mot de passe gMSA peuvent également se produire lors de l’utilisation de contrôleurs de la planification de réplication limitée ou en cas de problème de réplication.

> [!NOTE]
> La suppression et la recréation de la clé racine peuvent entraîner des problèmes où l’ancienne clé continue à être utilisée après la suppression en raison de la mise en cache de la clé. Le service de distribution de clés (KDC) doit être redémarré sur tous les contrôleurs de domaine si la clé racine est recréée.

Vous devez appartenir au groupe **Admins du domaine** ou **Administrateurs de l'entreprise**, ou à un groupe équivalent, pour réaliser cette procédure. Pour plus d’informations sur l’utilisation des comptes et appartenances à des groupes appropriés, voir [Groupes locaux et de domaine par défaut](https://technet.microsoft.com/library/dd728026(WS.10).aspx).

> [!NOTE]
> Une architecture 64 bits est requise pour exécuter les commandes Windows PowerShell utilisées pour administrer les comptes de service administrés de groupe.

#### <a name="to-create-the-kds-root-key-using-the-add-kdsrootkey-cmdlet"></a>Pour créer la clé racine KDS à l’aide de l’applet de commande Add-KdsRootKey

1.  Sur le contrôleur de domaine Windows Server 2012 ou version ultérieure, exécutez Windows PowerShell à partir de la barre des tâches.

2.  À l'invite de commandes du module Active Directory pour Windows PowerShell, tapez les commandes suivantes et appuyez sur Entrée :

    **Add-KdsRootKey-EffectiveImmediately**

    > [!TIP]
    > Le paramètre Heure d'effectivité peut être utilisé pour laisser le temps aux clés d'être propagées sur tous les contrôleurs de domaine avant leur utilisation. L’utilisation de Add-KdsRootKey-EffectiveImmediately ajoute une clé racine au DC cible qui sera utilisée immédiatement par le service KDS. Toutefois, les autres contrôleurs de domaine ne seront pas en mesure d’utiliser la clé racine tant que la réplication n’aura pas réussi.

Dans les environnements de test à contrôleur de domaine unique, vous pouvez créer une clé racine KDS et définir l'heure de début sur une heure passée afin d'éviter l'intervalle d'attente avant la génération de clés. Utilisez pour cela procédure suivante. Vérifiez qu'un événement 4004 a été consigné dans le journal des événements kds.

#### <a name="to-create-the-kds-root-key-in-a-test-environment-for-immediate-effectiveness"></a>Pour créer la clé racine KDS dans un environnement de test et faire qu'elle soit immédiatement effective

1.  Sur le contrôleur de domaine Windows Server 2012 ou version ultérieure, exécutez Windows PowerShell à partir de la barre des tâches.

2.  À l'invite de commandes du module Active Directory pour Windows PowerShell, tapez les commandes suivantes et appuyez sur Entrée :

    **$a = obtient la date**

    **$b = $a. AddHours (-10)**

    **Add-KdsRootKey-EffectiveTime $b**

    Ou utilisez une commande unique

    **Add-KdsRootKey-EffectiveTime ((obtient-date). AddHours (-10))**

## <a name="see-also"></a>Voir aussi
[Prise en main des comptes de service administrés de groupe](getting-started-with-group-managed-service-accounts.md)


