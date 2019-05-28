---
ms.assetid: 0039fbbb-b981-4526-a550-f3456ff27635
title: Créer une règle pour transformer une revendication entrante
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c87b76224d1ac5dbe3befc837fad8879d0b9a1ef
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189397"
---
# <a name="create-a-rule-to-send-an-ad-fs-1x-compatible-claim"></a>Créer une règle pour envoyer une revendication Compatible de AD FS 1.x

Dans les situations où vous utilisez Active Directory Federation Services \(AD FS\) à émettre des revendications qui seront reçues par les serveurs de fédération exécutant AD FS 1.0 \(Windows Server 2003 R2\) ou AD FS 1.1 \(Windows Server 2008 ou Windows Server 2008 R2\), vous devez procédez comme suit :  
  
-   Créer une règle qui envoie un type de revendication d’ID de nom avec un format de nom UPN, E-mail ou nom commun.  
  
-   Tous les autres revendications qui sont envoyées doivent avoir un des types de revendication suivants :  
  
    -   AD FS 1. *x* adresse de messagerie  
  
    -   AD FS 1. *x* UPN  
  
    -   Nom commun  
  
    -   Regrouper  
  
    -   N’importe quel autre type de revendication qui commence par https://schemas.xmlsoap.org/claims/, tel que https://schemas.xmlsoap.org/claims/EmployeeID  
  
Selon les besoins de votre organisation, utilisez une des procédures suivantes pour créer un AD FS 1. *x* revendication NameID compatible avec :  
  
-   Créer cette règle de revendication d’à l’aide des ID de nom problème an AD FS 1.x le **passer ou filtrer un modèle de règle de revendication entrante**  
  
-   Créer cette règle de revendication d’à l’aide des ID de nom problème an AD FS 1.x le **transformer un modèle de règle de revendication entrante**. Vous pouvez utiliser ce modèle de règle dans les situations dans lesquelles vous souhaitez modifier le type de revendication existant en un nouveau type de revendication qui fonctionne avec AD FS 1.  *x* revendications.  
  
> [!NOTE]  
> Pour cette règle de fonctionner comme prévu, assurez-vous que la partie de confiance ou approbation de fournisseur de revendications dans lequel vous créez cette règle a été configurée pour utiliser le **profil ADFS 1.0 et 1.1**. 




## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Pour créer une règle pour émettre un AD FS 1. *x* ID de nom de revendication à l’aide de la passer ou filtrer un modèle de règle de revendication entrante sur une confiance dans Windows Server 2016 

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **confiance**. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier la stratégie d’émission de revendication**.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Dans le **modifier la stratégie d’émission de revendication** boîte de dialogue **règles de transformation d’émission** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **passer ou filtrer une revendication entrante** dans la liste, puis cliquez sur **suivant** .  
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  Sur le **configurer la règle** page, tapez un nom de règle de revendication.  
  
7.  Dans **type de revendication entrante**, sélectionnez **nom ID** dans la liste.  
  
8.  Dans **format d’ID de nom entrantes**, sélectionnez une des suivante AD FS 1. *x*\-compatible avec les formats à partir de la liste de revendication :  
  
    -   **UPN**  
  
    -   **E\-Mail**  
  
    -   **Nom commun**  
  
9. Sélectionnez une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Passer toutes les valeurs de revendication**  
  
    -   **Transmettre uniquement la valeur de revendication spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui correspondent à une valeur de suffixe d’adresse de messagerie spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui commencent par une valeur spécifique**  
