---
title: "Scénario de délégation d’identité avec ADFS"
description: "Ce scénario décrit une application qui doit accéder aux ressources principal qui nécessitent la chaîne de la délégation d’identité pour effectuer des vérifications de contrôle d’accès."
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b82d5fd749ac874d09bc54123727aaf902c4d778
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>Scénario de délégation d’identité avec ADFS


[Depuis le .NETFramework 4.5, WindowsIdentityFoundation (WIF) a été entièrement intégrée à .NETFramework. La version de WIF décrit dans cette rubrique, WIF 3.5 est déconseillée et ne doit être utilisée pour le développement pour le .NETFramework 3.5SP1 ou .NETFramework 4. Pour plus d’informations sur WIF dans le .NETFramework 4.5, également appelé WIF 4.5, consultez la documentation de WindowsIdentityFoundation dans le Guide de développement .NETFramework 4.5.] 

Ce scénario décrit une application qui doit accéder aux ressources principal qui nécessitent la chaîne de la délégation d’identité pour effectuer des vérifications de contrôle d’accès. Une chaîne de délégation d’identité simple se compose généralement des informations sur l’appelant initial et l’identité de l’appelant immédiat.

Le modèle de délégation Kerberos sur la plateforme Windows aujourd'hui, les ressources principal doivent accéder uniquement à l’identité de l’appelant immédiat et pas à celle de l’appelant initial. Ce modèle est communément appelé le modèle de sous-système approuvé. WIF conserve l’identité de l’appelant initial, ainsi que l’appelant immédiat dans la chaîne de délégation à l’aide de la propriété de l’intervenant.

Le diagramme suivant illustre un scénario de délégation d’identité classique dans lequel un employé de Fabrikam accède aux ressources exposées dans une application Contoso.com.

![Identité](media/ad-fs-identity-delegation/id1.png)

Les utilisateurs fictifs participant dans ce scénario sont:

- Frank: Un employé de Fabrikam qui souhaite accéder aux ressources de Contoso.
- Daniel: Un Contoso développeur d’applications qui implémente les modifications nécessaires dans l’application.
- ADAM: Contoso administrateur informatique.

Les composants impliqués dans ce scénario sont:

- web1: une application Web avec des liens vers des ressources principal qui exigent que l’identité de l’appelant initial déléguée. Cette application est générée avec ASP.NET.
- Un service Web qui accède à un serveur SQLServer, ce qui nécessite l’identité du délégué de l’appelant initial, ainsi que celui de l’appelant immédiat. Ce service est créé avec WCF.
- STS1: un service STS qui se trouve dans le rôle de fournisseur de revendications et émet les revendications qui sont attendues par l’application (web1). Il a établi l’approbation avec les Fabrikam.com et l’application.
- sts2: un service STS qui figure dans le rôle de fournisseur d’identité pour Fabrikam.com et fournit un point de terminaison, l’employé de Fabrikam utilise pour s’authentifier. Il a établi la relation d’approbation avec Contoso.com afin que les employés de Fabrikam sont autorisés à accéder aux ressources sur Contoso.com.

>[!NOTE] 
>Le terme "Jeton ActAs", qui est souvent utilisé dans ce scénario, fait référence à un jeton est émis par un service STS et qui contient l’identité de l’utilisateur. La propriété acteur contient identité de l’émission.

Comme indiqué dans le diagramme précédent, le flux dans ce scénario est:


1. L’application Contoso est configurée pour obtenir un jeton ActAs qui contient l’identité de l’employé de Fabrikam et identité de l’appelant immédiat dans la propriété de l’intervenant. Daniel a implémenté ces modifications à l’application.
2. L’application Contoso est configurée pour transmettre le jeton ActAs au service principal. Daniel a implémenté ces modifications à l’application.
3. Le service Web de Contoso est configuré pour valider le jeton ActAs en appelant sts1. ADAM a activé sts1 traiter les demandes de délégation.
4. Fabrikam utilisateur Frank accède à l’application Contoso et est autorisé à accéder aux ressources principal.

## <a name="set-up-the-identity-provider-ip"></a>Configurer le fournisseur d’identité (IP)

Il existe trois options disponibles pour l’administrateur Fabrikam.com, Frank:


1. Acheter et installer un produit STS tels que les Services de fédération ActiveDirectory® (ADFS).
2. S’abonner à un produit de STS cloud comme LiveID STS.
3. Créer un service STS personnalisé à l’aide de WIF.

Pour cet exemple de scénario, nous partons du principe que Frank sélectionne option1 et installe l’IP-STS ADFS. Il configure également un point de terminaison, nommé \windowsauth, pour authentifier les utilisateurs. En faisant référence à la documentation du produit ADFS et donner la parole à Adam, l’administrateur informatique de Contoso, Frank établit une relation d’approbation avec le domaine Contoso.com.

## <a name="set-up-the-claims-provider"></a>Configurer le fournisseur de revendications

Les options disponibles pour l’administrateur Contoso.com, Adam, sont les mêmes que ceux décrits précédemment pour le fournisseur d’identité. Pour cet exemple de scénario, nous partons du principe qu’Adam sélectionne l’Option 1 et installe le RP-service STS ADFS 2.0.

## <a name="set-up-trust-with-the-ip-and-application"></a>Configurer la relation d’approbation avec l’adresse IP et l’Application

En faisant référence à la documentation ADFS, Adam établit une relation d’approbation entre Fabrikam.com et l’application.

## <a name="set-up-delegation"></a>Configurer la délégation

