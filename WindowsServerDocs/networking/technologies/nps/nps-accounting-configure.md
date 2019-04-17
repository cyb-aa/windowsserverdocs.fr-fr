---
title: Configurer la gestion de serveur de stratégie réseau
description: Cette rubrique fournit des informations sur les fichiers de texte et de journalisation pour le serveur NPS dans Windows Server 2016 SQL Server.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: dfde2e21-f3d5-41e8-8492-cb3f0d028afb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 69341e30d90ee1be29c40d835a4f71fe433c11dc
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-network-policy-server-accounting"></a>Configurer la gestion de serveur de stratégie réseau

Il existe trois types de journalisation pour le serveur de stratégie réseau \(NPS\):

- **La journalisation des événements**. Principalement utilisées pour l’audit et la résolution des problèmes de tentatives de connexion. Vous pouvez configurer la journalisation en obtenant les propriétés du serveur NPS dans la console NPS des événements NPS.

- **Journalisation de l’authentification des utilisateurs et les demandes de gestion dans un fichier local**. À des fins principalement utilisés pour l’analyse de la connexion et de facturation. Également utile comme un outil d’enquête sécurité car elle vous offre une méthode de suivi de l’activité d’un utilisateur malveillant après une attaque. Vous pouvez configurer la journalisation de fichier local à l’aide de l’Assistant Configuration de gestion des comptes.

- **Journalisation de l’authentification des utilisateurs et les demandes de gestion pour une base de données Microsoft SQL Server compatible XML**. Permet à plusieurs serveurs exécutant NPS pour une seule source de données. Fournit également les avantages de l’utilisation d’une base de données relationnelle. Vous pouvez configurer la journalisation SQL Server à l’aide de l’Assistant Configuration de gestion des comptes.

## <a name="use-the-accounting-configuration-wizard"></a>Utilisez l’Assistant Configuration de gestion des comptes

À l’aide de l’Assistant Configuration de gestion, vous pouvez configurer les quatre paramètres de gestion des comptes suivants:

- **La journalisation SQL uniquement**. À l’aide de ce paramètre, vous pouvez configurer une liaison de données vers un serveur SQL qui permet au serveur NPS pour se connecter à et envoyer des données de gestion pour le serveur SQL server. En outre, l’Assistant peut configurer la base de données sur le serveur SQL Server pour vous assurer que la base de données est compatible avec la journalisation du serveur NPS SQL.
- **Journalisation texte uniquement**. À l’aide de ce paramètre, vous pouvez configurer NPS pour enregistrer les données de gestion dans un fichier texte.
- **La journalisation en parallèle**. À l’aide de ce paramètre, vous pouvez configurer la liaison de données SQL Server et la base de données. Vous pouvez également configurer la journalisation de fichier texte afin que NPS enregistre simultanément dans le fichier texte et de la base de données SQL Server. 
- **La journalisation SQL avec sauvegarde**. À l’aide de ce paramètre, vous pouvez configurer la liaison de données SQL Server et la base de données. En outre, vous pouvez configurer la journalisation de fichier texte que NPS utilise en cas d’échec de la journalisation SQL Server.

En plus de ces paramètres, la journalisation SQL Server et la journalisation du texte permettent de spécifier si NPS continue de traiter les demandes de connexion si la journalisation échoue. Vous pouvez spécifier cela dans le **section d’action de l’échec de la journalisation** dans les propriétés de journalisation de fichier local, dans les propriétés de journalisation du serveur SQL et pendant l’exécution de l’Assistant Configuration de comptabilité.

### <a name="to-run-the-accounting-configuration-wizard"></a>Pour exécuter l’Assistant Configuration de gestion des comptes

Pour exécuter l’Assistant Configuration de comptabilité, procédez comme suit:

1. Ouvrez la console NPS ou le composant logiciel enfichable serveur NPS Microsoft Management Console (MMC).
2. Dans l’arborescence de la console, cliquez sur **comptabilité**.
3. Dans le volet d’informations, dans **comptabilité**, cliquez sur **configurer la gestion des comptes**.

## <a name="configure-nps-log-file-properties"></a>Configurer les propriétés du fichier journal NPS

