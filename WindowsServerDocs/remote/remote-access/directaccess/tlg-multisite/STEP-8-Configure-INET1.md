---
title: ÉTAPE 8 configurer INET1
description: 'Cette rubrique fait partie du Guide de laboratoire de Test : illustrer un déploiement Multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: coreyp
author: coreyp-at-msft
ms.openlocfilehash: d6967b975b3a950c90de465872832d623755a494
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281415"
---
# <a name="step-8-configure-inet1"></a>ÉTAPE 8 : Configurer INET1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Pour activer les ordinateurs clients pour se connecter aux serveurs d’accès à distance via Internet, vous devez configurer une entrée DNS pour 2-EDGE1 sur INET1.  
  
### <a name="to-create-the-2-edge1-dns-entry"></a>Pour créer l’entrée DNS de 2-EDGE1  
  
1.  Sur le **Démarrer** , tapez**dnsmgmt.msc**, puis appuyez sur ENTRÉE.  
  
2.  Dans l’arborescence de la console, ouvrez **Zones de recherche directe**, cliquez sur **contoso.com**, puis cliquez sur **contoso.com**, puis cliquez sur **nouvel hôte (A ou AAAA)** .  
  
3.  Dans **nom**, type **2-EDGE1**. Dans **adresse IP**, type **131.107.0.20**. Cliquez sur **Ajouter un hôte**, sur **OK**, puis sur **Terminé**.  
  


