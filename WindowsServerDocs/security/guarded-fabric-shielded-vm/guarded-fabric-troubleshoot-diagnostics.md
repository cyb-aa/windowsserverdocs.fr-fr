---
title: Résolution des problèmes à l’aide de l’outil de Diagnostic de structure protégée
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 07691d5b-046c-45ea-8570-a0a85c3f2d22
manager: dongill
author: huu
ms.technology: security-guarded-fabric
ms.openlocfilehash: 0fb257f693cc27c0bc6dd18fc89e8dc6328ee638
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447343"
---
# <a name="troubleshooting-using-the-guarded-fabric-diagnostic-tool"></a>Résolution des problèmes à l’aide de l’outil de Diagnostic de structure protégée

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit l’utilisation de la service Guardian Fabric outil de Diagnostic pour identifier et corriger les erreurs courantes dans le déploiement, la configuration et le fonctionnement de l’infrastructure protégée. Cela inclut le Service de surveillance d’hôte (SGH), tous les hôtes service Guardian et prise en charge des services tels que DNS et Active Directory. L’outil de diagnostic peut être utilisé pour effectuer un premier passage au triage une structure échec service Guardian, offrant aux administrateurs un point de départ pour la résolution des pannes et l’identification des ressources mal configurés. L’outil n’est pas un remplacement pour une compréhension solide de fonctionnement d’une structure protégée et sert uniquement à vérifier rapidement les problèmes les plus courants rencontrés pendant des opérations quotidiennes.

