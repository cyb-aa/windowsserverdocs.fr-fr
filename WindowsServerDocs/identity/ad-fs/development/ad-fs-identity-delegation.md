---
title: Scénario de délégation d’identité avec AD FS
description: Ce scénario décrit une application qui doit accéder aux ressources de back-end qui nécessitent la chaîne de délégation d’identité pour effectuer des vérifications de contrôle d’accès.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b82d5fd749ac874d09bc54123727aaf902c4d778
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819850"
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>Scénario de délégation d’identité avec AD FS


[À compter de .NET Framework 4.5, Windows Identity Foundation (WIF) a été entièrement intégré à .NET Framework. La version de WIF décrit dans cette rubrique, WIF 3.5, est déconseillée et ne doit être utilisée lors du développement avec le .NET Framework 3.5 SP1 ou .NET Framework 4. Pour plus d’informations sur WIF dans .NET Framework 4.5, également appelé WIF 4.5, consultez la documentation de Windows Identity Foundation dans le Guide de développement .NET Framework 4.5.] 

Ce scénario décrit une application qui doit accéder aux ressources de back-end qui nécessitent la chaîne de délégation d’identité pour effectuer des vérifications de contrôle d’accès. Une chaîne de délégation d’identité simple se compose généralement des informations sur l’appelant initial et l’identité de l’appelant immédiat.

Le modèle de délégation Kerberos sur la plateforme Windows dès aujourd'hui, les ressources back-end doivent accéder uniquement à l’identité de l’appelant immédiat et non à celle de l’appelant initial. Ce modèle est communément appelé modèle de sous-système approuvé. WIF gère l’identité de l’appelant initial, ainsi que l’appelant immédiat dans la chaîne de délégation à l’aide de la propriété de l’acteur.

Le diagramme suivant illustre un scénario de délégation d’identité standard dans lequel un employé de Fabrikam accède aux ressources exposées dans une application de Contoso.com.

![Identité](media/ad-fs-identity-delegation/id1.png)

Les utilisateurs fictifs participant à ce scénario sont :

- Frank : Un employé de Fabrikam souhaite accéder aux ressources de Contoso.
- Daniel : Un développeur d’applications Contoso qui implémente les modifications nécessaires dans l’application.
- ADAM : L’administrateur informatique de Contoso.

Les composants impliqués dans ce scénario sont :

- web1 : Une application Web avec des liens vers des ressources back-end qui nécessitent l’identité déléguée de l’appelant initial. Cette application est générée avec ASP.NET.
- Un service Web qui accède à un serveur SQL, ce qui nécessite l’identité déléguée de l’appelant initial, ainsi que celle de l’appelant immédiat. Ce service est créé avec WCF.
- sts1: Un service STS qui est dans le rôle de fournisseur de revendications et émet des revendications qui sont attendues par l’application (web1). Il a établi l’approbation avec Fabrikam.com ainsi qu’avec l’application.
- sts2: Un service STS qui est dans le rôle de fournisseur d’identité pour Fabrikam.com et fournit un point de terminaison, les employés de Fabrikam utilise pour s’authentifier. Il a établi la relation d’approbation avec Contoso.com afin que les employés de Fabrikam sont autorisés à accéder aux ressources de Contoso.com.

>[!NOTE] 
>Le terme « Jeton ActAs », qui est souvent utilisé dans ce scénario, fait référence à un jeton qui est émis par un STS et contient l’identité de l’utilisateur. La propriété de l’acteur contient les identités du STS.

Comme indiqué dans le diagramme précédent, le flux dans ce scénario est :


1. L’application Contoso est configurée pour obtenir un jeton ActAs qui contient l’identité de l’employé de Fabrikam et identité de l’appelant immédiat dans la propriété de l’acteur. Daniel a implémenté ces modifications à l’application.
2. L’application Contoso est configurée pour transmettre le jeton ActAs pour le service back-end. Daniel a implémenté ces modifications à l’application.
3. Le service Web de Contoso est configuré pour valider le jeton ActAs en appelant sts1. ADAM a activé sts1 traiter les demandes de délégation.
4. Utilisateur de Fabrikam Frank accède à l’application Contoso et accède aux ressources back-end.

