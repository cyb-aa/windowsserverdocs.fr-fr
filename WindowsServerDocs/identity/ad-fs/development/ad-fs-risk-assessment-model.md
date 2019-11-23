---
title: Créer des plug-ins avec le modèle d’évaluation des risques AD FS 2019
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: ''
ms.date: 04/16/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1a4569b1fa3791d1d3b412b0801f216c975aa4dc
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867611"
---
# <a name="build-plug-ins-with-ad-fs-2019-risk-assessment-model"></a>Créer des plug-ins avec le modèle d’évaluation des risques AD FS 2019

Vous pouvez maintenant créer vos propres plug-ins pour bloquer ou attribuer un score de risque aux demandes d’authentification au cours des différentes étapes : demande reçue, pré-authentification et après authentification. Pour ce faire, vous pouvez utiliser le nouveau modèle d’évaluation des risques introduit avec AD FS 2019. 

## <a name="what-is-the-risk-assessment-model"></a>Qu’est-ce que le modèle d’évaluation des risques ?

Le modèle d’évaluation des risques est un ensemble d’interfaces et de classes qui permettent aux développeurs de lire les en-têtes de demande d’authentification et d’implémenter leur propre logique d’évaluation des risques. Le code implémenté (plug-in) s’exécute ensuite en fonction du processus d’authentification AD FS. Par exemple, en utilisant les interfaces et les classes incluses dans le modèle, vous pouvez implémenter le code pour bloquer ou autoriser la demande d’authentification en fonction de l’adresse IP du client incluse dans l’en-tête de demande. AD FS exécutera le code pour chaque demande d’authentification et prendra les mesures appropriées conformément à la logique implémentée. 

Le modèle autorise le code de plug-in à trois étapes de AD FS pipeline d’authentification, comme indiqué ci-dessous.

![modèle](media/ad-fs-risk-assessment-model/risk1.png)

1.  **Étape de la demande reçue** : active la génération de plug-ins pour autoriser ou bloquer la demande lorsque AD FS reçoit la demande d’authentification, c.-à-d. avant que l’utilisateur entre des informations d’identification. Vous pouvez utiliser le contexte de la requête (par exemple, IP du client, méthode http, DNS du serveur proxy, etc.) disponible à ce niveau pour effectuer l’évaluation des risques. Par exemple, vous pouvez créer un plug-in pour lire l’adresse IP à partir du contexte de la demande et bloquer la demande d’authentification si l’adresse IP se trouve dans la liste prédéfinie des adresses IP à risque. 

2.  **Étape de pré-authentification** : permet de créer des plug-ins pour autoriser ou bloquer les demandes au point où l’utilisateur fournit les informations d’identification, mais avant que AD FS les évalue. À ce niveau, en plus du contexte de la requête, vous avez également des informations sur le contexte de sécurité (par exemple, jeton d’utilisateur, identificateur d’utilisateur, etc.) et le contexte de protocole (par exemple, le protocole d’authentification, ClientID, ResourceId, etc.) à utiliser dans votre logique d’évaluation des risques. Par exemple, vous pouvez créer un plug-in pour empêcher les attaques par pulvérisation de mot de passe en lisant le mot de passe utilisateur à partir du jeton de l’utilisateur et en bloquant la demande d’authentification si le mot de passe est dans la liste prédéfinie des mots de passe à risque. 

3.  **Après l’authentification** : active la génération du plug-in pour évaluer les risques après que l’utilisateur a fourni les informations d’identification et que AD FS a effectué l’authentification. À ce niveau, en plus du contexte de la requête, du contexte de sécurité et du contexte de protocole, vous avez également des informations sur le résultat de l’authentification (réussite ou échec). Le plug-in peut évaluer le score de risque en fonction des informations disponibles et passer le score de risque à la revendication et aux règles de stratégie pour une évaluation supplémentaire. 

Pour mieux comprendre comment créer un plug-in d’évaluation des risques et l’exécuter en ligne avec AD FS processus, nous allons créer un exemple de plug-in qui bloque les demandes provenant de certaines adresses IP **extranet** identifiées comme étant risquées, enregistrer le plug-in avec AD FS et enfin tester la fonctionnalité. 

>[!NOTE]
>Cette procédure pas à pas sert uniquement à vous montrer comment créer un exemple de plug-in. En aucun cas, la solution nous crée une solution d’entreprise prête.  

