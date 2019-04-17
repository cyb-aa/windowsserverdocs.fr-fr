---
title: "Travailler avec des règles de stratégies de Restriction logicielle"
description: "Sécurité de Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a8047d5-9bb9-4bed-bc8f-583a237731e2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 2dd1810b50f4f02be99eb2e2c0893501f99d1e93
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="work-with-software-restriction-policies-rules"></a>Travailler avec des règles de stratégies de Restriction logicielle

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique décrit les procédures d’utilisation de certificat, chemin d’accès, internet les règles de zone et le hachage à l’aide de stratégies de Restriction logicielle.

## <a name="introduction"></a>Introduction
Avec les stratégies de restriction logicielle, vous pouvez protéger votre environnement informatique contre les logiciels non approuvés en identifiant et en spécifiant les logiciels autorisés à s’exécuter. Vous pouvez définir un niveau de sécurité par défaut **non restreint** ou **non autorisé** pour un objet de stratégie de groupe (GPO) afin que le logiciel est autorisée ou non autorisées à s’exécuter par défaut. Vous pouvez créer des exceptions à ce niveau de sécurité par défaut en créant des règles de stratégies d’un logiciel spécifique de restriction logicielle. Par exemple, si le niveau de sécurité par défaut est défini sur **non autorisé**, vous pouvez créer des règles qui autorisent l’exécution de logiciels spécifiques. Les types de règles sont les suivantes:

