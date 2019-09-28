---
title: ÉTAPE 8 configurer INET1
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer un déploiement multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: coreyp
author: coreyp-at-msft
ms.openlocfilehash: 54674adc33f45a58f2515d07fed4c8a070ded5a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404704"
---
# <a name="step-8-configure-inet1"></a>ÉTAPE 8 : Configurer INET1

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Pour permettre aux ordinateurs clients de se connecter aux serveurs d’accès à distance via Internet, vous devez configurer une entrée DNS pour 2-EDGE1 sur INET1.  
  
### <a name="to-create-the-2-edge1-dns-entry"></a>Pour créer l’entrée DNS 2-EDGE1  
  
1.  Dans l’écran d' **Accueil** , tapez**dnsmgmt. msc**, puis appuyez sur entrée.  
  
2.  Dans l’arborescence de la console, ouvrez **zones de recherche directe**, cliquez sur **contoso.com**, cliquez avec le bouton droit sur **contoso.com**, puis cliquez sur **nouvel hôte (A ou AAAA)** .  
  
3.  Dans **nom**, tapez **2-Edge1**. Dans **adresse IP**, tapez **131.107.0.20**. Cliquez sur **Ajouter un hôte**, sur **OK**, puis sur **Terminé**.  
  


