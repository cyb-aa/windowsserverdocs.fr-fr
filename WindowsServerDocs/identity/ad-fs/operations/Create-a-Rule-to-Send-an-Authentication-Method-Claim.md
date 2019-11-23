---
ms.assetid: 96b9f4e6-f01c-4517-8299-017d187d447e
title: Créer une règle pour envoyer une revendication de méthode d’authentification
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4efcae02b96904c9f869a5ed9e14eba161892b74
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358160"
---
# <a name="create-a-rule-to-send-an-authentication-method-claim"></a>Créer une règle pour envoyer une revendication de méthode d’authentification


Vous pouvez utiliser le modèle de règle envoyer l’appartenance à un **groupe en tant que revendications** ou le modèle de règle **transformer une revendication entrante** pour envoyer une revendication de méthode d’authentification. La partie de confiance peut utiliser une revendication de méthode d’authentification pour déterminer le mécanisme d’ouverture de session que l’utilisateur utilise pour s’authentifier et obtenir des revendications à partir de Services ADFS \(AD FS\). Vous pouvez également utiliser la fonctionnalité d’assurance du mécanisme d’authentification de Services ADFS \(AD FS\) dans Windows Server 2012 R2 comme entrée pour générer des revendications de méthode d’authentification dans des situations où la partie de confiance souhaite déterminer le niveau d’accès basé sur les ouvertures de session par carte à puce. Par exemple, un développeur peut attribuer différents niveaux d’accès aux utilisateurs fédérés de l’application par partie de confiance. Les niveaux d’accès sont basés sur le fait que les utilisateurs se connectent avec leurs informations d’identification de nom d’utilisateur et de mot de passe, par opposition à leurs cartes à puce.  

Selon les exigences de votre organisation, utilisez l’une des procédures suivantes :  

-   Créez cette règle à l’aide du modèle de règle **Envoyer l’appartenance au groupe en tant que revendications** \- vous pouvez utiliser ce modèle de règle lorsque vous souhaitez que le groupe que vous spécifiez dans ce modèle détermine en fin de compte la revendication de méthode d’authentification à émettre.  

-   Créez cette règle à l’aide du modèle de règle **transformer une revendication entrante** \- vous pouvez utiliser ce modèle de règle lorsque vous souhaitez modifier la méthode d’authentification existante en une nouvelle méthode d’authentification qui fonctionne avec un produit qui ne reconnaît pas les revendications de méthode d’authentification AD FS standard.  



## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Pour créer à l’aide du modèle de règle envoyer l’appartenance à un groupe en tant que revendications sur une approbation de partie de confiance dans Windows Server 2016 

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  

2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de partie de confiance**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  

3.  Cliquez avec le bouton droit\-sur l’approbation sélectionnée, puis cliquez sur **modifier la stratégie d’émission de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   

4.  Dans la boîte de dialogue **modifier la stratégie d’émission de revendication** , sous règles de transformation d' **émission** , cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **Envoyer l’appartenance à un groupe en tant que revendication** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.  Dans la page **configurer la règle** , tapez un nom de règle de revendication.  

7.  Cliquez sur **Parcourir**, sélectionnez le groupe dont les membres doivent recevoir cette revendication de méthode d’authentification, puis cliquez sur **OK**.  

8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  

9. Dans **valeur de revendication sortante**, tapez l’un des identificateurs de ressource uniformes par défaut \(URI\) valeurs dans le tableau suivant, en fonction de votre méthode d’authentification par défaut, cliquez sur **Terminer**, puis sur **OK** pour enregistrer la règle.  

|                            Méthode d’authentification réelle                             |                                URI correspondant                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        Authentification par nom d’utilisateur et mot de passe                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Authentification Windows                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| Transport Layer Security \(TLS\) l’authentification mutuelle qui utilise des certificats X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  Authentification\-basée sur X. 509 qui n’utilise pas TLS                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![créer une règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)

## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer à l’aide du modèle de règle envoyer l’appartenance à un groupe en tant que revendications sur une approbation de fournisseur de revendications dans Windows Server 2016 

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  

2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de fournisseur de revendications**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  

3.  Cliquez avec le bouton droit\-sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   

4.  Dans la boîte de dialogue **modifier les règles de revendication** , sous règles de transformation d' **acceptation** , cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **Envoyer l’appartenance à un groupe en tant que revendication** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.  Dans la page **configurer la règle** , tapez un nom de règle de revendication.  

