---
ms.assetid: 6127963f-71b2-4d8f-8b53-7c525bf06521
title: Créer une règle pour transmettre ou filtrer une revendication entrante
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 145558e620188c4311d79d2a9ba4ed7aaf7b13a8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358141"
---
# <a name="create-a-rule-to-pass-through-or-filter-an-incoming-claim"></a>Créer une règle pour transmettre ou filtrer une revendication entrante

À l’aide du modèle de règle de transmission ou de filtrage d’une revendication entrante dans Services ADFS \(AD FS @ no__t-1, vous pouvez passer toutes les revendications entrantes avec un type de revendication sélectionné. Vous pouvez également filtrer les valeurs des revendications entrantes avec un type de revendication sélectionné. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle qui enverra toutes les revendications de groupe entrantes. Vous pouvez également utiliser cette règle pour envoyer uniquement les revendications de nom d’utilisateur principal \(UPN @ no__t-1 qui se terminent par @fabrikam.  
  
Vous pouvez utiliser la procédure suivante pour créer une règle de revendication avec le composant logiciel\-enfichable de gestion AD FS.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Pour créer une règle de transmission ou de filtrage d’une revendication entrante sur une approbation de partie de confiance dans Windows Server 2016 

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de partie de confiance**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Cliquez\-avec le bouton droit sur l’approbation sélectionnée, puis cliquez sur **modifier la stratégie d’émission de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Dans la boîte de dialogue **modifier la stratégie d’émission de revendication** , sous règles de transformation d' **émission** , cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **transférer ou filtrer une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  Dans la page **configurer la règle** , sous nom de la règle de **revendication** , tapez le nom complet de cette règle, dans **type de revendication entrante** , sélectionnez un type de revendication dans la liste, puis sélectionnez l’une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Transmettre toutes les valeurs de revendication**  
  
    -   **Passer uniquement une valeur de revendication spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui correspondent à une valeur de suffixe d’adresse de messagerie spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui commencent par une valeur spécifique**  
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  Cliquez sur le bouton **Terminer** .  
  
8.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK** pour enregistrer la règle.
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer une règle de transmission ou de filtrage d’une revendication entrante sur une approbation de fournisseur de revendications dans Windows Server 2016 
  
1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de fournisseur de revendications**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Cliquez\-avec le bouton droit sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Dans la boîte de dialogue **modifier les règles de revendication** , sous règles de transformation d' **acceptation** , cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **transférer ou filtrer une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  Dans la page **configurer la règle** , sous nom de la règle de **revendication** , tapez le nom complet de cette règle, dans **type de revendication entrante** , sélectionnez un type de revendication dans la liste, puis sélectionnez l’une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Transmettre toutes les valeurs de revendication**  
  
    -   **Passer uniquement une valeur de revendication spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui correspondent à une valeur de suffixe d’adresse de messagerie spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui commencent par une valeur spécifique**  
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  Cliquez sur le bouton **Terminer** .  
  
8.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK** pour enregistrer la règle.  

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-in-windows-server-2012-r2"></a>Pour créer une règle de transmission ou de filtrage d’une revendication entrante dans Windows Server 2012 R2

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **relations\\d’approbation ad FSAD FS**, cliquez sur approbations de **fournisseur de revendications** ou **approbations de partie de confiance**, puis cliquez sur une approbation spécifique dans la liste dans laquelle vous souhaitez créer cette règle.  
  
3.  Cliquez\-avec le bouton droit sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   
  
4.  Dans la boîte de dialogue **modifier les règles de revendication** , sélectionnez l’un des onglets suivants, en fonction de l’approbation que vous modifiez et de l’ensemble de règles dans lequel vous souhaitez créer cette règle, puis cliquez sur Ajouter une **règle** pour démarrer l’Assistant règle associé à cet ensemble de règles. :  
  
    -   **Règles de transformation d’acceptation**  
  
    -   **Règles de transformation d’émission**  
  
    -   **Règles d’autorisation d’émission**  
  
    -   **Règles d’autorisation de délégation**  
![créer une règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)    

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **transférer ou filtrer une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)    

6.  Dans la page **configurer la règle** , sous nom de la règle de **revendication** , tapez le nom complet de cette règle, dans **type de revendication entrante** , sélectionnez un type de revendication dans la liste, puis sélectionnez l’une des options suivantes, selon les besoins de votre organisation :  
  
    -   **Transmettre toutes les valeurs de revendication**  
  
    -   **Passer uniquement une valeur de revendication spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui correspondent à une valeur de suffixe d’adresse de messagerie spécifique**  
  
    -   **Transmettre uniquement les valeurs de revendication qui commencent par une valeur spécifique**  
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule8.PNG)    

7.  Cliquez sur le bouton **Terminer** .  
  
8.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK** pour enregistrer la règle.  



  
## <a name="additional-references"></a>Références supplémentaires  
[Configurer les règles de revendication](Configure-Claim-Rules.md)  
  
[Quand utiliser une règle de transfert ou une règle de revendication de filtre](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)  
  
[Rôle des revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Rôle des règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
  
