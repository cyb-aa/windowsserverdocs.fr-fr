---
title: ÉTAPE 7 de tester la connectivité lors du retour au réseau d’entreprise
description: Cette rubrique fait partie du Guide de laboratoire de Test - décrire de DirectAccess dans un Cluster avec équilibrage de charge réseau Windows pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a7252d0-6db8-4a9d-98ee-75082ecd2929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 477d11f0e6bf296c41fb7116a7aae43787df263c
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283380"
---
# <a name="step-7-test-connectivity-when-returning-to-the-corpnet"></a>ÉTAPE 7 de tester la connectivité lors du retour au réseau d’entreprise

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

La plupart de vos utilisateurs déplacera entre des emplacements distants et le réseau d’entreprise, il est donc important que lorsqu’ils retournent sur le réseau d’entreprise qu’ils sont en mesure d’accéder aux ressources sans avoir à effectuer n’importe quelle configuration change. Accès à distance rend cela possible car lorsque le client DirectAccess revienne au réseau d’entreprise, il est en mesure d’établir une connexion au serveur d’emplacement réseau. Une fois la connexion HTTPS établie correctement au serveur d’emplacement réseau, le client DirectAccess désactive la configuration du client DirectAccess et utilise une connexion directe au réseau d’entreprise.  
  
### <a name="test-connectivity-on-client1"></a>Tester la connectivité sur CLIENT1  
  
1. Arrêter de CLIENT1 puis débranchez CLIENT1 à partir du sous-réseau de réseau domestique ou un commutateur virtuel et connectez-le au commutateur virtuel ou sous-réseau Corpnet. Activez sur CLIENT1 et connectez-vous en tant que CORP\User1.  
  
2. Ouvrez une fenêtre Windows PowerShell avec élévation de privilèges, tapez **ipconfig/all**, puis appuyez sur ENTRÉE. La sortie indique que CLIENT1 dispose d’une adresse IP locale, et qu’aucune ou n’est active 6to4, Teredo, IP-HTTPS tunnel.  
  
3. Tester la connectivité au partage réseau sur APP2. Sur le **Démarrer** , tapez<strong>\\\APP2\Files</strong>, puis appuyez sur ENTRÉE. Vous serez en mesure d’ouvrir le fichier dans ce dossier.  
  


