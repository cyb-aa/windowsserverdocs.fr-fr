---
ms.assetid: 0039fbbb-b981-4526-a550-f3456ff27635
title: Créer une règle pour transformer une revendication entrante
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3721afc5e03ea99ea1b73e72d3f2e98e942701e5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816642"
---
# <a name="create-a-rule-to-send-an-ad-fs-1x-compatible-claim"></a>Créer une règle pour envoyer une revendication compatible AD FS 1. x

Dans les situations où vous utilisez Services ADFS \(AD FS\) pour émettre des revendications qui seront reçues par des serveurs de Fédération exécutant AD FS 1,0 \(Windows Server 2003 R2\) ou AD FS 1,1 \(Windows Server 2008 ou Windows Server 2008 R2\), vous devez effectuer les opérations suivantes :  
  
-   Créez une règle qui enverra un type de revendication d’ID de nom avec un format UPN, E-mail ou nom commun.  
  
-   Toutes les autres revendications envoyées doivent avoir l’un des types de revendications suivants :  
  
    -   AD FS 1. *x* adresse e-mail  
  
    -   AD FS 1. *x* UPN  
  
    -   Nom commun  
  
    -   Groupe  
  
    -   Tout autre type de revendication qui commence par https://schemas.xmlsoap.org/claims/, tel que https://schemas.xmlsoap.org/claims/EmployeeID  
  
Selon les besoins de votre organisation, utilisez l’une des procédures suivantes pour créer un AD FS 1. revendication NameID compatible *x* :  
  
-   Créez cette règle pour émettre une revendication d’ID de nom AD FS 1. x à l’aide du **modèle de règle de transmission ou de filtrage d’une revendication entrante**  
  
-   Créez cette règle pour émettre une revendication d’ID de nom AD FS 1. x à l’aide du **modèle de règle transformer une revendication entrante**. Vous pouvez utiliser ce modèle de règle dans les situations où vous souhaitez modifier le type de revendication existant en un nouveau type de revendication qui fonctionnera avec AD FS 1.  *x* revendications.  
  
> [!NOTE]  
> Pour que cette règle fonctionne comme prévu, assurez-vous que l’approbation de la partie de confiance ou du fournisseur de revendications dans laquelle vous créez cette règle a été configurée pour utiliser le **profil AD FS 1,0 et 1,1**. 

## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Pour créer une règle pour émettre un AD FS 1. *x* ID de la revendication à l’aide du modèle de règle de transmission ou de filtrage d’une revendication entrante sur une partie de confiance dans Windows Server 2016 

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de partie de confiance**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Cliquez avec le bouton droit\-sur l’approbation sélectionnée, puis cliquez sur **modifier la stratégie d’émission de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Dans la boîte de dialogue **modifier la stratégie d’émission de revendication** , sous règles de transformation d' **émission** , cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **transférer ou filtrer une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  Dans la page **configurer la règle** , tapez un nom de règle de revendication.  
  
7.  Dans **type de revendication entrante**, sélectionnez **ID de nom** dans la liste.  
  
8.  Dans **format d’ID de nom entrant**, sélectionnez l’une des AD FS 1 ci-dessous. *x*\-formats de revendication compatibles de la liste :  
  
    -   **NOMENCLATURE**  
  
    -   **E\-courrier électronique**  
  
    -   **Nom commun**  
  
9. Sélectionnez l’une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Transmettre toutes les valeurs de revendication**  
  
    -   **Passer uniquement une valeur de revendication spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui correspondent à une valeur de suffixe d’adresse de messagerie spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui commencent par une valeur spécifique**  
