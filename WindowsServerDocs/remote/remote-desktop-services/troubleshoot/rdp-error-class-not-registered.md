---
title: Les clients ne peuvent pas se connecter et obtiennent l’erreur Classe non enregistrée
description: Résolution de l’erreur « Classe non enregistrée » avec une connexion Bureau à distance.
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7a98e894f3eaf47e257ab39c640e93101fd76fc8
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265901"
---
# <a name="clients-cant-connect-and-get-the-class-not-registered-error"></a>Les clients ne peuvent pas se connecter et obtiennent l’erreur « Classe non enregistrée »

Quand vous essayez de vous connecter à un ordinateur distant à l’aide d’un client qui exécute Windows 10 version 1709 ou ultérieure, le client peut ne pas se connecter et le serveur Hôte de session Bureau à distance signale un message qui contient le code d’erreur « Classe non enregistrée (0x80040154) ».

Ce problème se produit quand l’utilisateur qui essaie de se connecter a un profil utilisateur obligatoire. Pour résoudre ce problème, installez la mise à jour de Windows 10 du [24 juillet 2018 — KB4338817 (build du système d’exploitation 16299.579)](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817).
