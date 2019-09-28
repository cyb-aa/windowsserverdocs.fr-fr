---
ms.assetid: 475e34f9-9399-43f4-a840-9dd77258e11a
title: Créer une règle pour envoyer l’appartenance à un groupe en tant que revendication
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b8217302cc0ec5bc6972004cb2f26ffae1371614
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407582"
---
# <a name="create-a-rule-to-send-group-membership-as-a-claim"></a>Créer une règle pour envoyer l’appartenance à un groupe en tant que revendication

À l’aide du modèle de règle envoyer l’appartenance à un \(groupe\)en tant que revendication dans services ADFS AD FS, vous pouvez créer une règle qui vous permet de sélectionner un groupe de sécurité Active Directory à envoyer en tant que revendication. Une seule revendication est émise à partir de cette règle, en fonction du groupe que vous sélectionnez. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle qui envoie une revendication de groupe avec la valeur admin si l’utilisateur est membre du groupe de sécurité administrateurs du domaine. Cette règle doit être utilisée uniquement pour les utilisateurs du domaine local Active Directory.  
  
Vous pouvez utiliser la procédure suivante pour créer une règle de revendication avec le composant logiciel\-enfichable de gestion AD FS.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Pour créer une règle pour envoyer l’appartenance à un groupe en tant que revendication sur une approbation de partie de confiance dans Windows Server 2016 

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de partie de confiance**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Cliquez\-avec le bouton droit sur l’approbation sélectionnée, puis cliquez sur **modifier la stratégie d’émission de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Dans la boîte de dialogue **modifier la stratégie d’émission de revendication** , sous règles de transformation d' **émission** , cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **Envoyer l’appartenance à un groupe en tant que revendication** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.   Dans la **page Configurer la règle** , sous nom de la règle de **revendication** , tapez le nom complet de cette règle, dans **groupe de l’utilisateur** , cliquez sur **Parcourir** et sélectionnez un groupe, sous **type de revendication sortante** , sélectionnez le type de revendication souhaité, puis sous  **Type de revendication sortante** tapez une valeur.
![créer une règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)   

7.  Cliquez sur le bouton **Terminer** .  
  
8.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK** pour enregistrer la règle.
  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer une règle pour envoyer l’appartenance à un groupe en tant que revendication sur une approbation de fournisseur de revendications dans Windows Server 2016 
  
1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de fournisseur de revendications**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Cliquez\-avec le bouton droit sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Dans la boîte de dialogue **modifier les règles de revendication** , sous règles de transformation d' **acceptation** , cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **Envoyer l’appartenance à un groupe en tant que revendication** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.   Dans la **page Configurer la règle** , sous nom de la règle de **revendication** , tapez le nom complet de cette règle, dans **groupe de l’utilisateur** , cliquez sur **Parcourir** et sélectionnez un groupe, sous **type de revendication sortante** , sélectionnez le type de revendication souhaité, puis sous  **Type de revendication sortante** tapez une valeur. 
![créer une règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)      

7.  Cliquez sur le bouton **Terminer** .  
  
8.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK** pour enregistrer la règle.  




  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-in-windows-server-2012-r2"></a>Pour créer une règle pour envoyer l’appartenance à un groupe en tant que revendication dans Windows Server 2012 R2 
  
1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **\\AD FS relations d’approbation**, cliquez sur approbations de fournisseur de **revendications** ou **approbations de partie de confiance**, puis cliquez sur une approbation spécifique dans la liste dans laquelle vous souhaitez créer cette règle.  
  
3.  Cliquez\-avec le bouton droit sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  Dans la boîte de dialogue **modifier les règles de revendication** , sélectionnez l’un des onglets suivants, en fonction de l’approbation que vous modifiez et de l’ensemble de règles dans lequel vous souhaitez créer cette règle, puis cliquez sur Ajouter une **règle** pour démarrer l’Assistant règle associé à cet ensemble de règles. :  
  
    -   **Règles de transformation d’acceptation**  
  
    -   **Règles de transformation d’émission**  
  
    -   **Règles d’autorisation d’émission**  
  
    -   **Règles d’autorisation de délégation**  
![créer une règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **Envoyer l’appartenance à un groupe en tant que revendication** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)

6.  Dans la **page Configurer la règle** , sous nom de la règle de **revendication** , tapez le nom complet de cette règle, dans **groupe de l’utilisateur** , cliquez sur **Parcourir** et sélectionnez un groupe, sous **type de revendication sortante** , sélectionnez le type de revendication souhaité, puis sous  **Type de revendication sortante** tapez une valeur.  
![créer une règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group2.PNG)  

7.  Cliquez sur **Terminer**.  
  
8.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK** pour enregistrer la règle.  



## <a name="additional-references"></a>Références supplémentaires 
[Configurer les règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification : création de règles de revendication pour une approbation de partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  

[Liste de vérification : création de règles de revendication pour une approbation de fournisseur de revendications](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Rôle des revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Rôle des règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
