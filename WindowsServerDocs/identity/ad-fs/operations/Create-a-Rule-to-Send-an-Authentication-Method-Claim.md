---
ms.assetid: 96b9f4e6-f01c-4517-8299-017d187d447e
title: "Créer une règle pour envoyer une revendication de méthode d’authentification"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4065a61e042f52298da656899289e718e010f932
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-an-authentication-method-claim"></a>Créer une règle pour envoyer une revendication de méthode d’authentification

>S’applique à: Windows Server2016, Windows Server2012R2

Vous pouvez utiliser le **envoyer l’appartenance au groupe en tant que revendications** modèle de règle ou le **transformer une revendication entrante** modèle de règle pour envoyer une revendication de méthode d’authentification. La partie de confiance permet de déterminer le mécanisme d’ouverture de session de l’utilisateur utilise pour authentifier et obtenir des revendications à partir d’ActiveDirectory Federation Services \(ADFS\) une revendication de méthode d’authentification. Vous pouvez également utiliser la fonctionnalité de l’Assurance du mécanisme d’authentification d’ActiveDirectory Federation Services \(ADFS\) dans Windows Server2012R2 en tant qu’entrée pour générer des revendications de méthode d’authentification pour les situations dans lesquelles la partie de confiance souhaite déterminer le niveau d’accès basé sur les ouvertures de session de carte à puce. Par exemple, un développeur peut attribuer différents niveaux d’accès pour les utilisateurs fédérés de l’application de partie de confiance. Les niveaux d’accès sont basées sur indique si les utilisateurs ouvrent une session avec leurs nom et mot de passe informations d’identification utilisateur, par opposition à leurs cartes à puce.  
  
Selon les besoins de votre organisation, utilisez une des procédures suivantes:  
  
-   Créez cette règle à l’aide de la **envoyer l’appartenance au groupe en tant que revendications** modèle de règle \-vous pouvez utiliser ce modèle de règle lorsque vous souhaitez que le groupe que vous spécifiez dans ce modèle détermine quelle méthode d’authentification de revendication émettre.  
  
-   Créez cette règle à l’aide de la **transformer une revendication entrante** modèle de règle \-vous pouvez utiliser ce modèle de règle lorsque vous souhaitez modifier la méthode d’authentification existante à une nouvelle méthode d’authentification qui fonctionne avec un produit qui ne reconnaît pas standards revendications de méthode d’authentification ADFS.  
  


## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Créer à l’aide de l’appartenance au groupe Envoyer en tant que modèle de règle de revendications sur une confiance dans Windows Server2016 

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **ADFS**, cliquez sur **approbations de partie de confiance **. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  L’approbation sélectionnée clic droit, puis cliquez sur **modifier la stratégie d’émission de revendication **.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Dans le **modifier la stratégie d’émission de revendication** la boîte de dialogue sous **règles de transformation d’émission** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **envoyer l’appartenance au groupe en tant que revendication** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.  Sur le **configurer la règle**, tapez un nom de règle de revendication.  
  
7.  Cliquez sur **Parcourir**, sélectionnez le groupe dont les membres doivent recevoir cette revendication de méthode d’authentification, puis cliquez sur **OK**.  
  
8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  
  
9. Dans **valeur de revendication sortante**, tapez une des valeurs par défaut uniform resource identifier identificateur \(URI\) dans le tableau suivant, en fonction de votre méthode d’authentification préféré, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  
  
|Méthode d’authentification réelle|URI correspondant|  
|--------------------------------|---------------------|  
|Nom et mot de passe de l’authentification utilisateur|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Authentification Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Authentification mutuelle Transport Layer Security \(TLS\) qui utilise des certificats X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Authentification basée sur les X.509\ qui n’utilise pas TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Créer la règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)
  
## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Créer à l’aide de l’appartenance au groupe Envoyer en tant que modèle de règle de revendications sur une approbation de fournisseur de revendications dans Windows Server2016 
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **ADFS**, cliquez sur **approbations de fournisseur de revendications **. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  L’approbation sélectionnée clic droit, puis cliquez sur **modifier les règles de revendication **.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Dans le **modifier les règles de revendication** la boîte de dialogue sous **règles de transformation d’acceptation** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **envoyer l’appartenance au groupe en tant que revendication** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.  Sur le **configurer la règle**, tapez un nom de règle de revendication.  
  
7.  Cliquez sur **Parcourir**, sélectionnez le groupe dont les membres doivent recevoir cette revendication de méthode d’authentification, puis cliquez sur **OK**.  
  
8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  
  
