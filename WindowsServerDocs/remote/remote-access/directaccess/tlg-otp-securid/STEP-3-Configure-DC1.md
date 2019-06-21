---
title: ÉTAPE 3 configurer DC1
description: Cette rubrique fait partie du Guide de laboratoire de Test - démontrer DirectAccess avec l’authentification OTP et RSA SecurID pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 836a2a08-3d22-48d2-873e-80d7e57ebbd6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 338e214ac10796d3f9864aef74190f2d27f173b7
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281357"
---
# <a name="step-3-configure-dc1"></a>ÉTAPE 3 configurer DC1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

DC1 joue un contrôleur de domaine, le serveur DNS et le serveur DHCP pour le domaine corp.contoso.com. Configurez DC1 comme suit :  
  
## <a name="verify-user1-has-a-user-principal-name-defined-on-dc1"></a>Vérifiez que User1 a un nom d’utilisateur Principal défini sur DC1  
  
1.  Sur DC1, ouvrez le Gestionnaire de serveur, puis cliquez sur **AD DS** dans le volet gauche. Avec le bouton droit **DC1** et sélectionnez **Active Directory Users and Computers**. Dans le volet gauche, développez **corp.contoso.com\Users**, double-cliquez sur User1.  
  
2.  Sur le **compte** onglet Vérifiez que **nom d’utilisateur d’ouverture de session** est définie à l’utilisateur User1. Dans le cas contraire, puis entrez **User1** dans le **nom d’utilisateur d’ouverture de session** champ.  
  
3.  Cliquez sur **OK**. Fermez la console **Utilisateurs et ordinateurs Active Directory** .  
  


