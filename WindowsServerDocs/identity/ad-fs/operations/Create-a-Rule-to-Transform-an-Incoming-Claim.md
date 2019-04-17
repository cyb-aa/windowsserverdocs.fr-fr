---
ms.assetid: ef83960f-d2cf-441f-b2b6-d97822ec7149
title: "Créer une règle pour transformer une revendication entrante"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e982b7608f7602268657ceae74f641bbaaaec939
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-transform-an-incoming-claim"></a>Créer une règle pour transformer une revendication entrante

>S’applique à: Windows Server2016, Windows Server2012R2

À l’aide de la **transformer une revendication entrante** modèle de règle dans ActiveDirectory Federation Services \(ADFS\), vous pouvez sélectionner une revendication entrante, modifiez le type de revendication et modifier sa valeur de revendication. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle qui envoie une revendication de rôle avec la même valeur de revendication d’une revendication entrante de groupe. Vous pouvez également utiliser cette règle pour envoyer un groupe de revendication avec une valeur de revendication de l’acheteur lorsqu’il existe une revendication entrante de groupe avec la valeur administrateurs, ou vous pouvez envoyer uniquement nom d’utilisateur principal qui se terminent par des revendications \(UPN\)@fabrikam.  
  
Vous pouvez utiliser la procédure suivante pour créer une règle de revendication avec la gestion ADFS de composants.  
  
