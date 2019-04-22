---
ms.assetid: 96b9f4e6-f01c-4517-8299-017d187d447e
title: Créer une règle pour envoyer une revendication de méthode d’authentification
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4065a61e042f52298da656899289e718e010f932
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819090"
---
# <a name="create-a-rule-to-send-an-authentication-method-claim"></a>Créer une règle pour envoyer une revendication de méthode d’authentification

>S'applique à : Windows Server 2016, Windows Server 2012 R2

Vous pouvez utiliser la **envoyer l’appartenance au groupe en tant que revendications** le modèle de règle ou le **transformer une revendication entrante** modèle de règle pour envoyer une revendication de méthode d’authentification. La partie de confiance peut utiliser une revendication de méthode d’authentification pour déterminer le mécanisme d’ouverture de session de l’utilisateur pour authentifier et obtenir des revendications à partir d’Active Directory Federation Services \(AD FS\). Vous pouvez également utiliser la fonctionnalité d’Assurance du mécanisme d’authentification d’Active Directory Federation Services \(AD FS\) dans Windows Server 2012 R2 en tant qu’entrée pour générer des revendications de méthode d’authentification pour les situations dans lesquelles la partie de confiance souhaite déterminer le niveau d’accès est basé sur les ouvertures de session de carte à puce. Par exemple, un développeur d’affecter différents niveaux d’accès pour les utilisateurs fédérés de l’application par partie de confiance. Les niveaux d’accès sont basés sur indique si les utilisateurs se connecter avec leurs nom et mot de passe informations d’identification utilisateur, par opposition à leurs cartes à puce.  
  
Selon les besoins de votre organisation, utilisez une des procédures suivantes :  
  
-   Créer cette règle à l’aide de la **envoyer l’appartenance au groupe en tant que revendications** le modèle de règle \- vous pouvez utiliser ce modèle de règle lorsque vous souhaitez que le groupe que vous spécifiez en fin de compte de ce modèle pour déterminer quelle méthode d’authentification revendication à émettre.  
  
-   Créer cette règle à l’aide de la **transformer une revendication entrante** le modèle de règle \- vous pouvez utiliser ce modèle de règle lorsque vous souhaitez modifier la méthode d’authentification existante à une nouvelle méthode d’authentification qui fonctionne avec un produit qui ne reconnaît pas les revendications standards de méthode d’authentification AD FS.  
  


## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Création à l’aide de l’appartenance au groupe Envoyer en tant que modèle de règle de revendications sur une confiance dans Windows Server 2016 

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **confiance**. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier la stratégie d’émission de revendication**.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Dans le **modifier la stratégie d’émission de revendication** boîte de dialogue **règles de transformation d’émission** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **envoyer l’appartenance au groupe en tant que revendication** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.  Sur le **configurer la règle** page, tapez un nom de règle de revendication.  
  
7.  Cliquez sur **Parcourir**, sélectionnez le groupe dont les membres doivent recevoir cette revendication de méthode d’authentification, puis cliquez sur **OK**.  
  
8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  
  
9. Dans **valeur de revendication sortante**, tapez une de l’identificateur de ressource uniforme par défaut \(URI\) les valeurs dans le tableau suivant, en fonction de votre méthode d’authentification recommandée, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  
  
|Méthode d’authentification réelle|URI correspondant|  
|--------------------------------|---------------------|  
|Authentification par nom d’utilisateur et mot de passe|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Authentification Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Transport Layer Security \(TLS\) l’authentification mutuelle qui utilise des certificats X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-en fonction d’authentification qui n’utilise pas TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Créer la règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)
  
## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Création à l’aide de l’appartenance au groupe Envoyer en tant que modèle de règle de revendications sur une approbation de fournisseur de revendications dans Windows Server 2016 
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de fournisseur de revendications**. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Dans le **modifier les règles de revendication** boîte de dialogue **règles de transformation d’acceptation** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **envoyer l’appartenance au groupe en tant que revendication** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.  Sur le **configurer la règle** page, tapez un nom de règle de revendication.  
  
7.  Cliquez sur **Parcourir**, sélectionnez le groupe dont les membres doivent recevoir cette revendication de méthode d’authentification, puis cliquez sur **OK**.  
  
8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  
  
9. Dans **valeur de revendication sortante**, tapez une de l’identificateur de ressource uniforme par défaut \(URI\) les valeurs dans le tableau suivant, en fonction de votre méthode d’authentification recommandée, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  
  
|Méthode d’authentification réelle|URI correspondant|  
|--------------------------------|---------------------|  
|Authentification par nom d’utilisateur et mot de passe|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Authentification Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Transport Layer Security \(TLS\) l’authentification mutuelle qui utilise des certificats X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-en fonction d’authentification qui n’utilise pas TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Créer la règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)


## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Pour créer cette règle à l’aide de la transformation une revendication entrante le modèle de règle sur une confiance dans Windows Server 2016 

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
  
7.  Dans **type de revendication entrante**, sélectionnez **méthode d’authentification** dans la liste.  
  
8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  
  
9. Sélectionnez **remplacer une valeur de revendication entrante avec une valeur de revendication sortante**, puis procédez comme suit :  
  
    1.  Dans **valeur de revendication entrante**, tapez une des valeurs URI suivantes qui reposent sur la méthode d’authentification réelle URI qui a été utilisé à l’origine, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  
  
    2.  Dans **valeur de revendication sortante**, tapez une des valeurs d’URI par défaut dans le tableau suivant, qui dépend de votre choix de méthode d’authentification préféré nouveau, cliquez sur **Terminer**, puis cliquez sur **OK**  pour enregistrer la règle.  
  
