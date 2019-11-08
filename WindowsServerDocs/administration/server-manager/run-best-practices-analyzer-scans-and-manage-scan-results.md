---
title: Exécuter Best Practices Analyzer analyses et gérer les Results_1 d’analyse
description: Gestionnaire de serveur
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 232f1c80-88ef-4a39-8014-14be788c2766
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6edd561749ea0d224058b482992d357357c12505
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383077"
---
# <a name="run-best-practices-analyzer-scans-and-manage-scan-results"></a>Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses

>S'applique à : Windows Server 2016

Dans l’infrastructure de gestion Windows, les *meilleures pratiques* sont des recommandations considérées comme le moyen idéal, en conditions normales, de configurer un serveur tel que défini par des experts. Par exemple, il est reconnu comme meilleure pratique pour la plupart des applications serveur de ne laisser ouverts que les ports nécessaires à ces applications pour communiquer avec d’autres ordinateurs du réseau, et de bloquer les ports inutilisés. Bien que le non-respect des meilleures pratiques, même les plus capitales, ne soit pas forcément problématique, se conformer à ces méthodes conseillées permet néanmoins de détecter les configurations de serveur pouvant entraîner des performances médiocres, une fiabilité douteuse, des conflits inattendus, des risques de sécurité accrus ou d’autres problèmes potentiels.

Best Practices Analyzer (BPA) est un outil de gestion de serveur disponible dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2008 R2. BPA peut aider les administrateurs à réduire les violations des meilleures pratiques en analysant les rôles installés sur les serveurs gérés qui exécutent Windows Server 2012 ou Windows Server 2008 R2 et en signalant les meilleures pratiques à l’administrateur.

Vous pouvez exécuter des analyses Best Practices Analyzer (BPA) à partir de Gestionnaire de serveur, à l’aide de l’interface graphique utilisateur (GUI) BPA ou à l’aide d’applets de commande dans Windows PowerShell. à compter de Windows Server 2012, vous pouvez analyser un ou plusieurs rôles à la fois, sur plusieurs serveurs, que vous utilisiez la vignette Best Practices Analyzer dans la console Gestionnaire de serveur ou les applets de commande Windows PowerShell pour exécuter des analyses. Vous pouvez également demander à BPA d’exclure ou d’ignorer les résultats des analyses que vous ne souhaitez pas consulter.

Cette rubrique contient les sections suivantes.

