---
ms.assetid: b734cbcb-342c-4a28-8ab5-b9cd990bb1c2
title: Quand utiliser une règle de revendication d’autorisation
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6b852a580bdc0ea02643d478dc51b5cbcd2eac4b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188309"
---
# <a name="when-to-use-an-authorization-claim-rule"></a>Quand utiliser une règle de revendication d’autorisation
Vous pouvez utiliser cette règle dans Active Directory Federation Services \(AD FS\) lorsque vous devez prendre un type de revendication entrante, puis appliquer une action qui détermine si un utilisateur sera autorisé ou refusé l’accès en fonction de la valeur que vous avez Spécifiez dans la règle. Lorsque vous utilisez cette règle, vous transmettez ou transformez les revendications qui correspondent à la logique de règle suivante, en fonction des options que vous configurez dans la règle :  
  
|Option de règle|Logique de règle|  
|---------------|--------------|  
|Autoriser tous les utilisateurs|Si le type de revendication entrante est égal à *tout type de revendication* et que la valeur est égale à *toute valeur*, la revendication émise avec la valeur est égale à *Autoriser*|  
|Autoriser l’accès aux utilisateurs avec cette revendication entrante|Si le type de revendication entrante est égal à *type de revendication spécifié* et que la valeur est égale à *valeur de revendication spécifiée*, la revendication émise avec la valeur est égale à *Autoriser*|  
|Refuser l’accès aux utilisateurs avec cette revendication entrante|Si le type de revendication entrante est égal à *type de revendication spécifié* et que la valeur est égale à *valeur de revendication spécifiée*, la revendication émise avec la valeur est égale à*Refuser*|  
  
Les sections suivantes constituent une introduction aux règles de revendication et fournissent des informations détaillées sur les conditions d’utilisation de cette règle.  
  
## <a name="about-claim-rules"></a>À propos des règles de revendication  
Une règle de revendication représente une instance de logique métier qui permettront une revendication entrante, lui appliquer une condition \(if x, alors y\) et génère une revendication sortante selon les paramètres de condition. La liste suivante présente d’importantes astuces sur les règles de revendication dont vous devez prendre connaissance avant de poursuivre la lecture de cette rubrique :  
  
-   Dans le composant logiciel enfichable Gestion AD FS\-, revendication règles peuvent uniquement être créées à l’aide de modèles de règle de revendication  
  
-   Processus de règles de revendication entrante de revendications directement à partir d’un fournisseur de revendications \(comme Active Directory ou un autre Service de fédération\) ou à partir de la sortie de l’acceptation des règles sur une approbation de fournisseur de revendications de transformation.  
  
-   Les règles de revendication sont traitées par le moteur d’émission des revendications au sein d’un ensemble de règles donné et dans l’ordre chronologique. En définissant la hiérarchie des règles, vous pouvez affiner ou filtrer des revendications générées par des règles précédentes dans un ensemble de règles donné.  
  
-   Les modèles de règles de revendication vous obligent toujours à spécifier un type de revendication entrante. Toutefois, vous pouvez traiter plusieurs valeurs de revendication avec le même type de revendication en vous appuyant sur une règle unique.  
  
