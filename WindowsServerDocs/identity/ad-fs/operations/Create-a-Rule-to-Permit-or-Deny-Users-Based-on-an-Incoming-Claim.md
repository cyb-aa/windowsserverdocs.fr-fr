---
ms.assetid: 3d770385-9834-4ebe-b66c-b684e0245971
title: Créer une règle pour autoriser ou refuser des utilisateurs en fonction d’une demande entrante
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d057c943b9c14b74b44472d446625b60f5ad9d22
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865961"
---
# <a name="create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim"></a>Créer une règle pour autoriser ou refuser des utilisateurs en fonction d’une demande entrante 


Dans Windows Server 2016, vous pouvez utiliser une **stratégie de Access Control** pour créer une règle qui autorise ou refuse des utilisateurs en fonction d’une revendication entrante.  Dans Windows Server 2012 R2, en utilisant le modèle de règle **autoriser ou refuser des utilisateurs en fonction d’une revendication entrante** dans services ADFS \(AD FS\), vous pouvez créer une règle d’autorisation qui accorde ou refuse l’accès de l’utilisateur au partie de confiance basée sur le type et la valeur d’une revendication entrante. 

Par exemple, vous pouvez l’utiliser pour créer une règle qui autorise uniquement les utilisateurs qui ont une revendication de groupe avec la valeur Admins du domaine à accéder à la partie de confiance. Si vous souhaitez autoriser tous les utilisateurs à accéder à la partie de confiance, utilisez la stratégie **autoriser tout le monde** Access Control ou le modèle de règle **autoriser tous les utilisateurs** en fonction de votre version de Windows Server. Les utilisateurs dont l’accès à la partie de confiance est autorisé par le service de fédération peuvent se voir refuser le service par la partie de confiance.  
  
Vous pouvez utiliser la procédure suivante pour créer une règle de revendication avec le composant logiciel\-enfichable de gestion AD FS.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-permit-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Pour créer une règle permettant d’autoriser les utilisateurs en fonction d’une revendication entrante sur Windows Server 2016
 
1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **Access Control stratégies**. 
![créer une règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Cliquez avec le bouton droit et sélectionnez **Ajouter une stratégie de Access Control**.
![créer une règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. Dans la zone Nom, entrez un nom pour votre stratégie, une description, puis cliquez sur **Ajouter**.
![créer une règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny5.PNG)

5. Dans l' **éditeur de règles**, sous utilisateurs, placez une coche **avec des revendications spécifiques dans la demande** , puis cliquez sur le souligné **spécifique** en bas.
![créer une règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny6.PNG)

6. Dans l' **écran Sélectionner des revendications** , cliquez sur la case d’option **revendications** , sélectionnez le **type de revendication**, l' **opérateur**et la **valeur de revendication** , puis cliquez sur **OK**.
![créer une règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny7.PNG)

7.  Dans l' **éditeur de règles** , cliquez sur **OK**.  Dans l’écran **Ajouter une stratégie de Access Control** , cliquez sur **OK**.

8. Dans l’arborescence de la console de **gestion AD FS** , sous **AD FS**, cliquez sur **approbations de partie de confiance**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Cliquez avec le bouton droit sur l' **approbation de partie de confiance** à laquelle vous souhaitez autoriser l’accès, puis sélectionnez Modifier la stratégie de **Access Control**.  
![créer une règle](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. Dans la stratégie de contrôle d’accès, sélectionnez votre stratégie, puis cliquez sur **appliquer** , puis sur **OK**.
![créer une règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny8.PNG)

## <a name="to-create-a-rule-to-deny-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Pour créer une règle pour refuser aux utilisateurs une revendication entrante sur Windows Server 2016
 
1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **Access Control stratégies**. 
![créer une règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Cliquez avec le bouton droit et sélectionnez **Ajouter une stratégie de Access Control**.
![créer une règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. Dans la zone Nom, entrez un nom pour votre stratégie, une description, puis cliquez sur **Ajouter**.
![créer une règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny9.PNG)

5. Dans l' **éditeur de règles**, assurez-vous que tout le monde est sélectionné et sous **, à l’exception** de placer une coche **avec des revendications spécifiques dans la demande** , puis cliquez sur le souligné **spécifique** en bas.
![créer une règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny10.PNG)

6. Dans l' **écran Sélectionner des revendications** , cliquez sur la case d’option **revendications** , sélectionnez le **type de revendication**, l' **opérateur**et la **valeur de revendication** , puis cliquez sur **OK**.
![créer une règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny11.PNG)

7.  Dans l' **éditeur de règles** , cliquez sur **OK**.  Dans l’écran **Ajouter une stratégie de Access Control** , cliquez sur **OK**.

8. Dans l’arborescence de la console de **gestion AD FS** , sous **AD FS**, cliquez sur **approbations de partie de confiance**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Cliquez avec le bouton droit sur l' **approbation de partie de confiance** à laquelle vous souhaitez autoriser l’accès, puis sélectionnez Modifier la stratégie de **Access Control**.  
![créer une règle](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. Dans la stratégie de contrôle d’accès, sélectionnez votre stratégie, puis cliquez sur **appliquer** , puis sur **OK**.
![créer une règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny12.PNG)

  
## <a name="to-create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim-on-windows-server-2012-r2"></a>Pour créer une règle pour autoriser ou refuser des utilisateurs en fonction d’une revendication entrante sur Windows Server 2012 R2
  
1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.    
  
2.  Dans l’arborescence de la console, sous **\\AD FS\\approbations de partie**de confiance de relations d’approbation, cliquez sur une approbation spécifique dans la liste dans laquelle vous souhaitez créer cette règle.  
  
3.  Cliquez\-avec le bouton droit sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.  
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   

4.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur l’onglet **règles d’autorisation d’émission** ou sur l’onglet \( **règles d’autorisation** de délégation\)en fonction du type de règle d’autorisation dont vous avez besoin, puis cliquez sur **Ajouter une règle.** pour démarrer l' **Assistant Ajouter une règle de revendication d’autorisation**.  
![créer une règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **autoriser ou refuser des utilisateurs en fonction d’une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny1.PNG)

6.  Dans la **page Configurer la règle** , sous nom de la règle de **revendication** , tapez le nom complet de cette règle, dans **type de revendication entrante** , sélectionnez un type de revendication dans la liste, \(sous **valeur de revendication entrante** tapez une valeur ou cliquez sur Parcourir si elle est disponible\) et sélectionnez une valeur, puis sélectionnez l’une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Autoriser l’accès aux utilisateurs avec cette revendication entrante**  
  
    -   **Refuser l’accès aux utilisateurs avec cette revendication entrante**  
![créer une règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny2.PNG)  
7.  Cliquez sur **Terminer**.  
  
8.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK** pour enregistrer la règle.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer les règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification : création de règles de revendication pour une approbation de partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Rôle des revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Rôle des règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