-   **Règles de certificat**

    Pour connaître les procédures, voir [fonctionne avec les règles de certificat](#BKMK_Cert_Rules).

-   **Règles de hachage**

    Pour connaître les procédures, voir [utilisation des règles de hachage](#BKMK_Hash_Rules).

-   **Règles de zone Internet**

    Pour connaître les procédures, voir [utilisation des règles de Zone Internet](#BKMK_Internet_Zone_Rules).

-   **Règles de chemin d’accès**

    Pour connaître les procédures, voir [utilisation des règles de chemin d’accès](#BKMK_Path_Rules).

Pour plus d’informations sur les autres tâches pour gérer les stratégies de Restriction logicielle, consultez [administrer les stratégies de Restriction logicielle](administer-software-restriction-policies.md).

## <a name="BKMK_Cert_Rules"></a>Utilisation des règles de certificat
Stratégies de restriction logicielle peuvent également identifier un logiciel par son certificat de signature. Vous pouvez créer une règle de certificat qui identifie le logiciel et autorise ou n’autorise pas le logiciel à s’exécuter, en fonction du niveau de sécurité. Par exemple, vous pouvez utiliser les règles de certificat pour approuver automatiquement les logiciels à partir d’une source approuvée dans un domaine sans solliciter l’utilisateur. Vous pouvez également utiliser les règles de certificat pour exécuter des fichiers dans les zones non autorisées de votre système d’exploitation. Les règles de certificat ne sont pas activés par défaut.

Lorsque les règles sont créées pour le domaine à l’aide de la stratégie de groupe, vous devez disposer des autorisations pour créer ou modifier un objet de stratégie de groupe. Si vous créez des règles pour l’ordinateur local, vous devez disposer des informations d’identification administratives sur cet ordinateur.

#### <a name="to-create-a-certificate-rule"></a>Pour créer une règle de certificat

1.  Ouvrez stratégies de Restriction logicielle.

2.  Dans l’arborescence de la console ou le volet d’informations, cliquez sur **des règles supplémentaires**, puis cliquez sur **nouvelle règle de certificat**.

3.  Cliquez sur **Parcourir**, puis sélectionnez un certificat ou un fichier signé.

4.  Dans **niveau de sécurité**, cliquez sur **non autorisé** ou **non restreint**.

5.  Dans **Description**, tapez une description pour cette règle, puis cliquez sur **OK**.

> [!NOTE]
> -   Il peut être nécessaire de créer un nouveau paramètre de stratégie de restriction logicielle pour l’objet de stratégie de groupe (GPO) si vous ne le n'avez pas déjà fait.
> -   Les règles de certificat ne sont pas activés par défaut.
> -   Les seuls types de fichiers qui sont affectés par les règles de certificat sont ceux qui sont répertoriés dans **types de fichiers désignés** dans le volet d’informations des stratégies de Restriction logicielle. Il existe une liste de types de fichiers désignés est partagée par toutes les règles.
> -   Stratégies de restriction logicielle prenne effet, les utilisateurs doivent mettre à jour les paramètres de stratégie en déconnectant et ouvert une session sur leurs ordinateurs.
> -   Lorsque plusieurs règles de stratégies de restriction logicielle est appliqué aux paramètres de stratégie, il existe un niveau de priorité des règles pour la gestion des conflits.

### <a name="enabling-certificate-rules"></a>L’activation des règles de certificat
Il existe différentes procédures d’activation des règles de certificat en fonction de votre environnement:

-   [Pour votre ordinateur local](#BKMK_1)

-   [Pour un objet de stratégie de groupe, lorsque vous vous trouvez sur un serveur qui est joint à un domaine](#BKMK_2)

-   [Pour un objet de stratégie de groupe, lorsque vous vous trouvez sur un contrôleur de domaine ou une station de travail suite qui a les outils d’Administration de serveur distant installé](#BKMK_3)

-   [Pour seulement le domaine contrôleurs et lorsque vous êtes sur un contrôleur de domaine ou sur une station de travail qui a le Remote Server Administration Tools Pack installé](#BKMK_4)

#### <a name="BKMK_1"></a>Pour activer les règles de certificat pour votre ordinateur local

1.  Ouvrir les paramètres de sécurité locale.

2.  Dans l’arborescence de la console, cliquez sur **Options de sécurité** situé sous sécurité paramètres/Stratégies locales.

3.  Dans le volet d’informations, double-cliquez sur **paramètres système: utiliser les règles de certificat avec les exécutables Windows pour les stratégies de Restriction logicielle**.

4.  Effectuez l’une des opérations suivantes, puis cliquez sur **OK**:

    -   Pour activer les règles de certificat, cliquez sur **activé**.

    -   Pour désactiver les règles de certificat, cliquez sur **désactivé**.

#### <a name="BKMK_2"></a>Pour activer les règles de certificat pour un objet de stratégie de groupe, vous vous trouvez sur un serveur qui est joint à un domaine

1.  Ouvrez Microsoft Management Console (MMC).

2.  Sur le **fichier** menu, cliquez sur **ajouter/supprimer un composant logiciel enfichable**, puis cliquez sur **ajouter**.

3.  Cliquez sur **Éditeur d’objets de stratégie de groupe Local**, puis cliquez sur **ajouter**.

4.  Dans **sélectionner un objet de stratégie de groupe**, cliquez sur **Parcourir**.

5.  Dans **rechercher un objet de stratégie de groupe**, sélectionnez un objet de stratégie de groupe (GPO) dans le domaine approprié, le site ou l’unité d’organisation- ou créer un nouveau, puis cliquez sur **Terminer**.

6.  Cliquez sur **fermer**, puis cliquez sur **OK**.

7.  Dans l’arborescence de la console, cliquez sur **Options de sécurité** situé sous *GroupPolicyObject* [*Nom_Ordinateur*] Policy/Computer Configuration/Windows Settings/Security paramètres/Stratégies locales /.

8.  Dans le volet d’informations, double-cliquez sur **paramètres système: utiliser les règles de certificat avec les exécutables Windows pour les stratégies de Restriction logicielle**.

9. Si ce paramètre de stratégie n’a pas encore été défini, sélectionnez le **définir ces paramètres de stratégie** case à cocher.

10. Effectuez l’une des opérations suivantes, puis cliquez sur **OK**:

    -   Pour activer les règles de certificat, cliquez sur **activé**.

    -   Pour désactiver les règles de certificat, cliquez sur **désactivé**.

#### <a name="BKMK_3"></a>Pour activer le certificat règles pour un objet de stratégie de groupe, et que vous êtes sur un contrôleur de domaine ou sur une station de travail qui a les outils d’Administration de serveur distant installé

1.  Ouvrez Utilisateurs et ordinateurs ActiveDirectory.

2.  Dans l’arborescence de la console, cliquez sur l’objet de stratégie de groupe (GPO) pour lequel vous souhaitez activer les règles de certificat.

3.  Cliquez sur **propriétés**, puis cliquez sur le **stratégie de groupe** onglet.

4.  Cliquez sur **modifier** pour ouvrir l’objet de stratégie de groupe que vous souhaitez modifier. Vous pouvez également cliquer sur **New** pour créer un nouvel objet GPO, puis cliquez sur **modifier**.

5.  Dans l’arborescence de la console, cliquez sur **Options de sécurité** situé sous *GroupPolicyObject*[*Nom_Ordinateur*] Policy/Computer Configuration/Windows Settings/Security paramètres/Stratégies locales.

6.  Dans le volet d’informations, double-cliquez sur **paramètres système: utiliser les règles de certificat avec les exécutables Windows pour les stratégies de Restriction logicielle**.

7.  Si ce paramètre de stratégie n’a pas encore été défini, sélectionnez le **définir ces paramètres de stratégie** case à cocher.

8.  Effectuez l’une des opérations suivantes, puis cliquez sur **OK**:

    -   Pour activer les règles de certificat, cliquez sur **activé**.

    -   Pour désactiver les règles de certificat, cliquez sur **désactivé**.

#### <a name="BKMK_4"></a>Pour activer les règles de certificat uniquement pour les contrôleurs de domaine et vous sont sur un contrôleur de domaine ou sur une station de travail qui a les outils d’Administration de serveur distant installé

1.  Ouvrez les paramètres de sécurité de contrôleur de domaine.

2.  Dans l’arborescence de la console, cliquez sur **Options de sécurité** situé sous *GroupPolicyObject* [*Nom_Ordinateur*] Policy/Computer Configuration/Windows Settings/Security paramètres/Stratégies locales.

3.  Dans le volet d’informations, double-cliquez sur **paramètres système: utiliser les règles de certificat avec les exécutables Windows pour les stratégies de Restriction logicielle**.

4.  Si ce paramètre de stratégie n’a pas encore été défini, sélectionnez le **définir ces paramètres de stratégie** case à cocher.

5.  Effectuez l’une des opérations suivantes, puis cliquez sur **OK**:

    -   Pour activer les règles de certificat, cliquez sur **activé**.

    -   Pour désactiver les règles de certificat, cliquez sur **désactivé**.

> [!NOTE]
> Vous devez effectuer cette procédure avant que les règles de certificat puissent prendre effet.

### <a name="set-trusted-publisher-options"></a>Définir les options des éditeurs approuvés
La signature de logiciel est utilisée par un nombre croissant d’éditeurs de logiciels et les développeurs d’applications pour vérifier que leurs applications proviennent d’une source approuvée. Toutefois, de nombreux utilisateurs comprendre ni ne font pas vraiment attention aux signature des certificats associés aux applications qu’ils installent.

Les paramètres de stratégie dans le **éditeurs approuvés** onglet de la stratégie de validation de chemin d’accès de certificat permet aux administrateurs de contrôler les certificats qui peuvent être acceptés comme provenant d’un éditeur approuvé.

##### <a name="to-configure-the-trusted-publishers-policy-settings-for-a-local-computer"></a>Pour configurer les paramètres de stratégie des éditeurs approuvés pour un ordinateur local

1.  Sur le **Démarrer** , tapez**gpedit.msc** et appuyez sur ENTRÉE.

2.  Dans l’arborescence de la console sous **Local ordinateur local\Configuration Ordinateur\paramètres Windows\Paramètres**, cliquez sur **stratégies de clé publique**.

3.  Double-cliquez sur **paramètres de Validation du chemin d’accès de certificat**, puis cliquez sur le **éditeurs approuvés** onglet.

4.  Sélectionnez le **définir ces paramètres de stratégie** case à cocher, sélectionnez les paramètres de stratégie que vous souhaitez appliquer, puis cliquez sur **OK** pour appliquer les nouveaux paramètres.

##### <a name="to-configure-the-trusted-publishers-policy-settings-for-a-domain"></a>Pour configurer les paramètres de stratégie des éditeurs approuvés pour un domaine

1.  Ouvrez **gestion des stratégies de groupe**.

2.  Dans l’arborescence de la console, double-cliquez sur **objets de stratégie de groupe** dans la forêt et le domaine contenant le **stratégie de domaine par défaut** objet de stratégie de groupe (GPO) que vous souhaitez modifier.

3.  Avec le bouton droit le **stratégie de domaine par défaut** objet stratégie de groupe, puis cliquez sur **modifier**.

4.  Dans l’arborescence de la console sous **Configuration ordinateur\Paramètres Windows\paramètres**, cliquez sur **stratégies de clé publique**.

5.  Double-cliquez sur **paramètres de Validation du chemin d’accès de certificat**, puis cliquez sur le **éditeurs approuvés** onglet.

6.  Sélectionnez le **définir ces paramètres de stratégie** case à cocher, sélectionnez les paramètres de stratégie que vous souhaitez appliquer, puis cliquez sur **OK** pour appliquer les nouveaux paramètres.

##### <a name="to-allow-only-administrators-to-manage-certificates-used-for-code-signing-for-a-local-computer"></a>Pour autoriser uniquement les administrateurs à gérer les certificats utilisés pour la signature du code pour un ordinateur local

1.  Sur le **Démarrer** écran, type, **gpedit.msc** dans les **rechercher les programmes et fichiers** ou dans Windows 8, sur le bureau et appuyez sur ENTRÉE.

2.  Dans l’arborescence de la console sous **stratégie de domaine par défaut** ou **stratégie ordinateur Local**, double-cliquez sur **Configuration ordinateur**, **paramètres Windows**, et **paramètres de sécurité**, puis cliquez sur **stratégies de clé publique**.

3.  Double-cliquez sur **paramètres de Validation du chemin d’accès de certificat**, puis cliquez sur le **éditeurs approuvés** onglet.

4.  Sélectionnez le **définir ces paramètres de stratégie** case à cocher.

5.  Sous **approuvé la gestion des éditeurs**, cliquez sur **autoriser seulement tous les administrateurs à gérer des éditeurs approuvés**, puis cliquez sur **OK** pour appliquer les nouveaux paramètres.

##### <a name="to-allow-only-administrators-to-manage-certificates-used-for-code-signing-for-a-domain"></a>Pour autoriser uniquement les administrateurs à gérer les certificats utilisés pour la signature du code pour un domaine

1.  Ouvrez **gestion des stratégies de groupe**.

2.  Dans l’arborescence de la console, double-cliquez sur **objets de stratégie de groupe** dans la forêt et le domaine contenant le **stratégie de domaine par défaut** objet de stratégie de groupe que vous souhaitez modifier.

3.  Avec le bouton droit le **stratégie de domaine par défaut** objet stratégie de groupe, puis cliquez sur **modifier**.

4.  Dans l’arborescence de la console sous **Configuration ordinateur\Paramètres Windows\paramètres**, cliquez sur **stratégies de clé publique**.

5.  Double-cliquez sur **paramètres de Validation du chemin d’accès de certificat**, puis cliquez sur le **éditeurs approuvés** onglet.

6.  Sélectionnez le **définir ces paramètres de stratégie** case à cocher, effectuez les modifications de votre choix, puis cliquez sur **OK** pour appliquer les nouveaux paramètres.

## <a name="BKMK_Hash_Rules"></a>Utilisation des règles de hachage
Un hachage est une série d’octets de longueur fixe qui identifie de façon unique un programme logiciel ou un fichier. Le hachage est calculé par un algorithme de hachage. Lorsqu’une règle de hachage est créée pour un programme logiciel, les stratégies de restriction logicielle calculent un hachage du programme. Lorsqu’un utilisateur tente d’ouvrir un programme logiciel, un hachage du programme est comparé aux règles de hachage existantes pour les stratégies de restriction logicielle. Le hachage d’un programme logiciel est toujours le même, quel que soit l’emplacement du programme sur l’ordinateur. Toutefois, si un programme logiciel est modifié, son hachage change, et il ne correspond plus au hachage de la règle de hachage pour les stratégies de restriction logicielle.

Par exemple, vous pouvez créer une règle de hachage et définir le niveau de la sécurité **non autorisé** pour empêcher les utilisateurs à partir d’un fichier spécifique en cours d’exécution. Un fichier peut être renommé ou déplacé vers un autre dossier, cela dans le même hachage. Toutefois, toutes les modifications au fichier lui-même modifier sa valeur de hachage également et autoriser le fichier de contourner les restrictions.

#### <a name="to-create-a-hash-rule"></a>Pour créer une règle de hachage

1.  Ouvrez stratégies de Restriction logicielle.

2.  Dans l’arborescence de la console ou le volet d’informations, cliquez sur **des règles supplémentaires**, puis cliquez sur **nouvelle règle de hachage**.

3.  Cliquez sur **Parcourir** pour rechercher un fichier.

    > [!NOTE]
    > Dans Windows XP, il est possible de collez un hachage calculé au préalable dans **hachage du fichier**. Dans Windows Server 2008 R2, Windows 7 et versions ultérieures, cette option n’est pas disponible.

4.  Dans **niveau de sécurité**, cliquez sur **non autorisé** ou **non restreint**.

5.  Dans **Description**, tapez une description pour cette règle, puis cliquez sur **OK**.

> [!NOTE]
> -   Il peut être nécessaire de créer un nouveau paramètre de stratégie de restriction logicielle pour l’objet de stratégie de groupe (GPO) si vous ne le n'avez pas déjà fait.
> -   Une règle de hachage peut être créée pour un virus ou un cheval de Troie afin d’empêcher en cours d’exécution.
> -   Si vous souhaitez que d’autres personnes d’utiliser une règle de hachage afin qu’un virus ne peut pas s’exécuter, calculez le hachage du virus à l’aide de stratégies de restriction logicielle et de messagerie électronique, la valeur de hachage pour les autres personnes. Messagerie jamais le virus lui-même.
> -   Si un virus a été envoyé par courrier électronique, vous pouvez également créer une règle de chemin d’accès pour empêcher l’exécution des pièces jointes.
> -   Un fichier qui a été renommé ou déplacé vers un autre dossier aboutit au même hachage. Toute modification apportée au fichier lui-même entraîne un hachage différent.
> -   Les seuls types de fichiers qui sont concernés par les règles de hachage sont ceux qui sont répertoriés dans **Types de fichiers désignés** dans le volet d’informations des stratégies de Restriction logicielle. Il existe une liste de types de fichiers désignés est partagée par toutes les règles.
> -   Stratégies de restriction logicielle prenne effet, les utilisateurs doivent mettre à jour les paramètres de stratégie en déconnectant et ouvert une session sur leurs ordinateurs.
> -   Lorsque plusieurs règles de stratégies de restriction logicielle est appliqué aux paramètres de stratégie, il existe un niveau de priorité des règles pour la gestion des conflits.

## <a name="BKMK_Internet_Zone_Rules"></a>Utilisation des règles de Zone Internet
Règles de zone Internet s’appliquent uniquement aux packages Windows Installer. Une règle de zone peut identifier un logiciel à partir d’une zone qui est spécifiée par le biais d’Internet Explorer. Ces zones sont Internet, intranet Local, sites sensibles, sites de confiance et poste de travail. Une règle de Zone Internet est conçue pour empêcher les utilisateurs de télécharger et installer des logiciels.

#### <a name="to-create-an-internet-zone-rule"></a>Pour créer une règle de zone Internet

1.  Ouvrez stratégies de Restriction logicielle.

2.  Dans l’arborescence de la console ou le volet d’informations, cliquez sur **des règles supplémentaires**, puis cliquez sur **nouvelle règle de Zone Internet**.

3.  Dans **zone Internet**, cliquez sur une zone Internet.

4.  Dans **niveau de sécurité**, cliquez sur **non autorisé** ou **non restreint**, puis cliquez sur **OK**.

> [!NOTE]
> -   Il peut être nécessaire de créer un nouveau paramètre de stratégie de restriction logicielle pour l’objet de stratégie de groupe (GPO) si vous ne le n'avez pas déjà fait.
> -   Règles de zone s’appliquent uniquement aux fichiers avec un type de fichier .msi, qui sont des packages Windows Installer.
> -   Stratégies de restriction logicielle prenne effet, les utilisateurs doivent mettre à jour les paramètres de stratégie en déconnectant et ouvert une session sur leurs ordinateurs.
> -   Lorsque plusieurs règles de stratégies de restriction logicielle est appliqué aux paramètres de stratégie, il existe un niveau de priorité des règles pour la gestion des conflits.

## <a name="BKMK_Path_Rules"></a>Utilisation des règles de chemin d’accès
Une règle de chemin d’accès identifie un logiciel par son chemin d’accès du fichier. Par exemple, si vous disposez d’un ordinateur qui a un niveau de sécurité par défaut **non autorisé**, vous pouvez tout de même accorder un accès illimité à un dossier spécifique pour chaque utilisateur. Vous pouvez créer une règle de chemin d’accès en utilisant le chemin d’accès du fichier et en définissant le niveau de sécurité de la règle de chemin d’accès à **non restreint**. Certains chemins d’accès courants pour ce type de règle sont % USERPROFILE%, % windir%, % AppData%, % ProgramFiles% et % TEMP%. Vous pouvez également créer du Registre des règles de chemin d’accès qui utilisent la clé de Registre du logiciel en tant que son chemin d’accès.

Dans la mesure où ces règles sont spécifiées par le chemin d’accès, si un programme logiciel est déplacé, la règle de chemin d’accès ne s’applique plus.

#### <a name="to-create-a-path-rule"></a>Pour créer une règle de chemin d’accès

1.  Ouvrez stratégies de Restriction logicielle.

2.  Dans l’arborescence de la console ou le volet d’informations, cliquez sur **des règles supplémentaires**, puis cliquez sur **nouvelle règle de chemin d’accès**.

3.  Dans **chemin d’accès**, tapez un chemin d’accès, ou cliquez sur **Parcourir** pour rechercher un fichier ou dossier.

4.  Dans **niveau de sécurité**, cliquez sur **non autorisé** ou **non restreint**.

5.  Dans **Description**, tapez une description pour cette règle, puis cliquez sur **OK**.

> [!CAUTION]
> -   Sur certains dossiers, tels que le dossier Windows, définition du niveau de sécurité pour **non autorisé** peut nuire à l’opération de votre système d’exploitation. Veillez à ne pas rejeter un composant vital du système d’exploitation ou l’une de ses programmes.

> [!NOTE]
> -   Il peut être nécessaire de créer des stratégies de restriction logicielle pour l’objet de stratégie de groupe (GPO) si vous ne le n'avez pas déjà fait.
> -   Si vous créez une règle de chemin d’accès pour les logiciels avec une sécurité au niveau de **non autorisé**, les utilisateurs peuvent toujours exécuter le logiciel en les copiant dans un autre emplacement.
> -   Les caractères génériques sont pris en charge par la règle de chemin d’accès sont * et?.
> -   Vous pouvez utiliser des variables d’environnement, tels que %ProgramFiles% ou % SystemRoot%, dans la règle de chemin d’accès.
> -   Si vous souhaitez créer une règle de chemin d’accès pour les logiciels lorsque vous ne savez pas où il est stocké sur un ordinateur, mais que vous avez sa clé de Registre, vous pouvez créer une règle de chemin d’accès du Registre.
> -   Pour empêcher les utilisateurs de l’exécution des pièces jointes, vous pouvez créer une règle de chemin d’accès de répertoire de pièces jointes de votre programme de messagerie qui bloque l’exécution des pièces jointes.
> -   Les seuls types de fichiers qui sont concernés par les règles de chemin d’accès sont ceux qui sont répertoriés dans **Types de fichiers désignés** dans le volet d’informations des stratégies de Restriction logicielle. Il existe une liste de types de fichiers désignés est partagée par toutes les règles.
> -   Stratégies de restriction logicielle prenne effet, les utilisateurs doivent mettre à jour les paramètres de stratégie en déconnectant et ouvert une session sur leurs ordinateurs.
> -   Lorsque plusieurs règles de stratégies de restriction logicielle est appliqué aux paramètres de stratégie, il existe un niveau de priorité des règles pour la gestion des conflits.

#### <a name="to-create-a-registry-path-rule"></a>Pour créer une règle de chemin d’accès du Registre

1.  Sur le **Démarrer** , tapez regedit.

2.  Dans l’arborescence de la console, cliquez sur la clé de Registre que vous souhaitez créer une règle, puis cliquez sur **copier le nom de clé**. Notez le nom de la valeur dans le volet d’informations.

3.  Ouvrez stratégies de Restriction logicielle.

4.  Dans l’arborescence de la console ou le volet d’informations, cliquez sur **des règles supplémentaires**, puis cliquez sur **nouvelle règle de chemin d’accès**.

5.  Dans **chemin d’accès**, collez le nom de clé de Registre, suivi du nom de valeur.

6.  Placez le chemin d’accès du Registre de pourcentage (%), par exemple, % HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PlatformSDK\Directories\InstallDir%.

7.  Dans **niveau de sécurité**, cliquez sur **non autorisé** ou **non restreint**.

8.  Dans **Description**, tapez une description pour cette règle, puis cliquez sur **OK**.


