---
ms.assetid: 38eb3726-e97b-484e-9926-67e8a046b0c5
title: Créer une règle pour envoyer des revendications à l’aide d’une règle personnalisée
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ade2a8304288d102608c81a0c29155478e5a4b7b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189431"
---
# <a name="create-a-rule-to-send-claims-using-a-custom-rule"></a>Créer une règle pour envoyer des revendications à l’aide d’une règle personnalisée


À l’aide de la **envoyer les revendications à l’aide d’une règle personnalisée** modèle dans Active Directory Federation Services (ADFS), vous pouvez créer des règles de revendication personnalisée pour une situation dans laquelle un modèle de règle standard ne satisfait pas aux exigences de votre organisation. Règles de revendication personnalisées sont écrites dans le langage de règle de revendication et doit ensuite être copiés dans le **règle personnalisée** zone de texte avant de pouvoir être utilisés dans un ensemble de règles. Pour plus d’informations sur la construction de la syntaxe pour une règle avancée, consultez [The Role of the Claim Rule Language](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md).  
  
Vous pouvez utiliser la procédure suivante pour créer une règle de revendication à l’aide du composant logiciel enfichable Gestion AD FS\-dans.  
  
L’appartenance au **administrateurs**, ou équivalente, sur l’ordinateur local est la configuration minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).



## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Pour créer une règle pour passer ou filtrer une revendication entrante sur une confiance dans Windows Server 2016 

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **confiance**. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier la stratégie d’émission de revendication**.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Dans le **modifier la stratégie d’émission de revendication** boîte de dialogue **règles de transformation d’émission** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **envoyer les revendications à l’aide d’une règle personnalisée** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  Sur le **configurer la règle** page sous **nom de règle de revendication**, tapez le nom complet pour cette règle. Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication pour cette règle.  
![Créer la règle](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  Cliquez sur **Terminer**.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.   
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer une règle pour passer ou filtrer une revendication entrante sur une approbation de fournisseur de revendications dans Windows Server 2016 
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de fournisseur de revendications**. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Dans le **modifier les règles de revendication** boîte de dialogue **règles de transformation d’acceptation** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **envoyer les revendications à l’aide d’une règle personnalisée** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  Sur le **configurer la règle** page sous **nom de règle de revendication**, tapez le nom complet pour cette règle. Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication pour cette règle.  
![Créer la règle](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  Cliquez sur **Terminer**.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.   

















   
  
## <a name="to-create-a-rule-to-send-claims-by-using-a-custom-claim-in-windows-server-2012-r2"></a>Pour créer une règle pour envoyer les revendications à l’aide d’une revendication personnalisée dans Windows Server 2012 R2 
  
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
  
5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **envoyer les revendications à l’aide d’une règle personnalisée** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom1.PNG)   
  
6.  Sur le **configurer la règle** page sous **nom de règle de revendication**, tapez le nom complet pour cette règle. Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication pour cette règle.  
![Créer la règle](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom2.PNG)     

7.  Cliquez sur **Terminer**.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer les règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification : création de règles de revendication pour une approbation de partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  

[Liste de vérification : création de règles de revendication pour une approbation de fournisseur de revendications](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Rôle des revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Rôle des règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
