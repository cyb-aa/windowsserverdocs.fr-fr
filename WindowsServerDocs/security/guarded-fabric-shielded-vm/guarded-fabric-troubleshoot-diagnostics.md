---
title: Résolution des problèmes à l’aide de l’outil de diagnostic de l’infrastructure protégée
ms.prod: windows-server
ms.topic: article
ms.assetid: 07691d5b-046c-45ea-8570-a0a85c3f2d22
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 01/14/2020
ms.openlocfilehash: 3cf2b71113e812774cfb39b2ed21df8b41f83f12
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856412"
---
# <a name="troubleshooting-using-the-guarded-fabric-diagnostic-tool"></a>Résolution des problèmes à l’aide de l’outil de diagnostic de l’infrastructure protégée

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit l’utilisation de l’outil de diagnostic de l’infrastructure protégée pour identifier et corriger les défaillances courantes dans le déploiement, la configuration et le fonctionnement continu de l’infrastructure de l’infrastructure protégée. Cela comprend le service de surveillance des hôtes (SGH), tous les hôtes service Guardian et les services de prise en charge tels que DNS et Active Directory. L’outil de diagnostic peut être utilisé pour effectuer une première passe au triage d’une infrastructure protégée défaillante, fournissant aux administrateurs un point de départ pour la résolution des pannes et l’identification des ressources mal configurées. L’outil ne remplace pas le bon fonctionnement d’une structure protégée et sert uniquement à vérifier rapidement les problèmes les plus courants rencontrés au cours des opérations quotidiennes.

La documentation complète des applets de commande utilisées dans cet article est disponible dans la [Référence du module HgsDiagnostics](https://docs.microsoft.com/powershell/module/hgsdiagnostics/?view=win10-ps).

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)]

## <a name="quick-start"></a>Démarrage rapide

Vous pouvez diagnostiquer un hôte service Guardian ou un nœud SGH en appelant la commande suivante à partir d’une session Windows PowerShell avec des privilèges d’administrateur local :

```PowerShell
Get-HgsTrace -RunDiagnostics -Detailed
```

Cela permet de détecter automatiquement le rôle de l’hôte actuel et de diagnostiquer tous les problèmes pertinents qui peuvent être détectés automatiquement.  Tous les résultats générés au cours de ce processus s’affichent en raison de la présence du commutateur `-Detailed`.

Le reste de cette rubrique fournit une procédure pas à pas sur l’utilisation avancée de `Get-HgsTrace` pour effectuer des opérations telles que le diagnostic de plusieurs ordinateurs hôtes à la fois et la détection d’une configuration complexe inter-nœuds.

## <a name="diagnostics-overview"></a>Vue d’ensemble des diagnostics

Les diagnostics de l’infrastructure protégée sont disponibles sur tous les ordinateurs hôtes disposant d’outils et de fonctionnalités associés à une machine virtuelle protégée, y compris les ordinateurs hôtes exécutant Server Core.  Actuellement, les diagnostics sont inclus avec les fonctionnalités/packages suivants :

1. Rôle du service Guardian hôte
2. Support Hyper-V pour Guardian hôte
3. Outils de protection d'ordinateur virtuel pour la gestion d'infrastructure
4. Outils d’administration de serveur distant (RSAT)

