---
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: "Créer une règle pour envoyer les attributs LDAP en tant que revendications"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e9762e4bc50a1c2b862999af5269a0da376ec9a1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>Créer une règle pour envoyer les attributs LDAP en tant que revendications

>S’applique à: Windows Server2016, Windows Server2012R2

À l’aide de l’envoyer les attributs LDAP en tant que modèle de règle de revendications dans ActiveDirectory Federation Services \(ADFS\), vous pouvez créer une règle qui sélectionne les attributs d’un magasin d’attributs Lightweight Directory Access Protocol \(LDAP\), par exemple, ActiveDirectory, pour envoyer en tant que revendications à la partie de confiance. Par exemple, vous pouvez utiliser ce modèle de règle pour créer un envoyer les attributs LDAP, comme des règles de revendication qui extrait les valeurs d’attribut pour les utilisateurs authentifiés à partir de la **displayName** et **telephoneNumber** ActiveDirectory des attributs et puis envoyer ces valeurs en tant que deux revendications sortantes.  
  
Vous pouvez également utiliser cette règle pour envoyer les appartenances à un groupe de tous les l’utilisateur. Si vous souhaitez envoyer uniquement les appartenances de groupe individuels, utilisez l’appartenance au groupe Envoyer en tant qu’un modèle de règle de revendication. Vous pouvez utiliser la procédure suivante pour créer une règle de revendication avec la gestion ADFS de composants.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>Pour créer une règle pour envoyer les attributs LDAP en tant que revendications pour une confiance dans Windows Server2016 

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **ADFS**, cliquez sur **approbations de partie de confiance **. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  L’approbation sélectionnée clic droit, puis cliquez sur **modifier la stratégie d’émission de revendication **.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Dans le **modifier la stratégie d’émission de revendication** la boîte de dialogue sous **règles de transformation d’émission** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **envoyer les attributs LDAP en tant que revendications** dans la liste, puis cliquez sur **suivant **.  
![Créer la règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)    

6.  Sur le **configurer la règle** page sous **nom de règle de revendication** tapez le nom d’affichage pour cette règle, sélectionnez le **magasin d’attributs**, puis sélectionnez l’attribut LDAP et le mapper au type de revendication sortante. 
![Créer la règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)    

7.  Cliquez sur le **Terminer** bouton.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer une règle pour envoyer les attributs LDAP en tant que revendications pour une approbation de fournisseur de revendications dans Windows Server2016 
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **ADFS**, cliquez sur **approbations de fournisseur de revendications **. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  L’approbation sélectionnée clic droit, puis cliquez sur **modifier les règles de revendication **.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Dans le **modifier les règles de revendication** la boîte de dialogue sous **règles de transformation d’acceptation** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **envoyer les attributs LDAP en tant que revendications** dans la liste, puis cliquez sur **suivant **.  
![Créer la règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)       

6.  Sur le **configurer la règle** page sous **nom de règle de revendication** tapez le nom d’affichage pour cette règle, sélectionnez le **magasin d’attributs**, puis sélectionnez l’attribut LDAP et le mapper au type de revendication sortante. 
![Créer la règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)      

7.  Cliquez sur le **Terminer** bouton.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.  

 
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>Pour créer une règle pour envoyer les attributs LDAP en tant que revendications pour Windows Server2012R2  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **ADFSAD FS\\Trust relations**, cliquez sur **approbations de fournisseur de revendications** ou **approbations de partie de confiance**, puis cliquez sur une approbation spécifique dans la liste dans laquelle vous souhaitez créer cette règle.  
  
3.  L’approbation sélectionnée clic droit, puis cliquez sur **modifier les règles de revendication **.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez une les onglets suivants, en fonction de l’approbation que vous modifiez et que la règle vous voulez créer cette règle, puis cliquez sur **ajouter une règle** pour démarrer l’Assistant règle qui est associé à cet ensemble de règles:  
  
    -   **Règles de transformation d’acceptation**  
  
    -   **Règles de transformation d’émission**  
  
    -   **Règles d’autorisation d’émission**  
  
    -   **Règles d’autorisation de délégation**  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG) 
  
5.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **envoyer les attributs LDAP en tant que revendications** dans la liste, puis cliquez sur **suivant **.  
![Créer la règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)  
  
6.  Sur le **configurer la règle** page sous **nom de règle de revendication** tapez le nom d’affichage pour cette règle, sous **magasin d’attributs** sélectionnez **ActiveDirectory**et sous **types de revendication de mappage d’attributs LDAP à sortant** sélectionnez l’élément **attribut LDAP** et correspondant **Type de revendication sortante** types dans les listes déroulantes.  
  
    Vous devez sélectionner un nouvel attribut LDAP et une paire de type revendication sortante sur une autre ligne pour chaque attribut ActiveDirectory que vous souhaitez émettre une revendication pour dans le cadre de cette règle.  
![Créer la règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)    
7.  Cliquez sur le **Terminer** bouton.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer des règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification: Création de règles de revendication pour une partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  

[Liste de vérification: Approuvent créer des règles de revendication pour un fournisseur de revendications](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Le rôle de revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Le rôle de règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
