---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: Déployer les revendications entre les forêts (procédure descriptive)
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 05ca21f343d2ad3db4ce00b53a66b3cd6dd6de16
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861242"
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>Déployer les revendications entre les forêts (procédure descriptive)

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans cette rubrique, nous allons aborder un scénario de base qui explique comment configurer des transformations de revendications entre des forêts approuvées et approuvées. Vous allez apprendre comment créer et lier des objets de stratégie de transformation de revendications à l’approbation de la forêt d’approbation et de la forêt approuvée. Vous allez ensuite valider le scénario.  

## <a name="scenario-overview"></a>Vue d’ensemble du scénario  
Adatum Corporation fournit des services financiers à contoso, Ltd. Chaque trimestre, les comptables adatum copient leurs feuilles de calcul de compte dans un dossier sur un serveur de fichiers situé à l’adresse contoso, Ltd. Il existe une approbation bidirectionnelle configurée de contoso à adatum. Contoso, Ltd. souhaite protéger le partage afin que seuls les employés adatum puissent accéder au partage distant.  

Dans ce scénario :  

1.  [Configurer les composants requis et l’environnement de test](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)  

2.  [Configurer une transformation de revendications sur une forêt approuvée (adatum)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)  

3.  [Configurer la transformation de revendications dans la forêt d’approbation (contoso)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)  

4.  [Valider le scénario](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)  

## <a name="set-up-the-prerequisites-and-the-test-environment"></a><a name="BKMK_1.1"></a>Configurer les composants requis et l’environnement de test  
La configuration de test implique la configuration de deux forêts : adatum Corporation et contoso, Ltd, et avoir une relation d’approbation bidirectionnelle entre contoso et adatum. « adatum.com » est la forêt approuvée et « contoso.com » est la forêt d’approbation.  

Le scénario de transformation des revendications illustre la transformation d’une revendication dans la forêt approuvée en une revendication dans la forêt d’approbation. Pour ce faire, vous devez configurer une nouvelle forêt appelée adatum.com et remplir la forêt avec un utilisateur de test dont la valeur de société est « adatum ». Vous devez ensuite configurer une relation d’approbation bidirectionnelle entre contoso.com et adatum.com.  

> [!IMPORTANT]  
> Lors de la configuration des forêts Contoso et Adatum, vous devez vous assurer que les deux domaines racine sont au niveau fonctionnel de domaine Windows Server 2012 pour que la transformation des revendications fonctionne.  

Vous devez configurer les éléments suivants pour le laboratoire. Ces procédures sont expliquées en détail dans [l’annexe B : configuration de l’environnement de test](Appendix-B--Setting-Up-the-Test-Environment.md)  

Vous devez mettre en œuvre les procédures suivantes pour configurer le laboratoire pour ce scénario :  

1.  [Définir adatum comme forêt approuvée sur contoso](Appendix-B--Setting-Up-the-Test-Environment.md)  

2.  [Créer le type de revendication « Company » sur contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)  

3.  [Activer la propriété de ressource « Company » sur contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)  

4.  [Créer la règle d’accès centralisée](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)  

5.  [Créer la stratégie d’accès centralisée](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)  

6.  [Publier la nouvelle stratégie via stratégie de groupe](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)  

7.  [Créer le dossier Earnings sur le serveur de fichiers](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)  

8.  [Définir la classification et appliquer la stratégie d’accès centralisée sur le nouveau dossier](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)  

Pour effectuer ce scénario, utilisez les informations suivantes :  

|Objets|Détails|  
|-----------|-----------|  
|Utilisateurs|Jeff Low, contoso|  
|Revendications de l’utilisateur sur Adatum et contoso|ID : ad://ext/Company :ContosoAdatum,<p>Attribut source : société<p>Valeurs suggérées : contoso, adatum **important :** vous devez définir l’ID du type de revendication « Company » sur Contoso et adatum pour que la transformation des revendications fonctionne.|  
|Règle d’accès centralisée sur contoso|Règle règleaccèsemployésadatum|  
|Stratégie d’accès centralisée sur contoso|Stratégie d’accès adatum uniquement|  
|Stratégies de transformation des revendications sur Adatum et contoso|Entreprise DenyAllExcept|  
|Dossier de fichiers sur contoso|D:\EARNINGS|  

