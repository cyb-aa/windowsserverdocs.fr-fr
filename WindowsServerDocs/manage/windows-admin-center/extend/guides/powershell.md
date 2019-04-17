---
title: Utilisation de PowerShell dans votre extension
description: À l’aide de PowerShell dans votre extension SDK Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b1be4fe7639d913243cc28371dff9e98e0f5827e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296721"
---
# Utilisation de PowerShell dans votre extension #

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Revenons plus détaillées dans le SDK Windows Admin Center Extensions - parlons sur l’ajout de commandes PowerShell à votre extension.

## PowerShell dans TypeScript ##

Le processus de génération gulp a une étape de génération qui dirigera une ```{!ScriptName}.ps1``` qui est placé dans le ```\src\resources\scripts``` dossier et les intégrer dans le ```powershell-scripts``` classe sous la ```\src\generated``` dossier.

>[!NOTE] 
> Ne pas mettre à jour manuellement les ```powershell-scripts.ts``` ni le ```strings.ts``` fichiers. Toute modification apportée est remplacée lors de la prochaine générer.

## Exécution d’un Script Powerhell ##
Des scripts que vous souhaitez exécuter sur un nœud peuvent être placés dans ```\src\resources\scripts\{!ScriptName}.ps1```. 
>[!IMPORTANT] 
> Toutes les modifications apportez dans une ```{!ScriptName}.ps1``` fichier n’est pas répercutée dans votre projet, sauf si une génération 

L’API fonctionne en créant d’abord une session PowerShell sur les nœuds vous ciblant, créez le script PowerShell avec tous les paramètres qui doivent être transmis dans et puis en exécutant le script sur les sessions qui ont été créées.

Par exemple, nous avons ce script ```\src\resources\scripts\Get-NodeName.ps1```:
``` ps1
Param
 (
    [String] $stringFormat
 )
 $nodeName = [string]::Format($stringFormat,$env:COMPUTERNAME)
 Write-Output $nodeName
```

Nous allons créer une session PowerShell pour notre nœud cible:
``` ts
const session = this.appContextService.powerShell.createSession('{!TargetNode}'); 
```
Ensuite, nous allons créer le script PowerShell avec un paramètre d’entrée:
```ts
const script = PowerShell.createScript(PowerShellScripts.Get_NodeName, {stringFormat: 'The name of the node is {0}!'});
```
Enfin, nous devons exécuter ce script dans la session que nous avons créé:
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
Nous devons maintenant s’abonner à la fonction observable que nous venons de créer. Placez ce où vous devez appeler la fonction pour exécuter le script PowerShell:
```ts
this.getNodeName().subscribe(
     response => {
    console.log(response)
     }
);
```
En fournissant le nom du nœud à la méthode createSession, une nouvelle session PowerShell est créée, utilisée et ensuite immédiatement détruite à la fin de l’appel de PowerShell. 

### Options de clé ###
Plusieurs options sont disponibles lorsque vous appelez l’API de PowerShell. Chaque fois qu’une session est créée il peut être créé avec ou sans une clé. 

**Clé:** Cela crée une session à clé qui peut être recherchée et réutilisée, même sur les composants (ce qui signifie que Component1 permet de créer une session avec la clé «PME-ROCHES», et Component2 peuvent utiliser cette même session). Si une clé est fournie, la session qui est créée doit être supprimée en appelant dispose() comme a été effectué dans l’exemple ci-dessus. Une session ne doit pas être conservée sans en cours de suppression de plus de 5 minutes. 
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

**Sans clé:** Une clé sera automatiquement créée pour la session. Cette session avec être supprimés automatiquement au bout de 3 minutes. À l’aide de sans clé permet à votre extension recycler l’utilisation de n’importe quel Runspace qui est déjà disponible au moment de la création d’une session. Si aucun Runspace n’est disponible à une autre sera créée. Cette fonctionnalité est adaptée aux appels uniques, mais l’utilisation répétée peut affecter les performances. Une session prend environ 1 seconde pour créer, par conséquent, en permanence recyclage sessions pouvant entraîner ralentissement.

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
ou 
``` ts 
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
Dans la plupart des situations, créez une session à clé dans le ```ngOnInit()``` méthode et puis dispose de celui-ci dans ```ngOnDestroy()```. Suivez ce modèle lorsqu’il existe plusieurs scripts PowerShell dans un composant, mais la session sous-jacente n’est pas partagé entre les composants.
Pour obtenir de meilleurs résultats, assurez-vous que la création de la session est gérée à l’intérieur des composants au lieu des services - cette permet de garantir que durée de vie et nettoyage peut être géré correctement.

Pour obtenir de meilleurs résultats, assurez-vous que la création de la session est gérée à l’intérieur des composants au lieu des services - cette permet de garantir que durée de vie et nettoyage peut être géré correctement.

### Flux de PowerShell ###
Si vous disposez d’un script et les données longue est sortie progressivement, qu'un flux de PowerShell vous permettra de traiter les données sans avoir à attendre que le script se termine. Le () observable suivant est appelée dès la réception de données.
```ts
this.appContextService.powerShellStream.run(session, script);
```

### Durée pendant laquelle les Scripts en cours d’exécution ###
Si vous disposez d’un durée pendant laquelle l’exécution du script que vous souhaitez exécuter en arrière-plan, un élément de travail peut être soumis. L’état du script est suivie par la passerelle et mises à jour de l’état peuvent être envoyées à une notification. 
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
> Pour la progression s’affiche, Write-Progress doit être inclus dans le script que vous avez écrit. Exemple :
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!’ -percentComplete 95
>```

#### Options de l’élément de travail ####

| function | Explication |
| ----- | ----------- |
| Submit() | Envoie l’élément de travail 
| submitAndWait() | Envoyer l’élément de travail et attendez la fin de son exécution
| Wait() | Attendre de travail existante élément pour effectuer
| Query() | Requête pour un élément de travail existant par id
| Find() | Recherchez et existant d’élément de travail par le TargetNodeName, NomModule ou typeId.

### API de commandes PowerShell n°
Si vous avez besoin exécuter le script même sur plusieurs nœuds, une session PowerShell de traitement par lots peut être utilisée. Exemple :
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


#### Options de PowerShellBatch ####
| option | Explication |
| ----- | ----------- |
| runSingleCommand | Exécuter une commande unique sur tous les nœuds dans le tableau 
| Courir | Exécutez commande correspondant sur le nœud associé
| Annuler | Annuler la commande sur tous les nœuds dans le tableau