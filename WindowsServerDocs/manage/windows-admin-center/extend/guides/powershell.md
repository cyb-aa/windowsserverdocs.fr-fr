---
title: Utilisation de PowerShell dans votre extension
description: À l’aide de PowerShell dans votre extension pour le Kit de développement Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 05/09/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 7375732fd464519cd1533043d271065e488fd46a
ms.sourcegitcommit: 7cb939320fa2613b7582163a19727d7b77debe4b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621359"
---
# <a name="using-powershell-in-your-extension"></a>Utilisation de PowerShell dans votre extension #

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Revenons plus approfondie dans le Kit de développement logiciel Windows Admin Center Extensions - parlons à présent ajouter des commandes PowerShell à votre extension.

## <a name="powershell-in-typescript"></a>PowerShell dans TypeScript ##

Le processus de génération gulp a une étape de génération qui prendra un ```{!ScriptName}.ps1``` qui est placé dans le ```\src\resources\scripts``` dossier et de les générer dans le ```powershell-scripts``` classe sous le ```\src\generated``` dossier.

>[!NOTE] 
> Ne pas mettre à jour manuellement la ```powershell-scripts.ts``` ni le ```strings.ts``` fichiers. Toute modification apportée est remplacée lors de la génération suivante.

## <a name="running-a-powershell-script"></a>Exécution d’un Script PowerShell ##
Tous les scripts que vous souhaitez exécuter sur un nœud peuvent être placés dans ```\src\resources\scripts\{!ScriptName}.ps1```. 
>[!IMPORTANT] 
> Toutes les modifications apportez dans un ```{!ScriptName}.ps1``` fichier est répercutée dans votre projet jusqu'à ce que ```gulp generate``` a été exécuté.

L’API fonctionne en créant d’abord une session PowerShell sur les nœuds vous ciblant, créez le script PowerShell avec tous les paramètres qui doivent être transmis et puis en exécutant le script sur les sessions qui ont été créées.

Par exemple, nous avons ce script ```\src\resources\scripts\Get-NodeName.ps1```:
``` ps1
Param
 (
    [String] $stringFormat
 )
 $nodeName = [string]::Format($stringFormat,$env:COMPUTERNAME)
 Write-Output $nodeName
```

Nous allons créer une session PowerShell pour notre nœud cible :
``` ts
const session = this.appContextService.powerShell.createSession('{!TargetNode}'); 
```
Ensuite, nous allons créer le script PowerShell avec un paramètre d’entrée :
```ts
const script = PowerShell.createScript(PowerShellScripts.Get_NodeName, {stringFormat: 'The name of the node is {0}!'});
```
Enfin, nous devons exécuter ce script dans la session que nous avons créé :
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
Il est possible que nous devons maintenant vous abonner à la fonction observable que nous venons de créer. Placez ce où vous devez appeler la fonction pour exécuter le script PowerShell :
```ts
this.getNodeName().subscribe(
     response => {
    console.log(response)
     }
);
```
En fournissant le nom du nœud à la méthode createSession, une nouvelle session PowerShell est créée, utilisée, puis immédiatement détruite à l’achèvement de l’appel de PowerShell. 

### <a name="key-options"></a>Options de clé ###
Quelques options sont disponibles lors de l’appel de l’API PowerShell. Chaque fois qu’une session est créée. il peut être créé avec ou sans une clé. 

**Clé :** Cette opération crée une session à clé qui peut être recherchée et réutilisée, même à travers les composants (ce qui signifie que composant1 peut créer une session avec la clé « SME ROCKS » et Component2 peut utiliser cette même session). Si une clé est fournie, la session est créée doit être éliminée par appelant dispose() comme dans l’exemple ci-dessus. Une session ne doit pas être conservée sans mis au rebut pendant plus de 5 minutes. 
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

**Sans clé :** Une clé est automatiquement créée pour la session. Cette session avec être supprimé automatiquement après 3 minutes. Sans clé grâce à votre extension recycler l’utilisation de toute instance d’exécution qui est déjà disponible au moment de la création d’une session. Si aucune instance d’exécution n’est disponible qu’un nouveau sera créé. Cette fonctionnalité est valable pour les appels uniques, mais une utilisation répétée peut affecter les performances. Une session prend environ 1 seconde, donc en permanence recyclage sessions peut entraîner des ralentissements.

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
ou Gestionnaire de configuration 
``` ts 
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
Dans la plupart des cas, créez une session à clé dans le ```ngOnInit()``` (méthode) et puis supprimez-la dans ```ngOnDestroy()```. Suivez ce modèle lorsqu’il y a plusieurs scripts PowerShell dans un composant, mais la session sous-jacente n’est pas partagé entre plusieurs composants.
Pour de meilleurs résultats, vérifiez que la création de session est gérée à l’intérieur des composants plutôt que des services - ce vous permet de vous assurer que durée de vie et le nettoyage peut être géré correctement.

Pour de meilleurs résultats, vérifiez que la création de session est gérée à l’intérieur des composants plutôt que des services - ce vous permet de vous assurer que durée de vie et le nettoyage peut être géré correctement.

### <a name="powershell-stream"></a>PowerShell Stream ###
Si vous avez un script et les données en cours d’exécution longue est sortie progressivement, qu'un flux de PowerShell vous permettra de traiter les données sans avoir à attendre que le script se termine. L’observable next() sera appelée dès que les données sont reçues.
```ts
this.appContextService.powerShellStream.run(session, script);
```

### <a name="long-running-scripts"></a>Durée pendant laquelle les Scripts en cours d’exécution ###
Si vous avez un script long terme que vous souhaitez exécuter en arrière-plan, un élément de travail peut être soumis. L’état du script sera suivie par la passerelle et les mises à jour de l’état peuvent être envoyés à une notification. 
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
> Pour connaître la progression à afficher, Write-Progress doit être inclus dans le script que vous avez écrit. Exemple :
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!’ -percentComplete 95
>```

#### <a name="workitem-options"></a>Options de l’élément de travail ####

| function | Explication |
| ----- | ----------- |
| submit() | Envoie l’élément de travail 
| submitAndWait() | Envoyer l’élément de travail et attendre la fin de son exécution
| wait() | Attente de travail existant élément pour terminer
| query() | Requête pour un élément de travail existant par id
| find() | Recherchez et existant d’élément de travail par TargetNodeName, ModuleName ou typeId.

### <a name="powershell-batch-apis"></a>API PowerShell Batch ###
Si vous avez besoin exécuter le même script sur plusieurs nœuds, une session PowerShell de traitement par lots peut être utilisée. Exemple :
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


#### <a name="powershellbatch-options"></a>PowerShellBatch options ####
| option | Explication |
| ----- | ----------- |
| runSingleCommand | Exécuter une commande unique par rapport à tous les nœuds dans le tableau 
| exécuter | Exécuter la commande correspondante sur le nœud associé
| annuler | Annuler la commande sur tous les nœuds dans le tableau
