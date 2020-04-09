---
title: Vue d’ensemble technique des stratégies de restriction logicielle
description: Sécurité de Windows Server
ms.prod: windows-server
ms.technology: security-software-restriction-policies
ms.topic: article
ms.assetid: dc7013b0-0efd-40fd-bd6d-75128adbd0b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 38625a8d416345a6a7ed40c021b55aa10d1fd92f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855272"
---
# <a name="software-restriction-policies-technical-overview"></a>Vue d’ensemble technique des stratégies de restriction logicielle

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit les stratégies de restriction logicielle, le moment et l’utilisation de la fonctionnalité, les modifications qui ont été implémentées dans les versions antérieures et fournit des liens vers des ressources supplémentaires pour vous aider à créer et déployer des stratégies de restriction logicielle à compter de Windows Server 2008 et Windows Vista.

## <a name="introduction"></a>Introduction
Les stratégies de restriction logicielle offrent aux administrateurs un mécanisme de stratégie de groupe pour identifier les logiciels et contrôler leur capacité à s’exécuter sur l’ordinateur local. Ces stratégies peuvent être utilisées pour protéger les ordinateurs exécutant des systèmes d’exploitation Microsoft Windows (à compter de Windows Server 2003 et Windows XP Professionnel) contre les conflits connus et pour protéger les ordinateurs contre les menaces de sécurité telles que les virus malveillants et les chevaux de Troie. Les stratégies de restriction logicielle peuvent aussi contribuer à créer une configuration fortement restreinte pour vos ordinateurs, dans laquelle seule l’exécution d’applications clairement identifiées est autorisée. Les stratégies de restriction logicielle sont intégrées à Microsoft Active Directory et à la stratégie de groupe. Vous avez également la possibilité de créer des stratégies de restriction logicielle sur des ordinateurs autonomes.

Les stratégies de restriction logicielle sont des stratégies d’approbation, c’est-à-dire des règles définies par un administrateur pour limiter des scripts et d’autres formes de code qui n’apparaissent pas entièrement fiables depuis l’exécution. L’extension des stratégies de restriction logicielle de l’éditeur de stratégie de groupe local fournit une interface utilisateur unique par l’intermédiaire de laquelle les paramètres de restriction de l’utilisation des applications peuvent être gérés sur l’ordinateur local ou dans un domaine.

## <a name="procedures"></a>Procédures

