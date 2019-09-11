---
title: Scénario de délégation d’identité avec AD FS
description: Ce scénario décrit une application qui doit accéder à des ressources principales qui requièrent la chaîne de délégation d’identité pour effectuer des contrôles de contrôle d’accès.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2c461e1051e59fcdb533c00b45157545ffb15df1
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867446"
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>Scénario de délégation d’identité avec AD FS


[À compter de la .NET Framework 4,5, WIF (Windows Identity Foundation) a été entièrement intégré dans le .NET Framework. La version de WIF traitée dans cette rubrique, WIF 3,5, est déconseillée et ne doit être utilisée que lors du développement sur le .NET Framework 3,5 SP1 ou le .NET Framework 4. Pour plus d’informations sur WIF dans le .NET Framework 4,5, également connu sous le nom de WIF 4,5, consultez la documentation de Windows Identity Foundation dans le .NET Framework 4,5 Guide de développement.] 

Ce scénario décrit une application qui doit accéder à des ressources principales qui requièrent la chaîne de délégation d’identité pour effectuer des contrôles de contrôle d’accès. Une chaîne de délégation d’identité simple se compose généralement des informations sur l’appelant initial et de l’identité de l’appelant immédiat.

Avec le modèle de délégation Kerberos sur la plateforme Windows aujourd’hui, les ressources principales ont accès uniquement à l’identité de l’appelant immédiat et non à celle de l’appelant initial. Ce modèle est communément appelé modèle de sous-système approuvé. WIF gère l’identité de l’appelant initial ainsi que l’appelant immédiat dans la chaîne de délégation à l’aide de la propriété Actor.

Le diagramme suivant illustre un scénario de délégation d’identité classique dans lequel un employé Fabrikam accède à des ressources exposées dans une application Contoso.com.

![Identité](media/ad-fs-identity-delegation/id1.png)

Les utilisateurs fictifs participant à ce scénario sont les suivants :

- Franc Un employé de Fabrikam qui souhaite accéder aux ressources contoso.
- Paul Un développeur d’applications Contoso qui implémente les modifications nécessaires dans l’application.
- Adam L’administrateur informatique de contoso.

Les composants impliqués dans ce scénario sont les suivants :

- web1 Application Web avec des liens vers des ressources principales qui requièrent l’identité déléguée de l’appelant initial. Cette application est générée avec ASP.NET.
- Service Web qui accède à un SQL Server, qui requiert l’identité déléguée de l’appelant initial, ainsi que celui de l’appelant immédiat. Ce service est généré avec WCF.
- sts1: STS qui est dans le rôle de fournisseur de revendications et qui émet des revendications attendues par l’application (web1). Il a établi une relation de confiance avec Fabrikam.com et également avec l’application.
- sts2: STS qui est dans le rôle de fournisseur d’identité pour Fabrikam.com et fournit un point de terminaison que l’employé de Fabrikam utilise pour s’authentifier. Il a établi une relation de confiance avec Contoso.com afin que les employés de Fabrikam soient autorisés à accéder aux ressources sur Contoso.com.

>[!NOTE] 
>Le terme « jeton ActAs », qui est souvent utilisé dans ce scénario, fait référence à un jeton émis par un STS et contenant l’identité de l’utilisateur. La propriété Actor contient l’identité du STS.

Comme indiqué dans le schéma précédent, le déroulement de ce scénario est le suivant :


1. L’application Contoso est configurée pour obtenir un jeton ActAs contenant à la fois l’identité de l’employé Fabrikam et l’identité de l’appelant immédiat dans la propriété Actor. Daniel a implémenté ces modifications dans l’application.
2. L’application Contoso est configurée pour transmettre le jeton ActAs au service principal. Daniel a implémenté ces modifications dans l’application.
3. Le service Web Contoso est configuré pour valider le jeton ActAs en appelant sts1. Adam a activé sts1 pour traiter les demandes de délégation.
4. L’utilisateur Fabrikam Frank accède à l’application Contoso et est autorisé à accéder aux ressources principales.

## <a name="set-up-the-identity-provider-ip"></a>Configurer le fournisseur d’identité (IP)

Trois options sont disponibles pour l’administrateur Fabrikam.com, Frank :


1. Achetez et installez un produit STS, par exemple Active Directory® Federation Services (AD FS).
2. Abonnez-vous à un produit STS Cloud tel que LiveID STS.
3. Générez un STS personnalisé à l’aide de WIF.

Pour cet exemple de scénario, Frank sélectionne option1 et installe AD FS comme IP-STS. Il configure également un point de terminaison, appelé \windowsauth, pour authentifier les utilisateurs. En vous référant à la documentation du produit AD FS et en parlant à Adam, l’administrateur informatique de contoso, Frank établit une relation de confiance avec le domaine Contoso.com.

## <a name="set-up-the-claims-provider"></a>Configurer le fournisseur de revendications

Les options disponibles pour l’administrateur Contoso.com, Adam, sont les mêmes que celles décrites précédemment pour le fournisseur d’identité. Pour cet exemple de scénario, nous partons du principe qu’Adam sélectionne l’option 1 et installe AD FS 2,0 comme RP-STS.

## <a name="set-up-trust-with-the-ip-and-application"></a>Configurer l’approbation avec l’adresse IP et l’application

En faisant référence à la documentation AD FS, Adam établit une relation de confiance entre Fabrikam.com et l’application.

## <a name="set-up-delegation"></a>Configurer la délégation

AD FS assure le traitement de la délégation. En faisant référence à la documentation AD FS, Adam permet le traitement des jetons ActAs.

## <a name="application-specific-changes"></a>Modifications spécifiques à l’application

Les modifications suivantes doivent être apportées pour ajouter la prise en charge de la délégation d’identité à une application existante. Daniel utilise WIF pour apporter ces modifications.


- Mettez en cache le jeton de démarrage que le web1 a reçu de sts1.
- Utilisez CreateChannelActingAs avec le jeton émis pour créer un canal vers le service Web principal.
- Appelez la méthode du service principal.

## <a name="cache-the-bootstrap-token"></a>Mettre en cache le jeton de démarrage

Le jeton de démarrage est le jeton initial émis par le STS et l’application extrait les revendications à partir de celui-ci. Dans cet exemple de scénario, ce jeton est émis par sts1 pour l’utilisateur Frank et l’application le met en cache. L’exemple de code suivant montre comment récupérer un jeton de démarrage dans une application ASP.NET :

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
WIF fournit une méthode, [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx), qui crée un canal du type spécifié qui augmente les demandes d’émission de jetons avec le jeton de sécurité spécifié en tant qu’élément ActAs. Vous pouvez passer le jeton de démarrage à cette méthode, puis appeler la méthode de service nécessaire sur le canal retourné. Dans cet exemple de scénario, l’identité de Frank a la propriété [Actor](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx) définie sur web1's Identity.

L’extrait de code suivant montre comment appeler le service Web avec [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx) , puis appeler l’une des méthodes du service, ComputeResponse, sur le canal retourné :

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
## <a name="web-service-specific-changes"></a>Modifications spécifiques au service Web

Étant donné que le service Web est généré avec WCF et activé pour WIF, une fois que la liaison est configurée avec IssuedSecurityTokenParameters avec l’adresse d’émetteur appropriée, la validation de la ActAs est gérée automatiquement par WIF. 

Le service Web expose les méthodes spécifiques requises par l’application. Aucune modification de code spécifique n’est nécessaire sur le service. L’exemple de code suivant montre la configuration du service Web avec IssuedSecurityTokenParameters :

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
[Développement des services AD FS](../../ad-fs/AD-FS-Development.md)  
