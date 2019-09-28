---
ms.assetid: 8c179884-f0d9-4c7a-973d-820119cf3c38
title: Créer une règle pour autoriser tous les utilisateurs
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1356218c5f9f47073f007286e8acfdf4c3608b73
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407626"
---
# <a name="create-a-rule-to-permit-all-users"></a>Créer une règle pour autoriser tous les utilisateurs

Dans Windows Server 2016, vous pouvez utiliser une **stratégie de Access Control** pour créer une règle qui donne à tous les utilisateurs l’accès à une partie de confiance.  Dans Windows Server 2012 R2, à l’aide du modèle de règle **autoriser tous les utilisateurs** dans services ADFS \(AD FS @ no__t-2, vous pouvez créer une règle d’autorisation qui donne à tous les utilisateurs l’accès à la partie de confiance. 

Vous pouvez utiliser des règles d’autorisation supplémentaires pour restreindre davantage l’accès. Les utilisateurs dont l’accès à la partie de confiance est autorisé par le service de fédération peuvent se voir refuser le service par la partie de confiance.  
  
Vous pouvez utiliser les procédures suivantes pour créer une règle de revendication avec le composant logiciel enfichable de gestion AD FS no__t-0in.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477). 

## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2016"></a>Pour créer une règle pour autoriser tous les utilisateurs dans Windows Server 2016

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de partie de confiance**. 
![créer une règle](media/Create-a-Rule-to-Permit-All-Users/permitall1.PNG)

3.  Cliquez avec le bouton droit sur l' **approbation de partie de confiance** à laquelle vous souhaitez autoriser l’accès, puis sélectionnez Modifier la stratégie de **Access Control**.  
![créer une règle](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

4. Dans la stratégie de contrôle d’accès, sélectionnez **autoriser tout le monde** , puis cliquez sur **appliquer** , puis sur **OK**.
![créer une règle](media/Create-a-Rule-to-Permit-All-Users/permitall3.PNG)
  
## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2012-r2"></a>Pour créer une règle pour autoriser tous les utilisateurs dans Windows Server 2012 R2 
  
1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **\\AD FS\\approbations de partie**de confiance de relations d’approbation, cliquez sur une approbation spécifique dans la liste dans laquelle vous souhaitez créer cette règle.  

3.  Cliquez\-avec le bouton droit sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.  
![créer une règle](media/Create-a-Rule-to-Permit-All-Users/permitall4.PNG)  

4.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur l’onglet **règles d’autorisation d’émission** ou sur l’onglet \( **règles d’autorisation** de délégation\)en fonction du type de règle d’autorisation dont vous avez besoin, puis cliquez sur **Ajouter une règle.** pour démarrer l' **Assistant Ajouter une règle de revendication d’autorisation**.  
![créer une règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)  
5.  Dans la **page Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **autoriser tous les utilisateurs** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Permit-All-Users/permitall6.PNG)    
6.  Dans la page **configurer la règle** , cliquez sur **Terminer**.  
  
7.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK** pour enregistrer la règle.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer les règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification : création de règles de revendication pour une approbation de partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Rôle des revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Rôle des règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
