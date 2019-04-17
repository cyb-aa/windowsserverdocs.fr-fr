---
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
title: "Présentation de la virtualisation de Services (ADDS) de domaine ActiveDirectory (niveau 100)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3c7fdc2e20bc4a27f2555af54f396a15f664d6c7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100"></a>Présentation de la virtualisation de Services (ADDS) de domaine ActiveDirectory (niveau 100)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

La virtualisation des environnements de Services de domaine ActiveDirectory (ADDS) a été en cours pour un nombre d’années. À compter de Windows Server2012, les services ADDS fournissent une prise en charge pour la virtualisation des contrôleurs de domaine par l’introduction de technologies de virtualisation et l’activation de déploiement rapide des contrôleurs de domaine virtuels via le clonage. Ces nouvelles fonctionnalités de virtualisation fournissent une prise en charge pour les nuages publics et privés, les environnements hybrides où des parties des services ADDS interviennent localement et dans le cloud et les infrastructures ADDS qui résident entièrement sur site.

**Dans ce document**

-   [Virtualisation sécurisée des contrôleurs de domaine](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#safe_virt_dc)

-   [Clonage de contrôleur de domaine virtualisés](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#virtualized_dc_cloning)

-   [Étapes de déploiement d’un contrôleur de domaine virtualisé clone](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc)

-   [Résolution des problèmes](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#troubleshooting)

## <a name="safe_virt_dc"></a>Virtualisation sécurisée des contrôleurs de domaine
Les environnements virtuels posent des défis uniques pour les charges de travail distribuées qui dépendent d’un schéma de la réplication basée sur l’horloge logique. La réplication ADDS, par exemple, utilise une valeur croissante (également appelée un USN ou un numéro de séquence de mise à jour) est affectée aux transactions sur chaque contrôleur de domaine. Instance de base de données de chaque contrôleur de domaine se voit également attribuer une identité appelée «invocationID». La valeur InvocationID d’un contrôleur de domaine et sa valeur USN jouent ensemble un identificateur unique associé à chaque transaction en écriture effectuée sur chaque contrôleur de domaine et doit être unique au sein de la forêt.

La réplication ADDS utilise les valeurs InvocationID et USN sur chaque contrôleur de domaine pour déterminer les changements à répliquer vers d’autres contrôleurs de domaine. Si un contrôleur de domaine est restauré à temps en dehors de la détection du contrôleur de domaine et un USN est réutilisé pour une transaction entièrement différente, la réplication ne peut pas converger car les autres contrôleurs de domaine penseront qu’ils ont déjà reçu les mises à jour liées à l’USN réutilisé dans le contexte de cette valeur InvocationID.

Par exemple, l’illustration suivante montre la séquence des événements qui se produit dans Windows Server2008R2 et les systèmes d’exploitation antérieurs lorsqu’une restauration USN est détectée sur VDC2, le contrôleur de domaine de destination qui est en cours d’exécution sur un ordinateur virtuel. Dans cette illustration, la détection de la restauration USN se produit sur VDC2 lorsqu’un partenaire de réplication détecte que VDC2 a envoyé une valeur USN de mise à jour qui a été vu précédemment par le partenaire de réplication, ce qui indique que base de données de VDC2 a restauré à temps incorrecte.

![Présentation des services ADDS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Une machine virtuelle (VM) facilite des administrateurs d’hyperviseurs restaurer un domaine USN du contrôleur (l’horloge logique), par exemple, appliquer une capture instantanée en dehors de la détection du contrôleur de domaine. Pour plus d’informations sur les USN et USN voir de restauration, y compris une autre illustration décrivant les instances de la restauration USN non détectées [USN et restauration USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

À compter de Windows Server2012, les contrôleurs de domaine virtuels ADDS hébergés sur les plateformes d’hyperviseur qui dévoilent un identificateur appelé ID VM-Generation peuvent détecter et employer des mesures de sécurité nécessaires pour protéger l’environnement ADDS si l’ordinateur virtuel est restauré à temps par l’application d’une capture instantanée d’ordinateur virtuel. La conception VM-GenerationID utilise un hyperviseur / fournisseur indépendant pour exposer cet identificateur dans l’espace d’adressage de l’ordinateur virtuel invité, afin de l’expérience de virtualisation sécurisée reste systématiquement disponible depuis chaque hyperviseur qui prend en charge VM-GenerationID. Cet identificateur peut être testé par les services et applications en cours d’exécution à l’intérieur de l’ordinateur virtuel pour détecter si un ordinateur virtuel a été restauré à temps.

### <a name="BKMK_HowSafeguardsWork"></a>Comment fonctionnent les ces dispositifs de protection?
Lors de l’installation du contrôleur de domaine, les services ADDS stocke d’abord l’identificateur de l’ID de génération de machine virtuelle dans le cadre de l’attribut msDS-GenerationID sur l’objet d’ordinateur du contrôleur de domaine dans sa base de données (souvent appelé l’arborescence de l’annuaire ou DIT). L’ID de génération d’ordinateur virtuel est contrôlé de manière indépendante par un pilote Windows à l’intérieur de l’ordinateur virtuel.

Lorsqu’un administrateur restaure l’ordinateur virtuel à partir d’une capture instantanée précédente, la valeur actuelle de l’ID de génération d’ordinateur virtuel à partir du pilote d’ordinateur virtuel est comparée à une valeur au sein du DIT.

Si les deux valeurs sont différentes, l’attribut invocationID est réinitialisé et le pool RID ignorés ce qui empêche réutilisation des numéros USN. Si les valeurs sont identiques, la transaction est validée comme normal.

Les services ADDS compare également la valeur actuelle de l’ID de génération d’ordinateur virtuel à partir de l’ordinateur virtuel par rapport à la valeur interne du dit chaque fois que le contrôleur de domaine est redémarré et que, si elles sont différentes, qu'il réinitialise l’attribut invocationID, supprime le pool RID et met à jour le DIT avec la nouvelle valeur. Il synchronise également faisant le dossier SYSVOL afin de terminer la restauration sécurisée. Ainsi, les mécanismes de protection étendre l’application des captures instantanées sur les ordinateurs virtuels qui ont été arrêtés. Ces protections introduites dans Windows Server2012 activer les administrateurs de domaine ActiveDirectory bénéficier des avantages uniques de déploiement et la gestion des contrôleurs de domaine dans un environnement virtualisé.

L’illustration suivante montre comment les dispositifs de protection sont appliqués lorsque la même restauration USN est détectée sur un contrôleur de domaine virtualisé qui exécute Windows Server2012 sur un hyperviseur qui prend en charge VM-GenerationID.

![Présentation des services ADDS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

Dans ce cas, lorsque l’hyperviseur détecte une modification de valeur VM-GenerationID, les dispositifs de protection de virtualisation sont déclenchés, y compris la réinitialisation de l’attribut InvocationID pour le contrôleur de domaine virtualisé (de A à B dans l’exemple précédent) et mise à jour de la valeur VM-GenerationID enregistrées sur l’ordinateur virtuel pour correspondre à la nouvelle valeur (G2) stockée par l’hyperviseur. Les mécanismes de protection vous assurer que la réplication converge pour les deux contrôleurs de domaine.

Avec Windows Server2012, les services ADDS utilise des dispositifs de protection sur les contrôleurs de domaine virtuels hébergés sur des hyperviseurs prenant en charge VM-GenerationID et garantit que l’application accidentelle de captures instantanées ou d’autres ces mécanismes compatible sur un hyperviseur qui peuvent restauration état d’un ordinateur virtuel n’interrompt pas l’environnement ADDS (en empêchant les problèmes de réplication comme une bulle USN ou des objets en attente). Toutefois, la restauration d’un contrôleur de domaine en appliquant une capture instantanée d’ordinateur virtuel n’est pas recommandée en tant qu’un autre mécanisme pour la sauvegarde d’un contrôleur de domaine. Il est recommandé que vous continuez à utiliser la sauvegarde de Windows Server ou d’autres solutions de sauvegarde de base-enregistreur VSS.

> [!CAUTION]
> Si un contrôleur de domaine dans un environnement de production est rétabli accidentellement une capture instantanée, il est recommandé que vous consultiez les éditeurs des applications, et de restauration des services hébergés sur cet ordinateur virtuel, pour obtenir des conseils sur la vérification de l’état de ces programmes après la capture instantanée.

Pour plus d’informations, voir [architecture de restauration sécurisée contrôleur de domaine virtualisés](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="virtualized_dc_cloning"></a>Clonage de contrôleur de domaine virtualisés
À compter de Windows Server2012, les administrateurs peuvent facilement et en toute sécurité déployer des contrôleurs de domaine répliqués en copiant un contrôleur de domaine virtuel existant. Dans un environnement virtuel, les administrateurs ne sont plus ont de déployer une image serveur préparée à l’aide de sysprep.exe à plusieurs reprises, de promouvoir le serveur en contrôleur de domaine, puis terminez configuration supplémentaire requise pour le déploiement de chaque contrôleur de domaine réplica.

> [!NOTE]
> Les administrateurs doivent suivre un processus existants pour déployer le premier contrôleur de domaine dans un domaine, par exemple, à l’aide d’un sysprep.exe pour préparer un disque dur serveur virtuel (VHD), promouvez le serveur en contrôleur de domaine et puis effectuez toutes les conditions requises de configuration supplémentaires. Dans un scénario de récupération d’urgence, utilisez la dernière sauvegarde du serveur pour restaurer le premier contrôleur de domaine dans un domaine.

### <a name="scenarios-that-benefit-from-virtual-domain-controller-cloning"></a>Scénarios bénéficiant du clonage du contrôleur de domaine virtuel

-   Déploiement rapide des contrôleurs de domaine supplémentaire dans un nouveau domaine

-   Restaurer rapidement la continuité d’activité lors de la récupération d’urgence à la restauration de la capacité ADDS via un déploiement rapide des contrôleurs de domaine à l’aide de clonage

-   Optimiser les déploiements de cloud privé en tirant parti de configuration souples des contrôleurs de domaine pour répondre aux exigences de l’augmentation de l’échelle

-   L’approvisionnement rapide d’environnements de test favorisant le déploiement et le test de nouvelles fonctionnalités et fonctions avant le déploiement en production

-   Rapidement des besoins dans les succursales en clonant des contrôleurs de domaine existants dans les filiales

Lorsque vous déployez rapidement un grand nombre de contrôleurs de domaine, continuez à suivre vos procédures existantes pour valider l’intégrité de chaque contrôleur de domaine une fois l’installation terminée. Déployer des contrôleurs de domaine dans des lots de taille raisonnable afin que vous pouvez valider leur intégrité une fois chaque lot d’installations terminée. La taille de lot recommandée est 10. Pour plus d’informations, voir [étapes de déploiement d’un contrôleur de domaine virtualisé clone](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc).

### <a name="clear-separation-of-responsibilities"></a>Séparation claire des responsabilités
L’autorisation de cloner des contrôleurs de domaine virtualisé est sous le contrôle de l’administrateur de domaine ActiveDirectory. Dans l’ordre pour les administrateurs d’hyperviseurs puissent déployer des contrôleurs de domaine supplémentaires en copiant les contrôleurs de domaine virtuel, l’administrateur ADDS doit sélectionner et autoriser un contrôleur de domaine, puis exécutez les étapes préparatoires pour l’activer en tant que source pour le clonage.

Avec l’approvisionnement d’ordinateur virtuel généralement sous la compétence de l’administrateur de l’hyperviseur, administrateurs de l’hyperviseur peuvent configurer virtuels de contrôleur de domaine réplica en copiant les contrôleurs de domaine virtualisés autorisés et préparés pour le clonage par l’administrateur de domaine ActiveDirectory.

> [!WARNING]
> Toute personne autorisée à administrer l’hyperviseur qui héberge le contrôleur de domaine virtuel doit être hautement sécurisée et auditée dans l’environnement.

### <a name="how-does-virtual-domain-controller-cloning-work"></a>Quels sont les travaux de clonage de contrôleur de domaine virtuel?
Le processus de clonage implique la création d’une copie du disque dur virtuel du contrôleur de domaine virtuel existant (ou pour des configurations plus complexes, l’ordinateur virtuel du contrôleur de domaine), autorisant ainsi le clonage dans ADDS et la création d’un fichier de configuration de clonage. Cela réduit le nombre d’étapes et le temps nécessaires au déploiement d’un contrôleur de domaine virtuel répliqué en éliminant les tâches de déploiement répétitives.

Le contrôleur de domaine clone respecte les critères suivants pour détecter qu’il s’agit d’une copie d’un autre contrôleur de domaine:

1.  La valeur de l’ID VM-Generation fourni par l’ordinateur virtuel est différente de la valeur de l’ID de VM-Generation stocké au sein du DIT.

    > [!NOTE]
    > La plateforme d’hyperviseur doit prendre en charge les ID VM-Generation (Windows Server2012 Hyper-V prend en charge les ID VM-Generation).

2.  Présence d’un fichier appelé DCCloneConfig.xml dans un des emplacements suivants:

    -   Le répertoire où réside le DIT

    -   %windir%\ntds

    -   La racine d’un lecteur de média amovible

Une fois que les critères sont satisfaits, il passe par le processus de clonage pour s’imposer en tant que contrôleur de domaine répliqué.

Le contrôleur de domaine clone utilise le contexte de sécurité du contrôleur de domaine source (le contrôleur de domaine dont il représente la copie) pour contacter le détenteur du rôle de maître Windows Server2012 principal contrôleur de domaine (PDC) émulateur opérations (également appelé opérations à maître uniques flottant ou FSMO). L’émulateur de contrôleur de domaine principal doit exécuter Windows Server2012, mais il ne doit pas être en cours d’exécution sur un hyperviseur.

> [!NOTE]
> Si vous disposez d’une extension de schéma avec des attributs qui font référence au contrôleur de domaine source et l’attribut sur un des objets copiés (objet ordinateur, objet Paramètres NTDS) pour créer le clone, cet attribut ne sera pas copié ou mis à jour pour référencer le contrôleur de domaine clone.

Après avoir vérifié que le contrôleur de domaine demandeur est autorisé pour le clonage, l’émulateur PDC crée une nouvelle identité d’ordinateur, y compris le nouveau compte, SID, nom et mot de passe qui identifie cet ordinateur en tant que contrôleur de domaine répliqué et envoyer ces informations au clone. Le contrôleur de domaine clone prépare ensuite les fichiers de base de données ADDS à utiliser comme un réplica, et il sera également nettoyer l’état de l’ordinateur.

Pour plus d’informations, voir [contrôleur de domaine virtuels clonage architecture](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch).

### <a name="cloning-components"></a>Composants de clonage
Les composants de clonage incluent les nouvelles applets de commande dans le module ActiveDirectory pour Windows PowerShell et les fichiers XML associés:

-   **New-ADDCCloneConfigFile** «cette applet de commande crée et place DCCloneConfig.xml au bon emplacement pour vous assurer qu’il est disponible pour déclencher le clonage. Il effectue également des vérifications de la configuration requise pour garantir le succès de clonage. Il est inclus dans le module ActiveDirectory pour Windows PowerShell. Vous pouvez l’exécuter localement sur un contrôleur de domaine virtualisé qui est préparé pour le clonage, ou vous pouvez exécuter à distance à l’aide de l’option - offline. Vous pouvez spécifier des paramètres pour le contrôleur de domaine clone, telles que son nom, le site et l’adresse IP.

    Les vérifications qu’il exécute sont:

    > [!NOTE]
    > La configuration requise vérifie ne sont pas effectuées lorsque le «option hors connexion est utilisée. Pour plus d’informations, voir [en cours d’exécution New-ADDCCloneConfigFile en mode hors connexion](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode).

    -   Le contrôleur de domaine en cours de préparation est autorisé pour le clonage (membre du **contrôleurs de domaine clonables** groupe)

    -   L’émulateur de contrôleur de domaine principal exécute Windows Server2012.

    -   Les programmes ou services répertoriés avec l’exécution **Get-ADDCCloningExcludedApplicationList** sont inclus dans CustomDCCloneAllowList.xml (expliqué plus en détail à la fin de la liste des composants de clonage).

-   **DCCloneConfig.xml**» pour cloner un contrôleur de domaine virtualisé, ce fichier doit être présent dans le répertoire où réside le DIT, *%windir%\NTDS*, ou à la racine d’un lecteur amovible. Outre utilisé comme l’un déclencheurs de détecter et de lancer le clonage, il fournit également un moyen de spécifier les paramètres de configuration pour le contrôleur de domaine clone.

    Le schéma et un exemple de fichier pour le fichier DCCloneConfig.xml sont stockés sur tous les ordinateurs Windows Server2012:

    -   %windir%\system32\DCCloneConfigSchema.xsd

    -   %windir%\system32\SampleDCCloneConfig.Xml

    Il est recommandé d’utiliser l’applet de commande New-ADDCCloneConfigFile pour créer le fichier DCCloneConfig.xml. Vous pouvez également utiliser le fichier de schéma avec un éditeur compatible XML pour créer ce fichier, modifier manuellement le fichier augmente la probabilité d’erreurs. Si vous modifiez le fichier, il doit être effectuée à l’aide d’éditeurs compatibles XML, tels que Visual Studio, [XML Notepad](https://www.microsoft.com/download/details.aspx?displaylang=en&id=7973), ou des applications tierces (n’utilisez pas le bloc-notes).

-   **Get-ADDCCloningExcludedApplicationList** «cette applet de commande est exécutée sur le contrôleur de domaine source avant de commencer le processus de clonage pour déterminer les services ou les programmes installés ne sont pas dans la liste par défaut de la prise en charge, DefaultDCCloneAllowList.xml, ou une inclusion définie par l’utilisateur nommé fichier CustomDCCloneAllowList.xml de la liste et ainsi n'ont pas été évalués pour le clonage impact.

    Cette applet de commande recherche dans le contrôleur de domaine source pour les services dans le Gestionnaire de contrôle des Services et programmes installés répertorié sous **HKLM\Software\Microsoft\Windows\CurrentVersion\Uninstall** qui ne sont pas spécifiés dans le (DefaultDCCloneAllowList.xml) liste par défaut ou, s’il est fourni, la liste d’inclusion définie par l’utilisateur (CustomDCCloneAllowList.xml file). La liste des applications et services qui est retournée en exécutant l’applet de commande est la différence entre ce qui ont déjà été fourni dans le DefaultDCCloneAllowList.xml ou le fichier CustomDCCloneAllowList.xml et la liste est construite à l’exécution, selon ce qui est installé sur le contrôleur de domaine source. La sortie de services et des programmes de Get-ADDCCloningExcludedApplicationList peut être ajoutée au fichier CustomDCCloneAllowList.xml si vous déterminez que les services et programmes peuvent être clonés. Pour déterminer si un service ou un programme installé peut être cloné sans, vérifiez les conditions suivantes:

    -   Est le service ou programme installé affectés par l’identité de l’ordinateur, telles que le nom, les SID, le mot de passe, etc.?

    -   Le service ou programme installé stocke-t-il tout état localement sur l’ordinateur qui peut-être avoir une incidence sur sa fonctionnalité sur le clone?

    Vous devez collaborer avec l’Éditeur du logiciel de l’application pour déterminer si le service ou programme peut être cloné.

    > [!NOTE]
    > Avant la configuration des services supplémentaires ou des programmes dans le fichier CustomDCCloneAllowList.xml, vérifiez si vous disposez de la licence nécessaire pour copier le logiciel que contient cet ordinateur virtuel.

    Si les applications ne sont pas clonables, supprimez-les du contrôleur de domaine source avant de créer le média clone. Si une application apparaît dans la sortie de l’applet de commande, mais n’est pas incluse dans le fichier CustomDCCloneAllowList.xml, le clonage échouera. Pour le clonage aboutisse, la sortie de l’applet de commande ne doit pas répertorier tous les services ou programmes. En d’autres termes, une application doit être incluse dans le fichier CustomDCCloneAllowList.xml ou supprimée à partir du contrôleur de domaine source.

    Le tableau suivant explique les options d’exécution Get-ADDCCloningExcludedApplicationList.

    |||
    |-|-|
    |Argument|Explication|
    |*<no argument specified>*|Affiche une liste des services ou programmes de la console qui n’ont pas été pris en compte pour le clonage. S’il existe déjà un CustomDCCloneAllowList.XML dans un des emplacements autorisés, il utilise ce fichier affiche les autres services et programmes (qui peut être rien si les listes correspondent).|
    |-GenerateXml|Crée le fichier CustomDCCloneAllowList.XML renseigné avec les services et les programmes répertoriés dans la console.|
    |-Force|Remplace un fichier CustomDCCloneAllowList.XML existant.|
    |-Chemin d’accès|Chemin d’accès du dossier à créer le CustomDCCloneAllowList.XML.|

-   **DefaultDCCloneAllowList.xml** «ce fichier est présent par défaut sur chaque Windows Server2012 domaine contrôleur dans le *%windir%\system32*. Il répertorie les services et programmes qui peuvent être clonés par défaut installés. Vous ne devez pas modifier l’emplacement ou le contenu de ce fichier ou le clonage échouera.

-   **CustomDCCloneAllowList.xml** «si vous avez des services ou programmes installés qui se trouvent sur votre contrôleur de domaine source qui sont en dehors de celles répertoriées dans le fichier DefaultDCCloneAllowList.xml, les services et programmes doivent être inclus dans ce fichier. Pour rechercher des services ou programmes installés qui ne figurent pas dans la dans le fichier DefaultDCCloneAllowList.xml, exécutez le **Get-ADDCCloningExcludedApplicationList** applet de commande. Vous devez utiliser le **«GenerateXml** argument pour générer le fichier XML.

    Le processus de clonage vérifie les emplacements suivants dans l’ordre pour ce fichier et utilise le premier fichier XML, quel que soit le contenu de l’autre dossier:

    1.  La clé de Registre suivante:

        ```
        HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters
        AllowListFolder (REG_SZ)
        ```

    2.  Répertoire de travail de DSA

    3.  %systemroot%\NTDS

    4.  Média amovible en lecture/écriture, dans l’ordre de la lettre de lecteur, à la racine du lecteur

### <a name="deployment-scenarios"></a>Scénarios de déploiement
Les scénarios de déploiement suivants sont pris en charge pour le clonage de contrôleur de domaine virtuel:

-   Déployer un contrôleur de domaine clone en effectuant une copie du fichier de disque dur virtuel (vhd) d’un contrôleur de domaine source.

-   Déployer un contrôleur de domaine clone en copiant l’ordinateur virtuel d’un contrôleur de domaine source à l’aide de la sémantique de l’exportation/importation exposée par l’hyperviseur.

> [!NOTE]
> Les étapes décrites dans la section [étapes de déploiement d’un contrôleur de domaine virtualisé clone](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc) montrent comment copier un ordinateur virtuel à l’aide de la fonctionnalité d’exportation/importation de Windows Server2012 Hyper-V.

## <a name="steps_deploy_vdc"></a>Étapes de déploiement d’un contrôleur de domaine virtualisé clone

-   [Conditions préalables](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#prerequisites)

-   [Étape1: Accorder le contrôleur de domaine virtualisé source l’autorisation d’être cloné](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk4_grant_source)

-   [Étape2: Exécuter Get-ADDCCloningExcludedApplicationList applet de commande](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet)

-   [Étape3: Exécution New-ADDCCloneConfigFile](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk5_create_insert_dccloneconfig)

-   [Étape4: Exporter et importer ensuite l’ordinateur virtuel du contrôleur de domaine source](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk7_export_import_vm_sourcedc)

### <a name="prerequisites"></a>Conditions préalables

-   Pour effectuer les étapes décrites dans les procédures suivantes, vous devez être membre du groupe Admins du domaine ou les autorisations équivalentes lui ont attribués.

-   Les commandes Windows PowerShell utilisés dans ce guide doivent être exécutées à partir d’une invite de commandes avec élévation de privilèges. Pour ce faire, cliquez droit sur le **Windows PowerShell** icône, puis cliquez sur **exécuter en tant qu’administrateur**.

-   Un serveur Windows Server2012 avec le rôle de serveur Hyper-V installé (**HyperV1**).

-   Un second serveur Windows Server2012 avec le rôle de serveur Hyper-V installé (**HyperV2**).

    > [!NOTE]
    > -   Si vous utilisez un autre hyperviseur, vous devez contacter le fournisseur pour vérifier si l’hyperviseur prend en charge les ID de VM-Generation. Si l’hyperviseur ne prend pas en charge les ID VM-Generation et que vous avez fourni un DCCloneConfig.xml, le nouvel ordinateur virtuel démarrera en Mode restauration des Services d’annuaire (DSRM).
    > -   Pour augmenter la disponibilité du service de domaine ActiveDirectory, ce guide recommande et fournit des instructions à l’aide de deux hôtes Hyper-V différents, ce qui empêche un point de défaillance unique. Toutefois, vous n’avez pas besoin de deux hôtes Hyper-V pour effectuer de clonage de contrôleur de domaine virtuel.
    > -   Vous devez être membre du groupe Administrateurs local sur chaque serveur Hyper-V (**HyperV1** et **HyperV2**).
    > -   Pour pouvoir importer et exporter un fichier de disque dur virtuel à l’aide d’Hyper-V, les commutateurs de réseau virtuel sur les deux hôtes Hyper-V doivent avoir le même nom. Par exemple, si vous disposez d’un commutateur réseau virtuel sur **HyperV1** appelé VNet, puis il doit exister un commutateur réseau virtuel **HyperV2** appelé VNet.
    > -   Si les deux hôtes Hyper-V (**HyperV1** et **HyperV2**) dotés de processeurs différents, arrêtez l’ordinateur virtuel (**VirtualDC1**) que vous souhaitez exporter, cliquez sur l’ordinateur virtuel, cliquez sur **paramètres**, cliquez sur **processeur**et sous **compatibilité du processeur** sélectionnez **migrer vers un ordinateur physique avec une autre version de processeur** et cliquez sur **OK**.

-   Un déployé Windows Server2012 contrôleur de domaine (virtuel ou physique) qui héberge le rôle d’émulateur de contrôleur de domaine principal (**DC1**). Pour vérifier si le rôle d’émulateur de contrôleur de domaine principal est hébergé sur un contrôleur de domaine Windows Server2012, exécutez la commande Windows PowerShell suivante:

    ```
    Get-ADComputer (Get-ADDomainController "Discover "Service "PrimaryDC").name "Property operatingsystemversion | fl
    ```

    La valeur OperatingSystemVersion renvoyée en tant que version6.2. Si nécessaire, vous pouvez transférer le rôle d’émulateur de contrôleur de domaine principal à un contrôleur de domaine qui exécute Windows Server2012. Pour plus d’informations, voir [l’utilisation de Ntdsutil.exe pour transférer ou prendre des rôles FSMO vers un contrôleur de domaine](https://support.microsoft.com/kb/255504).

-   Un contrôleur de domaine virtualisé invité Windows Server2012 déployé (**VirtualDC1**) qui se trouve dans le même domaine que le contrôleur de domaine Windows Server2012 qui héberge le rôle d’émulateur de contrôleur de domaine principal (**DC1**). Ce sera le contrôleur de domaine source utilisé pour le clonage. Le contrôleur de domaine virtuel invité sera hébergé sur un serveur Windows Server2012 Hyper-V (**HyperV1**).

    > [!NOTE]
    > -   Pour le clonage réussisse, le contrôleur de domaine source qui est utilisé pour créer le clone ne peut pas être à partir d’un contrôleur de domaine qui a été rétrogradé puisque le support VHD source a été créé.
    > -   Arrêtez le contrôleur de domaine source avant de copier la machine virtuelle ou son disque dur virtuel.
    > -   Vous ne devez pas cloner un disque dur virtuel ou restaurer une capture instantanée est plus ancienne que la valeur de durée de vie de temporisation (ou la valeur de durée de vie d’objet supprimé si la Corbeille ActiveDirectory est activée). Si vous copiez un disque dur virtuel d’un contrôleur de domaine existant, veillez à ce que le fichier de disque dur virtuel n’est pas plus anciens que la durée de vie de temporisation valeur (par défaut, 60jours). Vous ne devez pas copier un disque dur virtuel d’un contrôleur de domaine en cours d’exécution pour créer un clone.

    Éjecter n’importe quel lecteur de disquette virtuel (VFD) la source de contrôleur de domaine peut avoir. Cela peut entraîner un problème de partage lorsque vous essayez d’importer le nouvel ordinateur virtuel.

    Uniquement les contrôleurs de domaine Windows Server2012hébergés sur un hyperviseur VM-GenerationID peuvent être utilisés en tant que source pour le clonage. Le contrôleur de domaine source Windows Server2012 utilisé pour le clonage doit être dans un état sain. Pour déterminer l’état du contrôleur de domaine source exécuter [dcdiag](https://technet.microsoft.com/library/cc731968(WS.10).aspx). Pour obtenir une meilleure compréhension du résultat retourné par l’outil dcdiag, voir [ce que fait DCDIAG réellement... faire?](http://blogs.technet.com/b/askds/archive/2011/03/22/what-does-dcdiag-actually-do.aspx).

    Si le contrôleur de domaine source est un serveur DNS, le contrôleur de domaine cloné sera également un serveur DNS. Vous devez choisir un serveur DNS que les zones intégrées à ActiveDirectory uniquement hôtes.

    Paramètres de client DNS ne sont pas clonés mais sont spécifiés à la place dans le fichier DCCloneConfig.xml. S’ils ne sont pas spécifiés, le contrôleur de domaine cloné pointera sur lui-même en tant que serveur DNS préféré par défaut. Le contrôleur de domaine cloné n’aura pas une délégation DNS. L’administrateur de la zone DNS parente doit mettre à jour la délégation DNS pour le contrôleur de domaine cloné en fonction des besoins.

    > [!WARNING]
    > Les dispositifs de protection n’étendent pas à ActiveDirectory Lightweight Directory Services (ADLDS). Par conséquent vous ne devez pas essayer de cloner un contrôleur de domaine ADDS qui héberge une instance ADLDS en ajoutant cette instance ADLDS à le CustomDCCloneAllowList.xml. ADLDS n’étant pas ID VM-Generation prenant en charge, le clonage d’un contrôleur de domaine avec ADLDS peut provoquer divergence induite par la restauration d’USN sur ce ADLDS jeu de configuration.

    Les rôles de serveur suivants ne sont pas pris en charge pour le clonage:

    -   Dynamic Host Configuration Protocol (DHCP)

    -   Services de certificats ActiveDirectory (ADCS)

    -   ActiveDirectory Lightweight Directory Services (ADLDS)

### <a name="bkmk4_grant_source"></a>Étape1: Accorder le contrôleur de domaine virtualisé source l’autorisation d’être cloné
Dans cette procédure, vous accordez le contrôleur de domaine source l’autorisation d’être cloné à l’aide de **centre d’administration ActiveDirectory** pour ajouter le contrôleur de domaine source pour le **contrôleurs de domaine clonables** groupe.

##### <a name="to-grant-the-source-virtualized-domain-controller-the-permission-to-be-cloned"></a>Pour accorder le contrôleur de domaine virtualisé source l’autorisation d’être cloné

1.  Sur n’importe quel contrôleur de domaine dans le même domaine que le contrôleur de domaine en cours de préparation pour le clonage (**VirtualDC1**), ouvrez **centre d’administration ActiveDirectory** (ADAC), recherchez l’objet contrôleur de domaine virtualisés (contrôleurs de domaine sont généralement situés sous le **contrôleurs de domaine** conteneur dans ADAC), avec le bouton droit dessus, choisissez **ajouter au groupe** sous **permet d’entrer le nom d’objet à sélectionner** type **contrôleurs de domaine clonables** puis cliquez sur **OK**.

    La mise à jour de l’appartenance au groupe effectuée à cette étape doit répliquer l’émulateur PDC avant le clonage peut être effectué. Si le **contrôleurs de domaine clonables** groupe n’est trouvé, le rôle d’émulateur PDC ne soit pas hébergé sur un contrôleur de domaine qui exécute Windows Server2012.

    > [!NOTE]
    > Pour ouvrir ADAC sur un contrôleur de domaine Windows Server2012, ouvrez Windows PowerShell, tapez **dsac.exe**.

![Présentation des services ADDS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***

L’applet de commande Windows PowerShell suivante effectue la même fonction que la procédure précédente:


    Add-ADGroupMember "Identity "CN=Cloneable Domain Controllers,CN=Users, DC=Fabrikam,DC=Com" "Member "CN=VirtualDC1,OU=Domain Controllers,DC=Fabrikam,DC=com"


### <a name="bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet"></a>Étape2: Exécuter Get-ADDCCloningExcludedApplicationList applet de commande
Dans cette procédure, vous devez exécuter le `Get-ADDCCloningExcludedApplicationList`applet de commande sur le contrôleur de domaine virtualisé source pour identifier les programmes ou services qui ne sont pas évalués pour le clonage. Vous devez exécuter l’applet de commande Get-ADDCCloningExcludedApplicationList avant de l’applet de commande New-ADDCCloneConfigFile, car si l’applet de commande New-ADDCCloneConfigFile détecte une application exclue, il ne crée pas un fichier DCCloneConfig.xml.

##### <a name="to-identify-applications-or-services-that-run-on-a-source-domain-controller-which-have-not-been-evaluated-for-cloning"></a>Pour identifier les applications ou services qui s’exécutent sur un contrôleur de domaine source qui n’ont pas été évalués pour le clonage

1.  Sur le contrôleur de domaine source (**VirtualDC1**), cliquez sur **le Gestionnaire de serveur**, cliquez sur **outils**, cliquez sur **Module ActiveDirectory pour Windows PowerShell** puis tapez la commande suivante:


    Get-ADDCCloningExcludedApplicationList


2.  Vérifiez la liste des services renvoyés et des programmes installés avec le fournisseur du logiciel pour déterminer si elles peuvent être clonés. Si les applications ou services dans la liste ne peut pas être clonés sans risque, vous devez les supprimer à partir du contrôleur de domaine source ou le clonage échouera.

3.  Pour l’ensemble des services et des programmes installés qui ont été déterminés soient clonés, exécutez la commande à l’aide de la **«GenerateXML** commutateur pour intégrer ces services et programmes dans le **CustomDCCloneAllowList.xml** fichier.


    Get-ADDCCloningExcludedApplicationList - GenerateXml


### <a name="bkmk5_create_insert_dccloneconfig"></a>Étape3: Exécution New-ADDCCloneConfigFile
Exécuter New-ADDCCloneConfigFile sur le contrôleur de domaine source et précisez éventuellement les paramètres de configuration pour le contrôleur de domaine clone, telles que le nom, l’adresse IP et la résolution DNS.

Par exemple, pour créer un contrôleur de domaine clone appelé VirtualDC2 avec une adresse IPv4 statique, tapez:


    New-ADDCCloneConfigFile "Static -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.255.0" -CloneComputerName "VirtualDC2" -IPv4DefaultGateway "10.0.0.3" -SiteName "REDMOND"

> [!NOTE]
> Le contrôleur de domaine clone sera se trouver dans le même site que le contrôleur de domaine source sauf si un autre site est spécifié dans le fichier DCCloneConfig.xml. Il est recommandé que vous spécifiez un site approprié dans le fichier DCCloneConfig.xml pour le contrôleur de domaine clone en fonction de son adresse IP.

Le nom d’ordinateur est facultatif. Si vous ne spécifiez pas un, un nom unique sera généré à partir de l’algorithme suivant:

-   Le préfixe est les 8premiers caractères du nom de l’ordinateur de contrôleur de domaine source. Par exemple, un nom de l’ordinateur source d’Ordinateursource est tronqué à une chaîne de préfixe d’Orsource.

-   Un suffixe d’attribution de noms unique au format ««CL*nnnn*» est ajouté à la chaîne de préfixe où *nnnn*est la valeur suivante disponible à partir de 0001-9999 le contrôleur de domaine principal détermine n’est pas actuellement en cours d’utilisation. Par exemple, si 0047 est le numéro suivant dans la plage autorisée, à l’aide de l’exemple précédent, le préfixe de nom de l’ordinateur Orsource, le nom à utiliser pour l’ordinateur clone sera être défini comme Orsource-CL0047.

> [!NOTE]
> Un serveur de catalogue global (GC) est requis pour l’applet de commande New-ADDCCloneConfigFile fonctionner correctement. L’appartenance du contrôleur de domaine source dans le **contrôleurs de domaine clonables** doit être mentionnée dans le catalogue global. Le catalogue global ne pas besoin d’être le même contrôleur de domaine en tant que l’émulateur de contrôleur de domaine principal, mais de préférence, il doit être dans le même site. Si un catalogue global n’est pas disponible, la commande échoue avec l’erreur «le serveur n’est pas opérationnel». Pour plus d’informations, voir [Virtualized Domain Controller Troubleshooting](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).

Pour créer un contrôleur de domaine clone appelé Clone1 avec des paramètres IPv4 statiques et préciser des serveurs WINS préféré et auxiliaire, tapez:


    New-ADDCCloneConfigFile "CloneComputerName "Clone1" "Static -IPv4Address "10.0.0.5" "IPv4DNSResolver "10.0.0.1" "IPv4SubnetMask "255.255.0.0" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


> [!NOTE]
> Si vous spécifiez des serveurs WINS, vous devez spécifier les deux **«PreferredWINSServer** et **«AlternateWINSServer**. Si vous spécifiez un seul de ces arguments, le clonage échoue avec l’erreur 0 x 80041005 de code figurant dans le dcpromo.log.

Pour créer un contrôleur de domaine clone appelé Clone2 avec des paramètres IPv4 dynamiques, tapez:


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" 


> [!NOTE]
> Dans ce cas, un serveur DHCP doit être dans l’environnement que le clone peut atteindre et obtenir une adresse IP et autres paramètres réseau pertinents.

Pour créer un contrôleur de domaine clone appelé Clone2 avec des paramètres IPv4 dynamiques et spécifier les serveurs WINS préféré et auxiliaire, tapez:


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" -SiteName "REDMOND" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


Pour créer un contrôleur de domaine clone avec des paramètres IPv6 dynamiques, tapez:


    New-ADDCCloneConfigFile -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


Pour créer un contrôleur de domaine clone avec des paramètres IPv6 statiques, tapez:


    New-ADDCCloneConfigFile "Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


> [!NOTE]
> Lorsque vous spécifiez des paramètres IPv6, la seule différence entre les paramètres statiques et dynamiques est l’inclusion de **-statiques** basculer. L’inclusion de la **-statiques** commutateur rend obligatoire pour spécifier au moins un **IPv6DNSResolver**. L’adresse IPv6 statique doit être configuré via la configuration automatique des adresses sans état (SLAAC) avec des préfixes routeur attribués. Avec IPv6 dynamiques, les résolutions DNS sont facultatives, mais il est prévu que le clone peut atteindre un serveur DHCP IPv6 sur le sous-réseau pour obtenir une adresse IPv6 et les informations de configuration DNS.

#### <a name="BKMK_OfflineMode"></a>New-ADDCCloneConfigFile en cours d’exécution en mode hors connexion
Si vous disposez de plusieurs copies de support de contrôleur de domaine source qui ont été préparés pour le clonage (ce qui signifie que le contrôleur de domaine source est autorisé pour le clonage, l’applet de commande Get-ADDCCloningExcludedApplicationList a été exécuté et ainsi de suite) et que vous souhaitez spécifier des paramètres différents pour chaque copie du média, vous pouvez exécuter New-ADDCCloneConfigFile en mode hors connexion. Cela peut être plus efficace que de préparer individuellement chaque ordinateur virtuel, par exemple, en important chaque copie.

Dans ce cas, les administrateurs de domaine peuvent monter le disque hors connexion et utiliser des outils d’Administration de serveur distant (RSAT) pour exécuter l’applet de commande New-ADDCCloneConfigFile avec l’argument - offline afin d’ajouter les fichiers XML, ce qui permet d’automatiser de type usine à l’aide de nouvelles options Windows PowerShell incluses dans Windows Server2012. Pour plus d’informations sur comment monter le disque hors connexion afin d’exécuter l’applet de commande New-ADDCCloneConfigFile en mode hors connexion, voir [Adding XML to the Offline System Disk](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_Offline).

Vous devez tout d’abord exécuter l’applet de commande localement sur le média source pour vous assurer que la vérification des pass. Les vérifications ne sont pas effectuées en mode hors connexion, car l’applet de commande peut être exécutée à partir d’un ordinateur qui ne peut pas être du même domaine ou d’un ordinateur joint au domaine. Après avoir exécuté l’applet de commande localement, il crée un fichier DCCloneConfig.xml. Vous pouvez supprimer le DCCloneConfig.xml qui est créé localement si vous prévoyez d’utiliser le mode hors connexion par la suite.

Pour créer un contrôleur de domaine clone appelé CloneDC1 en mode hors connexion, dans un site nommé REDMOND» avec l’adresse IPv4 statique, tapez:


    New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS


Pour créer un contrôleur de domaine clone appelé Clone2 en mode hors connexion avec IPv4 statiques et des paramètres IPv6 statiques, tapez:


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS


Pour créer un contrôleur de domaine clone en mode hors connexion avec IPv4 statiques et des paramètres IPv6 dynamiques et spécifier plusieurs serveurs DNS pour les paramètres de résolution DNS, tapez:


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS 


Pour créer un contrôleur de domaine clone appelé Clone1 en mode hors connexion avec les paramètres IPv4 dynamiques et des paramètres IPv6 statiques, tapez:


    New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS


Pour créer un contrôleur de domaine clone en mode hors connexion avec les paramètres IPv4 dynamiques et des paramètres IPv6 dynamiques, tapez:


    New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS


### <a name="bkmk7_export_import_vm_sourcedc"></a>Étape4: Exporter et importer ensuite l’ordinateur virtuel du contrôleur de domaine source
Dans cette procédure, exportez l’ordinateur virtuel du contrôleur de domaine virtualisé source, puis importez l’ordinateur virtuel. Cette action crée un contrôleur de domaine virtualisé clone dans votre domaine.

Vous devez être membre du groupe Administrateurs local sur chaque ordinateur hôte Hyper-V. Si vous utilisez des informations d’identification différentes pour chaque serveur, exécutez les applets de commande Windows PowerShell pour exporter et importer l’ordinateur virtuel dans différentes sessions Windows PowerShell.

S’il existe des captures instantanées sur le contrôleur de domaine source, ils doivent être supprimés avant d’exporter le contrôleur de domaine source car l’ordinateur virtuel n’est pas importé si une capture instantanée possède des paramètres de processeur qui ne sont pas compatibles avec l’hôte hyper-v cible. Si les paramètres de processeur sont compatibles entre les ordinateurs hôtes hyper-v source et cible, vous pouvez exporter et copier la source sans supprimer au préalable les captures instantanées. Après l’importation, toutefois, les captures instantanées doivent être supprimées de l’ordinateur virtuel clone avant le début.

##### <a name="to-copy-a-virtual-domain-controller-by-exporting-and-then-importing-the-virtualized-source-domain-controller"></a>Pour copier un contrôleur de domaine virtuel par exportation et importation puis le contrôleur de domaine source virtualisé

1.  Sur **HyperV1**, arrêt le contrôleur de domaine source (**VirtualDC1**).

    ![Présentation des services ADDS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***

    Stop-VM-nom VirtualDC1 - ComputerName HyperV1


2.  Sur **HyperV1**, supprimez les captures instantanées, puis exporter le contrôleur de domaine source (VirtualDC1) dans le répertoire c:\CloneDCs.

> [!NOTE]
> Vous devez supprimer les captures instantanées associées, car chaque fois qu'un instantané, un nouveau fichier AVHD est créé qui agit en tant que disque de différenciation. Cela crée une réaction en chaîne. Si vous avez réalisé des captures instantanées et insérez le fichier DCCLoneConfig.xml dans le disque dur virtuel, vous risquez de créer un clone à partir d’une ancienne version du DIT ou insérer le fichier de configuration dans le mauvais fichier VHD. La suppression de la capture instantanée fusionne toutes ces AVHDs le disque dur virtuel de base.

![Présentation des services ADDS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***


    Get-VMSnapshot VirtualDC1 | Remove-VMSnapshot -IncludeAllChildSnapshots
    Export-VM -Name VirtualDC1 -ComputerName HyperV1 -Path c:\CloneDCs\VirtualDC1


3.  Copiez le dossier **virtualdc1** dans le répertoire c:\Import de **HyperV2**.

4.  Sur **HyperV2**, à l’aide **Gestionnaire Hyper-V**, importez l’ordinateur virtuel (à l’aide de la **importation d’ordinateur virtuel** Assistant **Gestionnaire Hyper-V**) à partir du dossier **c:\Import\virtualdc1** et supprimez toutes les associés **captures instantanées**.

Utilisez le **copier l’ordinateur virtuel (créer un ID unique)** option lors de l’importation de l’ordinateur virtuel.

![Présentation des services ADDS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***

    $path = Get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual Machines"
    $vm = Import-VM -Path $path.fullname -Copy -GenerateNewId
    Rename-VM $vm VirtualDC2


Pour créer le clone plusieurs contrôleurs de domaine à partir du même contrôleur de domaine source:

  -   L’interface utilisateur: le **importer l’ordinateur virtuel** Assistant, spécifiez des nouveaux emplacements pour **dossier de configuration de machine virtuelle**, **magasin de captures instantanées**, **dossier de pagination intelligente**et une autre **emplacement** pour les disques durs virtuels de l’ordinateur virtuel.

  -   Windows PowerShell: spécifiez des nouveaux emplacements pour l’ordinateur virtuel en utilisant les paramètres suivants pour le `Import-VM`applet de commande:

        $path = Get-ChildItem «ordinateurs C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual» Import-VM-chemin d’accès $path.fullname-copie - GenerateNewId - VhdDestinationPath - ComputerName HyperV2 «path» - SnapshotFilePath «path» - SmartPagingFilePath «path» - VirtualMachinePath «path»


> [!NOTE]
> La taille de lot recommandée pour la création de contrôleurs de domaine clone plusieurs simultanément est 10. Le nombre maximal est limité par le nombre maximal de connexions de réplications sortantes, soit par défaut 16 pour la réplication distribué de système de fichiers (DFSR) et 10 pour le Service de réplication de fichiers (FRS). Vous ne devez pas déployer plus que le nombre recommandé de contrôleurs de domaine clone simultanément, sauf si vous avez soigneusement testé ce nombre pour votre environnement.

5.  Sur **HyperV1**, redémarrez le contrôleur de domaine source (**(VirtualDC1**) pour la mettre en ligne.

![Présentation des services ADDS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***

    Start-VM -Name VirtualDC1 -ComputerName HyperV1


6.  Sur **HyperV2**, démarrez l’ordinateur virtuel (**VirtualDC2**) pour la mettre en ligne en tant qu’un contrôleur de domaine clone dans le domaine.

![Présentation des services ADDS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***


    Start-VM -Name VirtualDC2 -ComputerName HyperV2

> [!NOTE]
> L’émulateur de contrôleur de domaine principal doit être en cours d’exécution pour le clonage aboutisse. Si elle a été arrêté, assurez-vous qu’il n’a démarré et la synchronisation initiale effectuée afin qu’il est prenant en charge conserve le rôle d’émulateur de contrôleur de domaine principal. Pour plus d’informations, voir Microsoft [l’article 305476](https://support.microsoft.com/kb/305476).

Une fois le clonage terminé, vérifiez le nom de l’ordinateur clone pour vous assurer de l’opération de clonage a réussi. Vérifiez que l’ordinateur virtuel n’a pas démarré en Mode restauration des Services d’annuaire (DSRM). Si vous essayez d’ouvrir une session et recevez une erreur indiquant qu'aucun serveur d’ouverture de session est disponible, essayez de vous connecter en mode DSRM. Si le contrôleur de domaine ne pas été correctement cloné et il est démarré en mode DSRM, vérifiez les journaux dans l’Observateur d’événements et les journaux dcpromo dans le dossier %systemroot%/debug.

Le contrôleur de domaine cloné sera un membre de la **contrôleurs de domaine clonables** puisqu’il copie l’appartenance du contrôleur de domaine source de groupe. Comme meilleure pratique, vous devez laisser la **contrôleurs de domaine clonables** groupe vide jusqu'à ce que vous êtes prêt à effectuer les opérations de clonage, vous devez supprimer des membres une fois les opérations de clonage.

Si le contrôleur de domaine source stocke un média de sauvegarde, le contrôleur de domaine cloné stockera aussi le support de sauvegarde. Vous pouvez exécuter `wbadmin get versions`pour afficher le média de sauvegarde sur le contrôleur de domaine cloné. Un membre du groupe Admins du domaine doit supprimer le support de sauvegarde sur le contrôleur de domaine cloné pour empêcher qu’elle soit accidentellement restauré. Pour plus d’informations sur la suppression d’une sauvegarde de l’état système à l’aide de wbadmin.exe, voir [Wbadmin delete systemstatebackup](https://technet.microsoft.com/library/cc742081(v=WS.10).aspx).

## <a name="troubleshooting"></a>Résolution des problèmes
Si le contrôleur de domaine clone (**VirtualDC2**) démarre en Mode restauration des Services d’annuaire (DSRM), elle ne retourne un en mode normal sur son propre au prochain redémarrage. Pour vous connecter à un contrôleur de domaine est démarré en mode DSRM, utilisez **. \Administrator** et spécifiez le mot de passe DSRM.

Corrigez la cause d’un échec du clonage et vérifiez que le dcpromo.log n’indique pas que le clonage ne peut pas être retenté. Si le clonage ne peut pas être retenté, en toute sécurité ignorer le média. Si le clonage contraire, vous devez supprimer l’indicateur de démarrage du Mode de restauration des services d’annuaire afin de retenter le clonage.

1.  Ouvrez Windows Server2012 avec une commande avec élévation de privilèges (cliquez sur Windows Server2012 de la droite et choisissez l’option Exécuter en tant qu’administrateur), puis tapez **msconfig**.

2.  Sur le **démarrage** sous **Options de démarrage**, désactivez **démarrage sans échec** (elle est déjà sélectionnée avec l’option **réparer ActiveDirectory activé**).

3.  Cliquez sur **OK** et redémarrez lorsque vous y êtes invité.

Pour plus d’informations sur la résolution des problèmes sur les contrôleurs de domaine virtualisés, voir [Virtualized Domain Controller Troubleshooting](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).