## <a name="building-a-sample-plug-in"></a>Génération d’un exemple de plug-in

### <a name="pre-requisites"></a>Prérequis
Voici la liste des conditions préalables requises pour générer cet exemple de plug-in

- AD FS 2019 installé et configuré
- .NET Framework 4,7 et versions ultérieures
- Visual Studio

### <a name="build-plug-in-dll"></a>Dll du plug-in de build
La procédure suivante vous guidera dans la création d’un exemple de dll de plug-in.

1. Téléchargez l’exemple de plug-in, utilisez git bash et tapez ce qui suit : 

   ```
   git clone https://github.com/Microsoft/adfs-sample-RiskAssessmentModel-RiskyIPBlock
   ```

2. Créez un fichier **. csv** à n’importe quel emplacement sur votre serveur AD FS (dans mon cas, j’ai créé le fichier **authconfigdb. csv** sur **C:\Extensions**) et j’ajoute les adresses IP que vous souhaitez bloquer à ce fichier. 

   L’exemple de plug-in bloque toutes les demandes d’authentification provenant des **adresses IP extranet** listées dans ce fichier. 

   >{! Remarque : Si vous disposez d’une batterie de serveurs AD FS, vous pouvez créer le fichier sur tout ou partie des serveurs AD FS. N’importe quel fichier peut être utilisé pour importer les adresses IP à risque dans AD FS. Nous présenterons en détail le processus d’importation dans la section [inscrire la dll du plug-in avec AD FS](#register-the-plug-in-dll-with-ad-fs) ci-dessous. 

3. Ouvrir le projet `ThreatDetectionModule.sln` à l’aide de Visual Studio

4. Supprimez le `Microsoft.IdentityServer.dll` de l’Explorateur de solutions, comme indiqué ci-dessous :</br>
   ](media/ad-fs-risk-assessment-model/risk2.png) du modèle de ![

5. Ajoutez une référence à la `Microsoft.IdentityServer.dll` de votre AD FS comme indiqué ci-dessous

   a.   Cliquez avec le bouton droit sur **références** dans l' **Explorateur de solutions** , puis sélectionnez **Ajouter une référence...**</br> 
   modèle de ![](media/ad-fs-risk-assessment-model/risk3.png)
   
   b.   Dans la fenêtre **Gestionnaire de références** , sélectionnez **Parcourir**. Dans la, **sélectionnez les fichiers à référencer...** , sélectionnez `Microsoft.IdentityServer.dll` dans votre dossier d’installation AD FS (dans mon cas **C:\Windows\ADFS**), puis cliquez sur **Ajouter**.
   
   >[!NOTE]
   >Dans mon cas, je crée le plug-in sur le serveur AD FS lui-même. Si votre environnement de développement se trouve sur un autre serveur, copiez le `Microsoft.IdentityServer.dll` à partir de votre dossier d’installation AD FS sur AD FS serveur dans votre boîte de développement.</br> 
   
   ![modèle](media/ad-fs-risk-assessment-model/risk4.png)
   
   c.   Cliquez sur **OK** dans la fenêtre **Gestionnaire de références** après avoir vérifié `Microsoft.IdentityServer.dll` case à cocher est activée</br>
   ](media/ad-fs-risk-assessment-model/risk5.png) du modèle de ![
 
6. Toutes les classes et références sont maintenant en place pour effectuer une génération.   Toutefois, étant donné que la sortie de ce projet est une dll, elle doit être installée dans le **global assembly cache**(GAC) du serveur AD FS et la dll doit d’abord être signée. Pour ce faire, procédez comme suit :

   a.   **Cliquez avec le bouton droit** sur le nom du projet, ThreatDetectionModule. Dans le menu, cliquez sur **Propriétés**.</br>
   ](media/ad-fs-risk-assessment-model/risk6.png) du modèle de ![
   
   b.   Dans la page **Propriétés** , cliquez sur **signature**, à gauche, puis cochez la case **signer l’assembly**. Dans le menu déroulant **choisir un fichier de clé de nom fort**:, sélectionnez **< nouveau... >**</br>
   ](media/ad-fs-risk-assessment-model/risk7.png) du modèle de ![

   c.   Dans la **boîte de dialogue créer une clé de nom fort**, tapez un nom (vous pouvez choisir n’importe quel nom) pour la clé, désactivez la case à cocher **protéger mon fichier de clé par un mot de passe**. Cliquez ensuite sur **OK**.
   ](media/ad-fs-risk-assessment-model/risk8.png) du modèle de ![</br>
 
   d.   Enregistrez le projet comme indiqué ci-dessous</br>
   ](media/ad-fs-risk-assessment-model/risk9.png) du modèle de ![

7. Générez le projet en cliquant sur **générer** , puis sur **régénérer la solution** comme indiqué ci-dessous</br>
   ](media/ad-fs-risk-assessment-model/risk10.png) du modèle de ![
 
   Vérifiez la **fenêtre Sortie**, en bas de l’écran, pour voir si des erreurs se sont produites.</br>
   ](media/ad-fs-risk-assessment-model/risk11.png) du modèle de ![


Le plug-in (dll) est maintenant prêt à être utilisé et se trouve dans le dossier **\bin\Debug** du dossier du projet (dans mon cas, il s’agit de **C:\extensions\ThreatDetectionModule\bin\Debug\ThreatDetectionModule.dll**). 

L’étape suivante consiste à inscrire cette dll auprès de AD FS, afin qu’elle s’exécute en fonction du processus d’authentification AD FS. 

### <a name="register-the-plug-in-dll-with-ad-fs"></a>Inscrire la dll du plug-in avec AD FS

Nous devons inscrire la dll dans AD FS à l’aide de la commande `Register-AdfsThreatDetectionModule` PowerShell sur le serveur de AD FS. Toutefois, avant de vous inscrire, nous devons récupérer le jeton de clé publique. Ce jeton de clé publique a été créé lors de la création de la clé et de la signature de la dll à l’aide de cette clé. Pour en savoir plus sur le jeton de clé publique de la dll, vous pouvez utiliser **sn. exe** comme suit :

1. Copiez le fichier dll du dossier **\bin\Debug** vers un autre emplacement (dans mon cas, en le copiant dans **C:\Extensions**)

2. Démarrez le **invite de commandes développeur** pour Visual Studio et accédez au répertoire contenant le fichier **sn. exe** (dans mon cas, le répertoire est **C:\Program Files (x86) \microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools**) ![modèle](media/ad-fs-risk-assessment-model/risk12.png)

3. Exécutez la commande **sn** avec le paramètre **-T** et l’emplacement du fichier (dans mon cas `SN -T “C:\extensions\ThreatDetectionModule.dll”`) ![modèle](media/ad-fs-risk-assessment-model/risk13.png)</br>
   La commande vous fournira le jeton de clé publique (pour moi, le **jeton de clé publique est 714697626ef96b35**)

4. Ajoutez la dll au **global assembly cache** du serveur AD FS. nous vous conseillons de créer un programme d’installation approprié pour votre projet et d’utiliser le programme d’installation pour ajouter le fichier au GAC. Une autre solution consiste à utiliser **Gacutil. exe** (plus d’informations sur **Gacutil. exe** disponibles [ici](https://docs.microsoft.com/dotnet/framework/tools/gacutil-exe-gac-tool)) sur votre ordinateur de développement.  Comme j’ai mon Visual Studio sur le même serveur que AD FS, j’utilise **Gacutil. exe** comme suit

   a.   Sur Invite de commandes développeur pour Visual Studio et accédez au répertoire contenant le fichier **Gacutil. exe** (dans mon cas, le répertoire est **C:\Program Files (x86) \Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools**)

   b.   Exécutez la commande **gacutil** (dans mon cas `Gacutil /IF C:\extensions\ThreatDetectionModule.dll`) ![modèle](media/ad-fs-risk-assessment-model/risk14.png)
 
   >[!NOTE]
   >Si vous disposez d’une batterie de serveurs AD FS, les éléments ci-dessus doivent être exécutés sur chaque serveur de AD FS de la batterie. 

5. Ouvrez **Windows PowerShell** et exécutez la commande suivante pour inscrire la dll
   ```
   Register-AdfsThreatDetectionModule -Name "<Add a name>" -TypeName "<class name that implements interface>, <dll name>, Version=10.0.0.0, Culture=neutral, PublicKeyToken=< Add the Public Key Token from Step 2. above>" -ConfigurationFilePath "<path of the .csv file>”
   ```
   Dans mon cas, la commande est : 
   ```
   Register-AdfsThreatDetectionModule -Name "IPBlockPlugin" -TypeName "ThreatDetectionModule.UserRiskAnalyzer, ThreatDetectionModule, Version=10.0.0.0, Culture=neutral, PublicKeyToken=714697626ef96b35" -ConfigurationFilePath "C:\extensions\authconfigdb.csv”
   ```
 
   >[!NOTE]
   >Vous devez inscrire la dll une seule fois, même si vous disposez d’une batterie de AD FS. 

6. Redémarrer le service AD FS après l’inscription de la dll

C’est là que la dll est maintenant enregistrée avec AD FS et prête à l’emploi.

 >[!NOTE]
 > Si des modifications sont apportées au plug-in et que le projet est régénéré, la dll mise à jour doit être inscrite à nouveau. Avant de procéder à l’inscription, vous devrez annuler l’inscription de la dll actuelle à l’aide de la commande suivante :</br></br>
 >`
  UnRegister-AdfsThreatDetectionModule -Name "<name used while registering the dll in 5. above>"
 >`</br></br> Dans mon cas, la commande est :
 >``` 
 >UnRegister-AdfsThreatDetectionModule -Name "IPBlockPlugin"
 >```

### <a name="testing-the-plug-in"></a>Test du plug-in

1. Ouvrez le fichier **authconfig. csv** que nous avons créé précédemment (dans mon cas, à l’emplacement **C:\Extensions**) et ajoutez les **adresses IP extranet** que vous souhaitez bloquer. Chaque adresse IP doit se trouver sur une ligne distincte et il ne doit y avoir aucun espace à la fin</br>
   ](media/ad-fs-risk-assessment-model/risk18.png) du modèle de ![
 
2. Enregistrez et fermez le fichier

3. Importez le fichier mis à jour dans AD FS en exécutant la commande PowerShell suivante 

   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "<name given while registering the dll>" -ConfigurationFilePath "<path of the .csv file>"
   ```
 
   Dans mon cas, la commande est : 
   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "IPBlockPlugin" -ConfigurationFilePath "C:\extensions\authconfigdb.csv")
   ```
 
4. Initiez une demande d’authentification à partir du serveur avec la même adresse IP que celle que vous avez ajoutée dans **authconfig. csv**.

   Pour cette démonstration, j’utiliserai AD FS l’aide de l' [outil de radiographie](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) pour lancer une demande. Si vous souhaitez utiliser l’outil de rayon X, suivez les instructions 

   Entrez l’instance du serveur de Fédération et le bouton **d’authentification du test** de positionnement.</br> 
   modèle de ![](media/ad-fs-risk-assessment-model/risk15.png) 

5. L’authentification est bloquée comme indiqué ci-dessous.</br>
   ](media/ad-fs-risk-assessment-model/risk16.png) du modèle de ![
 
Maintenant que nous savons comment générer et inscrire le plug-in, voyons pas à pas le code de plug-in pour comprendre l’implémentation à l’aide des nouvelles interfaces et classes introduites avec le modèle. 

## <a name="plug-in-code-walkthrough"></a>Procédure pas à pas de code de plug-in

Ouvrez le projet `ThreatDetectionModule.sln` à l’aide de Visual Studio, puis ouvrez le fichier principal **UserRiskAnalyzer.cs** à partir de l' **Explorateur de solutions** à droite de l’écran.</br>
](media/ad-fs-risk-assessment-model/risk17.png) du modèle de ![
 
Le fichier contient la classe principale UserRiskAnalyzer qui implémente les classes abstraites [ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019) et interface [IRequestReceivedThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019) pour lire l’adresse IP à partir du contexte de la requête, comparer l’adresse IP obtenue avec les adresses IP chargées à partir de AD FS DB et bloquer la demande en cas de correspondance d’adresse IP. Passons en revue ces types plus en détail

### <a name="threatdetectionmodule-abstract-class"></a>Classe abstraite ThreatDetectionModule

Cette classe abstraite charge le plug-in dans AD FS pipeline, ce qui permet d’exécuter le code de plug-in en ligne avec AD FS processus. 

```
public abstract class ThreatDetectionModule 
{ 
    protected ThreatDetectionModule(); 
 
    public abstract string VendorName { get; } 
    public abstract string ModuleIdentifier { get; } 
 
 public abstract void OnAuthenticationPipelineLoad(ThreatDetectionLogger logger, ThreatDetectionModuleConfiguration configData); 
 public abstract void OnAuthenticationPipelineUnload(ThreatDetectionLogger logger); 
  public abstract void OnConfigurationUpdate(ThreatDetectionLogger logger, ThreatDetectionModuleConfiguration configData); 
 }   
```
La classe comprend les méthodes et les propriétés suivantes.

|Méthode |Type|Définition|
|-----|-----|-----| 
|[OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) |Nullité|Appelé par AD FS lorsque le plug-in est chargé dans son pipeline| 
|[OnAuthenticationPipelineUnload](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineunload?view=adfs-2019) |Nullité|Appelé par AD FS lorsque le plug-in est déchargé de son pipeline| 
|[OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019)| Nullité|Appelé par AD FS sur la mise à jour de la configuration |
|**Propriété** |**Type** |**Définition**|
|[NomFournisseur](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.vendorname?view=adfs-2019)|Chaîne |Obtient le nom du fournisseur propriétaire du plug-in.|
|[ModuleIdentifier](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.moduleidentifier?view=adfs-2019)|Chaîne |Obtient l’identificateur du plug-in.|

Dans notre exemple de plug-in, nous utilisons les méthodes [OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) et [OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019) pour lire les adresses IP prédéfinies à partir de AD FS DB. [OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) est appelé quand le plug-in est inscrit avec AD FS tandis que [OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019) est appelé lorsque le fichier. csv est importé à l’aide de l’applet de commande `Import-AdfsThreatDetectionModuleConfiguration`. 

#### <a name="irequestreceivedthreatdetectionmodule-interface"></a>Interface IRequestReceivedThreatDetectionModule

Cette [interface](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019) vous permet d’implémenter l’évaluation des risques au point où AD FS reçoit la demande d’authentification, mais avant que l’utilisateur n’entre les informations d’identification, c’est-à-dire à l’étape de demande reçue du processus d’authentification. 
 
```
public interface IRequestReceivedThreatDetectionModule
{
Task<ThrottleStatus> EvaluateRequest (
ThreatDetectionLogger logger, 
RequestContext requestContext );
}
```

L’interface comprend la méthode [EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019) qui vous permet d’utiliser le contexte de la demande d’authentification passée dans le paramètre d’entrée RequestContext pour écrire votre logique d’évaluation des risques. Le paramètre requestContext est de type [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019). 

L’autre paramètre d’entrée passé est Logger, qui est de type [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). Le paramètre peut être utilisé pour écrire les messages d’erreur, d’audit et/ou de débogage dans AD FS journaux. 

La méthode retourne [ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) (0 si NotEvaluated, 1 à Block et 2 to allow) à AD FS qui bloque ou autorise ensuite la requête.

Dans notre exemple de plug-in, l’implémentation de la méthode [EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019) analyse le [clientIpAddress](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext.clientipaddresses?view=adfs-2019#Microsoft_IdentityServer_Public_ThreatDetectionFramework_RequestContext_ClientIpAddresses) à partir du paramètre [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019) et le compare à toutes les adresses IP chargées à partir de la base de AD FS DB. Si une correspondance est trouvée, la méthode retourne 2 pour le **bloc**, sinon elle retourne 1 pour **allow**. En fonction de la valeur retournée, AD FS bloque ou autorise la demande. 

>[!NOTE]
>L’exemple de plug-in abordé ci-dessus implémente uniquement l’interface IRequestReceivedThreatDetectionModule. Toutefois, le modèle d’évaluation des risques fournit deux interfaces supplémentaires : IPreAuthenticationThreatDetectionModule (pour implémenter la logique d’évaluation des risques au cours étape de pré-authentification) et IPostAuthenticationThreatDetectionModule (pour mettre en œuvre les risques logique d’évaluation lors de la phase après l’authentification). Les détails sur les deux interfaces sont fournis ci-dessous. 

#### <a name="ipreauthenticationthreatdetectionmodule-interface"></a>Interface IPreAuthenticationThreatDetectionModule 

Cette [interface](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule?view=adfs-2019) vous permet d’implémenter la logique d’évaluation des risques au point où l’utilisateur fournit les informations d’identification, mais avant AD FS les évalue, c.-à-d. l’étape de pré-authentification. 

```
public interface IPreAuthenticationThreatDetectionModule
{
Task<ThrottleStatus> EvaluatePreAuthentication (
ThreatDetectionLogger logger, 
RequestContext requestContext, 
SecurityContext securityContext, 
ProtocolContext protocolContext, 
IList<Claim> additionalClams
);
}
```
L’interface comprend la méthode [EvaluatePreAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule.evaluatepreauthentication?view=adfs-2019) qui vous permet d’utiliser les informations passées dans les paramètres d’entrée [RequestContext RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), [ProtocolContext ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)et [IList<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2) pour écrire votre logique d’évaluation des risques de pré-authentification. 

>[!NOTE]
>Pour obtenir la liste des propriétés transmises avec chaque type de contexte, visitez les définitions de classe [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)et [ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019) . 

L’autre paramètre d’entrée passé est Logger, qui est de type [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). Le paramètre peut être utilisé pour écrire les messages d’erreur, d’audit et/ou de débogage dans AD FS journaux.

La méthode retourne [ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) (0 si NotEvaluated, 1 à Block et 2 to allow) à AD FS qui bloque ou autorise ensuite la requête. 

#### <a name="ipostauthenticationthreatdetectionmodule-interface"></a>Interface IPostAuthenticationThreatDetectionModule

Cette [interface](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule?view=adfs-2019) vous permet d’implémenter la logique d’évaluation des risques une fois que l’utilisateur a fourni les informations d’identification et que AD FS a effectué l’authentification, c.-à-d. l’étape de postconnexion. 

```
public interface IPostAuthenticationThreatDetectionModule
{
Task<RiskScore> EvaluatePostAuthentication (
ThreatDetectionLogger logger, 
RequestContext requestContext, 
SecurityContext securityContext, 
ProtocolContext protocolContext, 
AuthenticationResult authenticationResult, 
IList<Claim> additionalClams
);
}
```

L’interface comprend la méthode [EvaluatePostAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule.evaluatepostauthentication?view=adfs-2019) qui vous permet d’utiliser les informations passées dans les paramètres d’entrée [RequestContext RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), [ProtocolContext ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)et [IList<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2) pour écrire votre logique d’évaluation des risques après l’authentification. 

>[!NOTE]
> Pour obtenir la liste complète des propriétés transmises avec chaque type de contexte, consultez les définitions de classe [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)et [ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019) . 

L’autre paramètre d’entrée passé est Logger, qui est de type [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). Le paramètre peut être utilisé pour écrire les messages d’erreur, d’audit et/ou de débogage dans AD FS journaux. 

La méthode retourne le [score de risque](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.authentication.riskscoreconstants?view=adfs-2019) qui peut être utilisé dans AD FS règles de stratégie et de revendication. 

>[!NOTE]
>Pour que le plug-in fonctionne, la classe principale (dans ce cas UserRiskAnalyzer) doit dériver la classe abstraite [ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019) et doit implémenter au moins l’une des trois interfaces décrites ci-dessus. Une fois la dll inscrite, AD FS vérifie les interfaces qui sont implémentées et les appelle à l’étape appropriée dans le pipeline.

### <a name="faqs"></a>FAQ

**Pourquoi dois-je créer ces plug-ins ?**</br>
**R :** Ces plug-ins vous offrent non seulement des fonctionnalités supplémentaires pour sécuriser votre environnement contre les attaques telles que les attaques par pulvérisation de mot de passe, mais également la possibilité de créer votre propre logique d’évaluation des risques en fonction de vos besoins. 

**Où les journaux sont-ils capturés ?**</br>
**R :** Vous pouvez écrire les journaux d’erreurs dans le journal des événements « AD FS/admin » à l’aide de la méthode WriteAdminLogErrorMessage, les journaux d’audit dans le journal de sécurité « AD FS audit » à l’aide de la méthode WriteAuditMessage et les journaux de débogage dans le journal de débogage « AD FS Tracing » en utilisant la méthode 

**L’ajout de ces plug-ins augmente-t-il AD FS latence du processus d’authentification ?**</br>
**R :** L’impact de la latence sera déterminé par le temps nécessaire à l’exécution de la logique d’évaluation des risques que vous implémentez. Nous vous recommandons d’évaluer l’impact de la latence avant de déployer le plug-in dans l’environnement de production. 

**Pourquoi ne puis-je pas AD FS suggérer la liste des adresses IP risquées, des utilisateurs, etc. ?**</br>
**R :** Bien qu’elles ne soient pas disponibles actuellement, nous travaillons sur la création de l’intelligence pour suggérer des adresses IP à risque, des utilisateurs, etc. dans le modèle d’évaluation des risques enfichables. Nous allons bientôt partager les dates de lancement. 