## <a name="set-up-the-identity-provider-ip"></a>Configurer le fournisseur d’identité (IP)

Il existe trois options disponibles pour l’administrateur de Fabrikam.com, Frank :


1. Acheter et installer un produit de STS tels que Active Directory® Federation Services (ADFS).
2. S’abonner à un produit de STS cloud tel que LiveID STS.
3. Créer un STS personnalisé à l’aide de WIF.

Pour cet exemple de scénario, nous partons du principe que Frank sélectionne option1 et installe les services AD FS en tant que l’IP-STS. Il configure également un point de terminaison nommé \windowsauth, pour authentifier les utilisateurs. En faisant référence à la documentation du produit AD FS et communique avec Adam, l’administrateur informatique de Contoso, Frank établit la relation d’approbation avec le domaine Contoso.com.

## <a name="set-up-the-claims-provider"></a>Configurer le fournisseur de revendications

Les options disponibles pour l’administrateur de Contoso.com, Adam, sont identiques à celles décrites précédemment pour le fournisseur d’identité. Pour cet exemple de scénario, nous partons du principe que Adam sélectionne l’Option 1 et installe les services AD FS 2.0 en tant que le service RP-STS.

## <a name="set-up-trust-with-the-ip-and-application"></a>Configurer la relation d’approbation avec l’adresse IP et l’Application

En vous reportant à la documentation d’AD FS, Adam établit la relation d’approbation entre Fabrikam.com et l’application.

## <a name="set-up-delegation"></a>Configurer la délégation

AD FS fournit le traitement de la délégation. En vous reportant à la documentation d’AD FS, Adam permet le traitement des jetons de ActAs.

## <a name="application-specific-changes"></a>Modifications spécifiques à l’application

Les modifications suivantes doivent être apportées pour ajouter la prise en charge de la délégation d’identité à une application existante. Daniel utilise WIF pour effectuer ces modifications.


- Mettre en cache le jeton de démarrage que web1 provenant de sts1.
- Utilisez CreateChannelActingAs avec le jeton émis pour créer un canal pour le service Web back-end.
- Appelez la méthode du service de serveur principal.

## <a name="cache-the-bootstrap-token"></a>Mettre en cache le jeton de démarrage

Le jeton de démarrage est le jeton initial émis par le STS et l’application extrait les revendications à partir de celui-ci. Dans cet exemple de scénario, ce jeton est émis par sts1 pour l’utilisateur Frank, et l’application met en cache. L’exemple de code suivant montre comment récupérer des données d’amorçage jeton dans une application ASP.NET :

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
WIF fournit une méthode, [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx), qui crée un canal du type spécifié qui augmente les demandes d’émission de jeton avec le jeton de sécurité spécifié comme un élément ActAs. Vous pouvez passer le jeton de démarrage à cette méthode et puis appelez la méthode de service nécessaire sur le canal retourné. Dans cet exemple de scénario, les identités de Frank a la [acteur](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx) propriété définie sur identité de web1.

L’extrait de code suivant montre comment appeler le service Web avec [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx) et appelez ensuite une des méthodes du service, ComputeResponse, sur le canal retourné :

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

Dans la mesure où le service Web créé avec WCF et activé pour WIF, une fois que la liaison est configurée avec IssuedSecurityTokenParameters avec l’adresse d’émetteur appropriée, la validation de la ActAs est gérée automatiquement par WIF. 

Le service Web expose les méthodes spécifiques requises par l’application. Il n’y a aucune modification de code spécifique du service. L’exemple de code suivant montre la configuration du service Web avec IssuedSecurityTokenParameters :

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
[Développement de AD FS](../../ad-fs/AD-FS-Development.md)  
