---
title: ÉTAPE 6 Testez la connectivité DirectAccess à partir du sous-réseau HomeNet
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer DirectAccess avec l’authentification par mot de passe à usage unique et RSA SecurID pour Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: b9b77cfd-8dd4-476b-a118-f3d6bf59e7b1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: dbbaae146a0101decfc62d950b98c1af10fb39b2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814404"
---
# <a name="step-6-test-directaccess-connectivity-from-the-homenet-subnet"></a>ÉTAPE 6 Testez la connectivité DirectAccess à partir du sous-réseau HomeNet

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Le déploiement du mot de passe à usage unique DirectAccess est maintenant terminé et vous pouvez commencer à tester la connectivité à partir du sous-réseau HomeNet.  
  
### <a name="to-test-otp-functionality-from-the-homenet-subnet-on-client1"></a>Pour tester la fonctionnalité de mot de passe à usage unique à partir du sous-réseau HomeNet sur CLIENT1  
  
1. Sur CLIENT1, assurez-vous que vous êtes connecté en tant que **User1**.  
  
2. Dans l’écran d' **Accueil** , tapez**PowerShell. exe**, cliquez avec le bouton droit sur **PowerShell**, cliquez sur **avancé**, puis sur **exécuter en tant qu’administrateur**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
3. Dans la fenêtre Windows PowerShell, tapez **gpupdate/force**, puis appuyez sur entrée.  
  
4. Débranchez CLIENT1 du sous-réseau Corpnet et connectez-le au sous-réseau HomeNet.  
  
5. Sur CLIENT1, ouvrez Internet Explorer, et dans la barre d’adresses, tapez **https://app1.corp.contoso.com/** , puis appuyez sur entrée. Appuyez sur F5.  
  
   Le site ne doit pas s’ouvrir.  
  
6. Dans l’écran d' **Accueil** , tapez**RSA**, puis cliquez sur **jeton RSA SecurID**.  
  
7. Attendez que le jeton RSA SecurID change le mot de passe à usage unique, puis cliquez sur **copier**.  
  
8. Cliquez sur le**connexions réseau**icône dans la zone de notification pour accéder au Gestionnaire de médias DA.  
  
9. Cliquez sur **connexion DirectAccess contoso**, puis sur **Continuer**.  
  
10. Appuyez sur Ctrl + Alt + Suppr, puis cliquez sur la vignette **mot de passe à usage** unique.  
  
11. Collez le jeton de jeton à huit chiffres précédemment copié, puis cliquez sur **OK**. Attendez la fin de l’authentification. L’état de la connexion à l’espace de travail DirectAccess sera maintenant **connecté**.  
  
12. Dans Internet Explorer, dans la barre d’adresses, tapez **https://app1.corp.contoso.com/** et appuyez sur entrée. Appuyez sur F5. Vous allez voir le site web IIS par défaut sur APP1.  
  
13. Dans la barre d’adresses d’Internet Explorer, tapez **https://app2.corp.contoso.com/** , puis appuyez sur entrée. Appuyez sur F5. Le site Web IIS par défaut s’affiche sur APP2.  
  
14. Dans l’écran d' **Accueil** , tapez<strong>\\\app1\files</strong>, puis appuyez sur entrée.  
  
15. Dans la fenêtre dossiers partagés de **fichiers** , double-cliquez sur le fichier **example. txt** . Vous verrez le contenu du fichier example. txt.  
  
16. Dans l’écran d' **Accueil** , tapez<strong>\\\app2\files</strong>, puis appuyez sur entrée.  
  
17. Dans la fenêtre dossiers partagés de **fichiers** , double-cliquez sur le **nouveau fichier texte Text. txt** . Vous verrez le contenu du nouveau fichier texte document. txt.  
  


