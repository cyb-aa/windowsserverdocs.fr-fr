---
title: Ajouter l’identification du partenaire de référence (ID POR) dans le cadre de l’accord partenaire Microsoft Online Service
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 9bd191d6-ecc5-4230-a88e-f3fc281cb956
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7a297ed077f4c1457bd1e59fc0ea22feedd5de0d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817584"
---
# <a name="add-microsoft-online-service-partner-agreement-partner-of-record-information"></a>Ajouter l’identification du partenaire de référence (ID POR) dans le cadre de l’accord partenaire Microsoft Online Service

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_3rdLevelDomanNames"></a>   
 Si vous êtes un partenaire Microsoft Online Service Partner Agreement (MOSPA) pour Office 365, pour vous assurer que vous êtes correctement compensé quand une demande d’abonnement provient de Windows Server Essentials via le module d’intégration Office 365, vous devez créer un clé de Registre qui contient votre identification de partenaire d’enregistrement (POR ID). Les informations suivantes sont lues et transmises au fournisseur de service via les URL d’inscription d’Office 365.  
  
-   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO  
  
-   Type = valeur de chaîne  
  
-   Nom de la clé = Partner  
  
-   Valeur = xxxxx, où xxxxx désigne votre ID POR  
  
#### <a name="to-add-the-por-id-key-to-the-registry"></a>Pour ajouter la clé ID POR au Registre  
  
1.  Sur l'ordinateur de référence, cliquez sur **Démarrer**, tapez **regedit**, puis appuyez sur Entrée.  
  
2.  Dans le volet gauche, développez successivement les entrées **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft** et **Windows Server**.  
  
3.  Cliquez avec le bouton droit sur **Windows Server**, pointez sur **Nouveau**, puis cliquez sur **Clé**.  
  
4.  Donnez le nom **MSO** à la clé.  
  
5.  Cliquez avec le bouton droit sur la clé que vous venez de créer, puis cliquez sur **Nom de la valeur**.  
  
6.  Tapez **Partner** comme nom de chaîne, puis appuyez sur Entrée.  
  
7.  Cliquez avec le bouton droit sur la nouvelle chaîne **Partner** dans le volet droit, puis cliquez sur **Modifier**.  
  
8.  Saisissez votre identificateur de partenaire de référence (ID POR) dans la zone de texte **Données de la valeur**, puis cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  

 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)

 [Création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](../install/Testing-the-Customer-Experience.md)

