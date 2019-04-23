---
title: Vue d’ensemble technique des stratégies de restriction logicielle
description: Sécurité de Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830850"
---
# <a name="software-restriction-policies-technical-overview"></a>Vue d’ensemble technique des stratégies de restriction logicielle

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit les stratégies de restriction logicielle, quand et comment utiliser la fonctionnalité, quelles modifications ont été implémentées dans les versions précédentes et fournit des liens vers des ressources supplémentaires pour vous aider à créer et déployer des stratégies de restriction logicielle à partir de Windows Server 2008 et Windows Vista.

## <a name="introduction"></a>Introduction
Stratégies de restriction logicielle fournissent aux administrateurs un mécanisme pilotés par stratégie de groupe pour identifier les logiciels et contrôler son exécution sur l’ordinateur local. Ces stratégies peuvent être utilisées pour protéger les ordinateurs qui exécutent les systèmes d’exploitation Microsoft Windows (commençant par Windows Server 2003 et Windows XP Professionnel) contre les conflits connus et protéger les ordinateurs contre les menaces telles que les virus malveillants et les chevaux de Troie. Les stratégies de restriction logicielle peuvent aussi contribuer à créer une configuration fortement restreinte pour vos ordinateurs, dans laquelle seule l’exécution d’applications clairement identifiées est autorisée. Les stratégies de restriction logicielle sont intégrées à Microsoft Active Directory et à la stratégie de groupe. Vous avez également la possibilité de créer des stratégies de restriction logicielle sur des ordinateurs autonomes.

Les stratégies de restriction logicielle sont des stratégies d’approbation, c’est-à-dire des règles définies par un administrateur pour limiter des scripts et d’autres formes de code qui n’apparaissent pas entièrement fiables depuis l’exécution. L’extension stratégies de Restriction logicielle pour l’éditeur de stratégie de groupe locale fournit une seule interface utilisateur par le biais duquel les paramètres pour restreindre l’utilisation d’applications peuvent être gérés sur l’ordinateur local ou dans tout le domaine.

## <a name="procedures"></a>Procédures

