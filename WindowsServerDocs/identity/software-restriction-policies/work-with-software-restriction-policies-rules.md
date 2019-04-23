---
title: Utiliser les règles des stratégies de restriction logicielle
description: Sécurité de Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844950"
---
# <a name="work-with-software-restriction-policies-rules"></a>Utiliser les règles des stratégies de restriction logicielle

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit les procédures d’utilisation de certificat, chemin d’accès, internet les règles de zone et de hachage à l’aide de stratégies de Restriction logicielle.

## <a name="introduction"></a>Introduction
Avec les stratégies de restriction logicielle, vous pouvez protéger votre environnement informatique contre les logiciels non approuvés en identifiant et en spécifiant les logiciels autorisés à exécuter. Vous pouvez définir un niveau de sécurité par défaut **Unrestricted** ou **rejeté** pour un objet de stratégie de groupe (GPO) afin que le logiciel est autorisée ou non autorisée à s’exécuter par défaut. Vous pouvez créer des exceptions à ce niveau de sécurité par défaut en créant des règles de stratégies d’un logiciel spécifique de restriction logicielle. Par exemple, si le niveau de sécurité par défaut a la valeur **Non autorisé**, vous pouvez créer des règles pour autoriser l’exécution de logiciels spécifiques. Les types de règles sont les suivantes :

