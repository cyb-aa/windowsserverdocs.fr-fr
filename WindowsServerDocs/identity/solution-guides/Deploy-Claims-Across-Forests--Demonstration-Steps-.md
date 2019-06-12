---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: Déployer les revendications entre les forêts (procédure descriptive)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a6f2e5d3a227384b20735ab99ee1ab5ea77bd913
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445844"
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>Déployer les revendications entre les forêts (procédure descriptive)

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans cette rubrique, nous traiterons un scénario de base qui explique comment configurer des transformations de revendications entre les forêts d’approbations et approuvées. Vous allez apprendre comment les objets de stratégie de transformation de revendications peuvent être créés et liés à l’approbation de la forêt d’approbation et de la forêt approuvée. Vous allez ensuite valider le scénario.  

## <a name="scenario-overview"></a>Vue d’ensemble du scénario  
Adatum Corporation provides financial services to Contoso, Ltd. Chaque trimestre, Adatum comptables copier leurs feuilles de calcul de compte dans un dossier sur un serveur de fichiers situé à Contoso, Ltd. Il existe une approbation bidirectionnelle configurer entre Contoso et Adatum. Contoso, Ltd. souhaite protéger le partage afin que seuls les employés Adatum peuvent accéder au partage distant.  

Dans ce scénario :  

1.  [Configurer les conditions préalables et l’environnement de test](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)  

2.  [Configurer la transformation des revendications sur une forêt approuvée (Adatum)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)  

3.  [Configurer la transformation des revendications dans la forêt d’approbation (Contoso)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)  

4.  [Valider le scénario](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)  

## <a name="BKMK_1.1"></a>Configurer les conditions préalables et l’environnement de test  
La configuration de test inclut les deux forêts : Adatum Corporation et Contoso, Ltd et avoir une approbation bidirectionnelle entre Contoso et Adatum. « adatum.com » est la forêt approuvée et « contoso.com » est la forêt d’approbation.  

Le scénario de transformation des revendications montre la transformation de revendication dans la forêt approuvée à une revendication dans la forêt d’approbation. Pour ce faire, vous devez configurer une nouvelle forêt nommée adatum.com et remplir la forêt avec un utilisateur de test avec la valeur « Adatum » entreprise. Vous devez ensuite configurer une approbation bidirectionnelle entre contoso.com et adatum.com.  

> [!IMPORTANT]  
> Lorsque vous configurez les forêts de Contoso et Adatum, vous devez vous assurer que les deux domaines racine sont à Windows Server 2012 niveau fonctionnel du domaine pour la transformation des revendications fonctionne.  

Vous devez configurer les éléments suivants pour le laboratoire. Ces procédures sont décrites en détail dans [annexe b : configuration de l’environnement de test](Appendix-B--Setting-Up-the-Test-Environment.md)  

Vous devez implémenter les procédures suivantes pour configurer le laboratoire pour ce scénario :  

1.  [Définir Adatum comme forêt approuvée pour Contoso](Appendix-B--Setting-Up-the-Test-Environment.md)  

2.  [Créer le type de revendication « Company » sur Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)  

3.  [Activer la propriété de ressource « Company » sur Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)  

4.  [Créer la règle d’accès centralisée](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)  

5.  [Créer la stratégie d’accès centralisée](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)  

6.  [Publier la nouvelle stratégie via la stratégie de groupe](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)  

7.  [Créer le dossier Earnings sur le serveur de fichiers](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)  

8.  [Définir la classification et appliquer la stratégie d’accès centralisée sur le nouveau dossier](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)  

Pour suivre ce scénario, utilisez les informations suivantes :  

|Objets|Détails|  
|-----------|-----------|  
|Utilisateurs|Jeff Low, Contoso|  
|Revendications d’utilisateur sur Adatum et Contoso|ID : ad : / / ext/ContosoAdatum : entreprise,<br /><br />Attribut source : entreprise<br /><br />Valeurs suggérées : Contoso, Adatum **Important :** Vous devez définir l’ID de la « Company » de type de revendication sur Contoso et Adatum pour être la même pour la transformation des revendications fonctionne.|  
|Règle d’accès centralisée sur Contoso|AdatumEmployeeAccessRule|  
|Stratégie d’accès centralisée sur Contoso|Stratégie d’accès Adatum uniquement|  
|Stratégies de Transformation des revendications sur Adatum et Contoso|DenyAllExcept entreprise|  
|Dossier de fichiers sur Contoso|D:\EARNINGS|  

