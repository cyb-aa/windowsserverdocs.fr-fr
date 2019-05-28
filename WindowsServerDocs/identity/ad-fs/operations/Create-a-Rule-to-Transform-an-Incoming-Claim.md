---
ms.assetid: ef83960f-d2cf-441f-b2b6-d97822ec7149
title: Créer une règle pour transformer une revendication entrante
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6bd107aca6c6f33cdf5f88e5b48a52fdea8d2086
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189345"
---
# <a name="create-a-rule-to-transform-an-incoming-claim"></a>Créer une règle pour transformer une revendication entrante


À l’aide de la **transformer une revendication entrante** le modèle de règle dans Active Directory Federation Services \(AD FS\), vous pouvez sélectionner une revendication entrante, modifiez son type de revendication et modifier sa valeur de revendication. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle qui envoie une revendication de rôle avec la même valeur de revendication d’une revendication de groupe entrante. Vous pouvez également utiliser cette règle pour envoyer un groupe de revendication avec une valeur de revendication de l’acheteur lorsqu’il existe une revendication de groupe entrante avec la valeur administrateurs, ou vous pouvez envoyer seulement nom d’utilisateur principal \(UPN\) les revendications qui se terminent par @fabrikam.  
  
Vous pouvez utiliser la procédure suivante pour créer une règle de revendication avec le composant logiciel enfichable Gestion AD FS\-dans.  
  
L’appartenance au **administrateurs**, ou équivalente, sur l’ordinateur local est la configuration minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477). 

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

6.  Sur le **configurer la règle** page sous **nom de règle de revendication**, tapez le nom complet pour cette règle. Dans **type de revendication entrante**, sélectionnez un type de revendication dans la liste. Dans **type de revendication sortante**, sélectionnez un type de revendication dans la liste, puis sélectionnez une des options suivantes, qui varie selon les besoins de votre organisation :  
  
    -   **Passer toutes les valeurs de revendication**  
  
    -   **Remplacer une valeur de revendication entrante avec une valeur de revendication sortante**  
  
    -   **Remplacez e entrant\-avec une nouvelle e des revendications de suffixe de messagerie\-suffixe de messagerie**  
![Créer la règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)   

7.  Cliquez sur le **Terminer** bouton.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.
  
> [!NOTE]  
> Si vous configurez le scénario de contrôle d’accès dynamique qui utilise les services AD FS\-reçoit des revendications, d’abord créer une règle de transformation sur l’approbation de fournisseur de revendications et **type de revendication entrante**, tapez le nom de la revendication entrante, ou, si un description de revendication a été précédemment créée, sélectionnez-le dans la liste. Ensuite, dans **type de revendication sortante**, sélectionnez l’URL de la revendication que vous souhaitez, puis créer une règle de transformation sur la partie de confiance pour émettre la revendication de l’appareil.  
>   
> Pour plus d’informations sur les scénarios de contrôle d’accès dynamique, consultez [dynamique plan du contenu contrôle accès](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [à l’aide de revendications AD DS avec AD FS](https://technet.microsoft.com/library/hh831504.aspx). 

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

6.  Sur le **configurer la règle** page sous **nom de règle de revendication**, tapez le nom complet pour cette règle. Dans **type de revendication entrante**, sélectionnez un type de revendication dans la liste. Dans **type de revendication sortante**, sélectionnez un type de revendication dans la liste, puis sélectionnez une des options suivantes, qui varie selon les besoins de votre organisation :  
  
    -   **Passer toutes les valeurs de revendication**  
  
    -   **Remplacer une valeur de revendication entrante avec une valeur de revendication sortante**  
  
    -   **Remplacez e entrant\-avec une nouvelle e des revendications de suffixe de messagerie\-suffixe de messagerie**  
![Créer la règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)       

7.  Cliquez sur le **Terminer** bouton.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.  

> [!NOTE]  
> Si vous configurez le scénario de contrôle d’accès dynamique qui utilise les services AD FS\-reçoit des revendications, d’abord créer une règle de transformation sur l’approbation de fournisseur de revendications et **type de revendication entrante**, tapez le nom de la revendication entrante, ou, si un description de revendication a été précédemment créée, sélectionnez-le dans la liste. Ensuite, dans **type de revendication sortante**, sélectionnez l’URL de la revendication que vous souhaitez, puis créer une règle de transformation sur la partie de confiance pour émettre la revendication de l’appareil.  
>   
> Pour plus d’informations sur les scénarios de contrôle d’accès dynamique, consultez [dynamique plan du contenu contrôle accès](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [à l’aide de revendications AD DS avec AD FS](https://technet.microsoft.com/library/hh831504.aspx).   
  
## <a name="to-create-a-rule-to-transform-an-incoming-claim-in-windows-server-2012-r2"></a>Pour créer une règle pour transformer une revendication entrante dans Windows Server 2012 R2 
  
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

6.  Sur le **configurer la règle** page sous **nom de règle de revendication**, tapez le nom complet pour cette règle. Dans **type de revendication entrante**, sélectionnez un type de revendication dans la liste. Dans **type de revendication sortante**, sélectionnez un type de revendication dans la liste, puis sélectionnez une des options suivantes, qui varie selon les besoins de votre organisation :  
  
    -   **Passer toutes les valeurs de revendication**  
  
    -   **Remplacer une valeur de revendication entrante avec une valeur de revendication sortante**  
  
    -   **Remplacez e entrant\-avec une nouvelle e des revendications de suffixe de messagerie\-suffixe de messagerie**  
![Créer la règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform2.PNG)  

> [!NOTE]  
> Si vous configurez le scénario de contrôle d’accès dynamique qui utilise les services AD FS\-reçoit des revendications, d’abord créer une règle de transformation sur l’approbation de fournisseur de revendications et **type de revendication entrante**, tapez le nom de la revendication entrante, ou, si un description de revendication a été précédemment créée, sélectionnez-le dans la liste. Ensuite, dans **type de revendication sortante**, sélectionnez l’URL de la revendication que vous souhaitez, puis créer une règle de transformation sur la partie de confiance pour émettre la revendication de l’appareil.  
>   
> Pour plus d’informations sur les scénarios de contrôle d’accès dynamique, consultez [dynamique plan du contenu contrôle accès](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [à l’aide de revendications AD DS avec AD FS](https://technet.microsoft.com/library/hh831504.aspx).  
  
7.  Cliquez sur **Terminer**.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer les règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification : création de règles de revendication pour une approbation de partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  

[Liste de vérification : création de règles de revendication pour une approbation de fournisseur de revendications](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Rôle des revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Rôle des règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
