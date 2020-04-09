---
ms.assetid: 38eb3726-e97b-484e-9926-67e8a046b0c5
title: Créer une règle pour envoyer des revendications à l’aide d’une règle personnalisée
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 81a1fbbd703a5d452c437b089b822e227f55af82
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816692"
---
# <a name="create-a-rule-to-send-claims-using-a-custom-rule"></a>Créer une règle pour envoyer des revendications à l’aide d’une règle personnalisée


En utilisant le modèle de **règle envoyer des revendications à l’aide d’une règle personnalisée** dans Services ADFS (AD FS), vous pouvez créer des règles de revendication personnalisées pour la situation dans laquelle un modèle de règle standard ne répond pas aux exigences de votre organisation. Les règles de revendication personnalisées sont écrites dans le langage de règle de revendication et doivent ensuite être copiées dans la zone de texte **règle personnalisée** avant de pouvoir être utilisées dans un ensemble de règles. Pour plus d’informations sur la construction de la syntaxe d’une règle avancée, consultez [le rôle du langage de règle de revendication](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md).  
  
Vous pouvez utiliser la procédure suivante pour créer une règle de revendication à l’aide du\-du composant logiciel enfichable de gestion AD FS dans.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **administrateurs**ou à un groupe équivalent sur l’ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).



## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Pour créer une règle de transmission ou de filtrage d’une revendication entrante sur une approbation de partie de confiance dans Windows Server 2016 

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de partie de confiance**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Cliquez avec le bouton droit\-sur l’approbation sélectionnée, puis cliquez sur **modifier la stratégie d’émission de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Dans la boîte de dialogue **modifier la stratégie d’émission de revendication** , sous règles de transformation d' **émission** , cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Dans la page **Sélectionner un modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **Envoyer des revendications à l’aide d’une règle personnalisée** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  Dans la page **configurer la règle** , sous nom de la règle de **revendication**, tapez le nom complet de cette règle. Sous **règle personnalisée**, tapez ou collez la syntaxe du langage de règle de revendication que vous souhaitez pour cette règle.  
![créer une règle](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  Cliquez sur **Terminer**.  
  
8.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK** pour enregistrer la règle.   
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer une règle de transmission ou de filtrage d’une revendication entrante sur une approbation de fournisseur de revendications dans Windows Server 2016 
  
1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de fournisseur de revendications**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Cliquez avec le bouton droit\-sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Dans la boîte de dialogue **modifier les règles de revendication** , sous règles de transformation d' **acceptation** , cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Dans la page **Sélectionner un modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **Envoyer des revendications à l’aide d’une règle personnalisée** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  Dans la page **configurer la règle** , sous nom de la règle de **revendication**, tapez le nom complet de cette règle. Sous **règle personnalisée**, tapez ou collez la syntaxe du langage de règle de revendication que vous souhaitez pour cette règle.  
![créer une règle](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  Cliquez sur **Terminer**.  
  
8.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK** pour enregistrer la règle.   

















   
  
## <a name="to-create-a-rule-to-send-claims-by-using-a-custom-claim-in-windows-server-2012-r2"></a>Pour créer une règle pour envoyer des revendications à l’aide d’une revendication personnalisée dans Windows Server 2012 R2 
  
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
  
5.  Dans la page **Sélectionner un modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **Envoyer des revendications à l’aide d’une règle personnalisée** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom1.PNG)   
  
6.  Dans la page **configurer la règle** , sous nom de la règle de **revendication**, tapez le nom complet de cette règle. Sous **règle personnalisée**, tapez ou collez la syntaxe du langage de règle de revendication que vous souhaitez pour cette règle.  
![créer une règle](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom2.PNG)     

7.  Cliquez sur **Terminer**.  
  
8.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK** pour enregistrer la règle.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer les règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification : création de règles de revendication pour une approbation de partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  

[Liste de vérification : création de règles de revendication pour une approbation de fournisseur de revendications](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Rôle des revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Rôle des règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
