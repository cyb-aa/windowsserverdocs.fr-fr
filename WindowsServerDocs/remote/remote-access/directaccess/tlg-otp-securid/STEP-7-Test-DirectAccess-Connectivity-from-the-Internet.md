---
title: ÉTAPE 7 Testez la connectivité DirectAccess à partir d’Internet
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer DirectAccess avec l’authentification par mot de passe à usage unique et RSA SecurID pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed2a1616-30c6-482a-9a02-4a5023621f58
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 89f26dfa3be83167b7b62b8f464eede7f4db8db0
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308567"
---
# <a name="step-7-test-directaccess-connectivity-from-the-internet"></a>ÉTAPE 7 Testez la connectivité DirectAccess à partir d’Internet

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Le déploiement du mot de passe à usage unique DirectAccess a été testé à partir du sous-réseau HomeNet et peut maintenant être testé à partir d’Internet.  
  
### <a name="to-test-otp-functionality-from-the-internet-on-client1"></a>Pour tester la fonctionnalité de mot de passe à usage unique à partir d’Internet sur CLIENT1  
  
1. Sur CLIENT1, assurez-vous que vous êtes connecté en tant qu’utilisateur **User1**. Connectez CLIENT1 au sous-réseau Corpnet.  
  
2. Dans l’écran d' **Accueil** , tapez**PowerShell. exe**, cliquez avec le bouton droit sur **PowerShell**, cliquez sur **avancé**, puis sur **exécuter en tant qu’administrateur**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
3. Dans la fenêtre Windows PowerShell, tapez **gpupdate/force** , puis appuyez sur entrée.  
  
4. Débranchez CLIENT1 du sous-réseau HomeNet, connectez-le à Internet, puis redémarrez l’ordinateur.  
  
5. Sur CLIENT1, ouvrez Internet Explorer, et dans la barre d’adresses, tapez **https://app1.corp.contoso.com/** , puis appuyez sur entrée. Appuyez sur F5.  
  
   Le site ne doit pas s’ouvrir.  
  
6. Dans l’écran d' **Accueil** , tapez**RSA**, puis cliquez sur **jeton RSA SecurID**.  
  
7. Attendez que le jeton RSA SecurID change le mot de passe à usage unique, puis cliquez sur **copier**.  
  
8. Cliquez sur le**connexions réseau**icône dans la zone de notification pour accéder au Gestionnaire de médias DA.  
  
9. Cliquez sur connexion à l' **espace de travail**, puis sur **Continuer**.  
  
10. Appuyez sur Ctrl + Alt + Suppr, puis cliquez sur la vignette **mot de passe à usage** unique.  
  
11. Collez le jeton de jeton à huit chiffres précédemment copié, puis cliquez sur **OK**. Attendez la fin de l’authentification. L’état de la connexion à l’espace de travail DirectAccess sera maintenant **connecté**.  
  
12. Dans Internet Explorer, dans la barre d’adresses, tapez **https://app1.corp.contoso.com/** et appuyez sur entrée. Appuyez sur F5. Vous allez voir le site web IIS par défaut sur APP1.  
  
13. Dans la barre d’adresses d’Internet Explorer, tapez **https://app2.corp.contoso.com/** , puis appuyez sur entrée. Appuyez sur F5. Le site Web IIS par défaut s’affiche sur APP2.  
  
14. Dans l’écran d' **Accueil** , tapez<strong>\\\app1\files</strong>, puis appuyez sur entrée.  
  
15. Dans la fenêtre dossiers partagés de **fichiers** , double-cliquez sur le fichier **example. txt** . Vous verrez le contenu du fichier example. txt.  
  
16. Dans l’écran d' **Accueil** , tapez<strong>\\\app2\files</strong>, puis appuyez sur entrée.  
  
17. Dans la fenêtre dossiers partagés de **fichiers** , double-cliquez sur le **nouveau fichier texte Text. txt** . Vous verrez le contenu du nouveau fichier texte document. txt.  
  


