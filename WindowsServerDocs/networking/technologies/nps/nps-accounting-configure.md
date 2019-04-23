---
title: Configurer la gestion des comptes de serveur NPS (Network Policy Server)
description: Cette rubrique fournit des informations sur le fichier texte et de journalisation pour le serveur NPS dans Windows Server 2016 de SQL Server.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: dfde2e21-f3d5-41e8-8492-cb3f0d028afb
ms.author: pashort
author: shortpatti
ms.date: 05/25/2018
ms.openlocfilehash: c732a9f42d942ad579468d1dd15d30324d6fea87
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839700"
---
# <a name="configure-network-policy-server-accounting"></a>Configurer la gestion des comptes de serveur NPS (Network Policy Server)

Il existe trois types de journalisation pour le serveur NPS \(NPS\):

- **Journalisation des événements**. Principalement utilisée pour l’audit et résolution des problèmes de tentatives de connexion. Vous pouvez configurer la journalisation en obtenant les propriétés de serveur NPS dans la console NPS des événements NPS.

- **Enregistrement des demandes de comptabilité et de l’authentification des utilisateurs dans un fichier local**. Utilisé principalement pour les fins de facturation et d’analyse de connexion. Également utile comme outil d’enquête de sécurité, car il fournit une méthode de suivi de l’activité d’un utilisateur malveillant après une attaque. Vous pouvez configurer la journalisation de fichier local à l’aide de l’Assistant Configuration de comptabilité.

- **Journalisation de l’authentification des utilisateurs et les demandes de gestion dans une base de données Microsoft SQL Server compatible XML**. Utilisé pour permettre à plusieurs serveurs NPS pour avoir une source de données en cours d’exécution. Fournit également les avantages de l’utilisation d’une base de données relationnelle. Vous pouvez configurer la journalisation de SQL Server à l’aide de l’Assistant Configuration de comptabilité.

## <a name="use-the-accounting-configuration-wizard"></a>Utilisez l’Assistant Configuration de comptabilité

À l’aide de l’Assistant Configuration de la comptabilité, vous pouvez configurer les quatre paramètres de gestion des comptes suivants :

- **La journalisation SQL uniquement**. À l’aide de ce paramètre, vous pouvez configurer une liaison de données à un serveur SQL Server qui permet au serveur NPS pour se connecter à et envoyer des données de comptabilité à SQL server. En outre, l’Assistant peut configurer la base de données sur le serveur SQL Server pour vous assurer que la base de données est compatible avec la journalisation du serveur NPS SQL.
- **Journalisation texte uniquement**. À l’aide de ce paramètre, vous pouvez configurer NPS pour enregistrer les données de gestion dans un fichier texte.
- **Journalisation en parallèle**. À l’aide de ce paramètre, vous pouvez configurer la liaison de données SQL Server et la base de données. Vous pouvez également configurer la journalisation de fichiers texte afin que NPS enregistre simultanément dans le fichier texte et de la base de données SQL Server. 
- **La journalisation SQL avec sauvegarde**. À l’aide de ce paramètre, vous pouvez configurer la liaison de données SQL Server et la base de données. En outre, vous pouvez configurer la journalisation de fichiers texte que NPS utilise si la journalisation SQL Server échoue.

En plus de ces paramètres, la journalisation SQL Server et journalisation de type texte permettent vous permettent de spécifier si NPS continue à traiter les demandes de connexion si la journalisation échoue. Vous pouvez le spécifier dans le **journalisation section d’action échec** dans les propriétés de journalisation de fichiers local, dans les propriétés de journalisation du serveur SQL, et pendant l’exécution de l’Assistant Configuration de comptabilité.

### <a name="to-run-the-accounting-configuration-wizard"></a>Pour exécuter l’Assistant Configuration de comptabilité

Pour exécuter l’Assistant Configuration de comptabilité, procédez comme suit :

1. Ouvrez la console NPS ou le composant logiciel enfichable NPS Microsoft Management Console (MMC).
2. Dans l’arborescence de la console, cliquez sur **comptabilité**.
3. Dans le volet de détails, dans **comptabilité**, cliquez sur **configurer Comptabilité**.

## <a name="configure-nps-log-file-properties"></a>Configurer les propriétés du fichier journal NPS

