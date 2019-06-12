---
title: Créer une méthode d’authentification personnalisée pour AD FS dans Windows Server
description: Ce scénario décrit comment créer une méthode d’authentification personnalisée pour AD FS dans Windows Server.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/23/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f28458ed9e781df6eca2478b02fb667d9240ca48
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445305"
---
# <a name="build-a-custom-authentication-method-for-ad-fs-in-windows-server"></a>Créer une méthode d’authentification personnalisée pour AD FS dans Windows Server

Cette procédure pas à pas fournit des instructions pour l’implémentation d’une méthode d’authentification personnalisée pour AD FS dans Windows Server 2012 R2. Pour plus d’informations, consultez [méthodes d’authentification supplémentaires](https://msdn.microsoft.com/en-us/library/dn758113\(v=msdn.10\)).


> [!WARNING]
> L’exemple que vous pouvez générer ici est&nbsp;des fins pédagogiques uniquement. &nbsp;Ces instructions concernent l’implémentation la plus simple, plus minimale possible d’exposer les éléments requis du modèle.&nbsp; Il n’existe aucun serveur principal de l’authentification, traitement des erreurs ou les données de configuration. 
> <P></P>



## <a name="setting-up-the-development-box"></a>Configuration de la boîte de développement

Cette procédure pas à pas utilise Visual Studio 2012.  Le projet peut être généré à l’aide de n’importe quel environnement de développement qui peut créer une classe .NET pour Windows. Le projet doit cibler .NET 4.5, car le **BeginAuthentication** et **TryEndAuthentication** méthodes utilisent le type **System.Security.Claims.Claim**, qui fait partie de .NET 4.5.There de version de Framework est une référence est requise pour le projet :


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Dll de référence</strong></p></td>
<td><p><strong>Où les trouver</strong></p></td>
<td><p><strong>Requis pour</strong></p></td>
</tr>
<tr class="even">
<td><p>Microsoft.IdentityServer.Web.dll</p></td>
<td><p>La dll se trouve dans %windir%\ADFS sur un serveur Windows Server 2012 R2 sur lequel AD FS a été installé.</p>
<p></p>
<p>Cette dll doit être copiée vers l’ordinateur de développement et une référence explicite créés dans le projet.</p></td>
<td><p>Types d’interface, y compris IAuthenticationContext, IProofData</p></td>
</tr>
</tbody>
</table>


## <a name="create-the-provider"></a>Créer le fournisseur

1.  Dans Visual Studio 2012 : Sélectionnez fichier -\>New -\>projet...

2.  Sélectionnez la bibliothèque de classes et veillez à ce que vous ciblez le .NET 4.5.

    ![créer le fournisseur](media/ad-fs-build-custom-auth-method/Dn783423.71a57ae1-d53d-462b-a846-5b3c02c7d3f2(MSDN.10).jpg "créer le fournisseur")

3.  Faites une copie de **Microsoft.IdentityServer.Web.dll** à partir de % windir%\\ADFS sur le serveur Windows Server 2012 R2 où AD FS a été installé et collez-le dans votre dossier de projet sur votre ordinateur de développement.

4.  Dans **l’Explorateur de solutions**, avec le bouton droit cliquez sur **références** et **ajouter une référence...**

5.  Accédez à votre copie locale du **Microsoft.IdentityServer.Web.dll** et **ajouter...**

6.  Cliquez sur **OK** pour confirmer la nouvelle référence :

    ![créer le fournisseur](media/ad-fs-build-custom-auth-method/Dn783423.f18df353-9259-4744-b4b6-dd780ce90951(MSDN.10).jpg "créer le fournisseur")

    Vous devez maintenant être configuré pour résoudre tous les types requis pour le fournisseur. 

7.  Ajoutez une nouvelle classe à votre projet (cliquez avec le bouton droit sur votre projet, **ajouter... Classe...** ) et donnez-lui un nom tel que **MyAdapter**, comme indiqué ci-dessous :

    ![créer le fournisseur](media/ad-fs-build-custom-auth-method/Dn783423.6b6a7a8b-9d66-40c7-8a86-a2e3b9e14d09(MSDN.10).jpg "créer le fournisseur")

8.  Dans le nouveau fichier MyAdapter.cs, remplacez le code existant par le code suivant :

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

    Vous devez désormais être en mesure de F12 (clic droit : atteindre la définition) sur IAuthenticationAdapter pour afficher l’ensemble des membres d’interface requis. 

    Ensuite, vous pouvez effectuer une implémentation simple de ceux-ci.

9.  Remplacez tout le contenu de votre classe avec les éléments suivants :

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

10. Nous ne sommes pas prêts à créer encore... Il existe deux interfaces plus à accéder.

    Ajoutez deux classes à votre projet : l’un concerne les métadonnées et l’autre pour le formulaire de présentation.  Vous pouvez ajouter ces dans le même fichier en tant que la classe ci-dessus.

        class MyMetadata : IAuthenticationAdapterMetadata
         {

         }

         class MyPresentationForm : IAdapterPresentationForm
         {

         }

11. Ensuite, vous pouvez ajouter les membres requis pour chacun. Tout d’abord, les métadonnées (avec les commentaires utiles inline)

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

         /// Returns an array indicating the type of claim that that the adapter uses to identify the user being authenticated.
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

12. Notez que le « todo » pour le **Resources.FormPageHtml** élément ci-dessus. 

   Vous pouvez y remédier dans une minute, mais le premier nous allons ajouter les instructions return requises finales, basées sur les types qui vient d’être implémentés, à votre classe MyAdapter initiale.  Pour ce faire, ajoutez les éléments de *italique* ci-dessous à votre implémentation IAuthenticationAdapter existante :

       classe MyAdapter : IAuthenticationAdapter     {     public IAuthenticationAdapterMetadata Metadata     {     //get { return new <instance of IAuthenticationAdapterMetadata derived class>; }     get { return new MyMetadata(); }     }

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

13. Maintenant, pour le fichier de ressources contenant le fragment html. Créer un nouveau fichier texte dans votre dossier de projet avec le contenu suivant :

       <div id="loginArea">
        <form method="post" id="loginForm" >
        <!-- These inputs are required by the presentation framework. Do not modify or remove -->
        <input id="authMethod" type="hidden" name="AuthMethod" value="%AuthMethod%"/>
        <input id="context" type="hidden" name="Context" value="%Context%"/>
        <!-- End inputs are required by the presentation framework. -->
        <p id="pageIntroductionText">Ce contenu est fourni par l’exemple d’adaptateur MFA. Entrées de demande doivent être présentées ci-dessous.</p>
        <label for="challengeQuestionInput" class="block">Texte de la question</label>
        <input id="challengeQuestionInput" name="ChallengeQuestionAnswer" type="text" value="" class="text" placeholder="Answer placeholder" />
        <div id="submissionArea" class="submitMargin">
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

14. Ensuite, sélectionnez **projet -\>ajouter un composant... Ressources** fichier et nommez le fichier **ressources**, puis cliquez sur **ajouter :**

   ![créer le fournisseur](media/ad-fs-build-custom-auth-method/Dn783423.3369ad8f-f65f-4f36-a6d5-6a3edbc1911a(MSDN.10).jpg "créer le fournisseur")

15. Ensuite, dans le **Resources.resx** de fichiers, choisissez **ajouter une ressource... Ajouter un fichier existant**.  Accédez au fichier texte (contenant le fragment html) que vous avez enregistré plus tôt.

   Vérifiez que votre code GetFormHtml résout correctement le nom de la nouvelle ressource par le préfixe de nom de fichier (.resx) ressources suivi du nom de la ressource elle-même :

       public string GetFormHtml(int lcid)    {     string htmlTemplate = Resources.MfaFormHtml; //Resxfilename.resourcename     return htmlTemplate;    }

   Vous devez maintenant être en mesure de générer.

## <a name="build-the-adapter"></a>Générez l’adaptateur

L’adaptateur doit être créé dans un assembly .NET de nom fort qui peut être installé dans le GAC dans Windows.  Pour ce faire, dans un projet Visual Studio, procédez comme suit :

1.  Cliquez avec le bouton droit sur le nom de votre projet dans l’Explorateur de solutions et cliquez sur **propriétés**.

2.  Sur le **signature** onglet, vérification **signer l’assembly** et choisissez **\<nouveau... \>** sous **choisir un fichier de clé de nom fort :**  Entrez un nom de fichier de clé et le mot de passe et cliquez sur **OK**.  Puis vérifiez **signer l’assembly** est vérifiée et **différer la signature uniquement** est désactivée.  Les propriétés **signature** page doit ressembler à ceci :

    ![créer le fournisseur](media/ad-fs-build-custom-auth-method/Dn783423.0b1a1db2-d64e-4bb8-8c01-ef34296a2668(MSDN.10).jpg "créer le fournisseur")

3.  Puis générez la solution.

## <a name="deploy-the-adapter-to-your-ad-fs-test-machine"></a>Déployer l’adaptateur sur votre ordinateur de test AD FS

Avant d’un fournisseur externe peut être appelé par AD FS, il doit être inscrit dans le système.  Fournisseurs de l’adaptateur doivent fournir un programme d’installation qui effectue les actions d’installation nécessaires y compris l’installation dans le GAC, et le programme d’installation doit prendre en charge l’inscription dans AD FS.  Si cela n’est pas fait, l’administrateur doit exécuter les étapes de Windows PowerShell ci-dessous.  Ces étapes peuvent être utilisés dans le laboratoire afin de test et de débogage.

### <a name="prepare-the-test-ad-fs-machine"></a>Préparer l’ordinateur de test AD FS

Copier les fichiers et ajouter au GAC.

1.  Assurez-vous de qu'avoir un ordinateur Windows Server 2012 R2 ou une machine virtuelle.

2.  Installez le service de rôle AD FS et configurer une batterie de serveurs avec au moins un nœud.

    Pour obtenir des instructions détaillées configurer un serveur de fédération dans un environnement lab, consultez le [Guide déploiement de Windows Server 2012 R2 AD FS](https://msdn.microsoft.com/en-us/library/dn486820\(v=msdn.10\)).

3.  Copiez les outils Gacutil.exe sur le serveur.

    Vous trouverez Gacutil.exe dans **%HOMEDRIVE%\\Program Files (x86)\\Microsoft SDKs\\Windows\\v8.0A\\bin\\NETFX 4.0 outils\\** sur un ordinateur Windows 8.  Vous devez le **gacutil.exe** fichier lui-même, ainsi que les **1033**, **en-US**et le dossier de ressource localisée ci-dessous le **NETFX 4.0 outils** emplacement.

4.  Copiez vos fichiers de fournisseur (un ou plusieurs nom fort signés fichiers .dll) au même emplacement de dossier en tant que **gacutil.exe** (l’emplacement est simplement pour des raisons pratiques)

5.  Ajoutez vos fichiers .dll dans le GAC sur chaque serveur de fédération AD FS dans la batterie de serveurs :

    Exemple : à l’aide de l’outil de ligne de commande GACutil.exe pour ajouter une dll dans le GAC : `C:\>.\gacutil.exe /if .\<yourdllname>.dll`

    Pour afficher l’entrée qui en résulte dans le GAC :`C:\>.\gacutil.exe /l <yourassemblyname>`

6.  

### <a name="register-your-provider-in-ad-fs"></a>Inscrire votre fournisseur dans AD FS

Une fois que les conditions préalables ci-dessus sont remplies, ouvrez une fenêtre de commande Windows PowerShell sur votre serveur de fédération et entrez les commandes suivantes (Notez que si vous utilisez une batterie de serveurs de fédération qui utilise la base de données interne Windows, vous devez exécuter ces commandes sur le serveur de fédération principal de la batterie de serveurs) :

1.  `Register-AdfsAuthenticationProvider –TypeName YourTypeName –Name “AnyNameYouWish” [–ConfigurationFilePath (optional)]`

    Où YourTypeName est votre nom de type fort .NET : « YourDefaultNamespace.YourIAuthenticationAdapterImplementationClassName, Nom_votre_assembly, Version = YourAssemblyVersion, Culture = neutral, PublicKeyToken = YourPublicKeyTokenValue, processorArchitecture = MSIL »

    Cela inscrira votre fournisseur externe dans AD FS, avec le nom que vous avez fourni comme AnyNameYouWish ci-dessus.

2.  Redémarrez le service AD FS (en utilisant le composant logiciel enfichable Services de Windows, par exemple).

3.  Exécutez la commande suivante : `Get-AdfsAuthenticationProvider`.

    Voici votre fournisseur en tant qu’un des fournisseurs dans le système.

    Exemple :

        PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”
        PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter”
        PS C:\>net stop adfssrv
        PS C:\>net start adfssrv

    Si vous avez activé dans votre environnement AD FS device registration service, exécutez également la commande suivante :  `PS C:\>net start drs`

    Pour vérifier le fournisseur enregistré, utilisez la commande suivante :`PS C:\>Get-AdfsAuthenticationProvider`.

    Voici votre fournisseur en tant qu’un des fournisseurs dans le système.

### <a name="create-the-ad-fs-authentication-policy-that-invokes-your-adapter"></a>Créer la stratégie d’authentification AD FS qui appelle votre adaptateur

#### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>Créer la stratégie d’authentification à l’aide du composant logiciel enfichable Gestion AD FS

1.  Ouvrez le composant logiciel enfichable Gestion AD FS (à partir du Gestionnaire de serveur **outils** menu).

2.  Cliquez sur **stratégies d’authentification**.

3.  Dans le volet central, sous **multi-Factor Authentication**, cliquez sur le **modifier** lien vers la droite de **paramètres globaux**.

4.  Sous **sélectionner des méthodes d’authentification supplémentaires** en bas de la page, cochez la case pour AdminName de votre fournisseur. Cliquez sur **Appliquer**.

5.  Pour fournir un « déclencheur » pour appeler MFA à l’aide de votre adaptateur, sous **emplacements** cochez à la fois **Extranet** et **Intranet**, par exemple. Cliquez sur **OK**. (Pour configurer les déclencheurs par partie de confiance de tiers, consultez « Créer la stratégie d’authentification à l’aide de Windows PowerShell » ci-dessous.)

6.  Vérifier les résultats à l’aide des commandes suivantes :

    Tout d’abord utiliser `Get-AdfsGlobalAuthenticationPolicy`. Vous devez voir votre nom de fournisseur en tant qu’une des valeurs AdditionalAuthenticationProvider.

    Utilisez ensuite `Get-AdfsAdditionalAuthenticationRule`. Vous devez voir les règles pour l’Extranet et Intranet configuré à la suite de votre sélection de la stratégie dans l’interface utilisateur de l’administrateur.

#### <a name="create-the-authentication-policy-using-windows-powershell"></a>Créer la stratégie d’authentification à l’aide de Windows PowerShell

1.  Tout d’abord activer le fournisseur de stratégie globale :

    `PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider “YourAuthProviderName”`


~~~
> [!NOTE]
> Note that the value provided for the AdditionalAuthenticationProvider parameter corresponds to the value you provided for the “Name” parameter in the Register-AdfsAuthenticationProvider cmdlet above and to the “Name” property from Get-AdfsAuthenticationProvider cmdlet output. 
> <P></P>


Example:`PS C:\>Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider “MyMFAAdapter”`
~~~

2. Ensuite, configurez des règles globales ou partie de confiance-spécifiques aux tiers au déclencheur MFA :

   Exemple 1 : créer une règle globale pour exiger l’authentification Multifacteur pour des demandes externes :`PS C:\>Set-AdfsAdditionalAuthenticationRule –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'`

   Exemple 2 : créer MFA règles pour exiger l’authentification Multifacteur pour les requêtes externes vers une partie de confiance spécifique de tiers.  (Notez que les fournisseurs individuels ne peut pas être connectés à des parties de confiance dans AD FS dans Windows Server 2012 R2).

       PS C:\>$rp = Get-AdfsRelyingPartyTrust –Name <Relying Party Name>
       PS C:\>Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'

### <a name="authenticate-with-mfa-using-your-adapter"></a>L’authentification Multifacteur à l’aide de votre adaptateur

Enfin, effectuez les étapes ci-dessous pour votre adaptateur de test :

1.  Vérifiez le type de l’authentification principale globale d’AD FS est configuré en tant que l’authentification par formulaire pour l’Extranet et Intranet (Cela facilite votre démonstration pour s’authentifier en tant qu’un utilisateur spécifique)

    1.  Dans les services AD FS-composant logiciel enfichable, sous **stratégies d’authentification**, dans le **l’authentification principale** zone, cliquez sur **modifier** regard **paramètres globaux**.

        1.  Ou cliquez simplement sur le **principal** onglet à partir de la **stratégie multifacteur** l’interface utilisateur.

2.  Vérifiez **l’authentification par formulaire** est la seule option activée pour l’Extranet et de la méthode d’authentification Intranet.  Cliquez sur **OK**.

3.  Ouvrir le fournisseur d’identité initié par page html de l’authentification (https://\<fsname\>/adfs/ls/idpinitiatedsignon.htm) et connectez-vous en tant qu’un utilisateur AD valide dans votre environnement de test.

4.  Entrez les informations d’identification pour l’authentification principale.

5.  Vous devez voir les formulaires MFA page avec des exemples de questions défi s’affiche. 

    Si vous avez plusieurs cartes configuré, vous verrez la page de choix d’authentification Multifacteur avec votre nom convivial ci-dessus.

    ![authentifier avec une carte](media/ad-fs-build-custom-auth-method/Dn783423.c98d2712-cbd3-4cb9-ac03-2838b81c4f63(MSDN.10).jpg "authentifier avec une carte")

    ![authentifier avec une carte](media/ad-fs-build-custom-auth-method/Dn783423.fd3aefc0-ef6c-4a8c-a737-4914c78ff2d2(MSDN.10).jpg "authentifier avec une carte")

Vous disposez maintenant d’une implémentation de l’utilisation de l’interface et que vous connaissez le fonctionnement du modèle. Vous pouvez trym comme exemple pour définir des points d’arrêt dans la BeginAuthentication, ainsi que la TryEndAuthentication supplémentaire.  Notez comment BeginAuthentication est exécutée lorsque l’utilisateur entre d’abord dans le formulaire d’authentification Multifacteur, tandis que TryEndAuthentication est déclenchée à chaque soumission du formulaire.

## <a name="update-the-adapter-for-successful-authentication"></a>Mettre à jour de l’adaptateur pour l’authentification réussie

Mais attente – votre adaptateur exemple aboutit jamais authentifiera\!  Il s’agit, car rien dans votre code retourne la valeur null pour TryEndAuthentication.

En effectuant les procédures ci-dessus, vous créé une implémentation de l’adaptateur de base et ajouté à un serveur AD FS.  Vous pouvez obtenir la page de forms MFA, mais vous ne pouvez pas encore authentifiée, car vous n’avez pas encore mis la logique appropriée dans votre implémentation TryEndAuthentication.  Nous allons donc ajouter qui.

Rappelez-vous votre implémentation TryEndAuthentication :

    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
     {
     //return new instance of IAdapterPresentationForm derived class
     outgoingClaims = new Claim[0];
     return new MyPresentationForm();

     }

Nous allons mettre à jour afin qu’il ne renvoie pas toujours MyPresentationForm().  Pour cela, vous pouvez créer une méthode d’utilitaire simple au sein de votre classe :

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

Vous devez à présent mettre à jour de la carte sur la zone de test.  Vous devez tout d’abord annuler la stratégie AD FS, puis annuler l’inscription à partir d’AD FS et redémarrer les services AD FS, puis supprimer le fichier .dll du GAC, puis ajouter la nouvelle DLL dans le GAC, puis inscrivez-le dans AD FS, redémarrer les services AD FS et reconfigurer stratégie AD FS.

## <a name="deploy-and-configure-the-updated-adapter-on-your-test-ad-fs-machine"></a>Déployer et configurer l’adaptateur de mise à jour sur votre machine AD FS de test

### <a name="clear-ad-fs-policy"></a>Désactivez la stratégie d’AD FS

Désactivez MFA tous les liés des cases à cocher dans l’UI MFA, illustré ci-dessous, puis cliquez sur OK.

![Désactivez la stratégie](media/ad-fs-build-custom-auth-method/Dn783423.c111b4e7-5b05-413c-8b0f-222a0e91ac1f(MSDN.10).jpg "stratégie clair")

### <a name="unregister-provider-windows-powershell"></a>Annuler l’inscription du fournisseur (Windows PowerShell)

`PS C:\> Unregister-AdfsAuthenticationProvider –Name “YourAuthProviderName”`

Exemple :`PS C:\> Unregister-AdfsAuthenticationProvider –Name “MyMFAAdapter”`

Notez que la valeur que vous passez pour « Name » est la même valeur que « Nom » que vous avez fourni à l’applet de commande Register-AdfsAuthenticationProvider.  Il est également la propriété « Name » qui est issue de Get-AdfsAuthenticationProvider.

Notez que, avant d’annuler l’inscription d’un fournisseur, vous devez supprimer le fournisseur de la AdfsGlobalAuthenticationPolicy (soit en désactivant les cases à cocher que vous avez activé dans le composant logiciel enfichable Gestion AD FS ou à l’aide de Windows PowerShell.)

Notez que le service AD FS doit être redémarré après cette opération.

### <a name="remove-assembly-from-gac"></a>Supprimer un assembly à partir du GAC

1.  Tout d’abord, utilisez la commande suivante pour rechercher le nom fort qualifié complet de l’entrée :`C:\>.\gacutil.exe /l <yourAdapterAssemblyName>`

    Exemple :`C:\>.\gacutil.exe /l mfaadapter`

2.  Ensuite, utilisez la commande suivante pour le supprimer du Global Assembly Cache :`.\gacutil /u “<output from the above command>”`

    Exemple :`C:\>.\gacutil /u “mfaadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

### <a name="add-the-updated-assembly-to-gac"></a>Ajouter l’assembly mis à jour au GAC

Assurez-vous que vous collez le fichier .dll mis à jour localement tout d’abord. `C:\>.\gacutil.exe /if .\MFAAdapter.dll`

### <a name="view-assembly-in-the-gac-cmd-line"></a>Assemblage de la vue dans le GAC (ligne de commande)

`C:\> .\gacutil.exe /l mfaadapter`

### <a name="register-your-provider-in-ad-fs"></a>Inscrire votre fournisseur dans AD FS

1.  `PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.1, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

2.  `PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter1”`

3.  Redémarrez le service AD FS.

### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>Créer la stratégie d’authentification à l’aide du composant logiciel enfichable Gestion AD FS

1.  Ouvrez le composant logiciel enfichable Gestion AD FS (à partir du Gestionnaire de serveur **outils** menu).

2.  Cliquez sur **stratégies d’authentification**.

3.  Sous **multi-Factor Authentication**, cliquez sur le **modifier** lien vers la droite de **paramètres globaux**.

4.  Sous **sélectionner des méthodes d’authentification supplémentaires**, cochez la case pour AdminName de votre fournisseur. Cliquez sur **Appliquer**.

5.  Pour fournir un « déclencheur » pour appeler MFA à l’aide de votre adaptateur, sous emplacements vérifier **Extranet** et **Intranet**, par exemple. Cliquez sur **OK**.

### <a name="authenticate-with-mfa-using-your-adapter"></a>L’authentification Multifacteur à l’aide de votre adaptateur

Enfin, effectuez les étapes ci-dessous pour votre adaptateur de test :

1.  Vérifiez le type de l’authentification principale globale d’AD FS est configuré en tant que **l’authentification par formulaire** pour l’Extranet et Intranet (Cela rend plus facile pour s’authentifier en tant qu’un utilisateur spécifique).

    1.  Dans la gestion AD FS-composant logiciel enfichable, sous **stratégies d’authentification**, dans le **l’authentification principale** zone, cliquez sur **modifier** regard **paramètres globaux**.

        1.  Ou cliquez simplement sur le **principal** onglet à partir de l’interface utilisateur des stratégies multi-factor.

2.  Vérifiez **l’authentification par formulaire** est la seule option activée pour les deux le **Extranet** et **Intranet** méthode d’authentification.  Cliquez sur **OK**.

3.  Ouvrir le fournisseur d’identité initié par page html de l’authentification (https://\<fsname\>/adfs/ls/idpinitiatedsignon.htm) et connectez-vous en tant qu’un utilisateur AD valide dans votre environnement de test.

4.  Entrez les informations d’identification pour l’authentification principale.

5.  Vous devez voir les formulaires MFA page avec le texte d’exemple défi s’affiche.

    1.  Si vous avez plusieurs cartes configuré, vous verrez la page de choix d’authentification Multifacteur avec votre nom convivial.

Lorsque vous entrez « adfabric » à la page d’authentification MFA, vous devez voir une réussite de connexion à.

![Connectez-vous avec l’adaptateur](media/ad-fs-build-custom-auth-method/Dn783423.630d8a91-3bfe-4cba-8acf-03eae21530ee(MSDN.10).jpg "connectez-vous avec l’adaptateur")

![Connectez-vous avec l’adaptateur](media/ad-fs-build-custom-auth-method/Dn783423.c340fa73-f70f-4870-b8dd-07900fea4469(MSDN.10).jpg "connectez-vous avec l’adaptateur")

## <a name="see-also"></a>Voir aussi

#### <a name="other-resources"></a>Autres ressources

[Méthodes d’authentification supplémentaires](https://msdn.microsoft.com/en-us/library/dn758113\(v=msdn.10\))  
[Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](https://msdn.microsoft.com/en-us/library/dn280949\(v=msdn.10\))

