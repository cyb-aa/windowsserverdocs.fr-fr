---
title: Étape 3 vérifier le déploiement
description: Cette rubrique fait partie du guide gérer des clients DirectAccess à distance dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a78a078-d2e7-4cbd-b8d5-20cfb6d1524b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e1a8d3208acb0e0ce891e517492916357b9fbfcc
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314353"
---
# <a name="step-3-verify-the-deployment"></a>Étape 3 vérifier le déploiement

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique explique comment vérifier que vous avez correctement configuré votre déploiement pour la gestion à distance des clients DirectAccess.  
  
### <a name="to-verify-proper-deployment"></a>Pour vérifier le bon déploiement  
  
1.  Connectez un ordinateur client DirectAccess au réseau d’entreprise et obtenez l’objet stratégie de groupe.  
  
2.  Sur l’ordinateur client, cliquez sur l’icône **connexions réseau** dans la zone de notification pour accéder au gestionnaire multimédia DirectAccess.  
  
3.  Cliquez sur **connexion DirectAccess**. vous verrez que l’État est **connecté localement**.  
  
4.  Retirez l’ordinateur du réseau d’entreprise et connectez-le à un réseau public.  
  
5.  À l’invite de commandes, tapez **nltest/dsgetdc : [nom de domaine complet]** . Cette commande permet de vérifier que le réseau d’entreprise est accessible au client. Si le contrôleur de domaine n’est pas accessible, le message d’erreur suivant indique que le domaine n’existe pas : ERROR_NO_SUCH_DOMAIN.  
  