Vous trouverez la documentation des applets de commande utilisées dans cette rubrique sur [TechNet](https://technet.microsoft.com/library/mt718834.aspx).

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

# <a name="quick-start"></a>Démarrage rapide de la

Vous pouvez diagnostiquer un hôte service Guardian ou un nœud SGH en appelant ce qui suit à partir d’une session Windows PowerShell avec des privilèges d’administrateur local :
```PowerShell
Get-HgsTrace -RunDiagnostics -Detailed
```
Cela sera automatiquement détecter le rôle de l’hôte actuel et diagnostiquer les problèmes pertinents qui peuvent être détectés automatiquement.  Tous les résultats générés au cours de ce processus sont affichés en raison de la présence de la `-Detailed` basculer.

Le reste de cette rubrique fournit une procédure pas à pas détaillées sur l’utilisation avancée de `Get-HgsTrace` pour faire les choses telles que le diagnostic plusieurs hôtes à la fois et la détection d’une configuration incorrecte de complexe entre nœuds.

## <a name="diagnostics-overview"></a>Vue d’ensemble des Diagnostics
Structure protégée diagnostics sont disponibles sur n’importe quel hôte avec l’ordinateur virtuel protégé liés des outils et fonctionnalités installés, y compris les ordinateurs hôtes exécutant Server Core.  Actuellement, les diagnostics sont inclus avec les fonctionnalités/packages suivants :

1. Rôle de Service Guardian hôte
2. Support Hyper-V pour Guardian hôte
3. Outils de protection d'ordinateur virtuel pour la gestion d'infrastructure
4. Outils d’administration de serveur distant (RSAT)

Cela signifie que les outils de diagnostic seront disponibles sur tous les hôtes service Guardian, SGH nœuds, certains serveurs de gestion d’infrastructure et des stations de travail Windows 10 avec [RSAT](https://www.microsoft.com/download/details.aspx?id=45520) installé.  Diagnostics peuvent être appelées à partir de tous les ordinateurs ci-dessus avec l’intention de diagnostic n’importe quel hôte service Guardian ou le nœud de SGH dans une structure protégée ; à l’aide de cibles de trace à distance, diagnostics peut localiser et se connecter aux hôtes autres que l’ordinateur qui exécute les tests de diagnostic.

Chaque hôte ciblé par les diagnostics est appelé une « cible de trace ».  Cibles de trace sont identifiés par leurs noms d’hôtes et les rôles.  Rôles décrivent la fonction effectue d’une cible de suivi spécifiée dans une infrastructure protégée.  Actuellement, la trace cible prise en charge `HostGuardianService` et `GuardedHost` rôles.  Notez qu’il est possible pour un ordinateur hôte occuper plusieurs rôles à la fois, et cela est également pris en charge par les diagnostics, mais cela ne doit pas être effectuée dans les environnements de production.  Les hôtes SGH et Hyper-V doivent être maintenues séparés et distinct à tout moment.

Les administrateurs peuvent commencer les tâches de diagnostic en exécutant `Get-HgsTrace`.  Cette commande effectue deux fonctions distinctes basées sur les commutateurs fournis lors de l’exécution : collecte et diagnostic de la trace.  Ces deux combinés constituent l’intégralité de l’outil de Diagnostic Fabric service Guardian.  Bien que pas explicitement requises, des diagnostics plus utiles nécessitent des traces qui peuvent uniquement être collectés avec les informations d’identification d’administrateur sur la cible de la trace.  Si des privilèges suffisants sont détenus par l’utilisateur qui exécute la collecte des traces, traces nécessitant une élévation échoue alors que toutes les autres passera.  Cela permet de diagnostic partielle dans un opérateur de privilège excessif effectue triage l’événement. 

### <a name="trace-collection"></a>Collecte des traces
Par défaut, `Get-HgsTrace` sera uniquement collecter les traces et les enregistrer dans un dossier temporaire.  Traces prennent la forme d’un dossier, nommé d’après l’hôte ciblé, rempli avec des fichiers spécialement mise en forme qui décrivent la configuration de l’hôte.  Les traces contiennent également des métadonnées qui décrivent la façon dont les tests de diagnostic ont été appelés pour collecter les traces.  Ces données sont utilisées par les diagnostics pour rafraîchir les informations sur l’hôte lors de l’exécution de diagnostic manuel.

Si nécessaire, les traces peuvent être consultés manuellement.  Tous les formats sont soit explicite (XML) ou peuvent être aisément inspectés à l’aide d’outils standard (par exemple, X509 certificats et les Extensions de Shell Windows Crypto).  Notez toutefois que les traces ne sont pas conçus pour le diagnostic manuel et il est toujours plus efficace de traiter les traces avec les fonctionnalités de diagnostic de `Get-HgsTrace`.

Les résultats de la collecte des traces en cours d’exécution n’apportez aucune indication de l’intégrité d’un hôte donné.  Elles indiquent simplement que les traces ont été collectées avec succès.  Il est nécessaire d’utiliser les fonctionnalités de diagnostic de `Get-HgsTrace` pour déterminer si les suivis indiquent un environnement défaillant.

À l’aide de le `-Diagnostic` paramètre, vous pouvez limiter la collecte de trace pour uniquement ces traces nécessaires au fonctionnement des diagnostics spécifiés.  Cela réduit la quantité de données collectées, ainsi que les autorisations requises pour appeler des diagnostics.

### <a name="diagnosis"></a>Diagnostic
Traces collectées peuvent être diagnostiqués en condition `Get-HgsTrace` l’emplacement des traces via le `-Path` paramètre et en spécifiant le `-RunDiagnostics` basculer.  En outre, `Get-HgsTrace` réalisables collecte et diagnostic dans un seul passage en fournissant le `-RunDiagnostics` commutateur et une liste de cibles de la trace.  Si aucune cible de trace n’est fournis, l’ordinateur actuel est utilisé en tant qu’implicite cible, avec son rôle déduit en examinant les modules Windows PowerShell installés.

Diagnostic fournira les résultats dans un format hiérarchique affichant les cibles de la trace, les jeux de diagnostic et des diagnostics individuels sont chargés pour un problème particulier.  Défaillances incluent la mise à jour et les solutions recommandées si une détermination permettre être réalisée corrélativement quelle action à entreprendre suivante.  Par défaut, les résultats en passant et non pertinentes sont masquées.  Pour voir tous les éléments testés par les diagnostics, spécifiez la `-Detailed` basculer.  Ainsi, tous les résultats s’affichent, quel que soit leur état.

Il est possible de restreindre l’ensemble des diagnostics qui sont exécutés en utilisant le `-Diagnostic` paramètre.  Cela vous permet de spécifier des classes de diagnostic doivent être exécutés sur les cibles de la trace et en supprimant toutes les autres.  Les classes de diagnostics disponibles exemples de mise en réseau, les meilleures pratiques et les clients matériel.  Consultez le [documentation de l’applet de commande](https://technet.microsoft.com/library/mt718831.aspx) pour rechercher une liste actualisée des tests de diagnostic disponibles.

> [!WARNING]
> Diagnostics ne sont pas un substitut pour un pipeline de réponse aux incidents et de surveillance strong.  Un package de System Center Operations Manager est disponible pour la surveillance des infrastructures protégées, ainsi que différents canaux de journal des événements qui peuvent être analysés pour détecter les problèmes au plus tôt.  Diagnostics peut ensuite servir à trier ces échecs rapidement et d’établir un plan d’action.

## <a name="targeting-diagnostics"></a>Ciblage des Diagnostics

`Get-HgsTrace` fonctionne par rapport aux cibles de la trace.  Une cible de la trace est un objet qui correspond à un nœud SGH ou un hôte service Guardian à l’intérieur d’une infrastructure protégée.  Elle peut être considérée comme une extension à un `PSSession` qui inclut des informations requises uniquement par les diagnostics tels que le rôle de l’hôte dans l’infrastructure.  Cibles peuvent être généré de manière implicite (par exemple, diagnostic local ou manuelle) ou explicitement avec le `New-HgsTraceTarget` commande.

### <a name="local-diagnosis"></a>Diagnostic local

Par défaut, `Get-HgsTrace` ciblera l’hôte local (par exemple, où l’applet de commande est appelée).  Il s’agit en tant que la cible locale implicite.  La cible locale implicite est utilisée uniquement lorsque aucune cible n’est fournis dans le `-Target` paramètre et aucune trace préexistant se trouvent dans le `-Path`.

La cible locale implicite utilise l’inférence de rôle pour déterminer le rôle joué par l’hôte actuel dans la structure protégée.  Cela est basée sur les modules Windows PowerShell installés qui correspondant à peu près à quelles fonctionnalités ont été installées sur le système.  La présence de la `HgsServer` module entraîne la cible de la trace jouent le rôle `HostGuardianService` et la présence de la `HgsClient` module entraîne la cible de la trace jouent le rôle `GuardedHost`.  Il est possible pour un hôte donné pour que les deux modules présents dans ce cas, il est traité à la fois comme un `HostGuardianService` et un `GuardedHost`.

Par conséquent, l’appel par défaut de diagnostics pour la collecte des traces localement :
```PowerShell
Get-HgsTrace
```
... est équivalente à la suivante :
```PowerShell
New-HgsTraceTarget -Local | Get-HgsTrace
```
> [!TIP]
> `Get-HgsTrace` peut accepter des cibles via le pipeline ou directement via le `-Target` paramètre.  Il n’existe aucune différence entre les deux sur le plan opérationnel.

### <a name="remote-diagnosis-using-trace-targets"></a>Cibles de Trace à l’aide de diagnostic à distance

Il est possible de diagnostiquer à distance un ordinateur hôte en générant les cibles de trace avec les informations de connexion à distance.  Tout ce qui est nécessaire est le nom d’hôte et un ensemble d’informations d’identification capables de se connecter à l’aide de la communication à distance de Windows PowerShell.
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $server
```
Cet exemple génère une invite pour collecter les informations d’identification de l’utilisateur distant, et puis diagnostics exécutera à l’aide de l’hôte distant à `hgs-01.secure.contoso.com` pour terminer la collecte des traces.  Les traces qui en résulte sont téléchargées vers l’hôte local et puis diagnostiqués.  Les résultats de diagnostic sont présentés identique lors de l’exécution [diagnostic local](#local-diagnosis).  De même, il n’est pas nécessaire de spécifier un rôle comme il peut être déduit basé sur les modules Windows PowerShell sont installés sur le système distant.

Diagnostic à distance utilise la communication à distance de Windows PowerShell pour tous les accès à l’hôte distant.  Par conséquent, il est indispensable que la cible de la trace a activé de la communication à distance Windows PowerShell (voir [PSRemoting activer](https://technet.microsoft.com/library/hh849694.aspx)) et que l’hôte local est correctement configuré pour le lancement des connexions à la cible.

> [!NOTE]
> Dans la plupart des cas, il est uniquement nécessaire que l’hôte local soit une partie de la même forêt Active Directory et qu’un nom d’hôte DNS valide est utilisé.  Si votre environnement utilise un modèle de fédération plus compliqué ou que vous souhaitez utiliser des adresses IP directe pour la connectivité, vous devrez peut-être effectuer une configuration supplémentaire telles que la définition de la WinRM [hôtes approuvés](https://technet.microsoft.com/library/ff700227.aspx).

Vous pouvez vérifier qu’une cible de la trace est correctement instanciée et configurée pour accepter les connexions à l’aide de la `Test-HgsTraceTarget` applet de commande :
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
$server | Test-HgsTraceTarget
```
Cette commande renvoie `$True` si et seulement si `Get-HgsTrace` serait en mesure d’établir une session de diagnostique à distance avec la cible de la trace.  En cas d’échec, cette applet de commande renvoie les informations d’état pertinentes de dépannage de la connexion de communication à distance de Windows PowerShell.

#### <a name="implicit-credentials"></a>Informations d’identification implicites

Lorsque vous effectuez des diagnostics à distance à partir d’un utilisateur disposant de privilèges suffisants pour se connecter à distance à la cible de la trace, il n’est pas nécessaire de fournir des informations d’identification à `New-HgsTraceTarget`.  Le `Get-HgsTrace` applet de commande réutilise automatiquement les informations d’identification de l’utilisateur qui a appelé l’applet de commande lors de l’ouverture d’une connexion.

> [!WARNING]
> Certaines restrictions s’appliquent à la réutilisation des informations d’identification, en particulier lorsque vous effectuez ce qui est appelé « second tronçon ».  Cela se produit lorsque vous tentez de réutiliser les informations d’identification à l’intérieur d’une session à distance sur un autre ordinateur.  Il est nécessaire de [configurer CredSSP](https://technet.microsoft.com/library/hh849872.aspx) pour prendre en charge ce scénario, mais il se trouve en dehors de l’étendue de gestion de structure protégée et résolution des problèmes.

#### <a name="using-windows-powershell-just-enough-administration-jea-and-diagnostics"></a>À l’aide de Windows PowerShell Just Enough Administration (JEA) et les Diagnostics

Diagnostic à distance prend en charge l’utilisation de points de terminaison JEA avec contraintes de PowerShell de Windows. Par défaut, les cibles de traçage à distance se connectera à l’aide de la valeur par défaut `microsoft.powershell` point de terminaison.  Si la cible de la trace a la `HostGuardianService` rôle, il peut aussi essayer d’utiliser le `microsoft.windows.hgs` point de terminaison qui est configuré lorsque SGH est installé.

Si vous souhaitez utiliser un point de terminaison personnalisé, vous devez spécifier le nom de configuration de session lors de la construction de la cible de la trace à l’aide du `-PSSessionConfigurationName` paramètre, comme ci-dessous :

```PowerShell
New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential) -PSSessionConfigurationName "microsoft.windows.hgs"
```

#### <a name="diagnosing-multiple-hosts"></a>Diagnostic de plusieurs hôtes

Vous pouvez passer plusieurs cibles de trace à `Get-HgsTrace` à la fois.  Cela inclut une combinaison de cibles locaux et distants.  Chaque cible va être tracée à son tour, puis suivis à partir de chaque cible seront diagnostiquées simultanément.  L’outil de diagnostic peut utiliser la base de connaissances accrue de votre déploiement pour identifier les erreurs de configuration entre nœuds complexes qui sinon ne serait pas détectables.  À l’aide de cette fonctionnalité, il suffit en fournissant des traces à partir de plusieurs hôtes simultanément (en cas de diagnostic manuel) ou en ciblant plusieurs héberge lors de l’appel `Get-HgsTrace` (dans le cas de diagnostic à distance).

Voici un exemple d’utilisation de diagnostic à distance à une structure composée de deux nœuds de service SGH de triage et deux hôtes service Guardian, où un des hôtes service Guardian est utilisé pour lancer `Get-HgsTrace`.

```PowerShell
$hgs01 = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Credential (Enter-Credential)
$hgs02 = New-HgsTraceTarget -HostName "hgs-02.secure.contoso.com" -Credential (Enter-Credential)
$gh01 = New-HgsTraceTarget -Local
$gh02 = New-HgsTraceTarget -HostName "guardedhost-02.contoso.com"
Get-HgsTrace -Target $hgs01,$hgs02,$gh01,$gh02 -RunDiagnostics
```

> [!NOTE]
> Vous n’avez pas besoin diagnostiquer votre infrastructure protégée entière lors du diagnostic de plusieurs nœuds.  Dans de nombreux cas, il suffit d’inclure tous les nœuds qui peuvent être impliqués dans une situation d’échec donnée.  C’est généralement un sous-ensemble des hôtes service Guardian et un certain nombre de nœuds du cluster SGH.

## <a name="manual-diagnosis-using-saved-traces"></a>Diagnostic manuel à l’aide des Traces enregistrées

Vous pouvez être amené à réexécuter les diagnostics sans collecter des traces à nouveau, ou vous n’avez pas les informations d’identification nécessaires à distance diagnostiquer tous les hôtes dans votre infrastructure simultanément.  Diagnostic manuel est un mécanisme par lequel vous pouvez encore effectuer un triage de fabric dans son ensemble à l’aide `Get-HgsTrace`, mais sans utiliser de collecte des traces à distance.

Avant d’effectuer le diagnostic manuel, vous devez vérifier les administrateurs de chaque ordinateur hôte dans l’infrastructure qui sera triée sont prêts et prêt à exécuter des commandes à votre place.  Sortie de trace de diagnostic n’expose pas toutes les informations qui sont généralement affichées comme sensibles, mais il incombe à l’utilisateur pour déterminer si elle est fiable d’exposer ces informations à d’autres personnes.

> [!NOTE]
> Les traces ne sont pas anonymes et révèlent configuration réseau, des paramètres de l’infrastructure à clé publique et autre configuration qui est parfois considérée informations privées.  Par conséquent, les traces doivent uniquement être transmises à des entités approuvées au sein d’une organisation et jamais publiée publiquement.

Étapes à effectuer un diagnostic manuel sont les suivantes :

1. Demande qui s’exécutent chaque administrateur de l’hôte `Get-HgsTrace` spécifiant un connu `-Path` et la liste des diagnostics que vous avez l’intention d’exécuter sur les traces qui en résulte.  Exemple :

   ```PowerShell
   Get-HgsTrace -Path C:\Traces -Diagnostic Networking,BestPractices
   ```
2. Demande que chaque administrateur de l’hôte le dossier traces obtenu du package et l’envoyer à vous.  Ce processus peut être piloté par courrier électronique, par le biais de partages de fichiers, ou tout autre mécanisme basé sur les stratégies d’exploitation et les procédures établies par votre organisation.

3. Fusionner toutes les traces reçues dans un dossier unique, avec aucun autre contenu ou dossiers.

    * Par exemple, supposons que vous avait votre envoi administrateurs vous traces collectées à partir de quatre ordinateurs nommés SGH-01, SGH-02, 12 / RR1N2608 et RR1N2608-13.  Chaque administrateur serait ont envoyé un dossier portant le même nom.  Assemblage une structure de répertoire qui s’affiche comme suit :

      ```
      FabricTraces
      |- HGS-01
      |  |- TargetMetadata.xml
      |  |- Metadata.xml
      |  |- [any other trace files for this host]
      |- HGS-02
      |  |- [...]
      |- RR1N2608-12
      |  |- [...]
      |- RR1N2608-13
         |- [..]
      ```

4. Exécuter des tests de diagnostic, en fournissant le chemin d’accès au dossier trace assemblé sur le `-Path` paramètre et en spécifiant le `-RunDiagnostics` commutateur, ainsi que les diagnostics pour lequel vous demande vos administrateurs pour recueillir des traces.  Diagnostics supposera qu’il ne peut pas accéder aux hôtes ont été trouvés dans le chemin d’accès et par conséquent essaye d’utiliser uniquement les traces collectées au préalable.  Si les traces sont manquants ou endommagés, diagnostics échoue uniquement les tests affectés et se poursuivent normalement.  Exemple :

   ```PowerShell
   Get-HgsTrace -RunDiagnostics -Diagnostic Networking,BestPractices -Path ".\FabricTraces"
   ```

### <a name="mixing-saved-traces-with-additional-targets"></a>Mélange des Traces enregistrées avec des cibles supplémentaires

Dans certains cas, vous pouvez avoir un ensemble de traces collectées au préalable que vous souhaitez augmenter avec les traces de l’ordinateur hôte supplémentaires.  Il est possible de combiner des traces collectées au préalable avec des cibles supplémentaires qui seront suivis et diagnostiqués en un seul appel de diagnostics.

Suivant les instructions pour collecter et assembler un dossier de trace spécifié ci-dessus, appelez `Get-HgsTrace` avec des cibles de suivi supplémentaires introuvable dans le dossier de trace collectées au préalable :

```PowerShell
$hgs03 = New-HgsTraceTarget -HostName "hgs-03.secure.contoso.com" -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $hgs03 -Path .\FabricTraces
``` 

L’applet de commande de diagnostic identifie tous les hôtes de collectées au préalable et un hôte de supplémentaire qui doit toujours être suivis et effectue le suivi nécessaire.  La somme de toutes les traces collectées au préalable et qui vient d’être collectées est ensuite diagnostiquée.  Le dossier de suivi obtenu contiendra les anciennes et nouvelles traces.
