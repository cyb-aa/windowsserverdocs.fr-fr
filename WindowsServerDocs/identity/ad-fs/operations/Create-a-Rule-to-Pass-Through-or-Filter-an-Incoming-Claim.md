---
ms.assetid: 6127963f-71b2-4d8f-8b53-7c525bf06521
title: "Créer une règle pour transmettre ou filtrer une revendication entrante"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 50f50cd4e096b107a2b58ac05328ff8ed413f2dc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-pass-through-or-filter-an-incoming-claim"></a>Créer une règle pour transmettre ou filtrer une revendication entrante

>S’applique à: Windows Server2016, Windows Server2012R2

À l’aide de la transmission ou filtre d’un modèle de règle de revendication entrante dans ActiveDirectory Federation Services \(ADFS\), vous pouvez passer par le biais de toutes les revendications entrantes avec un type de revendication sélectionné. Vous pouvez également filtrer les valeurs des revendications entrantes avec un type de revendication sélectionné. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle qui envoie toutes les revendications de groupe entrantes. Vous pouvez également utiliser cette règle pour envoyer des revendications \(UPN\) nom principal qui se terminent par utilisateur uniquement @fabrikam.  
  
Vous pouvez utiliser la procédure suivante pour créer une règle de revendication avec la gestion ADFS de composants.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Pour créer une règle pour transmettre ou filtrer une revendication entrante sur une confiance dans Windows Server2016 

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **ADFS**, cliquez sur **approbations de partie de confiance **. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  L’approbation sélectionnée clic droit, puis cliquez sur **modifier la stratégie d’émission de revendication **.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Dans le **modifier la stratégie d’émission de revendication** la boîte de dialogue sous **règles de transformation d’émission** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **passer ou filtrer une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  Sur le **configurer la règle** page sous **nom de règle de revendication** tapez le nom d’affichage pour cette règle, **type de revendication entrante** sélectionner un type de revendication la liste, puis sélectionnez une des options suivantes, selon les besoins de votre organisation:  
  
    -   **Passer toutes les valeurs de revendication**  
  
    -   **Transmettre uniquement la valeur de revendication spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui correspondent à une valeur de suffixe de messagerie spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui commencent par une valeur spécifique**  
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  Cliquez sur le **Terminer** bouton.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer une règle pour transmettre ou filtrer une revendication entrante sur une approbation de fournisseur de revendications dans Windows Server2016 
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **ADFS**, cliquez sur **approbations de fournisseur de revendications **. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  L’approbation sélectionnée clic droit, puis cliquez sur **modifier les règles de revendication **.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Dans le **modifier les règles de revendication** la boîte de dialogue sous **règles de transformation d’acceptation** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **passer ou filtrer une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  Sur le **configurer la règle** page sous **nom de règle de revendication** tapez le nom d’affichage pour cette règle, **type de revendication entrante** sélectionner un type de revendication la liste, puis sélectionnez une des options suivantes, selon les besoins de votre organisation:  
  
    -   **Passer toutes les valeurs de revendication**  
  
    -   **Transmettre uniquement la valeur de revendication spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui correspondent à une valeur de suffixe de messagerie spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui commencent par une valeur spécifique**  
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  Cliquez sur le **Terminer** bouton.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.  

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-in-windows-server-2012-r2"></a>Pour créer une règle pour transmettre ou filtrer une revendication entrante dans Windows Server2012R2

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **ADFSAD FS\\Trust relations**, cliquez sur **approbations de fournisseur de revendications** ou **approbations de partie de confiance**, puis cliquez sur une approbation spécifique dans la liste dans laquelle vous souhaitez créer cette règle.  
  
3.  L’approbation sélectionnée clic droit, puis cliquez sur **modifier les règles de revendication **.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   
  
4.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez une les onglets suivants, en fonction de l’approbation que vous modifiez et définie de règle qui vous souhaitez créer cette règle, puis cliquez sur **ajouter une règle** pour démarrer l’Assistant règle qui est associé à cet ensemble de règles:  
  
    -   **Règles de transformation d’acceptation**  
  
    -   **Règles de transformation d’émission**  
  
    -   **Règles d’autorisation d’émission**  
  
    -   **Règles d’autorisation de délégation**  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)    

5.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **passer ou filtrer une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)    

6.  Sur le **configurer la règle** page sous **nom de règle de revendication** tapez le nom d’affichage pour cette règle, **type de revendication entrante** sélectionner un type de revendication la liste, puis sélectionnez une des options suivantes, selon les besoins de votre organisation:  
  
    -   **Passer toutes les valeurs de revendication**  
  
    -   **Transmettre uniquement la valeur de revendication spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui correspondent à une valeur de suffixe de messagerie spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui commencent par une valeur spécifique**  
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule8.PNG)    

7.  Cliquez sur le **Terminer** bouton.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.  



  
## <a name="additional-references"></a>Références supplémentaires  
[Configurer des règles de revendication](Configure-Claim-Rules.md)  
  
[Quand utiliser un transfert direct ou filtrer la règle de revendication](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)  
  
[Le rôle de revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Le rôle de règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
  