![Créer la règle](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. Cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  

  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer une règle pour émettre un AD FS 1. *x* ID de nom de revendication à l’aide de la passer ou filtrer un modèle de règle de revendication entrante sur une approbation de fournisseur de revendications dans Windows Server 2016 
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de fournisseur de revendications**. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Dans le **modifier les règles de revendication** boîte de dialogue **règles de transformation d’acceptation** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **passer ou filtrer une revendication entrante** dans la liste, puis cliquez sur **suivant** .  
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  Sur le **configurer la règle** page, tapez un nom de règle de revendication.  
  
7.  Dans **type de revendication entrante**, sélectionnez **nom ID** dans la liste.  
  
8.  Dans **format d’ID de nom entrantes**, sélectionnez une des suivante AD FS 1. *x*\-compatible avec les formats à partir de la liste de revendication :  
  
    -   **UPN**  
  
    -   **E\-Mail**  
  
    -   **Nom commun**  
  
9. Sélectionnez une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Passer toutes les valeurs de revendication**  
  
    -   **Transmettre uniquement la valeur de revendication spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui correspondent à une valeur de suffixe d’adresse de messagerie spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui commencent par une valeur spécifique**  
![Créer la règle](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. Cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  

  

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Pour créer une règle pour transformer une revendication entrante sur une confiance dans Windows Server 2016 

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **confiance**. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier la stratégie d’émission de revendication**.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Dans le **modifier la stratégie d’émission de revendication** boîte de dialogue **règles de transformation d’émission** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **transformer une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Sur le **configurer la règle** page, tapez un nom de règle de revendication.  
  
7.  Dans **type de revendication entrante**, sélectionnez le type de revendication entrante que vous souhaitez transformer dans la liste.  
  
8.  Dans **type de revendication sortante**, sélectionnez **nom ID** dans la liste.  
  
9. Dans **format d’ID de nom sortant**, sélectionnez une des suivante AD FS 1. *x*\-compatible avec les formats à partir de la liste de revendication :  
  
    -   **UPN**  
  
    -   **E\-Mail**  
  
    -   **Nom commun**  
  
10. Sélectionnez une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Passer toutes les valeurs de revendication**  
  
    -   **Remplacer une valeur de revendication entrante avec une valeur de revendication sortante**  
  
    -   **Remplacez e entrant\-avec une nouvelle e des revendications de suffixe de messagerie\-suffixe de messagerie**  
![Créer la règle](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. Cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  

  


## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer une règle pour transformer une revendication entrante sur une approbation de fournisseur de revendications dans Windows Server 2016 
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de fournisseur de revendications**. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Dans le **modifier les règles de revendication** boîte de dialogue **règles de transformation d’acceptation** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **transformer une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Sur le **configurer la règle** page, tapez un nom de règle de revendication.  
  
7.  Dans **type de revendication entrante**, sélectionnez le type de revendication entrante que vous souhaitez transformer dans la liste.  
  
8.  Dans **type de revendication sortante**, sélectionnez **nom ID** dans la liste.  
  
9. Dans **format d’ID de nom sortant**, sélectionnez une des suivante AD FS 1. *x*\-compatible avec les formats à partir de la liste de revendication :  
  
    -   **UPN**  
  
    -   **E\-Mail**  
  
    -   **Nom commun**  
  
10. Sélectionnez une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Passer toutes les valeurs de revendication**  
  
    -   **Remplacer une valeur de revendication entrante avec une valeur de revendication sortante**  
  
    -   **Remplacez e entrant\-avec une nouvelle e des revendications de suffixe de messagerie\-suffixe de messagerie**  
![Créer la règle](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. Cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  













  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-windows-server-2012-r2"></a>Pour créer une règle pour émettre un AD FS 1. *x* ID de nom de revendication à l’aide de la passer ou filtrer un modèle de règle de revendication entrante sur Windows Server 2012 R2
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS\\relations d’approbation**, cliquez sur **approbations de fournisseur de revendications** ou **confiance**, puis cliquez sur un spécifique dans la liste où vous souhaitez créer cette règle d’approbation.  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.  
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez une les onglets suivants, en fonction de l’approbation que vous modifiez et quelle règle vous voulez créer cette règle dans, puis cliquez sur **ajouter une règle** pour démarrer l’Assistant règle qui est associé à cet ensemble de règles :  
  
    -   **Règles de transformation d’acceptation**  
  
    -   **Règles de transformation d’émission**  
  
    -   **Règles d’autorisation d’émission**  
  
    -   **Règles d’autorisation de délégation**  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)    

5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **passer ou filtrer une revendication entrante** dans la liste, puis cliquez sur **suivant** .  
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)  
  
6.  Sur le **configurer la règle** page, tapez un nom de règle de revendication.  
  
7.  Dans **type de revendication entrante**, sélectionnez **nom ID** dans la liste.  
  
8.  Dans **format d’ID de nom entrantes**, sélectionnez une des suivante AD FS 1. *x*\-compatible avec les formats à partir de la liste de revendication :  
  
    -   **UPN**  
  
    -   **E\-Mail**  
  
    -   **Nom commun**  
  
9. Sélectionnez une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Passer toutes les valeurs de revendication**  
  
    -   **Transmettre uniquement la valeur de revendication spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui correspondent à une valeur de suffixe d’adresse de messagerie spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui commencent par une valeur spécifique**  
![Créer la règle](media/\Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs1.PNG)   

10. Cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  

  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Pour créer une règle pour émettre un AD FS 1. *x* revendication d’ID de nom à l’aide de la transformation un modèle de règle de revendication entrante dans Windows Server 2012 R2  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS\\relations d’approbation**, cliquez sur **approbations de fournisseur de revendications** ou **confiance**, puis cliquez sur un spécifique dans la liste où vous souhaitez créer cette règle d’approbation.  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.  
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez une les onglets suivants, qui dépend de l’approbation que vous modifiez et dans la règle qui vous souhaitez créer cette règle, puis cliquez sur **ajouter une règle** pour démarrer la règle Assistant qui est associé à cet ensemble de règles :  
  
    -   **Règles de transformation d’acceptation**  
  
    -   **Règles de transformation d’émission**  
  
    -   **Règles d’autorisation d’émission**  
  
    -   **Règles d’autorisation de délégation**  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **transformer une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)   
  
6.  Sur le **configurer la règle** page, tapez un nom de règle de revendication.  
  
7.  Dans **type de revendication entrante**, sélectionnez le type de revendication entrante que vous souhaitez transformer dans la liste.  
  
8.  Dans **type de revendication sortante**, sélectionnez **nom ID** dans la liste.  
  
9. Dans **format d’ID de nom sortant**, sélectionnez une des suivante AD FS 1. *x*\-compatible avec les formats à partir de la liste de revendication :  
  
    -   **UPN**  
  
    -   **E\-Mail**  
  
    -   **Nom commun**  
  
10. Sélectionnez une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Passer toutes les valeurs de revendication**  
  
    -   **Remplacer une valeur de revendication entrante avec une valeur de revendication sortante**  
  
    -   **Remplacez e entrant\-avec une nouvelle e des revendications de suffixe de messagerie\-suffixe de messagerie**  
![Créer la règle](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs2.PNG)    

11. Cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer les règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification : création de règles de revendication pour une approbation de partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  

[Liste de vérification : création de règles de revendication pour une approbation de fournisseur de revendications](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Rôle des revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Rôle des règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