## <a name="set-up-claims-transformation-on-trusted-forest-adatum"></a><a name="BKMK_3"></a>Configurer une transformation de revendications sur une forêt approuvée (adatum)  
Au cours de cette étape, vous allez créer une stratégie de transformation dans Adatum pour refuser toutes les revendications, à l’exception de « Company » à passer à contoso.  

Le module Active Directory pour Windows PowerShell fournit l’argument **DenyAllExcept** , qui supprime tout sauf les revendications spécifiées dans la stratégie de transformation.  

Pour configurer une transformation de revendications, vous devez créer une stratégie de transformation des revendications et la lier entre les forêts de confiance et d’approbation.  

### <a name="create-a-claims-transformation-policy-in-adatum"></a><a name="BKMK_2.2"></a>Créer une stratégie de transformation des revendications dans Adatum  

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>Pour créer une stratégie de transformation adatum pour refuser toutes les revendications sauf « Company »  

1. Connectez-vous au contrôleur de domaine adatum.com en tant qu’administrateur avec le mot de passe <strong>pass@word1</strong>.  

2. Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et tapez ce qui suit :  

   ```  
   New-ADClaimTransformPolicy `  
   -Description:"Claims transformation policy to deny all claims except Company"`  
   -Name:"DenyAllClaimsExceptCompanyPolicy" `  
   -DenyAllExcept:company `  
   -Server:"adatum.com" `  

   ```  

### <a name="set-a-claims-transformation-link-on-adatums-trust-domain-object"></a><a name="BKMK_2.3"></a>Définir un lien de transformation des revendications sur l’objet domaine d’approbation de adatum  
Au cours de cette étape, vous allez appliquer la stratégie de transformation des revendications nouvellement créée à l’objet domaine d’approbation de adatum pour contoso.  

##### <a name="to-apply-the-claims-transformation-policy"></a>Pour appliquer la stratégie de transformation des revendications  

1. Connectez-vous au contrôleur de domaine adatum.com en tant qu’administrateur avec le mot de passe <strong>pass@word1</strong>.  

2. Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et tapez ce qui suit :  

   ```  

     Set-ADClaimTransformLink `  
   -Identity:"contoso.com" `  
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
   '"TrustRole:Trusted `  

   ```  

## <a name="set-up-claims-transformation-in-the-trusting-forest-contoso"></a><a name="BKMK_4"></a>Configurer la transformation de revendications dans la forêt d’approbation (contoso)  
Au cours de cette étape, vous allez créer une stratégie de transformation des revendications dans contoso (la forêt d’approbation) pour refuser toutes les revendications, à l’exception de « Company ». Vous devez créer une stratégie de transformation des revendications et la lier à l’approbation de la forêt.  

### <a name="create-a-claims-transformation-policy-in-contoso"></a><a name="BKMK_4.1"></a>Créer une stratégie de transformation des revendications dans contoso  

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>Pour créer une stratégie de transformation adatum pour refuser tout sauf « Company »  

1. Connectez-vous au contrôleur de domaine contoso.com en tant qu’administrateur avec le mot de passe <strong>pass@word1</strong>.  

2. Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et tapez la commande suivante :  

   ```  
   New-ADClaimTransformPolicy `  
   -Description:"Claims transformation policy to deny all claims except company" `  
   -Name:"DenyAllClaimsExceptCompanyPolicy" `  
   -DenyAllExcept:company `  
   -Server:"contoso.com" `  

   ```  

### <a name="set-a-claims-transformation-link-on-contosos-trust-domain-object"></a><a name="BKMK_4.2"></a>Définir un lien de transformation des revendications sur l’objet domaine d’approbation de contoso  
Au cours de cette étape, vous allez appliquer la stratégie de transformation des revendications nouvellement créée à l’objet de domaine d’approbation contoso.com pour adatum afin d’autoriser le passage de « Company » à contoso.com. L’objet domaine d’approbation est nommé adatum.com.  

##### <a name="to-set-the-claims-transformation-policy"></a>Pour définir la stratégie de transformation des revendications  