## <a name="BKMK_3"></a>Configurer la transformation des revendications sur une forêt approuvée (Adatum)  
Dans cette étape, vous créez une stratégie de transformation dans Adatum à refuser toutes les revendications à l’exception de « Company » à passer à Contoso.  

Le module Active Directory pour Windows PowerShell fournit le **DenyAllExcept** argument, ce qui supprime tout sauf les revendications spécifiées dans la stratégie de transformation.  

Pour configurer une transformation des revendications, vous devez créer une stratégie de transformation des revendications et le lier entre les forêts approuvés et autorisé à approuver.  

### <a name="BKMK_2.2"></a>Créer une stratégie de transformation de revendications dans Adatum  

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>Pour créer une stratégie de transformation Adatum à refuser toutes les revendications à l’exception de « Company »  

1. Connectez-vous au contrôleur de domaine, adatum.com en tant qu’administrateur avec le mot de passe <strong>pass@word1</strong>.  

2. Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et tapez la commande suivante :  

   ```  
   New-ADClaimTransformPolicy `  
   -Description:"Claims transformation policy to deny all claims except Company"`  
   -Name:"DenyAllClaimsExceptCompanyPolicy" `  
   -DenyAllExcept:company `  
   -Server:"adatum.com" `  

   ```  

### <a name="BKMK_2.3"></a>Définir un lien de transformation des revendications sur l’objet d’approbation de domaine Adatum  
Dans cette étape, vous appliquez la stratégie de transformation des revendications qui vient d’être créé sur l’objet d’approbation de domaine Adatum pour Contoso.  

##### <a name="to-apply-the-claims-transformation-policy"></a>Pour appliquer la stratégie de transformation de revendications  

1. Connectez-vous au contrôleur de domaine, adatum.com en tant qu’administrateur avec le mot de passe <strong>pass@word1</strong>.  

2. Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et tapez la commande suivante :  

   ```  

     Set-ADClaimTransformLink `  
   -Identity:"contoso.com" `  
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
   '"TrustRole:Trusted `  

   ```  

## <a name="BKMK_4"></a>Configurer la transformation des revendications dans la forêt d’approbation (Contoso)  
Dans cette étape, vous créez une stratégie de transformation des revendications dans Contoso (la forêt d’approbation) pour refuser toutes les revendications à l’exception de « Company ». Vous devez créer une stratégie de transformation de revendications et liez-le à l’approbation de forêt.  

### <a name="BKMK_4.1"></a>Créer une stratégie de transformation de revendications dans Contoso  

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>Pour créer une stratégie de transformation Adatum afin de refuser tout sauf la « Company »  

1. Connectez-vous au contrôleur de domaine, contoso.com en tant qu’administrateur avec le mot de passe <strong>pass@word1</strong>.  

2. Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et tapez la commande suivante :  

   ```  
   New-ADClaimTransformPolicy `  
   -Description:"Claims transformation policy to deny all claims except company" `  
   -Name:"DenyAllClaimsExceptCompanyPolicy" `  
   -DenyAllExcept:company `  
   -Server:"contoso.com" `  

   ```  

### <a name="BKMK_4.2"></a>Définir un lien de transformation des revendications sur l’objet d’approbation de domaine de Contoso  
Dans cette étape, vous appliquez nouvellement créé contoso.com passer via Stratégie de transformation des revendications sur l’objet de domaine d’approbation contoso.com pour Adatum à autoriser « Société ». L’objet d’approbation de domaine est nommé adatum.com.  

##### <a name="to-set-the-claims-transformation-policy"></a>Pour définir les revendications de stratégie de transformation  