Vous pouvez configurer le serveur NPS (Network Policy) pour effectuer l’authentification Dial-In Service RADIUS (Remote User), périodique et gestion des comptes pour les demandes d’authentification utilisateur, messages d’acceptation d’accès, les messages de refus d’accès, les demandes de gestion et les réponses mises à jour de l’état. Vous pouvez utiliser cette procédure pour configurer les fichiers journaux dans lequel vous souhaitez stocker les données de gestion.

Pour plus d’informations sur l’interprétation des fichiers journaux, consultez [interpréter des fichiers journaux NPS de base de données au Format](https://technet.microsoft.com/library/cc771748.aspx).

Pour empêcher les fichiers journaux de remplir le disque dur, il est fortement recommandé de les conserver sur une partition distincte de la partition système. Ce qui suit fournit plus d’informations sur la configuration de gestion d’un serveur NPS :

- Pour envoyer les données de fichier journal de collection par un autre processus, vous pouvez configurer NPS à écrire dans un canal nommé. Pour utiliser des canaux nommés, la valeur est le dossier du fichier journal \\. \pipe ou \\ComputerName\pipe. Le programme de serveur de canal nommé crée un canal nommé appelé \\.\pipe\iaslog.log pour accepter les données. Dans la boîte de dialogue Propriétés de fichier Local, dans créer un nouveau fichier journal, sélectionnez jamais (taille illimitée) lorsque vous utilisez des canaux nommés.

- Le répertoire des fichiers journaux peut être créé à l’aide de variables d’environnement système (au lieu de variables de l’utilisateur), telles que % SystemDrive%, %SystemRoot% et % Windir%. Par exemple, le chemin d’accès suivant, à l’aide de l’environnement variable % windir%, recherche le fichier journal dans le répertoire système dans le sous-dossier \System32\Logs (autrement dit, %windir%\System32\Logs\).

- Changement de format de fichier journal n’entraîne pas un nouveau journal doit être créé. Si vous modifiez les formats de fichier journal, le fichier qui est actif au moment de la modification contient un mélange des deux formats (enregistrements au début du journal sont le format précédent, et à la fin du journal aura le nouveau format).

- Si la gestion des comptes RADIUS échoue en raison d’un lecteur de disque dur complet ou d’autres causes, NPS arrête le traitement des demandes de connexion, empêchant les utilisateurs d’accéder aux ressources réseau.

- NPS fournit la possibilité de se connecter à une base de données Microsoft® SQL Server™, en plus, ou à la place de journalisation dans un fichier local.

L’appartenance à la **Admins du domaine** groupe est la condition minimale requise pour effectuer cette procédure.


### <a name="to-configure-nps-log-file-properties"></a>Pour configurer les propriétés du fichier journal NPS

1. Ouvrez la console NPS ou le composant logiciel enfichable NPS Microsoft Management Console (MMC).
2. Dans l’arborescence de la console, cliquez sur **comptabilité**.
3. Dans le volet de détails, dans **propriétés du fichier journal**, cliquez sur **modifier les propriétés de fichier journal**. Le **propriétés du fichier journal** boîte de dialogue s’ouvre.
4. Dans **propriétés du fichier journal**, dans le **paramètres** sous l’onglet **enregistrer les informations suivantes**, assurez-vous que vous choisissez de journaliser suffisamment d’informations pour atteindre vos objectifs de comptabilité. Par exemple, si vos journaux doivent effectuer la corrélation de session, sélectionnez toutes les cases à cocher.
5. Dans **action d’échec de journalisation**, sélectionnez **si la journalisation échoue, ignorer les demandes de connexion** si vous souhaitez que le serveur NPS pour arrêter le traitement des messages de demande d’accès lorsque les fichiers journaux sont complète ou indisponible pour une raison quelconque. Si vous souhaitez que le serveur NPS pour continuer le traitement des demandes de connexion si la journalisation échoue, ne sélectionnez pas cette case à cocher.
6. Dans le **propriétés du fichier journal** boîte de dialogue, cliquez sur le **fichier journal** onglet.
7. Sur le **fichier journal** sous l’onglet **répertoire**, tapez l’emplacement où vous souhaitez stocker les fichiers journaux de serveur NPS. L’emplacement par défaut est le dossier systemroot\System32\LogFiles.<br>Si vous ne fournissez pas une instruction de chemin d’accès complet dans **répertoire du fichier journal**, le chemin d’accès par défaut est utilisé. Par exemple, si vous tapez **NPSLogFile** dans **répertoire du fichier journal**, le fichier se trouve dans % systemroot%\System32\NPSLogFile.
8. Dans **Format**, cliquez sur **compatible DTS**. Si vous préférez, vous pouvez sélectionner à la place un format de fichier hérité, tel que **ODBC \(hérités\)**  ou **IAS \(hérités\)**.<br>**ODBC** et **IAS** types de fichiers hérités contiennent un sous-ensemble des informations que NPS envoie à sa base de données SQL Server. Le **compatible DTS** format XML du type de fichier est identique au format XML que NPS utilise pour importer des données dans sa base de données SQL Server. Par conséquent, le **compatible DTS** format de fichier fournit un transfert plus efficace et complète des données dans la base de données SQL Server standard pour NPS.
9. Dans **créer un nouveau fichier journal**, pour configurer NPS pour démarrer de nouveaux fichiers journaux à des intervalles spécifiés, cliquez sur l’intervalle que vous souhaitez utiliser :
    - Pour un volume de transactions lourdes et la journalisation de l’activité, cliquez sur **quotidienne**.
    - Pour les volumes de transactions et journalisation de l’activité moins importants, cliquez sur **hebdomadaire** ou **mensuel**.
    - Pour stocker toutes les transactions dans un fichier journal, cliquez sur **jamais \(taille de fichier illimitée\)**.
    - Pour limiter la taille de chaque fichier journal, cliquez sur **lorsque le fichier journal atteint cette taille**, puis tapez une taille de fichier, après laquelle un nouveau journal est créé. La taille par défaut est de 10 mégaoctets (Mo).
10. Si vous souhaitez que NPS pour supprimer les anciens fichiers journaux pour créer l’espace disque pour les nouveaux fichiers journaux lorsque le disque dur est presque saturé, assurez-vous que **lorsque le disque est plein, supprimer les anciens fichiers journaux** est sélectionné. Cette option n’est pas disponible, toutefois, si la valeur de **créer un nouveau fichier journal** est **jamais \(taille de fichier illimitée\)**. En outre, si le fichier journal le plus ancien est le fichier journal actuel, il n’est pas supprimé.

## <a name="configure-nps-sql-server-logging"></a>Configurer la journalisation du serveur NPS SQL

Vous pouvez utiliser cette procédure pour les données de gestion de RADIUS de journal à une base de données local ou distant exécutant Microsoft SQL Server.

>[!NOTE]
>NPS met en forme les données de gestion dans un document XML qu’il envoie à la **report_event** procédure stockée dans la base de données SQL Server que vous désignez dans NPS. Pour SQL Server de journalisation pour fonctionner correctement, vous devez disposer d’une procédure stockée nommée **report_event** dans la base de données SQL Server qui peut recevoir et analyser les documents XML à partir du serveur NPS.

L’appartenance à Admins du domaine, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-configure-sql-server-logging-in-nps"></a>Pour configurer la journalisation SQL Server dans NPS

1. Ouvrez la console NPS ou le composant logiciel enfichable NPS Microsoft Management Console (MMC).
2. Dans l’arborescence de la console, cliquez sur **comptabilité**.
3. Dans le volet de détails, dans **propriétés de journalisation SQL Server**, cliquez sur **propriétés de journalisation de modification SQL Server**. Le **propriétés de journalisation SQL Server** boîte de dialogue s’ouvre.
4. Dans **enregistrer les informations suivantes**, sélectionnez les informations que vous souhaitez ouvrir une session : 
    - Pour consigner toutes les demandes de gestion des comptes, cliquez sur **les demandes de gestion**.
    - Pour enregistrer les demandes d’authentification, cliquez sur **les demandes d’authentification**.
    - Pour enregistrer l’état de comptabilité périodique, cliquez sur **périodiques état comptabilité**.
    - Pour enregistrer l’état périodique, telles que les demandes de gestion provisoires, cliquez sur **état périodique**.
5. Pour configurer le nombre de sessions simultanées autorisées entre le serveur exécutant NPS et le serveur SQL Server, tapez un nombre dans **nombre maximal de sessions simultanées**.
6. Pour configurer la source de données SQL Server, dans **la journalisation SQL Server**, cliquez sur **configurer**. Le **propriétés des liaisons de données** boîte de dialogue s’ouvre. Sur le **connexion** onglet, spécifiez les éléments suivants : 
    - Pour spécifier le nom du serveur sur lequel est stockée la base de données, tapez ou sélectionnez un nom dans **sélectionner ou entrer un nom de serveur**.
    - Pour spécifier la méthode d’authentification avec lequel vous vous connectez au serveur, cliquez sur **utilisez Windows NT la sécurité intégrée**. Ou, cliquez sur **utiliser un nom d’utilisateur spécifique et un mot de passe**, puis tapez les informations d’identification dans **nom d’utilisateur** et **mot de passe**.
    - Pour autoriser un mot de passe vide, cliquez sur **mot de passe vide**.
    - Pour stocker le mot de passe, cliquez sur **autoriser l’enregistrement du mot de passe**.
    - Pour spécifier quelle base de données pour se connecter à sur l’ordinateur exécutant SQL Server, cliquez sur **sélectionner la base de données sur le serveur**, puis sélectionnez un nom de base de données à partir de la liste.
7. Pour tester la connexion entre le serveur NPS et SQL Server, cliquez sur **tester la connexion**. Cliquez sur **OK** pour fermer **propriétés des liaisons de données**.
8. Dans **action d’échec de journalisation**, sélectionnez **activer la journalisation de fichier texte pour le basculement** si vous souhaitez que NPS pour continuer avec la journalisation de fichiers texte si la journalisation SQL Server échoue. 
9. Dans **action d’échec de journalisation**, sélectionnez **si la journalisation échoue, ignorer les demandes de connexion** si vous souhaitez que le serveur NPS pour arrêter le traitement des messages de demande d’accès lorsque les fichiers journaux sont complète ou indisponible pour une raison quelconque. Si vous souhaitez que le serveur NPS pour continuer le traitement des demandes de connexion si la journalisation échoue, ne sélectionnez pas cette case à cocher.

## <a name="ping-user-name"></a>Nom d’utilisateur ping

Certains serveurs proxy RADIUS et les serveurs d’accès réseau envoient régulièrement des demandes d’authentification et gestion des comptes (appelées requêtes ping) pour vérifier que le serveur NPS se trouve sur le réseau. Ces requêtes ping contiennent des noms d’utilisateur fictif. Lorsque NPS traite ces demandes, les journaux des événements et de comptabilité deviennent remplis avec les enregistrements de rejet d’accès, rendant plus difficile à effectuer le suivi des enregistrements valides.

Lorsque vous configurez une entrée de Registre pour **un test ping du nom d’utilisateur**, NPS correspond à la valeur d’entrée de Registre par rapport à la valeur de nom d’utilisateur dans les requêtes de ping par d’autres serveurs. Un **un test ping du nom d’utilisateur** entrée de Registre spécifie le nom d’utilisateur fictif (ou un utilisateur modèle de nom, avec des variables, qui correspond au nom d’utilisateur fictif) envoyés par les serveurs proxy RADIUS et serveurs d’accès réseau. Lorsque NPS reçoit les requêtes de ping qui correspondent à la **un test ping du nom d’utilisateur** valeur d’entrée de Registre, il la rejette les demandes d’authentification sans traitement de la demande. NPS n’enregistre pas les transactions impliquant le nom d’utilisateur fictif dans tous les fichiers journaux, ce qui rend le journal des événements plus faciles à interpréter.

**Un test ping du nom d’utilisateur** n’est pas installé par défaut. Vous devez ajouter **un test ping du nom d’utilisateur** dans le Registre. Vous pouvez ajouter une entrée dans le Registre à l’aide de l’Éditeur du Registre.

>[!CAUTION]
>Une modification incorrecte du Registre peut gravement endommager votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.

### <a name="to-add-ping-user-name-to-the-registry"></a>Pour ajouter le ping de nom d’utilisateur dans le Registre

Ping-nom d’utilisateur peut être ajoutée à la clé de Registre suivante comme valeur de chaîne par un membre du groupe Administrateurs local :

`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\IAS\Parameters`

- **Nom**: `ping user-name`
- **Type**: `REG_SZ`
- **Données**:  *Nom d’utilisateur*

>[!TIP]
>Pour indiquer plusieurs noms d’utilisateur pour un **un test ping du nom d’utilisateur** valeur, entrez un modèle de nom, tel qu’un nom DNS, y compris les caractères génériques, dans **données**.
