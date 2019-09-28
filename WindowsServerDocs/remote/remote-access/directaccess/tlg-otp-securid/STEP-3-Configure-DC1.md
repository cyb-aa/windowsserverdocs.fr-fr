---
title: ÉTAPE 3 configurer DC1
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer DirectAccess avec l’authentification par mot de passe à usage unique et RSA SecurID pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 836a2a08-3d22-48d2-873e-80d7e57ebbd6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7655208fb537e78839f2b459c8df0e24c0573aa7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367548"
---
# <a name="step-3-configure-dc1"></a>ÉTAPE 3 configurer DC1

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

DC1 agit en tant que contrôleur de domaine, serveur DNS et serveur DHCP pour le domaine corp.contoso.com. Configurez DC1 comme suit :  
  
## <a name="verify-user1-has-a-user-principal-name-defined-on-dc1"></a>Vérifier que User1 a un nom d’utilisateur principal défini sur DC1  
  
1.  Sur DC1, ouvrez Gestionnaire de serveur, puis cliquez sur **AD DS** dans le volet gauche. Cliquez avec le bouton droit sur **DC1** , puis sélectionnez **Active Directory utilisateurs et ordinateurs**. Dans le volet gauche, développez **Corp. contoso. com\Users**, puis double-cliquez sur User1.  
  
2.  Sous l’onglet **compte** , vérifiez que **nom d’ouverture de session** de l’utilisateur a la valeur utilisateur1. Si ce n’est pas le cas, entrez **User1** dans le champ **nom d’ouverture de session** de l’utilisateur.  
  
3.  Cliquez sur **OK**. Fermez la console **Utilisateurs et ordinateurs Active Directory** .  
  