9. Dans **valeur de revendication sortante**, tapez une des valeurs par défaut uniform resource identifier identificateur \(URI\) dans le tableau suivant, en fonction de votre méthode d’authentification préféré, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  
  
|Méthode d’authentification réelle|URI correspondant|  
|--------------------------------|---------------------|  
|Nom et mot de passe de l’authentification utilisateur|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Authentification Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Authentification mutuelle Transport Layer Security \(TLS\) qui utilise des certificats X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Authentification basée sur les X.509\ qui n’utilise pas TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Créer la règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)


## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Pour créer cette règle à l’aide de la transformation une revendication entrante modèle de règle sur une confiance dans Windows Server2016 

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **ADFS**, cliquez sur **approbations de partie de confiance **. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  L’approbation sélectionnée clic droit, puis cliquez sur **modifier la stratégie d’émission de revendication **.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Dans le **modifier la stratégie d’émission de revendication** la boîte de dialogue sous **règles de transformation d’émission** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **transformer une revendication entrante** dans la liste, puis cliquez sur **suivant **.  
![Créer la règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Sur le **configurer la règle**, tapez un nom de règle de revendication.  
  
7.  Dans **type de revendication entrante**, sélectionnez **méthode d’authentification** dans la liste.  
  
8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  
  
9. Sélectionnez **remplacer une valeur de revendication entrante avec une valeur de revendication sortante**, puis procédez comme suit:  
  
    1.  Dans **valeur de revendication entrante**, tapez une des valeurs suivantes URI qui sont basées sur la méthode d’authentification réel URI qui a été utilisé à l’origine, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  
  
    2.  Dans **valeur de revendication sortante**, tapez une des valeurs par défaut URI dans le tableau suivant, qui dépend de votre choix de méthode d’authentification préféré nouveau, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  
  
|Méthode d’authentification réel|URI correspondant|  
|--------------------------------|---------------------|  
|Nom et mot de passe de l’authentification utilisateur|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Authentification Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Authentification mutuelle TLS qui utilise des certificats X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Authentification basée sur les X.509\ qui n’utilise pas TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Créer la règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)
  
> [!NOTE]  
> Autres valeurs d’URI peuvent être utilisées en plus des valeurs dans le tableau. Les valeurs d’URI répertoriés ion du tableau précédent reflètent les URI qui accepte les la partie de confiance par défaut.  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer cette règle à l’aide de la transformation une revendication entrante modèle de règle sur une approbation de fournisseur de revendications dans Windows Server2016 
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **ADFS**, cliquez sur **approbations de fournisseur de revendications **. 
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  L’approbation sélectionnée clic droit, puis cliquez sur **modifier les règles de revendication **.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Dans le **modifier les règles de revendication** la boîte de dialogue sous **règles de transformation d’acceptation** cliquez sur **ajouter une règle** pour démarrer l’Assistant règle.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **transformer une revendication entrante** dans la liste, puis cliquez sur **suivant **.  
![Créer la règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Sur le **configurer la règle**, tapez un nom de règle de revendication.  
  
7.  Dans **type de revendication entrante**, sélectionnez **méthode d’authentification** dans la liste.  
  
8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  
  
9. Sélectionnez **remplacer une valeur de revendication entrante avec une valeur de revendication sortante**, puis procédez comme suit:  
  
    1.  Dans **valeur de revendication entrante**, tapez une des valeurs suivantes URI qui sont basées sur la méthode d’authentification réel URI qui a été utilisé à l’origine, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  
  
    2.  Dans **valeur de revendication sortante**, tapez une des valeurs par défaut URI dans le tableau suivant, qui dépend de votre choix de méthode d’authentification préféré nouveau, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  
  
|Méthode d’authentification réel|URI correspondant|  
|--------------------------------|---------------------|  
|Nom et mot de passe de l’authentification utilisateur|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Authentification Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Authentification mutuelle TLS qui utilise des certificats X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Authentification basée sur les X.509\ qui n’utilise pas TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Créer la règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)























## <a name="to-create-this-rule-by-using-the-send-group-membership-as-claims-rule-template-in-windows-server-2012-r2"></a>Pour créer cette règle à l’aide de l’appartenance au groupe Envoyer en tant que modèle de règle de revendications dans Windows Server2012R2  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **ADFS\\Trust relations**, cliquez sur **approbations de fournisseur de revendications** ou **approbations de partie de confiance**, puis cliquez sur une approbation spécifique dans la liste dans laquelle vous souhaitez créer cette règle.  
  
