---
title: "Présentation technique de stratégies de Restriction logicielle"
description: "Sécurité de Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7013b0-0efd-40fd-bd6d-75128adbd0b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: d007d55ced9c6a18581eaedb4edb66db9eeccab9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="software-restriction-policies-technical-overview"></a>Présentation technique de stratégies de Restriction logicielle

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique décrit les stratégies de restriction logicielle, quand et comment utiliser la fonctionnalité, les modifications ont été implémentés dans les versions antérieures et fournit des liens vers des ressources supplémentaires pour vous aider à créer et déployer des stratégies de restriction logicielle à partir de Windows Server2008 et WindowsVista.

## <a name="introduction"></a>Introduction
Stratégies de restriction logicielle fournissent aux administrateurs un mécanisme basé sur la stratégie de groupe pour identifier les logiciels et contrôler son exécution sur l’ordinateur local. Ces stratégies peuvent être utilisées pour protéger les ordinateurs exécutant les systèmes d’exploitation MicrosoftWindows (à compter de Windows Server2003 et WindowsXPProfessionnel) contre les conflits connus et de protéger les ordinateurs contre les menaces de sécurité tels que les virus et programmes cheval de Troie. Vous pouvez également utiliser des stratégies de restriction logicielle pour créer une configuration fortement restreinte pour vos ordinateurs, dans laquelle vous uniquement spécifiquement les applications identifiées est autorisée à s’exécuter. Stratégies de restriction logicielle sont intégrées avec MicrosoftActiveDirectory et la stratégie de groupe. Vous pouvez également créer des stratégies de restriction logicielle sur des ordinateurs autonomes.

Stratégies de restriction logicielle sont des stratégies d’approbation, qui sont définies par un administrateur pour limiter des scripts et tout autre code qui n’est pas entièrement fiable depuis l’exécution des réglementations. L’extension stratégies de Restriction logicielle pour l’éditeur de stratégie de groupe locale fournit une interface utilisateur unique par le biais duquel les paramètres de la restriction de l’utilisation d’applications peuvent être gérés sur l’ordinateur local ou dans tout le domaine.

## <a name="procedures"></a>Procédures