Vous pouvez configurer le serveur NPS (Network Policy) pour effectuer la gestion des comptes Authentication Dial-In utilisateur RADIUS (Remote Service) pour les demandes d’authentification utilisateur, accès-accepter les messages, les messages de refus d’accès, les demandes de gestion des réponses et des mises à jour périodiques de l’état. Vous pouvez utiliser cette procédure pour configurer les fichiers journaux dans lequel vous souhaitez stocker les données de gestion.

Pour plus d’informations sur l’interprétation des fichiers journaux, consultez [interpréter les fichiers journaux NPS base de données de Format](https://technet.microsoft.com/library/cc771748.aspx).

Pour empêcher les fichiers journaux de remplir le disque dur, il est fortement recommandé de les conserver sur une partition distincte de la partition système. La liste suivante fournit plus d’informations sur la configuration de gestion d’un serveur NPS:

- Pour envoyer les données du fichier journal pour le regroupement par un autre processus, vous pouvez configurer NPS pour écrire sur un canal nommé. Pour utiliser des canaux nommés, affectez le dossier du fichier journal \\.\pipe ou \\ComputerName\pipe. Le programme de serveur de canal nommé crée un canal nommé appelé \\.\pipe\iaslog.log pour accepter les données. Dans la boîte de dialogue Propriétés de fichier Local, dans créer un nouveau fichier journal, sélectionnez jamais (taille du fichier illimité) lorsque vous utilisez des canaux nommés.

- Le répertoire des fichiers journaux peut être créé à l’aide de variables d’environnement système (au lieu de variables de l’utilisateur), par exemple, % windir%, % SystemDrive% et % SystemRoot%. Par exemple, le chemin d’accès suivant, à l’aide de l’environnement variable % windir%, recherche le fichier journal dans le répertoire du système dans le sous-dossier \System32\Logs (autrement dit, % windir%\System32\Logs\).

- Changement des formats de fichier journal n’entraîne pas un nouveau journal doit être créé. Si vous modifiez les formats de fichier journal, le fichier est actif au moment de la modification contient un mélange des deux formats (enregistrements du début du journal seront l’ancien format, et à la fin du journal aura le nouveau format).

- Si la gestion de comptes RADIUS échoue en raison d’un disque dur saturé ou d’autres causes, NPS cesse de traiter les demandes de connexion, empêchant les utilisateurs d’accéder aux ressources réseau.

- NPS fournit la possibilité de se connecter à une base de données Microsoft® SQL Server™, en plus, ou à la place de journalisation dans un fichier local.

L’appartenance au groupe le **Admins du domaine** groupe est la condition minimale requise pour effectuer cette procédure.


### <a name="to-configure-nps-log-file-properties"></a>Pour configurer les propriétés du fichier journal NPS

1. Ouvrez la console NPS ou le composant logiciel enfichable serveur NPS Microsoft Management Console (MMC).
2. Dans l’arborescence de la console, cliquez sur **comptabilité**.
3. Dans le volet d’informations, dans **propriétés du fichier journal**, cliquez sur **modifier les propriétés de fichier journal**. Le **propriétés du fichier journal** boîte de dialogue s’ouvre.
4. Dans **propriétés du fichier journal**, dans le **paramètres** onglet **consigner les informations suivantes**, assurez-vous que vous choisissez de journaliser suffisamment d’informations pour atteindre vos objectifs de comptabilité. Par exemple, si vos journaux doivent effectuer la corrélation de session, sélectionnez toutes les cases à cocher.
5. Dans **journalisation de l’action de l’échec**, sélectionnez **si la journalisation échoue, ignorer les demandes de connexion** si vous souhaitez que NPS pour arrêter le traitement des messages de demande d’accès lorsque les fichiers journaux sont complète ou indisponible pour une raison quelconque. Si vous souhaitez que NPS pour poursuivre le traitement des demandes de connexion si la journalisation échoue, ne sélectionnez pas cette case à cocher.
6. Dans le **propriétés du fichier journal** boîte de dialogue, cliquez sur le **fichier journal** onglet.
7. Sur le **fichier journal** onglet **active**, tapez l’emplacement où vous souhaitez stocker les fichiers journaux NPS. L’emplacement par défaut est le dossier systemroot\System32\LogFiles.
    >[!NOTE]
    >Si vous ne spécifiez pas une instruction de chemin d’accès complet dans **répertoire du fichier journal**, le chemin d’accès par défaut est utilisé. Par exemple, si vous tapez **NPSLogFile** dans **répertoire du fichier journal**, le fichier se trouve dans % systemroot%\System32\NPSLogFile.
8. Dans **Format**, cliquez sur **compatible DTS**. Si vous préférez, vous pouvez sélectionner à la place un format de fichier hérité, tel que **ODBC \(Legacy\)** ou **IAS \(Legacy\)**.
    >[!NOTE]
    >**ODBC** et **IAS** types de fichiers hérités contiennent un sous-ensemble des informations que NPS envoie sa base de données SQL Server. Le **compatible DTS** au format XML du type de fichier est identique au format XML que NPS utilise pour importer des données dans sa base de données SQL Server. Par conséquent, le **compatible DTS** format de fichier fournit un transfert plus efficace et plus complète des données dans la base de données SQL Server standard d’un serveur NPS.
9. Dans **créer un nouveau fichier journal**, pour configurer NPS pour démarrer les nouveaux fichiers journaux à intervalles réguliers, cliquez sur l’intervalle que vous souhaitez utiliser:
    - Volume lourd de transactions et de journalisation de l’activité, cliquez sur **quotidienne**.
    - Pour les volumes de transactions et journalisation de l’activité moins importants, cliquez sur **hebdomadaire** ou **mensuel**.
    - Pour stocker toutes les transactions dans un fichier journal, cliquez sur **jamais \(unlimited file size\)**.
    - Pour limiter la taille de chaque fichier journal, cliquez sur **lorsque le fichier journal atteint cette taille**, puis tapez une taille de fichier, après laquelle un nouveau journal est créé. La taille par défaut est 10 mégaoctets (Mo).
10. Si vous souhaitez que NPS pour supprimer les anciens fichiers journaux pour créer l’espace disque pour les nouveaux fichiers journaux lorsque le disque dur est saturé, assurez-vous que **lorsque le disque est plein, supprimer les anciens fichiers journaux** est sélectionné. Cette option n’est pas disponible, toutefois, si la valeur de **créer un nouveau fichier journal** est **jamais \(unlimited file size\)**. En outre, si l’ancien fichier journal est le fichier journal actuel, il n’est pas supprimé.

## <a name="configure-nps-sql-server-logging"></a>Configurer la journalisation du serveur NPS SQL

Vous pouvez utiliser cette procédure aux données de gestion de comptes RADIUS de journal à une base de données local ou distant exécutant Microsoft SQL Server.

>[!NOTE]
>NPS formate les données de gestion en tant qu’un document XML qu’il envoie à la **report_event** procédure stockée dans la base de données SQL Server que vous désignez dans NPS. Pour SQL Server de la journalisation pour fonctionner correctement, vous devez disposer d’une procédure stockée nommée **report_event** dans la base de données SQL Server qui peut recevoir et analyser les documents XML à partir du serveur NPS.

L’appartenance à Admins du domaine, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-configure-sql-server-logging-in-nps"></a>Pour configurer la journalisation SQL Server dans NPS

1. Ouvrez la console NPS ou le composant logiciel enfichable serveur NPS Microsoft Management Console (MMC).
2. Dans l’arborescence de la console, cliquez sur **comptabilité**.
3. Dans le volet d’informations, dans **propriétés de journalisation SQL Server**, cliquez sur **propriétés de journalisation de modification de SQL Server**. Le **propriétés de journalisation SQL Server** boîte de dialogue s’ouvre.
4. Dans **consigner les informations suivantes**, sélectionnez les informations que vous souhaitez ouvrir une session: 
    - Pour vous connecter à toutes les demandes, cliquez sur **demandes**.
    - Pour enregistrer les demandes d’authentification, cliquez sur **les demandes d’authentification**.
    - Pour enregistrer l’état périodique comptabilité, cliquez sur **l’état périodique comptabilisation**.
    - Pour enregistrer l’état périodique, tels que les demandes de gestion provisoires, cliquez sur **état périodique**.
5. Pour configurer le nombre de sessions simultanées autorisées entre le serveur exécutant NPS et le serveur SQL Server, tapez un nombre dans **nombre maximal de sessions simultanées**.
6. Pour configurer la source de données SQL Server, dans **la journalisation SQL Server**, cliquez sur **configurer**. Le **propriétés des liaisons de données** boîte de dialogue s’ouvre. Sur le **connexion** onglet, spécifiez les éléments suivants: 
    - Pour spécifier le nom du serveur sur lequel est stockée la base de données, tapez ou sélectionnez un nom dans **Sélectionnez ou entrez un nom de serveur**.
    - Pour spécifier la méthode d’authentification permettant d’ouvrir une session sur le serveur, cliquez sur **sécurité intégrée utilisation Windows NT**. Ou, cliquez sur **utiliser un nom d’utilisateur spécifique et un mot de passe**, puis tapez les informations d’identification **nom d’utilisateur** et **mot de passe**.
    - Pour autoriser un mot de passe vide, cliquez sur **mot de passe vide**.
    - Pour stocker le mot de passe, cliquez sur **autoriser l’enregistrement du mot de passe**.
    - Pour spécifier une base de données qui se connectent à sur l’ordinateur exécutant SQL Server, cliquez sur **sélectionner la base de données sur le serveur**, puis sélectionnez un nom de base de données à partir de la liste.
7. Pour tester la connexion entre le serveur NPS et SQL Server, cliquez sur **tester la connexion**. Cliquez sur **OK** pour fermer **propriétés des liaisons de données**.
8. Dans **journalisation de l’action de l’échec**, sélectionnez **activer la journalisation de fichier texte pour le basculement** si vous souhaitez que NPS pour poursuivre la journalisation de fichiers texte en cas d’échec de la journalisation SQL Server. 
9. Dans **journalisation de l’action de l’échec**, sélectionnez **si la journalisation échoue, ignorer les demandes de connexion** si vous souhaitez que NPS pour arrêter le traitement des messages de demande d’accès lorsque les fichiers journaux sont complète ou indisponible pour une raison quelconque. Si vous souhaitez que NPS pour poursuivre le traitement des demandes de connexion si la journalisation échoue, ne sélectionnez pas cette case à cocher.

## <a name="ping-user-name"></a>Nom d’utilisateur ping

Certains serveurs proxy RADIUS et les serveurs d’accès réseau envoient régulièrement des demandes d’authentification et de gestion des comptes (appelées requêtes ping) pour vérifier que le serveur NPS est présent sur le réseau. Ces requêtes ping contiennent des noms d’utilisateurs fictifs. Lorsque le serveur NPS traite ces demandes, les journaux des événements et gestion des comptes sont remplissent avec des enregistrements de refuser l’accès, rend plus difficile à effectuer le suivi des enregistrements valides.

Lorsque vous configurez une entrée de Registre pour **ping nom d’utilisateur**, NPS correspond à la valeur d’entrée de Registre par rapport à la valeur de nom d’utilisateur dans des requêtes ping par d’autres serveurs. Un **ping nom d’utilisateur** entrée de Registre spécifie le nom d’utilisateur fictif (ou un utilisateur modèle de nom, avec des variables, qui correspond au nom d’utilisateur fictif) envoyés par les serveurs proxy RADIUS et serveurs d’accès réseau. Lorsque le serveur NPS reçoit des requêtes ping qui correspondent à la **ping nom d’utilisateur** valeur d’entrée de Registre, la rejette les demandes d’authentification sans traitement de la demande. NPS n’enregistre pas les transactions impliquant le nom d’utilisateur fictif dans tous les fichiers journaux, ce qui facilite le journal des événements interpréter.

**Nom d’utilisateur de ping** n’est pas installé par défaut. Vous devez ajouter **ping nom d’utilisateur** dans le Registre. Vous pouvez ajouter une entrée au Registre à l’aide de l’Éditeur du Registre.

>[!CAUTION]
>Une modification incorrecte du Registre peut endommager gravement votre système. Avant d’apporter des modifications au Registre, sauvegardez toutes vos données importantes sur l’ordinateur.

### <a name="to-add-ping-user-name-to-the-registry"></a>Pour ajouter un nom d’utilisateur ping au Registre

Ping-nom d’utilisateur peut être ajouté à la clé de Registre suivante comme une valeur de chaîne par un membre du groupe Administrateurs local:

`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\IAS\Parameters`

- **Nom**: `ping user-name`
- **Type**: `REG_SZ`
- **Données**: *nom d’utilisateur*

>[!TIP]
>Pour indiquer plusieurs noms d’utilisateur pour un **ping nom d’utilisateur** valeur, entrez un modèle de nom, par exemple, un nom DNS, y compris des caractères génériques dans **données**.
