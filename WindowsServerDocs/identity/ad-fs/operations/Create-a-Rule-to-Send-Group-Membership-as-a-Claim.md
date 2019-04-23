---
ms.assetid: 475e34f9-9399-43f4-a840-9dd77258e11a
title: Créer une règle pour envoyer l’appartenance à un groupe en tant que revendication
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 96ab653393fbc5f0a4306db53f84c2d9ba6c7f5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847450"
---
# <a name="create-a-rule-to-send-group-membership-as-a-claim"></a>Créer une règle pour envoyer l’appartenance à un groupe en tant que revendication

>S'applique à : Windows Server 2016, Windows Server 2012 R2

À l’aide de l’appartenance au groupe Envoyer en tant qu’un modèle de règle de revendication dans Active Directory Federation Services \(AD FS\), vous pouvez créer une règle qui le rendre possible pour vous permet de sélectionner un groupe de sécurité Active Directory à envoyer en tant que revendication. Une seule demande est émise à partir de cette règle, basée sur le groupe que vous sélectionnez. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle qui envoie une revendication de groupe avec une valeur de l’administrateur si l’utilisateur est membre du groupe de sécurité Admins du domaine. Cette règle doit être utilisée uniquement pour les utilisateurs dans le domaine Active Directory local.  
  
Vous pouvez utiliser la procédure suivante pour créer une règle de revendication avec le composant logiciel enfichable Gestion AD FS\-dans.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Pour créer une règle pour envoyer l’appartenance au groupe en tant que revendication sur une confiance dans Windows Server 2016 

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **confiance**. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier la stratégie d’émission de revendication**.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Dans le **modifier la stratégie d’émission de revendication** boîte de dialogue **règles de transformation d’émission** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **envoyer l’appartenance au groupe en tant que revendication** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.   Sur le **configurer la règle** page sous **nom de règle de revendication** tapez le nom complet de cette règle, **groupe de l’utilisateur** cliquez sur **Parcourir** et sélectionnez un groupe, sous **type de revendication sortante** sélectionner le type de revendication souhaitée, puis sous **Type de revendication sortante** tapez une valeur.
![Créer la règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)   

7.  Cliquez sur le **Terminer** bouton.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.
  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer une règle pour envoyer l’appartenance au groupe en tant que revendication sur une approbation de fournisseur de revendications dans Windows Server 2016 
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de fournisseur de revendications**. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Dans le **modifier les règles de revendication** boîte de dialogue **règles de transformation d’acceptation** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **envoyer l’appartenance au groupe en tant que revendication** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.   Sur le **configurer la règle** page sous **nom de règle de revendication** tapez le nom complet de cette règle, **groupe de l’utilisateur** cliquez sur **Parcourir** et sélectionnez un groupe, sous **type de revendication sortante** sélectionner le type de revendication souhaitée, puis sous **Type de revendication sortante** tapez une valeur. 
![Créer la règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)      

7.  Cliquez sur le **Terminer** bouton.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.  




  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-in-windows-server-2012-r2"></a>Pour créer une règle pour envoyer l’appartenance au groupe en tant que revendication dans Windows Server 2012 R2 
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS\\relations d’approbation**, cliquez sur **approbations de fournisseur de revendications** ou **confiance**, puis cliquez sur un spécifique dans la liste où vous souhaitez créer cette règle d’approbation.  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez une les onglets suivants, en fonction de l’approbation que vous modifiez et quelle règle vous voulez créer cette règle dans, puis cliquez sur **ajouter une règle** pour démarrer la règle Assistant qui est associé à cet ensemble de règles :  
  
    -   **Règles de transformation d’acceptation**  
  
    -   **Règles de transformation d’émission**  
  
    -   **Règles d’autorisation d’émission**  
  
    -   **Règles d’autorisation de délégation**  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **envoyer l’appartenance au groupe en tant que revendication** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)

6.  Sur le **configurer la règle** page sous **nom de règle de revendication** tapez le nom complet de cette règle, **groupe de l’utilisateur** cliquez sur **Parcourir** et sélectionnez un groupe, sous **type de revendication sortante** sélectionner le type de revendication souhaitée, puis sous **Type de revendication sortante** tapez une valeur.  
![Créer la règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group2.PNG)  

7.  Cliquez sur **Terminer**.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.  



## <a name="additional-references"></a>Références supplémentaires 
[Configurer des règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification : Création de règles de revendication pour une partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  

[Liste de vérification : Création de règles de revendication pour un fournisseur de revendications d’approbation](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Le rôle de revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Le rôle de règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
