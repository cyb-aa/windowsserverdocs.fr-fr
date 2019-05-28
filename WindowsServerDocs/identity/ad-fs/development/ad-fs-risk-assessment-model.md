---
title: Créer des Plug-ins avec un modèle d’évaluation des risques 2019 AD FS
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: ''
ms.date: 04/16/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 80f42695af917084ee63297df052adc069340bb3
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190522"
---
# <a name="build-plug-ins-with-ad-fs-2019-risk-assessment-model"></a>Créer des Plug-ins avec un modèle d’évaluation des risques 2019 AD FS

Vous pouvez maintenant créer vos propres plug-ins pour bloquer ou affecter un score de risque pour les demandes d’authentification pendant plusieurs étapes – demande reçue, l’authentification préalable et après l’authentification. Cela peut être accompli utilisant le nouveau modèle d’évaluation des risques introduits avec AD FS 2019. 

## <a name="what-is-the-risk-assessment-model"></a>Qu’est le modèle d’évaluation des risques ?

Le modèle d’évaluation des risques est un ensemble d’interfaces et classes qui permettent aux développeurs de lire les en-têtes de demande d’authentification et d’implémenter leur propre logique d’évaluation des risques. Il exécute ensuite le code implémenté (plug-in) conformément aux processus d’authentification AD FS. Pour par exemple, utilisez les interfaces et classes inclus avec le modèle, vous pouvez implémenter un bloc de code ou autoriser la demande d’authentification basée sur l’adresse IP de client inclus dans l’en-tête de demande. AD FS exécute le code pour chaque demande d’authentification et prendre les mesures appropriées en fonction de la logique implémentée. 

Le modèle permet au code de plug-in à un des trois étapes du pipeline d’authentification AD FS comme indiqué ci-dessous

![modèle](media\ad-fs-risk-assessment-model\risk1.png)

1.  **Demande reçu de phase** – permet de créer des plug-ins pour autoriser ou bloquer la demande lorsque AD FS reçoit la demande d’authentification par exemple, avant que l’utilisateur entre des informations d’identification. Vous pouvez utiliser le contexte de requête (par exemple, adresse IP du client, méthode Http, le serveur proxy DNS, etc.) disponibles à ce stade pour effectuer l’évaluation des risques. Pour par exemple, vous pouvez créer un plug-in pour lire l’adresse IP à partir du contexte de demande et de bloquer la demande d’authentification si l’adresse IP se trouve dans la liste prédéfinie d’adresses IP à risque. 

2.  **Étape de pré-authentification** – permet de créer des plug-ins pour autoriser ou bloquer la demande au point où l’utilisateur fournit les informations d’identification, mais avant que les évalue par AD FS. À ce stade, en plus du contexte de requête vous avez également plus d’informations sur le contexte de sécurité (par exemple, jeton d’utilisateur, l’identificateur d’utilisateur, etc.) et le contexte de protocole (par exemple, protocole d’authentification, clientid, resourceid, etc.) à utiliser dans votre logique d’évaluation des risques. Pour par exemple, vous pouvez créer un plug-in pour empêcher les attaques de mot de passe pulvérisation en lisant le mot de passe utilisateur à partir du jeton d’utilisateur et en bloquant la demande d’authentification si le mot de passe se trouve dans la liste prédéfinie des mots de passe à risque. 

3.  **Post-Authentication** – permet de créer un plug-in pour évaluer les risques face une fois que l’utilisateur a fourni des informations d’identification et AD FS a effectué l’authentification. À ce stade, outre le contexte de requête, le contexte de sécurité et le contexte du protocole, vous avez également plus d’informations sur le résultat de l’authentification (réussite ou échec). Le plug-in peut évaluer le score de risque selon les informations disponibles et transmettre le score de risque à revendiquer et les règles de stratégie pour l’évaluation supplémentaire. 

Pour mieux comprendre comment créer une évaluation des risques plug-in et l’exécuter conformément aux processus d’AD FS, nous allons générer un exemple plug-in qui bloque les demandes en provenance de certains **extranet** IPs identifié comme risquée, inscrire le plug-in avec AD FS et enfin tester la fonctionnalité. 