L’appartenance au groupe **administrateurs**, ou équivalent, sur l’ordinateur local est le minimum requis pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477). 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Pour créer une règle pour transformer une revendication entrante sur une confiance dans Windows Server2016 

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **ADFS**, cliquez sur **approbations de partie de confiance **. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  L’approbation sélectionnée clic droit, puis cliquez sur **modifier la stratégie d’émission de revendication **.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Dans le **modifier la stratégie d’émission de revendication** la boîte de dialogue sous **règles de transformation d’émission** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **transformer une revendication entrante** dans la liste, puis cliquez sur **suivant **.  
![Créer la règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Sur le **configurer la règle** sous **nom de règle de revendication**, tapez le nom d’affichage pour cette règle. Dans **type de revendication entrante**, sélectionnez une revendication tapez la liste. Dans **type de revendication sortante**, sélectionnez un type de revendication la liste, puis sélectionnez une des options suivantes, selon les besoins de votre organisation:  
  
    -   **Passer toutes les valeurs de revendication**  
  
    -   **Remplacer une valeur de revendication entrante avec une valeur de revendication sortante**  
  
    -   **Remplacer les revendications de suffixe e\ de messagerie entrantes par un nouveau suffixe de messagerie e\**  
![Créer la règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)   

7.  Cliquez sur le **Terminer** bouton.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.
  
> [!NOTE]  
> Si vous configurez le scénario de contrôle d’accès dynamique qui utilise des revendications ADFS\ émis, tout d’abord créer une règle de transformation sur l’approbation de fournisseur de revendications et **type de revendication entrante**, tapez le nom de la revendication entrante, ou, si une description de revendication a été créée précédemment, sélectionnez-le dans la liste. Ensuite, dans **type de revendication sortante**, sélectionnez l’URL de revendication que vous souhaitez et ensuite créer une règle de transformation sur la partie de confiance pour émettez la revendication de périphérique.  
>   
> Pour plus d’informations sur les scénarios de contrôle d’accès dynamique, voir [dynamique contrôle contenu feuille de route accès](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [à l’aide de revendications ADDS avec ADFS ](https://technet.microsoft.com/library/hh831504.aspx). 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer une règle pour transformer une revendication entrante sur une approbation de fournisseur de revendications dans Windows Server2016 
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **ADFS**, cliquez sur **approbations de fournisseur de revendications **. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  L’approbation sélectionnée clic droit, puis cliquez sur **modifier les règles de revendication **.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Dans le **modifier les règles de revendication** la boîte de dialogue sous **règles de transformation d’acceptation** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **transformer une revendication entrante** dans la liste, puis cliquez sur **suivant **.  
![Créer la règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Sur le **configurer la règle** sous **nom de règle de revendication**, tapez le nom d’affichage pour cette règle. Dans **type de revendication entrante**, sélectionnez une revendication tapez la liste. Dans **type de revendication sortante**, sélectionnez un type de revendication la liste, puis sélectionnez une des options suivantes, selon les besoins de votre organisation:  
  
    -   **Passer toutes les valeurs de revendication**  
  
    -   **Remplacer une valeur de revendication entrante avec une valeur de revendication sortante**  
  
    -   **Remplacer les revendications de suffixe e\ de messagerie entrantes par un nouveau suffixe de messagerie e\**  
![Créer la règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)       

7.  Cliquez sur le **Terminer** bouton.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.  

> [!NOTE]  
> Si vous configurez le scénario de contrôle d’accès dynamique qui utilise des revendications ADFS\ émis, tout d’abord créer une règle de transformation sur l’approbation de fournisseur de revendications et **type de revendication entrante**, tapez le nom de la revendication entrante, ou, si une description de revendication a été créée précédemment, sélectionnez-le dans la liste. Ensuite, dans **type de revendication sortante**, sélectionnez l’URL de revendication que vous souhaitez et ensuite créer une règle de transformation sur la partie de confiance pour émettez la revendication de périphérique.  
>   
> Pour plus d’informations sur les scénarios de contrôle d’accès dynamique, voir [dynamique contrôle contenu feuille de route accès](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [à l’aide de revendications ADDS avec ADFS ](https://technet.microsoft.com/library/hh831504.aspx).   
  
## <a name="to-create-a-rule-to-transform-an-incoming-claim-in-windows-server-2012-r2"></a>Pour créer une règle pour transformer une revendication entrante dans Windows Server2012R2 
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **gestion ADFS **.  
  
2.  Dans l’arborescence de la console, sous **ADFS\\Trust relations**, cliquez sur **approbations de fournisseur de revendications** ou **approbations de partie de confiance**, puis cliquez sur une approbation spécifique dans la liste dans laquelle vous souhaitez créer cette règle.  
  
3.  L’approbation sélectionnée clic droit, puis cliquez sur **modifier les règles de revendication **.  
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez une les onglets suivants, qui dépend de l’approbation que vous modifiez et dans la règle définissez vous souhaitent créer cette règle, puis cliquez sur **ajouter une règle** pour démarrer l’Assistant règle qui est associé à cet ensemble de règles:  
  
    -   **Règles de transformation d’acceptation**  
  
    -   **Règles de transformation d’émission**  
  
    -   **Règles d’autorisation d’émission**  
  
    -   **Règles d’autorisation de délégation**  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **transformer une revendication entrante** dans la liste, puis cliquez sur **suivant **.  
![Créer la règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)   

6.  Sur le **configurer la règle** sous **nom de règle de revendication**, tapez le nom d’affichage pour cette règle. Dans **type de revendication entrante**, sélectionnez une revendication tapez la liste. Dans **type de revendication sortante**, sélectionnez un type de revendication la liste, puis sélectionnez une des options suivantes, selon les besoins de votre organisation:  
  
    -   **Passer toutes les valeurs de revendication**  
  
    -   **Remplacer une valeur de revendication entrante avec une valeur de revendication sortante**  
  
    -   **Remplacer les revendications de suffixe e\ de messagerie entrantes par un nouveau suffixe de messagerie e\**  
![Créer la règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform2.PNG)  

> [!NOTE]  
> Si vous configurez le scénario de contrôle d’accès dynamique qui utilise des revendications ADFS\ émis, tout d’abord créer une règle de transformation sur l’approbation de fournisseur de revendications et **type de revendication entrante**, tapez le nom de la revendication entrante, ou, si une description de revendication a été créée précédemment, sélectionnez-le dans la liste. Ensuite, dans **type de revendication sortante**, sélectionnez l’URL de revendication que vous souhaitez et ensuite créer une règle de transformation sur la partie de confiance pour émettez la revendication de périphérique.  
>   
> Pour plus d’informations sur les scénarios de contrôle d’accès dynamique, voir [dynamique contrôle contenu feuille de route accès](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [à l’aide de revendications ADDS avec ADFS ](https://technet.microsoft.com/library/hh831504.aspx).  
  
7.  Cliquez sur **Terminer**.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer des règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification: Création de règles de revendication pour une partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  

[Liste de vérification: Approuvent créer des règles de revendication pour un fournisseur de revendications](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Le rôle de revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Le rôle de règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
