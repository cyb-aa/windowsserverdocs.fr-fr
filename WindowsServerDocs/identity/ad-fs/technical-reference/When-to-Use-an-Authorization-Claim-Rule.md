---
ms.assetid: b734cbcb-342c-4a28-8ab5-b9cd990bb1c2
title: Quand utiliser une règle de revendication d’autorisation
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2e8be891a8f09e38809503c40469e21a7e99687a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853762"
---
# <a name="when-to-use-an-authorization-claim-rule"></a>Quand utiliser une règle de revendication d’autorisation
Vous pouvez utiliser cette règle dans Services ADFS \(AD FS\) lorsque vous devez prendre un type de revendication entrante, puis appliquer une action qui détermine si l’accès est autorisé ou refusé à un utilisateur en fonction de la valeur que vous spécifiez dans la règle. Lorsque vous utilisez cette règle, vous transmettez ou transformez les revendications qui correspondent à la logique de règle suivante, en fonction des options que vous configurez dans la règle :  
  
|Option de règle|Logique de règle|  
|---------------|--------------|  
|Autoriser tous les utilisateurs|Si le type de revendication entrante est égal à *tout type de revendication* et que la valeur est égale à *toute valeur*, la revendication émise avec la valeur est égale à *Autoriser*|  
|Autoriser l’accès aux utilisateurs avec cette revendication entrante|Si le type de revendication entrante est égal à *type de revendication spécifié* et que la valeur est égale à *valeur de revendication spécifiée*, la revendication émise avec la valeur est égale à *Autoriser*|  
|Refuser l’accès aux utilisateurs avec cette revendication entrante|Si le type de revendication entrante est égal à *type de revendication spécifié* et que la valeur est égale à *valeur de revendication spécifiée*, la revendication émise avec la valeur est égale à*Refuser*|  
  
Les sections suivantes constituent une introduction aux règles de revendication et fournissent des informations détaillées sur les conditions d’utilisation de cette règle.  
  
## <a name="about-claim-rules"></a>À propos des règles de revendication  
Une règle de revendication représente une instance de logique métier qui prend une revendication entrante, lui applique une condition \(si x Then y\) et génère une revendication sortante en fonction des paramètres de condition. La liste suivante présente d’importantes astuces sur les règles de revendication dont vous devez prendre connaissance avant de poursuivre la lecture de cette rubrique :  
  
-   Dans le\-du composant logiciel enfichable Gestion de l’AD FS dans, les règles de revendication peuvent uniquement être créées à l’aide de modèles de règle de revendication  
  
-   Les règles de revendication traitent les revendications entrantes soit directement à partir d’un fournisseur de revendications \(comme Active Directory ou un autre service FS (Federation Service)\), soit à partir de la sortie des règles de transformation d’acceptation sur une approbation de fournisseur de revendications.  
  
-   Les règles de revendication sont traitées par le moteur d’émission des revendications au sein d’un ensemble de règles donné et dans l’ordre chronologique. En définissant la hiérarchie des règles, vous pouvez affiner ou filtrer des revendications générées par des règles précédentes dans un ensemble de règles donné.  
  
-   Les modèles de règles de revendication vous obligent toujours à spécifier un type de revendication entrante. Toutefois, vous pouvez traiter plusieurs valeurs de revendication avec le même type de revendication en vous appuyant sur une règle unique.  
  