7.  Cliquez sur **Parcourir**, sélectionnez le groupe dont les membres doivent recevoir cette revendication de méthode d’authentification, puis cliquez sur **OK**.  

8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  

9. Dans **valeur de revendication sortante**, tapez l’un des identificateurs de ressource uniformes par défaut \(URI\) valeurs dans le tableau suivant, en fonction de votre méthode d’authentification par défaut, cliquez sur **Terminer**, puis sur **OK** pour enregistrer la règle.  

|                            Méthode d’authentification réelle                             |                                URI correspondant                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        Authentification par nom d’utilisateur et mot de passe                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Authentification Windows                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| Transport Layer Security \(TLS\) l’authentification mutuelle qui utilise des certificats X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  Authentification\-basée sur X. 509 qui n’utilise pas TLS                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![créer une règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)


## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Pour créer cette règle à l’aide du modèle de règle transformer une revendication entrante sur une approbation de partie de confiance dans Windows Server 2016 

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  

2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de partie de confiance**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  

3.  Cliquez avec le bouton droit\-sur l’approbation sélectionnée, puis cliquez sur **modifier la stratégie d’émission de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   

4.  Dans la boîte de dialogue **modifier la stratégie d’émission de revendication** , sous règles de transformation d' **émission** , cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **transformer une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Dans la page **configurer la règle** , tapez un nom de règle de revendication.  

7.  Dans **type de revendication entrante**, sélectionnez **méthode d’authentification** dans la liste.  

8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  

9. Sélectionnez **remplacer une valeur de revendication entrante par une autre valeur de revendication sortante**, puis procédez comme suit :  

    1.  Dans **valeur de revendication entrante**, tapez l’une des valeurs d’URI suivantes qui sont basées sur l’URI de la méthode d’authentification réel qui a été utilisé à l’origine, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  

    2.  Dans **valeur de revendication sortante**, tapez l’une des valeurs d’URI par défaut dans le tableau suivant, qui dépend de votre choix de méthode d’authentification par défaut, cliquez sur **Terminer**, puis sur **OK** pour enregistrer la règle.  

|              Méthode d’authentification réelle              |                                URI correspondant                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         Authentification par nom d’utilisateur et mot de passe          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Authentification Windows                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| Authentification mutuelle TLS qui utilise des certificats X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   Authentification\-basée sur X. 509 qui n’utilise pas TLS    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![créer une règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)

> [!NOTE]  
> D’autres valeurs d’URI peuvent être utilisées en plus des valeurs de la table. Les valeurs d’URI présentées dans le tableau précédent reflètent les URI que la partie de confiance accepte par défaut.  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Pour créer cette règle à l’aide du modèle de règle transformer une revendication entrante sur une approbation de fournisseur de revendications dans Windows Server 2016 

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  

2.  Dans l’arborescence de la console, sous **AD FS**, cliquez sur **approbations de fournisseur de revendications**. 
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  

3.  Cliquez avec le bouton droit\-sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   

4.  Dans la boîte de dialogue **modifier les règles de revendication** , sous règles de transformation d' **acceptation** , cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **transformer une revendication entrante** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Dans la page **configurer la règle** , tapez un nom de règle de revendication.  

7.  Dans **type de revendication entrante**, sélectionnez **méthode d’authentification** dans la liste.  

8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  

9. Sélectionnez **remplacer une valeur de revendication entrante par une autre valeur de revendication sortante**, puis procédez comme suit :  

    1.  Dans **valeur de revendication entrante**, tapez l’une des valeurs d’URI suivantes qui sont basées sur l’URI de la méthode d’authentification réel qui a été utilisé à l’origine, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  

    2.  Dans **valeur de revendication sortante**, tapez l’une des valeurs d’URI par défaut dans le tableau suivant, qui dépend de votre choix de méthode d’authentification par défaut, cliquez sur **Terminer**, puis sur **OK** pour enregistrer la règle.  

|              Méthode d’authentification réelle              |                                URI correspondant                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         Authentification par nom d’utilisateur et mot de passe          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Authentification Windows                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| Authentification mutuelle TLS qui utilise des certificats X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   Authentification\-basée sur X. 509 qui n’utilise pas TLS    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![créer une règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)























## <a name="to-create-this-rule-by-using-the-send-group-membership-as-claims-rule-template-in-windows-server-2012-r2"></a>Pour créer cette règle à l’aide du modèle de règle envoyer l’appartenance à un groupe en tant que revendications dans Windows Server 2012 R2  

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  

