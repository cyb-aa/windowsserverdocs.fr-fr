---
title: Les clients ne peuvent pas se connecter et obtiennent l’erreur Classe non enregistrée
description: Résolution de l’erreur « Classe non enregistrée » avec une connexion Bureau à distance.
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 52e696bd4229b947ea63a379211192b8664a9f93
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857182"
---
# <a name="clients-cant-connect-and-get-the-class-not-registered-error"></a>Les clients ne peuvent pas se connecter et obtiennent l’erreur « Classe non enregistrée »

Quand vous essayez de vous connecter à un ordinateur distant à l’aide d’un client qui exécute Windows 10 version 1709 ou ultérieure, le client peut ne pas se connecter et le serveur Hôte de session Bureau à distance signale un message qui contient le code d’erreur « Classe non enregistrée (0x80040154) ».

Ce problème se produit quand l’utilisateur qui essaie de se connecter a un profil utilisateur obligatoire. Pour résoudre ce problème, installez la mise à jour de Windows 10 du [24 juillet 2018 — KB4338817 (build du système d’exploitation 16299.579)](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817).
