---
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
title: Présentation de la virtualisation des services de domaine Active Directory (AD DS) (niveau 100)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b818ba5a58db38bdb3c0f630a8d9d2daa1494403
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878090"
---
# <a name="introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100"></a>Présentation de la virtualisation des services de domaine Active Directory (AD DS) (niveau 100)

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La virtualisation des environnements de services de domaine Active Directory (AD DS) est un processus engagé depuis de nombreuses années. À compter de Windows Server 2012, les services AD DS fournissent une prise en charge pour la virtualisation de contrôleurs de domaine en introduisant des fonctionnalités de virtualisation-safe.

## <a name="safe-virtualization-of-domain-controllers"></a>Virtualisation sécurisée des contrôleurs de domaine

Les environnements virtuels posent des défis uniques pour les charges de travail distribuées qui dépendent d’un schéma de réplication fondé sur un mécanisme d’horloge logique. Par exemple, la réplication AD DS utilise une valeur qui augmente de manière monotone (appelée « USN » ou « numéro de séquence de mise à jour ») et est affectée aux transactions sur chaque contrôleur de domaine. Instance de base de données de chaque contrôleur de domaine est également attribuer une identité, appelée un ID d’invocation. La valeur InvocationID d’un contrôleur de domaine et sa valeur USN jouent ensemble le rôle d’un identificateur unique associé à chaque transaction en écriture réalisée dans chaque contrôleur de domaine et doivent être uniques au sein de la forêt.

La réplication AD DS exploite des valeurs InvocationID et USN sur chaque contrôleur de domaine afin de déterminer les changements à répliquer sur d’autres contrôleurs de domaine. Si un contrôleur de domaine est restauré à temps en dehors de la reconnaissance du contrôleur de domaine et un USN est réutilisé pour une transaction entièrement différente, la réplication ne fera aucun lien, car les autres contrôleurs de domaine penseront qu’ils ont déjà reçu les mises à jour associé à l’USN réutilisé dans le contexte de cette valeur InvocationID.

Par exemple, l’illustration suivante décrit la séquence des événements qui ont lieu dans Windows Server 2008 R2 et des systèmes d’exploitation antérieurs lorsqu’une restauration USN est détectée sur VDC2, le contrôleur de domaine de destination exécuté sur un ordinateur virtuel. Dans cette illustration, la détection de restauration USN se produit sur VDC2 lorsqu’un partenaire de réplication détecte que VDC2 a envoyé une valeur USN de mise à jour qui a été vu précédemment par le partenaire de réplication, ce qui indique que base de données de VDC2 a été restaurée de manière incorrecte.

