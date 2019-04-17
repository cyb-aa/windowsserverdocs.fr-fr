---
ms.assetid: b734cbcb-342c-4a28-8ab5-b9cd990bb1c2
title: "Quand utiliser une règle de revendication d’autorisation"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 144e382692e8f2a6732f8c7c5b8a1dd6049192cb
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

# <a name="when-to-use-an-authorization-claim-rule"></a>Quand utiliser une règle de revendication d’autorisation
Vous pouvez utiliser cette règle dans Active Directory Federation Services \(AD FS\) lorsque vous avez besoin prendre un type de revendication entrante, puis appliquez une action qui détermine si un utilisateur est autorisé ou refusé accès en fonction de la valeur que vous spécifiez dans la règle. Lorsque vous utilisez cette règle, vous transmettez ou transformez les revendications qui correspondent à la logique de règle suivante, en fonction des options que vous configurez dans la règle:  
  
|Option de règle|Logique de règle|  
|---------------|--------------|  
|Autoriser tous les utilisateurs|Si le type de revendication entrante est égal à *tout type de revendication* et que la valeur est égale à *n’importe quelle valeur*, revendication émise avec la valeur est égale à *autoriser*|  
|Autoriser l’accès aux utilisateurs avec cette revendication entrante|Si le type de revendication entrante est égal à *type de revendication spécifiée* et que la valeur est égale à *valeur de revendication spécifiée*, revendication émise avec la valeur est égale à *autoriser*|  
|Refuser l’accès aux utilisateurs avec cette revendication entrante|Si le type de revendication entrante est égal à *type de revendication spécifiée* et que la valeur est égale à *valeur de revendication spécifiée*, revendication émise avec la valeur est égale à *refuser*|  
  
Les sections suivantes fournissent une introduction aux règles de revendication et fournissent des informations détaillées sur l’utilisation de cette règle.  
  
## <a name="about-claim-rules"></a>À propos des règles de revendication  
Une règle de revendication représente une instance de logique métier qui prend une revendication entrante, lui appliquer une condition \ (s’il x y\ puis) et génère une revendication sortante basée sur les paramètres de condition. La liste suivante présente d’importantes astuces que vous devez connaître les règles de revendication avant de poursuivre la lecture de cette rubrique:  
  
-   Dans la gestion AD FS de composants, les règles de revendication ne peuvent être créés à l’aide de modèles de règle de revendication  
  
-   Processus de règles de revendication entrante de revendications directement à partir d’un fournisseur de revendications \ (par exemple, Active Directory ou un autre fédération Service\) ou de la sortie de l’acceptation de règles de transformation sur une approbation de fournisseur de revendications.  
  
-   Les règles de revendication sont traitées par le moteur d’émission des revendications dans l’ordre chronologique, dans un ensemble de règles donné. En définissant la hiérarchie des règles, vous pouvez affiner ou filtrer les revendications qui sont générées par les règles précédentes au sein d’un ensemble de règles donné.  
  
-   Modèles de règle de revendication vous obligent toujours à spécifier un type de revendication entrante. Toutefois, vous pouvez traiter plusieurs valeurs de revendication avec le même type de revendication à l’aide d’une règle unique.  
  