-   [Administrer les stratégies de Restriction logicielle](administer-software-restriction-policies.md)

    -   [Déterminer la liste d’autorisation / exclusion et l’inventaire des applications pour les stratégies de Restriction logicielle](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [Travailler avec des règles de stratégies de Restriction logicielle](work-with-software-restriction-policies-rules.md)

    -   [Utiliser des stratégies de Restriction logicielle pour protéger votre ordinateur contre un Virus de courrier électronique](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [Résoudre les problèmes de stratégies de Restriction logicielle](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>Scénarios de l’utilisation de stratégie de restriction logicielle
Les utilisateurs professionnels collaborent à l’aide par courrier électronique, messagerie instantanée et des applications d’homologue à homologue. Comme ces collaborations augmenter, en particulier avec l’utilisation d’Internet en informatique d’entreprise, cela les menaces de programmes malveillants, tels que les vers, virus et utilisateur malveillant ou menaces personne malveillante.

Les utilisateurs peuvent recevoir un code malveillant dans de nombreux formulaires, allant des fichiers exécutables Windows natives (.exe files), macros dans des documents (par exemple, les fichiers .doc), scripts (tels que des fichiers .vbs). Les utilisateurs malveillants ou des intrus souvent utilisent techniques d’ingénierie sociale pour permettre aux utilisateurs d’exécuter du code contenant des virus et vers. (Ingénierie sociale est un terme de ruse leur mot de passe ou une forme des informations de sécurité.) Si ce code est activé, elle peut générer des attaques par déni de service sur le réseau, envoyer des données sensibles ou privées à Internet, risque de compromettre la sécurité de l’ordinateur ou endommager le contenu du lecteur de disque dur.

Les utilisateurs et les organisations informatiques doivent être en mesure de déterminer quels logiciels exécuter en toute sécurité et qui n’est pas. Avec les grands nombres et les formulaires code malveillant peut prendre, cela devient une tâche difficile.

Pour protéger leurs ordinateurs réseau à partir d’un code malveillant et de logiciels inconnus ou non pris en charge, organisations peuvent implémenter des stratégies de restriction logicielle dans le cadre de leur stratégie de sécurité globale.

Les administrateurs peuvent utiliser des stratégies de restriction logicielle pour les tâches suivantes:

-   Définir ce qu’est un code de confiance

-   Conception une stratégie de groupe souple réglementant les scripts, les fichiers exécutables et les contrôles ActiveX

Stratégies de restriction logicielle sont appliquées par le système d’exploitation et par les applications (par exemple, les applications de l’écriture de scripts) conformes aux stratégies de restriction logicielle.

Plus précisément, les administrateurs peuvent utiliser des stratégies de restriction logicielle pour les raisons suivantes:

-   Spécifier quels logiciels (fichiers exécutables) peuvent s’exécuter sur les ordinateurs clients

-   Empêcher les utilisateurs d’exécuter des programmes spécifiques sur les ordinateurs partagés

-   Spécifier qui peut ajouter des éditeurs approuvés sur les ordinateurs clients

-   Définir la portée des stratégies de restriction logicielle (spécifier si elles affectent tous les utilisateurs ou un sous-ensemble d’utilisateurs sur les ordinateurs clients)

-   Empêcher les fichiers exécutables d’en cours d’exécution sur l’ordinateur local, une unité d’organisation (UO), un site ou un domaine. Cela est approprié dans le cas lorsque vous n’utilisez pas de stratégies de restriction logicielle à résoudre les problèmes potentiels avec les utilisateurs malveillants.

## <a name="BKMK_Diffs_Changes"></a>Différences et les modifications apportées aux fonctionnalités
Il n’existe aucune modification de fonctionnalités des stratégies de restriction logicielle pour Windows Server2012 et Windows8.

**Versions prises en charge**

Stratégies de Restriction logicielle peuvent être uniquement configurées sur et appliqués aux ordinateurs qui exécutent au moins Windows Server2003, y compris Windows Server2012 et au moins WindowsXP, y compris Windows8.

> [!NOTE]
> Certaines éditions du début de système d’exploitation client Windows avec WindowsVista n’ont pas de stratégies de restriction logicielle. Ordinateurs n'administrés pas dans un domaine par la stratégie de groupe ne peuvent pas recevoir des stratégies distribuées.

**Comparaison des fonctions de contrôle d’application dans les stratégies de Restriction logicielle et AppLocker**

Le tableau suivant compare les fonctionnalités et fonctions de la fonctionnalité stratégies de Restriction logicielle (SRP) et d’AppLocker.

|Fonction de contrôle d’application|STRATÉGIES DE RESTRICTION LOGICIELLE|AppLocker|
|----------------|----|-------|
|Étendue|Stratégies de restriction logicielle peuvent être appliquées à tous les systèmes d’exploitation de Windows à partir de WindowsXP et Windows Server2003.|Les stratégies AppLocker s’appliquent uniquement à Windows Server2008R2, Windows Server2012, Windows7 et Windows8.|
|Création de stratégies|Stratégies de restriction logicielles sont gérées via une stratégie de groupe et seul l’administrateur de l’objet de stratégie de groupe peut mettre à jour la stratégie de restriction logicielle. L’administrateur sur l’ordinateur local peut modifier les stratégies de restriction logicielles définies dans la stratégie de groupe locale.|Les stratégies AppLocker sont gérées via une stratégie de groupe et seul l’administrateur de l’objet de stratégie de groupe peut mettre à jour la stratégie. L’administrateur sur l’ordinateur local peut modifier les stratégies AppLocker définis dans la stratégie de groupe locale.<br /><br />AppLocker permet la personnalisation des messages d’erreur pour diriger les utilisateurs vers une page Web de l’aide.|
|Maintenance des stratégies|Stratégies de restriction logicielles doivent être mis à jour à l’aide du composant logiciel enfichable Stratégie de sécurité locale (si les stratégies sont créées localement) ou de la Console de gestion des stratégies de groupe (GPMC).|Les stratégies AppLocker peuvent être mis à jour à l’aide de la stratégie de sécurité locale enfichable (si les stratégies sont créées localement), ou la console GPMC ou les applets de commande WindowsPowerShellAppLocker.|
|Application de la stratégie|Stratégies de restriction logicielles sont distribuées via la stratégie de groupe.|Les stratégies AppLocker sont distribuées via la stratégie de groupe.|
|Mode d’application|Stratégies de restriction logicielle fonctionne dans le "mode liste de refus" où les administrateurs peuvent créer des règles pour les fichiers qu’ils ne souhaitent pas autoriser de cette entreprise, tandis que le reste du fichier sont autorisés à s’exécuter par défaut.<br /><br />Stratégies de restriction logicielle peuvent également être configurés dans le "mode liste Autoriser" tels que les par défaut, tous les fichiers sont bloqués et les administrateurs doivent créer autoriser des règles pour les fichiers qu’ils souhaitent autoriser.|AppLocker en fonctionnement par défaut dans le "autoriser le mode liste" où seuls les fichiers sont autorisés à exécuter pour lesquels il est correspondant à une règle d’autorisation.|
|Types de fichiers qui peuvent être contrôlés|Stratégies de restriction logicielle peuvent contrôler les types de fichier suivants:<br /><br />-Fichiers exécutables<br />-DLL<br />-Scripts<br />-Programmes d’installation de Windows<br /><br />Stratégies de restriction logicielle ne peut pas contrôler chaque type de fichier séparément. Toutes les règles de stratégies de restriction logicielle sont dans un regroupement de règles unique.|AppLocker peut contrôler les types de fichiers suivants:<br /><br />-Fichiers exécutables<br />-DLL<br />-Scripts<br />-Programmes d’installation de Windows<br />-Applications empaquetées et programmes d’installation (Windows Server2012 et Windows8)<br /><br />AppLocker maintient un regroupement de règles distinct pour chacun des types de cinq fichier.|
|Types de fichiers désignés|Stratégies de restriction logicielle prend en charge une liste des types de fichiers qui sont considérés comme exécutable extensible. Les administrateurs peuvent ajouter des extensions de fichiers doivent être considérée comme exécutables.|AppLocker ne prend pas en charge. AppLocker prend actuellement en charge les extensions de fichier suivantes:<br /><br />-Fichiers exécutables (.exe, .com)<br />-DLL (.ocx, .dll)<br />-Scripts (.vbs, .js, .ps1, .cmd, .bat)<br />-Programmes d’installation de Windows (.msi, .mst, .msp)<br />-Application empaquetée programmes d’installation (.appx)|
|Types de règles|Stratégies de restriction logicielle prend en charge les quatre types de règles:<br /><br />-Hash<br />-Chemin d’accès<br />-Signature<br />: Zone Internet|AppLocker prend en charge trois types de règles:<br /><br />-Hash<br />-Chemin d’accès<br />-Publisher|
|Modification de la valeur de hachage|Stratégies de restriction logicielle permet aux administrateurs de fournir des valeurs de hachage personnalisé.|AppLocker calcule la valeur de hachage proprement dite. En interne, elle utilise le hachage SHA1 Authenticode pour les fichiers exécutables portables (Exe et Dll) et les programmes d’installation de Windows et un hachage de fichier plat SHA1 pour le reste.|
|Prise en charge de différents niveaux de sécurité|Avec les stratégies de restriction logicielle, les administrateurs peuvent spécifier les autorisations qui permet d’exécuter une application. Par conséquent, un administrateur peut configurer une règle que le bloc-notes s’exécute toujours avec des autorisations restreintes et jamais avec des privilèges d’administration.<br /><br />Stratégies de restriction logicielle sur WindowsVista et versions antérieures pris en charge plusieurs niveaux de sécurité. Sur Windows7, cette liste était limitée à deux niveaux: non autorisés et illimité (utilisateur de base se traduit par non autorisé).|AppLocker ne prend pas en charge les niveaux de sécurité.|
|Gérer les applications empaquetées et programmes d’installation des applications empaquetées|Impossible de|.AppX est un type de fichier valide capable de gérer AppLocker.|
|Ciblage d’une règle à un utilisateur ou un groupe d’utilisateurs|Règles de stratégies de restriction logicielle s’appliquent à tous les utilisateurs sur un ordinateur particulier.|Les règles AppLocker peuvent être destinées à un utilisateur spécifique ou un groupe d’utilisateurs.|
|Prise en charge des exceptions de règle|Stratégies de restriction logicielle ne prend pas en charge les exceptions de règle|Les règles AppLocker peuvent avoir des exceptions qui permettent aux administrateurs de créer des règles telles que "Autoriser tout à partir de Windows à l’exception de Regedit.exe".|
|Prise en charge pour le mode d’audit|Stratégies de restriction logicielle ne gère pas le mode audit. La seule façon de tester les stratégies de restriction logicielles consiste à configurer un environnement de test et exécuter des expériences quelques.|AppLocker prend en charge le mode d’audit qui permet aux administrateurs de tester l’effet de leur stratégie dans l’environnement de production réel sans affecter l’expérience utilisateur. Une fois que vous êtes satisfait des résultats, vous pouvez commencer à appliquer la stratégie.|
|Prise en charge pour l’exportation et importation de stratégies|Stratégies de restriction logicielle ne gère pas la stratégie d’importation/exportation.|AppLocker prend en charge l’importation et exportation de stratégies. Cela vous permet de créer la stratégie AppLocker sur un ordinateur de test, tester et exportez cette stratégie et réimporter dans l’objet de stratégie de groupe souhaité.|
|Application des règles|En interne, la mise en application des règles de stratégies de restriction logicielle se produit en mode d’utilisateur qui est moins sécurisé.|En interne, les règles AppLocker pour les fichiers exe et DLL sont appliquées dans le mode noyau qui est plus sûr que l’application de ces derniers dans le mode utilisateur.|

## <a name="system-requirements"></a>Configuration système requise
Stratégies de restriction logicielle peuvent être uniquement configurées sur et appliqués aux ordinateurs qui exécutent au moins Windows Server2003 et au moins WindowsXP. Stratégie de groupe est requise pour distribuer les objets de stratégie de groupe qui contiennent des stratégies de restriction logicielle.

## <a name="software-restriction-policies-components-and-architecture"></a>Architecture et les composants de stratégies de restriction logicielle
Stratégies de restriction logicielle fournissent un mécanisme pour le système d’exploitation et les applications compatibles avec les stratégies de restriction logicielle pour limiter l’exécution de logiciels.

À un niveau élevé, les stratégies de restriction logicielle sont constituées des composants suivants:

-   Stratégies de restriction logicielle API. Les Interfaces de programmation d’Application (API) sont utilisés pour créer et configurer les règles qui constituent la stratégie de restriction logicielle. Il existe également des API pour l’interrogation, traitement des stratégies de restriction logicielle et les stratégies de restriction logicielle lors de l’application.

-   Un outil de gestion des stratégies de restriction logicielle. Il s’agit de la **stratégies de Restriction logicielle** extension du **Éditeur d’objets de stratégie de groupe Local** enfichable, que les administrateurs utilisent pour créer et modifier des stratégies de restriction logicielle.

-   Un ensemble d’API du système d’exploitation et les applications qui appellent des stratégies de restriction logicielle API mise en application des stratégies de restriction logicielle lors de l’exécution.

-   Stratégie de groupe et ActiveDirectory. Stratégies de restriction logicielle dépendent de l’infrastructure de stratégie de groupe pour propager les stratégies de restriction logicielle à partir d’ActiveDirectory pour les clients appropriés et pour l’étendue et de filtrage de l’application de ces stratégies sur les ordinateurs cibles appropriés.

-   Authenticode et WinVerify confiance API qui sont utilisées pour traiter les fichiers exécutables signés.

-   Observateur d’événements. Les fonctions utilisées par les événements de journal de stratégies de restriction logiciel dans les journaux de l’Observateur d’événements.

-   Résultant jeu de stratégies résultant (RSoP), qui peuvent aider dans le diagnostic de la stratégie actuelle qui est appliquée à un client.

Pour plus d’informations sur l’architecture des stratégies de restriction logicielle, voir comment les stratégies de restriction logicielle gère des règles, processus et interactions, [Software Restriction stratégies fonctionnement](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx) dans la bibliothèque technique Windows Server2003.

## <a name="BKMK_Best_Practices"></a>Meilleures pratiques

### <a name="do-not-modify-the-default-domain-policy"></a>Ne modifiez pas la stratégie de domaine par défaut.

-   Si vous ne modifiez pas la stratégie de domaine par défaut, vous avez toujours la possibilité de réappliquer la stratégie de domaine par défaut en cas de problème avec votre stratégie de domaine personnalisé.

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>Créer un objet de stratégie de groupe distinct pour les stratégies de restriction logicielle.

-   Si vous créez un objet de stratégie groupe distinct (GPO) pour les stratégies de restriction logicielle, vous pouvez désactiver les stratégies de restriction logicielle en cas d’urgence sans désactiver le reste de votre stratégie de domaine.

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>Si vous rencontrez des problèmes avec les paramètres de stratégie appliqués, redémarrez Windows en Mode sans échec.

-   Stratégies de restriction logicielle ne s’appliquent pas lorsque Windows est démarré en Mode sans échec. Si vous verrouillez accidentellement une station de travail avec les stratégies de restriction logicielle, redémarrez l’ordinateur en Mode sans échec, connectez-vous en tant qu’administrateur local, modifiez la stratégie, exécuter **gpupdate**, redémarrez l’ordinateur, puis ouvrez une session normalement.

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>Soyez prudent lors de la définition d’un paramètre par défaut est rejeté.

-   Lorsque vous définissez un paramètre par défaut **non autorisé**, tous les logiciels ne sont pas autorisé à l’exception des logiciels qui ont été explicitement autorisés. N’importe quel fichier que vous souhaitez ouvrir doit avoir une restriction logicielle des stratégies de règle qui lui permet d’ouvrir.

-   Pour protéger les administrateurs de verrouiller eux-mêmes hors du système, lorsque le niveau de sécurité par défaut est défini sur **non autorisé**, quatre règles de chemin d’accès du Registre sont créées automatiquement. Vous pouvez supprimer ou modifier ces règles de chemin d’accès du Registre; toutefois, cela n’est pas recommandée.

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>Pour une sécurité optimale, utilisez les listes de contrôle d’accès conjointement avec les stratégies de restriction logicielle.

-   Les utilisateurs peuvent essayer de contourner les stratégies de restriction logicielle en renommant ou en déplaçant des fichiers rejetés ou en remplaçant des fichiers illimitées. Par conséquent, il est recommandé que vous utilisez des listes de contrôle d’accès (ACL) pour refuser les utilisateurs l’accès nécessaire pour effectuer ces tâches.

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>Tester soigneusement les nouveaux paramètres de stratégie dans les environnements de test avant d’appliquer les paramètres de stratégie à votre domaine.

-   Nouveaux paramètres de stratégie peuvent agir différemment que prévu à l’origine. Test permet de réduire le risque de rencontrer un problème lorsque vous déployez des paramètres de stratégie de votre réseau.

-   Vous pouvez configurer un domaine de test distinct du domaine dans lequel vous souhaitez tester les nouveaux paramètres de stratégie de votre organisation. Vous pouvez également tester les paramètres de stratégie en créant un test objet stratégie de groupe et en le liant à une unité d’organisation de test. Lorsque vous avez soigneusement testé les paramètres de stratégie avec les utilisateurs de test, vous pouvez lier le test objet stratégie de groupe à votre domaine.

-   Ne définissez pas de programmes ou fichiers **non autorisé** sans les tester pour voir l’effet peut être. Restrictions sur certains fichiers peuvent affecter sérieusement le fonctionnement de votre ordinateur ou votre réseau.

-   Informations incorrect sont entrées ou fautes de frappe peut entraîner un paramètre de stratégie qui n’effectue pas comme prévu. Nouveaux paramètres de stratégie de test avant de les appliquer peut empêcher un comportement inattendu.

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>Filtrer les paramètres de stratégie utilisateur selon l’appartenance aux groupes de sécurité.

-   Vous pouvez spécifier les utilisateurs ou groupes pour lesquels vous ne souhaitez pas un paramètre de stratégie à appliquer en désactivant le **appliquer la stratégie de groupe** et **lecture** cases à cocher, ce qui se trouvent sur le **sécurité** onglet de la boîte de dialogue Propriétés de l’objet de stratégie de groupe.

-   Lors de l’autorisation de lecture est refusée, le paramètre de stratégie n’est pas téléchargé par l’ordinateur. Par conséquent, moins de bande passante est consommée en téléchargeant les paramètres de stratégie inutiles, ce qui permet le réseau fonctionne plus rapidement. Pour refuser l’autorisation de lecture, sélectionnez **refuser** pour le **lecture** case à cocher, ce qui se trouve sur le **sécurité** onglet de la boîte de dialogue Propriétés de l’objet de stratégie de groupe.

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>Ne pas lier à un objet de stratégie de groupe dans un autre domaine ou le site.

-   Liaison à un objet de stratégie de groupe dans un autre domaine ou un site peut entraîner une dégradation des performances.

## <a name="additional-resources"></a>Ressources supplémentaires

|Type de contenu|Références|
|--------|-------|
|**Planification**|[Référence technique de stratégies de Restriction logicielle](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx)|
|**Opérations**|[Administrer les stratégies de Restriction logicielle](administer-software-restriction-policies.md)|
|**Résolution des problèmes**|[Stratégies de Restriction logicielle résolution des problèmes (2003)](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx)|
|**Sécurité**|[Menaces et contre-mesures de Restriction logicielle stratégies (2008)](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx)<br /><br />[Menaces et contre-mesures de Restriction logicielle stratégies (2008R2)](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx)|
|**Outils et paramètres**|[Stratégies de Restriction logicielle outils et paramètres (2003)](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx)|
|**Ressources de la Communauté**|[Verrouillage d’applications avec les stratégies de Restriction logicielle](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|


