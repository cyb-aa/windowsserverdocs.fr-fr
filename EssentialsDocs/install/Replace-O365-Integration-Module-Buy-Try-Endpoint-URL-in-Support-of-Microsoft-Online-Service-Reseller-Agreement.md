---
title: Remplacer l’URL du point d’arrêt d’achat/évaluation du module d’intégration O365 dans le cadre de l’accord partenaire Microsoft Online Service
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9860a6b9-baea-4bf0-9a9f-6f1a288f996e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 11e18d7d61edb0a618fb71791f77c4df84c9d708
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311542"
---
# <a name="replace-o365-integration-module-buy-try-endpoint-url-in-support-of-microsoft-online-service-reseller-agreement"></a>Remplacer l’URL du point d’arrêt d’achat/évaluation du module d’intégration O365 dans le cadre de l’accord partenaire Microsoft Online Service

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_O365"></a>   
 Si vous êtes un partenaire MOSRA (Microsoft Online Service Reseller Agreement), pour vous assurer que les transactions d’inscription client sont traitées via votre portail, vous devez remplacer les URL de point de terminaison utilisées par le module d’intégration de Windows Server Essentials Office 365.  
  
 Le module d’intégration utilise les quatre URL de point d’arrêt suivantes :  
  
1.  Un point d’arrêt d’achat d’abonnement Office 365 Enterprise.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Type = REG-SZ  
  
    -   Nom de la clé = MOSRASTDBUY  
  
    -   Valeur = *xxxxx*, où xxxxx représente l’URL d’évaluation d’abonnement de votre entreprise. Par exemple, valeur = http://syndicatepartner.office365.com/enterprisebuy.html  
  
2.  Un point d’arrêt d’évaluation d’abonnement Office 365 Enterprise.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Type = REG-SZ  
  
    -   Nom de la clé = MOSRASTDTRY  
  
    -   Valeur = *xxxxx*, où xxxxx représente l’URL d’évaluation d’abonnement de votre entreprise. Par exemple, valeur = http://syndicatepartner.office365.com/enterprisetry.html  
  
3.  Un point de terminaison d’achat d’abonnement Office 365 Small Business Premium.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Type = REG-SZ  
  
    -   Nom de la clé = MOSRALITEBUY  
  
    -   Valeur = *xxxxx*, où xxxxx représente l’URL d’évaluation d’abonnement de votre entreprise. Par exemple, valeur = http://syndicatepartner.office365.com/smallbizbuy.html  
  
4.  Un point de terminaison d’abonnement Office 365 Small Business Premium.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Type = REG-SZ  
  
    -   Nom de la clé = MOSRALITETRY  
  
    -   Valeur = *xxxxx*, où xxxxx représente l’URL d’évaluation d’abonnement de votre entreprise. Par exemple, valeur = http://syndicatepartner.office365.com/smallbiztry.html  
  
#### <a name="to-add-an-endpoint-url-key-to-the-registry"></a>Pour ajouter une clé d’URL de point d’arrêt au Registre  
  
1.  Sur l'ordinateur de référence, cliquez sur **Démarrer**, tapez **regedit**, puis appuyez sur Entrée.  
  
2.  Dans le volet gauche, développez successivement les entrées **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, **Windows Server** et **MSO**.  
  
3.  Si l'entrée MSO n'existe pas, cliquez avec le bouton droit sur **Windows Server**, pointez sur **Nouveau**, cliquez sur **Clé**, puis tapez **MSO** en guise de nom de clé.  
  
4.  Cliquez avec le bouton droit sur MSO, puis cliquez sur **Nom de la valeur**. Entrez l’un des noms de chaîne de point d’arrêt suivants en guise de nom de chaîne :  
  
    -   MOSRASTDBUY  
  
    -   MOSRASTDTRY  
  
    -   MOSRALITEBUY  
  
    -   MOSRALITETRY  
  
5.  Cliquez avec le bouton droit sur la nouvelle chaîne dans le volet droit, puis cliquez sur **Modifier**.  
  
6.  Saisissez la nouvelle URL de point d’arrêt dans le champ **Données de la valeur**, puis cliquez sur **OK**.  
  
7.  Répétez les étapes 4 à 6 pour chaque nom de chaîne énuméré à l’étape 4.  
  
## <a name="see-also"></a>Voir aussi  

 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md) [création et personnalisation de l’image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](../install/Testing-the-Customer-Experience.md)

