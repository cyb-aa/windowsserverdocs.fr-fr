---
title: "Ajouter des informations de MicrosoftOnline Service partenaire contrat partenaire d’enregistrement"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9bd191d6-ecc5-4230-a88e-f3fc281cb956
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 39ce43228cd7392bcc86de4a410c52676ce15047
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="add-microsoft-online-service-partner-agreement-partner-of-record-information"></a>Ajouter des informations de MicrosoftOnline Service partenaire contrat partenaire d’enregistrement

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

##  <a name="BKMK_3rdLevelDomanNames"></a>   
 Si vous êtes un partenaire MicrosoftOnline Service Partner Agreement (MOSPA) pour Office 365, vous devez créer une clé de Registre contenant votre partenaire de (référence POR ID) pour vous assurer que programme une demande d’abonnement émanant de WindowsServerEssentials via le Module d’intégration d’Office 365. Les informations suivantes sont lu et transmises au fournisseur de service par le biais de l’URL d’abonnement à Office 365.  
  
-   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO  
  
-   Type = valeur de chaîne  
  
-   Nom de la clé = Partner  
  
-   Valeur = xxxxx, où xxxxx désigne votre ID POR  
  
#### <a name="to-add-the-por-id-key-to-the-registry"></a>Pour ajouter la clé ID POR au Registre  
  
1.  Sur l’ordinateur de référence, cliquez sur **Démarrer**, type **regedit**, puis appuyez sur ENTRÉE.  
  
2.  Dans le volet gauche, développez **HKEY_LOCAL_MACHINE**, développez **logiciel**, développez **Microsoft**, puis développez **Windows Server**.  
  
3.  Avec le bouton droit **Windows Server**, pointez sur **New**, puis cliquez sur **clé**.  
  
4.  Type **MSO** pour le nom de la clé.  
  
5.  Cliquez sur la clé que vous venez de créé, puis cliquez sur **valeur de chaîne**.  
  
6.  Type **partenaire** pour le nom de chaîne, puis appuyez sur ENTRÉE.  
  
7.  Cliquez sur le nouveau **partenaire** chaîne dans le volet droit, puis cliquez sur **modifier**.  
  
8.  Entrez votre ID POR dans le **données de la valeur** zone de texte, puis cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  

 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)

 [Création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](../install/Testing-the-Customer-Experience.md)