2.  Dans l’arborescence de la console, sous **AD FS\\relations d’approbation**, cliquez sur approbations de **fournisseur de revendications** ou **approbations de partie de confiance**, puis cliquez sur une approbation spécifique dans la liste dans laquelle vous souhaitez créer cette règle.  

3.  Cliquez avec le bouton droit\-sur l’approbation sélectionnée, puis cliquez sur **modifier les règles de revendication**.
![créer une règle](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  

4.  Dans la boîte de dialogue **modifier les règles de revendication** , sélectionnez l’un des onglets suivants, en fonction de l’approbation que vous modifiez et de l’ensemble de règles dans lequel vous souhaitez créer cette règle, puis cliquez sur Ajouter une **règle** pour démarrer l’Assistant règle associé à cet ensemble de règles :  

    -   **Règles de transformation d’acceptation**  

    -   **Règles de transformation d’émission**  

    -   **Règles d’autorisation d’émission**  

    -   **Règles d’autorisation de délégation**  
![créer une règle](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  Dans la page **Sélectionner le modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **Envoyer l’appartenance à un groupe en tant que revendication** dans la liste, puis cliquez sur **suivant**.  
![créer une règle](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)

6.  Dans la page **configurer la règle** , tapez un nom de règle de revendication.  

7.  Cliquez sur **Parcourir**, sélectionnez le groupe dont les membres doivent recevoir cette revendication de méthode d’authentification, puis cliquez sur **OK**.  

8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  

9. Dans **valeur de revendication sortante**, tapez l’un des identificateurs de ressource uniformes par défaut \(URI\) valeurs dans le tableau suivant, en fonction de votre méthode d’authentification par défaut, cliquez sur **Terminer**, puis sur **OK** pour enregistrer la règle.  

|                            Méthode d’authentification réelle                             |                                URI correspondant                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        Authentification par nom d’utilisateur et mot de passe                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Authentification Windows                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| Transport Layer Security \(TLS\) l’authentification mutuelle qui utilise des certificats X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  Authentification\-basée sur X. 509 qui n’utilise pas TLS                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![créer une règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth1.PNG)

> [!NOTE]  
> D’autres valeurs d’URI peuvent être utilisées en plus des valeurs de la table. Les valeurs d’URI indiquées dans le tableau précédent reflètent les URI que la partie de confiance accepte par défaut.  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Pour créer cette règle à l’aide du modèle de règle transformer une revendication entrante dans Windows Server 2012 R2  



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

6.  Dans la page **configurer la règle** , tapez un nom de règle de revendication.  

7.  Dans **type de revendication entrante**, sélectionnez **méthode d’authentification** dans la liste.  

8.  Dans **type de revendication sortante**, sélectionnez **méthode d’authentification** dans la liste.  

9. Sélectionnez **remplacer une valeur de revendication entrante par une autre valeur de revendication sortante**, puis procédez comme suit :  

    1.  Dans **valeur de revendication entrante**, tapez l’une des valeurs d’URI suivantes qui sont basées sur l’URI de la méthode d’authentification réel qui a été utilisé à l’origine, cliquez sur **Terminer**, puis cliquez sur **OK** pour enregistrer la règle.  

    2.  Dans **valeur de revendication sortante**, tapez l’une des valeurs d’URI par défaut dans le tableau suivant, qui dépend de votre choix de méthode d’authentification par défaut, cliquez sur **Terminer**, puis sur **OK** pour enregistrer la règle.  

|              Méthode d’authentification réelle              |                                URI correspondant                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         Authentification par nom d’utilisateur et mot de passe          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Authentification Windows                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| Authentification mutuelle TLS qui utilise des certificats X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   Authentification\-basée sur X. 509 qui n’utilise pas TLS    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![créer une règle](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth3.PNG)

> [!NOTE]  
> D’autres valeurs d’URI peuvent être utilisées en plus des valeurs de la table. Les valeurs d’URI présentées dans le tableau précédent reflètent les URI que la partie de confiance accepte par défaut.  

## <a name="additional-references"></a>Références supplémentaires 
[Configurer les règles de revendication](Configure-Claim-Rules.md)  

[Liste de vérification : création de règles de revendication pour une approbation de partie de confiance](https://technet.microsoft.com/library/ee913578.aspx)  

[Liste de vérification : création de règles de revendication pour une approbation de fournisseur de revendications](https://technet.microsoft.com/library/ee913564.aspx)  

[Quand utiliser une règle de revendication d’autorisation](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Rôle des revendications](../../ad-fs/technical-reference/The-Role-of-Claims.md)  

[Rôle des règles de revendication](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
