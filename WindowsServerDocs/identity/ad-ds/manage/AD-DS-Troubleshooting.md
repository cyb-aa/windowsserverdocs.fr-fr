---
ms.assetid: fd3bc84a-48eb-4f00-9dc2-846bf2c2668b
title: Résolution des problèmes liés aux services AD DS
description: Vue d’ensemble de la section de résolution des problèmes pour AD DS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: dcscontentpm
ms.date: 11/22/2019
ms.topic: article
ms.prod: windows-server
audience: Admin
ms.custom:
- CSSTroubleshoot
ms.technology: identity-adds
ms.openlocfilehash: 500ffe05fe75db99f98fda09b5f86b8547ea7e32
ms.sourcegitcommit: 30de12eebeb0fc79567d6bb6ab513692ea2415d3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74853584"
---
# <a name="ad-ds-troubleshooting"></a>Résolution des problèmes liés aux services AD DS

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Cette section comprend des recommandations et des procédures de dépannage pour le diagnostic et la résolution des problèmes qui peuvent se produire pendant la réplication de Active Directory. Il se concentre sur la façon de répondre aux entrées du journal des événements du service d’annuaire et sur l’interprétation des messages que les outils tels que repadmin. exe et Dcdiag. exe peuvent signaler.

Repadmin. exe et Dcdiag. exe sont disponibles sur tous les contrôleurs de domaine qui exécutent Windows Server 2012 R2 ou versions ultérieures. Pour plus d’informations sur l’utilisation de ces outils pour résoudre les problèmes, consultez les articles suivants.

- [Configuration d’un ordinateur pour la résolution des problèmes Active Directory](../manage/troubleshoot/Configuring-a-Computer-for-Troubleshooting.md)
- [Résolution des problèmes de réplication Active Directory](../manage/troubleshoot/Troubleshooting-Active-Directory-Replication-Problems.md)

Une autre technologie utile est Suivi d’v nements pour Windows (ETW). Vous pouvez utiliser ETW pour résoudre les problèmes de communication LDAP entre les contrôleurs de domaine. Pour plus d’informations, consultez [utilisation d’ETW pour résoudre les problèmes de connexion LDAP](../manage/troubleshoot/troubleshoot-ldap-using-etw.md).

Vous pouvez également installer des Outils d’administration de serveur distant (RSAT) sur un serveur membre qui exécute Windows 10. Pour plus d’informations sur l’installation de RSAT, voir [Outils d’administration de serveur distant](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).
