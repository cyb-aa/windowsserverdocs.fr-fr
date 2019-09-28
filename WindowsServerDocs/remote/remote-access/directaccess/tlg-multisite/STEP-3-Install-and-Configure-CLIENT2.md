---
title: ÉTAPE 3 installation et configuration de CLIENT2
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer un déploiement multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f009fdd1-94e6-4ccb-8c6e-609a5394db53
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b2d8cb1538de35a97ae83888d26abd7ad7bc8b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388318"
---
# <a name="step-3-install-and-configure-client2"></a>ÉTAPE 3 installation et configuration de CLIENT2

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

CLIENT2 est un ordinateur Windows 7 @ no__t-0 utilisé pour illustrer la compatibilité descendante de l’accès à distance exécuté sur les serveurs Windows Server 2016.  
  
1. Pour installer le système d’exploitation sur CLIENT2. Installez Windows @ no__t-0 7 entreprise ou Windows @ no__t-1 7 Ultimate sur CLIENT2.  
  
2. Pour joindre CLIENT2 au domaine CORP. Joignez CLIENT2 au domaine corp.contoso.com.  
  
## <a name="to-install-the-operating-system-on-client2"></a>Pour installer le système d’exploitation sur CLIENT2  
  
1.  Démarrez l’installation de Windows 7.  
  
2.  Lorsque vous êtes invité à entrer un nom d’utilisateur, tapez **User1**. Lorsque vous êtes invité à entrer un nom d’ordinateur, tapez **client2**.  
  
3.  Lorsque vous êtes invité à entrer un mot de passe, tapez un mot de passe fort à deux reprises.  
  
4.  Lorsque vous êtes invité à entrer les paramètres de protection, cliquez sur **utiliser les paramètres recommandés**.  
  
5.  Lorsque vous êtes invité à entrer l’emplacement actuel de votre ordinateur, cliquez sur **réseau professionnel**.  
  
6.  Connectez CLIENT2 à un réseau disposant d’un accès Internet et exécutez Windows Update pour installer les dernières mises à jour pour Windows 7, puis vous déconnecter d’Internet.  
  
7.  Connectez CLIENT2 au sous-réseau Corpnet.  
  
## <a name="user-account-control"></a>Contrôle de compte d’utilisateur  
Quand vous configurez le système d’exploitation Windows 7, vous devez cliquer sur **Continuer** dans la boîte de dialogue **contrôle de compte d’utilisateur** (UAC) pour certaines tâches. Plusieurs tâches de configuration requièrent l’approbation du contrôle de compte d’utilisateur. Quand vous y êtes invité, cliquez toujours sur **Continuer** pour autoriser ces modifications.  
  
## <a name="to-join-client2-to-the-corp-domain"></a>Pour joindre CLIENT2 au domaine CORP  
  
1.  Cliquez sur **Démarrer**, cliquez avec le bouton droit sur **Ordinateur**, puis cliquez sur **Propriétés**.  
  
2.  Dans la page **système** , dans la zone nom de l' **ordinateur, domaine et groupe de travail** , cliquez sur **modifier les paramètres**.  
  
3.  Dans la boîte de dialogue **Propriétés système**, sous l'onglet **Nom de l'ordinateur**, cliquez sur **Modifier**.  
  
4.  Dans la boîte de dialogue **modification du nom ou du domaine** de l’ordinateur, cliquez sur **domaine**, tapez **Corp.contoso.com**, puis cliquez sur **OK**.  
  
5.  Lorsque vous êtes invité à entrer un nom d’utilisateur et un mot de passe, tapez le nom d’utilisateur et le mot de passe du compte de domaine User1, puis cliquez sur **OK**.  
  
6.  Lorsqu’une boîte de dialogue de bienvenue s’affiche dans le domaine corp.contoso.com, cliquez sur **OK**.  
  
7.  Quand une boîte de dialogue s’affiche pour vous inviter à redémarrer l’ordinateur, cliquez sur **OK**.  
  
8.  Dans la boîte de dialogue **Propriétés système** , cliquez sur **Fermer**. quand une boîte de dialogue s’affiche pour vous inviter à redémarrer l’ordinateur, cliquez sur **redémarrer maintenant**.  
  
9. Après le redémarrage de l’ordinateur, ouvrez une session en tant que CORP\User1.
