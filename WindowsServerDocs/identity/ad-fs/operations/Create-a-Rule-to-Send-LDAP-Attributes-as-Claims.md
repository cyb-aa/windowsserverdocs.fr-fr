---
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: Créer une règle pour envoyer les attributs LDAP en tant que revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 00ea4f9f868b9c82c2a0859be971db26394251a3
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189351"
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>Créer une règle pour envoyer les attributs LDAP en tant que revendications


À l’aide de l’option Envoyer les attributs LDAP en tant que modèle de règle de revendications dans Active Directory Federation Services \(AD FS\), vous pouvez créer une règle qui sélectionne les attributs à partir d’un protocole Lightweight Directory Access \(LDAP\)magasin d’attributs, tels qu’Active Directory, à envoyer en tant que revendications à la partie de confiance. Par exemple, vous pouvez utiliser ce modèle de règle pour créer un envoyer les attributs LDAP comme les règles de revendication qui extrait les valeurs d’attribut pour les utilisateurs authentifiés à partir de la **displayName** et **telephoneNumber** Active Répertoire des attributs, puis envoyez ces valeurs en tant que deux revendications sortantes.  
  
Vous pouvez également utiliser cette règle pour envoyer les appartenances de l’utilisateur à des groupes. Si vous souhaitez n’envoyer qu’une appartenance à un groupe, utilisez le modèle de règle Envoyer l’appartenance au groupe en tant que revendication. Vous pouvez utiliser la procédure suivante pour créer une règle de revendication avec le composant logiciel enfichable Gestion AD FS\-dans.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>Pour créer une règle pour envoyer les attributs LDAP en tant que revendications pour une confiance dans Windows Server 2016 

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **confiance**. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier la stratégie d’émission de revendication**.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Dans le **modifier la stratégie d’émission de revendication** boîte de dialogue **règles de transformation d’émission** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **envoyer les attributs LDAP en tant que revendications** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)    

6.  Sur le **configurer la règle** page sous **nom de règle de revendication** taper le nom complet pour cette règle, sélectionnez le **Store de l’attribut**, puis sélectionnez l’attribut LDAP et mappez-le à le type de revendication sortante. 
![Créer la règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)    

7.  Cliquez sur le **Terminer** bouton.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer une règle pour envoyer les attributs LDAP en tant que revendications pour une approbation de fournisseur de revendications dans Windows Server 2016 
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de fournisseur de revendications**. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Dans le **modifier les règles de revendication** boîte de dialogue **règles de transformation d’acceptation** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **envoyer les attributs LDAP en tant que revendications** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)       

6.  Sur le **configurer la règle** page sous **nom de règle de revendication** taper le nom complet pour cette règle, sélectionnez le **Store de l’attribut**, puis sélectionnez l’attribut LDAP et mappez-le à le type de revendication sortante. 
![Créer la règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)      

7.  Cliquez sur le **Terminer** bouton.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.  

 
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>Pour créer une règle pour envoyer les attributs LDAP en tant que revendications pour Windows Server 2012 R2  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **FSAD AD FS\\relations d’approbation**, cliquez sur **approbations de fournisseur de revendications** ou **confiance**, puis cliquez sur un niveau de confiance spécifique dans la liste où vous souhaitez créer cette règle.  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez une les onglets suivants, en fonction de l’approbation que vous modifiez et quelle règle vous voulez créer cette règle dans, puis cliquez sur **ajouter une règle** pour démarrer la règle Assistant qui est associé à cet ensemble de règles :  
  
    -   **Règles de transformation d’acceptation**  
  
    -   **Règles de transformation d’émission**  
  
    -   **Règles d’autorisation d’émission**  
  
    -   **Règles d’autorisation de délégation**  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG) 
  
5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **envoyer les attributs LDAP en tant que revendications** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)  
  
6.  Sur le **configurer la règle** page sous **nom de règle de revendication** tapez le nom complet de cette règle, sous **magasin d’attributs** sélectionnez **Active Directory**, puis, sous **types de revendications de mappage d’attributs LDAP à sortant** sélectionnez souhaité **attribut LDAP** et correspondant **Type de revendication sortante** types dans la liste déroulante\-listes déroulantes.  
  
    Vous devez sélectionner un nouvel attribut LDAP et une paire de type revendication sortante sur une ligne différente pour chaque attribut Active Directory que vous souhaitez émettre une revendication pour le cadre de cette règle.  
![Créer la règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)    
7.  Cliquez sur le **Terminer** bouton.  
  
8.  Dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK** pour enregistrer la règle.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer les règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification : création de règles de revendication pour une approbation de partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  

[Liste de vérification : création de règles de revendication pour une approbation de fournisseur de revendications](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Rôle des revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Rôle des règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