-   **Règles de certificat**

    Pour obtenir des procédures, voir [Utilisation des règles de certificat](#BKMK_Cert_Rules).

-   **Règles de hachage**

    Pour obtenir des procédures, voir [Utilisation des règles de hachage](#BKMK_Hash_Rules).

-   **Règles de zone Internet**

    Pour connaître les procédures, consultez [utilisation des règles de Zone Internet](#BKMK_Internet_Zone_Rules).

-   **Règles de chemin d’accès**

    Pour obtenir des procédures, voir [Utilisation des règles de chemins d’accès](#BKMK_Path_Rules).

Pour plus d’informations sur les autres tâches pour gérer les stratégies de Restriction logicielle, consultez [administrer les stratégies de Restriction logicielle](administer-software-restriction-policies.md).

## <a name="BKMK_Cert_Rules"></a>Utilisation des règles de certificat
Stratégies de restriction logicielle peuvent également identifier un logiciel par son certificat de signature. Vous pouvez créer une règle de certificat qui identifie le logiciel et autorise ou refuse l’exécution du logiciel, en fonction du niveau de sécurité. Par exemple, vous pouvez utiliser les règles de certificat pour approuver automatiquement les logiciels d’une source approuvée dans un domaine sans intervention de l’utilisateur. Vous pouvez également utiliser des règles de certificat pour exécuter des fichiers dans des zones non autorisées de votre système d’exploitation. Les règles de certificat ne sont pas activées par défaut.

Lorsque les règles sont créées pour le domaine à l’aide de la stratégie de groupe, vous devez disposer des autorisations pour créer ou modifier un objet de stratégie de groupe. Si vous créez des règles pour l’ordinateur local, vous devez disposer d’informations d’authentification d’administration sur cet ordinateur.

#### <a name="to-create-a-certificate-rule"></a>Pour créer une règle de certificat

1.  Ouvrez Stratégies de restriction logicielle.

2.  Dans l’arborescence de la console ou dans le volet d’informations, cliquez sur **des règles supplémentaires**, puis cliquez sur **nouvelle règle de certificat**.

3.  Cliquez sur **Parcourir**, et sélectionnez un certificat ou un fichier signé.

4.  Dans **au niveau de sécurité**, cliquez sur **rejeté** ou **Unrestricted**.

5.  Dans **Description**, tapez une description pour cette règle, puis cliquez sur **OK**.

> [!NOTE]
> -   Il peut être nécessaire de créer un paramètre de stratégie de restriction logicielle pour l’objet de stratégie de groupe si cela n’est pas encore fait.
> -   Les règles de certificat ne sont pas activées par défaut.
> -   Les seuls types de fichiers concernés par les règles de certificat sont ceux qui sont répertoriés dans **Types de fichiers désignés** dans le volet d’informations des stratégies de restriction logicielle. Une liste de types de fichiers désignés est partagée par toutes les règles.
> -   Stratégies de restriction logicielle entrent en vigueur, les utilisateurs doivent mettre à jour les paramètres de stratégie en fermant, puis ouvrir une session sur leurs ordinateurs.
> -   Lorsque plusieurs règles de stratégies de restriction logicielle est appliquée aux paramètres de stratégie, il existe une priorité des règles de gestion des conflits.

### <a name="enabling-certificate-rules"></a>Activation des règles de certificat
En fonction de votre environnement, différentes procédures existent pour activer les règles de certificat :

-   [Pour votre ordinateur local](#BKMK_1)

-   [Pour un objet de stratégie de groupe et que vous êtes sur un serveur qui est joint à un domaine](#BKMK_2)

-   [Pour un objet de stratégie de groupe et que vous êtes sur un contrôleur de domaine ou une station de travail on a les outils d’Administration de serveur distant installé](#BKMK_3)

-   [Pour que domaine contrôleurs, vous êtes sur un contrôleur de domaine ou sur une station de travail qui a le Pack d’outils d’Administration à distance serveur](#BKMK_4)

#### <a name="BKMK_1"></a>Pour activer les règles de certificat pour votre ordinateur local

1.  Ouvrez Paramètres de sécurité locaux.

2.  Dans l’arborescence de la console, cliquez sur **Options de sécurité** situé sous sécurité paramètres/Stratégies locales.

3.  Dans le volet détails, double-cliquez sur **paramètres système : Utiliser des règles de certificat avec les exécutables Windows pour les stratégies de Restriction logicielle**.

4.  Effectuez les actions suivantes, puis cliquez sur **OK** :

    -   Pour activer les règles de certificat, cliquez sur **Activé**.

    -   Pour désactiver les règles de certificat, cliquez sur **Désactivé**.

#### <a name="BKMK_2"></a>Pour activer les règles de certificat pour un objet de stratégie de groupe, vous vous trouvez sur un serveur qui est joint à un domaine

1.  Ouvrez la console MMC.

2.  Dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**, puis sur **Ajouter**.

3.  Cliquez sur **Éditeur d’objet de stratégie de groupe locale.**, puis sur **Ajouter**.

4.  Dans **Sélectionner un objet de stratégie de groupe**, cliquez sur **Parcourir**.

5.  Dans **rechercher un objet de stratégie de groupe**, sélectionnez un objet de stratégie de groupe (GPO) dans le domaine approprié, un site ou une unité d’organisation- ou créez-en un, puis cliquez sur **Terminer**.

6.  Cliquez sur **Fermer**, puis sur **OK**.

7.  Dans l’arborescence de la console, cliquez sur **Options de sécurité** situé sous *GroupPolicyObject* [*Nom_Ordinateur*] / ordinateur stratégie paramètres de Configuration/Windows/sécurité Paramètres/Stratégies locales /.

8.  Dans le volet détails, double-cliquez sur **paramètres système : Utiliser des règles de certificat avec les exécutables Windows pour les stratégies de Restriction logicielle**.

9. Si ce paramètre de stratégie n’a pas encore été défini, activez la case à cocher **Définir ces paramètres de stratégie**.

10. Effectuez les actions suivantes, puis cliquez sur **OK** :

    -   Pour activer les règles de certificat, cliquez sur **Activé**.

    -   Pour désactiver les règles de certificat, cliquez sur **Désactivé**.

#### <a name="BKMK_3"></a>Pour activer le certificat d’un objet de stratégie de groupe, et vous êtes sur un contrôleur de domaine ou sur une station de travail qui a les outils d’Administration de serveur distant installé

1.  Ouvrez le composant Utilisateurs et ordinateurs Active Directory.

2.  Dans l’arborescence de la console, cliquez avec le bouton droit sur l’objet de stratégie de groupe pour lequel vous voulez activer les règles de certificat.

3.  Cliquez sur **Propriétés**, puis sur l’onglet **Stratégie de groupe**.

4.  Cliquez sur **Modifier** pour ouvrir l’objet de stratégie de groupe à modifier. Vous pouvez également cliquer sur **Nouveau** pour créer un objet de stratégie de groupe, puis cliquez sur **Modifier**.

5.  Dans l’arborescence de la console, cliquez sur **Options de sécurité** situé sous *GroupPolicyObject*[*Nom_Ordinateur*] / ordinateur stratégie paramètres de Configuration/Windows/sécurité Paramètres/Stratégies locales.

6.  Dans le volet détails, double-cliquez sur **paramètres système : Utiliser des règles de certificat avec les exécutables Windows pour les stratégies de Restriction logicielle**.

7.  Si ce paramètre de stratégie n’a pas encore été défini, activez la case à cocher **Définir ces paramètres de stratégie**.

8.  Effectuez les actions suivantes, puis cliquez sur **OK** :

    -   Pour activer les règles de certificat, cliquez sur **Activé**.

    -   Pour désactiver les règles de certificat, cliquez sur **Désactivé**.

#### <a name="BKMK_4"></a>Pour activer les règles de certificat uniquement pour les contrôleurs de domaine et vous sont sur un contrôleur de domaine ou sur une station de travail qui a les outils d’Administration de serveur distant installé

1.  Ouvrez Paramètres de sécurité du contrôleur de domaine.

2.  Dans l’arborescence de la console, cliquez sur **Options de sécurité** sous Stratégie *[NomOrdinateur]**GroupPolicyObject*/Configuration ordinateur/Paramètres Windows/Paramètres de sécurité/Stratégies locales.

3.  Dans le volet détails, double-cliquez sur **paramètres système : Utiliser des règles de certificat avec les exécutables Windows pour les stratégies de Restriction logicielle**.

4.  Si ce paramètre de stratégie n’a pas encore été défini, activez la case à cocher **Définir ces paramètres de stratégie**.

5.  Effectuez les actions suivantes, puis cliquez sur **OK** :

    -   Pour activer les règles de certificat, cliquez sur **Activé**.

    -   Pour désactiver les règles de certificat, cliquez sur **Désactivé**.

> [!NOTE]
> Vous devez effectuer cette procédure avant la mise en œuvre des règles de certificat.

### <a name="set-trusted-publisher-options"></a>Définir les options d’éditeur approuvé
La signature de logiciel est utilisée par un nombre croissant d’éditeurs de logiciels et de développeurs d’applications pour vérifier que leurs applications proviennent d’une source de confiance. Cependant, beaucoup d’utilisateurs ne comprennent pas ou ne font pas vraiment attention à la signature des certificats associés aux applications qu’ils installent.

Les paramètres de stratégie sous l’onglet **Éditeurs approuvés** de la stratégie de validation du chemin d’accès du certificat permettent aux administrateurs de contrôler les certificats qui peuvent être acceptés comme provenant d’un éditeur approuvé.

##### <a name="to-configure-the-trusted-publishers-policy-settings-for-a-local-computer"></a>Pour configurer les paramètres de stratégie des éditeurs approuvés pour un ordinateur local

1.  Sur le **Démarrer** , tapez**gpedit.msc** puis appuyez sur ENTRÉE.

2.  Dans l’arborescence de la console, sous **Stratégie de l’ordinateur local\Configuration ordinateur\Paramètres Windows\Paramètres de sécurité**, cliquez sur **Stratégies de clé publique**.

3.  Double-cliquez sur **Paramètres de validation du chemin d’accès de certificat**, puis cliquez sur l’onglet **Éditeurs approuvés**.

4.  Activez la case à cocher **Définir ces paramètres de stratégie**, sélectionnez les paramètres de stratégie à appliquer, puis cliquez sur **OK** pour appliquer les nouveaux paramètres.

##### <a name="to-configure-the-trusted-publishers-policy-settings-for-a-domain"></a>Pour configurer les paramètres de stratégie des éditeurs approuvés pour un domaine

1.  Ouvrez **gestion de stratégie de groupe**.

2.  Dans l’arborescence de la console, double-cliquez sur **les objets de stratégie de groupe** dans la forêt et le domaine contenant le **Default Domain Policy** objet de stratégie de groupe (GPO) que vous souhaitez modifier.

3.  Cliquez avec le bouton droit sur l’objet **Stratégie de domaine par défaut**, puis cliquez sur **Modifier**.

4.  Dans l’arborescence de la console, sous **Configuration ordinateur\Paramètres Windows\Paramètres de sécurité**, cliquez sur **Stratégies de clé publique**.

5.  Double-cliquez sur **Paramètres de validation du chemin d’accès de certificat**, puis cliquez sur l’onglet **Éditeurs approuvés**.

6.  Activez la case à cocher **Définir ces paramètres de stratégie**, sélectionnez les paramètres de stratégie à appliquer, puis cliquez sur **OK** pour appliquer les nouveaux paramètres.

##### <a name="to-allow-only-administrators-to-manage-certificates-used-for-code-signing-for-a-local-computer"></a>Pour n’autoriser que les administrateurs à gérer des certificats utilisés pour la signature du code pour un ordinateur local

1.  Sur le **Démarrer** écran, tapez, **gpedit.msc** dans le **rechercher les programmes et fichiers** ou dans Windows 8, sur le bureau et appuyez sur ENTRÉE.

2.  Dans l’arborescence de la console sous **Default Domain Policy** ou **stratégie ordinateur Local**, double-cliquez sur **Configuration ordinateur**, **lesparamètresWindows**, et **paramètres de sécurité**, puis cliquez sur **stratégies de clé publique**.

3.  Double-cliquez sur **Paramètres de validation du chemin d’accès de certificat**, puis cliquez sur l’onglet **Éditeurs approuvés**.

4.  Activez la case à cocher **Définir ces paramètres de stratégie**.

5.  Sous **Gestion des éditeurs approuvés**, cliquez sur **Autoriser seulement tous les administrateurs à gérer les éditeurs approuvés**, puis cliquez sur **OK** pour appliquer les nouveaux paramètres.

##### <a name="to-allow-only-administrators-to-manage-certificates-used-for-code-signing-for-a-domain"></a>Pour n’autoriser que les administrateurs à gérer des certificats utilisés pour la signature du code pour un domaine

1.  Ouvrez **gestion de stratégie de groupe**.

2.  Dans l’arborescence de la console, double-cliquez sur **les objets de stratégie de groupe** dans la forêt et le domaine contenant le **Default Domain Policy** GPO que vous souhaitez modifier.

3.  Cliquez avec le bouton droit sur l’objet **Stratégie de domaine par défaut**, puis cliquez sur **Modifier**.

4.  Dans l’arborescence de la console, sous **Configuration ordinateur\Paramètres Windows\Paramètres de sécurité**, cliquez sur **Stratégies de clé publique**.

5.  Double-cliquez sur **Paramètres de validation du chemin d’accès de certificat**, puis cliquez sur l’onglet **Éditeurs approuvés**.

6.  Activez la case à cocher **Définir ces paramètres de stratégie**, effectuez les modifications de votre choix, puis cliquez sur **OK** pour appliquer les nouveaux paramètres.

## <a name="BKMK_Hash_Rules"></a>Utilisation des règles de hachage
Un hachage est une série d’octets de longueur fixe qui identifie de façon unique un programme logiciel ou un fichier. Le hachage est calculé par un algorithme de hachage. Quand une règle de hachage est créée pour un programme logiciel, les stratégies de restriction logicielle calculent un hachage du programme. Quand un utilisateur essaie d’ouvrir un programme logiciel, un hachage du programme est comparé aux règles de hachage existantes pour les stratégies de restriction logicielle. Le hachage d’un programme logiciel est toujours le même, quel que soit l’emplacement du programme sur l’ordinateur. Cependant, si un programme logiciel est modifié, son hachage change, et il ne correspond plus au hachage de la règle de hachage pour les stratégies de restriction logicielle.

Par exemple, vous pouvez créer une règle de hachage et définir le niveau de sécurité à **Non autorisé** pour empêcher l’exécution d’un fichier spécifique par les utilisateurs. Un fichier peut être renommé ou déplacé dans un autre dossier, cela ne change pas le hachage. Cependant, les modifications apportées au fichier lui-même changent la valeur de hachage du fichier et ce qui peut permettre au fichier de contourner les restrictions.

#### <a name="to-create-a-hash-rule"></a>Pour créer une règle de hachage

1.  Ouvrez Stratégies de restriction logicielle.

2.  Dans l’arborescence de la console ou dans le volet d’informations, cliquez sur **des règles supplémentaires**, puis cliquez sur **nouvelle règle de hachage**.

3.  Cliquez sur **Parcourir** pour rechercher un fichier.

    > [!NOTE]
    > Dans Windows XP, il est possible de coller un hachage précalculée dans **hachage du fichier**. Dans Windows Server 2008 R2, Windows 7 et versions ultérieures, cette option n’est pas disponible.

4.  Dans **au niveau de sécurité**, cliquez sur **rejeté** ou **Unrestricted**.

5.  Dans **Description**, tapez une description pour cette règle, puis cliquez sur **OK**.

> [!NOTE]
> -   Il peut être nécessaire de créer un paramètre de stratégie de restriction logicielle pour l’objet de stratégie de groupe si cela n’est pas encore fait.
> -   Une règle de hachage peut être créée pour un virus ou un cheval de Troie afin d’empêcher son exécution.
> -   Si vous souhaitez que d’autres personnes utilisent une règle de hachage afin qu’un virus ne peut pas exécuter, calculer le hachage du virus à l’aide de stratégies de restriction logicielle et de messagerie électronique, la valeur de hachage pour les autres personnes. Messagerie jamais le virus lui-même.
> -   Si un virus a été envoyé par courrier électronique, vous pouvez également créer une règle de chemin d’accès pour empêcher l’exécution de pièces jointes.
> -   Un fichier peut être renommé ou déplacé dans un autre dossier, cela ne change pas le hachage. Toute modification au fichier lui-même entraîne un hachage différent.
> -   Les seuls types de fichiers concernés par les règles de hachage sont ceux qui sont répertoriés dans **Types de fichiers désignés** dans le volet d’informations des stratégies de restriction logicielle. Une liste de types de fichiers désignés est partagée par toutes les règles.
> -   Stratégies de restriction logicielle entrent en vigueur, les utilisateurs doivent mettre à jour les paramètres de stratégie en fermant, puis ouvrir une session sur leurs ordinateurs.
> -   Lorsque plusieurs règles de stratégies de restriction logicielle est appliquée aux paramètres de stratégie, il existe une priorité des règles de gestion des conflits.

## <a name="BKMK_Internet_Zone_Rules"></a>Utilisation des règles de Zone Internet
Les règles de zone Internet s’appliquent uniquement aux packages Windows Installer. Une règle de zone peut identifier un logiciel d’une zone spécifiée dans Internet Explorer. Ces zones sont Internet, Intranet local, Sites sensibles, Sites de confiance et Mon ordinateur. Une règle de Zone Internet est conçue pour empêcher les utilisateurs de télécharger et installer le logiciel.

#### <a name="to-create-an-internet-zone-rule"></a>Pour créer une règle de zone Internet

1.  Ouvrez Stratégies de restriction logicielle.

2.  Dans l’arborescence de la console ou dans le volet d’informations, cliquez sur **des règles supplémentaires**, puis cliquez sur **nouvelle règle de Zone Internet**.

3.  Dans **Zone Internet**, cliquez sur une zone Internet.

4.  Dans **au niveau de sécurité**, cliquez sur **rejeté** ou **Unrestricted**, puis cliquez sur **OK**.

> [!NOTE]
> -   Il peut être nécessaire de créer un paramètre de stratégie de restriction logicielle pour l’objet de stratégie de groupe si cela n’est pas encore fait.
> -   Les règles de zone s’appliquent uniquement aux fichiers de type .msi, qui sont des packages Windows Installer.
> -   Stratégies de restriction logicielle entrent en vigueur, les utilisateurs doivent mettre à jour les paramètres de stratégie en fermant, puis ouvrir une session sur leurs ordinateurs.
> -   Lorsque plusieurs règles de stratégies de restriction logicielle est appliquée aux paramètres de stratégie, il existe une priorité des règles de gestion des conflits.

## <a name="BKMK_Path_Rules"></a>Utilisation des règles de chemin d’accès
Une règle de chemin d’accès identifie un logiciel par le chemin du fichier. Par exemple, si le niveau de sécurité par défaut de votre ordinateur est **Non autorisé**, vous pouvez tout de même accorder un accès illimité à un dossier spécifique pour chaque utilisateur. Vous pouvez créer une règle de chemin d’accès en utilisant le chemin du fichier et en définissant le niveau de sécurité de la règle de chemin d’accès à **Non restreint**. Certains chemins d’accès courants pour ce type de règle sont %userprofile%, %windir%, %appdata%, %programfiles%, et %temp%. Vous pouvez également créer des règles de chemin d’accès du registre qui utilisent la clé de registre du logiciel comme chemin.

Comme ces règles sont indiquées par leur chemin, si un programme logiciel est déplacé, la règle de chemin d’accès ne s’applique plus.

#### <a name="to-create-a-path-rule"></a>Pour créer une règle de chemin d’accès

1.  Ouvrez Stratégies de restriction logicielle.

2.  Dans l’arborescence de la console ou dans le volet d’informations, cliquez sur **des règles supplémentaires**, puis cliquez sur **nouvelle règle de chemin d’accès**.

3.  Dans **Chemin d’accès**, tapez un chemin ou cliquez sur **Parcourir** pour trouver un fichier ou un dossier.

4.  Dans **au niveau de sécurité**, cliquez sur **rejeté** ou **Unrestricted**.

5.  Dans **Description**, tapez une description pour cette règle, puis cliquez sur **OK**.

> [!CAUTION]
> -   Sur certains dossiers, tels que le dossier Windows, la définition de la sécurité au niveau **rejeté** peuvent affecter négativement le fonctionnement de votre système d’exploitation. Assurez-vous de ne pas refuser l’accès à un composant vital du système d’exploitation ou à l’un de ses programmes.

> [!NOTE]
> -   Il peut être nécessaire de créer des stratégies de restriction logicielle pour l’objet de stratégie de groupe si cela n’est pas encore fait.
> -   Si vous créez une règle de chemin d’accès avec un niveau de sécurité **Non autorisé**, les utilisateurs peuvent toujours exécuter le logiciel en le copiant à un autre emplacement.
> -   Les caractères génériques * et ? sont pris en charge par la règle de chemin d’accès.
> -   Vous pouvez utiliser les variables d’environnement, tels que %programfiles% ou %systemroot%, dans la règle de chemin d’accès.
> -   Si vous voulez créer une règle de chemin d’accès pour un logiciel quand vous ne savez pas où il est stocké sur un ordinateur, mais que vous connaissez sa clé de registre, vous pouvez créer une règle de chemin d’accès de registre.
> -   Pour empêcher les utilisateurs de l’exécution des pièces jointes de courrier électronique, vous pouvez créer une règle de chemin d’accès pour le répertoire de pièce jointe de votre programme de messagerie qui empêche les utilisateurs de pièces jointes en cours d’exécution.
> -   Les seuls types de fichiers concernés par les règles de chemin d’accès sont ceux qui sont répertoriés dans **Types de fichiers désignés** dans le volet d’informations des stratégies de restriction logicielle. Une liste de types de fichiers désignés est partagée par toutes les règles.
> -   Stratégies de restriction logicielle entrent en vigueur, les utilisateurs doivent mettre à jour les paramètres de stratégie en fermant, puis ouvrir une session sur leurs ordinateurs.
> -   Lorsque plusieurs règles de stratégies de restriction logicielle est appliquée aux paramètres de stratégie, il existe une priorité des règles de gestion des conflits.

#### <a name="to-create-a-registry-path-rule"></a>Pour créer une règle de chemin d’accès de registre

1.  Sur le **Démarrer** , tapez regedit.

2.  Dans l’arborescence de la console, cliquez avec le bouton droit sur la clé de registre pour laquelle vous voulez créer une règle, puis cliquez sur **Copier le nom de la clé**. Notez le nom de la valeur dans le volet d’informations.

3.  Ouvrez Stratégies de restriction logicielle.

4.  Dans l’arborescence de la console ou dans le volet d’informations, cliquez sur **des règles supplémentaires**, puis cliquez sur **nouvelle règle de chemin d’accès**.

5.  Dans **chemin d’accès**, collez le nom de clé de Registre, suivi du nom de valeur.

6.  Placez le chemin de Registre dans des signes de pourcentage (%), par exemple, % HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PlatformSDK\Directories\InstallDir%.

7.  Dans **au niveau de sécurité**, cliquez sur **rejeté** ou **Unrestricted**.

8.  Dans **Description**, tapez une description pour cette règle, puis cliquez sur **OK**.