-   [Administrer les stratégies de restriction logicielle](administer-software-restriction-policies.md)

    -   [Déterminer l’inventaire des applications et de la liste verte-refuser pour les stratégies de restriction logicielle](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [Utiliser des règles de stratégies de restriction logicielle](work-with-software-restriction-policies-rules.md)

    -   [Utiliser des stratégies de restriction logicielle pour aider à protéger votre ordinateur contre un virus de courrier électronique](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [Résoudre les problèmes liés aux stratégies de restriction logicielle](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>Scénarios d’utilisation de la stratégie de restriction logicielle
Les utilisateurs professionnels collaborent à l’aide du courrier électronique, de la messagerie instantanée et des applications d’égal à égal. À mesure que ces collaborations augmentent, en particulier avec l’utilisation d’Internet dans l’informatique d’entreprise, les menaces liées à du code malveillant, telles que des vers, des virus et des utilisateurs malveillants ou des attaques malveillantes, sont les menaces.

Les utilisateurs peuvent recevoir du code hostile sous de nombreuses formes, allant des fichiers exécutables Windows natifs (fichiers. exe) aux macros dans les documents (tels que les fichiers. doc), aux scripts (tels que les fichiers. vbs). Les utilisateurs malveillants ou les attaquants utilisent souvent des méthodes d’ingénierie sociale pour permettre aux utilisateurs d’exécuter du code contenant des virus et des vers. (Ingénierie sociale est un terme permettant de tromper les utilisateurs en révélant leur mot de passe ou une certaine forme d’informations de sécurité.) Si ce code est activé, il peut générer des attaques par déni de service sur le réseau, envoyer des données sensibles ou privées à Internet, mettre en péril la sécurité de l’ordinateur ou endommager le contenu du disque dur.

Les organisations informatiques et les utilisateurs doivent être en mesure de déterminer les logiciels qui peuvent être exécutés en toute sécurité et qui ne le sont pas. Avec les grands nombres et les formulaires que le code hostile peut prendre, cela devient une tâche difficile.

Pour aider à protéger leurs ordinateurs réseau contre le code hostile et les logiciels inconnus ou non pris en charge, les organisations peuvent implémenter des stratégies de restriction logicielle dans le cadre de leur stratégie de sécurité globale.

Les administrateurs peuvent faire appel aux stratégies de restriction logicielle pour la réalisation des tâches suivantes :

-   Définir ce qu’est un code de confiance

-   Concevoir une stratégie de groupe souple réglementant les scripts, les fichiers exécutables et les contrôles ActiveX

Les stratégies de restriction logicielle sont mises en place par le système d’exploitation et par des applications (notamment les applications d’exécution de scripts) conformes aux stratégies de restriction logicielle.

Les administrateurs peuvent en particulier faire appel aux stratégies de restriction logicielle pour la réalisation des tâches suivantes :

-   Spécifier quels logiciels (fichiers exécutables) peuvent être exécutés sur les ordinateurs clients

-   Empêcher les utilisateurs d’exécuter des programmes spécifiques sur des ordinateurs partagés

-   Spécifier qui peut ajouter des éditeurs approuvés aux ordinateurs clients

-   Définir la portée des stratégies de restriction logicielle (spécifier si les stratégies affectent tous les utilisateurs ou un sous-ensemble d’utilisateurs sur les ordinateurs clients)

-   Empêcher l’exécution des fichiers exécutables sur l’ordinateur local, l’unité d’organisation, le site ou le domaine. Ceci peut s’avérer judicieux dans des cas où vous n’avez pas recours aux stratégies de restriction logicielle pour résoudre d’éventuels problèmes liés à des utilisateurs malveillants.

## <a name="differences-and-changes-in-functionality"></a><a name="BKMK_Diffs_Changes"></a>Différences et modifications des fonctionnalités
Il n’y a aucune modification dans les fonctionnalités de SRP pour Windows Server 2012 et Windows 8.

**Versions prises en charge**

Les stratégies de restriction logicielle peuvent uniquement être configurées et appliquées aux ordinateurs exécutant au moins Windows Server 2003, y compris Windows Server 2012 et au moins Windows XP, y compris Windows 8.

> [!NOTE]
> Certaines éditions du système d’exploitation client Windows à compter de Windows Vista n’ont pas de stratégies de restriction logicielle. Les ordinateurs non administrés dans un domaine par stratégie de groupe peuvent ne pas recevoir de stratégies distribuées.

**Comparaison des fonctions de contrôle d’application dans les stratégies de restriction logicielle et AppLocker**

Le tableau suivant compare les fonctions et fonctionnalités de la fonctionnalité Stratégies de restriction logicielle (SRP) et d'AppLocker.

|Fonction de contrôle de l'application|Stratégies de restriction logicielle|AppLocker|
|----------------|----|-------|
|Portée|Les stratégies SRP peuvent être appliquées à tous les systèmes d'exploitation Windows à compter de Windows XP et de Windows Server 2003.|Les stratégies AppLocker s’appliquent uniquement à Windows Server 2008 R2, Windows Server 2012, Windows 7 et Windows 8.|
|Création de stratégie|Les stratégies de stratégie de restriction logicielle sont gérées via stratégie de groupe et seul l’administrateur de l’objet de stratégie de groupe peut mettre à jour la stratégie SRP. L’administrateur de l’ordinateur local peut modifier les stratégies de SRP définies dans l’objet de stratégie de groupe local.|Les stratégies AppLocker sont gérées par le biais de stratégie de groupe et seul l’administrateur de l’objet de stratégie de groupe peut mettre à jour la stratégie. L’administrateur de l’ordinateur local peut modifier les stratégies AppLocker définies dans l’objet de stratégie de groupe local.<p>AppLocker permet la personnalisation des messages d’erreur pour diriger les utilisateurs vers une page Web pour obtenir de l’aide.|
|Maintenance des stratégies|Les stratégies de stratégie de restriction logicielle doivent être mises à jour à l’aide du composant logiciel enfichable Stratégie de sécurité locale (si les stratégies sont créées localement) ou de la Console de gestion des stratégies de groupe (GPMC).|Les stratégies AppLocker peuvent être mises à jour à l’aide du composant logiciel enfichable Stratégie de sécurité locale (si les stratégies sont créées localement) ou de la console GPMC ou des applets de commande Windows PowerShell AppLocker.|
|Application de stratégie|Les stratégies de stratégie de restriction logicielle sont distribuées via stratégie de groupe.|Les stratégies AppLocker sont distribuées via stratégie de groupe.|
|Mode d’application|SRP fonctionne en mode liste de refus, où les administrateurs peuvent créer des règles pour les fichiers qu’ils ne souhaitent pas autoriser dans cette entreprise, tandis que le reste du fichier est autorisé à s’exécuter par défaut.<p>Le SRP peut également être configuré en « mode liste verte » de telle sorte que tous les fichiers par défaut soient bloqués et que les administrateurs doivent créer des règles d’autorisation pour les fichiers qu’ils souhaitent autoriser.|AppLocker fonctionne par défaut en mode liste verte, où seuls ces fichiers sont autorisés à s’exécuter pour lesquels il existe une règle d’autorisation correspondante.|
|Types de fichiers pouvant être contrôlés|Le SRP peut contrôler les types de fichiers suivants :<p>-Exécutables<br />-Dll<br />-Scripts<br />-Programmes d’installation Windows<p>Le SRP ne peut pas contrôler chaque type de fichier séparément. Toutes les règles de SRP se trouvent dans un seul regroupement de règles.|AppLocker peut contrôler les types de fichiers suivants :<p>-Exécutables<br />-Dll<br />-Scripts<br />-Programmes d’installation Windows<br />-Applications et programmes d’installation empaquetés (Windows Server 2012 et Windows 8)<p>AppLocker gère un regroupement de règles distinct pour chacun des cinq types de fichiers.|
|Types de fichiers désignés|SRP prend en charge une liste extensible de types de fichiers qui sont considérés comme exécutables. Les administrateurs peuvent ajouter des extensions pour les fichiers qui doivent être considérés comme exécutables.|AppLocker ne prend pas en charge cette. AppLocker prend actuellement en charge les extensions de fichier suivantes :<p>-Exécutables (. exe,. com)<br />-Dll (. ocx,. dll)<br />-Scripts (. vbs,. js,. ps1,. cmd,. bat)<br />-Programmes d’installation Windows (. msi,. MST,. msp)<br />-Programmes d’installation d’applications empaquetées (. AppX)|
|Types de règles|SRP prend en charge quatre types de règles :<p>-Hash<br />-Chemin d’accès<br />-Signature<br />-Zone Internet|AppLocker prend en charge trois types de règles :<p>-Hash<br />-Chemin d’accès<br />-Serveur de publication|
|Modification de la valeur de hachage|Les stratégies de restriction logicielle permettent aux administrateurs de fournir des valeurs de hachage personnalisées.|AppLocker calcule la valeur de hachage elle-même. En interne, elle utilise le hachage Authenticode SHA1 pour les exécutables portables (exe et dll) et les programmes d’installation Windows et un hachage de fichier plat SHA1 pour le reste.|
|Prise en charge de différents niveaux de sécurité|Les administrateurs de SRP peuvent spécifier les autorisations avec lesquelles une application peut s’exécuter. Par conséquent, un administrateur peut configurer une règle de sorte que le bloc-notes s’exécute toujours avec des autorisations restreintes et jamais avec des privilèges d’administrateur.<p>SRP sur Windows Vista et versions antérieures prenait en charge plusieurs niveaux de sécurité. Sur Windows 7, cette liste était limitée à deux niveaux seulement : non autorisé et non restreint (l’utilisateur de base se traduit par non autorisé).|AppLocker ne prend pas en charge les niveaux de sécurité.|
|Gérer les applications empaquetées et les programmes d’installation d’applications empaquetés|Mesure|. AppX est un type de fichier valide qu’AppLocker peut gérer.|
|Ciblage d’une règle pour un utilisateur ou un groupe d’utilisateurs|Les règles de SRP s’appliquent à tous les utilisateurs sur un ordinateur particulier.|Les règles AppLocker peuvent être ciblées pour un utilisateur ou un groupe d’utilisateurs spécifique.|
|Prise en charge des exceptions de règle|SRP ne prend pas en charge les exceptions de règle|Les règles AppLocker peuvent avoir des exceptions qui permettent aux administrateurs de créer des règles telles que « autoriser tous les éléments à partir de Windows à l’exception de Regedit. exe ».|
|Prise en charge du mode audit|SRP ne prend pas en charge le mode audit. La seule façon de tester des stratégies de stratégie de restriction logicielle consiste à configurer un environnement de test et à exécuter quelques expériences.|AppLocker prend en charge le mode audit qui permet aux administrateurs de tester l’effet de leur stratégie dans l’environnement de production réel sans affecter l’expérience utilisateur. Une fois que vous êtes satisfait des résultats, vous pouvez commencer à appliquer la stratégie.|
|Prise en charge de l’exportation et de l’importation de stratégies|SRP ne prend pas en charge l’importation/exportation de stratégie.|AppLocker prend en charge l’importation et l’exportation de stratégies. Cela vous permet de créer une stratégie AppLocker sur un exemple d’ordinateur, de la tester, puis de l’exporter et de l’importer à nouveau dans l’objet de stratégie de groupe souhaité.|
|Application des règles|En interne, l’application des règles SRP s’effectue en mode utilisateur, ce qui est moins sécurisé.|En interne, les règles AppLocker pour les fichiers exe et les dll sont appliquées en mode noyau, ce qui est plus sécurisé que de les appliquer en mode utilisateur.|

## <a name="system-requirements"></a>Configuration requise
Les stratégies de restriction logicielle peuvent uniquement être configurées et appliquées aux ordinateurs exécutant au moins Windows Server 2003 et au moins Windows XP. Stratégie de groupe est requis pour distribuer des objets stratégie de groupe qui contiennent des stratégies de restriction logicielle.

## <a name="software-restriction-policies-components-and-architecture"></a>Architecture et composants des stratégies de restriction logicielle
Les stratégies de restriction logicielle fournissent un mécanisme pour le système d’exploitation et les applications compatibles avec les stratégies de restriction logicielle pour limiter l’exécution du runtime des programmes logiciels.

À un niveau élevé, les stratégies de restriction logicielle comprennent les composants suivants :

-   API des stratégies de restriction logicielle. Les interfaces de programmation d’applications (API) sont utilisées pour créer et configurer les règles qui constituent la stratégie de restriction logicielle. Il existe également des API de stratégies de restriction logicielle pour l’interrogation, le traitement et l’application des stratégies de restriction logicielle.

-   Outil de gestion des stratégies de restriction logicielle. Il s’agit de l’extension des **stratégies de restriction logicielle** du composant logiciel enfichable de l' **éditeur d’objets stratégie de groupe local** , que les administrateurs utilisent pour créer et modifier les stratégies de restriction logicielle.

-   Ensemble d’API de système d’exploitation et d’applications qui appellent les API de stratégies de restriction logicielle pour assurer l’application des stratégies de restriction logicielle au moment de l’exécution.

-   Active Directory et stratégie de groupe. Les stratégies de restriction logicielle dépendent de l’infrastructure stratégie de groupe pour propager les stratégies de restriction logicielle du Active Directory aux clients appropriés, et pour définir l’étendue et filtrer l’application de ces stratégies sur les ordinateurs cibles appropriés.

-   API d’approbation Authenticode et WinVerify utilisées pour traiter les fichiers exécutables signés.

-   observateur d'événements. Les fonctions utilisées par les stratégies de restriction logicielle consignent les événements dans les journaux de observateur d’événements.

-   Jeu de stratégies résultant (RSoP), qui peut faciliter le diagnostic de la stratégie effective qui sera appliquée à un client.

Pour plus d’informations sur l’architecture du SRP, sur la façon dont les stratégies de restriction logicielle gèrent les règles, les processus et les interactions, voir [Comment les stratégies de restriction logicielle fonctionnent](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx) dans la bibliothèque technique de Windows Server 2003.

## <a name="best-practices"></a><a name="BKMK_Best_Practices"></a>Meilleures pratiques

### <a name="do-not-modify-the-default-domain-policy"></a>Ne modifiez pas la stratégie de domaine par défaut.

-   Si vous ne modifiez pas la stratégie de domaine par défaut, vous avez toujours la possibilité de réappliquer la stratégie de domaine par défaut en cas de problème avec votre stratégie de domaine personnalisée.

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>Créez un objet stratégie de groupe distinct pour les stratégies de restriction logicielle.

-   Si vous créez un objet de stratégie de groupe distinct pour les stratégies de restriction logicielle, vous pouvez désactiver les stratégies de restriction logicielle en cas d’urgence sans désactiver le reste de votre stratégie de domaine.

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>Si vous rencontrez des problèmes avec les paramètres de stratégie appliqués, redémarrez Windows en mode sans échec.

-   Les stratégies de restriction logicielle ne s’appliquent pas lorsque Windows est démarré en mode sans échec. Si vous verrouillez accidentellement une station de travail avec des stratégies de restriction logicielle, redémarrez l’ordinateur en mode sans échec, connectez-vous en tant qu’administrateur local, modifiez la stratégie, exécutez **gpupdate**, redémarrez l’ordinateur, puis ouvrez une session normalement.

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>Soyez prudent lors de la définition d’un paramètre par défaut non autorisé.

-   Lorsque vous définissez un paramètre par défaut sur non **autorisé**, tous les logiciels sont interdits, à l’exception des logiciels qui ont été explicitement autorisés. Tout fichier que vous souhaitez ouvrir doit avoir une règle de stratégie de restriction logicielle qui l’autorise à s’ouvrir.

-   Pour empêcher les administrateurs de verrouiller eux-mêmes le système, lorsque le niveau de sécurité par défaut est défini sur non **autorisé**, quatre règles de chemin d’accès au Registre sont automatiquement créées. Vous pouvez supprimer ou modifier ces règles de chemin d’accès au registre ; Toutefois, cela n’est pas recommandé.

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>Pour une sécurité optimale, utilisez les listes de contrôle d’accès conjointement avec les stratégies de restriction logicielle.

-   Les utilisateurs peuvent tenter de contourner les stratégies de restriction logicielle en renommant ou en déplaçant des fichiers non autorisés ou en remplaçant des fichiers non restreints. Par conséquent, il est recommandé d’utiliser des listes de contrôle d’accès (ACL) pour refuser aux utilisateurs l’accès nécessaire à l’exécution de ces tâches.

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>Testez les nouveaux paramètres de stratégie minutieusement dans les environnements de test avant d’appliquer les paramètres de stratégie à votre domaine.

-   Les nouveaux paramètres de stratégie peuvent agir différemment de ceux prévus initialement. Les tests réduisent le risque de rencontrer un problème lorsque vous déployez des paramètres de stratégie sur votre réseau.

-   Vous pouvez configurer un domaine de test, distinct du domaine de votre organisation, dans lequel tester les nouveaux paramètres de stratégie. Vous pouvez également tester les paramètres de stratégie en créant un objet de stratégie de groupe de test et en le liant à une unité d’organisation de test. Une fois que vous avez testé minutieusement les paramètres de stratégie avec les utilisateurs de test, vous pouvez lier l’objet de stratégie de groupe de test à votre domaine.

-   Ne définissez pas de programmes ou de fichiers à **interdire** sans les tester pour voir ce que l’effet peut être. Les restrictions sur certains fichiers peuvent avoir un impact sérieux sur le fonctionnement de votre ordinateur ou réseau.

-   Les informations qui sont entrées de manière incorrecte ou qui frappent des erreurs peuvent entraîner un paramètre de stratégie qui ne fonctionne pas comme prévu. Tester les nouveaux paramètres de stratégie avant de les appliquer peut empêcher un comportement inattendu.

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>Filtrez les paramètres de stratégie utilisateur en fonction de l’appartenance aux groupes de sécurité.

-   Vous pouvez spécifier les utilisateurs ou les groupes pour lesquels vous ne souhaitez pas qu’un paramètre de stratégie s’applique en désactivant les cases à cocher **appliquer le stratégie de groupe** et **lire** , situées sous l’onglet **sécurité** de la boîte de dialogue Propriétés de l’objet de stratégie de groupe.

-   Lorsque l’autorisation de lecture est refusée, le paramètre de stratégie n’est pas téléchargé par l’ordinateur. Par conséquent, moins de bande passante est consommée en téléchargeant des paramètres de stratégie inutiles, ce qui permet au réseau de fonctionner plus rapidement. Pour refuser l’autorisation lecture, sélectionnez **refuser** pour la case à cocher **lecture** , qui se trouve sous l’onglet **sécurité** de la boîte de dialogue Propriétés de l’objet de stratégie de groupe.

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>Ne pas lier à un objet de stratégie de groupe dans un autre domaine ou site.

-   La liaison à un objet de stratégie de groupe dans un autre domaine ou site peut entraîner des performances médiocres.

## <a name="additional-resources"></a>Ressources supplémentaires

|Type de contenu|Références|
|--------|-------|
|**Planification**|[Informations techniques de référence sur les stratégies de restriction logicielle](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx)|
|**Opérations**|[Administrer les stratégies de restriction logicielle](administer-software-restriction-policies.md)|
|**Résolution des problèmes**|[Résolution des problèmes liés aux stratégies de restriction logicielle (2003)](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx)|
|**Sécurité**|[Menaces et contre-mesures pour les stratégies de restriction logicielle (2008)](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx)<p>[Menaces et contre-mesures pour les stratégies de restriction logicielle (2008 R2)](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx)|
|**Outils et paramètres**|[Outils et paramètres des stratégies de restriction logicielle (2003)](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx)|
|**Ressources de la communauté**|[Verrouillage d’applications avec des stratégies de restriction logicielle](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|