Pour plus d’informations sur les règles de revendication et des ensembles de règles de revendication, consultez [le rôle des règles de revendication](The-Role-of-Claim-Rules.md). Pour plus d’informations sur le traitement des règles, consultez [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Pour plus d’informations mode de traitement des ensembles de règles de revendication, consultez [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="permit-all-users"></a>Autoriser tous les utilisateurs  
Lorsque vous utilisez le modèle de règle Autoriser tous les utilisateurs, tous les utilisateurs ont accès à la partie de confiance. Vous pouvez toutefois utiliser des règles d’autorisation supplémentaires pour restreindre davantage les accès. Si une règle autorise un utilisateur à accéder à la partie de confiance et qu’une autre règle refuse l’accès de l’utilisateur à la partie de confiance, le refus l’emporte sur l’autorisation et l’accès est refusé à l’utilisateur.  
  
Les utilisateurs dont l’accès à la partie de confiance est autorisé par le service de fédération peuvent se voir refuser le service par la partie de confiance.  
  
## <a name="permit-access-to-users-with-this-incoming-claim"></a>Autoriser l’accès aux utilisateurs avec cette revendication entrante  
Lorsque vous utilisez le modèle de règle Autoriser ou refuser les utilisateurs en fonction d’une revendication entrante pour créer une règle et définir la condition d’autorisation, vous pouvez autoriser un accès utilisateur spécifique à la partie de confiance selon le type et la valeur d’une revendication entrante. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle qui autorise uniquement les utilisateurs dotés d’une revendication de groupe avec la valeur d’administrateurs du domaine. Si une règle autorise un utilisateur à accéder à la partie de confiance et qu’une autre règle refuse l’accès de l’utilisateur à la partie de confiance, le refus l’emporte sur l’autorisation et l’accès est refusé à l’utilisateur.  
  
Les utilisateurs dont l’accès à la partie de confiance est autorisé par le service de fédération peuvent se voir refuser le service par la partie de confiance. Pour autoriser tous les utilisateurs à accéder à la partie de confiance, utilisez le modèle de règle Autoriser tous les utilisateurs.  
  
## <a name="deny-access-to-users-with-this-incoming-claim"></a>Refuser l’accès aux utilisateurs avec cette revendication entrante  
Lorsque vous utilisez le modèle de règle Autoriser ou refuser les utilisateurs en fonction d’une revendication entrante pour créer une règle et définir la condition de refus, vous pouvez rejeter un accès utilisateur spécifique à la partie de confiance selon le type et la valeur d’une revendication entrante. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle qui rejette tous les utilisateurs dotés d’une revendication de groupe avec la valeur d’utilisateurs du domaine.  
  
Si vous souhaitez utiliser la condition de refus, mais aussi activer l’accès à la partie de confiance pour certains utilisateurs, vous devez ajouter ultérieurement et explicitement les règles d’autorisation avec la condition d’autorisation permettant d’activer l’accès de ces utilisateurs à la partie de confiance.  
  
Si l’accès est refusé à un utilisateur lorsque le moteur d’émission de revendications traite l’ensemble de règles, des règles supplémentaires de traitement s’arrête et AD FS renvoie une erreur « Accès refusé » à la demande de l’utilisateur.  
  
## <a name="authorizing-users"></a>Autorisation des utilisateurs  
Dans AD FS, les règles d’autorisation sont utilisés pour émettre une autorisation ou refuser des revendications qui déterminent si un utilisateur ou un groupe d’utilisateurs \(selon le type de revendication utilisé\) seront autorisés à accéder à Web\-en fonction des ressources dans une partie de confiance donné tiers ou non. Les règles d’autorisation ne peuvent être définies que sur des approbations de partie de confiance.  
  
### <a name="authorization-rule-sets"></a>Ensembles de règles d’autorisation  
Différents ensembles de règles d’autorisation s’appliquent selon le type d’opération d’autorisation ou de refus que vous devez configurer. Ces ensembles de règles sont les suivants :  
  
-   **Règles d’autorisation d’émission** : Ces règles déterminent si un utilisateur peut recevoir des revendications pour une partie de confiance et, par conséquent, accéder à la partie de confiance.  
  
-   **Règles d’autorisation de délégation** : Ces règles déterminent si un utilisateur peut agir comme un autre utilisateur avec la partie de confiance. Lorsqu’un utilisateur agit comme un autre utilisateur, les revendications relatives à l’utilisateur auteur de la requête sont toujours placées dans le jeton.  
  
-   **Règles d’autorisation d’emprunt d’identité** : Ces règles déterminent si un utilisateur peut se substituer complètement à un autre utilisateur face à la partie de confiance. Emprunter l’identité d’un autre utilisateur est une fonctionnalité très puissante, car la partie de confiance ne sait pas qu’il s’agit d’un autre utilisateur.  
  
Pour plus d’informations sur l’intégration du processus de règle d’autorisation dans le pipeline d’émission de revendications, consultez le rôle du moteur d’émission de revendications.  
  
### <a name="supported-claim-types"></a>Types de revendications pris en charge  
AD FS définit deux types de revendications qui sont utilisées pour déterminer si un utilisateur est autorisé ou refusé. Ces revendications type Uniform Resource Identifiers \(URI\) sont les suivantes :  
  
1.  **Autoriser**: http :\/\/schemas.microsoft.com\/autorisation\/revendications\/autoriser  
  
2.  **Refuser**: http :\/\/schemas.microsoft.com\/autorisation\/revendications\/refuser  
  
## <a name="how-to-create-this-rule"></a>Comment créer cette règle  
Vous pouvez créer les deux règles d’autorisation à l’aide du langage de règle de revendication ou les **autoriser tous les utilisateurs** le modèle de règle ou le **autoriser ou refuser les utilisateurs en fonction d’une revendication entrante** le modèle de règle dans les services AD FS Composant logiciel enfichable de gestion\-dans. Le modèle de règle Autoriser tous les utilisateurs ne fournit pas d’options de configuration. Toutefois, le modèle de règle Autoriser ou refuser les utilisateurs en fonction d’une revendication entrante fournit les options de configuration suivantes :  
  
-   Spécifier un nom de règle de revendication  
  
-   Spécifier un type de revendication entrante  
  
-   Taper une valeur de revendication entrante  
  
-   Autoriser l’accès aux utilisateurs avec cette revendication entrante  
  
-   Refuser l’accès aux utilisateurs avec cette revendication entrante  
  
Pour plus d’instructions sur la création de ce modèle, consultez [créer une règle pour autoriser tous les utilisateurs](https://technet.microsoft.com/library/ee913577.aspx) ou [créer une règle pour autoriser ou refuser les utilisateurs en fonction d’une revendication entrante](https://technet.microsoft.com/library/ee913594.aspx) dans le Guide de déploiement d’AD FS.  
  
## <a name="using-the-claim-rule-language"></a>Utilisation du langage des règles de revendication  
Si une revendication doit être envoyée uniquement lorsque la valeur de revendication correspond à un modèle personnalisé, vous devez utiliser une règle personnalisée. Pour plus d'informations, voir [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md).  
  
### <a name="example-of-how-to-create-an-authorization-rule-based-on-multiple-claims"></a>Exemple de création d’une règle d'autorisation basée sur des revendications multiples  
Lorsque vous utilisez la syntaxe du langage de règle de revendication pour autoriser les revendications, une revendication peut être également émise en fonction de la présence de plusieurs revendications d’origine de l’utilisateur. La règle suivante émet une revendication d’autorisation uniquement si l’utilisateur est membre du groupe Éditeurs et qu’il s’est authentifié avec le mécanisme d’authentification Windows :  
  
```  
[type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod",   
value == "urn:federation:authentication:windows" ]  
&& [type == "http://schemas.xmlsoap.org/claims/Group ", value == “editors”]   
=> issue(type = "http://schemas.xmlsoap.org/claims/authZ", value = "Granted");  
```  
  
### <a name="example-of-how-to-create-authorization-rules-that-will-delegate-who-can-create-or-remove-federation-server-proxy-trusts"></a>Exemple de création des règles d’autorisation qui délèguent le pouvoir de créer ou de supprimer des approbations de serveur proxy de fédération  
Avant qu’un service de fédération puisse utiliser un serveur proxy de fédération pour rediriger les requêtes du client, une relation d’approbation doit d'abord être établie entre le service de fédération et le proxy du serveur de fédération. Par défaut, une approbation de proxy est établie lorsqu’une des informations d’identification suivantes est fournie dans l’Assistant Configuration du serveur proxy de fédération AD FS :  
  
-   Le compte de service, utilisé par le service de fédération, que le proxy protège  
  
-   Un compte du domaine Active Directory qui est membre du groupe Administrateurs local sur tous les serveurs de fédération d’une batterie de serveurs de fédération  
  
Lorsque vous souhaitez spécifier quels utilisateurs peuvent créer une approbation de proxy pour un service de fédération donné, vous pouvez utiliser une des méthodes de délégation suivantes. Cette liste de méthodes s’affiche par ordre de priorité, selon les recommandations de l’équipe des produits AD FS sur les méthodes de délégation les plus sûres et les plus fiables. Il est nécessaire de se limiter à une seule de ces méthodes, selon les besoins de votre organisation :  
  
1.  Créer un groupe de sécurité de domaine dans Active Directory \(par exemple, FSProxyTrustCreators\), ajoutez ce groupe au groupe Administrateurs local sur chacun des serveurs de fédération de la batterie de serveurs, puis ajoutez uniquement les comptes d’utilisateur auquel vous souhaitez Pour déléguer ce droit au nouveau groupe. Il s’agit de la méthode recommandée.  
  
2.  Ajoutez le compte de domaine de l’utilisateur au groupe Administrateurs sur chacun des serveurs de fédération de la batterie de serveurs.  
  
3.  Si pour une raison quelconque vous ne pouvez pas utiliser une de ces méthodes, vous pouvez également créer une règle d’autorisation à cet effet. Bien que cela ne soit pas recommandé en raison de complications risquant de survenir si cette règle n’est pas écrite correctement, vous pouvez utiliser une règle d’autorisation personnalisée pour déléguer le pouvoir de créer ou même de supprimer les approbations entre tous les serveurs proxy de fédération qui sont associés à un service de fédération donné à certains comptes d’utilisateur du domaine Active Directory.  
  
    Si vous choisissez la méthode 3, vous pouvez utiliser la syntaxe de règle suivante pour émettre une revendication d’autorisation qui permet à un utilisateur spécifié \(dans ce cas, contoso\\frankm\) pour créer des approbations de fédération d’un ou plusieurs serveurs proxy à la Service de fédération. Vous devez appliquer cette règle à l’aide de la commande Windows PowerShell **définir\-ADFSProperties AddProxyAuthorizationRules**.  
  
    ```  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", issuer=~"^AD AUTHORITY$" value == "contoso\frankm" ] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true")  
  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
    Ultérieurement, pour supprimer l’utilisateur de sorte qu’il ne puisse plus créer d’approbations de proxy, vous pouvez revenir à la règle d’autorisation d’approbation de proxy par défaut pour supprimer le droit de créer des approbations de proxy pour le service de fédération. Vous devez également appliquer cette règle à l’aide de la commande Windows PowerShell **définir\-ADFSProperties AddProxyAuthorizationRules**.  
  
    ```  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
Pour plus d’informations sur l’utilisation du langage de règle de revendication, consultez [The Role of du langage des règles de revendication](The-Role-of-the-Claim-Rule-Language.md).  
  

