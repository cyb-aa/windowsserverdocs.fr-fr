---
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: Créer une règle pour envoyer des attributs LDAP en tant que revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d391c77ba309c84e2f8f8d0676b71b7c198fc241
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865940"
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>Créer une règle pour envoyer des attributs LDAP en tant que revendications


À l’aide du modèle de règle envoyer les attributs LDAP \(en\)tant que revendications dans services ADFS AD FS, vous pouvez créer une règle qui sélectionne les attributs \(d’un LDAP LDAP\)(LightweightDirectoryAccessProtocol).magasin d’attributs, tel que Active Directory, à envoyer en tant que revendications à la partie de confiance. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle envoyer les attributs LDAP en tant que règles de revendication qui extrait les valeurs d’attribut pour les utilisateurs authentifiés à partir des attributs **DisplayName** et **telephoneNumber** Active Directory, puis les envoie valeurs sous la forme de deux revendications sortantes différentes.  
  
Vous pouvez également utiliser cette règle pour envoyer toutes les appartenances aux groupes de l’utilisateur. Si vous souhaitez n’envoyer qu’une appartenance à un groupe, utilisez le modèle de règle Envoyer l’appartenance au groupe en tant que revendication. Vous pouvez utiliser la procédure suivante pour créer une règle de revendication avec le composant logiciel\-enfichable de gestion AD FS.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>Pour créer une règle pour envoyer des attributs LDAP en tant que revendications pour une approbation de partie de confiance dans Windows Server 2016 

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de partie de confiance**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Cliquez\-avec le bouton droit sur l’approbation sélectionnée, puis cliquez sur **modifier la stratégie d’émission de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Dans la boîte de dialogue **modifier la stratégie d’émission de revendication** , sous règles de transformation d' **émission** , cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **Envoyer les attributs LDAP en tant que revendications** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)    

6.  Dans la page **configurer la règle** , sous nom de la règle de **revendication** , tapez le nom complet de cette règle, sélectionnez le magasin d' **attributs**, puis sélectionnez l’attribut LDAP et mappez-le au type de revendication sortante. 
![créer une règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)    

7.  Cliquez sur le bouton **Terminer** .  
  
8.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK** pour enregistrer la règle.
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer une règle pour envoyer des attributs LDAP en tant que revendications pour une approbation de fournisseur de revendications dans Windows Server 2016 
  
1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de fournisseur de revendications**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Cliquez\-avec le bouton droit sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Dans la boîte de dialogue **modifier les règles de revendication** , sous règles de transformation d' **acceptation** , cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **Envoyer les attributs LDAP en tant que revendications** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)       

6.  Dans la page **configurer la règle** , sous nom de la règle de **revendication** , tapez le nom complet de cette règle, sélectionnez le magasin d' **attributs**, puis sélectionnez l’attribut LDAP et mappez-le au type de revendication sortante. 
![créer une règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)      

7.  Cliquez sur le bouton **Terminer** .  
  
8.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK** pour enregistrer la règle.  

 
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>Pour créer une règle pour envoyer des attributs LDAP en tant que revendications pour Windows Server 2012 R2  
  
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
  
5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **Envoyer les attributs LDAP en tant que revendications** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)  
  
6.  Dans la **page Configurer la règle** , sous nom de la règle de **revendication** , tapez le nom d’affichage de cette règle, sous l’option magasin d' **attributs** , sélectionnez **Active Directory**, puis sous **mappage des attributs LDAP aux types de revendications sortantes** , sélectionnez les L' **attribut LDAP** et les types de **type de revendication sortante** correspondants dans les listes déroulantes\-.  
  
    Vous devez sélectionner un nouvel attribut LDAP et une paire de type de revendication sortante sur une ligne différente pour chaque attribut Active Directory pour lequel vous souhaitez émettre une revendication dans le cadre de cette règle.  
![créer une règle](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)    
7.  Cliquez sur le bouton **Terminer** .  
  
8.  Dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK** pour enregistrer la règle.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer les règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification : création de règles de revendication pour une approbation de partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  

[Liste de vérification : création de règles de revendication pour une approbation de fournisseur de revendications](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Rôle des revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Rôle des règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