1. Connectez-vous au contrôleur de domaine, contoso.com en tant qu’administrateur avec le mot de passe <strong>pass@word1</strong>.  

2. Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et tapez la commande suivante :  

   ```  

     Set-ADClaimTransformLink   
   -Identity:"adatum.com" `  
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
   -TrustRole:Trusting `  

   ```  

## <a name="BKMK_5"></a>Valider le scénario  
Dans cette étape vous essayez d’accéder au dossier D:\EARNINGS qui a été configuré sur le serveur de fichiers FILE1 pour valider que l’utilisateur a accès au dossier partagé.  

#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>Pour vous assurer que l’utilisateur Adatum peut accéder au dossier partagé  

1. Connectez-vous au Client d’ordinateur, CLIENT1 en tant que Jeff Low avec le mot de passe <strong>pass@word1</strong>.  

2. Accédez au dossier \\\FILE1.contoso.com\Earnings.  

3. Jeff Low doit être en mesure d’accéder au dossier.  

## <a name="additional-scenarios-for-claims-transformation-policies"></a>Scénarios supplémentaires pour les stratégies de transformation de revendications  
Voici une liste des cas courants supplémentaires dans la transformation des revendications.  


|                                                 Scénario                                                 |                                                                                                                                                                                                                                           Stratégie                                                                                                                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  Autoriser toutes les revendications provenant d’Adatum soient appliquées à Contoso Adatum                  |                                                          Code - <br />Nouvelle ADClaimTransformPolicy \`<br /> -Description : « Stratégie de transformation afin d’autoriser toutes les revendications de revendications » \`<br />-Name:"AllowAllClaimsPolicy" \`<br />-AutoriserTout \`<br />-Server:"contoso.com » \`<br />Set-ADClaimTransformLink \`<br />-Identity:"adatum.com" \`<br />-Policy:"AllowAllClaimsPolicy" \`<br />-TrustRole : approbation \`<br />-Server:"contoso.com » \`                                                          |
|                  Refuser toutes les revendications provenant d’Adatum soient appliquées à Contoso Adatum                   |                                                            Code - <br />Nouvelle ADClaimTransformPolicy \`<br />-Description : « Stratégie de transformation pour refuser toutes les revendications de revendications » \`<br />-Name : « DenyAllClaimsPolicy » \`<br /> -DenyAll \`<br />-Server:"contoso.com » \`<br />Set-ADClaimTransformLink \`<br />-Identity:"adatum.com" \`<br />-Policy:"DenyAllClaimsPolicy" \`<br />-TrustRole : approbation \`<br />-Server:"contoso.com »\`                                                             |
| Autoriser toutes les revendications provenant d’Adatum, à l’exception de « Société » et « Department » à appliquer pour Contoso Adatum | Code <br />Nouvelle ADClaimTransformationPolicy \`<br />-Description : « Stratégie pour autoriser toutes les revendications à l’exception de la société et de services de transformation des revendications » \`<br /> -Name:"AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br />-AllowAllExcept : entreprise, le service \`<br />-Server:"contoso.com » \`<br />Set-ADClaimTransformLink \`<br /> -Identity:"adatum.com" \`<br />-Policy:"AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br /> -TrustRole : approbation \`<br />-Server:"contoso.com » \` |

## <a name="BKMK_Links"></a>Voir aussi  

-   Pour obtenir la liste de toutes les applets de commande Windows PowerShell qui sont disponibles pour la transformation des revendications, consultez [référence d’applet de commande PowerShell Active Directory](https://go.microsoft.com/fwlink/?LinkId=243150).  

-   Pour des tâches avancées qui impliquent des exportations et importations DAC d’informations de configuration entre deux forêts, utilisez la [PowerShell de référence dynamiques de contrôle d’accès](https://go.microsoft.com/fwlink/?LinkId=243150)  

-   [Déployer des revendications dans les forêts](Deploy-Claims-Across-Forests.md)  

-   [Langage de règles de transformation de revendications](Claims-Transformation-Rules-Language.md)  

-   [Contrôle d’accès dynamique : Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  