Pour plus d’informations sur les règles de revendication et les jeux de règles de revendication, consultez [le rôle des règles de revendication](The-Role-of-Claim-Rules.md). Pour plus d’informations sur le traitement des règles, voir [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Pour plus d’informations, mode de traitement des jeux de règles de revendication, voir [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="permit-all-users"></a>Autoriser tous les utilisateurs  
Lorsque vous utilisez le modèle de règle Autoriser tous les utilisateurs, tous les utilisateurs auront accès à la partie de confiance. Toutefois, vous pouvez utiliser des règles d’autorisation supplémentaires pour restreindre davantage l’accès. Si une règle autorise un utilisateur à accéder à la partie de confiance, et une autre règle refuse l’accès utilisateur à la partie de confiance, le résultat de refus remplace le résultat de l’autorisation et l’accès est refusé à l’utilisateur.  
  
Les utilisateurs qui sont autorisés à accéder à la partie de confiance à partir du Service de fédération peuvent se voir refuser le service par la partie de confiance.  
  
## <a name="permit-access-to-users-with-this-incoming-claim"></a>Autoriser l’accès aux utilisateurs avec cette revendication entrante  
Lorsque vous utilisez l’autoriser ou refuser les utilisateurs en fonction sur un modèle de règle de revendication entrante pour créer une règle et définir la condition d’autorisation, vous pouvez autoriser un accès utilisateur spécifique à la partie de confiance selon le type et la valeur d’une revendication entrante. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle qui autorise uniquement les utilisateurs qui ont un groupe de revendication avec la valeur Admins du domaine. Si une règle autorise un utilisateur à accéder à la partie de confiance, et une autre règle refuse l’accès utilisateur à la partie de confiance, le résultat de refus remplace le résultat de l’autorisation et l’accès est refusé à l’utilisateur.  
  
Les utilisateurs qui sont autorisés à accéder à la partie de confiance à partir du Service de fédération peuvent se voir refuser le service par la partie de confiance. Si vous souhaitez autoriser tous les utilisateurs à accéder à la partie de confiance, utilisez le modèle de règle Autoriser tous les utilisateurs.  
  
## <a name="deny-access-to-users-with-this-incoming-claim"></a>Refuser l’accès aux utilisateurs avec cette revendication entrante  
Lorsque vous utilisez l’autoriser ou refuser les utilisateurs en fonction sur un modèle de règle de revendication entrante pour créer une règle et définir la condition de refus, vous pouvez refuser l’accès utilisateur à la partie de confiance selon le type et la valeur d’une revendication entrante. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle qui rejette tous les utilisateurs disposant d’un groupe de revendication avec la valeur d’utilisateurs du domaine.  
  
Si vous souhaitez utiliser la condition de refus, mais également activer l’accès à la partie de confiance des utilisateurs spécifiques, vous devez ajouter ultérieurement et explicitement les règles d’autorisation avec la condition d’autorisation pour activer les utilisateurs d’accéder à la partie de confiance.  
  
Si l’accès est refusé à un utilisateur lorsque le moteur d’émission des revendications traite l’ensemble de règles, traitement des règles supplémentaires s’arrête et AD FS renvoie une erreur «Accès refusé» à la demande de l’utilisateur.  
  
## <a name="authorizing-users"></a>Autorisation des utilisateurs  
Dans AD FS, les règles d’autorisation sont utilisés pour émettre une autorisation ou refuser des revendications qui déterminent si un utilisateur ou un groupe d’utilisateurs \ (selon l’used\ de type de revendication) pourront accéder aux ressources de la console Web dans une partie de confiance donné ou non. Règles d’autorisation ne peuvent être définies sur les approbations de partie de confiance.  
  
### <a name="authorization-rule-sets"></a>Ensembles de règles d’autorisation  
Ensembles de règles d’autorisation différentes selon le type d’autorisation ou refuser les opérations que vous devez configurer. Ces ensembles de règles sont les suivantes:  
  
-   **Les règles d’autorisation d’émission**: ces règles déterminent si un utilisateur peut recevoir des revendications pour une partie de confiance et, par conséquent, accéder à la partie de confiance.  
  
-   **Règles d’autorisation de délégation**: ces règles déterminent si un utilisateur peut agir comme un autre utilisateur à la partie de confiance. Lorsqu’un utilisateur agit comme un autre utilisateur, les revendications relatives à l’utilisateur demandeur sont toujours placées dans le jeton.  
  
-   **Les règles d’autorisation d’emprunt d’identité**: ces règles déterminent si un utilisateur peut entièrement emprunter l’identité d’un autre utilisateur à la partie de confiance. L’emprunt d’identité d’un autre utilisateur n’est une fonctionnalité très puissante, car la partie de confiance ne sait pas que l’utilisateur est empruntée.  
  
Pour plus d’informations sur la façon dont le processus de règle d’autorisation s’inscrit dans le pipeline d’émission de revendications, consultez le rôle du moteur d’émission de revendications.  
  
### <a name="supported-claim-types"></a>Types de revendications pris en charge  
AD FSdefines deux des types de revendications qui sont utilisées pour déterminer si un utilisateur est autorisé ou refusé. Ces revendications type Uniform Resource Identifiers \(URIs\) sont les suivantes:  
  
1.  **Autoriser**: http:///\/schemas.microsoft.com\/authorization\/claims\/permit  
  
2.  **Refuser**: http:///\/schemas.microsoft.com\/authorization\/claims\/deny  
  
## <a name="how-to-create-this-rule"></a>Comment créer cette règle  
Vous pouvez créer des règles d’autorisation à l’aide du langage de règle de revendication ou les **autoriser tous les utilisateurs** modèle de règle ou le **autoriser ou refuser les utilisateurs selon une revendication entrante** modèle de règle dans la gestion AD FS de composants. Le modèle de règle Autoriser tous les utilisateurs ne fournit pas les options de configuration. Toutefois, l’autoriser ou refuser les utilisateurs basés sur un modèle de règle de revendication entrante fournit les options de configuration suivantes:  
  
-   Spécifiez un nom de règle de revendication  
  
-   Spécifier un type de revendication entrante  
  
-   Tapez une valeur de revendication entrante  
  
-   Autoriser l’accès aux utilisateurs avec cette revendication entrante  
  
-   Refuser l’accès aux utilisateurs avec cette revendication entrante  
  
Pour plus d’informations sur la création de ce modèle, voir [créer une règle pour autoriser tous les utilisateurs](https://technet.microsoft.com/library/ee913577.aspx) ou [créer une règle pour autoriser ou refuser les utilisateurs basés sur une revendication entrante](https://technet.microsoft.com/library/ee913594.aspx) dans le Guide de déploiement d’AD FS.  
  
## <a name="using-the-claim-rule-language"></a>À l’aide du langage de règle de revendication  
Si une revendication doit être envoyée uniquement lorsque la valeur de revendication correspond à un modèle personnalisé, vous devez utiliser une règle personnalisée. Pour plus d’informations, voir [quand utiliser une règle de revendication personnalisée](When-to-Use-a-Custom-Claim-Rule.md).  
  
### <a name="example-of-how-to-create-an-authorization-rule-based-on-multiple-claims"></a>Exemple de procédure de création d’une règle d’autorisation basée sur les revendications multiples  
Lorsque vous utilisez la syntaxe du langage de règle revendication pour autoriser les revendications, une revendication peut être également émise en fonction de la présence de plusieurs revendications dans les revendications d’origine de l’utilisateur. La règle suivante émet une revendication d’autorisation uniquement si l’utilisateur est membre du groupe éditeurs et a authentifié à l’aide de l’authentification Windows:  
  
```  
[type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod",   
value == "urn:federation:authentication:windows" ]  
&& [type == "http://schemas.xmlsoap.org/claims/Group ", value == “editors”]   
=> issue(type = "http://schemas.xmlsoap.org/claims/authZ", value = "Granted");  
```  
  
### <a name="example-of-how-to-create-authorization-rules-that-will-delegate-who-can-create-or-remove-federation-server-proxy-trusts"></a>Exemple de création d’autorisation qui délèguent peut créer ou supprimer des approbations de proxy de serveur de fédération des règles  
Un Service de fédération peut utiliser un serveur proxy de fédération pour rediriger les demandes des clients, une relation d’approbation doit tout d’abord être établie entre le Service de fédération et l’ordinateur de proxy de serveur de fédération. Une approbation de proxy est établie par défaut, lorsque une des informations d’identification suivantes est fournie dans l’Assistant Configuration Proxy des serveurs de fédération AD FS:  
  
-   Le compte de service utilisé par le Service de fédération, le proxy protège  
  
-   Un compte de domaine Active Directory qui est membre du groupe Administrateurs local sur tous les serveurs de fédération dans une batterie de serveurs de fédération  
  
Lorsque vous souhaitez spécifier les utilisateurs ou un utilisateur peuvent créer une approbation de proxy pour un Service de fédération donné, vous pouvez utiliser une des méthodes de délégation suivantes. Cette liste de méthodes est dans l’ordre de priorité, selon les recommandations de l’équipe du produit AD FS des méthodes de délégation plus sûres et moins problématiques. Il est nécessaire d’utiliser un seul de ces méthodes, selon les besoins de votre organisation:  
  
1.  Créer un groupe de sécurité de domaine dans Active Directory \ (par exemple, FSProxyTrustCreators\), ajoutez ce groupe au groupe Administrateurs local sur chacun des serveurs de fédération de la batterie de serveurs, puis ajoutez uniquement les comptes d’utilisateur auquel vous souhaitez déléguer ce droit au nouveau groupe. Il s’agit de la méthode préférée.  
  
2.  Ajoutez le compte de domaine de l’utilisateur au groupe Administrateurs sur chacun des serveurs de fédération de la batterie de serveurs.  
  
3.  Si pour une raison quelconque, vous ne pouvez pas utiliser une de ces méthodes, vous pouvez également créer une règle d’autorisation à cet effet. Bien qu’il n’est pas recommandé, en raison de complications possibles qui peuvent se produire si cette règle n’est pas écrite correctement, vous pouvez utiliser une règle d’autorisation personnalisée pour déléguer les Active Directory comptes d’utilisateur de domaine peuvent également créer ou même de supprimer les approbations entre tous les serveurs proxy de fédération qui sont associés à un Service de fédération donné.  
  
    Si vous choisissez la méthode 3, vous pouvez utiliser la syntaxe de règle suivante pour émettre une revendication d’autorisation qui permet à un utilisateur spécifié \ (dans ce cas, contoso\\frankm\) pour créer des approbations de fédération un ou plusieurs serveurs proxy pour le Service de fédération. Vous devez appliquer cette règle à l’aide de la commande Windows PowerShell **Set-ADFSProperties AddProxyAuthorizationRules**.  
  
    ```  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", issuer=~"^AD AUTHORITY$" value == "contoso\frankm" ] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true")  
  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
    Plus tard, si vous souhaitez supprimer l’utilisateur afin que l’utilisateur peut ne plus créer d’approbations de proxy, vous pouvez revenir à la règle par défaut proxy approbation d’autorisation pour supprimer le droit de l’utilisateur créer des approbations de proxy pour le Service de fédération. Vous devez également appliquer cette règle à l’aide de la commande Windows PowerShell **Set-ADFSProperties AddProxyAuthorizationRules**.  
  
    ```  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
Pour plus d’informations sur l’utilisation du langage de règle de revendication, voir [le rôle du langage de règle de revendication](The-Role-of-the-Claim-Rule-Language.md).  
  