![créer une règle](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. Cliquez sur **Terminer**, puis sur **OK** pour enregistrer la règle.  

  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer une règle pour émettre un AD FS 1. *x* ID de la revendication à l’aide du modèle de règle de transmission ou de filtrage d’une revendication entrante sur une approbation de fournisseur de revendications dans Windows Server 2016 
  
1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de fournisseur de revendications**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Cliquez avec le bouton droit\-sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Dans la boîte de dialogue **modifier les règles de revendication** , sous règles de transformation d' **acceptation** , cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **transférer ou filtrer une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  Dans la page **configurer la règle** , tapez un nom de règle de revendication.  
  
7.  Dans **type de revendication entrante**, sélectionnez **ID de nom** dans la liste.  
  
8.  Dans **format d’ID de nom entrant**, sélectionnez l’une des AD FS 1 ci-dessous. *x*\-formats de revendication compatibles de la liste :  
  
    -   **NOMENCLATURE**  
  
    -   **E\-courrier électronique**  
  
    -   **Nom commun**  
  
9. Sélectionnez l’une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Transmettre toutes les valeurs de revendication**  
  
    -   **Passer uniquement une valeur de revendication spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui correspondent à une valeur de suffixe d’adresse de messagerie spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui commencent par une valeur spécifique**  
![créer une règle](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)

10. Cliquez sur **Terminer**, puis sur **OK** pour enregistrer la règle.  


## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Pour créer une règle de transformation d’une revendication entrante sur une approbation de partie de confiance dans Windows Server 2016 

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de partie de confiance**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Cliquez avec le bouton droit\-sur l’approbation sélectionnée, puis cliquez sur **modifier la stratégie d’émission de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)
  
4.  Dans la boîte de dialogue **modifier la stratégie d’émission de revendication** , sous règles de transformation d' **émission** , cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **transformer une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)

6.  Dans la page **configurer la règle** , tapez un nom de règle de revendication.  
  
7.  Dans **type de revendication entrante**, sélectionnez le type de revendication entrante que vous souhaitez transformer dans la liste.  
  
8.  Dans **type de revendication sortante**, sélectionnez **ID de nom** dans la liste.  
  
9. Dans le **format d’ID de nom sortant**, sélectionnez l’une des AD FS 1 ci-dessous. *x*\-formats de revendication compatibles de la liste :  
  
    -   **NOMENCLATURE**  
  
    -   **E\-courrier électronique**  
  
    -   **Nom commun**  
  
10. Sélectionnez l’une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Transmettre toutes les valeurs de revendication**  
  
    -   **Remplacer une valeur de revendication entrante par une autre valeur de revendication sortante**  
  
    -   **Remplacer les revendications e\-de messagerie entrantes par un nouveau suffixe de courrier électronique\-**  
![créer une règle](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)

11. Cliquez sur **Terminer**, puis sur **OK** pour enregistrer la règle.  

  


## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer une règle de transformation d’une revendication entrante sur une approbation de fournisseur de revendications dans Windows Server 2016 
  
1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de fournisseur de revendications**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Cliquez avec le bouton droit\-sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Dans la boîte de dialogue **modifier les règles de revendication** , sous règles de transformation d' **acceptation** , cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **transformer une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Dans la page **configurer la règle** , tapez un nom de règle de revendication.  
  
7.  Dans **type de revendication entrante**, sélectionnez le type de revendication entrante que vous souhaitez transformer dans la liste.  
  
8.  Dans **type de revendication sortante**, sélectionnez **ID de nom** dans la liste.  
  
9. Dans le **format d’ID de nom sortant**, sélectionnez l’une des AD FS 1 ci-dessous. *x*\-formats de revendication compatibles de la liste :  
  
    -   **NOMENCLATURE**  
  
    -   **E\-courrier électronique**  
  
    -   **Nom commun**  
  
10. Sélectionnez l’une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Transmettre toutes les valeurs de revendication**  
  
    -   **Remplacer une valeur de revendication entrante par une autre valeur de revendication sortante**  
  
    -   **Remplacer les revendications e\-de messagerie entrantes par un nouveau suffixe de courrier électronique\-**  
