---
title: Étape 3 vérifier le déploiement
description: Cette rubrique fait partie du guide les Clients DirectAccess de gérer à distance dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a78a078-d2e7-4cbd-b8d5-20cfb6d1524b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 89b9e53b7321ebeddda50a448829ad73395eeed0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835610"
---
# <a name="step-3-verify-the-deployment"></a>Étape 3 vérifier le déploiement

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment vérifier que vous avez correctement configuré votre déploiement pour la gestion à distance des clients DirectAccess.  
  
### <a name="to-verify-proper-deployment"></a>Pour vérifier le bon déroulement  
  
1.  Connecter un ordinateur client DirectAccess au réseau d’entreprise et d’obtenir l’objet de stratégie de groupe.  
  
2.  Sur l’ordinateur client, cliquez sur le **connexions réseau** icône dans la zone de notification pour accéder au Gestionnaire de médias de DirectAccess.  
  
3.  Cliquez sur **connexion DirectAccess**, et vous verrez que l’état est **connecté localement**.  
  
4.  Supprimer l’ordinateur du réseau d’entreprise et vous connecter à un réseau public.  
  
5.  À l’invite de commandes, tapez **nltest/dsgetdc : [nom de domaine complet]**. Cette commande vérifie que le réseau d’entreprise est accessible au client. Si le contrôleur de domaine n’est pas accessible, le message d’erreur suivant affiche la création de rapports qui le domaine n’existe pas : ERROR_NO_SUCH_DOMAIN.  
  


