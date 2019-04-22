---
ms.assetid: 8c179884-f0d9-4c7a-973d-820119cf3c38
title: Créer une règle pour autoriser tous les utilisateurs
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: de85af27e699242977054420178dd3c424b2ddb3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822590"
---
# <a name="create-a-rule-to-permit-all-users"></a>Créer une règle pour autoriser tous les utilisateurs

>S'applique à : Windows Server 2016, Windows Server 2012 R2

Dans Windows Server 2016, vous pouvez utiliser un **stratégie de contrôle d’accès** pour créer une règle qui donnera tous les utilisateurs l’accès à une partie de confiance.  Dans Windows Server 2012 R2, à l’aide de la **autoriser tous les utilisateurs** le modèle de règle dans Active Directory Federation Services \(AD FS\), vous pouvez créer une règle d’autorisation qui donnera tous les utilisateurs l’accès à la partie de confiance tiers. 

Vous pouvez utiliser des règles d’autorisation supplémentaires pour restreindre davantage l’accès. Les utilisateurs dont l’accès à la partie de confiance est autorisé par le service de fédération peuvent se voir refuser le service par la partie de confiance.  
  
Vous pouvez utiliser les procédures suivantes pour créer une règle de revendication avec le composant logiciel enfichable Gestion AD FS\-dans.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477). 

## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2016"></a>Pour créer une règle pour autoriser tous les utilisateurs dans Windows Server 2016

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **confiance**. 
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall1.PNG)

3.  Cliquez sur le **Relying Party Trust** que vous souhaitez autoriser l’accès à et sélectionnez **modifier une stratégie de contrôle d’accès**.  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

4. Sur l’accès contrôle stratégie sélectionnez **autoriser tout le monde** puis cliquez sur **appliquer** et **Ok**.
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall3.PNG)
  
## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2012-r2"></a>Pour créer une règle pour autoriser tous les utilisateurs dans Windows Server 2012 R2 
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS\\relations d’approbation\\confiance**, cliquez sur une approbation spécifique dans la liste où vous souhaitez créer cette règle.  

3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall4.PNG)  

4.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur le **les règles d’autorisation d’émission** onglet ou la **règles d’autorisation de délégation** onglet \(selon le type de règle d’autorisation que vous avez besoin de\), puis cliquez sur **ajouter une règle** pour démarrer le **Assistant de règle de revendication d’autorisation Ajouter**.  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)  
5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **autoriser tous les utilisateurs** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall6.PNG)    
6.  Sur le **configurer la règle** , cliquez sur **Terminer**.  
  
7.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer des règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification : Création de règles de revendication pour une partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Le rôle de revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Le rôle de règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