|Méthode d’authentification réelle|URI correspondant|  
|--------------------------------|---------------------|  
|Authentification par nom d’utilisateur et mot de passe|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Authentification Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Authentification mutuelle TLS qui utilise des certificats X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-en fonction d’authentification qui n’utilise pas TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Créer la règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)
  
> [!NOTE]  
> Autres valeurs d’URI peuvent être utilisées en plus des valeurs dans la table. Les valeurs d’URI qui sont affichés ion le tableau précédent reflètent les URI acceptant la partie de confiance par défaut.  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer cette règle à l’aide de la transformation une revendication entrante le modèle de règle sur une approbation de fournisseur de revendications dans Windows Server 2016 
  
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
  
7.  Dans **type de revendication entrante**, sélectionnez **méthode d’authentification** dans la liste.  
  
8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  
  
9. Sélectionnez **remplacer une valeur de revendication entrante avec une valeur de revendication sortante**, puis procédez comme suit :  
  
    1.  Dans **valeur de revendication entrante**, tapez une des valeurs URI suivantes qui reposent sur la méthode d’authentification réelle URI qui a été utilisé à l’origine, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  
  
    2.  Dans **valeur de revendication sortante**, tapez une des valeurs d’URI par défaut dans le tableau suivant, qui dépend de votre choix de méthode d’authentification préféré nouveau, cliquez sur **Terminer**, puis cliquez sur **OK**  pour enregistrer la règle.  
  
|Méthode d’authentification réelle|URI correspondant|  
|--------------------------------|---------------------|  
|Authentification par nom d’utilisateur et mot de passe|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Authentification Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Authentification mutuelle TLS qui utilise des certificats X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-en fonction d’authentification qui n’utilise pas TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Créer la règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)























## <a name="to-create-this-rule-by-using-the-send-group-membership-as-claims-rule-template-in-windows-server-2012-r2"></a>Pour créer cette règle à l’aide de l’appartenance au groupe Envoyer en tant que modèle de règle de revendications dans Windows Server 2012 R2  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Dans l’arborescence de la console, sous **AD FS\\relations d’approbation**, cliquez sur **approbations de fournisseur de revendications** ou **confiance**, puis cliquez sur un spécifique dans la liste où vous souhaitez créer cette règle d’approbation.  
  
3.  Droite\-cliquez sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez une les onglets suivants, en fonction de l’approbation que vous modifiez et quelle règle vous voulez créer cette règle dans, puis cliquez sur **ajouter une règle** pour démarrer la règle Assistant qui est associé à cet ensemble de règles :  
  
    -   **Règles de transformation d’acceptation**  
  
    -   **Règles de transformation d’émission**  
  
    -   **Règles d’autorisation d’émission**  
  
    -   **Règles d’autorisation de délégation**  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **envoyer l’appartenance au groupe en tant que revendication** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)
  
6.  Sur le **configurer la règle** page, tapez un nom de règle de revendication.  
  
7.  Cliquez sur **Parcourir**, sélectionnez le groupe dont les membres doivent recevoir cette revendication de méthode d’authentification, puis cliquez sur **OK**.  
  
8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  
  
9. Dans **valeur de revendication sortante**, tapez une de l’identificateur de ressource uniforme par défaut \(URI\) les valeurs dans le tableau suivant, en fonction de votre méthode d’authentification recommandée, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  
  
|Méthode d’authentification réelle|URI correspondant|  
|--------------------------------|---------------------|  
|Authentification par nom d’utilisateur et mot de passe|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Authentification Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Transport Layer Security \(TLS\) l’authentification mutuelle qui utilise des certificats X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-en fonction d’authentification qui n’utilise pas TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Créer la règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth1.PNG)
 
> [!NOTE]  
> Autres valeurs d’URI peuvent être utilisées en plus des valeurs dans la table. Les valeurs d’URI qui figurent dans le tableau précédent reflètent les URI acceptant la partie de confiance par défaut.  
  
## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Pour créer cette règle à l’aide de la transformation un modèle de règle de revendication entrante dans Windows Server 2012 R2  
   
  
  
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
  
7.  Dans **type de revendication entrante**, sélectionnez **méthode d’authentification** dans la liste.  
  
8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  
  
9. Sélectionnez **remplacer une valeur de revendication entrante avec une valeur de revendication sortante**, puis procédez comme suit :  
  
    1.  Dans **valeur de revendication entrante**, tapez une des valeurs URI suivantes qui reposent sur la méthode d’authentification réelle URI qui a été utilisé à l’origine, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  
  
    2.  Dans **valeur de revendication sortante**, tapez une des valeurs d’URI par défaut dans le tableau suivant, qui dépend de votre choix de méthode d’authentification préféré nouveau, cliquez sur **Terminer**, puis cliquez sur **OK**  pour enregistrer la règle.  
  
|Méthode d’authentification réelle|URI correspondant|  
|--------------------------------|---------------------|  
|Authentification par nom d’utilisateur et mot de passe|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Authentification Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Authentification mutuelle TLS qui utilise des certificats X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-en fonction d’authentification qui n’utilise pas TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Créer la règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth3.PNG)
  
> [!NOTE]  
> Autres valeurs d’URI peuvent être utilisées en plus des valeurs dans la table. Les valeurs d’URI qui sont affichés ion le tableau précédent reflètent les URI acceptant la partie de confiance par défaut.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer des règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification : Création de règles de revendication pour une partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  

[Liste de vérification : Création de règles de revendication pour un fournisseur de revendications d’approbation](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Le rôle de revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Le rôle de règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
