---
ms.assetid: 8c179884-f0d9-4c7a-973d-820119cf3c38
title: "Créer une règle pour autoriser tous les utilisateurs"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: de85af27e699242977054420178dd3c424b2ddb3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-permit-all-users"></a>Créer une règle pour autoriser tous les utilisateurs

>S’applique à: Windows Server2016, Windows Server2012R2

Dans Windows Server2016, vous pouvez utiliser un **stratégie de contrôle d’accès** pour créer une règle qui s’accorder tous les utilisateurs l’accès à une partie de confiance.  Dans Windows Server2012R2, à l’aide de la **autoriser tous les utilisateurs** modèle de règle dans ActiveDirectory Federation Services \(ADFS\), vous pouvez créer une règle d’autorisation qui s’accorder tous les utilisateurs l’accès à la partie de confiance. 

Vous pouvez utiliser des règles d’autorisation supplémentaires pour restreindre davantage l’accès. Les utilisateurs qui sont autorisés à accéder à la partie de confiance à partir du Service de fédération peuvent se voir refuser le service par la partie de confiance.  
  
Vous pouvez utiliser les procédures suivantes pour créer une règle de revendication avec la gestion ADFS de composants.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477). 

## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2016"></a>Pour créer une règle pour autoriser tous les utilisateurs dans Windows Server2016

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **ADFS**, cliquez sur **approbations de partie de confiance **. 
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall1.PNG)

3.  Avec le bouton droit le **confiance** que vous souhaitez autoriser l’accès à et sélectionnez **modifier une stratégie de contrôle d’accès **.  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

4. Sur l’accès au contrôle sélectionnez stratégie **autoriser tout le monde** puis cliquez sur **appliquer** et **Ok **.
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall3.PNG)
  
## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2012-r2"></a>Pour créer une règle pour autoriser tous les utilisateurs dans Windows Server2012R2 
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **ADFS\\Trust Relationships\\Relying approbations**, cliquez sur une approbation spécifique dans la liste dans laquelle vous souhaitez créer cette règle.  

3.  L’approbation sélectionnée clic droit, puis cliquez sur **modifier les règles de revendication **.  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall4.PNG)  

4.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur le **les règles d’autorisation d’émission** onglet ou le **règles d’autorisation de délégation** onglet \ (selon le type de règle d’autorisation vous require\), puis cliquez sur **ajouter une règle** pour démarrer le **Assistant de règle de revendication d’autorisation d’ajouter **.  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)  
5.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **autoriser tous les utilisateurs** dans la liste, puis cliquez sur **suivant **.  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall6.PNG)    
6.  Sur le **configurer la règle**, cliquez sur **Terminer **.  
  
7.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer des règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification: Création de règles de revendication pour une partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Le rôle de revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Le rôle de règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
