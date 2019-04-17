---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: "Déployer des revendications dans les forêts (procédure descriptive)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3bab7a396061ecae8a187cc6986d6d026a9e4b32
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>Déployer des revendications dans les forêts (procédure descriptive)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Dans cette rubrique, nous en parlerons un scénario de base qui explique comment configurer des transformations de revendications entre les forêts d’approbation et approuvées. Vous allez découvrir comment les objets de stratégie de transformation de revendications peuvent être créés et liés à l’approbation de la forêt d’approbation et de la forêt approuvée. Vous allez ensuite valider le scénario.  
  
## <a name="scenario-overview"></a>Vue d’ensemble du scénario  
Adatum Corporation fournit des services financiers à Contoso, Ltd. Chaque trimestre, éprouve des difficultés Adatum copier leurs feuilles de calcul de compte dans un dossier sur un serveur de fichiers situé à Contoso, Ltd. Il existe une approbation bidirectionnelle configurer entre Contoso et Adatum. Contoso, Ltd. souhaite protéger le partage, afin que seuls les employés Adatum puissent accéder au partage distant.  
  
Dans ce scénario:  
  
1.  [Configurer les conditions préalables et l’environnement de test](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)  
  
2.  [Configurer la transformation des revendications dans la forêt approuvée (Adatum)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)  
  
3.  [Configurer la transformation des revendications dans la forêt d’approbation (Contoso)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)  
  
4.  [Valider le scénario](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)  
  
## <a name="BKMK_1.1"></a>Configurer les conditions préalables et l’environnement de test  
La configuration de test inclut les deux forêts: Adatum Corporation et Contoso, Ltd et avoir une approbation bidirectionnelle entre Contoso et Adatum. «adatum.com» est la forêt approuvée et «contoso.com» est la forêt d’approbation.  
  
Le scénario de transformation des revendications montre la transformation de revendication dans la forêt approuvée à une revendication dans la forêt d’approbation. Pour ce faire, vous devez configurer une nouvelle forêt nommée adatum.com et remplir la forêt avec un utilisateur de test avec une valeur de la société de «Adatum». Ensuite, vous devez définir une relation d’approbation bidirectionnelle entre contoso.com et adatum.com.  
  
> [!IMPORTANT]  
> Lorsque vous configurez les forêts Contoso et Adatum, vous devez vous assurer que les deux domaines racines sont à Windows Server2012 niveau fonctionnel du domaine pour la transformation des revendications pour fonctionner.  
  