Cela signifie que les outils de diagnostic seront disponibles sur tous les hôtes service Guardian, nœuds SGH, certains serveurs d’administration de structure et toutes les stations de travail Windows [10 avec les](https://www.microsoft.com/download/details.aspx?id=45520) outils d’administration de serveur distant installés.  Les diagnostics peuvent être appelés à partir de l’un des ordinateurs ci-dessus, dans le but de diagnostiquer un hôte ou un nœud SGH dans une infrastructure protégée. à l’aide des cibles de trace à distance, les diagnostics peuvent localiser les hôtes autres que l’ordinateur exécutant les diagnostics et se connecter à ceux-ci.

Chaque hôte ciblé par les diagnostics est appelé « cible de suivi ».  Les cibles de suivi sont identifiées par leur nom d’hôte et leur rôle.  Les rôles décrivent la fonction qu’une cible de trace donnée effectue dans une infrastructure protégée.  Actuellement, les cibles de trace prennent en charge les rôles `HostGuardianService` et `GuardedHost`.  Notez qu’il est possible qu’un hôte occupe plusieurs rôles à la fois et que cela soit également pris en charge par les Diagnostics, mais cela ne doit pas être fait dans les environnements de production.  Les hôtes SGH et Hyper-V doivent être séparés et distincts à tout moment.

Les administrateurs peuvent commencer toutes les tâches de diagnostic en exécutant `Get-HgsTrace`.  Cette commande exécute deux fonctions distinctes en fonction des commutateurs fournis au moment de l’exécution : la collecte de trace et le diagnostic.  Ces deux combinaisons composent l’intégralité de l’outil de diagnostic de l’infrastructure protégée.  Bien que cela ne soit pas explicitement requis, les diagnostics les plus utiles nécessitent des traces qui ne peuvent être collectées qu’avec des informations d’identification d’administrateur sur la cible de trace.  Si des privilèges insuffisants sont détenus par l’utilisateur qui exécute la collecte de trace, les traces nécessitant une élévation échoueront, tandis que les autres passeront.  Cela permet un diagnostic partiel dans le cas où un opérateur sous-privilégié effectue le triage. 

### <a name="trace-collection"></a>Collection de traces

Par défaut, `Get-HgsTrace` ne collecte les traces et les enregistre dans un dossier temporaire.  Les traces prennent la forme d’un dossier, nommé d’après l’hôte ciblé, avec des fichiers spécialement mis en forme qui décrivent la manière dont l’ordinateur hôte est configuré.  Les traces contiennent également des métadonnées qui décrivent comment les diagnostics ont été appelés pour collecter les traces.  Ces données sont utilisées par les diagnostics pour réalimenter les informations relatives à l’hôte lors de l’exécution manuelle du diagnostic.

Si nécessaire, les traces peuvent être révisées manuellement.  Tous les formats sont lisibles par l’utilisateur (XML) ou peuvent être inspectés facilement à l’aide d’outils standard (par exemple, certificats X509 et extensions de l’interpréteur de commandes Windows crypto).  Notez cependant que les traces ne sont pas conçues pour le diagnostic manuel et qu’il est toujours plus efficace de traiter les traces avec les fonctions de diagnostic de `Get-HgsTrace`.

Les résultats de l’exécution de la collecte de trace ne font aucune indication quant à l’intégrité d’un hôte donné.  Ils indiquent simplement que les traces ont été collectées avec succès.  Il est nécessaire d’utiliser les fonctionnalités de diagnostic de `Get-HgsTrace` pour déterminer si les suivis indiquent un environnement défaillant.

À l’aide du paramètre `-Diagnostic`, vous pouvez limiter la collection de suivis uniquement aux suivis requis pour faire fonctionner les diagnostics spécifiés.  Cela réduit la quantité de données collectées, ainsi que les autorisations requises pour appeler les Diagnostics.

### <a name="diagnosis"></a>Diagnostic

Les suivis collectés peuvent être diagnostiqués en fournissant `Get-HgsTrace` l’emplacement des traces via le paramètre `-Path` et en spécifiant le commutateur `-RunDiagnostics`.  En outre, `Get-HgsTrace` pouvez effectuer des collectes et des diagnostics en une seule passe en fournissant le commutateur `-RunDiagnostics` et une liste de cibles de suivi.  Si aucune cible de trace n’est fournie, l’ordinateur actuel est utilisé comme cible implicite, avec son rôle déduit en inspectant les modules Windows PowerShell installés.

Le diagnostic fournira des résultats dans un format hiérarchique indiquant les cibles de suivi, les jeux de diagnostic et les diagnostics individuels qui sont responsables d’une défaillance particulière.  Les défaillances incluent des recommandations de correction et de résolution si une détermination peut être effectuée à la suite de l’action à entreprendre.  Par défaut, le passage et les résultats non pertinents sont masqués.  Pour voir tout ce qui a été testé par les Diagnostics, spécifiez le commutateur `-Detailed`.  Cela entraîne l’affichage de tous les résultats, quel que soit leur état.

Il est possible de limiter l’ensemble de diagnostics qui sont exécutés à l’aide du paramètre `-Diagnostic`.  Cela vous permet de spécifier les classes de diagnostics qui doivent être exécutées sur les cibles de suivi et de supprimer toutes les autres.  Les exemples de classes de diagnostic disponibles incluent la mise en réseau, les meilleures pratiques et le matériel client.  Consultez la [documentation](https://technet.microsoft.com/library/mt718831.aspx) de l’applet de commande pour obtenir une liste à jour des diagnostics disponibles.

> [!WARNING]
> Les diagnostics ne remplacent pas un pipeline de surveillance et de réponse aux incidents puissant.  Un package de Operations Manager System Center est disponible pour la surveillance des structures protégées, ainsi que des différents canaux de journaux des événements qui peuvent être analysés pour détecter les problèmes plus tôt.  Les diagnostics peuvent ensuite être utilisés pour trier rapidement ces défaillances et établir un cours d’action.

## <a name="targeting-diagnostics"></a>Ciblage des diagnostics

`Get-HgsTrace` fonctionne sur les cibles de suivi.  Une cible de trace est un objet qui correspond à un nœud SGH ou un hôte service Guardian au sein d’une infrastructure protégée.  Il peut être considéré comme une extension d’un `PSSession` qui comprend les informations requises uniquement par les diagnostics tels que le rôle de l’hôte dans l’infrastructure.  Les cibles peuvent être générées implicitement (par exemple, diagnostic local ou manuel) ou explicitement avec la commande `New-HgsTraceTarget`.

### <a name="local-diagnosis"></a>Diagnostic local

Par défaut, `Get-HgsTrace` ciblera l’hôte local (par exemple, où l’applet de commande est appelée).  C’est ce que l’on appelle la cible locale implicite.  La cible locale implicite est utilisée uniquement quand aucune cible n’est fournie dans le paramètre `-Target` et qu’aucune trace préexistante n’est trouvée dans le `-Path`.

La cible locale implicite utilise l’inférence de rôle pour déterminer le rôle joué par l’hôte actuel dans l’infrastructure protégée.  Cela est basé sur les modules Windows PowerShell installés qui correspondent approximativement aux fonctionnalités qui ont été installées sur le système.  La présence du module `HgsServer` fait que la cible de suivi prend le rôle `HostGuardianService` et que la présence du module `HgsClient` entraîne le `GuardedHost`rôle de la cible de suivi.  Il est possible qu’un hôte donné ait les deux modules présents, auquel cas il sera traité à la fois comme un `HostGuardianService` et comme un `GuardedHost`.

Par conséquent, l’appel par défaut des diagnostics pour la collecte locale des traces est le suivant :

```PowerShell
Get-HgsTrace
```

... équivaut à ce qui suit :

```PowerShell
New-HgsTraceTarget -Local | Get-HgsTrace
```

> [!TIP]
> `Get-HgsTrace` pouvez accepter des cibles via le pipeline ou directement via le paramètre `-Target`.  Il n’y a aucune différence entre les deux.

### <a name="remote-diagnosis-using-trace-targets"></a>Diagnostic à distance à l’aide de cibles de trace

Il est possible de diagnostiquer à distance un hôte en générant des cibles de suivi avec des informations de connexion à distance.  Tout ce qui est nécessaire est le nom d’hôte et un ensemble d’informations d’identification pouvant être connectées à l’aide de la communication à distance Windows PowerShell.
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $server
```
Cet exemple génère une invite pour collecter les informations d’identification de l’utilisateur distant, puis les Diagnostics s’exécutent à l’aide de l’hôte distant sur `hgs-01.secure.contoso.com` pour terminer la collecte des traces.  Les suivis résultants sont téléchargés vers l’hôte local, puis diagnostiqués.  Les résultats du diagnostic sont présentés de la même façon que lors de l’exécution du [diagnostic local](#local-diagnosis).  De même, il n’est pas nécessaire de spécifier un rôle, car il peut être déduit en fonction des modules Windows PowerShell installés sur le système distant.

Le diagnostic à distance utilise la communication à distance Windows PowerShell pour tous les accès à l’hôte distant.  Par conséquent, il est nécessaire que la cible de la trace ait activé la communication à distance Windows PowerShell (consultez [Enable PSRemoting](https://technet.microsoft.com/library/hh849694.aspx)) et que localhost soit correctement configuré pour lancer des connexions à la cible.

> [!NOTE]
> Dans la plupart des cas, il est uniquement nécessaire que l’hôte local fasse partie de la même forêt Active Directory et qu’un nom d’hôte DNS valide soit utilisé.  Si votre environnement utilise un modèle de Fédération plus complexe ou si vous souhaitez utiliser des adresses IP directes pour la connectivité, vous devrez peut-être effectuer une configuration supplémentaire telle que la définition des [hôtes approuvés](https://technet.microsoft.com/library/ff700227.aspx)WinRM.

Vous pouvez vérifier qu’une cible de trace est correctement instanciée et configurée pour accepter les connexions à l’aide de l’applet de commande `Test-HgsTraceTarget` :
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
$server | Test-HgsTraceTarget
```
Cette commande retourne `$True` si et seulement si `Get-HgsTrace` peut établir une session de diagnostic à distance avec la cible de trace.  En cas de défaillance, cette applet de commande renvoie des informations d’État pertinentes pour la résolution des problèmes liés à la connexion à distance Windows PowerShell.

#### <a name="implicit-credentials"></a>Informations d’identification implicites

Lorsque vous effectuez un diagnostic à distance à partir d’un utilisateur disposant de privilèges suffisants pour se connecter à distance à la cible de trace, il n’est pas nécessaire de fournir des informations d’identification pour `New-HgsTraceTarget`.  L’applet de commande `Get-HgsTrace` réutilisera automatiquement les informations d’identification de l’utilisateur qui a appelé l’applet de commande lors de l’ouverture d’une connexion.

> [!WARNING]
> Certaines restrictions s’appliquent à la réutilisation des informations d’identification, en particulier lors de l’exécution de ce que l’on appelle un « deuxième tronçon ».  Cela se produit lors d’une tentative de réutilisation d’informations d’identification à partir d’une session à distance sur un autre ordinateur.  Il est nécessaire d' [installer CredSSP](https://technet.microsoft.com/library/hh849872.aspx) pour prendre en charge ce scénario, mais cela n’entre pas dans le cadre de la gestion et du dépannage de la structure protégée.

#### <a name="using-windows-powershell-just-enough-administration-jea-and-diagnostics"></a>Utilisation de Windows PowerShell juste assez d’administration (JEA) et de diagnostics

Le diagnostic à distance prend en charge l’utilisation de points de terminaison Windows PowerShell avec restriction JEA. Par défaut, les cibles de suivi distantes se connectent à l’aide du point de terminaison `microsoft.powershell` par défaut.  Si la cible de trace a le rôle `HostGuardianService`, elle tentera également d’utiliser le point de terminaison `microsoft.windows.hgs` qui est configuré lorsque SGH est installé.

Si vous souhaitez utiliser un point de terminaison personnalisé, vous devez spécifier le nom de la configuration de session lors de la construction de la cible de trace à l’aide du paramètre `-PSSessionConfigurationName`, comme ci-dessous :

```PowerShell
New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential) -PSSessionConfigurationName "microsoft.windows.hgs"
```

#### <a name="diagnosing-multiple-hosts"></a>Diagnostic de plusieurs ordinateurs hôtes

Vous pouvez passer plusieurs cibles de trace à `Get-HgsTrace` à la fois.  Cela comprend une combinaison de cibles locales et distantes.  Chaque cible sera suivie successivement, puis les traces de chaque cible seront diagnostiquées simultanément.  L’outil de diagnostic peut utiliser la connaissance accrue de votre déploiement pour identifier les problèmes complexes de configuration inter-nœuds qui, autrement, ne seraient pas détectables.  L’utilisation de cette fonctionnalité nécessite de fournir des traces à partir de plusieurs ordinateurs hôtes simultanément (dans le cas d’un diagnostic manuel) ou en ciblant plusieurs hôtes lors de l’appel de `Get-HgsTrace` (dans le cas du diagnostic à distance).

Voici un exemple d’utilisation du diagnostic à distance pour trier une infrastructure composée de deux nœuds SGH et de deux hôtes service Guardian, où l’un des hôtes service Guardian est utilisé pour lancer `Get-HgsTrace`.

```PowerShell
$hgs01 = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Credential (Enter-Credential)
$hgs02 = New-HgsTraceTarget -HostName "hgs-02.secure.contoso.com" -Credential (Enter-Credential)
$gh01 = New-HgsTraceTarget -Local
$gh02 = New-HgsTraceTarget -HostName "guardedhost-02.contoso.com"
Get-HgsTrace -Target $hgs01,$hgs02,$gh01,$gh02 -RunDiagnostics
```

> [!NOTE]
> Vous n’avez pas besoin de diagnostiquer l’ensemble de votre infrastructure protégée pour diagnostiquer plusieurs nœuds.  Dans de nombreux cas, il suffit d’inclure tous les nœuds qui peuvent être impliqués dans une condition d’échec donnée.  Il s’agit généralement d’un sous-ensemble des hôtes service Guardian et d’un certain nombre de nœuds du cluster SGH.

## <a name="manual-diagnosis-using-saved-traces"></a>Diagnostic manuel à l’aide de suivis enregistrés

Parfois, vous souhaiterez peut-être réexécuter les diagnostics sans collecter à nouveau les traces, ou vous n’avez peut-être pas les informations d’identification nécessaires pour diagnostiquer à distance tous les ordinateurs hôtes de votre infrastructure simultanément.  Le diagnostic manuel est un mécanisme par lequel vous pouvez toujours effectuer un triage de structure entière à l’aide de `Get-HgsTrace`, mais sans utiliser la collecte de trace à distance.

Avant d’effectuer un diagnostic manuel, vous devez vous assurer que les administrateurs de chaque ordinateur hôte de l’infrastructure qui seront triés sont prêts et prêts à exécuter des commandes en votre nom.  La sortie de suivi de diagnostic n’expose pas les informations qui sont généralement considérées comme sensibles. Toutefois, il incombe à l’utilisateur de déterminer s’il est possible d’exposer ces informations à d’autres personnes en toute sécurité.

> [!NOTE]
> Les traces ne sont pas rendues anonymes et révèlent la configuration du réseau, les paramètres d’infrastructure à clé publique et d’autres configurations qui sont parfois considérées comme des informations privées.  Par conséquent, les traces doivent être transmises uniquement à des entités approuvées au sein d’une organisation et jamais publiées publiquement.

Les étapes à suivre pour effectuer un diagnostic manuel sont les suivantes :

1. Demandez à chaque administrateur hôte de s’exécuter `Get-HgsTrace` spécifiant une `-Path` connue et la liste des diagnostics que vous avez l’intention d’exécuter sur les suivis résultants.  Par exemple :

   ```PowerShell
   Get-HgsTrace -Path C:\Traces -Diagnostic Networking,BestPractices
   ```

2. Demandez à chaque administrateur hôte de placer le dossier traces résultant et de vous l’envoyer.  Ce processus peut être piloté par courrier électronique, via des partages de fichiers ou tout autre mécanisme basé sur les stratégies et les procédures d’exploitation établies par votre organisation.

3. Fusionnez tous les suivis reçus dans un dossier unique, sans aucun autre contenu ou dossier.

    * Par exemple, supposons que vos administrateurs vous ont envoyé des traces collectées à partir de quatre ordinateurs nommés SGH-01, SGH-02, RR1N2608-12 et RR1N2608-13.  Chaque administrateur vous a envoyé un dossier portant le même nom.  Vous pouvez assembler une structure de répertoires qui se présente comme suit :

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

4. Exécutez les Diagnostics, en fournissant le chemin d’accès au dossier des traces assemblées sur le paramètre `-Path` et en spécifiant le commutateur `-RunDiagnostics` ainsi que les diagnostics pour lesquels vous avez demandé à vos administrateurs de collecter des traces.  Les diagnostics supposent qu’il ne peut pas accéder aux ordinateurs hôtes qui se trouvent dans le chemin d’accès et tente donc d’utiliser uniquement les suivis précollectés.  Si des suivis sont manquants ou endommagés, les diagnostics échouent uniquement aux tests affectés et se poursuivent normalement.  Par exemple :

   ```PowerShell
   Get-HgsTrace -RunDiagnostics -Diagnostic Networking,BestPractices -Path ".\FabricTraces"
   ```

### <a name="mixing-saved-traces-with-additional-targets"></a>Combinaison de traces enregistrées et de cibles supplémentaires

Dans certains cas, vous pouvez avoir un ensemble de suivis précollectés que vous souhaitez augmenter avec des suivis d’hôte supplémentaires.  Il est possible de mélanger des suivis précollectés avec des cibles supplémentaires qui seront suivies et diagnostiquées en un seul appel de Diagnostics.

En suivant les instructions pour collecter et assembler un dossier de trace spécifié ci-dessus, appelez `Get-HgsTrace` avec des cibles de trace supplémentaires introuvables dans le dossier de trace pré-collecté :

```PowerShell
$hgs03 = New-HgsTraceTarget -HostName "hgs-03.secure.contoso.com" -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $hgs03 -Path .\FabricTraces
``` 

L’applet de commande de diagnostic identifie tous les ordinateurs hôtes précollectés, ainsi que l’hôte supplémentaire qui doit toujours être suivi et effectue le suivi nécessaire.  La somme de toutes les traces collectées au préalable et regroupées sera ensuite diagnostiquée.  Le dossier de trace résultant contient à la fois les traces anciennes et nouvelles.

## <a name="known-issues"></a>Problèmes connus

Le module de diagnostics de l’infrastructure protégée a des limitations connues lorsqu’il s’exécute sur Windows Server 2019 ou Windows 10, version 1809 et versions ultérieures du système d’exploitation.
L’utilisation des fonctionnalités suivantes peut entraîner des résultats erronés :

* Attestation de clé hôte
* Configuration du SGH d’attestation uniquement (pour les scénarios de SQL Server Always Encrypted)
* Utilisation d’artefacts de stratégie v1 sur un serveur SGH où la valeur par défaut de la stratégie d’attestation est v2

Une défaillance dans `Get-HgsTrace` lors de l’utilisation de ces fonctionnalités n’indique pas nécessairement que le serveur SGH ou l’hôte service Guardian n’est pas configuré de façon incorrecte.
Utilisez d’autres outils de diagnostic comme `Get-HgsClientConfiguration` sur un hôte service Guardian pour tester si un ordinateur hôte a passé l’attestation.
