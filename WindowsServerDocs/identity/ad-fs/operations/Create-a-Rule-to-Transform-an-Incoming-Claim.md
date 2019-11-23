---
ms.assetid: ef83960f-d2cf-441f-b2b6-d97822ec7149
title: Créer une règle pour transformer une revendication entrante
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 15a4583d429de9383e9405cfcd444777aa55c921
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407572"
---
# <a name="create-a-rule-to-transform-an-incoming-claim"></a>Créer une règle pour transformer une revendication entrante


En utilisant le modèle de règle **transformer une revendication entrante** dans Services ADFS \(AD FS\), vous pouvez sélectionner une revendication entrante, modifier son type de revendication et modifier sa valeur de revendication. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle qui envoie une revendication de rôle avec la même valeur de revendication d’une revendication de groupe entrante. Vous pouvez également utiliser cette règle pour envoyer une revendication de groupe avec une valeur de revendication des acheteurs lorsqu’il existe une revendication de groupe entrante avec la valeur admins, ou vous pouvez envoyer uniquement le nom d’utilisateur principal \(UPN\) revendications qui se terminent par @fabrikam.  
  
Vous pouvez utiliser la procédure suivante pour créer une règle de revendication avec le\-du composant logiciel enfichable Gestion de la AD FS dans.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **administrateurs**ou à un groupe équivalent sur l’ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477). 

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

6.  Dans la page **configurer la règle** , sous nom de la règle de **revendication**, tapez le nom complet de cette règle. Dans **type de revendication entrante**, sélectionnez un type de revendication dans la liste. Dans **type de revendication sortante**, sélectionnez un type de revendication dans la liste, puis sélectionnez l’une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Transmettre toutes les valeurs de revendication**  
  
    -   **Remplacer une valeur de revendication entrante par une autre valeur de revendication sortante**  
  
    -   **Remplacer les revendications e\-de messagerie entrantes par un nouveau suffixe de courrier électronique\-**  
![créer une règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)   

7.  Cliquez sur le bouton **Terminer** .  
  
8.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK** pour enregistrer la règle.
  
> [!NOTE]  
> Si vous configurez le scénario de Access Control dynamique qui utilise AD FS des revendications émises par\-, commencez par créer une règle de transformation sur l’approbation de fournisseur de revendications et, dans **type de revendication entrante**, tapez le nom de la revendication entrante, ou, si une description de revendication a été créée précédemment, sélectionnez-la dans la liste. Ensuite, dans **type de revendication sortante**, sélectionnez l’URL de revendication souhaitée, puis créez une règle de transformation sur l’approbation de la partie de confiance pour émettre la revendication de l’appareil.  
>   
> Pour plus d’informations sur les scénarios de Access Control dynamique, consultez feuille de [route de contenu dynamique Access Control](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [utilisation de AD DS des revendications avec AD FS](https://technet.microsoft.com/library/hh831504.aspx). 

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

6.  Dans la page **configurer la règle** , sous nom de la règle de **revendication**, tapez le nom complet de cette règle. Dans **type de revendication entrante**, sélectionnez un type de revendication dans la liste. Dans **type de revendication sortante**, sélectionnez un type de revendication dans la liste, puis sélectionnez l’une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Transmettre toutes les valeurs de revendication**  
  
    -   **Remplacer une valeur de revendication entrante par une autre valeur de revendication sortante**  
  
    -   **Remplacer les revendications e\-de messagerie entrantes par un nouveau suffixe de courrier électronique\-**  
![créer une règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)       

7.  Cliquez sur le bouton **Terminer** .  
  
8.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK** pour enregistrer la règle.  

> [!NOTE]  
> Si vous configurez le scénario de Access Control dynamique qui utilise AD FS des revendications émises par\-, commencez par créer une règle de transformation sur l’approbation de fournisseur de revendications et, dans **type de revendication entrante**, tapez le nom de la revendication entrante, ou, si une description de revendication a été créée précédemment, sélectionnez-la dans la liste. Ensuite, dans **type de revendication sortante**, sélectionnez l’URL de revendication souhaitée, puis créez une règle de transformation sur l’approbation de la partie de confiance pour émettre la revendication de l’appareil.  
>   
> Pour plus d’informations sur les scénarios de Access Control dynamique, consultez feuille de [route de contenu dynamique Access Control](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [utilisation de AD DS des revendications avec AD FS](https://technet.microsoft.com/library/hh831504.aspx).   
  
## <a name="to-create-a-rule-to-transform-an-incoming-claim-in-windows-server-2012-r2"></a>Pour créer une règle de transformation d’une revendication entrante dans Windows Server 2012 R2 
  
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

6.  Dans la page **configurer la règle** , sous nom de la règle de **revendication**, tapez le nom complet de cette règle. Dans **type de revendication entrante**, sélectionnez un type de revendication dans la liste. Dans **type de revendication sortante**, sélectionnez un type de revendication dans la liste, puis sélectionnez l’une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Transmettre toutes les valeurs de revendication**  
  
    -   **Remplacer une valeur de revendication entrante par une autre valeur de revendication sortante**  
  
    -   **Remplacer les revendications e\-de messagerie entrantes par un nouveau suffixe de courrier électronique\-**  
![créer une règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform2.PNG)  

> [!NOTE]  
> Si vous configurez le scénario de Access Control dynamique qui utilise AD FS des revendications émises par\-, commencez par créer une règle de transformation sur l’approbation de fournisseur de revendications et, dans **type de revendication entrante**, tapez le nom de la revendication entrante, ou, si une description de revendication a été créée précédemment, sélectionnez-la dans la liste. Ensuite, dans **type de revendication sortante**, sélectionnez l’URL de revendication souhaitée, puis créez une règle de transformation sur l’approbation de la partie de confiance pour émettre la revendication de l’appareil.  
>   
> Pour plus d’informations sur les scénarios de Access Control dynamique, consultez feuille de [route de contenu dynamique Access Control](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [utilisation de AD DS des revendications avec AD FS](https://technet.microsoft.com/library/hh831504.aspx).  
  
7. Cliquez sur **Terminer**.  
  
8. Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK** pour enregistrer la règle.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer les règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification : création de règles de revendication pour une approbation de partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  

[Liste de vérification : création de règles de revendication pour une approbation de fournisseur de revendications](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Rôle des revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Rôle des règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
