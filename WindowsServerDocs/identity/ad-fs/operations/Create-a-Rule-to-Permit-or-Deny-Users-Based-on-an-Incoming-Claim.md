---
ms.assetid: 3d770385-9834-4ebe-b66c-b684e0245971
title: "Créer une règle pour autoriser ou refuser les utilisateurs selon une revendication entrante"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: afcbb8c7a08a84eda2a794c9565061d7d61d470b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim"></a>Créer une règle pour autoriser ou refuser les utilisateurs selon une revendication entrante 

>S’applique à: Windows Server2016, Windows Server2012R2

Dans Windows Server2016, vous pouvez utiliser un **stratégie de contrôle d’accès** pour créer une règle qui autorise de refuser des utilisateurs en fonction d’une revendication entrante.  Dans Windows Server2012R2, à l’aide de la **autoriser ou refuser les utilisateurs selon une revendication entrante** modèle de règle dans ActiveDirectory Federation Services \(ADFS\), vous pouvez créer une règle d’autorisation qui s’accorder ou refuser l’accès de l’utilisateur à la partie de confiance selon le type et la valeur d’une revendication entrante. 

Par exemple, vous pouvez l’utiliser pour créer une règle qui autorise uniquement les utilisateurs qui ont un groupe de revendication avec la valeur Admins du domaine pour accéder à la partie de confiance. Si vous souhaitez autoriser tous les utilisateurs d’accéder à la partie de confiance, utilisez le **autoriser tout le monde** stratégie de contrôle d’accès ou le **autoriser tous les utilisateurs** modèle de règle selon votre version de Windows Server. Les utilisateurs qui sont autorisés à accéder à la partie de confiance à partir du Service de fédération peuvent se voir refuser le service par la partie de confiance.  
  
Vous pouvez utiliser la procédure suivante pour créer une règle de revendication avec la gestion ADFS de composants.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-permit-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Pour créer une règle pour autoriser les utilisateurs en fonction d’une revendication entrante sur Windows Server2016
 
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **ADFS**, cliquez sur **stratégies de contrôle d’accès **. 
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Avec le bouton droit et sélectionnez **ajouter une stratégie de contrôle d’accès **.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. Dans la zone Nom, entrez un nom pour votre stratégie, une description et un clic **ajouter **.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny5.PNG)

5. Sur le **règle éditeur**, sous utilisateurs, placez une case à cocher **avec les revendications spécifiques dans la demande** et cliquez sur le texte souligné **spécifique** en bas.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny6.PNG)

6. Sur le **revendications sélectionnez**, cliquez sur le **revendications** case d’option, sélectionnez le **type de revendication**, le **opérateur**et le **valeur de revendication** puis cliquez sur **Ok **.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny7.PNG)

7.  Sur le **règle éditeur** cliquez sur **Ok **.  Sur le **ajouter une stratégie de contrôle d’accès**, cliquez sur **Ok **.

8. Dans le **gestion ADFS** arborescence, de la console sous **ADFS**, cliquez sur **approbations de partie de confiance **. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Avec le bouton droit le **confiance** que vous souhaitez autoriser l’accès à et sélectionnez **modifier une stratégie de contrôle d’accès **.  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. Dans la stratégie de contrôle d’accès, sélectionnez votre stratégie, puis sur **appliquer** et **Ok **.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny8.PNG)

## <a name="to-create-a-rule-to-deny-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Pour créer une règle pour refuser des utilisateurs en fonction d’une revendication entrante sur Windows Server2016
 
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **ADFS**, cliquez sur **stratégies de contrôle d’accès **. 
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Avec le bouton droit et sélectionnez **ajouter une stratégie de contrôle d’accès **.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. Dans la zone Nom, entrez un nom pour votre stratégie, une description et un clic **ajouter **.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny9.PNG)

5. Sur le **règle éditeur**, vérifiez que tout le monde est sélectionné et, sous **sauf** placer la case à cocher **avec les revendications spécifiques dans la demande** et cliquez sur le texte souligné **spécifique** en bas.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny10.PNG)

6. Sur le **revendications sélectionnez**, cliquez sur le **revendications** case d’option, sélectionnez le **type de revendication**, le **opérateur**et le **valeur de revendication** puis cliquez sur **Ok **.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny11.PNG)

7.  Sur le **règle éditeur** cliquez sur **Ok **.  Sur le **ajouter une stratégie de contrôle d’accès**, cliquez sur **Ok **.

8. Dans le **gestion ADFS** arborescence, de la console sous **ADFS**, cliquez sur **approbations de partie de confiance **. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Avec le bouton droit le **confiance** que vous souhaitez autoriser l’accès à et sélectionnez **modifier une stratégie de contrôle d’accès **.  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. Dans la stratégie de contrôle d’accès, sélectionnez votre stratégie, puis sur **appliquer** et **Ok **.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny12.PNG)

  
## <a name="to-create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim-on-windows-server-2012-r2"></a>Pour créer une règle pour autoriser ou refuser des utilisateurs en fonction d’une revendication entrante sur Windows Server2012R2
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.    
  
2.  Dans l’arborescence de la console, sous **ADFS\\Trust Relationships\\Relying approbations**, cliquez sur une approbation spécifique dans la liste dans laquelle vous souhaitez créer cette règle.  
  
3.  L’approbation sélectionnée clic droit, puis cliquez sur **modifier les règles de revendication **.  
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   

4.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur le **les règles d’autorisation d’émission** onglet ou le **règles d’autorisation de délégation** onglet \ (selon le type de règle d’autorisation vous require\), puis cliquez sur **ajouter une règle** pour démarrer le **Assistant de règle de revendication d’autorisation d’ajouter **.  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **autoriser ou refuser les utilisateurs en fonction une revendication entrante** dans la liste, puis cliquez sur **suivant **.  
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny1.PNG)

6.  Sur le **configurer la règle** page sous **nom de règle de revendication** tapez le nom d’affichage pour cette règle, **type de revendication entrante** sélectionner un type de revendication la liste, sous **valeur de revendication entrante** tapez une valeur ou cliquez sur Parcourir \ (s’il est available\) et sélectionnez une valeur, puis sélectionnez une des options suivantes, selon les besoins de votre organisation:  
  
    -   **Autoriser l’accès aux utilisateurs avec cette revendication entrante**  
  
    -   **Refuser l’accès aux utilisateurs avec cette revendication entrante**  
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny2.PNG)  
7.  Cliquez sur **Terminer**.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer des règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification: Création de règles de revendication pour une partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Le rôle de revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Le rôle de règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