![créer une règle](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. Cliquez sur **Terminer**, puis sur **OK** pour enregistrer la règle.  













  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-windows-server-2012-r2"></a>Pour créer une règle pour émettre un AD FS 1. ID de la revendication de nom *x* à l’aide du modèle de règle de transmission ou de filtrage d’une revendication entrante sur Windows Server 2012 R2
  
1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS\\relations d’approbation**, cliquez sur approbations de **fournisseur de revendications** ou **approbations de partie de confiance**, puis cliquez sur une approbation spécifique dans la liste dans laquelle vous souhaitez créer cette règle.  
  
3.  Cliquez avec le bouton droit\-sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.  
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  Dans la boîte de dialogue **modifier les règles de revendication** , sélectionnez l’un des onglets suivants, en fonction de l’approbation que vous modifiez et de l’ensemble de règles dans lequel vous souhaitez créer cette règle, puis cliquez sur Ajouter une **règle** pour démarrer l’Assistant règle associé à cet ensemble de règles :  
  
    -   **Règles de transformation d’acceptation**  
  
    -   **Règles de transformation d’émission**  
  
    -   **Règles d’autorisation d’émission**  
  
    -   **Règles d’autorisation de délégation**  
![créer une règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)    

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **transférer ou filtrer une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)  
  
6.  Dans la page **configurer la règle** , tapez un nom de règle de revendication.  
  
7.  Dans **type de revendication entrante**, sélectionnez **ID de nom** dans la liste.  
  
8.  Dans **format d’ID de nom entrant**, sélectionnez l’une des AD FS 1 ci-dessous. *x*\-formats de revendication compatibles de la liste :  
  
    -   **NOMENCLATURE**  
  
    -   **E\-courrier électronique**  
  
    -   **Nom commun**  
  
9. Sélectionnez l’une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Transmettre toutes les valeurs de revendication**  
  
    -   **Passer uniquement une valeur de revendication spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui correspondent à une valeur de suffixe d’adresse de messagerie spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui commencent par une valeur spécifique**  
![créer une règle](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs1.PNG)

10. Cliquez sur **Terminer**, puis sur **OK** pour enregistrer la règle.  

  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Pour créer une règle pour émettre un AD FS 1. ID de la revendication de nom *x* à l’aide du modèle de règle transformer une revendication entrante dans Windows Server 2012 R2  
  
1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS\\relations d’approbation**, cliquez sur approbations de **fournisseur de revendications** ou **approbations de partie de confiance**, puis cliquez sur une approbation spécifique dans la liste dans laquelle vous souhaitez créer cette règle.  
  
3.  Cliquez avec le bouton droit\-sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.  
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  Dans la boîte de dialogue **modifier les règles de revendication** , sélectionnez l’un des onglets suivants, qui dépend de l’approbation que vous modifiez et de l’ensemble de règles pour lequel vous souhaitez créer cette règle, puis cliquez sur Ajouter une **règle** pour démarrer l’Assistant règle associé à cet ensemble de règles :  
  
    -   **Règles de transformation d’acceptation**  
  
    -   **Règles de transformation d’émission**  
  
    -   **Règles d’autorisation d’émission**  
  
    -   **Règles d’autorisation de délégation**  
![créer une règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **transformer une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)   
  
6.  Dans la page **configurer la règle** , tapez un nom de règle de revendication.  
  
7.  Dans **type de revendication entrante**, sélectionnez le type de revendication entrante que vous souhaitez transformer dans la liste.  
  
8.  Dans **type de revendication sortante**, sélectionnez **ID de nom** dans la liste.  
  
9. Dans le **format d’ID de nom sortant**, sélectionnez l’une des AD FS 1 ci-dessous. *x*\-formats de revendication compatibles de la liste :  
  
    -   **NOMENCLATURE**  
  
    -   **E\-courrier électronique**  
  
    -   **Nom commun**  
  
10. Sélectionnez l’une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Transmettre toutes les valeurs de revendication**  
  
    -   **Remplacer une valeur de revendication entrante par une autre valeur de revendication sortante**  
  
    -   **Remplacer les revendications e\-de messagerie entrantes par un nouveau suffixe de courrier électronique\-**  
![créer une règle](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs2.PNG)    

11. Cliquez sur **Terminer**, puis sur **OK** pour enregistrer la règle.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer les règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification : création de règles de revendication pour une approbation de partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  

[Liste de vérification : création de règles de revendication pour une approbation de fournisseur de revendications](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Rôle des revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Rôle des règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
