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
ms.openlocfilehash: 167e43d49c08d0e39549bf46888118f985e3876d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863770"
---
# <a name="create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim"></a>Créer une règle pour autoriser ou refuser des utilisateurs en fonction d’une demande entrante 

>S'applique à : Windows Server 2016, Windows Server 2012 R2

Dans Windows Server 2016, vous pouvez utiliser un **stratégie de contrôle d’accès** pour créer une règle qui autorise ou refuse les utilisateurs basés sur une revendication entrante.  Dans Windows Server 2012 R2, à l’aide de la **autoriser ou refuser les utilisateurs en fonction d’une revendication entrante** le modèle de règle dans Active Directory Federation Services \(AD FS\), vous pouvez créer une règle d’autorisation qui accordera ou refuser l’accès de l’utilisateur à la partie de confiance selon le type et la valeur d’une revendication entrante. 

Par exemple, vous pouvez l’utiliser pour créer une règle qui autorise uniquement les utilisateurs qui ont un groupe avec une valeur du groupe Admins du domaine à accéder à la confiance de revendication. Si vous souhaitez autoriser tous les utilisateurs pour accéder à la partie de confiance, utilisez le **autoriser tout le monde** stratégie de contrôle d’accès ou le **autoriser tous les utilisateurs** le modèle de règle selon votre version de Windows Server. Les utilisateurs dont l’accès à la partie de confiance est autorisé par le service de fédération peuvent se voir refuser le service par la partie de confiance.  
  
Vous pouvez utiliser la procédure suivante pour créer une règle de revendication avec le composant logiciel enfichable Gestion AD FS\-dans.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-permit-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Pour créer une règle pour autoriser les utilisateurs basés sur une revendication entrante sur Windows Server 2016
 
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **stratégies de contrôle d’accès**. 
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Avec le bouton droit et sélectionnez **ajouter une stratégie de contrôle d’accès**.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. Dans la zone Nom, entrez un nom pour votre stratégie, d’une description et cliquez sur **ajouter**.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny5.PNG)

5. Sur le **éditeur de règles**, sous les utilisateurs, cochez la case **avec des revendications spécifiques dans la demande** et cliquez sur le texte souligné **spécifique** en bas.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny6.PNG)

6. Sur le **revendications sélectionnez** , cliquez sur le **revendications** bouton radio, sélectionnez le **type de revendication**, le **opérateur**et le  **Valeur de revendication** puis cliquez sur **Ok**.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny7.PNG)

7.  Sur le **éditeur de règles** cliquez sur **Ok**.  Sur le **ajouter une stratégie de contrôle d’accès** , cliquez sur **Ok**.

8. Dans le **gestion AD FS** arborescence, sous **AD FS**, cliquez sur **confiance**. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Cliquez sur le **Relying Party Trust** que vous souhaitez autoriser l’accès à et sélectionnez **modifier une stratégie de contrôle d’accès**.  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. Dans la stratégie de contrôle d’accès, sélectionnez votre stratégie, puis sur **appliquer** et **Ok**.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny8.PNG)

## <a name="to-create-a-rule-to-deny-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Pour créer une règle pour refuser des utilisateurs en fonction d’une revendication entrante sur Windows Server 2016
 
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **stratégies de contrôle d’accès**. 
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Avec le bouton droit et sélectionnez **ajouter une stratégie de contrôle d’accès**.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. Dans la zone Nom, entrez un nom pour votre stratégie, d’une description et cliquez sur **ajouter**.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny9.PNG)

5. Sur le **éditeur de règles**, assurez-vous que tout le monde est sélectionné puis, sous **sauf** Cochez la case **avec des revendications spécifiques dans la demande** et cliquez sur le texte souligné  **spécifique** en bas.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny10.PNG)

6. Sur le **revendications sélectionnez** , cliquez sur le **revendications** bouton radio, sélectionnez le **type de revendication**, le **opérateur**et le  **Valeur de revendication** puis cliquez sur **Ok**.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny11.PNG)

7.  Sur le **éditeur de règles** cliquez sur **Ok**.  Sur le **ajouter une stratégie de contrôle d’accès** , cliquez sur **Ok**.

8. Dans le **gestion AD FS** arborescence, sous **AD FS**, cliquez sur **confiance**. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Cliquez sur le **Relying Party Trust** que vous souhaitez autoriser l’accès à et sélectionnez **modifier une stratégie de contrôle d’accès**.  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. Dans la stratégie de contrôle d’accès, sélectionnez votre stratégie, puis sur **appliquer** et **Ok**.
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny12.PNG)

  
## <a name="to-create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim-on-windows-server-2012-r2"></a>Pour créer une règle pour autoriser ou refuser des utilisateurs basés sur une revendication entrante sur Windows Server 2012 R2
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.    
  
2.  Dans l’arborescence de la console, sous **AD FS\\relations d’approbation\\confiance**, cliquez sur une approbation spécifique dans la liste où vous souhaitez créer cette règle.  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.  
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   

4.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur le **les règles d’autorisation d’émission** onglet ou la **règles d’autorisation de délégation** onglet \(selon le type de règle d’autorisation que vous avez besoin de\), puis cliquez sur **ajouter une règle** pour démarrer le **Assistant de règle de revendication d’autorisation Ajouter**.  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **autoriser ou refuser les utilisateurs en fonction d’une revendication entrante** dans la liste, puis cliquez sur  **Suivant**.  
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny1.PNG)

6.  Sur le **configurer la règle** page sous **nom de règle de revendication** tapez le nom complet de cette règle, **type de revendication entrante** sélectionner un type de revendication dans la liste, sous  **Valeur de revendication entrante** taper une valeur ou cliquez sur Parcourir \(s’il est disponible\) et sélectionnez une valeur, puis sélectionnez une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Autoriser l’accès aux utilisateurs avec cette revendication entrante**  
  
    -   **Refuser l’accès aux utilisateurs avec cette revendication entrante**  
![Créer la règle](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny2.PNG)  
7.  Cliquez sur **Terminer**.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer des règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification : Création de règles de revendication pour une partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Le rôle de revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Le rôle de règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
