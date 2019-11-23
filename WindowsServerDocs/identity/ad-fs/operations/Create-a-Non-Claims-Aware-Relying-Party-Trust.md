---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: Créer une approbation de partie de confiance qui ne prend pas en charge les revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b0ea877170a07db6abe9ac82e72d1722600ec933
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358110"
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>Créer une approbation de partie de confiance ne prenant pas en charge les revendications


Dans le\-du composant logiciel enfichable de gestion de AD FS, les revendications non\-s\-les approbations de partie de confiance sont des objets qui sont créés pour représenter l’approbation entre le service de Fédération et une application Web\-unique qui ne prend pas en charge les revendications\-et qui est accessible via le proxy d’application Web.  
  
Une approbation de partie de confiance qui n’est pas\-revendications\-est une approbation de partie de confiance qui se compose d’identificateurs, de noms et de règles d’authentification et d’autorisation lors de l’accès à l’approbation de la partie de confiance par le biais du proxy d’application Web. Ces applications Web\-qui ne s’appuient pas sur des revendications, en d’autres termes, ces applications intégrées\-basées sur l’authentification Windows, peuvent avoir des règles d’autorisation qui appliquent l’accès basé sur des revendications lorsque l’accès est externe au réseau d’entreprise via le proxy d’application Web.  
  
Pour ajouter une nouvelle approbation de partie de confiance non\-revendications\-, à l’aide du\-du composant logiciel enfichable de gestion AD FS dans, effectuez la procédure suivante.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>Pour créer manuellement une approbation de partie de confiance ne prenant pas en charge les revendications 
1. Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Sous **actions**, cliquez sur **Ajouter une approbation de partie de confiance**.  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Sur la page **Bienvenue** , choisissez prise en **charge des revendications non** , puis cliquez sur **Démarrer**.  
![partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon1.PNG) 
  
4.  Dans la page **spécifier le nom complet** , tapez un nom dans **nom complet**, sous **Remarques** , tapez une description pour cette approbation de partie de confiance, puis cliquez sur **suivant**.  
![partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon2.PNG)

5. Dans la page **Configurer les identificateurs**, spécifiez un ou plusieurs identificateurs pour cette partie de confiance, cliquez sur **Ajouter** pour les ajouter à la liste, puis cliquez sur **Suivant**.  
![partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon3.PNG)

6.  Dans la **stratégie choisir un Access Control** , sélectionnez une stratégie, puis cliquez sur **suivant**.  Pour plus d’informations sur les stratégies de Access Control, consultez [Access Control des stratégies dans AD FS](Access-Control-Policies-in-AD-FS.md). 
![partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon4.PNG)

7. Dans la page **Prêt à ajouter l'approbation**, vérifiez les paramètres, puis cliquez sur **Suivant** pour enregistrer les informations de la nouvelle approbation de partie de confiance.  
   ![partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon5.PNG) 

8. Dans la page **Terminer** , cliquez sur **Fermer**. Cette action affiche automatiquement la boîte de dialogue **Modifier les règles de revendication**.  
![partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon6.PNG)  
  
## <a name="see-also"></a>Voir aussi  
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
