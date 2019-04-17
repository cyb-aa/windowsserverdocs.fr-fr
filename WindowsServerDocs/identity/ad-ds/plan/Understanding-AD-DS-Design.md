---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: "Présentation de conception d’ADDS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 09afe3d19add87327d05bfafba6e0492a278b968
ms.sourcegitcommit: 1c3e6375b2e8eb01cfd299d0e9478fee46905c99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="understanding-ad-ds-design"></a>Présentation de conception d’ADDS

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Organisations peuvent utiliser des Services de domaine ActiveDirectory (ADDS) dans Windows Server pour simplifier la gestion des utilisateurs et des ressources lors de la création des infrastructures évolutives, sécurisées et gérables. Vous pouvez utiliser les services ADDS pour gérer votre infrastructure réseau, y compris la filiale MicrosoftExchange Server et plusieurs environnements de forêt.  
  
Un projet de déploiement des services ADDS comprend trois phases: une phase de conception, une phase de déploiement et une phase d’opérations. Pendant la phase de conception, l’équipe de conception crée une conception de la structure logique de domaine ActiveDirectory qui répond le mieux aux besoins de chaque division de l’organisation qui utilise le service d’annuaire. Une fois la conception est approuvée, l’équipe de déploiement teste la conception dans un environnement de laboratoire et puis implémente la conception de l’environnement de production. Étant donné que le test est effectué par l’équipe de déploiement et elle affecte potentiellement la phase de conception, il est une activité intermédiaire qui chevauche la conception et déploiement. Lorsque le déploiement est terminé, l’équipe des opérations est responsable de la gestion du service d’annuaire.  
  
Bien que les stratégies de conception et de déploiement de WindowsServerADDS qui sont présentées dans ce guide sont basés sur approfondies et de test de programme pilote et de mise en œuvre dans les environnements de client, vous devrez peut-être personnaliser la présentation des services ADDS et déploiement pour mieux répondre aux environnements spécifiques et complexes.  
  
-   Pour plus d’informations sur le déploiement des services ADDS dans un environnement de filiales, voir la Read-Only le contrôleur de domaine (RODC) Branch Office Planning Guide ([https://go.microsoft.com/fwlink/?LinkId=100207](https://go.microsoft.com/fwlink/?LinkId=100207)).  
  
-   Pour plus d’informations sur le déploiement des services ADDS dans un environnement Exchange, voir Exchange2007 - planification d’ActiveDirectory ([https://go.microsoft.com/fwlink/?LinkId=88904](https://go.microsoft.com/fwlink/?LinkId=88904)).  
  
-   Pour plus d’informations sur le déploiement des services ADDS dans un environnement à plusieurs forêts, voir plusieurs considérations relatives à la forêt dans Windows2000 et Windows Server2003 ([https://go.microsoft.com/fwlink/?LinkId=88905](https://go.microsoft.com/fwlink/?LinkId=88905)).  
  


