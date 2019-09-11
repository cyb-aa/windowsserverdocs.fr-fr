---
title: Utilisation de PowerShell dans votre extension
description: Utilisation de PowerShell dans votre extension SDK du centre d’administration Windows (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 05/09/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: c30f8a9b856db8250a16210931e6f8dd73c07aa7
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869610"
---
# <a name="using-powershell-in-your-extension"></a>Utilisation de PowerShell dans votre extension #

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Voyons plus en détail le kit de développement logiciel (SDK) des extensions du centre d’administration Windows : parlons de l’ajout de commandes PowerShell à votre extension.

## <a name="powershell-in-typescript"></a>PowerShell dans une machine à écrire ##

Le processus de génération Gulp a une étape de génération qui prendra ```{!ScriptName}.ps1``` tout ce qui est placé ```\src\resources\scripts``` dans le dossier et les générera dans la ```\src\generated``` ```powershell-scripts``` classe sous le dossier.

>[!NOTE] 
> Ne mettez pas à ```powershell-scripts.ts``` jour manuellement ```strings.ts``` les fichiers ni. Toute modification apportée sera remplacée lors de la prochaine génération.

## <a name="running-a-powershell-script"></a>Exécution d’un script PowerShell ##
Vous pouvez placer ```\src\resources\scripts\{!ScriptName}.ps1```tous les scripts que vous souhaitez exécuter sur un nœud. 
>[!IMPORTANT] 
> Toute modification apportée ```{!ScriptName}.ps1``` à un fichier ne sera pas reflétée dans ```gulp generate``` votre projet tant que n’a pas été exécuté.

L’API fonctionne en créant d’abord une session PowerShell sur les nœuds que vous ciblez, en créant le script PowerShell avec tous les paramètres qui doivent être transmis, puis en exécutant le script sur les sessions qui ont été créées.

Par exemple, nous avons ce script ```\src\resources\scripts\Get-NodeName.ps1```:
``` ps1
Param
 (
    [String] $stringFormat
 )
 $nodeName = [string]::Format($stringFormat,$env:COMPUTERNAME)
 Write-Output $nodeName
```

Nous allons créer une session PowerShell pour le nœud cible :
``` ts
const session = this.appContextService.powerShell.createSession('{!TargetNode}'); 
```
Ensuite, nous allons créer le script PowerShell avec un paramètre d’entrée :
```ts
const script = PowerShell.createScript(PowerShellScripts.Get_NodeName, {stringFormat: 'The name of the node is {0}!'});
```
Enfin, nous devons exécuter ce script dans la session que vous avez créée :
``` ts
  public ngOnInit(): void {
    this.session = this.appContextService.powerShell.createAutomaticSession('{!TargetNode}');
  }

  public getNodeName(): Observable<any> {
    const script = PowerShell.createScript(PowerShellScripts.Get_NodeName.script, { stringFormat: 'The name of the node is {0}!'});
    return this.appContextService.powerShell.run(this.session, script)
    .pipe(
        map(
        response => {
            if (response && response.results) {
                return response.results;
            }
            return 'no response';
        }
      ) 
    );
  }

  public ngOnDestroy(): void {
    this.session.dispose()
  }

```
À présent, nous devons vous abonner à la fonction observable que nous venons de créer. Placez-le là où vous devez appeler la fonction pour exécuter le script PowerShell :
```ts
this.getNodeName().subscribe(
     response => {
    console.log(response)
     }
);
```
En fournissant le nom de nœud à la méthode createSession, une nouvelle session PowerShell est créée, utilisée, puis immédiatement détruite à la fin de l’appel PowerShell. 

### <a name="key-options"></a>Options de clé ###
Certaines options sont disponibles lors de l’appel de l’API PowerShell. Chaque fois qu’une session est créée, elle peut être créée avec ou sans clé. 

**Essentiel** Cela crée une session de clé qui peut être recherchée et réutilisée, même sur plusieurs composants (ce qui signifie que Composant1 peut créer une session avec la clé « SME-ROCKs » et COMPONENT2 peut utiliser cette même session). Si une clé est fournie, la session créée doit être supprimée en appelant dispose () comme indiqué dans l’exemple ci-dessus. Une session ne doit pas être conservée pendant plus de 5 minutes. 
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

**Sans clé** Une clé est automatiquement créée pour la session. Cette session est supprimée automatiquement après 3 minutes. L’utilisation de l’utilisation minimale de la clé permet à votre extension de recycler l’utilisation d’une instance d’exécution qui est déjà disponible au moment de la création d’une session. Si aucune instance d’exécution n’est disponible, une nouvelle instance sera créée. Cette fonctionnalité est utile pour les appels uniques, mais l’utilisation répétée peut affecter les performances. Une session prend environ 1 seconde à créer, donc les sessions de recyclage continue peuvent entraîner des ralentissements.

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
ou Gestionnaire de configuration 
``` ts 
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
Dans la plupart des cas, créez une session à ```ngOnInit()``` clé dans la méthode, puis supprimez ```ngOnDestroy()```-la dans. Suivez ce modèle quand il existe plusieurs scripts PowerShell dans un composant, mais que la session sous-jacente n’est pas partagée entre les composants.
Pour de meilleurs résultats, assurez-vous que la création de session est gérée à l’intérieur de composants plutôt qu’en tant que services. cela permet de garantir la gestion de la durée de vie et du nettoyage.

Pour de meilleurs résultats, assurez-vous que la création de session est gérée à l’intérieur de composants plutôt qu’en tant que services. cela permet de garantir la gestion de la durée de vie et du nettoyage.

### <a name="powershell-stream"></a>Flux PowerShell ###
Si vous avez un script de longue durée et que les données sont générées progressivement, un flux PowerShell vous permet de traiter les données sans avoir à attendre la fin du script. L’observable Next () sera appelée dès la réception des données.
```ts
this.appContextService.powerShellStream.run(session, script);
```

### <a name="long-running-scripts"></a>Scripts à long terme ###
Si vous avez un script à exécution longue que vous souhaitez exécuter en arrière-plan, un élément de travail peut être envoyé. L’état du script est suivi par la passerelle et les mises à jour de l’État peuvent être envoyées à une notification. 
```ts
const workItem: WorkItemSubmitRequest = {
    typeId: 'Long Running Script',
    objectName: 'My long running service',
    powerShellScript: script,
    
    //in progress notifications
    inProgressTitle: 'Executing long running request',
    startedMessage: 'The long running request has been started',
    progressMessage: 'Working on long running script – {{ percent }} %',

    //success notification
    successTitle: 'Successfully executed a long running script!',
    successMessage: '{{objectName}} was successful',
    successLinkText: 'Bing',
    successLink: 'http://www.bing.com',
    successLinkType: NotificationLinkType.Absolute,

    //error notification
    errorTitle: 'Failed to execute long running script',
    errorMessage: 'Error: {{ message }}'

    nodeRequestOptions: {
       logAudit: true,
       logTelemetry: true
    }
};

return this.appContextService.workItem.submit('{!TargetNode}', workItem);
```

>[!NOTE] 
> Pour que la progression soit affichée, la progression de l’écriture doit être incluse dans le script que vous avez écrit. Exemple :
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!' -percentComplete 95
>```

#### <a name="workitem-options"></a>Options d’élément de travail ####

| function | Explication |
| ----- | ----------- |
| Envoyer () | Envoi de l’élément de travail 
| submitAndWait() | Envoyer l’élément de travail et attendre la fin de son exécution
| Wait () | Attendre la fin de l’élément de travail existant
| requête () | Rechercher un élément de travail existant par ID
| Find () | Rechercher un élément de travail existant par TargetNodeName, ModuleName ou typeId.

### <a name="powershell-batch-apis"></a>API de traitement par lots PowerShell ###
Si vous devez exécuter le même script sur plusieurs nœuds, vous pouvez utiliser une session PowerShell par lot. Exemple :
```ts
const batchSession = this.appContextService.powerShell.createBatchSession(
    ['{!TargetNode1}', '{!TargetNode2}', sessionKey);
  this.appContextService.powerShell.runBatchSingleCommand(batchSession, command).subscribe((responses: PowerShellBatchResponseItem[]) => {
    for (const response of responses) { 
      if (response.error || response.errors) {
        //handle error
      } else {
        const results = response.properties && response.properties.results;
        //response.nodeName
        //results[0]
      }
    }
     },
     Error => { /* handle error */ });  

```


#### <a name="powershellbatch-options"></a>Options de PowerShellBatch ####
| Option | Explication |
| ----- | ----------- |
| runSingleCommand | Exécuter une seule commande sur tous les nœuds du tableau 
| exécuter | Exécuter la commande correspondante sur le nœud couplé
| annuler | Annuler la commande sur tous les nœuds du tableau