ADFS fournit le traitement de la délégation. En faisant référence à la documentation ADFS, Adam permet le traitement des jetons ActAs.

## <a name="application-specific-changes"></a>Modifications spécifiques à l’application

Les modifications suivantes doivent être apportées pour prendre en charge pour la délégation d’identité pour une application existante. Daniel utilise WIF pour apporter ces modifications.


- Mettre en cache le jeton d’amorçage qui web1 reçu à partir de sts1.
- Utilisez CreateChannelActingAs avec le jeton émis pour créer un canal pour le service Web principal.
- Appelez la méthode du service principal.

## <a name="cache-the-bootstrap-token"></a>Mettre en cache le jeton d’amorçage

Le jeton d’amorçage est le jeton initial émis par le STS et l’application extrait les revendications à partir de celui-ci. Dans cet exemple de scénario, ce jeton est émis par sts1 pour l’utilisateur Frank et l’application met en cache. L’exemple de code suivant montre comment récupérer les données d’amorçage jeton dans une application ASP.NET:

```
// Get the Bootstrap Token
SecurityToken bootstrapToken = null;

IClaimsPrincipal claimsPrincipal = Thread.CurrentPrincipal as IClaimsPrincipal;
if ( claimsPrincipal != null )
{
    IClaimsIdentity claimsIdentity = (IClaimsIdentity)claimsPrincipal.Identity;
    bootstrapToken = claimsIdentity.BootstrapToken;
}
```
WIF fournit une méthode, [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx), qui crée un canal du type spécifié qui augmente les demandes d’émission de jeton avec le jeton de sécurité spécifié comme un élément ActAs. Vous pouvez transmettre le jeton d’amorçage à cette méthode et appelez ensuite la méthode de service nécessaires sur le canal retourné. Dans cet exemple de scénario a identité de Frank le [acteur](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx) propriété définie sur l’identité de web1.

L’extrait de code suivant montre comment appeler le service Web avec [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx), puis appelez une des méthodes du service, ComputeResponse, sur le canal retourné:

```
// Get the channel factory to the backend service from the application state
ChannelFactory<IService2Channel> factory = (ChannelFactory<IService2Channel>)Application[Global.CachedChannelFactory];

// Create and setup channel to talk to the backend service
IService2Channel channel;
lock (factory)
{
// Setup the ActAs to point to the caller's token so that we perform a 
// delegated call to the backend service
// on behalf of the original caller.
    channel = factory.CreateChannelActingAs<IService2Channel>(callerToken);
}

string retval = null;

// Call the backend service and handle the possible exceptions
try
{
    retval = channel.ComputeResponse(value);
    channel.Close();
} catch (Exception exception)
{
    StringBuilder sb = new StringBuilder();
    sb.AppendLine("An unexpected exception occurred.");
    sb.AppendLine(exception.StackTrace);
    channel.Abort();
    retval = sb.ToString();
}

```
## <a name="web-service-specific-changes"></a>Modifications spécifiques au Service Web

Dans la mesure où le service Web est créé avec WCF et activé pour WIF, une fois que la liaison est configurée avec IssuedSecurityTokenParameters avec l’adresse de l’émetteur appropriée, la validation de l’ActAs est gérée automatiquement par WIF. 

Le service Web expose les méthodes spécifiques requis par l’application. Il n’existe aucune modification du code spécifique requis sur le service. L’exemple de code suivant illustre la configuration du service Web avec IssuedSecurityTokenParameters:

```
// Configure the issued token parameters with the correct settings
IssuedSecurityTokenParameters itp = new IssuedSecurityTokenParameters( "http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1" );
itp.IssuerMetadataAddress = new EndpointAddress( "http://localhost:6000/STS/mex" );
itp.IssuerAddress = new EndpointAddress( "http://localhost:6000/STS" );

// Create the security binding element
SecurityBindingElement sbe = SecurityBindingElement.CreateIssuedTokenForCertificateBindingElement( itp );
sbe.MessageSecurityVersion = MessageSecurityVersion.WSSecurity11WSTrust13WSSecureConversation13WSSecurityPolicy12BasicSecurityProfile10;

// Create the HTTP transport binding element
HttpTransportBindingElement httpBE = new HttpTransportBindingElement();

// Create the custom binding using the prepared binding elements
CustomBinding binding = new CustomBinding( sbe, httpBE );

using ( ServiceHost host = new ServiceHost( typeof( Service2 ), new Uri( "http://localhost:6002/Service2" ) ) )
{
    host.AddServiceEndpoint( typeof( IService2 ), binding, "" );
    host.Credentials.ServiceCertificate.SetCertificate( "CN=localhost", StoreLocation.LocalMachine, StoreName.My );

// Enable metadata generation via HTTP GET
    ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
    smb.HttpGetEnabled = true;
    host.Description.Behaviors.Add( smb );
    host.AddServiceEndpoint( typeof( IMetadataExchange ), MetadataExchangeBindings.CreateMexHttpBinding(), "mex" );

// Configure the service host to use WIF
    ServiceConfiguration configuration = new ServiceConfiguration();
    configuration.IssuerNameRegistry = new TrustedIssuerNameRegistry();

    FederatedServiceCredentials.ConfigureServiceHost( host, configuration );

    host.Open();

    Console.WriteLine( "Service2 started, press ENTER to stop ..." );
    Console.ReadLine();

    host.Close();
}
```

## <a name="next-steps"></a>Étapes suivantes
[Développement d’ADFS](../../ad-fs/AD-FS-Development.md)  
