---
title: ÉTAPE 3 installer et configurer CLIENT2
description: 'Cette rubrique fait partie du Guide de laboratoire de Test : illustrer un déploiement Multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f009fdd1-94e6-4ccb-8c6e-609a5394db53
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d19f204e139433d11cf674c4ec39a134cde7eefa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813170"
---
# <a name="step-3-install-and-configure-client2"></a>ÉTAPE 3 installer et configurer CLIENT2

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

CLIENT2 est un 7 Windows&reg; ordinateur qui est utilisé pour illustrer la descendante compatibilité d’accès à distance s’exécutant sur les serveurs Windows Server 2016.  
  
1. Pour installer le système d’exploitation sur le CLIENT2. Installer Windows&reg; Enterprise 7 ou Windows&reg; 7 Édition intégrale sur le CLIENT2.  
  
2. Pour joindre CLIENT2 au domaine CORP. Joindre CLIENT2 au domaine corp.contoso.com.  
  
## <a name="to-install-the-operating-system-on-client2"></a>Pour installer le système d’exploitation sur le CLIENT2  
  
1.  Démarrez l’installation de Windows 7.  
  
2.  Lorsque vous êtes invité à un nom d’utilisateur, tapez **User1**. Lorsque vous êtes invité à un nom d’ordinateur, tapez **CLIENT2**.  
  
3.  Lorsque vous y êtes invité pour un mot de passe, tapez un mot de passe fort à deux reprises.  
  
4.  Lorsque vous y êtes invité pour les paramètres de protection, cliquez sur **utiliser les paramètres recommandés**.  
  
5.  Lorsque vous êtes invité à l’emplacement actuel de votre ordinateur, cliquez sur **réseau professionnel**.  
  
6.  Connecter CLIENT2 à un réseau qui a accès à Internet et exécutez la mise à jour de Windows pour installer les dernières mises à jour pour Windows 7 et vous déconnecter puis à partir d’Internet.  
  
7.  Connectez CLIENT2 au sous-réseau Corpnet.  
  
## <a name="user-account-control"></a>Contrôle de compte d’utilisateur  
Lorsque vous configurez le système d’exploitation Windows 7, vous devez cliquer sur **continuer** sur le **contrôle de compte d’utilisateur** boîte de dialogue (UAC) pour certaines tâches. Plusieurs tâches de configuration nécessitent l’approbation de l’UAC. Lorsque vous y êtes invité, toujours cliquer sur **continuer** pour autoriser ces modifications.  
  
## <a name="to-join-client2-to-the-corp-domain"></a>Pour joindre le CLIENT2 au domaine CORP  
  
1.  Cliquez sur **Démarrer**, cliquez avec le bouton droit sur **Ordinateur**, puis cliquez sur **Propriétés**.  
  
2.  Sur le **système** page, dans le **paramètres nom, domaine et groupe de travail de l’ordinateur** zone, cliquez sur **modifier les paramètres**.  
  
3.  Dans la boîte de dialogue **Propriétés système**, sous l'onglet **Nom de l'ordinateur**, cliquez sur **Modifier**.  
  
4.  Sur le **modification du nom ou du domaine d’ordinateur** boîte de dialogue, cliquez sur **domaine**, type **corp.contoso.com**, puis cliquez sur **OK**.  
  
5.  Lorsque vous y êtes invité pour un nom d’utilisateur et le mot de passe, tapez le nom d’utilisateur et le mot de passe du compte de domaine utilisateur1, puis cliquez sur **OK**.  
  
6.  Lorsqu’une boîte de dialogue de bienvenue s’affiche dans le domaine corp.contoso.com, cliquez sur **OK**.  
  
7.  Lorsque vous voyez une boîte de dialogue zone qui vous invite à redémarrer l’ordinateur, cliquez sur **OK**.  
  
8.  Sur le **propriétés système** boîte de dialogue, cliquez sur **fermer**, et lorsque vous voyez une boîte de dialogue qui vous invite à redémarrer l’ordinateur, cliquez sur **redémarrer maintenant**.  
  
9. Une fois l’ordinateur redémarré, connectez-vous en tant que CORP\User1.