-   [Rechercher BPA](#BKMK_find)

-   [Fonctionnement de BPA](#BKMK_how)

-   [Exécution d’analyses Best Practices Analyzer sur des rôles](#BKMK_BPAscan)

-   [Gérer les résultats de l’analyse](#BKMK_manage)

## <a name="BKMK_find"></a>Rechercher BPA
Vous pouvez trouver la vignette Best Practices Analyzer sur les pages de rôles et de groupes de serveurs de Gestionnaire de serveur dans Windows Server 2012 R2 et Windows Server 2012, ou vous pouvez ouvrir une session Windows PowerShell avec des droits d’utilisateur élevés pour exécuter les applets de commande Best Practices Analyzer.

## <a name="BKMK_how"></a>Fonctionnement de BPA
BPA fonctionne en mesurant la conformité d’un rôle aux règles des meilleures pratiques dans huit catégories d’efficacité, de fiabilité et de fiabilité. Les résultats de ces mesures se traduisent par l’un des trois niveaux de gravité décrits dans le tableau ci-dessous.

|Niveau de gravité|Description|
|---------|--------|
|Error|Des résultats définis en tant qu’erreurs sont renvoyés lorsqu’un rôle ne respecte pas les conditions d’une règle relevant des meilleures pratiques et que des problèmes d’ordre fonctionnel sont attendus.|
|Information|Des résultats à titre d’informations conformes sont renvoyés lorsqu’un rôle satisfait aux conditions d’une règle des meilleures pratiques.|
|Warning|Des résultats d’avertissement sont renvoyés si les résultats de non-conformité peuvent causer des problèmes en cas de modifications. L’application peut être jugée conforme par rapport à son fonctionnement actuel, mais peut ne pas remplir les conditions d’une règle si des modifications ne sont pas apportées à ses paramètres de configuration ou de stratégie. Par exemple, une analyse des services Bureau à distance peut montrer un résultat d’avertissement si un serveur de licences n’est pas disponible pour le rôle, car même si aucune connexion distante n’est active au moment de l’analyse, le fait de ne pas avoir de serveur de licences empêche de futures connexions distantes d’obtenir des licences d’accès client valides.|

### <a name="rule-categories"></a>Catégories de règles
Le tableau suivant décrit les catégories de règles de meilleures pratiques par rapport aux rôles mesurés pendant une analyse de Best Practices Analyzer.

|Nom de la catégorie|Description|
|---------|--------|
|Sécurité|Des règles de sécurité sont appliquées pour mesurer le risque relatif d’un rôle pour l’exposition à des menaces telles que des utilisateurs non autorisés ou malveillants, ou encore la perte ou le vol de données confidentielles ou propriétaires.|
|Performances|Les règles de performances s’appliquent pour mesurer la capacité d’un rôle à traiter les demandes et à effectuer ses tâches prescrites dans l’entreprise dans les délais prévus, en fonction de la charge de travail du rôle.|
|Configuration|Les règles de configuration sont appliquées pour identifier des paramètres de rôle pouvant nécessiter des modifications au niveau du rôle afin que celui-ci s’exécute de façon optimale. Les règles de configuration permettent d’empêcher l’apparition de conflits entre paramètres pouvant se solder par des messages d’erreur ou par l’impossibilité pour le rôle d’assurer les fonctions qui lui ont été assignées dans l’entreprise.|
|Stratégie|Des règles de stratégie sont appliquées pour identifier les paramètres de Registre stratégie de groupe ou Windows qui peuvent nécessiter des modifications pour qu’un rôle fonctionne de façon optimale et sécurisée.|
|Opération|Les règles d’opération sont appliquées pour identifier les échecs possibles d’un rôle lors de l’exécution des tâches qui lui ont été prescrites dans l’entreprise.|
|Prédéploiement|Les règles de prédéploiement sont appliquées avant le déploiement d’un rôle installé dans l’entreprise. Elles permettent aux administrateurs d’évaluer si les meilleures pratiques ont été respectées avant que le rôle ne soit utilisé à des fins de production.|
|Postdéploiement|Les règles de postdéploiement sont appliquées une fois que tous les services requis ont démarré pour un rôle, et après que le rôle s’exécute dans l’entreprise.|
|Prérequis|Les règles en matière de conditions préalables expliquent les paramètres de configuration, les paramètres de stratégie et les fonctionnalités qui sont nécessaires au rôle avant que l’analyseur BPA puisse appliquer des règles spécifiques à partir d’autres catégories. Une condition préalable dans les résultats d’analyse indique qu’un paramètre incorrect, un programme manquant, une stratégie activée ou désactivée de façon incorrecte, un paramètre de clé de Registre ou un autre élément de configuration a empêché BPA d’appliquer une ou plusieurs règles au cours d’une analyse. Un résultat de condition préalable ne sous-entend pas conformité ou non-conformité. Cela signifie simplement qu’une règle n’a pas pu être appliquée et que, par conséquent, elle ne fait pas partie des résultats de l’analyse.|

## <a name="BKMK_BPAscan"></a>Exécution d’analyses Best Practices Analyzer sur des rôles
Vous pouvez effectuer des analyses BPA sur des rôles à l’aide de l’interface graphique utilisateur BPA dans Gestionnaire de serveur, ou à l’aide des applets de commande Windows PowerShell.

Dans Windows Server 2012 R2 et Windows Server 2012, certains rôles vous invitent à spécifier des paramètres supplémentaires, tels que les noms de serveurs spécifiques ou de partages qui exécutent des parties du rôle, ou les ID de sous-modèles, avant de commencer une analyse BPA. Pour des analyses BPA dans des modèles exigeant l’apport de paramètres supplémentaires, utilisez les applets de commande BPA ; l’interface graphique utilisateur BPA ne peut pas accepter des paramètres supplémentaires comme les ID de sous-modèles. Par exemple, l'ID de sous-modèle **FSRM** représente le sous-modèle BPA Services de fichiers pour les Outils de gestion de ressources pour serveur de fichiers, un service de rôle des services de fichiers et de stockage. Pour exécuter une analyse uniquement sur le serveur de fichiers Gestionnaire des ressources service de rôle, exécutez une analyse BPA à l’aide des applets de commande Windows PowerShell, puis ajoutez le paramètre `SubmodelId` à votre applet de commande.

Bien que vous ne pouvez pas transmettre des paramètres supplémentaires à une analyse que vous démarrez dans l’interface graphique utilisateur de BPA, la vignette BPA dans Gestionnaire de serveur affiche les résultats de l’analyse BPA la plus récente, quelle que soit la façon dont l’analyse a été démarrée.

-   [Analyse des rôles à l’aide de l’interface graphique utilisateur BPA](#BKMK_GUIscan)

-   [Analyse des rôles à l’aide des applets de commande Windows PowerShell](#BKMK_PSscan)

### <a name="BKMK_GUIscan"></a>Analyse des rôles à l’aide de l’interface graphique utilisateur BPA
Procédez comme suit pour analyser un ou plusieurs rôles dans l’interface graphique utilisateur BPA.

##### <a name="to-scan-roles-by-using-the-bpa-gui"></a>Pour analyser des rôles à l’aide de l’interface graphique utilisateur BPA

1.  Effectuez l’une des opérations suivantes pour ouvrir Gestionnaire de serveur s’il n’est pas déjà ouvert.

    -   Dans la barre des tâches Windows, cliquez sur le bouton Gestionnaire de serveur.

    -   Dans l’écran d' **Accueil** , cliquez sur la vignette gestionnaire de serveur.

2.  Dans le volet de navigation, ouvrez la page d’un rôle ou d’un groupe.

    Lorsque vous exécutez des analyses BPA à partir de la page d’un rôle ou d’un groupe, tous les rôles installés sur les serveurs du groupe sont analysés.

3.  Dans le menu **tâches** de la vignette **Best Practices Analyzer** , cliquez sur **Démarrer l’analyse BPA**.

4.  En fonction du nombre de règles que vous évaluez pour le rôle ou le groupe sélectionné, l’exécution de l’analyse BPA peut prendre plusieurs minutes.

### <a name="BKMK_PSscan"></a>Analyse des rôles à l’aide des applets de commande Windows PowerShell
Utilisez les procédures suivantes pour analyser un ou plusieurs rôles à l’aide des applets de commande Windows PowerShell.

> [!NOTE]
> Les procédures de cette section ne montrent pas toutes les applets de commande ni tous les paramètres. Pour plus d'informations sur les opérations BPA dans **wps_2 ***, dans votre session*** wps_2**, entrez *Get-Help*applet_BPA-full, où applet_BPA peut avoir l'une des valeurs suivantes. Vous pouvez également trouver des rubriques d’aide sur les applets de commande BPA sur le [TechCenter Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=240177).

-   **BPAmodel**

-   **Invoke-BPAmodel**

-   **BPAResult**

-   **Set-BPAResult**

#### <a name="BKMK_singlerole"></a>Pour analyser un rôle unique à l’aide d’applets de commande Windows PowerShell

1.  Effectuez l’une des opérations suivantes pour exécuter Windows PowerShell avec des droits d’utilisateur élevés.

    -   Pour exécuter Windows PowerShell en tant qu’administrateur à partir de l’écran d' **Accueil** , cliquez avec le bouton droit sur la vignette **Windows PowerShell** dans les résultats **applications** , puis dans la barre des applications, cliquez sur **exécuter en tant qu’administrateur**.

    -   Pour exécuter Windows PowerShell en tant qu’administrateur à partir du bureau, cliquez avec le bouton droit sur le raccourci **Windows PowerShell** dans la barre des tâches, puis cliquez sur **exécuter en tant qu’administrateur**.

2.  à compter de Windows PowerShell 3,0, les modules d’applet de commande sont importés automatiquement dans votre session Windows PowerShell la première fois qu’une applet de commande du module est utilisée. Vous n’avez pas besoin d’importer ou de charger le module d’applets de commande BPA.

3.  Recherchez les ID de modèles de tous les rôles pour lesquels des analyses BPA peuvent être effectuées en entrant l’applet de commande **BPAModel** , comme indiqué dans l’exemple suivant.

    `Get-Bpamodel`

4.  Dans les résultats de l’étape 3, repérez les ID de modèles des rôles pour lesquels vous voulez effectuer une analyse BPA.

5.  Entrez l'une des commandes suivantes pour démarrer l'analyse BPA pour un rôle en particulier. Pour plusieurs rôles, séparez les ID des modèles par des virgules.

    `Invoke-BPAmodel -modelId <modelID_from_Step3>`

    `Invoke-BPAmodel <modelID_from_Step3>`

    **Exemple :** `Invoke-BPAmodel -modelId Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    > [!NOTE]
    > L’ID de modèle comprend le chemin d’accès complet mentionné dans la colonne **Id** ; par exemple, **Microsoft/Windows/Hyper-V**.

    Vous pouvez également démarrer une analyse sur un rôle spécifique à partir des résultats de l’étape 3 en transférant les résultats de l’applet de commande `Get-BPAmodel` à l’applet de commande `Invoke-BPAmodel` , comme indiqué dans l’exemple suivant.

    `Get-BPAmodel <model_ID> | Invoke-BPAmodel`

    L’exécution de cette applet de commande sans spécifier d’ID de modèle conduit tous les modèles retournés par l’applet de commande `Get-BPAmodel` dans l’applet de commande `Invoke-BPAmodel`, en démarrant les analyses sur tous les modèles disponibles sur les serveurs qui ont été ajoutés au pool de serveurs Gestionnaire de serveur.

#### <a name="BKMK_allroles"></a>Pour analyser tous les rôles à l’aide d’applets de commande Windows PowerShell

1.  Ouvrez une session Windows PowerShell avec des droits d’utilisateur élevés, si celle-ci n’est pas déjà ouverte. Reportez-vous à la procédure précédente pour obtenir des instructions.

2.  Redirigez tous les rôles pour lesquels les analyses BPA peuvent être effectuées dans l’applet de commande `Invoke-BPAmodel` pour démarrer les analyses.

    `Get-BPAmodel | Invoke-BPAmodel`

3.  Une fois l’analyse terminée, Windows PowerShell renvoie des résultats similaires à ce qui suit, pour chaque rôle analysé.

    ```
    modelId                 :  Microsoft/Windows/FileServices
    SubmodelId              :
    Success                 :  True
    Scantime                :  1/01/2012  12:18:40 PM
    ScantimeUtcOffset       :  -08:00:00
    detail                  :  {server_name1, server_name2}

    ```

## <a name="BKMK_manage"></a>Gérer les résultats de l’analyse
Après avoir réalisé une analyse BPA dans l’interface graphique utilisateur, vous pouvez afficher les résultats de l’analyse dans une vignette BPA. Lorsque vous sélectionnez un résultat dans une vignette, un volet de visualisation dans cette dernière affiche les propriétés du résultat, et notamment un élément indiquant si le rôle est conforme à la meilleure pratique qui lui est associé. Si un résultat n’est pas conforme et que vous souhaitez savoir comment résoudre les problèmes décrits dans les propriétés de résultat, les liens hypertexte dans les propriétés des résultats d’erreur et d’avertissement ouvrent des rubriques d’aide sur la résolution détaillée sur le TechCenter Windows Server.

> [!NOTE]
> Les résultats d’analyse BPA ne sont pas automatiquement enregistrés ou archivés. Lorsque vous soumettez un modèle ou un sous-modèle à une nouvelle analyse, les résultats de la dernière analyse sont remplacés. Pour enregistrer les résultats d’analyse BPA à des fins d’archivage, d’impression ou d’envoi à d’autres personnes, voir [Pour afficher ou enregistrer des résultats BPA à partir de sessions Windows PowerShell dans différents formats](#BKMK_formats) dans cette section.

### <a name="exclude-and-include-bpa-results"></a>Exclure et inclure des résultats BPA
Si vous n’avez pas besoin de voir des résultats BPA, tels que des résultats qui se produisent fréquemment dans vos analyses BPA mais ne nécessitent aucune résolution, vous pouvez exclure les résultats à l’aide de l’interface utilisateur graphique ou des applets de commande BPA dans Windows PowerShell. Les résultats peuvent être réintégrés à tout moment.

> [!NOTE]
> Lorsque vous excluez des résultats, ceux-ci sont également exclus de l’affichage sur les serveurs gérés. D’autres administrateurs ne peuvent pas voir les résultats exclus sur les serveurs gérés. Pour exclure des résultats de la vue dans une console de Gestionnaire de serveur locale uniquement, créez une requête personnalisée au lieu d’utiliser la commande **exclure les résultats** .

#### <a name="BKMK_exclude"></a>Exclure les résultats de l’analyse
Le paramètre **Exclure** est persistant ; les résultats que vous excluez demeurent exclus des analyses ultérieures du même modèle sur le même ordinateur, à moins qu’ils ne soient de nouveau inclus.

Vous pouvez exclure des résultats d’analyse en exécutant l’applet de commande `Set-BPAResult` avec le paramètre `Exclude` . Comme dans la vignette Best Practices Analyzer dans Gestionnaire de serveur, vous pouvez exclure des objets de résultats individuels, ou vous pouvez également exclure un ensemble de résultats dont les champs (catégorie, titre et gravité, par exemple) sont égaux à ou contiennent des valeurs spécifiées. Par exemple, vous pouvez exclure tous les résultats **Performance** d’un ensemble de résultats d’analyse pour un modèle.

> [!NOTE]
> Vous devez exécuter au moins une analyse BPA dans un modèle avant de pouvoir utiliser des procédures dans cette section.

###### <a name="to-exclude-scan-results-by-using-the-gui"></a>Pour exclure des résultats d’analyse à l’aide de l’interface graphique utilisateur

1.  Ouvrez une page de groupe de serveurs ou de rôles dans Gestionnaire de serveur.

2.  Dans la vignette de Best Practices Analyzer pour le groupe de serveurs ou de rôles, cliquez avec le bouton droit sur un résultat dans la liste, puis cliquez sur **exclure les résultats**.

    Le résultat n’apparaît plus dans la liste des résultats.

3.  Pour afficher les résultats exclus dans l’interface graphique utilisateur, exécutez la requête **Résultats exclus** intégrée. Cliquez sur **Requêtes de recherche enregistrées**, puis sur **Résultats exclus**.

    Après l’exécution de la requête **Résultats exclus**, notez que le texte de sous-en-tête de vignette, une description des résultats affichés dans la liste, devient **Résultats exclus**. Seuls les résultats exclus s’affichent dans la liste.

###### <a name="to-exclude-scan-results-by-using-windows-powershell-cmdlets"></a>Pour exclure des résultats d’analyse à l’aide d’applets de commande Windows PowerShell

1.  Ouvrez une session Windows PowerShell avec des droits de l'utilisateur élevés.

2.  Excluez des résultats spécifiques d'une analyse de modèle en exécutant la commande suivante :

    `Get-BPAResult -modelId <model ID> | Where { $_.<Field Name> -eq "Value"} | Set-BPAResult -Exclude $true`

    La commande précédente récupère des éléments de résultats d’analyse BPA pour l’ID de modèle représenté par l' *ID de modèle*.

    La deuxième partie de la commande filtre les résultats de l’applet de commande `Get-BPAResult` pour n’extraire que les résultats d’analyse pour lesquels la valeur d’un champ de résultat, représentée par *nom_champ*, correspond au texte mis entre guillemets.

    La dernière partie de la commande qui suit la deuxième barre verticale exclut les résultats filtrés par la précédente section de l'applet de commande.

    **Exemple :** `Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq "Information"} | Set-BPAResult -Exclude $true`

#### <a name="include-scan-results"></a>Inclure des résultats d’analyse
Lorsque vous voulez afficher des résultats d’analyse exclus, il suffit de les inclure. Le paramètre **Inclure** est persistant ; les résultats inclus demeurent inclus dans les analyses ultérieures du même modèle sur le même ordinateur.

##### <a name="BKMK_gui"></a>Pour inclure des résultats d’analyse à l’aide de l’interface graphique

1.  Ouvrez une page de groupe de serveurs ou de rôles dans Gestionnaire de serveur.

2.  Dans la vignette de Best Practices Analyzer pour le groupe de serveurs ou de rôles, cliquez avec le bouton droit sur un résultat exclu dans la liste de requêtes **résultats exclus** , puis cliquez sur **inclure les résultats**.

    Le résultat n’apparaît plus dans la liste des résultats exclus. Effacez la requête en cliquant sur **Effacer tout** pour afficher le résultat inclus dans la liste de tous les résultats inclus.

##### <a name="BKMK_cmdlets"></a>Pour inclure des résultats d’analyse à l’aide d’applets de commande Windows PowerShell

1.  Ouvrez une session Windows PowerShell avec des droits de l'utilisateur élevés.

2.  Incluez des résultats spécifiques d'une analyse de modèle en tapant la commande suivante et en appuyant sur **Entrée**.

    `Get-BPAResult -modelId <model Id> | Where { $_.<Field Name> -eq "Value" } | Set-BPAResult -Exclude $false`

    La commande précédente récupère des éléments de résultats d’analyse BPA pour le modèle représenté par l' *ID de modèle*.

    La deuxième partie de la commande, après le premier caractère de barre verticale ( **|** ) filtre les résultats de l’applet de commande **BPAResult** pour récupérer uniquement les résultats d’analyse pour lesquels la valeur du champ de résultat, représentée par le *nom de champ*, correspond texte entre guillemets.

    La dernière partie de la commande, après la deuxième barre verticale, intègre les résultats qui sont filtrés par la deuxième partie de l'applet de commande en définissant la valeur du paramètre **-Exclude** sur **false**.

    **Exemple :** `Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq "Information"} | Set-BPAResult -Exclude $false`

### <a name="view-and-export-bpa-scan-results-in-windows-powershell"></a>Afficher et exporter des résultats d’analyse BPA dans Windows PowerShell
Pour afficher et gérer les résultats d’analyse à l’aide des applets de commande Windows PowerShell, consultez les procédures suivantes. Avant d’utiliser l’une des procédures suivantes, exécutez au moins une analyse BPA sur un au moins un modèle ou un sous-modèle.

#### <a name="BKMK_recentPS"></a>Pour afficher les résultats de l’analyse la plus récente d’un rôle à l’aide de Windows PowerShell

1.  Ouvrez une session Windows PowerShell avec des droits de l'utilisateur élevés.

2.  Procurez-vous les résultats de l’analyse la plus récente pour un ID de modèle défini. tapez la commande suivante, dans laquelle le modèle est représenté par l' *ID de modèle*, puis appuyez sur **entrée**. Vous pouvez obtenir les résultats de plusieurs ID de modèles en séparant les ID par des virgules.

    `Get-BPAResult <model ID>`

    **Exemple :** `Get-BPAResult Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    Si vous avez analysé un sous-modèle d’un modèle, tel qu’un service de rôle, récupérez les résultats uniquement pour ce sous-modèle en incluant l’ID de sous-modèle dans l’applet de commande.

    **Exemple :** `Get-BPAResult Microsoft/Windows/FileServices -SubmodelID FSRM`

#### <a name="BKMK_formats"></a>Pour afficher ou enregistrer des résultats BPA à partir de sessions Windows PowerShell dans différents formats

-   Dans Windows PowerShell, chaque résultat BPA ressemble à ce qui suit.

    ```
    ResultNumber     :  14
    ResultId         :  1557706192
    modelId          :  Microsoft/Windows/FileServices
    SubmodelId       :  FSRM
    RuleId           :  16
    computerName     :  server_name1, server_name2
    Context          :  FileServices
    Source           :  server_name1
    Severity         :  Information
    Category         :  Configuration
    Title            :  Access Denied remediation requires remote management be enabled on this server
    Problem          :
    Impact           :
    Resolution       :
    compliance       :  The File Server Best Practices Analyzer scan has determined that you are in compliance with this best practice.
    help             :
    Excluded         :  False

    ```

    Effectuez l’une des opérations suivantes :

    -   Pour mettre en forme des résultats BPA dans un tableau, exécutez l’applet de commande suivante en ajoutant les propriétés de résultat de l’exemple précédent que vous souhaitez afficher.

        `Get-BPAResult model ID | format-Table -Property <property1,property2,property3...>`

        **Exemple :** `Get-BPAResult Microsoft/Windows/FileServices | format-Table -Property modelId,SubmodelId,computerName,Source,Severity,Category,Title,Problem,Impact,Resolution,compliance,help`

    -   Pour mettre en forme des résultats BPA dans une grille basée sur l’interface graphique utilisateur, avec un filtre pour les chaînes de caractères et des en-têtes de colonne sur lesquels vous pouvez cliquer pour trier des résultats, exécutez l’applet de commande suivante.

        `Get-BPAResult <model ID> | OGV`

    -   Pour exporter les résultats BPA vers un fichier HTML qui peut être archivé ou envoyé à des destinataires de courrier électronique, exécutez l’applet de commande suivante, où *chemin d’accès* représente le chemin d’accès et le nom de fichier dans lequel vous souhaitez enregistrer les résultats html.

        `Get-BPAResult <model ID> | convertTo-Html | Set-Content <path>`

        **Exemple :** `Get-BPAResult Microsoft/Windows/FileServices | convertTo-Html | Set-Content C:\BPAResults\FileServices.htm`

    -   Pour exporter les résultats BPA vers un fichier texte de valeurs séparées par des virgules (CSV), exécutez l’applet de commande suivante, où *chemin d'* accès représente le chemin d’accès et le nom du fichier texte dans lequel vous souhaitez enregistrer les résultats CSV. Les résultats CSV peuvent être importés dans Microsoft Excel ou dans d’autres programmes qui affichent des données dans des feuilles de calcul ou des grilles.

        `Get-BPAResult <model ID> | Export-CSV <path>`

        **Exemple :** `Get-BPAResult Microsoft/Windows/FileServices | Export-CSV C:\BPAResults\FileServices.txt`

## <a name="see-also"></a>Voir aussi
[Best Practices Analyzer contenu de résolution sur le TechCenter Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241597)
[Filtrer, trier et interroger des données dans des vignettes gestionnaire de serveur](filter-sort-and-query-data-in-server-manager-tiles.md)
[gérer plusieurs serveurs distants avec Gestionnaire de serveur](manage-multiple-remote-servers-with-server-manager.md)
