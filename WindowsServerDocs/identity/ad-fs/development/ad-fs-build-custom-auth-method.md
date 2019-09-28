---
title: Créer une méthode d’authentification personnalisée pour AD FS dans Windows Server
description: Ce scénario décrit comment créer une méthode d’authentification personnalisée pour AD FS dans Windows Server.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/23/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2ef16ddeb241d55b61b484805ff91cb247985d8d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358884"
---
# <a name="build-a-custom-authentication-method-for-ad-fs-in-windows-server"></a>Créer une méthode d’authentification personnalisée pour AD FS dans Windows Server

Cette procédure pas à pas fournit des instructions pour l’implémentation d’une méthode d’authentification personnalisée pour AD FS dans Windows Server 2012 R2. Pour plus d’informations, consultez [méthodes d’authentification supplémentaires](https://msdn.microsoft.com/library/dn758113\(v=msdn.10\)).


> [!WARNING]
> L’exemple que vous pouvez générer ici est&nbsp;fourni à titre éducatif uniquement. &nbsp;Ces instructions sont destinées à l’implémentation la plus simple et la plus minimale possible pour exposer les éléments requis du modèle.&nbsp; Il n’y a pas d’back end d’authentification, de traitement des erreurs ou de données de configuration. 
> <P></P>



## <a name="setting-up-the-development-box"></a>Configuration de la zone de développement

Cette procédure pas à pas utilise Visual Studio 2012.  Le projet peut être généré à l’aide de n’importe quel environnement de développement capable de créer une classe .NET pour Windows. Le projet doit cibler .NET 4,5, car les méthodes **BeginAuthentication** et **TryEndAuthentication** utilisent le type **System. Security. Claims. claim**, qui fait partie de .NET Framework version 4.5. une référence est requise pour le projet :


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Dll de référence</strong></p></td>
<td><p><strong>Où la trouver</strong></p></td>
<td><p><strong>Requis pour</strong></p></td>
</tr>
<tr class="even">
<td><p>Microsoft. IdentityServer. Web. dll</p></td>
<td><p>La dll se trouve dans%windir%\ADFS sur un serveur Windows Server 2012 R2 sur lequel AD FS a été installé.</p>
<p></p>
<p>Cette dll doit être copiée sur l’ordinateur de développement et une référence explicite créée dans le projet.</p></td>
<td><p>Types d’interface, notamment IAuthenticationContext, IProofData</p></td>
</tr>
</tbody>
</table>


## <a name="create-the-provider"></a>Créer le fournisseur

1.  Dans Visual Studio 2012 : Choisissez fichier-\>nouveau-\>projet...

2.  Sélectionnez Bibliothèque de classes et assurez-vous que vous ciblez .NET 4,5.

    ![créer le fournisseur](media/ad-fs-build-custom-auth-method/Dn783423.71a57ae1-d53d-462b-a846-5b3c02c7d3f2(MSDN.10).jpg "créer le fournisseur")

3.  Effectuez une copie de **Microsoft. IdentityServer. Web. dll** à partir de% windir% \\ADFS sur le serveur Windows Server 2012 R2 sur lequel AD FS a été installé, puis collez-le dans votre dossier de projet sur votre ordinateur de développement.

4.  Dans **Explorateur de solutions**, cliquez avec le bouton droit sur **références** et **Ajoutez une référence...**

5.  Accédez à votre copie locale de **Microsoft. IdentityServer. Web. dll** et **Ajoutez...**

6.  Cliquez sur **OK** pour confirmer la nouvelle référence :

    ![créer le fournisseur](media/ad-fs-build-custom-auth-method/Dn783423.f18df353-9259-4744-b4b6-dd780ce90951(MSDN.10).jpg "créer le fournisseur")

    Vous devez maintenant être configuré pour résoudre tous les types requis pour le fournisseur. 

7.  Ajoutez une nouvelle classe à votre projet (cliquez avec le bouton droit sur votre projet, puis sur **Ajouter... Classe...** ) et donnez-lui un nom tel que **MyAdapter**, comme indiqué ci-dessous :

    ![créer le fournisseur](media/ad-fs-build-custom-auth-method/Dn783423.6b6a7a8b-9d66-40c7-8a86-a2e3b9e14d09(MSDN.10).jpg "créer le fournisseur")

8.  Dans le nouveau fichier MyAdapter.cs, remplacez le code existant par ce qui suit :

        using System;
         using System.Collections.Generic;
         using System.Linq;
         using System.Text;
         using System.Threading.Tasks;
         using System.Globalization;
         using System.IO;
         using System.Net;
         using System.Xml.Serialization;
         using Microsoft.IdentityServer.Web.Authentication.External;
         using Claim = System.Security.Claims.Claim;

         namespace MFAadapter
         {
         class MyAdapter : IAuthenticationAdapter
         {

         }
         }

    Maintenant, vous devez être en mesure de cliquer avec le bouton droit sur IAuthenticationAdapter pour afficher l’ensemble des membres d’interface requis. 

    Ensuite, vous pouvez effectuer une implémentation simple de ces.

9.  Remplacez l’intégralité du contenu de votre classe par le code suivant :

        namespace MFAadapter
         {
         class MyAdapter : IAuthenticationAdapter
         {
         public IAuthenticationAdapterMetadata Metadata
         {
         //get { return new <instance of IAuthenticationAdapterMetadata derived class>; }
         }

         public IAdapterPresentation BeginAuthentication(Claim identityClaim, HttpListenerRequest request, IAuthenticationContext authContext)
         {
         //return new instance of IAdapterPresentationForm derived class

         }

         public bool IsAvailableForUser(Claim identityClaim, IAuthenticationContext authContext)
         {
         return true; //its all available for now

         }

         public void OnAuthenticationPipelineLoad(IAuthenticationMethodConfigData configData)
         {
         //this is where AD FS passes us the config data, if such data was supplied at registration of the adapter

         }

         public void OnAuthenticationPipelineUnload()
         {

         }

         public IAdapterPresentation OnError(HttpListenerRequest request, ExternalAuthenticationException ex)
         {
         //return new instance of IAdapterPresentationForm derived class

         }

         public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
         {
         //return new instance of IAdapterPresentationForm derived class

         }

         }
         }

10. Nous ne sommes pas encore prêts à générer... Il y a deux autres interfaces à atteindre.

    Ajoutez deux classes supplémentaires à votre projet : l’une concerne les métadonnées et l’autre pour le formulaire de présentation.  Vous pouvez les ajouter dans le même fichier que la classe ci-dessus.

        class MyMetadata : IAuthenticationAdapterMetadata
         {

         }

         class MyPresentationForm : IAdapterPresentationForm
         {

         }

11. Ensuite, vous pouvez ajouter les membres requis pour chaque. Tout d’abord, les métadonnées (avec des commentaires Inline utiles)

        class MyMetadata : IAuthenticationAdapterMetadata
         {
         //Returns the name of the provider that will be shown in the AD FS management UI (not visible to end users)
         public string AdminName
         {
         get { return "My Example MFA Adapter"; }
         }

         //Returns an array of strings containing URIs indicating the set of authentication methods implemented by the adapter 
         /// AD FS requires that, if authentication is successful, the method actually employed will be returned by the
         /// final call to TryEndAuthentication(). If no authentication method is returned, or the method returned is not
         /// one of the methods listed in this property, the authentication attempt will fail.
         public virtual string[] AuthenticationMethods 
         {
         get { return new[] { "http://example.com/myauthenticationmethod1", "http://example.com/myauthenticationmethod2" }; }
         }

         /// Returns an array indicating which languages are supported by the provider. AD FS uses this information
         /// to determine the best language\locale to display to the user.
         public int[] AvailableLcids
         {
         get
         {
         return new[] { new CultureInfo("en-us").LCID, new CultureInfo("fr").LCID};
         }
         }

         /// Returns a Dictionary containing the set of localized friendly names of the provider, indexed by lcid. 
         /// These Friendly Names are displayed in the "choice page" offered to the user when there is more than 
         /// one secondary authentication provider available.
         public Dictionary<int, string> FriendlyNames
         {
         get
         {
         Dictionary<int, string> _friendlyNames = new Dictionary<int, string>();
         _friendlyNames.Add(new CultureInfo("en-us").LCID, "Friendly name of My Example MFA Adapter for end users (en)");
         _friendlyNames.Add(new CultureInfo("fr").LCID, "Friendly name translated to fr locale");
         return _friendlyNames;
         }
         }

         /// Returns a Dictionary containing the set of localized descriptions (hover over help) of the provider, indexed by lcid. 
         /// These descriptions are displayed in the "choice page" offered to the user when there is more than one 
         /// secondary authentication provider available.
         public Dictionary<int, string> Descriptions
         {
         get 
         {
         Dictionary<int, string> _descriptions = new Dictionary<int, string>();
         _descriptions.Add(new CultureInfo("en-us").LCID, "Description of My Example MFA Adapter for end users (en)");
         _descriptions.Add(new CultureInfo("fr").LCID, "Description translated to fr locale");
         return _descriptions; 
         }
         }

         /// Returns an array indicating the type of claim that the adapter uses to identify the user being authenticated.
         /// Note that although the property is an array, only the first element is currently used.
         /// MUST BE ONE OF THE FOLLOWING
         /// "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"
         /// "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
         /// "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"
         /// "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid"
         public string[] IdentityClaims
         {
         get { return new[] { "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" }; }
         }

         //All external providers must return a value of "true" for this property.
         public bool RequiresIdentity
         {
         get { return true; }
         }
        }

    Ensuite, le formulaire de présentation :

        class MyPresentationForm : IAdapterPresentationForm
         {
         /// Returns the HTML Form fragment that contains the adapter user interface. This data will be included in the web page that is presented
         /// to the cient.
         public string GetFormHtml(int lcid)
         {
         string htmlTemplate = Resources.FormPageHtml; //todo we will implement this
         return htmlTemplate;
         }

         /// Return any external resources, ie references to libraries etc., that should be included in 
         /// the HEAD section of the presentation form html. 
         public string GetFormPreRenderHtml(int lcid)
         {
         return null;
         }

         //returns the title string for the web page which presents the HTML form content to the end user
         public string GetPageTitle(int lcid)
         {
         return "MFA Adapter";
         }


~~~
     }
~~~

12. Notez l’élément « TODO » pour l’élément **Resources. FormPageHtml** ci-dessus. 

   Vous pouvez la corriger dans une minute, mais nous allons tout d’abord ajouter les instructions Return requises finales, basées sur les types nouvellement implémentés, à votre classe MyAdapter initiale.  Pour ce faire, ajoutez les éléments en *italique* ci-dessous à votre implémentation IAuthenticationAdapter existante :

       MyAdapter de classe : IAuthenticationAdapter {public IAuthenticationAdapterMetadata Metadata {//get {return <instance of IAuthenticationAdapterMetadata derived class>New ;}     obtenir {Return New MyMetadata ();}     }

        public IAdapterPresentation BeginAuthentication(Claim identityClaim, HttpListenerRequest request, IAuthenticationContext authContext)
        {
        //return new instance of IAdapterPresentationForm derived class
        return new MyPresentationForm();
        }

        public bool IsAvailableForUser(Claim identityClaim, IAuthenticationContext authContext)
        {
        return true; //its all available for now
        }

        public void OnAuthenticationPipelineLoad(IAuthenticationMethodConfigData configData)
        {
        //this is where AD FS passes us the config data, if such data was supplied at registration of the adapter

        }

        public void OnAuthenticationPipelineUnload()
        {

        }

        public IAdapterPresentation OnError(HttpListenerRequest request, ExternalAuthenticationException ex)
        {
        //return new instance of IAdapterPresentationForm derived class
        return new MyPresentationForm();
        }

        public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
        {
        //return new instance of IAdapterPresentationForm derived class
        outgoingClaims = new Claim[0];
        return new MyPresentationForm();
        }

        }

13. Maintenant, pour le fichier de ressources contenant le fragment HTML. Créez un nouveau fichier texte dans votre dossier de projet avec le contenu suivant :

       <div id="loginArea">
        <form method="post" id="loginForm" >
        <!-- These inputs are required by the presentation framework. Do not modify or remove -->
        <input id="authMethod" type="hidden" name="AuthMethod" value="%AuthMethod%"/>
        <input id="context" type="hidden" name="Context" value="%Context%"/>
        <!-- End inputs are required by the presentation framework. -->
        <p id="pageIntroductionText">Ce contenu est fourni par l’exemple d’adaptateur MFA. Les entrées de stimulation doivent être présentées ci-dessous.</p>
        <label for="challengeQuestionInput" class="block">Question texte @ no__t-1 @ no__t-2 @ no__t-3<div id="submissionArea" class="submitMargin">
        <input id="submitButton" type="submit" name="Submit" value="Submit" onclick="return AuthPage.submitAnswer()"/>
        </div>
        </form>
        <div id="intro" class="groupMargin">
        <p id="supportEmail">Informations concernant le support</p>
        </div>
        <script type="text/javascript" language="JavaScript">
        //<![CDATA[
        function AuthPage() { }
        AuthPage.submitAnswer = function () { return true; };
        //]]>
        </script></div>

14. Puis, sélectionnez **projet-\>ajouter un composant... Fichier** de ressources et nommez le fichier **ressources**, puis cliquez sur **Ajouter :**

   ![créer le fournisseur](media/ad-fs-build-custom-auth-method/Dn783423.3369ad8f-f65f-4f36-a6d5-6a3edbc1911a(MSDN.10).jpg "créer le fournisseur")

15. Ensuite, dans le fichier **Resources. resx** , choisissez **Ajouter une ressource... Ajoutez un fichier existant**.  Accédez au fichier texte (contenant le fragment HTML) que vous avez enregistré ci-dessus.

   Assurez-vous que votre code GetFormHtml résout correctement le nom de la nouvelle ressource par le préfixe du nom du fichier de ressources (fichier. resx) suivi du nom de la ressource elle-même :

       public String GetFormHtml (int LCID) {String htmlTemplate = Resources. MfaFormHtml ;//resxFilename.ResourceName Return htmlTemplate ;    }

   Vous devez maintenant être en mesure de générer.

## <a name="build-the-adapter"></a>Créer l’adaptateur

L’adaptateur doit être intégré à un assembly .NET portant un nom fort qui peut être installé dans le GAC dans Windows.  Pour y parvenir dans un projet Visual Studio, procédez comme suit :

1.  Cliquez avec le bouton droit sur le nom de votre projet dans Explorateur de solutions, puis cliquez sur **Propriétés**.

2.  Sous l’onglet **signature** , activez la case à cocher **signer l’assembly** , puis choisissez **\<nouveau... sous\>** **choisir un fichier de clé de nom fort :**  Entrez un nom de fichier de clé et un mot de passe, puis cliquez sur **OK**.  Assurez-vous ensuite que **l’option signer l’assembly** est cochée et **que la signature différée uniquement** est désactivée.  La page **signature** des propriétés doit se présenter comme suit :

    ![générer le fournisseur](media/ad-fs-build-custom-auth-method/Dn783423.0b1a1db2-d64e-4bb8-8c01-ef34296a2668(MSDN.10).jpg "générer le fournisseur")

3.  Générez ensuite la solution.

## <a name="deploy-the-adapter-to-your-ad-fs-test-machine"></a>Déployer l’adaptateur sur votre ordinateur de test AD FS

Pour qu’un fournisseur externe puisse être appelé par AD FS, il doit être inscrit dans le système.  Les fournisseurs d’adaptateur doivent fournir un programme d’installation qui exécute les actions d’installation nécessaires, y compris l’installation dans le GAC, et le programme d’installation doit prendre en charge l’inscription dans AD FS.  Si cela n’est pas fait, l’administrateur doit exécuter les étapes Windows PowerShell ci-dessous.  Ces étapes peuvent être utilisées dans le laboratoire pour activer le test et le débogage.

### <a name="prepare-the-test-ad-fs-machine"></a>Préparer l’ordinateur de test AD FS

Copiez les fichiers et ajoutez-les au GAC.

1.  Vérifiez que vous disposez d’un ordinateur Windows Server 2012 R2 ou d’une machine virtuelle.

2.  Installez le service de rôle AD FS et configurez une batterie de serveurs avec au moins un nœud.

    Pour obtenir des instructions détaillées sur la configuration d’un serveur de Fédération dans un environnement de laboratoire, consultez le [Guide de déploiement de Windows server 2012 R2 AD FS](https://msdn.microsoft.com/library/dn486820\(v=msdn.10\)).

3.  Copiez les outils Gacutil. exe sur le serveur.

    Gacutil. exe se trouve dans **% HomeDrive% \\Program Files (x86) \\Microsoft SDK @ no__t-3Windows @ no__t-4V 8.0 a @ no__t-5bin @ no__t-6NETFX 4,0 Tools @ no__t-7** sur un ordinateur Windows 8.  Vous aurez besoin du fichier **Gacutil. exe** proprement dit, ainsi que des **1033**, en **-US**et des autres dossiers de ressources localisés sous l’emplacement des **Outils netfx 4,0** .

4.  Copiez vos fichiers de fournisseur (un ou plusieurs fichiers. dll signés avec un nom fort) dans le même emplacement de dossier que **Gacutil. exe** (l’emplacement est juste pour des raisons pratiques)

5.  Ajoutez vos fichiers. dll au GAC sur chaque AD FS serveur de Fédération de la batterie de serveurs :

    Exemple : utilisation de l’outil de ligne de commande GACutil. exe pour ajouter une dll au GAC :`C:\>.\gacutil.exe /if .\<yourdllname>.dll`

    Pour afficher l’entrée résultante dans le GAC :`C:\>.\gacutil.exe /l <yourassemblyname>`

6.  

### <a name="register-your-provider-in-ad-fs"></a>Inscrire votre fournisseur dans AD FS

Une fois que les conditions préalables ci-dessus sont remplies, ouvrez une fenêtre de commande Windows PowerShell sur votre serveur de Fédération et entrez les commandes suivantes (Notez que si vous utilisez une batterie de serveurs de Fédération qui utilise la base de données interne Windows, vous devez exécuter ces commandes sur le serveur de Fédération principal de la batterie de serveurs :

1.  `Register-AdfsAuthenticationProvider –TypeName YourTypeName –Name “AnyNameYouWish” [–ConfigurationFilePath (optional)]`

    Où YourTypeName est votre nom de type fort .NET : « YourDefaultNamespace. YourIAuthenticationAdapterImplementationClassName, YourAssemblyName, version = YourAssemblyVersion, culture = neutral, PublicKeyToken = YourPublicKeyTokenValue, processorArchitecture = MSIL »

    Votre fournisseur externe est inscrit dans AD FS, avec le nom que vous avez fourni comme AnyNameYouWish ci-dessus.

2.  Redémarrez le service AD FS (à l’aide du composant logiciel enfichable Services Windows, par exemple).

3.  Exécutez la commande suivante: `Get-AdfsAuthenticationProvider`.

    Cela montre que votre fournisseur est l’un des fournisseurs dans le système.

    Exemple :

        PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”
        PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter”
        PS C:\>net stop adfssrv
        PS C:\>net start adfssrv

    Si vous avez activé le service Device Registration dans votre environnement AD FS, exécutez également la commande suivante :`PS C:\>net start drs`

    Pour vérifier le fournisseur inscrit, utilisez la commande suivante :`PS C:\>Get-AdfsAuthenticationProvider`.

    Cela montre que votre fournisseur est l’un des fournisseurs dans le système.

### <a name="create-the-ad-fs-authentication-policy-that-invokes-your-adapter"></a>Créer la stratégie d’authentification AD FS qui appelle votre adaptateur

#### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>Créer la stratégie d’authentification à l’aide du composant logiciel enfichable Gestion des AD FS

1.  Ouvrez le composant logiciel enfichable Gestion des AD FS (à partir du menu **outils** gestionnaire de serveur).

2.  Cliquez sur **stratégies d’authentification**.

3.  Dans le volet central, sous **Multi-Factor Authentication**, cliquez sur le lien **modifier** à droite de **paramètres globaux**.

4.  Sous **Sélectionner des méthodes d’authentification supplémentaires** en bas de la page, activez la case à cocher correspondant à la adminname de votre fournisseur. Cliquez sur **Appliquer**.

5.  Pour fournir un « déclencheur » pour appeler MFA à l’aide de votre adaptateur, dans **emplacements** , vérifiez à la fois **extranet** et **Intranet**, par exemple. Cliquez sur **OK**. (Pour configurer les déclencheurs par partie de confiance, consultez la section « créer la stratégie d’authentification à l’aide de Windows PowerShell » ci-dessous.)

6.  Vérifiez les résultats à l’aide des commandes suivantes :

    Première utilisation `Get-AdfsGlobalAuthenticationPolicy`. Vous devez voir le nom de votre fournisseur comme l’une des valeurs AdditionalAuthenticationProvider.

    Utilisez `Get-AdfsAdditionalAuthenticationRule`ensuite. Vous devez voir les règles pour l’extranet et l’intranet configurées à la suite de votre sélection de stratégie dans l’interface utilisateur de l’administrateur.

#### <a name="create-the-authentication-policy-using-windows-powershell"></a>Créer la stratégie d’authentification à l’aide de Windows PowerShell

1.  Tout d’abord, activez le fournisseur dans la stratégie globale :

    `PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider “YourAuthProviderName”`


~~~
> [!NOTE]
> Note that the value provided for the AdditionalAuthenticationProvider parameter corresponds to the value you provided for the “Name” parameter in the Register-AdfsAuthenticationProvider cmdlet above and to the “Name” property from Get-AdfsAuthenticationProvider cmdlet output. 
> <P></P>


Example:`PS C:\>Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider “MyMFAAdapter”`
~~~

2. Ensuite, configurez des règles globales ou spécifiques à la partie de confiance pour déclencher l’authentification multifacteur :

   Exemple 1 : pour créer une règle globale pour exiger l’authentification MFA pour les demandes externes :`PS C:\>Set-AdfsAdditionalAuthenticationRule –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'`

   Exemple 2 : pour créer des règles MFA pour exiger l’authentification MFA pour des demandes externes adressées à une partie de confiance spécifique.  (Notez que les fournisseurs individuels ne peuvent pas être connectés à des parties de confiance individuelles dans AD FS dans Windows Server 2012 R2).

       PS C:\>$rp = Get-AdfsRelyingPartyTrust –Name <Relying Party Name>
       PS C:\>Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'

### <a name="authenticate-with-mfa-using-your-adapter"></a>S’authentifier avec MFA à l’aide de votre adaptateur

Enfin, effectuez les étapes ci-dessous pour tester votre adaptateur :

1.  Assurez-vous que le type d’authentification principale globale AD FS est configuré en tant qu’authentification par formulaire pour l’extranet et l’intranet (cela rend votre démonstration plus facile à authentifier en tant qu’utilisateur spécifique).

    1.  Dans le composant logiciel enfichable AD FS, sous **stratégies d’authentification**, dans la zone **authentification principale** , cliquez sur **modifier** en regard de **paramètres globaux**.

        1.  Ou cliquez simplement sur l’onglet **principal** dans l’interface utilisateur de la **stratégie multi-Factor** .

2.  Assurez-vous que l' **authentification par formulaire** est la seule option activée pour l’extranet et la méthode d’authentification intranet.  Cliquez sur **OK**.

3.  Ouvrez la page HTML de connexion initiée par IDP (\<https://\>FSName/ADFS/LS/idpinitiatedsignon.htm) et connectez-vous en tant qu’utilisateur Active Directory valide dans votre environnement de test.

4.  Entrez les informations d’identification pour l’authentification principale.

5.  Vous devez voir s’afficher la page Formulaires MFA avec des exemples de questions de demande. 

    Si plusieurs adaptateurs sont configurés, la page de choix MFA s’affiche avec votre nom convivial ci-dessus.

    ![s’authentifier avec l’adaptateur](media/ad-fs-build-custom-auth-method/Dn783423.c98d2712-cbd3-4cb9-ac03-2838b81c4f63(MSDN.10).jpg "s’authentifier avec l’adaptateur")

    ![s’authentifier avec l’adaptateur](media/ad-fs-build-custom-auth-method/Dn783423.fd3aefc0-ef6c-4a8c-a737-4914c78ff2d2(MSDN.10).jpg "s’authentifier avec l’adaptateur")

Vous disposez maintenant d’une implémentation opérationnelle de l’interface et vous savez comment le modèle fonctionne. Vous pouvez Trym comme exemple supplémentaire pour définir des points d’arrêt dans le BeginAuthentication, ainsi que dans le TryEndAuthentication.  Notez la manière dont BeginAuthentication est exécuté lorsque l’utilisateur entre pour la première fois dans le formulaire MFA, tandis que TryEndAuthentication est déclenché à chaque envoi du formulaire.

## <a name="update-the-adapter-for-successful-authentication"></a>Mettre à jour l’adaptateur pour une authentification réussie

Mais Wait : votre exemple d’adaptateur ne s’authentifiera jamais correctement\!  Cela est dû au fait que rien dans votre code ne retourne null pour TryEndAuthentication.

En suivant les procédures ci-dessus, vous avez créé une implémentation de l’adaptateur de base et vous l’avez ajoutée à un serveur de AD FS.  Vous pouvez obtenir la page de formulaires MFA, mais vous ne pouvez pas encore l’authentifier, car vous n’avez pas encore placé la logique correcte dans votre implémentation de TryEndAuthentication.  Nous allons donc l’ajouter.

Souvenez-vous de votre implémentation TryEndAuthentication :

    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
     {
     //return new instance of IAdapterPresentationForm derived class
     outgoingClaims = new Claim[0];
     return new MyPresentationForm();

     }

Nous allons la mettre à jour pour qu’elle ne retourne pas toujours MyPresentationForm ().  Pour cela, vous pouvez créer une méthode utilitaire simple dans votre classe :

    static bool ValidateProofData(IProofData proofData, IAuthenticationContext authContext)
     {
     if (proofData == null || proofData.Properties == null || !proofData.Properties.ContainsKey("ChallengeQuestionAnswer"))
     {
     throw new ExternalAuthenticationException("Error - no answer found", authContext);
     }

     if ((string)proofData.Properties["ChallengeQuestionAnswer"] == "adfabric")
     {
     return true;
     }
     else
     {
     return false;
     }
     }

Ensuite, mettez à jour TryEndAuthentication comme indiqué ci-dessous :

    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
     {
     outgoingClaims = new Claim[0];
     if (ValidateProofData(proofData, authContext))
     {
     //authn complete - return authn method
     outgoingClaims = new[] 
     {
     // Return the required authentication method claim, indicating the particulate authentication method used.
     new Claim( "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", 
     "http://example.com/myauthenticationmethod1" )
     };
     return null;
     }
     else
     {
     //authentication not complete - return new instance of IAdapterPresentationForm derived class
     return new MyPresentationForm();
     }
     }

À présent, vous devez mettre à jour l’adaptateur sur la boîte de test.  Vous devez d’abord annuler la stratégie de AD FS, puis annuler l’inscription à partir d’AD FS et redémarrer AD FS, supprimer le fichier. dll du GAC, puis ajouter le nouveau fichier. dll au GAC, puis l’inscrire dans AD FS, redémarrer AD FS et reconfigurer AD FS stratégie.

## <a name="deploy-and-configure-the-updated-adapter-on-your-test-ad-fs-machine"></a>Déployer et configurer l’adaptateur mis à jour sur votre ordinateur de AD FS de test

### <a name="clear-ad-fs-policy"></a>Désactiver la stratégie de AD FS

Désactivez toutes les cases à cocher MFA associées dans l’interface utilisateur MFA, comme indiqué ci-dessous, puis cliquez sur OK.

![Supprimer la stratégie](media/ad-fs-build-custom-auth-method/Dn783423.c111b4e7-5b05-413c-8b0f-222a0e91ac1f(MSDN.10).jpg "Supprimer la stratégie")

### <a name="unregister-provider-windows-powershell"></a>Annuler l’inscription du fournisseur (Windows PowerShell)

`PS C:\> Unregister-AdfsAuthenticationProvider –Name “YourAuthProviderName”`

Tels`PS C:\> Unregister-AdfsAuthenticationProvider –Name “MyMFAAdapter”`

Notez que la valeur que vous transmettez pour « nom » est identique à la valeur « nom » que vous avez fournie à l’applet de commande Register-Adfsauthenticationprovider,.  C’est également la propriété « Name » qui est sortie de la Adfsauthenticationprovider,.

Notez qu’avant d’annuler l’inscription d’un fournisseur, vous devez supprimer le fournisseur du AdfsGlobalAuthenticationPolicy (en désactivant les cases à cocher que vous avez activées dans AD FS composant logiciel enfichable de gestion ou à l’aide de Windows PowerShell.)

Notez que le service de AD FS doit être redémarré après cette opération.

### <a name="remove-assembly-from-gac"></a>Supprimer l’assembly du GAC

1.  Tout d’abord, utilisez la commande suivante pour rechercher le nom fort complet de l’entrée :`C:\>.\gacutil.exe /l <yourAdapterAssemblyName>`

    Tels`C:\>.\gacutil.exe /l mfaadapter`

2.  Ensuite, utilisez la commande suivante pour le supprimer du GAC :`.\gacutil /u “<output from the above command>”`

    Tels`C:\>.\gacutil /u “mfaadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

### <a name="add-the-updated-assembly-to-gac"></a>Ajouter l’assembly mis à jour au GAC

Veillez à coller le fichier. dll mis à jour en premier. `C:\>.\gacutil.exe /if .\MFAAdapter.dll`

### <a name="view-assembly-in-the-gac-cmd-line"></a>Afficher l’assembly dans le GAC (ligne de commande)

`C:\> .\gacutil.exe /l mfaadapter`

### <a name="register-your-provider-in-ad-fs"></a>Inscrire votre fournisseur dans AD FS

1.  `PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.1, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

2.  `PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter1”`

3.  Redémarrez le service AD FS.

### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>Créer la stratégie d’authentification à l’aide du composant logiciel enfichable Gestion des AD FS

1.  Ouvrez le composant logiciel enfichable Gestion des AD FS (à partir du menu **outils** gestionnaire de serveur).

2.  Cliquez sur **stratégies d’authentification**.

3.  Sous **Multi-Factor Authentication**, cliquez sur le lien **modifier** à droite de **paramètres globaux**.

4.  Sous **Sélectionner des méthodes d’authentification supplémentaires**, activez la case à cocher correspondant à la adminname de votre fournisseur. Cliquez sur **Appliquer**.

5.  Pour fournir un « déclencheur » pour appeler MFA à l’aide de votre adaptateur, dans emplacements, vérifiez à la fois **extranet** et **Intranet**, par exemple. Cliquez sur **OK**.

### <a name="authenticate-with-mfa-using-your-adapter"></a>S’authentifier avec MFA à l’aide de votre adaptateur

Enfin, effectuez les étapes ci-dessous pour tester votre adaptateur :

1.  Assurez-vous que le type d’authentification principale globale AD FS est configuré en tant qu' **authentification par formulaire pour l'** extranet et l’intranet (cela facilite l’authentification en tant qu’utilisateur spécifique).

    1.  Dans le composant logiciel enfichable Gestion des AD FS, sous **stratégies d’authentification**, dans la zone **authentification principale** , cliquez sur **modifier** en regard de **paramètres globaux**.

        1.  Ou cliquez simplement sur l’onglet **principal** dans l’interface utilisateur de la stratégie multi-Factor.

2.  Assurez-vous que l' **authentification par formulaire** est la seule option activée pour l' **extranet** et la méthode d’authentification **Intranet** .  Cliquez sur **OK**.

3.  Ouvrez la page HTML de connexion initiée par IDP (\<https://\>FSName/ADFS/LS/idpinitiatedsignon.htm) et connectez-vous en tant qu’utilisateur Active Directory valide dans votre environnement de test.

4.  Entrez les informations d’identification pour l’authentification principale.

5.  Vous devez voir s’afficher la page Formulaires MFA avec un exemple de texte de stimulation.

    1.  Si vous avez plusieurs adaptateurs configurés, la page de choix MFA s’affiche avec votre nom convivial.

Vous devez voir une connexion réussie lors de l’entrée de « adfabric » sur la page d’authentification MFA.

![se connecter avec l’adaptateur](media/ad-fs-build-custom-auth-method/Dn783423.630d8a91-3bfe-4cba-8acf-03eae21530ee(MSDN.10).jpg "se connecter avec l’adaptateur")

![se connecter avec l’adaptateur](media/ad-fs-build-custom-auth-method/Dn783423.c340fa73-f70f-4870-b8dd-07900fea4469(MSDN.10).jpg "se connecter avec l’adaptateur")

## <a name="see-also"></a>Voir aussi

#### <a name="other-resources"></a>Autres ressources

[Méthodes d’authentification supplémentaires](https://msdn.microsoft.com/library/dn758113\(v=msdn.10\))  
[Gérer les risques avec une authentification multifacteur supplémentaire pour les applications sensibles](https://msdn.microsoft.com/library/dn280949\(v=msdn.10\))

