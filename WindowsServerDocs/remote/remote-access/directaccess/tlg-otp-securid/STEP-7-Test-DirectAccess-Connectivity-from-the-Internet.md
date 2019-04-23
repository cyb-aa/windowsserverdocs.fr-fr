---
title: ÉTAPE 7 de tester la connectivité DirectAccess à partir d’Internet
description: Cette rubrique fait partie du Guide de laboratoire de Test - démontrer DirectAccess avec l’authentification OTP et RSA SecurID pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed2a1616-30c6-482a-9a02-4a5023621f58
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1f79eece13f473e2dd7773a72e10e87612b9cc2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861770"
---
# <a name="step-7-test-directaccess-connectivity-from-the-internet"></a>ÉTAPE 7 de tester la connectivité DirectAccess à partir d’Internet

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Le déploiement de mot de passe à usage unique (OTP) DirectAccess a été testé à partir du sous-réseau de réseau domestique et peut maintenant être testé à partir d’Internet.  
  
### <a name="to-test-otp-functionality-from-the-internet-on-client1"></a>Pour tester la fonctionnalité de secret à usage unique à partir d’Internet sur CLIENT1  
  
1.  Sur CLIENT1, assurez-vous que vous êtes connecté en tant que **User1**. Connectez CLIENT1 au sous-réseau Corpnet.  
  
2.  Sur le **Démarrer** , tapez**powershell.exe**, avec le bouton droit **powershell**, cliquez sur **avancé**, puis cliquez sur **exécuter en tant qu’administrateur**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
3.  Dans la fenêtre Windows PowerShell, tapez **gpupdate /force** et appuyez sur ENTRÉE.  
  
4.  Débranchez CLIENT1 du sous-réseau de réseau domestique, se connecter à Internet et redémarrez l’ordinateur.  
  
5.  Sur CLIENT1, ouvrez Internet Explorer et dans la barre d’adresses, tapez **https://app1.corp.contoso.com/** et appuyez sur ENTRÉE. Appuyez sur F5.  
  
    N’ouvrez pas le site.  
  
6.  Sur le **Démarrer** , tapez**RSA**, puis cliquez sur **RSA SecurID jeton**.  
  
7.  Attendez que le jeton RSA SecurID modifie le mot de passe à usage unique, puis cliquez sur **copie**.  
  
8.  Cliquez sur le**connexions réseau**icône dans la zone de notification pour accéder au Gestionnaire de médias DA.  
  
9. Cliquez sur **connexion de l’espace de travail**, puis cliquez sur **continuer**.  
  
10. Appuyez sur Ctrl + Alt + Suppr, puis cliquez sur le **mot de passe à usage unique (OTP)** vignette.  
  
11. Collez le code de jeton précédemment copiés à huit chiffres, puis cliquez sur **OK**. Attendez que l’authentification. L’état de connexion d’espace de travail de DirectAccess seront désormais **connecté**.  
  
12. Dans Internet Explorer, dans la barre d’adresses, tapez **https://app1.corp.contoso.com/** et appuyez sur ENTRÉE. Appuyez sur F5. Vous allez voir le site web IIS par défaut sur APP1.  
  
13. Dans la barre d’adresses Internet Explorer, tapez **https://app2.corp.contoso.com/** et appuyez sur ENTRÉE. Appuyez sur F5. Vous verrez le site Web IIS par défaut sur APP2.  
  
14. Sur le **Démarrer** , tapez**\\\app1\files**, puis appuyez sur ENTRÉE.  
  
15. Dans le **fichiers** fenêtre de dossier partagé, double-cliquez sur le **Example.txt** fichier. Vous verrez le contenu du fichier Example.txt.  
  
16. Sur le **Démarrer** , tapez**\\\app2\files**, puis appuyez sur ENTRÉE.  
  
17. Dans le **fichiers** fenêtre de dossier partagé, double-cliquez sur le **nouveau texte seulement.txt** fichier. Vous verrez le contenu du fichier nouveau texte seulement.txt.  
  