>[!NOTE]
>Cette procédure pas à pas est uniquement pour vous montrer comment vous pouvez créer un exemple de plug-in. En aucun cas est la solution, nous allons créer une solution d’entreprise prêt.  

## <a name="building-a-sample-plug-in"></a>Création d’un exemple de plug-in

### <a name="pre-requisites"></a>Conditions préalables
Voici la liste des composants requis pour générer cet exemple de plug-in

- AD FS 2019 installé et configuré
- .NET framework 4.7 et versions ultérieures
- Visual Studio

### <a name="build-plug-in-dll"></a>Générez la dll de plug-in
La procédure suivante vous guidera dans la génération d’une dll de plug-in d’exemple.

 1. Téléchargez l’exemple de plug-in, utilisez Git Bash et tapez la commande suivante : 

   ```
   git clone https://github.com/Microsoft/adfs-sample-RiskAssessmentModel-RiskyIPBlock
   ```

 2. Créer un **.csv** fichier à n’importe quel emplacement sur votre serveur AD FS (dans mon cas, j’ai créé le **authconfigdb.csv** fichier **C:\extensions**) et ajoutez les adresses IP que vous souhaitez bloquer à ce fichier. 

   L’exemple de plug-in bloque les demandes d’authentification provenant de la **IPs Extranet** répertoriés dans ce fichier. 

   >{! Remarque] Si vous avez une batterie de serveurs AD FS, vous pouvez créer le fichier sur un ou tous les serveurs AD FS. Les fichiers peuvent être utilisés pour importer les adresses IP à risque dans AD FS. Nous allons aborder le processus d’importation en détail dans le [inscrire la dll de plug-in avec AD FS](#register-the-plug-in-dll-with-ad-fs) section ci-dessous. 

 3. Ouvrez le projet `ThreatDetectionModule.sln` à l’aide de Visual Studio

 4. Supprimer le `Microsoft.IdentityServer.dll` à partir de l’Explorateur de Solutions, comme indiqué ci-dessous :</br>
 ![model](media\ad-fs-risk-assessment-model\risk2.png)

 5. Ajouter une référence à la `Microsoft.IdentityServer.dll` de vos services AD FS comme indiqué ci-dessous

   a.   Cliquez avec le bouton droit sur **références** dans **l’Explorateur de Solutions** et sélectionnez **ajouter une référence...**</br> 
   ![model](media\ad-fs-risk-assessment-model\risk3.png)
   
   b.   Sur le **Gestionnaire de références** fenêtre Sélectionnez **Parcourir**. Dans le **sélectionner les fichiers à référencer...** boîte de dialogue, sélectionnez `Microsoft.IdentityServer.dll` à partir de votre dossier d’installation AD FS (dans mon cas **C:\Windows\ADFS**) et cliquez sur **ajouter**.
   
   >[!NOTE]
    >Dans mon cas, je crée le plug-in sur le serveur AD FS elle-même. Si votre environnement de développement se trouve sur un autre serveur, copiez le `Microsoft.IdentityServer.dll` à partir de votre dossier d’installation AD FS sur le serveur AD FS à votre boîte de développement.</br> 
   
   ![modèle](media\ad-fs-risk-assessment-model\risk4.png)
   
   c.   Cliquez sur **OK** sur le **Gestionnaire de références** fenêtre après avoir vérifié `Microsoft.IdentityServer.dll` case à cocher est activée</br>
   ![model](media\ad-fs-risk-assessment-model\risk5.png)
 
 6. Toutes les classes et les références sont désormais en place pour effectuer une build.   Toutefois, étant donné que la sortie de ce projet est une dll, il devra être installé dans le **Global Assembly Cache**, ou le GAC, du serveur AD FS et la dll doit être signé tout d’abord. Cela est possible comme suit :

   a.   **Avec le bouton droit** sur le nom du projet, ThreatDetectionModule. Dans le menu, cliquez sur **propriétés**.</br>
   ![model](media\ad-fs-risk-assessment-model\risk6.png)
   
   b.   À partir de la **propriétés** , cliquez sur **signature**, sur la gauche, puis activez la case à cocher marquée **signer l’assembly**. À partir de la **choisir un fichier de clé de nom fort**: extraire vers le bas du menu, sélectionnez **< nouveau... >**</br>
   ![model](media\ad-fs-risk-assessment-model\risk7.png)

   c.   Dans le **boîte de dialogue Créer une clé de nom fort**, tapez un nom (vous pouvez choisir n’importe quel nom) de la clé, décochez la case à cocher **protéger mon fichier de clé avec un mot de passe**. Cliquez ensuite sur **OK**.
   ![model](media\ad-fs-risk-assessment-model\risk8.png)</br>
 
   d.   Enregistrez le projet, comme indiqué ci-dessous</br>
   ![model](media\ad-fs-risk-assessment-model\risk9.png)

 7. Générez le projet en cliquant sur **Build** , puis **régénérer la Solution** comme indiqué ci-dessous</br>
 ![model](media\ad-fs-risk-assessment-model\risk10.png)
 
 Vérifier le **fenêtre sortie**, au bas de l’écran, pour voir si des erreurs se sont produites</br>
 ![model](media\ad-fs-risk-assessment-model\risk11.png)


La plug-in (dll) est maintenant prêt à être utilisé et est dans le **\bin\Debug** dossier du dossier du projet (dans mon cas, c’est la **C:\extensions\ThreatDetectionModule\bin\Debug\ThreatDetectionModule.dll**). 

L’étape suivante consiste à inscrire cette dll avec AD FS, s’il s’exécute conformément aux processus d’authentification AD FS. 

### <a name="register-the-plug-in-dll-with-ad-fs"></a>Inscrivez la dll de plug-in avec AD FS

Nous devons inscrire la dll dans AD FS à l’aide de la `Register-AdfsThreatDetectionModule` commande PowerShell sur le serveur AD FS, toutefois, avant de nous enregistrons, nous devons obtenir le jeton de clé publique. Ce jeton de clé publique a été créé lorsque nous avons créé la clé et signé la dll à l’aide de cette clé. Pour savoir ce que le jeton de clé publique pour la dll est, vous pouvez utiliser la **SN.exe** comme suit

 1. Copiez le fichier dll à partir de la **\bin\Debug** dossier vers un autre emplacement (dans mon cas copier dans **C:\extensions**)

 2. Démarrer le **invite de commandes développeur** pour Visual Studio et accédez au répertoire contenant le **sn.exe** (dans mon cas, le répertoire est **C:\Program Files (x86) \Microsoft SDKs\Windows\v10.0A outils de \bin\NETFX 4.7.2**) ![modèle](media\ad-fs-risk-assessment-model\risk12.png)

 3. Exécutez le **SN** commande avec le **-T** paramètre et l’emplacement du fichier (dans mon cas `SN -T “C:\extensions\ThreatDetectionModule.dll”`) ![modèle](media\ad-fs-risk-assessment-model\risk13.png)</br>
 La commande vous fournira le jeton de clé publique (pour moi la **jeton de clé publique est 714697626ef96b35**)

 4. Ajouter la dll à la **Global Assembly Cache** du serveur AD FS notre meilleure pratique serait que vous créez un programme d’installation approprié pour votre projet et utilisez le programme d’installation pour ajouter le fichier dans le GAC. Une autre solution consiste à utiliser **Gacutil.exe** (plus d’informations sur **Gacutil.exe** disponible [ici](https://docs.microsoft.com/dotnet/framework/tools/gacutil-exe-gac-tool)) sur votre ordinateur de développement.  Dans la mesure où j’ai mon visual studio sur le même serveur qu’AD FS, je l’utiliserez **Gacutil.exe** comme suit

   a.   Sur invite de commandes développeur pour Visual Studio et accédez au répertoire contenant le **Gacutil.exe** (dans mon cas, le répertoire est **C:\Program fichiers (x86) \Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools**)

   b.   Exécutez le **Gacutil** commande (dans mon cas `Gacutil /IF C:\extensions\ThreatDetectionModule.dll`) ![modèle](media\ad-fs-risk-assessment-model\risk14.png)
 
 >[!NOTE]
 >Si vous avez une batterie de serveurs AD FS, la méthode ci-dessus doit être exécutée sur chaque serveur AD FS dans la batterie de serveurs. 

 5. Ouvrez **Windows PowerShell** et exécutez la commande suivante pour inscrire la dll
    ```
    Register-AdfsThreatDetectionModule -Name "<Add a name>" -TypeName "<class name that implements interface>, <dll name>, Version=10.0.0.0, Culture=neutral, PublicKeyToken=< Add the Public Key Token from Step 2. above>" -ConfigurationFilePath "<path of the .csv file>”
    ```
    Dans mon cas, la commande est : 
    ```
    Register-AdfsThreatDetectionModule -Name "IPBlockPlugin" -TypeName "ThreatDetectionModule.UserRiskAnalyzer, ThreatDetectionModule, Version=10.0.0.0, Culture=neutral, PublicKeyToken=714697626ef96b35" -ConfigurationFilePath "C:\extensions\authconfigdb.csv”
    ```
 
    >[!NOTE]
    >Vous devez inscrire la dll qu’une seule fois, même si vous avez une batterie de serveurs AD FS. 

 6. Redémarrez le service AD FS après l’inscription de la dll

C’est tout, la dll est maintenant inscrit d’AD FS et prêt à utiliser !

 >[!NOTE]
 > Si des modifications sont apportées pour le plug-in et le projet est régénéré, la dll mise à jour doit être inscrit à nouveau. Avant d’enregistrer, vous devez annuler l’inscription de la dll actuelle à l’aide de la commande suivante :</br></br>
 >`
  UnRegister-AdfsThreatDetectionModule -Name "<name used while registering the dll in 5. above>"
 >`</br></br> Dans mon cas, la commande est :
 >``` 
 >UnRegister-AdfsThreatDetectionModule -Name "IPBlockPlugin"
 >```

### <a name="testing-the-plug-in"></a>Le plug-in de test

 1. Ouvrir le **authconfig.csv** fichier créé précédemment (dans mon cas à l’emplacement **C:\extensions**) et ajoutez le **IPs Extranet** vous souhaitez bloquer. Chaque adresse IP doit être sur une ligne distincte et il ne doit y avoir aucun espace à la fin</br>
 ![model](media\ad-fs-risk-assessment-model\risk18.png)
 
 2. Enregistrez et fermez le fichier

 3. Importez le fichier mis à jour dans AD FS en exécutant la commande PowerShell suivante 

  ```
  Import-AdfsThreatDetectionModuleConfiguration -name "<name given while registering the dll>" -ConfigurationFilePath "<path of the .csv file>"
  ```
 
  Dans mon cas, la commande est : 
  ```
   Import-AdfsThreatDetectionModuleConfiguration -name "IPBlockPlugin" -ConfigurationFilePath "C:\extensions\authconfigdb.csv")
 ```
 
 4. Demande initie l’authentification du serveur avec la même adresse IP que vous avez ajouté dans **authconfig.csv**.

 Pour cette démonstration, j’utilise [outil d’AD FS aide Claims x-ray](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) pour lancer une requête. Si vous souhaitez utiliser l’outil de rayons x, suivez les instructions 

 Entrez l’instance de serveur de fédération et d’accès **tester l’authentification** bouton.</br> 
 ![model](media\ad-fs-risk-assessment-model\risk15.png) 

 5. L’authentification est bloquée comme indiqué ci-dessous.</br>
 ![model](media\ad-fs-risk-assessment-model\risk16.png)
 
Maintenant que nous savons comment générer et inscrire le plug-in, nous allons procédure pas à pas le code de plug-in pour comprendre l’implémentation en utilisant les nouvelles interfaces et les classes introduite avec le modèle. 

## <a name="plug-in-code-walkthrough"></a>Procédure pas à pas du code de plug-in

Ouvrez le projet `ThreatDetectionModule.sln` à l’aide de Visual Studio, puis ouvrez le fichier principal **UserRiskAnalyzer.cs** à partir de la **l’Explorateur de Solutions** sur la droite de l’écran</br>
![model](media\ad-fs-risk-assessment-model\risk17.png)
 
Le fichier contient la classe principale UserRiskAnalyzer qui implémente la classe abstraite [ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019) et interface [IRequestReceivedThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019) pour lire l’adresse IP à partir de la demande contexte, comparer l’adresse IP obtenue avec les adresses IP chargé à partir de la base de données AD FS et bloquer la demande s’il existe une correspondance IP. Attardons-nous sur ces types de plus en détail

### <a name="threatdetectionmodule-abstract-class"></a>Classe abstraite ThreatDetectionModule

Cette classe abstraite charge le plug-in dans le pipeline d’AD FS permettant d’exécuter le code de plug-in conformément aux processus d’AD FS. 

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
La classe inclut des méthodes et propriétés suivants.

|Méthode |type|Définition|
|-----|-----|-----| 
|[OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) |Void|Appelé par AD FS lorsque le plug-in est chargé dans son pipeline| 
|[OnAuthenticationPipelineUnload](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineunload?view=adfs-2019) |Void|Appelé par AD FS lorsque le plug-in est déchargé de son pipeline| 
|[OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019)| Void|Appelé par AD FS sur la mise à jour de configuration |
|**Propriété** |**Type** |**Définition**|
|[VendorName](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.vendorname?view=adfs-2019)|Chaîne |Obtient le nom du fournisseur qui possède le plug-in|
|[ModuleIdentifier](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.moduleidentifier?view=adfs-2019)|Chaîne |Obtient l’identificateur du plug-in|

Dans le plug-in de notre exemple, nous utilisons [OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) et [OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019) méthodes pour lire les adresses IP prédéfinie à partir de la base de données AD FS. [OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) est appelée lorsque le plug-in est inscrit avec AD FS lors de la [OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019) est appelée lorsque le fichier .csv est importé à l’aide de la `Import-AdfsThreatDetectionModuleConfiguration` applet de commande. 

#### <a name="irequestreceivedthreatdetectionmodule-interface"></a>IRequestReceivedThreatDetectionModule Interface

Cela [interface](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019) vous permet d’implémenter l’évaluation des risques au niveau du point où AD FS reçoit la demande d’authentification, mais avant que l’utilisateur entre les informations d’identification par exemple, au stade de demande reçu du processus d’authentification. 
 
```
public interface IRequestReceivedThreatDetectionModule
{
Task<ThrottleStatus> EvaluateRequest (
ThreatDetectionLogger logger, 
RequestContext requestContext );
}
```

Inclut l’interface [EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019) transmis à la méthode qui vous permet d’utiliser le contexte de la demande d’authentification dans le paramètre d’entrée requestContext pour écrire votre logique d’évaluation des risques. Le paramètre requestContext est de type [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019). 

L’autre paramètre d’entrée transmis est enregistreur d’événements qui est de type [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). Le paramètre peut être utilisé pour écrire l’erreur, d’audit et/ou les messages dans les journaux d’AD FS de débogage. 

La méthode retourne [ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) (0 si NotEvaluated, 1 au bloc et 2 pour autoriser) à AD FS qui bloque ou autorise la requête.

Dans notre exemple de plug-in, [EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019) implémentation de méthode analyse la [clientIpAddress](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext.clientipaddresses?view=adfs-2019#Microsoft_IdentityServer_Public_ThreatDetectionFramework_RequestContext_ClientIpAddresses) à partir de la [requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019) paramètre et le compare avec toutes les adresses IP chargé à partir de la base de données AD FS. Si une correspondance est trouvée, méthode retourne 2 pour **bloc**, sinon elle retourne 1 pour **autoriser**. Selon la valeur retournée, AD FS bloque ou autorise la requête. 

>[!NOTE]
>L’exemple de plug-in décrit ci-dessus implémente l’interface IRequestReceivedThreatDetectionModule uniquement. Toutefois, le modèle d’évaluation des risques fournit deux autres interfaces – IPreAuthenticationThreatDetectionModule (à implémenter l’étape de pré-authentification au cours de risque évaluation logique) et IPostAuthenticationThreatDetectionModule (à implémenter des risques évaluation de la logique de pendant l’étape d’authentification après). Vous trouverez ci-dessous les détails sur les deux interfaces. 

#### <a name="ipreauthenticationthreatdetectionmodule-interface"></a>IPreAuthenticationThreatDetectionModule Interface 

Cela [interface](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule?view=adfs-2019) vous permet d’implémenter la logique d’évaluation des risques au niveau du point où l’utilisateur fournit les informations d’identification, mais avant que AD FS les évalue l’étape de pré-authentification par exemple. 

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
Inclut l’interface [EvaluatePreAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule.evaluatepreauthentication?view=adfs-2019) transmis à la méthode qui vous permet d’utiliser les informations dans le [RequestContext requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext securityContext ](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), [ProtocolContext protocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019), et [IList<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2) paramètres pour écrire votre logique d’évaluation de l’authentification préalable risque d’entrée. 

>[!NOTE]
>Pour la liste des propriétés transmises avec chaque type de contexte, visitez [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), et [ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019) définitions de classe. 

L’autre paramètre d’entrée transmis est enregistreur d’événements qui est de type [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). Le paramètre peut être utilisé pour écrire l’erreur, d’audit et/ou les messages dans les journaux d’AD FS de débogage.

La méthode retourne [ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) (0 si NotEvaluated, 1 au bloc et 2 pour autoriser) à AD FS qui bloque ou autorise la requête. 

#### <a name="ipostauthenticationthreatdetectionmodule-interface"></a>IPostAuthenticationThreatDetectionModule Interface

Cela [interface](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule?view=adfs-2019) vous permet d’implémenter la logique d’évaluation des risques une fois que l’utilisateur a fourni des informations d’identification et d’AD FS a effectué l’authentification par exemple, après l’authentification phase. 

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

Inclut l’interface [EvaluatePostAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule.evaluatepostauthentication?view=adfs-2019) transmis à la méthode qui vous permet d’utiliser les informations dans le [RequestContext requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext securityContext ](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), [ProtocolContext protocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019), et [IList<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2) paramètres pour écrire votre logique d’évaluation post-Authentication risque d’entrée. 

>[!NOTE]
> Pour obtenir une liste complète des propriétés transmises avec chaque type de contexte, consultez [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), et [ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019) définitions de classe. 

L’autre paramètre d’entrée transmis est enregistreur d’événements qui est de type [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). Le paramètre peut être utilisé pour écrire l’erreur, d’audit et/ou les messages dans les journaux d’AD FS de débogage. 

La méthode retourne la [Score de risque](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.authentication.riskscoreconstants?view=adfs-2019) qui peut être utilisé dans la stratégie d’AD FS et les règles de revendication. 

>[!NOTE]
>Pour plug-in fonctionne, la classe principale (dans ce cas UserRiskAnalyzer) doit dériver [ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019) classe abstraite et doit implémenter au moins une des trois interfaces décrites ci-dessus. Une fois que la dll est inscrit, AD FS vérifie parmi les interfaces sont implémentées et les appels à l’étape appropriée dans le pipeline.

### <a name="faqs"></a>FAQ

**Pourquoi dois-je créer ces plug-ins ?**</br>
**R :** Ces plug-ins non seulement vous fournissent des fonctions supplémentaires pour sécuriser votre environnement contre les attaques telles que les attaques de mot de passe pulvérisation, mais vous donnent également la possibilité de créer votre propre logique d’évaluation des risques selon vos besoins. 

**Où les journaux sont capturés ?**</br>
**R :** Vous pouvez écrire des journaux d’erreurs dans le journal des événements « AD FS/Admin » à l’aide de la méthode de WriteAdminLogErrorMessage, les journaux d’audit pour la sécurité de « AD FS audit » se connecter à l’aide de la méthode de WriteAuditMessage et les journaux pour le journal de débogage « AD FS suivi » à l’aide de la méthode de WriteDebugMessage de débogage. 

**Peut ajouter ces plug-ins augmenter la latence du processus d’authentification AD FS ?**</br>
**R :** Latence impact sera déterminé par le temps nécessaire pour exécuter la logique d’évaluation des risques que vous implémentez. Nous vous recommandons d’évaluer l’impact de la latence avant de déployer le plug-in dans l’environnement de production. 

**Pourquoi AD FS ne peut pas proposer la liste des adresses IP à risque, les utilisateurs, etc. ?**</br>
**R :** Bien que pas actuellement disponible, nous travaillons sur la création de l’intelligence pour suggérer des adresses IP à risque, les utilisateurs, etc. dans le modèle d’évaluation des risques enfichables. Nous partagerons dates dès le lancement. 
