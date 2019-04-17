---
title: "Remplacement de l’URL de point de terminaison O365 intégration Module acheter-essayer à l’appui de contrat de revendeur MicrosoftOnline Service"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9860a6b9-baea-4bf0-9a9f-6f1a288f996e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b690cedd2f692cc6d11af6e05dd0cd4b4ea5a1d6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="replace-o365-integration-module-buy-try-endpoint-url-in-support-of-microsoft-online-service-reseller-agreement"></a>Remplacement de l’URL de point de terminaison O365 intégration Module acheter-essayer à l’appui de contrat de revendeur MicrosoftOnline Service

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

##  <a name="BKMK_O365"></a>   
 Si vous êtes un partenaire MicrosoftOnline Service revendeur accord (MOSRA), pour vous assurer que mosra est traitées via votre portail, vous devez remplacer les URL de point de terminaison utilisés par le module d’intégration WindowsServerEssentialsOffice 365.  
  
 Le module d’intégration utilise les quatre URL de point de terminaison suivantes:  
  
1.  Un point de terminaison Office 365 Enterprise abonnement achat.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Type = REG-SZ  
  
    -   Nom de la clé = MOSRASTDBUY  
  
    -   Valeur = *xxxxx*, où xxxxx représente l’URL achat abonnement de votre entreprise. Par exemple, valeur = http://syndicatepartner.office365.com/enterprisebuy.html  
  
2.  Un point de d’évaluation de l’abonnement Office 365 Enterprise.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Type = REG-SZ  
  
    -   Nom de la clé = MOSRASTDTRY  
  
    -   Valeur = *xxxxx*, où xxxxx représente l’URL achat abonnement de votre entreprise. Par exemple, valeur = http://syndicatepartner.office365.com/enterprisetry.html  
  
3.  Un point de terminaison Office 365 petite entreprise Premium abonnement achat.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Type = REG-SZ  
  
    -   Nom de la clé = MOSRALITEBUY  
  
    -   Valeur = *xxxxx*, où xxxxx représente l’URL achat abonnement de votre entreprise. Par exemple, valeur = http://syndicatepartner.office365.com/smallbizbuy.html  
  
4.  Un point de version d’évaluation de l’abonnement Office 365 petite entreprise Premium.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Type = REG-SZ  
  
    -   Nom de la clé = MOSRALITETRY  
  
    -   Valeur = *xxxxx*, où xxxxx représente l’URL achat abonnement de votre entreprise. Par exemple, valeur = http://syndicatepartner.office365.com/smallbiztry.html  
  
#### <a name="to-add-an-endpoint-url-key-to-the-registry"></a>Pour ajouter une clé d’URL de point de terminaison dans le Registre  
  
1.  Sur l’ordinateur de référence, cliquez sur **Démarrer**, type **regedit**, puis appuyez sur ENTRÉE.  
  
2.  Dans le volet gauche, développez **HKEY_LOCAL_MACHINE**, développez **logiciel**, développez **Microsoft**, développez **Windows Server**, puis développez **MSO**.  
  
3.  Si MSO n’existe pas, faites un clic droit **Windows Server**, pointez sur **New**, cliquez sur **clé**, puis tapez **MSO** pour le nom de la clé.  
  
4.  Avec le bouton droit MSO, puis cliquez sur **valeur de chaîne**. Tapez un des noms de chaîne de point de terminaison suivants pour le nom de la chaîne:  
  
    -   MOSRASTDBUY  
  
    -   MOSRASTDTRY  
  
    -   MOSRALITEBUY  
  
    -   MOSRALITETRY  
  
5.  Avec le bouton droit de la nouvelle chaîne dans le volet droit, puis cliquez sur **modifier**.  
  
6.  Tapez la nouvelle URL de point de terminaison dans la **données de la valeur** zone de texte, puis cliquez sur **OK**.  
  
7.  Répétez les étapes4 à 6 pour chaque nom de chaîne énuméré à l’étape4.  
  
## <a name="see-also"></a>Voir aussi  

 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)[création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](../install/Testing-the-Customer-Experience.md)