3.  L’approbation sélectionnée clic droit, puis cliquez sur **modifier les règles de revendication **.
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez une les onglets suivants, en fonction de l’approbation que vous modifiez et que la règle vous voulez créer cette règle, puis cliquez sur **ajouter une règle** pour démarrer l’Assistant règle qui est associé à cet ensemble de règles:  
  
    -   **Règles de transformation d’acceptation**  
  
    -   **Règles de transformation d’émission**  
  
    -   **Règles d’autorisation d’émission**  
  
    -   **Règles d’autorisation de délégation**  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **envoyer l’appartenance comme une revendication** dans la liste, puis cliquez sur **suivant**.  
![Créer la règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)
  
6.  Sur le **configurer la règle**, tapez un nom de règle de revendication.  
  
7.  Cliquez sur **Parcourir**, sélectionnez le groupe dont les membres doivent recevoir cette revendication de méthode d’authentification, puis cliquez sur **OK**.  
  
8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  
  
9. Dans **valeur de revendication sortante**, tapez une des valeurs par défaut uniform resource identifier identificateur \(URI\) dans le tableau suivant, en fonction de votre méthode d’authentification préféré, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  
  
|Méthode d’authentification réelle|URI correspondant|  
|--------------------------------|---------------------|  
|Nom et mot de passe de l’authentification utilisateur|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Authentification Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Authentification mutuelle Transport Layer Security \(TLS\) qui utilise des certificats X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Authentification basée sur les X.509\ qui n’utilise pas TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Créer la règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth1.PNG)
 
> [!NOTE]  
> Autres valeurs d’URI peuvent être utilisées en plus des valeurs dans le tableau. Les valeurs d’URI qui sont affichées dans le tableau précédent reflètent les URI qui accepte les la partie de confiance par défaut.  
  
## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Pour créer cette règle à l’aide de la transformation un modèle de règle de revendication entrante dans Windows Server2012R2  
   
  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **gestion ADFS **.  
  
2.  Dans l’arborescence de la console, sous **ADFS\\Trust relations**, cliquez sur **approbations de fournisseur de revendications** ou **approbations de partie de confiance**, puis cliquez sur une approbation spécifique dans la liste dans laquelle vous souhaitez créer cette règle.  
  
3.  L’approbation sélectionnée clic droit, puis cliquez sur **modifier les règles de revendication **.  
![Créer la règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
 
4.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez une les onglets suivants, qui dépend de l’approbation que vous modifiez et dans la règle définissez vous souhaitent créer cette règle, puis cliquez sur **ajouter une règle** pour démarrer l’Assistant règle qui est associé à cet ensemble de règles:  
  
    -   **Règles de transformation d’acceptation**  
  
    -   **Règles de transformation d’émission**  
  
    -   **Règles d’autorisation d’émission**  
  
    -   **Règles d’autorisation de délégation**  
![Créer la règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **transformer une revendication entrante** dans la liste, puis cliquez sur **suivant **.  
![Créer la règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)    
  
6.  Sur le **configurer la règle**, tapez un nom de règle de revendication.  
  
7.  Dans **type de revendication entrante**, sélectionnez **méthode d’authentification** dans la liste.  
  
8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  
  
9. Sélectionnez **remplacer une valeur de revendication entrante avec une valeur de revendication sortante**, puis procédez comme suit:  
  
    1.  Dans **valeur de revendication entrante**, tapez une des valeurs suivantes URI qui sont basées sur la méthode d’authentification réel URI qui a été utilisé à l’origine, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  
  
    2.  Dans **valeur de revendication sortante**, tapez une des valeurs par défaut URI dans le tableau suivant, qui dépend de votre choix de méthode d’authentification préféré nouveau, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  
  
|Méthode d’authentification réel|URI correspondant|  
|--------------------------------|---------------------|  
|Nom et mot de passe de l’authentification utilisateur|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Authentification Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Authentification mutuelle TLS qui utilise des certificats X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Authentification basée sur les X.509\ qui n’utilise pas TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Créer la règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth3.PNG)
  
> [!NOTE]  
> Autres valeurs d’URI peuvent être utilisées en plus des valeurs dans le tableau. Les valeurs d’URI répertoriés ion du tableau précédent reflètent les URI qui accepte les la partie de confiance par défaut.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer des règles de revendication](Configure-Claim-Rules.md)  
 
[Liste de vérification: Création de règles de revendication pour une partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  

[Liste de vérification: Approuvent créer des règles de revendication pour un fournisseur de revendications](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Le rôle de revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Le rôle de règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