Vous devez configurer les éléments suivants pour le laboratoire. Ces procédures sont décrites en détail dans [annexe b: paramètres de l’environnement de Test](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
Vous devez mettre en œuvre les procédures suivantes pour configurer le laboratoire pour ce scénario:  
  
1.  [Définir Adatum comme forêt approuvée pour Contoso](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
2.  [Créer le type de revendication «Société» sur Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)  
  
3.  [Activer la propriété de ressource «Société» sur Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)  
  
4.  [Créer la règle d’accès centralisée](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)  
  
5.  [Créer la stratégie d’accès centralisée](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)  
  
6.  [Publier la nouvelle stratégie via la stratégie de groupe](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)  
  
7.  [Créer le dossier Earnings sur le serveur de fichiers](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)  
  
8.  [Définir la classification et appliquer la stratégie d’accès centralisée sur le nouveau dossier](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)  
  
Utilisez les informations suivantes pour effectuer ce scénario:  
  
|Objets|Détails|  
|-----------|-----------|  
|Utilisateurs|Jeff Low, Contoso|  
|Revendications d’utilisateur sur Adatum et Contoso|ID: ad: / / ext/société: ContosoAdatum,<br /><br />Attribut source: entreprise<br /><br />Les valeurs proposées: Contoso, Adatum **Important:** vous devez définir le type de revendication l’ID de la société' ' sur Contoso et Adatum pour être le même pour la transformation des revendications pour fonctionner.|  
|Règle d’accès centralisée sur Contoso|Règleaccèsemployésadatum|  
|Stratégie d’accès centralisée sur Contoso|Stratégie d’accès Adatum uniquement|  
|Stratégies de Transformation des revendications sur Adatum et Contoso|Société DenyAllExcept|  
|Dossier de fichiers dans Contoso|D:\EARNINGS|  
  
## <a name="BKMK_3"></a>Configurer la transformation des revendications dans la forêt approuvée (Adatum)  
Dans cette étape, vous créez une stratégie de transformation dans Adatum pour interdire toutes les revendications à l’exception de «Société» pour passer à Contoso.  
  
Le module ActiveDirectory pour Windows PowerShell fournit le **DenyAllExcept** argument, qui supprime tout excepté les revendications spécifiées dans la stratégie de transformation.  
  
Pour configurer une transformation des revendications, vous devez créer une stratégie de transformation des revendications et le lier entre les forêts de confiance et autorisé à approuver.  
  
### <a name="BKMK_2.2"></a>Créer une stratégie de transformation des revendications dans Adatum  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>Pour créer une stratégie de transformation Adatum pour interdire toutes les revendications à l’exception de «Société»  
  
1.  Connectez-vous au contrôleur de domaine, adatum.com en tant qu’administrateur avec le mot de passe **pass@word1**.  
  
2.  Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et tapez la commande suivante:  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except Company"`  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"adatum.com" `  
  
    ```  
  
### <a name="BKMK_2.3"></a>Définir un lien de transformation des revendications sur l’objet d’approbation de domaine de Adatum  
Dans cette étape, vous appliquez la stratégie de transformation des revendications qui vient d’être créé sur objet domaine d’approbation de Adatum pour Contoso.  
  
##### <a name="to-apply-the-claims-transformation-policy"></a>Pour appliquer la stratégie de transformation des revendications  
  
1.  Connectez-vous au contrôleur de domaine, adatum.com en tant qu’administrateur avec le mot de passe **pass@word1**.  
  
2.  Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et tapez la commande suivante:  
  
    ```  
  
      Set-ADClaimTransformLink `  
    -Identity:"contoso.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    '"TrustRole:Trusted `  
  
    ```  
  
## <a name="BKMK_4"></a>Configurer la transformation des revendications dans la forêt d’approbation (Contoso)  
Dans cette étape, vous créez une stratégie de transformation des revendications de Contoso (forêt d’approbation) pour interdire toutes les revendications à l’exception de «Société». Vous devez créer une stratégie de transformation des revendications et le lier à l’approbation de forêt.  
  
### <a name="BKMK_4.1"></a>Créer une stratégie de transformation des revendications de Contoso  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>Pour créer une stratégie de transformation Adatum pour refuser tous sauf «Société»  
  
1.  Connectez-vous au contrôleur de domaine, contoso.com en tant qu’administrateur avec le mot de passe **pass@word1**.  
  
2.  Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et tapez la commande suivante:  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except company" `  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"contoso.com" `  
  
    ```  
  
### <a name="BKMK_4.2"></a>Définir un lien de transformation des revendications sur l’objet d’approbation de domaine de Contoso  
Dans cette étape, vous appliquez nouvellement créé stratégie de transformation des revendications de l’objet de domaine d’approbation contoso.com pour Adatum permettre à "Société" est passé à contoso.com. L’objet d’approbation de domaine est nommé adatum.com.  
  
##### <a name="to-set-the-claims-transformation-policy"></a>Pour définir les revendications de stratégie de transformation  
  
1.  Connectez-vous au contrôleur de domaine, contoso.com en tant qu’administrateur avec le mot de passe **pass@word1**.  
  
2.  Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et tapez la commande suivante:  
  
    ```  
  
      Set-ADClaimTransformLink   
    -Identity:"adatum.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    -TrustRole:Trusting `  
  
    ```  
  
## <a name="BKMK_5"></a>Valider le scénario  
Dans cette étape vous essayez d’accéder au dossier D:\EARNINGS qui a été configuré sur le serveur de fichiers FILE1 pour valider que l’utilisateur a accès au dossier partagé.  
  
#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>Pour vous assurer que l’utilisateur Adatum permettre accéder au dossier partagé  
  
1.  Connectez-vous à l’ordinateur Client, CLIENT1 en tant que Jeff Low avec le mot de passe **pass@word1**.  
  
2.  Recherchez le dossier \\\FILE1.contoso.com\Earnings.  
  
3.  Jeff Low doit être en mesure d’accéder au dossier.  
  
## <a name="additional-scenarios-for-claims-transformation-policies"></a>Scénarios supplémentaires pour les stratégies de transformation des revendications  
Voici une liste des cas courants supplémentaires dans la transformation des revendications.  
  
|Scénario|Stratégie|  
|------------|----------|  
|Autoriser toutes les revendications qui proviennent d’Adatum pour atteindre par le biais de Contoso Adatum|Code: <br />New-ADClaimTransformPolicy \»<br /> -Description: «Autoriser toutes les revendications de la stratégie de transformation des revendications» \»<br />-Nom: «AllowAllClaimsPolicy» \»<br />-AutoriserTout \»<br />-Serveur: «contoso.com» \»<br />Set-ADClaimTransformLink \»<br />-Identité: «adatum.com» \»<br />-Policy: «AllowAllClaimsPolicy» \»<br />-TrustRole: approuver \»<br />-Serveur: «contoso.com»»|  
|Interdire toutes les revendications qui proviennent d’Adatum pour atteindre par le biais de Contoso Adatum|Code: <br />New-ADClaimTransformPolicy \»<br />-Description: «Stratégie de transformation pour refuser toutes les revendications de revendications» \»<br />-Nom: «DenyAllClaimsPolicy» \»<br /> -DenyAll \»<br />-Serveur: «contoso.com» \»<br />Set-ADClaimTransformLink \»<br />-Identité: «adatum.com» \»<br />-Policy: «DenyAllClaimsPolicy» \»<br />-TrustRole: approuver \»<br />-Serveur: «contoso.com»»|  
|Autoriser toutes les revendications qui proviennent d’Adatum à l’exception «Société» et «Service» pour atteindre par le biais de Contoso Adatum|Code <br />-New-ADClaimTransformationPolicy \»<br />-Description: «Autoriser toutes les revendications à l’exception de la société et de services de la stratégie de transformation des revendications» \»<br /> -Nom: «AllowAllClaimsExceptCompanyAndDepartmentPolicy» \»<br />-AllowAllExcept: entreprise, le service \»<br />-Serveur: «contoso.com» \»<br />Set-ADClaimTransformLink \»<br /> -Identité: «adatum.com» \»<br />-Policy: «AllowAllClaimsExceptCompanyAndDepartmentPolicy» \»<br /> -TrustRole: approuver \»<br />-Serveur: «contoso.com»»|  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   Pour obtenir une liste de toutes les applets de commande Windows PowerShell sont disponibles pour la transformation des revendications, voir [référence d’applet de commande PowerShell ActiveDirectory ](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
-   Pour des tâches avancées qui impliquent l’exportation et importation des informations de configuration DAC entre deux forêts, utilisez le [PowerShell référence de contrôle d’accès dynamique](https://go.microsoft.com/fwlink/?LinkId=243150)  
  
-   [Déployer des revendications dans les forêts](Deploy-Claims-Across-Forests.md)  
  
-   [Langage de règles de Transformation de revendications](Claims-Transformation-Rules-Language.md)  
  
-   [Contrôle d’accès dynamique: Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  