-   [Administrer les stratégies de Restriction logicielle](administer-software-restriction-policies.md)

    -   [Déterminer la liste d’autorisation / exclusion et l’inventaire des applications pour les stratégies de Restriction logicielle](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [Travailler avec des règles de stratégies de Restriction logicielle](work-with-software-restriction-policies-rules.md)

    -   [Utiliser des stratégies de Restriction logicielle pour vous aider à protéger votre ordinateur contre un Virus de courrier électronique](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [Résoudre les problèmes de stratégies de Restriction logicielle](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>Scénarios de l’utilisation de stratégie de restriction logicielle
Les utilisateurs professionnels collaborent à l’aide de courrier électronique, messagerie instantanée et les applications de peer-to-peer. Comme ces collaborations augmente, en particulier avec l’utilisation d’Internet dans l’entreprise informatique, cela les menaces à partir du code malveillant, tels que les vers, virus et utilisateur malveillant ou les menaces attaquant.

Les utilisateurs peuvent recevoir un code hostile sous plusieurs formes, allant de natif Windows exécutable (fichiers .exe), aux macros dans les documents (tels que les fichiers .doc) pour les scripts (tels que les fichiers .vbs). Les utilisateurs malveillants ou les attaquants utilisent souvent des méthodes d’ingénierie sociale pour que les utilisateurs d’exécuter du code contenant des virus et vers. (Ingénierie sociale est un terme ruse son mot de passe ou d’une forme d’informations de sécurité.) Si ce code est activé, il peut générer les attaques par déni de service sur le réseau, envoyer des données sensibles ou privées à Internet, risque de compromettre la sécurité de l’ordinateur ou d’endommager le contenu du lecteur de disque dur.

Les utilisateurs et les organisations informatiques doivent être en mesure de déterminer quels logiciels doivent sont exécuter sans risque et qui n’est pas. Grand nombre et forms code hostile peut prendre, cela devient une tâche difficile.

Pour aider à protéger leurs ordinateurs réseau à partir d’un code hostile et logiciels inconnus ou non pris en charge, les organisations peuvent implémenter des stratégies de restriction logicielle dans le cadre de leur stratégie de sécurité.

Les administrateurs peuvent faire appel aux stratégies de restriction logicielle pour la réalisation des tâches suivantes :

-   Définir ce qu’est un code de confiance

-   Concevoir une stratégie de groupe souple réglementant les scripts, les fichiers exécutables et les contrôles ActiveX

Les stratégies de restriction logicielle sont mises en place par le système d’exploitation et par des applications (notamment les applications d’exécution de scripts) conformes aux stratégies de restriction logicielle.

Les administrateurs peuvent en particulier faire appel aux stratégies de restriction logicielle pour la réalisation des tâches suivantes :

-   Spécifiez quels logiciels (fichiers exécutables) peuvent s’exécuter sur les ordinateurs clients

-   Empêcher les utilisateurs d’exécuter des programmes spécifiques sur des ordinateurs partagés

-   Spécifiez qui peut ajouter des éditeurs approuvés sur les ordinateurs clients

-   Définir l’étendue des stratégies de restriction logicielle (spécifier si les stratégies affectent tous les utilisateurs ou un sous-ensemble d’utilisateurs sur les ordinateurs clients)

-   Empêcher l’exécution des fichiers exécutables sur l’ordinateur local, l’unité d’organisation, le site ou le domaine. Ceci peut s’avérer judicieux dans des cas où vous n’avez pas recours aux stratégies de restriction logicielle pour résoudre d’éventuels problèmes liés à des utilisateurs malveillants.

## <a name="BKMK_Diffs_Changes"></a>Différences et les modifications apportées aux fonctionnalités
Il n’existe aucune modification de fonctionnalités des stratégies de restriction logicielle pour Windows Server 2012 et Windows 8.

**Versions prises en charge**

Stratégies de Restriction logicielle peuvent uniquement être configurés sur et appliquées aux ordinateurs exécutant au moins Windows Server 2003, notamment Windows Server 2012 et au moins Windows XP, y compris Windows 8.

> [!NOTE]
> Certaines éditions du début de système d’exploitation client Windows avec Windows Vista n’ont pas de stratégies de restriction logicielle. Ordinateurs n'administrés pas dans un domaine par la stratégie de groupe ne peuvent pas recevoir des stratégies distribuées.

**Comparaison des fonctions de contrôle d’application dans les stratégies de Restriction logicielle et AppLocker**

Le tableau suivant compare les fonctions et fonctionnalités de la fonctionnalité Stratégies de restriction logicielle (SRP) et d'AppLocker.

|Fonction de contrôle de l'application|Stratégies de restriction logicielle|AppLocker|
|----------------|----|-------|
|Étendue|Les stratégies SRP peuvent être appliquées à tous les systèmes d'exploitation Windows à compter de Windows XP et de Windows Server 2003.|Les stratégies AppLocker s’appliquent uniquement à Windows Server 2008 R2, Windows Server 2012, Windows 7 et Windows 8.|
|Création d’une stratégie|Stratégies de restriction logicielles sont gérées via une stratégie de groupe, et seul l’administrateur de l’objet de stratégie de groupe permettre mettre à jour la stratégie de restriction logicielle. L’administrateur sur l’ordinateur local peut modifier les stratégies de restriction logicielles définies dans l’objet de stratégie de groupe local.|Les stratégies AppLocker sont gérées via une stratégie de groupe, et seul l’administrateur de l’objet de stratégie de groupe permettre mettre à jour la stratégie. L’administrateur sur l’ordinateur local peut modifier les stratégies AppLocker définis dans l’objet de stratégie de groupe local.<br /><br />AppLocker permet la personnalisation des messages d’erreur pour diriger les utilisateurs vers une page Web pour obtenir de l’aide.|
|Maintenance des stratégies|Stratégies de restriction logicielles doivent être mis à jour en utilisant le composant logiciel enfichable Stratégie de sécurité locale (si les stratégies sont créées localement) ou de la Console de gestion des stratégies de groupe (GPMC).|Les stratégies AppLocker peuvent être mis à jour à l’aide de la stratégie de sécurité locale enfichable (si les stratégies sont créées localement), ou la console GPMC ou les applets de commande Windows PowerShell AppLocker.|
|Application de la stratégie|Stratégies de restriction logicielles sont distribuées via la stratégie de groupe.|Les stratégies AppLocker sont distribuées via la stratégie de groupe.|
|Mode d’application|Stratégies de restriction logicielle fonctionne dans le « mode liste de deny » où les administrateurs peuvent créer des règles pour les fichiers, ils ne souhaitent pas autoriser dans cette entreprise, tandis que le reste du fichier sont autorisés à exécuter par défaut.<br /><br />SRP peut également être configuré dans « Autoriser le mode liste » telles que le par défaut tous les fichiers sont bloqués et les administrateurs ont besoin créer des règles d’autorisation pour les fichiers qu’ils souhaitent autoriser.|AppLocker par fonctionne par défaut dans la section « Autoriser le mode liste » où les seuls ces fichiers sont autorisés à exécuter pour lequel il est mise en correspondance une règle d’autorisation.|
|Types de fichiers qui peuvent être contrôlées|Stratégies de restriction logicielle peuvent contrôler les types de fichier suivants :<br /><br />-Fichiers exécutables<br />-   Dlls<br />-   Scripts<br />-Programmes d’installation de Windows<br /><br />Stratégies de restriction logicielle ne peut pas contrôler chaque type de fichier séparément. Toutes les règles de stratégies de restriction logicielle sont dans un regroupement de règles unique.|AppLocker peut contrôler les types de fichiers suivants :<br /><br />-Fichiers exécutables<br />-   Dlls<br />-   Scripts<br />-Programmes d’installation de Windows<br />-Applications empaquetées et programmes d’installation (Windows Server 2012 et Windows 8)<br /><br />AppLocker gère une collection de règle distincte pour chacun des types de cinq fichiers.|
|Types de fichiers désignés|Stratégies de restriction logicielle prend en charge une liste extensible des types de fichiers qui sont considérés comme exécutable. Les administrateurs peuvent ajouter des extensions de fichiers qui doivent être considéré comme exécutables.|AppLocker ne prend pas en charge cela. AppLocker prend actuellement en charge les extensions de fichier suivantes :<br /><br />-Fichiers exécutables (.exe, .com)<br />-DLL (.ocx, .dll)<br />-   Scripts (.vbs, .js, .ps1, .cmd, .bat)<br />-Programmes d’installation de Windows (.msi, .mst, .msp)<br />-Programmes d’installation de l’application empaquetée (.aspx)|
|Types de règles|Stratégies de restriction logicielle prend en charge quatre types de règles :<br /><br />-Hash<br />-   Path<br />-   Signature<br />-   Internet zone|AppLocker prend en charge trois types de règles :<br /><br />-Hash<br />-   Path<br />-   Publisher|
|Modification de la valeur de hachage|SRP permet aux administrateurs de fournir des valeurs de hachage personnalisé.|AppLocker calcule la valeur de hachage lui-même. En interne, il utilise le hachage SHA1 Authenticode pour portables exécutables (Exe et Dll) et les programmes d’installation de Windows et un hachage de fichier plat de SHA1 pour le reste.|
|Prise en charge de différents niveaux de sécurité|Avec les stratégies de restriction logicielle, les administrateurs peuvent spécifier les autorisations dont une application peut exécuter. Par conséquent, un administrateur peut configurer une règle telle que le bloc-notes s’exécute toujours avec des autorisations restreintes et jamais avec des privilèges d’administrateur.<br /><br />Stratégies de restriction logicielle sur Windows Vista et versions antérieures prises en charge plusieurs niveaux de sécurité. Sur Windows 7, cette liste était limitée à deux niveaux : Interdites et illimité (utilisateur de base se traduit par non autorisé).|AppLocker ne prend pas en charge les niveaux de sécurité.|
|Gérer les applications empaquetées et programmes d’installation des applications empaquetées|Impossible de|.aspx est un type de fichier valide qui AppLocker peut gérer.|
|Ciblage d’une règle à un utilisateur ou un groupe d’utilisateurs|Règles de stratégies de restriction logicielle s’appliquent à tous les utilisateurs sur un ordinateur particulier.|Les règles AppLocker peuvent être destinées à un utilisateur spécifique ou un groupe d’utilisateurs.|
|Prise en charge pour les exceptions de règle|Stratégies de restriction logicielle ne prend pas en charge les exceptions de règle|Les règles AppLocker peuvent avoir des exceptions qui permettent aux administrateurs de créer des règles telles que « Autoriser tous les éléments à partir de Windows à l’exception de Regedit.exe ».|
|Prise en charge pour le mode d’audit|Stratégies de restriction logicielle ne prend pas en charge le mode audit. La seule façon de tester les stratégies de restriction logicielle consiste à configurer un environnement de test et exécuter des expériences de quelques.|AppLocker prend en charge le mode d’audit qui permet aux administrateurs de tester l’effet de leur stratégie dans l’environnement de production réel, sans affecter l’expérience utilisateur. Une fois que vous êtes satisfait des résultats, vous pouvez commencer en appliquant la stratégie.|
|Prise en charge pour exporter et importer des stratégies|Stratégies de restriction logicielle ne prend pas en charge l’importation/exportation de stratégie.|AppLocker prend en charge l’importation et exportation de stratégies. Cela vous permet de créer la stratégie AppLocker sur un ordinateur de l’exemple, tester et exportez cette stratégie, puis le réimporter dans l’objet de stratégie de groupe souhaité.|
|Application de règles|En interne, la mise en œuvre des règles de stratégies de restriction logicielle se produit dans le mode utilisateur qui est moins sécurisé.|En interne, les règles AppLocker pour les fichiers exe et DLL sont appliquées dans le mode noyau qui est plus sécurisé que les appliquer dans le mode utilisateur.|

## <a name="system-requirements"></a>Configuration requise
Stratégies de restriction logicielle peuvent uniquement être configurés sur et appliquées aux ordinateurs exécutant au moins Windows Server 2003 et au moins Windows XP. Stratégie de groupe est requise pour distribuer des objets de stratégie de groupe qui contiennent des stratégies de restriction logicielle.

## <a name="software-restriction-policies-components-and-architecture"></a>Architecture et composants de stratégies de restriction logicielle
Stratégies de restriction logicielle fournissent un mécanisme pour le système d’exploitation et les applications conformes aux stratégies de restriction logicielle pour limiter l’exécution de l’exécution de logiciels.

À un niveau élevé, les stratégies de restriction logicielle sont constituées des composants suivants :

-   Stratégies de restriction logicielle API. Les Interfaces de programmation d’Application (API) sont utilisés pour créer et configurer les règles qui constituent la stratégie de restriction logicielle. Il existe également des stratégies de restriction logicielle API pour l’interrogation, le traitement et l’application des stratégies de restriction logicielle.

-   Un outil de gestion des stratégies de restriction logicielle. Il s’agit de la **stratégies de Restriction logicielle** extension de la **Éditeur d’objets de stratégie de groupe locale** enfichable, que les administrateurs utilisent pour créer et modifier des stratégies de restriction logicielle.

-   Un ensemble d’API du système d’exploitation et les applications qui appellent les stratégies de restriction logicielle API pour fournir la mise en œuvre de stratégies de restriction logicielle lors de l’exécution.

-   Active Directory et stratégie de groupe. Stratégies de restriction logicielle dépendent de l’infrastructure de stratégie de groupe pour propager des stratégies de restriction logicielle à partir d’Active Directory pour les clients appropriés et pour l’étendue et de filtrage de l’application de ces stratégies appropriés ordinateurs cibles.

-   Authenticode et WinVerify d’approbation des API qui permettent de traiter les fichiers exécutables signés.

-   Observateur d’événements. Les fonctions utilisées par les événements de journal de stratégies de restriction logiciel dans les journaux de l’Observateur d’événements.

-   Résultant jeu de stratégies résultant (RSoP), ce qui peut faciliter le diagnostic de la stratégie actuelle qui est appliquée à un client.

Pour plus d’informations sur l’architecture des stratégies de restriction logicielle, la manière dont les stratégies de restriction logicielle gère des règles, processus et interactions, consultez [Software Restriction stratégies fonctionnement](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx) dans la bibliothèque technique de Windows Server 2003.

## <a name="BKMK_Best_Practices"></a>Meilleures pratiques

### <a name="do-not-modify-the-default-domain-policy"></a>Ne modifiez pas la stratégie de domaine par défaut.

-   Si vous ne modifiez pas la stratégie de domaine par défaut, vous avez toujours la possibilité de réappliquer la stratégie de domaine par défaut, si une erreur survient avec votre stratégie de domaine personnalisé.

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>Créer un objet de stratégie de groupe distinct pour les stratégies de restriction logicielle.

-   Si vous créez un objet de stratégie groupe distinct (GPO) pour les stratégies de restriction logicielle, vous pouvez désactiver les stratégies de restriction logicielle en cas d’urgence sans désactiver le reste de votre stratégie de domaine.

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>Si vous rencontrez des problèmes avec les paramètres de stratégie appliqués, redémarrez Windows en Mode sans échec.

-   Stratégies de restriction logicielle ne s’appliquent pas lorsque Windows est démarré en Mode sans échec. Si vous verrouillez accidentellement une station de travail avec les stratégies de restriction logicielle, redémarrez l’ordinateur en Mode sans échec, connectez-vous en tant qu’administrateur local, modifiez la stratégie, exécutez **gpupdate**, redémarrez l’ordinateur, puis ouvrir une session normalement.

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>Soyez prudent lorsque vous définissez un paramètre par défaut est rejeté.

-   Lorsque vous définissez le paramètre par défaut est **rejeté**, tous les logiciels ne sont pas autorisé à l’exception des logiciels qui ont été explicitement autorisés. N’importe quel fichier que vous souhaitez ouvrir doit avoir une restriction de logiciels des stratégies de règle qui lui permet d’ouvrir.

-   Pour protéger les administrateurs de procéder eux-mêmes au verrouillage du système, lorsque le niveau de sécurité par défaut est défini sur **rejeté**, quatre règles de chemin d’accès de Registre sont créées automatiquement. Vous pouvez supprimer ou modifier ces règles de chemin d’accès de Registre ; Toutefois, cela n’est pas recommandé.

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>Pour une sécurité optimale, utilisez les listes de contrôle d’accès conjointement avec les stratégies de restriction logicielle.

-   Les utilisateurs peuvent tenter de contourner les stratégies de restriction logicielle en renommant ou en déplaçant les fichiers rejetés ou en remplaçant des fichiers sans restriction. Par conséquent, il est recommandé d’utiliser des listes de contrôle d’accès (ACL) afin de refuser aux utilisateurs l’accès nécessaire pour effectuer ces tâches.

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>Tester minutieusement les nouveaux paramètres de stratégie dans les environnements de test avant d’appliquer les paramètres de stratégie à votre domaine.

-   Nouveaux paramètres de stratégie peuvent agir différemment que prévu à l’origine. Test permet de réduire le risque de rencontrer un problème lorsque vous déployez des paramètres de stratégie sur votre réseau.

-   Vous pouvez configurer un domaine de test distinct du domaine de votre organisation dans laquelle effectuer un test de nouveaux paramètres de stratégie. Vous pouvez également tester les paramètres de stratégie en créant un objet stratégie de groupe de test et de lier à une unité d’organisation de test. Lorsque vous avez soigneusement testé les paramètres de stratégie avec les utilisateurs de test, vous pouvez lier l’objet stratégie de groupe de test à votre domaine.

-   Ne définissez pas de programmes ou des fichiers à **rejeté** sans les tester pour voir ce qui peut représenter l’effet. Restrictions sur certains fichiers peuvent affecter sérieusement le fonctionnement de votre ordinateur ou réseau.

-   Informations incorrectement entrées ou des fautes de frappe peut entraîner un paramètre de stratégie qui n’effectue pas comme prévu. Testez les nouveaux paramètres de stratégie avant de les appliquer pour éviter un comportement inattendu.

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>Filtrer les paramètres de stratégie utilisateur selon l’appartenance aux groupes de sécurité.

-   Vous pouvez spécifier les utilisateurs ou groupes pour lesquels vous ne souhaitez pas un paramètre de stratégie à appliquer en désactivant le **appliquer la stratégie de groupe** et **en lecture** cases à cocher, qui se trouvent sur le **sécurité**onglet de la boîte de dialogue Propriétés de l’objet de stratégie de groupe.

-   Lorsque l’autorisation en lecture est refusée, le paramètre de stratégie n’est pas téléchargé par l’ordinateur. Par conséquent, moins de bande passante est consommée en téléchargeant les paramètres de stratégie inutiles, ce qui permet le réseau de fonctionner plus rapidement. Pour refuser l’autorisation de lecture, sélectionnez **Deny** pour le **en lecture** case à cocher, qui se trouve sur le **sécurité** onglet de la boîte de dialogue Propriétés de l’objet de stratégie de groupe.

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>Ne pas lier à un objet GPO dans un autre domaine ou site.

-   Liaison à un objet GPO dans un autre domaine ou un site peut entraîner une dégradation des performances.

## <a name="additional-resources"></a>Ressources supplémentaires

|Type de contenu|Références|
|--------|-------|
|**Planification**|[Référence technique de stratégies de Restriction logicielle](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx)|
|**Opérations**|[Administrer les stratégies de Restriction logicielle](administer-software-restriction-policies.md)|
|**Résolution des problèmes**|[Stratégies de Restriction logicielle dépannage (2003)](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx)|
|**Sécurité**|[Menaces et contre-mesures de Restriction logicielle stratégies (2008)](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx)<br /><br />[Menaces et contre-mesures de Restriction logicielle stratégies (2008 R2)](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx)|
|**Outils et paramètres**|[Stratégies de Restriction logicielle outils et paramètres (2003)](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx)|
|**Ressources de la communauté**|[Verrouillage d’applications avec les stratégies de Restriction logicielle](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|