Pour plus d’informations sur les règles de revendication et les ensembles de règles de revendication, consultez [rôle des règles de revendication](The-Role-of-Claim-Rules.md). Pour plus d’informations sur la façon dont les règles sont traitées, consultez [le rôle du moteur de revendications](The-Role-of-the-Claims-Engine.md). Pour plus d’informations sur le traitement des ensembles de règles de revendication, consultez [le rôle du pipeline de revendications](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="permit-all-users"></a>Autoriser tous les utilisateurs  
Lorsque vous utilisez le modèle de règle Autoriser tous les utilisateurs, tous les utilisateurs ont accès à la partie de confiance. Vous pouvez toutefois utiliser des règles d’autorisation supplémentaires pour restreindre davantage les accès. Si une règle autorise un utilisateur à accéder à la partie de confiance et qu’une autre règle refuse l’accès de l’utilisateur à la partie de confiance, le refus l’emporte sur l’autorisation et l’accès est refusé à l’utilisateur.  
  
Les utilisateurs dont l’accès à la partie de confiance est autorisé par le service de fédération peuvent se voir refuser le service par la partie de confiance.  
  
## <a name="permit-access-to-users-with-this-incoming-claim"></a>Autoriser l’accès aux utilisateurs avec cette revendication entrante  
Lorsque vous utilisez le modèle de règle autoriser ou refuser des utilisateurs en fonction d’une revendication entrante pour créer une règle et définir la condition sur autoriser, vous pouvez autoriser l’accès d’un utilisateur spécifique à la partie de confiance en fonction du type et de la valeur d’une revendication entrante. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle qui autorise uniquement les utilisateurs dotés d’une revendication de groupe avec la valeur d’administrateurs du domaine. Si une règle autorise un utilisateur à accéder à la partie de confiance et qu’une autre règle refuse l’accès de l’utilisateur à la partie de confiance, le refus l’emporte sur l’autorisation et l’accès est refusé à l’utilisateur.  
  
Les utilisateurs dont l’accès à la partie de confiance est autorisé par le service de fédération peuvent se voir refuser le service par la partie de confiance. Pour autoriser tous les utilisateurs à accéder à la partie de confiance, utilisez le modèle de règle Autoriser tous les utilisateurs.  
  
## <a name="deny-access-to-users-with-this-incoming-claim"></a>Refuser l’accès aux utilisateurs avec cette revendication entrante  
Lorsque vous utilisez le modèle de règle autoriser ou refuser des utilisateurs en fonction d’une revendication entrante pour créer une règle et définir la condition sur refusé, vous pouvez refuser l’accès de l’utilisateur à la partie de confiance en fonction du type et de la valeur d’une revendication entrante. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle qui rejette tous les utilisateurs dotés d’une revendication de groupe avec la valeur d’utilisateurs du domaine.  
  
Si vous souhaitez utiliser la condition de refus, mais aussi activer l’accès à la partie de confiance pour certains utilisateurs, vous devez ajouter ultérieurement et explicitement les règles d’autorisation avec la condition d’autorisation permettant d’activer l’accès de ces utilisateurs à la partie de confiance.  
  
Si l’accès est refusé à un utilisateur lorsque le moteur d’émission de revendications traite l’ensemble de règles, le traitement des règles supplémentaires s’arrête et AD FS renvoie une erreur « Accès refusé » à la demande de l’utilisateur.  
  
## <a name="authorizing-users"></a>Autorisation des utilisateurs  
Dans AD FS, les règles d’autorisation sont utilisées pour émettre une revendication d’autorisation ou de refus qui détermine si un utilisateur ou un groupe d’utilisateurs \(selon le type de revendication utilisé\) est autorisé à accéder aux ressources de\-Web dans une partie de confiance donnée. Les règles d’autorisation ne peuvent être définies que sur des approbations de partie de confiance.  
  
### <a name="authorization-rule-sets"></a>Ensembles de règles d’autorisation  
Différents ensembles de règles d’autorisation s’appliquent selon le type d’opération d’autorisation ou de refus que vous devez configurer. Ces ensembles de règles sont les suivants :  
  
-   **Règles d’autorisation d’émission** : ces règles déterminent si un utilisateur peut recevoir des revendications pour une partie de confiance et, par conséquent, accéder à la partie de confiance.  
  
-   **Onglet Règles d’autorisation de délégation** : ces règles déterminent si un utilisateur peut agir comme un autre utilisateur avec la partie de confiance. Lorsqu’un utilisateur agit comme un autre utilisateur, les revendications relatives à l’utilisateur auteur de la requête sont toujours placées dans le jeton.  
  
-   **Règles d’autorisation d’emprunt d’identité** : ces règles déterminent si un utilisateur peut se substituer complètement à un autre utilisateur face à la partie de confiance. Emprunter l’identité d’un autre utilisateur est une fonctionnalité très puissante, car la partie de confiance ne sait pas qu’il s’agit d’un autre utilisateur.  
  
Pour plus d’informations sur l’intégration du processus de règle d’autorisation dans le pipeline d’émission de revendications, consultez le rôle du moteur d’émission de revendications.  
  
### <a name="supported-claim-types"></a>Types de revendications pris en charge  
AD FS définit deux types de revendication utilisés pour déterminer si un utilisateur est autorisé ou refusé. Ces identificateurs de ressource uniforme de type de revendication \(URI\) sont les suivants :  
  
1.  **Autoriser**: http :\/\/schemas.Microsoft.com\/autorisation\/revendications\/autoriser  
  
2.  **Deny**: http :\/\/schemas.Microsoft.com\/authorization\/revendications\/Deny  
  
## <a name="how-to-create-this-rule"></a>Comment créer cette règle  
Vous pouvez créer les deux règles d’autorisation à l’aide du langage de règle de revendication ou à l’aide du modèle de règle **autoriser tous les utilisateurs** ou **autoriser ou refuser des utilisateurs en fonction d’une revendication entrante** dans le\-du composant logiciel enfichable Gestion du AD FS dans. Le modèle de règle Autoriser tous les utilisateurs ne fournit pas d’options de configuration. Toutefois, le modèle de règle Autoriser ou refuser les utilisateurs en fonction d’une revendication entrante fournit les options de configuration suivantes :  
  
-   Spécifier un nom de règle de revendication  
  
-   Spécifier un type de revendication entrante  
  
-   Taper une valeur de revendication entrante  
  
-   Autoriser l’accès aux utilisateurs avec cette revendication entrante  
  
-   Refuser l’accès aux utilisateurs avec cette revendication entrante  
  
Pour plus d’informations sur la création de ce modèle, consultez [créer une règle pour autoriser tous les utilisateurs](https://technet.microsoft.com/library/ee913577.aspx) ou [créer une règle pour autoriser ou refuser des utilisateurs en fonction d’une revendication entrante](https://technet.microsoft.com/library/ee913594.aspx) dans le Guide de déploiement AD FS.  
  
## <a name="using-the-claim-rule-language"></a>Utilisation du langage des règles de revendication  
Si une revendication doit être envoyée uniquement lorsque la valeur de revendication correspond à un modèle personnalisé, vous devez utiliser une règle personnalisée. Pour plus d'informations, voir [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md).  
  
### <a name="example-of-how-to-create-an-authorization-rule-based-on-multiple-claims"></a>Exemple de création d’une règle d'autorisation basée sur des revendications multiples  
Lorsque vous utilisez la syntaxe du langage de règle de revendication pour autoriser les revendications, une revendication peut également être émise en fonction de la présence de plusieurs revendications dans les revendications d’origine de l’utilisateur. La règle suivante émet une revendication d’autorisation uniquement si l’utilisateur est membre du groupe Éditeurs et qu’il s’est authentifié avec le mécanisme d’authentification Windows :  
  
```  
[type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod",   
value == "urn:federation:authentication:windows" ]  
&& [type == "http://schemas.xmlsoap.org/claims/Group ", value == "editors"]   
=> issue(type = "http://schemas.xmlsoap.org/claims/authZ", value = "Granted");  
```  
  
### <a name="example-of-how-to-create-authorization-rules-that-will-delegate-who-can-create-or-remove-federation-server-proxy-trusts"></a>Exemple de création des règles d’autorisation qui délèguent le pouvoir de créer ou de supprimer des approbations de serveur proxy de fédération  
Avant qu’un service de fédération puisse utiliser un serveur proxy de fédération pour rediriger les requêtes du client, une relation d’approbation doit d'abord être établie entre le service de fédération et le proxy du serveur de fédération. Par défaut, une approbation de proxy est établie lorsqu’une des informations d’identification suivantes est fournie dans l’Assistant Configuration du serveur proxy de fédération AD FS :  
  
-   Le compte de service, utilisé par le service de fédération, que le proxy protège  
  
-   Un compte du domaine Active Directory qui est membre du groupe Administrateurs local sur tous les serveurs de fédération d’une batterie de serveurs de fédération  
  
Lorsque vous souhaitez spécifier quels utilisateurs peuvent créer une approbation de proxy pour un service de fédération donné, vous pouvez utiliser une des méthodes de délégation suivantes. Cette liste de méthodes est par ordre de priorité, selon les recommandations de l’équipe de produit AD FS des méthodes de délégation les plus sûres et les moins problématiques. Il est nécessaire de se limiter à une seule de ces méthodes, selon les besoins de votre organisation :  
  
1.  Créez un groupe de sécurité de domaine dans Active Directory \(par exemple, FSProxyTrustCreators\), ajoutez ce groupe au groupe Administrateurs local sur chacun des serveurs de Fédération de la batterie, puis ajoutez uniquement les comptes d’utilisateur auxquels vous souhaitez déléguer ce droit au nouveau groupe. Il s’agit de la méthode recommandée.  
  
2.  Ajoutez le compte de domaine de l’utilisateur au groupe Administrateurs sur chacun des serveurs de Fédération de la batterie.  
  
3.  Si pour une raison quelconque vous ne pouvez pas utiliser une de ces méthodes, vous pouvez également créer une règle d’autorisation à cet effet. Bien que cela ne soit pas recommandé en raison de complications risquant de survenir si cette règle n’est pas écrite correctement, vous pouvez utiliser une règle d’autorisation personnalisée pour déléguer le pouvoir de créer ou même de supprimer les approbations entre tous les serveurs proxy de fédération qui sont associés à un service de fédération donné à certains comptes d’utilisateur du domaine Active Directory.  
  
    Si vous choisissez la méthode 3, vous pouvez utiliser la syntaxe de règle suivante pour émettre une revendication d’autorisation qui permet à un utilisateur spécifié \(dans ce cas, contoso\\Frank\) de créer des approbations pour un ou plusieurs serveurs proxys de Fédération pour le service FS (Federation Service). Vous devez appliquer cette règle à l’aide du jeu de commandes Windows PowerShell **\-ADFSProperties AddProxyAuthorizationRules**.  
  
    ```  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", issuer=~"^AD AUTHORITY$" value == "contoso\frankm" ] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true")  
  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
    Ultérieurement, pour supprimer l’utilisateur de sorte qu’il ne puisse plus créer d’approbations de proxy, vous pouvez revenir à la règle d’autorisation d’approbation de proxy par défaut pour supprimer le droit de créer des approbations de proxy pour le service de fédération. Vous devez également appliquer cette règle à l’aide du jeu de commandes Windows PowerShell **\-ADFSProperties AddProxyAuthorizationRules**.  
  
    ```  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
Pour plus d’informations sur l’utilisation du langage de règle de revendication, consultez [le rôle du langage de règle de revendication](The-Role-of-the-Claim-Rule-Language.md).  
  