1. Connectez-vous au contrôleur de domaine contoso.com en tant qu’administrateur avec le mot de passe <strong>pass@word1</strong>.  

2. Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et tapez la commande suivante :  

   ```  

     Set-ADClaimTransformLink   
   -Identity:"adatum.com" `  
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
   -TrustRole:Trusting `  

   ```  

## <a name="validate-the-scenario"></a><a name="BKMK_5"></a>Valider le scénario  
Au cours de cette étape, vous essayez d’accéder au dossier D:\EARNINGS qui a été configuré sur le serveur de fichiers fichier1 pour confirmer que l’utilisateur a accès au dossier partagé.  

#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>Pour vous assurer que l’utilisateur adatum peut accéder au dossier partagé  

1. Connectez-vous à la machine cliente, CLIENT1 comme Jeff Low avec le mot de passe <strong>pass@word1</strong>.  

2. Accédez au dossier \\\FILE1.contoso.com\Earnings.  

3. Jeff Low doit pouvoir accéder au dossier.  

## <a name="additional-scenarios-for-claims-transformation-policies"></a>Scénarios supplémentaires pour les stratégies de transformation des revendications  
Voici une liste de cas courants supplémentaires dans la transformation de revendications.  


|                                                 Scénario                                                 |                                                                                                                                                                                                                                           Stratégie                                                                                                                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  Autoriser toutes les revendications provenant de adatum à traverser contoso adatum                  |                                                          Code <br />New-ADClaimTransformPolicy \`<br /> -Description : « stratégie de transformation des revendications pour autoriser toutes les revendications » \`<br />-Name : « AllowAllClaimsPolicy » \`<br />-AutoriserTout \`<br />-Server :"contoso. com» \`<br />Set-ADClaimTransformLink \`<br />-Identity :"adatum. com" \`<br />-Stratégie : « AllowAllClaimsPolicy » \`<br />-TrustRole : approbation \`<br />-Server :"contoso. com» \`                                                          |
|                  Refuser toutes les revendications provenant de adatum pour passer à contoso adatum                   |                                                            Code <br />New-ADClaimTransformPolicy \`<br />-Description : « stratégie de transformation des revendications pour refuser toutes les revendications » \`<br />-Name : « DenyAllClaimsPolicy » \`<br /> -DenyAll \`<br />-Server :"contoso. com» \`<br />Set-ADClaimTransformLink \`<br />-Identity :"adatum. com" \`<br />-Stratégie : « DenyAllClaimsPolicy » \`<br />-TrustRole : approbation \`<br />-Server :"contoso. com»\`                                                             |
| Autoriser toutes les revendications provenant de Adatum, à l’exception de « Company » et « Department », à traverser contoso adatum | Code <br />-New-ADClaimTransformationPolicy \`<br />-Description : « stratégie de transformation des revendications pour autoriser toutes les revendications, à l’exception de la société et du service » \`<br /> -Name : « AllowAllClaimsExceptCompanyAndDepartmentPolicy » \`<br />-AllowAllExcept : Company, service \`<br />-Server :"contoso. com» \`<br />Set-ADClaimTransformLink \`<br /> -Identity :"adatum. com" \`<br />-Stratégie : « AllowAllClaimsExceptCompanyAndDepartmentPolicy » \`<br /> -TrustRole : approbation \`<br />-Server :"contoso. com» \` |

## <a name="see-also"></a><a name="BKMK_Links"></a>Voir aussi  

-   Pour obtenir la liste de toutes les applets de commande Windows PowerShell qui sont disponibles pour la transformation des revendications, consultez [Active Directory référence des applets](https://go.microsoft.com/fwlink/?LinkId=243150)de commande PowerShell.  

-   Pour les tâches avancées qui impliquent l’exportation et l’importation d’informations de configuration DAC entre deux forêts, utilisez la [référence Dynamic Access Control PowerShell](https://go.microsoft.com/fwlink/?LinkId=243150)  

-   [Déployer des revendications dans les forêts](Deploy-Claims-Across-Forests.md)  

-   [Langage de règles de transformation de revendications](Claims-Transformation-Rules-Language.md)  

-   [Access Control dynamique : vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  