![Introduction aux services AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Une machine virtuelle (VM) rend plus facile pour les administrateurs d’hyperviseurs à restaurer un domaine USN du contrôleur (son horloge logique) par, par exemple, application d’un instantané en dehors de la reconnaissance du contrôleur de domaine. Pour obtenir des informations sur les numéros USN et la restauration USN et consulter une autre illustration décrivant les instances de restauration USN non détectées, voir les sections [USN et Restauration USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

À compter de Windows Server 2012, les contrôleurs de domaine virtuels AD DS hébergés sur des plateformes d’hyperviseur qui dévoilent un identificateur appelé ID de génération d’ordinateur virtuel peuvent détecter et employer des mesures de sécurité nécessaires pour protéger l’environnement AD DS si l’ordinateur virtuel est restauré. dans le temps par l’application d’un instantané de machine virtuelle. La structure de l’ID de génération d’ordinateur virtuel repose sur un mécanisme hyperviseur/fournisseur indépendant qui présente l’identificateur dans l’espace d’adressage de l’ordinateur virtuel invité, de sorte que l’expérience de virtualisation sécurisée reste systématiquement disponible depuis chaque hyperviseur prenant en charge les ID de génération d’ordinateur virtuel. Cet identificateur peut être testé par les services et les applications en cours d’exécution sur l’ordinateur virtuel afin de détecter si un ordinateur virtuel a été restauré à temps.

### <a name="BKMK_HowSafeguardsWork"></a>Comment fonctionnent ces dispositifs de protection ?
Pendant l’installation du contrôleur de domaine, les services AD DS stocke d’abord l’identificateur de l’ID de génération de machine virtuelle dans le cadre de l’attribut msDS-GenerationID sur l’objet d’ordinateur du contrôleur de domaine dans sa base de données (souvent appelé l’arborescence d’information d’annuaire ou DIT). L’ID de génération d’ordinateur virtuel est contrôlé de manière indépendante par un pilote Windows au sein de l’ordinateur virtuel.

Lorsqu’un administrateur restaure l’ordinateur virtuel à partir d’une capture instantanée précédente, la valeur actuelle de l’ID de génération d’ordinateur virtuel extraite du pilote de l’ordinateur virtuel est comparée à une valeur au sein du DIT.

Si les deux valeurs sont différentes, l’attribut InvocationID est réinitialisé et le pool RID est rejeté, ce qui empêche toute réutilisation des numéros USN. Si les valeurs sont identiques, la transaction est validée comme une transaction normale.

Les services de domaine Active Directory (AD DS) comparent également la valeur actuelle de l’ID de génération d’ordinateur virtuel provenant de l’ordinateur virtuel à la valeur interne du DIT chaque fois que le contrôleur de domaine est redémarré. S’ils trouvent une valeur différente, ils réinitialisent l’attribut InvocationID, rejettent le pool RID et mettent à jour le DIT pour y inclure la nouvelle valeur. Ils procèdent également à une synchronisation ne faisant pas autorité du dossier SYSVOL pour mener la restauration à terme, en toute sécurité. Les mécanismes de protection peuvent ainsi étendre l’application des captures instantanées sur des ordinateurs virtuels qui ont été arrêtés. Ces mécanismes de protection introduits dans Windows Server 2012 permettent aux administrateurs de services AD DS bénéficier d’avantages uniques inhérents au déploiement et la gestion des contrôleurs de domaine dans un environnement virtualisé.

L’illustration suivante montre comment les dispositifs de protection sont appliqués lorsque la même restauration USN est détectée sur un contrôleur de domaine virtualisé qui exécute Windows Server 2012 sur un hyperviseur qui prend en charge VM-GenerationID.

![Introduction aux services AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

Dans ce cas, dès que l’hyperviseur détecte que la valeur de l’ID de génération d’ordinateur virtuel a été modifiée, les mécanismes de protection de la virtualisation sont déclenchés, y compris la réinitialisation de l’attribut InvocationID pour le contrôleur de domaine virtualisé (de A à B dans l’exemple précédent) et la mise à jour de la valeur de l’ID de génération d’ordinateur virtuel enregistrée sur l’ordinateur virtuel pour correspondre à la nouvelle valeur (G2) stockée par l’hyperviseur. Les mécanismes de protection de la virtualisation s’assurent que la réplication converge pour les deux contrôleurs de domaine.

Avec Windows Server 2012, les services AD DS utilise des dispositifs de protection sur les contrôleurs de domaine virtuels hébergés sur des hyperviseurs prenant en charge VM-GenerationID et garantit que l’application accidentelle de captures instantanées ou d’autres ces mécanismes prenant en charge l’hyperviseur peut restaurer une machine virtuelle état de l’ordinateur n’interrompt pas l’environnement AD DS (en empêchant les problèmes de réplication comme une bulle USN ou des objets en attente). En revanche, la restauration d’un contrôleur de domaine obtenue par application d’une capture instantanée d’ordinateur virtuel n’est pas conseillée et ne constitue pas un mécanisme de secours pour la sauvegarde d‘un contrôleur de domaine. Nous vous recommandons de continuer à utiliser la fonctionnalité Sauvegarde Windows Server ou d’autres solutions de sauvegarde fondées sur l’enregistreur VSS.

> [!CAUTION]
> Si un instantané est rétabli accidentellement un contrôleur de domaine dans un environnement de production, il est recommandé que vous consultiez les éditeurs pour les applications et les services hébergés sur cette machine virtuelle, pour obtenir des conseils sur la vérification de l’état de ces programmes après restauration de capture instantanée.

Pour plus d‘informations, voir [Architecture de restauration sécurisée des contrôleurs de domaine virtualisés](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="virtualized_dc_cloning"></a>Clonage du contrôleur de domaine virtualisés
À compter de Windows Server 2012, les administrateurs peuvent facilement et en toute sécurité déployer contrôleurs de domaine répliqués en copiant un contrôleur de domaine virtuel existant. Dans un environnement virtuel, les administrateurs n’ont plus besoin de déployer à plusieurs reprises une image serveur préparée à l’aide de l’outil sysprep.exe, de promouvoir le serveur sur un contrôleur de domaine, puis d’effectuer des tâches de configuration supplémentaires requises pour le déploiement de chaque contrôleur de domaine répliqué.

> [!NOTE]
> Les administrateurs doivent suivre des processus existants pour déployer le premier contrôleur de domaine dans un domaine, notamment faire appel à l’outil sysprep.exe pour préparer un disque dur virtuel serveur (VHD), promouvoir le serveur sur un contrôleur de domaine, puis réaliser d’autres tâches de configuration obligatoires. Dans un scénario de récupération d’urgence, utilisez la dernière sauvegarde du serveur pour restaurer le premier contrôleur de domaine dans un domaine.

### <a name="scenarios-that-benefit-from-virtual-domain-controller-cloning"></a>Scénarios bénéficiant du clonage des contrôleurs de domaine virtuels

-   Déploiement rapide de contrôleurs de domaine supplémentaires dans un nouveau domaine

-   Reprise rapide des activités après une récupération d’urgence : restauration des fonctions des services de domaine Active Directory (AD DS) par un déploiement accéléré des contrôleurs de domaine grâce au clonage

-   Déploiements optimaux des nuages privés grâce à une configuration souple des contrôleurs de domaine capable de répondre à des besoins de plus grande ampleur

-   Mise en place rapide d’environnements de test favorisant le déploiement et l’évaluation de fonctionnalités et d’outils innovants avant la phase de production

-   Prise en charge rapide des besoins en fonctionnalités des succursales en clonant des contrôleurs de domaine existants au sein même de ces succursales

Lorsque vous procédez au déploiement rapide d’un grand nombre de contrôleurs de domaine, suivez en permanence vos procédures existantes pour valider l’intégrité de chacun des contrôleurs de domaine à l’issue de l’installation. Déployez les contrôleurs de domaine dans des lots de taille raisonnable pour pouvoir valider leur intégrité une fois chaque lot d’installations exécuté. La taille de lot recommandée est 10. Pour plus d’informations, voir [Étapes de déploiement d’un contrôleur de domaine virtualisé clone](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc).

### <a name="clear-separation-of-responsibilities"></a>Séparation claire des responsabilités
L’autorisation de cloner des contrôleurs de domaine virtualisation est contrôlée par l’administrateur des services de domaine Active Directory (AD DS). Pour que les administrateurs d’hyperviseurs puissent déployer des contrôleurs de domaine supplémentaires en copiant des contrôleurs de domaine supplémentaires, l’administrateur des services AD DS doit sélectionner et autoriser un contrôleur de domaine, puis effectuer des étapes préparatoires pour l’activer en tant que source du clonage.

L’approvisionnement des ordinateurs virtuels relevant généralement de la compétence de l’administrateur de l’hyperviseur, les administrateurs d’hyperviseurs peuvent fournir les ordinateurs virtuels des contrôleurs de domaine répliqués en copiant les contrôleurs de domaine virtualisés autorisés et préparés dans l’optique des opérations de clonage de l’administrateur AD DS.

> [!WARNING]
> Toute personne autorisée à administrer l’hyperviseur qui héberge le contrôleur de domaine virtuel doit être une personne de très grande confiance ayant fait l’objet d’un audit dans l’environnement.

### <a name="how-does-virtual-domain-controller-cloning-work"></a>Comment le clonage des contrôleurs de domaine virtuels fonctionne-t-il ?
Le processus de clonage implique de copier de disque dur virtuel du contrôleur de domaine virtuel existant (ou, pour des configurations plus complexes, la machine virtuelle du contrôleur de domaine), autorisant ainsi le clonage dans AD DS et la création d’un fichier de configuration de clonage. Ceci réduit le nombre des étapes et le temps nécessaires au déploiement d’un contrôleur de domaine virtuel répliqué en éliminant toutes les tâches de déploiement répétitives.

Le contrôleur de domaine clone respecte les critères suivants pour détecter s’il s’agit d’une copie d’un autre contrôleur de domaine :

1.  La valeur de l’ID de génération d’ordinateur virtuel fournie par l’ordinateur virtuel est différente de la valeur de l’ID de génération d’ordinateur virtuel stockée dans l’arborescence d’information d’annuaire (DIT).

    > [!NOTE]
    > La plateforme d’hyperviseur doit prendre en charge les ID de génération de la machine virtuelle (Windows Server 2012 Hyper-V prend en charge les ID de génération de la machine virtuelle).

2.  Présence d’un fichier appelé « DCCloneConfig.xml » à l’un des emplacements suivants :

    -   Répertoire dans lequel réside le DIT

    -   %windir%\NTDS

    -   Racine d’un lecteur de média amovible

Une fois les critères remplis, il exécute le processus de clonage pour s’imposer en tant que contrôleur de domaine répliqué.

Le contrôleur de domaine clone utilise le contexte de sécurité du contrôleur de domaine source (le contrôleur de domaine dont il représente la copie) pour contacter le détenteur de rôle de maître de Windows Server 2012 domaine contrôleur principal (PDC) émulateur opérations (également appelé opérations à maître uniques flottant ou FSMO). L’émulateur de contrôleur de domaine principal doit exécuter Windows Server 2012, mais il n’a pas d’être exécuté sur un hyperviseur.

> [!NOTE]
> Si vous disposez d’une extension de schéma avec des attributs faisant référence au contrôleur de domaine source et si l’attribut figure dans l’un des objets copiés (objet ordinateur, objet Paramètres NTDS) pour créer le clone, cet attribut ne sera pas copié ou mis à jour pour faire référence au contrôleur de domaine clone.

Après avoir vérifié que le contrôleur de domaine à l’origine de la demande est autorisé pour le clonage, l’émulateur PDC crée une nouvelle identité d’ordinateur comprenant un nouveau compte, un identificateur de sécurité (SID), un nom et un mot de passe. Elle identifie cet ordinateur en tant que contrôleur de domaine répliqué et transmet ces informations au clone. Le contrôleur de domaine clone prépare ensuite les fichiers de base de données AD DS pour qu’ils servent de réplique et il procède également à un nettoyage de l’état de l’ordinateur.

Pour plus d’informations, voir [Architecture de clonage des contrôleurs de domaine virtualisés](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch).

### <a name="cloning-components"></a>Composants de clonage
Les composants de clonage incluent des nouvelles applets de commande du module Active Directory pour Windows PowerShell et des fichiers XML associés :

-   **New-ADDCCloneConfigFile** « cette applet de commande crée et place le fichier DCCloneConfig.xml au bon emplacement pour s’assurer qu’il est disponible pour déclencher le clonage. Elle procède également à des vérifications de configuration requise pour garantir un clonage en bonne et due forme. Elle apparaît dans le module Active Directory pour Windows PowerShell. Vous pouvez l’exécuter localement dans un contrôleur de domaine virtualisé préparé à des fins de clonage ou bien l’exécuter à distance à l’aide de l’option -offline. Vous pouvez spécifier des paramètres pour le contrôleur de domaine clone (par exemple, son nom, son site et son adresse IP).

    Les vérifications de configuration requise effectuées sont les suivantes :

    > [!NOTE]
    > La vérification des composants ne sont pas effectuées lorsque le « option hors connexion est utilisée. Pour plus d'informations, voir [Running New-ADDCCloneConfigFile in offline mode](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode).

    -   Le contrôleur de domaine en cours de préparation est autorisé pour le clonage (membre du groupe **Contrôleurs de domaine clonables**)

    -   L’émulateur de contrôleur de domaine principal exécute Windows Server 2012.

    -   Tous les programmes et services répertoriés avec l’exécution de l’applet de commande **Get-ADDCCloningExcludedApplicationList** sont inclus dans CustomDCCloneAllowList.xml (fichier détaillé à la fin de cette liste des composants de clonage).

-   **DCCloneConfig.xml** » pour cloner un contrôleur de domaine virtualisé, ce fichier doit être présent dans le répertoire où réside le DIT, *%windir%\NTDS*, ou la racine d’un lecteur de média amovible. Ce fichier est l’un des composants qui permettent de déclencher les processus de détection et de lancement du clonage. Mais il offre aussi le moyen de préciser les paramètres de configuration du contrôleur de domaine clone.

    Le schéma et un exemple de fichier pour le fichier DCCloneConfig.xml sont stockés sur tous les ordinateurs Windows Server 2012 :

    -   %windir%\system32\DCCloneConfigSchema.xsd

    -   %windir%\system32\SampleDCCloneConfig.xml

    Nous vous recommandons d’utiliser l‘applet de commande New-ADDCCloneConfigFile pour créer le fichier DCCloneConfig.xml. Il est possible d’utiliser le fichier de schéma avec un éditeur compatible XML pour créer ce fichier mais toute modification manuelle du fichier augmente le risque d’erreurs. Si vous modifiez le fichier, vous devez le faire à l’aide d’éditeurs compatibles XML, tels que Visual Studio, [XML Notepad](https://www.microsoft.com/download/details.aspx?displaylang=en&id=7973)ou des applications tierces (n’utilisez pas le Bloc-notes).

-   **Get-ADDCCloningExcludedApplicationList** « cette applet de commande est exécutée sur le contrôleur de domaine source avant de commencer le processus de clonage pour déterminer quels services ou les programmes installés ne sont pas dans la liste par défaut pris en charge, DefaultDCCloneAllowList.xml ou une inclusion définie par l’utilisateur liste nommée fichier CustomDCCloneAllowList.xml et ainsi n'ont pas été évaluées pour le clonage d’impact.

    Cette applet de commande recherche des services le gestionnaire de contrôle des services, au sein du contrôleur de domaine source, ainsi que les programmes installés et répertoriés sous **HKLM\Software\Microsoft\Windows\CurrentVersion\Uninstall** qui n’apparaissent pas dans la liste par défaut (DefaultDCCloneAllowList.xml) ou, le cas échéant, dans la liste d’inclusion définie par l’utilisateur (fichier CustomDCCloneAllowList.xml). La liste des applications et des services qui est renvoyée après exécution de l’applet de commande dévoile les différences entre les éléments déjà fournis par le fichier DefaultDCCloneAllowList.xml ou le fichier CustomDCCloneAllowList.xml et la liste créée au moment de l’exécution, en fonction de ce qui est installé dans le contrôleur de domaine source. Les services et les programmes obtenus avec l’applet de commande Get-ADDCCloningExcludedApplicationList peuvent être ajoutés au fichier CustomDCCloneAllowList.xml si vous déterminez qu’ils peuvent être clonés en toute sécurité. Pour déterminer si un service ou un programme installé peut être cloné sans risque, vérifiez les conditions suivantes :

    -   Le service ou le programme installé est-il affecté par l’identité de l’ordinateur, comme son nom, son identificateur de sécurité (SID), son mot de passe, etc. ?

    -   Le service ou le programme installé stocke-t-il localement sur l’ordinateur des états susceptibles de gêner son fonctionnement sur le clone ?

    Vous devez collaborer avec l’éditeur de l’application pour évaluer si le service ou programme peut être cloné sans danger.

    > [!NOTE]
    > Avant de communiquer des services ou des programmes supplémentaires au fichier CustomDCCloneAllowList.xml, vérifiez que vous disposez de la licence adéquate pour copier le logiciel que contient cet ordinateur virtuel.

    Si les applications ne sont pas clonables, supprimez-les du contrôleur de domaine source avant de créer le clone. Si une application apparaît dans la sortie de l’applet de commande mais n’est pas incluse dans le fichier CustomDCCloneAllowList.xml, le clonage échouera. Pour que le clonage aboutisse, la sortie de l’applet de commande ne doit pas répertorier tous les services ou programmes. En d’autres termes, une application doit soit être incluse dans le fichier CustomDCCloneAllowList.xml, soit être supprimée du contrôleur de domaine source.

    Le tableau qui suit décrit les options d’exécution de l’applet de commande Get-ADDCCloningExcludedApplicationList.

    |||
    |-|-|
    |Argument|Explication|
    |*<no argument specified>*|Affiche une liste des services ou des programmes de la console qui n’ont pas été pris en compte pour le clonage. S’il existe déjà un fichier CustomDCCloneAllowList.XML à l’un des emplacements autorisés, l’argument utilise ce fichier pour afficher les services et les programmes restants (avec un résultat nul si les listes correspondent).|
    |-GenerateXml|Crée le fichier CustomDCCloneAllowList.XML renseigné avec les services et les programs répertoriés dans la console.|
    |-Force|Remplace un fichier CustomDCCloneAllowList.XML existant.|
    |-Path|Chemin d’accès au dossier dans lequel le fichier CustomDCCloneAllowList.XML est créé.|

-   **DefaultDCCloneAllowList.xml** « ce fichier est présent par défaut sur chaque copie de Windows Server 2012 domaine contrôleur in les *%windir%\system32*. Il dresse la liste des services et des programmes installés par défaut qu’il est possible de cloner sans risque. Vous ne devez pas modifier l’emplacement ni le contenu de ce fichier sinon le clonage échouera.

-   **CustomDCCloneAllowList.xml** « si vous avez des services ou programmes installés résidant sur votre contrôleur de domaine source qui sont trouvent en dehors de celles répertoriées dans le fichier DefaultDCCloneAllowList.xml, ces services et programmes doivent être inclus dans ce fichier. Pour rechercher des services ou programmes installés non répertoriés dans le fichier DefaultDCCloneAllowList.xml, exécutez l’applet de commande **Get-ADDCCloningExcludedApplicationList** . Vous devez utiliser le **» GenerateXml** argument pour générer le fichier XML.

    Le processus de clonage vérifie les emplacements suivants dans l’ordre pour ce fichier et exploite le premier fichier XML qu’il détecte sans tenir compte du contenu de l’autre dossier :

    1.  La clé de Registre suivante :

        ```
        HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters
        AllowListFolder (REG_SZ)
        ```

    2.  DSA Working Directory (répertoire de travail DSA)

    3.  %systemroot%\NTDS

    4.  Médias de lecture/écriture amovibles, en fonction de l’ordre de la lettre qui identifie le lecteur, sur la racine du lecteur.

### <a name="deployment-scenarios"></a>Scénarios de déploiement
Les scénarios de déploiement qui suivent sont pris en charge pour le clonage des contrôleurs de domaine virtuels :

-   Déployer un contrôleur de domaine clone en effectuant une copie du fichier de disque dur virtuel (vhd) d’un contrôleur de domaine source.

-   Déployez un contrôleur de domaine clone en copiant l’ordinateur virtuel d’un contrôleur de domaine à l’aide des fonctions d’importation/exportation proposées par l’hyperviseur.

> [!NOTE]
> Les étapes décrites dans la section [étapes de déploiement d’un contrôleur de domaine virtualisé clone](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc) montrent comment copier une machine virtuelle à l’aide de la fonctionnalité d’exportation/importation de Windows Server 2012 Hyper-V.

## <a name="steps_deploy_vdc"></a>Étapes de déploiement d’un contrôleur de domaine virtualisé clone

-   [Conditions préalables](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#prerequisites)

-   [Étape 1 : Accordez le contrôleur de domaine virtualisé source l’autorisation d’être cloné](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk4_grant_source)

-   [Étape 2 : Exécutez l’applet de commande Get-ADDCCloningExcludedApplicationList](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet)

-   [Étape 3 : Exécuter New-ADDCCloneConfigFile](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk5_create_insert_dccloneconfig)

-   [Étape 4 : Exporter et importer l’ordinateur virtuel du contrôleur de domaine source](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk7_export_import_vm_sourcedc)

### <a name="prerequisites"></a>Conditions préalables

-   Pour effectuer les étapes des procédures suivantes, vous devez être membre du groupe Administrateurs du domaine ou bien disposer des autorisations équivalentes qui lui ont été octroyées.

-   Les commandes Windows PowerShell utilisées dans ce guide doivent être exécutées à partir d’une invite de commandes avec élévation de privilèges. Pour ce faire, cliquez droit sur le **Windows PowerShell** icône, puis cliquez sur **exécuter en tant qu’administrateur**.

-   Un serveur Windows Server 2012 avec le rôle de serveur Hyper-V installé (**HyperV1**).

-   Un second serveur Windows Server 2012 avec le rôle de serveur Hyper-V installé (**HyperV2**).

    > [!NOTE]
    > -   Si vous utilisez un autre hyperviseur, vous devez contacter son fournisseur pour vérifier si cet hyperviseur prend en charge l’ID de génération d’ordinateur virtuel. Si l’hyperviseur ne prend pas en charge l’ID de génération d’ordinateur virtuel et si vous avez précisé un fichier DCCloneConfig.xml, le nouvel ordinateur virtuel démarrera en mode de restauration des services d’annuaire (DSRM).
    > -   Pour accroître la disponibilité des services de domaine Active Directory (AD DS), ce guide recommande (et fournit des instructions dans ce sens) d’utiliser deux hôtes Hyper-V, ce qui empêche tout point de défaillance unique éventuel. En revanche, vous n’avez pas besoin de deux hôtes Hyper-V pour cloner des contrôleurs de domaine virtuels.
    > -   Vous devez être membre du groupe Administrateurs local sur chaque serveur Hyper-V (**HyperV1** et **HyperV2**).
    > -   Pour pouvoir importer et exporter avec succès un fichier VHD avec Hyper-V, les commutateurs réseau virtuel sur les deux hôtes Hyper-V doivent porter le même nom. Par exemple, si vous disposez d’un commutateur réseau virtuel sur l’hôte **HyperV1** appelé VNet, vous avez alors besoin d’un commutateur réseau virtuel également nommé VNet sur l’hôte **HyperV2**.
    > -   Si les deux hôtes Hyper-V (**HyperV1** et **HyperV2**) ont des processeurs différents, arrêtez l’ordinateur virtuel (**VirtualDC1**) que vous cherchez à exporter, cliquez avec le bouton droit sur l’ordinateur virtuel, cliquez sur **Paramètres** et sur **Processeur**, puis sous **Compatibilité du processeur**, sélectionnez **Migrer vers un ordinateur physique ayant une autre version de processeur** et cliquez sur **OK**.

-   Un déployé Windows Server 2012 contrôleur de domaine (virtualisé ou physique) qui héberge le rôle d’émulateur PDC (**DC1**). Pour vérifier si le rôle d’émulateur de contrôleur de domaine principal est hébergé sur un contrôleur de domaine Windows Server 2012, exécutez la commande Windows PowerShell suivante :

    ```
    Get-ADComputer (Get-ADDomainController "Discover "Service "PrimaryDC").name "Property operatingsystemversion | fl
    ```

    La valeur OperatingSystemVersion renvoyée doit apparaître en tant que version 6.2. Si nécessaire, vous pouvez transférer le rôle d’émulateur PDC à un contrôleur de domaine qui exécute Windows Server 2012. Pour plus d’informations, voir [Utilisation de Ntdsutil.exe pour prendre ou transférer des rôles FSMO vers un contrôleur de domaine](https://support.microsoft.com/kb/255504).

-   Un contrôleur de domaine virtualisé invité Windows Server 2012 déployé (**VirtualDC1**) qui se trouve dans le même domaine que le contrôleur de domaine Windows Server 2012 hébergeant le rôle d’émulateur PDC (**DC1**). Il s’agit du contrôleur de domaine source qui sera utilisé pour le clonage. Le contrôleur de domaine virtuel invité sera hébergé sur un serveur Windows Server 2012 Hyper-V (**HyperV1**).

    > [!NOTE]
    > -   Pour que le clonage réussisse, le contrôleur de domaine source qui sert à créer le clone ne peut provenir d’un contrôleur de domaine qui a été rétrogradé puisque le support VHD source a été créé.
    > -   Arrêtez le contrôleur de domaine source avant de copier l’ordinateur virtuel ou son disque dur virtuel (VHD).
    > -   Vous ne devez pas cloner un disque dur virtuel ni restaurer une capture instantanée plus ancienne que la valeur de la durée de vie de temporisation (ou celle de la durée de vie de l’objet supprimé si la corbeille Active Directory est activée). Si vous copiez le disque dur virtuel d’un contrôleur de domaine existant, assurez-vous que le fichier VHD n’est pas antérieur à la valeur de durée de vie de temporisation (par défaut, 60 jours). Vous ne devez pas copier le disque dur virtuel d’un contrôleur de domaine en cours d’exécution pour créer un clone.

    Éjectez tous les lecteurs de disquette virtuels (VFD) que peut comporter le contrôleur de domaine source. Ceci peut entraîner un problème de partage lors de la tentative d’importation du nouvel ordinateur virtuel.

    Uniquement les contrôleurs de domaine Windows Server 2012 hébergés sur un hyperviseur de VM-GenerationID peuvent être utilisés en tant que source pour le clonage. Le contrôleur de domaine source Windows Server 2012 utilisé pour le clonage doit être dans un état sain. Pour déterminer l’état du contrôleur de domaine source, exécutez l’outil [dcdiag](https://technet.microsoft.com/library/cc731968(WS.10).aspx). Pour une meilleure compréhension de la sortie retournée par l’outil dcdiag, voir [mais que fait l’outil DCDIAG... faire ? ](http://blogs.technet.com/b/askds/archive/2011/03/22/what-does-dcdiag-actually-do.aspx).

    Si le contrôleur de domaine source est un serveur DNS, le contrôleur de domaine cloné sera aussi un serveur DNS. Vous devez choisir un serveur DNS qui héberge exclusivement des zones intégrées dans Active Directory.

    Les paramètres DNS clients ne sont pas clonés mais sont précisés dans le fichier DCCloneConfig.xml à la place. S’ils ne sont pas spécifiés, le contrôleur de domaine cloné pointera sur lui-même en tant que serveur DNS préféré par défaut. Le contrôleur de domaine cloné n’aura aucune délégation DNS. Si nécessaire, l’administrateur de la zone DNS parente doit mettre à jour la délégation DNS du contrôleur de domaine cloné.

    > [!WARNING]
    > Les mécanismes de protection de la virtualisation ne s’étendent pas aux services AD LDS (Active Directory Lightweight Directory Services). Par conséquent, vous ne devez pas essayer de cloner un contrôleur de domaine AD DS qui héberge une instance AD LDS en ajoutant celle-ci au fichier CustomDCCloneAllowList.xml. Du fait que les services AD LDS ne prennent pas en charge l’ID de génération d’ordinateur virtuel, le clonage d’un contrôleur de domaine à l’aide de ces services peut entraîner une divergence liée à la restauration USN dans ce jeu de configuration AD LDS.

    Les rôles serveur suivants ne sont pas pris en charge pour le clonage :

    -   Protocole DHCP (Dynamic Host Configuration Protocol)

    -   Services de certificats Active Directory (AD CS)

    -   Services AD LDS (Active Directory Lightweight Directory Services)

### <a name="bkmk4_grant_source"></a>Étape 1 : accorder au contrôleur de domaine virtualisé source l’autorisation d’être cloné
Dans cette procédure, vous autorisez le contrôleur de domaine source à être cloné par le biais du **Centre d’administration Active Directory** afin d’ajouter ce contrôleur de domaine source au groupe **Contrôleurs de domaine clonables**.

##### <a name="to-grant-the-source-virtualized-domain-controller-the-permission-to-be-cloned"></a>Pour autoriser le contrôleur de domaine virtualisé source à être cloné

1.  Sur tout contrôleur de domaine appartenant au même domaine que le contrôleur de domaine que vous préparez à des fins de clonage (**VirtualDC1**), ouvrez le **Centre d’administration Active Directory** (ADAC), recherchez l’objet contrôleur de domaine virtualisé (les contrôleurs de domaine se trouvent généralement sous le conteneur **Contrôleurs de domaine** dans le centre ADAC), cliquez dessus avec le bouton droit, choisissez **Ajouter au groupe**, puis sous **Entrez le nom de l’objet à sélectionner**, tapez **Contrôleurs de domaine clonables** et cliquez sur **OK**.

    La mise à jour d’appartenance à un groupe effectuée à cette étape doit répliquer l’émulateur de contrôleur de domaine principal (PDC) avant que le clonage n’ait lieu. Si le **contrôleurs de domaine clonables** groupe est introuvable, le rôle d’émulateur PDC ne peut pas être hébergé sur un contrôleur de domaine qui exécute Windows Server 2012.

    > [!NOTE]
    > Pour ouvrir ADAC sur un contrôleur de domaine Windows Server 2012, ouvrez Windows PowerShell, tapez **dsac.exe**.

![Introduction aux services AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***

L’applet de commande Windows PowerShell qui suit permet de réaliser la même fonction que la procédure précédente :


    Add-ADGroupMember "Identity "CN=Cloneable Domain Controllers,CN=Users, DC=Fabrikam,DC=Com" "Member "CN=VirtualDC1,OU=Domain Controllers,DC=Fabrikam,DC=com"


### <a name="bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet"></a>Étape 2 : exécuter l’applet de commande Get-ADDCCloningExcludedApplicationList
Pour cette procédure, exécutez l’applet de commande `Get-ADDCCloningExcludedApplicationList` dans le contrôleur de domaine virtualisé source afin d’identifier tous les programmes ou services qui ne sont pas évalués pour le clonage. Vous devez exécuter l’applet de commande Get-ADDCCloningExcludedApplicationList avant l’applet de commande New-ADDCCloneConfigFile car si cette dernière détecte une application exclue, aucun fichier DCCloneConfig.xml ne sera créé.

##### <a name="to-identify-applications-or-services-that-run-on-a-source-domain-controller-which-have-not-been-evaluated-for-cloning"></a>Pour identifier des applications ou des services qui fonctionnent sur un contrôleur de domaine source qui n’a pas été évalué pour le clonage

1.  Dans le contrôleur de domaine source (**VirtualDC1**), cliquez successivement sur **Gestionnaire de serveur**, sur **Outils**et sur **Module Active Directory pour Windows PowerShell** , puis tapez la commande suivante :


    Get-ADDCCloningExcludedApplicationList


2.  Vérifiez la liste des services renvoyés et des programmes installés auprès de l’éditeur afin de déterminer s’ils peuvent être clonés sans danger. S’il s’avère impossible de cloner les applications ou les services de la liste en toute sécurité, vous devez les supprimer dans le contrôleur de domaine source sans quoi le clonage échouera.

3.  Pour l’ensemble des services et des programmes installés qui ont été déterminés soient clonés sans risque, exécutez la commande à l’aide de la **» GenerateXML** basculer vers ces services et programmes dans le **CustomDCCloneAllowList.xml**  fichier.


    Get-ADDCCloningExcludedApplicationList -GenerateXml


### <a name="bkmk5_create_insert_dccloneconfig"></a>Étape 3 : Exécuter New-ADDCCloneConfigFile
Exécutez New-ADDCCloneConfigFile sur le contrôleur de domaine source et précisez éventuellement les paramètres de configuration du contrôleur de domaine clone, notamment le nom, l’adresse IP et la résolution DNS.

Par exemple, pour créer un contrôleur de domaine clone appelé VirtualDC2 avec une adresse IPv4 statique, tapez :


    New-ADDCCloneConfigFile "Static -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.255.0" -CloneComputerName "VirtualDC2" -IPv4DefaultGateway "10.0.0.3" -SiteName "REDMOND"

> [!NOTE]
> Le contrôleur de domaine clone sera situé sur le même site que le contrôleur de domaine source sauf si vous précisez un site différent dans le fichier DCCloneConfig.xml. Nous vous conseillons de spécifier un site adapté au fichier DCCloneConfig.xml du contrôleur de domaine clone en fonction de son adresse IP.

Le nom de l’ordinateur est facultatif. Si vous n’en précisez aucun, un nom unique sera généré à partir de l’algorithme suivant :

-   Le préfixe comprend les 8 premiers caractères du nom d’ordinateur du contrôleur de domaine source. Par exemple, le nom d’ordinateur source OrdinateurSource sera tronqué pour former une chaîne de préfixe plus courte (par exemple, OrSource).

-   Un suffixe unique d’affectation de noms du format » « CL*nnnn*» est ajouté à la chaîne de préfixe où *nnnn* est la valeur suivante disponible entre 0001 et 9999 que le contrôleur de domaine principal détermine n’est pas actuellement en cours d’utilisation. Par exemple, si 0047 est le nombre disponible suivant dans la plage autorisée, en reprenant le préfixe de nom d’ordinateur « OrSource » présenté dans le précédent exemple, le nom obtenu et à utiliser pour l’ordinateur clone sera OrSource-CL0047.

> [!NOTE]
> Pour fonctionner correctement, l’applet de commande New-ADDCCloneConfigFile nécessite un serveur de catalogue global. L’appartenance du contrôleur de domaine source dans le **contrôleurs de domaine clonables** groupe doit être reflété dans le catalogue global. Le serveur de catalogue global ne doit pas nécessairement correspondre au même contrôleur de domaine que l’émulateur de contrôleur de domaine principal (PDC) mais il est préférable qu’il se trouve sur le même site. Si un catalogue global n’est pas disponible, la commande échoue avec l’erreur « le serveur n’est pas opérationnel ». Pour plus d'informations, voir [Virtualized Domain Controller Troubleshooting](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).

Pour créer un contrôleur de domaine clone appelé Clone1 avec des paramètres IPv4 statiques et préciser des serveurs WINS de préférence et secondaires, tapez :


    New-ADDCCloneConfigFile "CloneComputerName "Clone1" "Static -IPv4Address "10.0.0.5" "IPv4DNSResolver "10.0.0.1" "IPv4SubnetMask "255.255.0.0" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


> [!NOTE]
> Si vous spécifiez des serveurs WINS, vous devez spécifier les **» PreferredWINSServer** et **» AlternateWINSServer**. Si vous spécifiez un seul de ces arguments, le clonage échouera avec le code d’erreur 0x80041005 visible dans le journal dcpromo.log.

Pour créer un contrôleur de domaine clone appelé Clone2 avec des paramètres IPv4 dynamiques, tapez :


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" 


> [!NOTE]
> Dans ce cas, l’environnement doit être pourvu d’un serveur DHCP auquel le clone peut accéder et où il peut se procurer une adresse IP et d’autres paramètres réseau pertinents.

Pour créer un contrôleur de domaine clone appelé Clone2 avec des paramètres IPv4 dynamiques et préciser des serveurs WINS de préférence et secondaires, tapez :


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" -SiteName "REDMOND" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


Pour créer un contrôleur de domaine clone avec des paramètres IPv6 dynamiques, tapez :


    New-ADDCCloneConfigFile -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


Pour créer un contrôleur de domaine clone avec des paramètres IPv6 statiques, tapez :


    New-ADDCCloneConfigFile "Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


> [!NOTE]
> Lorsque vous spécifiez des paramètres IPv6, la seule différence entre les paramètres statiques et dynamiques est l’inclusion de **-statique** basculer. L’inclusion de la **-statique** commutateur rend obligatoire de spécifier au moins un **IPv6DNSResolver**. L’adresse IPv6 statique doit habituellement être configurée via la configuration d’automatique d’adresse sans état (SLAAC) avec des préfixes routeur attribués. Avec IPv6 dynamiques, les programmes de résolution DNS sont facultatifs, mais il est probable que le clone peut atteindre un serveur DHCP compatible IPv6 sur le sous-réseau pour obtenir une adresse IPv6 et les informations de configuration de DNS.

#### <a name="BKMK_OfflineMode"></a>New-ADDCCloneConfigFile en cours d’exécution en mode hors connexion
Si vous disposez de plusieurs copies du contrôleur de domaine source qui ont été préparées à des fins de clonage (ce qui signifie que le contrôleur de domaine source est autorisé pour le clonage, que l’applet de commande Get-ADDCCloningExcludedApplicationList a été exécutée, et ainsi de suite) et si vous souhaitez préciser des paramètres distincts pour chaque copie, vous pouvez exécuter New-ADDCCloneConfigFile en mode hors connexion. Cette approche peut s’avérer plus efficace que de préparer individuellement chaque ordinateur virtuel, par exemple en important chaque copie.

Dans ce cas, les administrateurs de domaine peuvent monter le disque hors connexion et utiliser les outils d’Administration de serveur distant (RSAT) pour exécuter l’applet de commande New-ADDCCloneConfigFile avec l’argument - offline afin d’ajouter les fichiers XML, permettant ainsi d’automation de la fabrique de type à l’aide de new Options de Windows PowerShell incluses dans Windows Server 2012. Pour plus d’informations sur le montage du disque hors connexion pour l’exécution hors connexion de l’applet de commande New-ADDCCloneConfigFile, voir [Adding XML to the Offline System Disk](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_Offline).

Vous devez d’abord exécuter l’applet de commande localement sur le média source pour vous assurer que les vérifications de configuration requise sont correctes. Les vérifications de configuration requise ne sont pas réalisées en mode hors connexion car l’applet de commande peut avoir été exécutée sur un ordinateur susceptible de ne pas provenir du même domaine ou d’un ordinateur appartenant à un domaine. Après l’exécution locale de l’applet de commande, un fichier DCCloneConfig.xml est créé. Vous pouvez supprimer le fichier DCCloneConfig.xml qui est créé localement si vous prévoyez de travailler en mode hors connexion par la suite.

Pour créer un contrôleur de domaine clone appelé CloneDC1 en mode hors connexion, sur un site nommé REDMOND » avec l’adresse IPv4 statique, type :


    New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS


Pour créer un contrôleur de domaine clone appelé Clone2 en mode hors connexion avec des paramètres IPv4 et IPv6 statiques, tapez :


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS


Pour créer un contrôleur de domaine clone en mode hors connexion avec des paramètres IPv4 statiques et des paramètres IPv6 dynamiques et spécifier plusieurs serveurs DNS pour les paramètres de résolution DNS, tapez :


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS 


Pour créer un contrôleur de domaine clone appelé Clone1 en mode hors connexion avec des paramètres IPv4 dynamiques et des paramètres IPv6 statiques, tapez :


    New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS


Pour créer un contrôleur de domaine clone en mode hors connexion avec des paramètres IPv4 et IPv6 dynamiques, tapez :


    New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS


### <a name="bkmk7_export_import_vm_sourcedc"></a>Étape 4 : exporter et importer ensuite l’ordinateur virtuel du contrôleur de domaine source
Dans cette procédure, vous allez exporter l’ordinateur virtuel du contrôleur de domaine virtualisé source, puis importer l’ordinateur virtuel. Cette action permet de créer un contrôleur de domaine virtualisé clone sur votre domaine.

Vous devez être membre du groupe Administrateurs local sur chaque hôte Hyper-V. Si vous utilisez des informations d’identification différentes pour chaque serveur, exécutez les applets de commande Windows PowerShell pour exporter et importer l’ordinateur virtuel dans des sessions Windows PowerShell distinctes.

S’il existe des captures instantanées dans le contrôleur de domaine source, vous devez les supprimer avant d’exporter le contrôleur de domaine source car l’ordinateur virtuel ne sera pas importé si une capture instantanée possède des paramètres de processeur qui ne sont pas compatibles avec l’hôte Hyper-V cible. Si les paramètres de processeur sont compatibles entre les hôtes Hyper-V source et cible, vous pouvez exporter et copier la source sans supprimer les captures instantanées au préalable. En revanche, après l’importation, les captures instantanées devront être supprimées de l’ordinateur virtuel clone avant le démarrage de celui-ci.

##### <a name="to-copy-a-virtual-domain-controller-by-exporting-and-then-importing-the-virtualized-source-domain-controller"></a>Pour copier un contrôleur de domaine virtuel par exportation puis importation du contrôleur de domaine source virtualisé

1.  Sur le serveur **HyperV1**, arrêtez le contrôleur de domaine source (**VirtualDC1**).

    ![Introduction aux services AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***

    Stop-VM -Name VirtualDC1 -ComputerName HyperV1


2.  Sur **HyperV1**, supprimez les captures instantanées, puis exportez le contrôleur de domaine source (VirtualDC1) dans le répertoire c:\CloneDCs.

> [!NOTE]
> Vous devez supprimer toutes les captures instantanées associées puisque lors de chaque capture instantanée, un nouveau fichier AVHD faisant office de disque de différenciation est créé. Ceci provoque une réaction en chaîne. Si vous avez réalisé des captures instantanées et insérez le fichier DCCLoneConfig.xml dans le disque dur virtuel (fichier VHD), vous risquez de créer un clone à partir d’une ancienne version du DIT ou d’insérer le fichier de configuration dans le mauvais fichier VHD. La suppression de la capture instantanée entraîne la fusion de l’ensemble de ces fichiers AVHD dans le fichier VHD de base.

![Introduction aux services AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***


    Get-VMSnapshot VirtualDC1 | Remove-VMSnapshot -IncludeAllChildSnapshots
    Export-VM -Name VirtualDC1 -ComputerName HyperV1 -Path c:\CloneDCs\VirtualDC1


3.  Copiez le dossier **virtualdc1** dans le répertoire c:\Import de **HyperV2**.

4.  Sur **HyperV2**, à l’aide du **Gestionnaire Hyper-V**, importez l’ordinateur virtuel (servez-vous de l’Assistant **Importation d‘ordinateur virtuel** du **Gestionnaire Hyper-V**) à partir du dossier **c:\Import\virtualdc1** et supprimez toutes les **captures instantanées** associées.

Utilisez l’option **Copier l’ordinateur virtuel (créer un ID unique)** au moment d’importer l’ordinateur virtuel.

![Introduction aux services AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***

    $path = Get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual Machines"
    $vm = Import-VM -Path $path.fullname -Copy -GenerateNewId
    Rename-VM $vm VirtualDC2


Pour créer plusieurs contrôleurs de domaine clones à partir du même contrôleur de domaine source :

  -   Interface utilisateur : dans l’Assistant **Importation d‘ordinateur virtuel**, spécifiez des nouveaux emplacements pour le **dossier de configuration de l’ordinateur virtuel**, le **magasin de captures instantanées** et le **dossier de pagination intelligente**, puis un **emplacement** distinct pour les disques durs virtuels de l’ordinateur virtuel.

  -   Windows PowerShell : précisez de nouveaux emplacements pour la machine virtuelle en utilisant les paramètres suivants pour le `Import-VM` applet de commande :

        $path = get-ChildItem « C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual Machines » Import-VM-chemin d’accès $path.fullname - copie - GenerateNewId - VhdDestinationPath - ComputerName HyperV2 « path » - SnapshotFilePath « path » - SmartPagingFilePath « path » - VirtualMachinePath « path »


> [!NOTE]
> La taille de lot recommandée pour la création simultanée de plusieurs contrôleurs de domaine clones est 10. Le nombre maximal est limité par le nombre maximal de connexions de réplications sortantes, soit par défaut 16 pour la réplication du système de fichiers DFS et 10 pour le service de réplication de fichiers (FRS). Vous ne devez pas déployer simultanément plus de contrôleurs de domaine clones que le nombre recommandé sauf si vous avez soigneusement testé ce nombre pour votre environnement.

5.  Sur **HyperV1**, redémarrez le contrôleur de domaine source (**(VirtualDC1**) pour le réactiver en ligne.

![Introduction aux services AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***

    Start-VM -Name VirtualDC1 -ComputerName HyperV1


6.  Sur **HyperV2**, démarrez l’ordinateur virtuel (**VirtualDC2**) pour le remettre en ligne en tant que contrôleur de domaine clone dans le domaine.

![Introduction aux services AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***


    Start-VM -Name VirtualDC2 -ComputerName HyperV2

> [!NOTE]
> L’émulateur de contrôleur de domaine principal (PDC) doit être exécuté pour que le clonage aboutisse. S’il s’est arrêté, assurez-vous qu’il a redémarré et a procédé à la synchronisation initiale et qu’il sait donc qu’il assume le rôle d’émulateur de contrôleur de domaine principal (PDC). Pour plus d’informations, voir l’[article 305476 de la Base de connaissances Microsoft](https://support.microsoft.com/kb/305476).

Une fois le clonage terminé, vérifiez le nom de l’ordinateur clone pour vous assurer que l’opération de clonage est réussie. Assurez-vous que l’ordinateur virtuel n’a pas démarré en mode de restauration des services d’annuaire (DSRM). Si vous tentez d’ouvrir une session et recevez une erreur indiquant qu’aucun serveur d’ouverture de session n’est disponible, essayez d’ouvrir une session en mode DSRM. Si le contrôleur de domaine n’a pas été correctement cloné et démarre en mode DSRM, vérifiez les journaux dans l’Observateur d’événements, ainsi que les journaux dcpromo dans le dossier %systemroot%/debug.

Le contrôleur de domaine cloné sera un membre du groupe **Contrôleurs de domaine clonables** puisqu’il copie l’appartenance à partir du contrôleur de domaine source. La meilleure approche est de laisser le groupe **Contrôleurs de domaine clonables** vide jusqu’à ce que vous soyez prêt à mener des opérations de clonage. Il est préférable aussi de supprimer les membres à l’issue des opérations de clonage.

Si le contrôleur de domaine source stocke un média de sauvegarde, le contrôleur de domaine cloné stockera aussi ce média de sauvegarde. Vous pouvez exécuter `wbadmin get versions` pour afficher le média de sauvegarde dans le contrôleur de domaine cloné. Un membre du groupe Administrateurs du domaine doit supprimer le média de sauvegarde dans le contrôleur de domaine cloné pour empêcher qu’il soit accidentellement restauré. Pour plus d’informations sur la suppression d’une sauvegarde de l’état du système au moyen de l’outil wbadmin.exe, voir [Wbadmin delete systemstatebackup](https://technet.microsoft.com/library/cc742081(v=WS.10).aspx).

## <a name="troubleshooting"></a>Résolution des problèmes
Si le contrôleur de domaine cloné (**VirtualDC2**) démarre en mode de restauration des services d’annuaire (DSRM), il ne revient pas spontanément à un état normal au cours du prochain redémarrage. Pour ouvrir une session sur un contrôleur de domaine démarré en mode DSRM, utilisez **.\Administrateur** et spécifiez le mot de passe DSRM.

Corrigez l’erreur responsable de l’échec du clonage et assurez-vous que le fichier journal dcpromo.log n’indique pas que le clonage ne peut être retenté. Si le clonage ne peut être retenté, prenez soin de vous débarrasser du média. Dans le cas contraire, vous devez supprimer l’indicateur de démarrage DSRM pour pouvoir retenter le clonage.

1.  Ouvrez Windows Server 2012 avec une commande avec élévation de privilèges (cliquez sur Windows Server 2012 de la droite et choisissez l’option Exécuter en tant qu’administrateur), puis tapez **msconfig**.

2.  Sous l’onglet **Démarrer** , sous **Options de démarrage**, désactivez la case à cocher **Démarrage sécurisé** (si elle est déjà sélectionnée avec la case à cocher **Réparer Active Directory**activée).

3.  Cliquez sur **OK** et redémarrez lorsque le système vous le demande.

Pour plus d’informations sur la résolution des problèmes liés aux contrôleurs de domaine virtualisés, voir [Résolution des problèmes des contrôleurs de domaine virtualisés](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).


