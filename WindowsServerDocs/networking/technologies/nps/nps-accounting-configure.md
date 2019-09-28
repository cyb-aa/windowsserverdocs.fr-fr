---
title: Configurer la gestion des comptes de serveur NPS (Network Policy Server)
description: Cette rubrique fournit des informations sur les fichiers texte et la journalisation des SQL Server pour le serveur NPS (Network Policy Server) dans Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: dfde2e21-f3d5-41e8-8492-cb3f0d028afb
ms.author: pashort
author: shortpatti
ms.date: 05/25/2018
ms.openlocfilehash: 0c154d4d4534f4c343107eecd158974b92903e39
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405568"
---
# <a name="configure-network-policy-server-accounting"></a>Configurer la gestion des comptes de serveur NPS (Network Policy Server)

Il existe trois types de journalisation pour NPS \(\)(Network Policy Server) :

- **Journalisation des événements**. Utilisé principalement pour l’audit et le dépannage des tentatives de connexion. Vous pouvez configurer la journalisation des événements NPS en obtenant les propriétés du serveur NPS dans la console NPS.

- **Enregistrement des demandes d’authentification des utilisateurs et de gestion des comptes dans un fichier local**. Principalement utilisé pour l’analyse des connexions et la facturation. Également utile comme outil d’investigation de sécurité, car il vous offre une méthode de suivi de l’activité d’un utilisateur malveillant après une attaque. Vous pouvez configurer la journalisation des fichiers locaux à l’aide de l’Assistant Configuration de la gestion des comptes.

- **Enregistrement des demandes d’authentification des utilisateurs et de gestion des comptes dans une base de données conforme à la norme XML Microsoft SQL Server**. Permet d’autoriser plusieurs serveurs exécutant NPS à avoir une seule source de données. Offre également les avantages de l’utilisation d’une base de données relationnelle. Vous pouvez configurer SQL Server la journalisation à l’aide de l’Assistant Configuration de la gestion des comptes.

## <a name="use-the-accounting-configuration-wizard"></a>Utiliser l’Assistant Configuration de la gestion des comptes

À l’aide de l’Assistant Configuration de la gestion des comptes, vous pouvez configurer les quatre paramètres de gestion des comptes suivants :

- **Journalisation SQL uniquement**. En utilisant ce paramètre, vous pouvez configurer un lien de données vers une SQL Server qui permet à NPS de se connecter à SQL Server et d’envoyer des données de gestion de comptes. En outre, l’Assistant peut configurer la base de données sur le SQL Server pour s’assurer que la base de données est compatible avec la journalisation SQL Server NPS.
- **Journalisation du texte uniquement**. En utilisant ce paramètre, vous pouvez configurer NPS pour enregistrer les données de gestion des comptes dans un fichier texte.
- **Journalisation parallèle**. En utilisant ce paramètre, vous pouvez configurer le SQL Server la liaison de données et la base de données. Vous pouvez également configurer la journalisation des fichiers texte afin que NPS journalise simultanément dans le fichier texte et la base de données SQL Server. 
- **Journalisation SQL avec sauvegarde**. En utilisant ce paramètre, vous pouvez configurer le SQL Server la liaison de données et la base de données. En outre, vous pouvez configurer la journalisation de fichier texte que NPS utilise si la journalisation SQL Server échoue.

Outre ces paramètres, les SQL Server la journalisation et la journalisation textuelle vous permettent de spécifier si NPS continue à traiter les demandes de connexion en cas d’échec de la journalisation. Vous pouvez le spécifier dans la **section action d’échec de la journalisation** dans les propriétés de journalisation de fichier local, dans les propriétés de journalisation SQL Server et Pendant l’exécution de l’Assistant Configuration de la gestion des comptes.

### <a name="to-run-the-accounting-configuration-wizard"></a>Pour exécuter l’Assistant Configuration de la gestion des comptes

Pour exécuter l’Assistant Configuration de la gestion des comptes, procédez comme suit :

1. Ouvrez la console NPS ou le composant logiciel enfichable MMC (Microsoft Management Console) NPS.
2. Dans l’arborescence de la console, cliquez sur **comptabilité**.
3. Dans le volet d’informations, dans **comptabilité**, cliquez sur **configurer la gestion des comptes**.

## <a name="configure-nps-log-file-properties"></a>Configurer les propriétés du fichier journal NPS

Vous pouvez configurer le serveur NPS (Network Policy Server) pour effectuer la gestion des comptes protocole RADIUS (Remote Authentication Dial-In User Service) (RADIUS) des demandes d’authentification des utilisateurs, des messages d’acceptation des accès, des messages de refus d’accès, des demandes de comptabilité et des réponses, et périodique mises à jour de l’État. Vous pouvez utiliser cette procédure pour configurer les fichiers journaux dans lesquels vous souhaitez stocker les données de gestion des comptes.

Pour plus d’informations sur l’interprétation des fichiers journaux, consultez [interpréter les fichiers journaux au format de base de données NPS](https://technet.microsoft.com/library/cc771748.aspx).

Pour empêcher les fichiers journaux de saturer le disque dur, il est fortement recommandé de les conserver sur une partition distincte de la partition système. Vous trouverez ci-dessous plus d’informations sur la configuration de la gestion des comptes pour NPS :

- Pour envoyer les données du fichier journal pour la collecte par un autre processus, vous pouvez configurer NPS pour écrire dans un canal nommé. Pour utiliser des canaux nommés, définissez le dossier du fichier \\journal sur \\.\pipe ou ComputerName\pipe. Le programme de serveur de canal nommé crée un canal \\nommé appelé .\pipe\iaslog.log pour accepter les données. Dans la boîte de dialogue Propriétés du fichier local, dans créer un fichier journal, sélectionnez jamais (taille de fichier illimitée) quand vous utilisez des canaux nommés.

- Le répertoire du fichier journal peut être créé à l’aide de variables d’environnement système (au lieu de variables utilisateur), telles que% systemdrive%,% systemroot% et% windir%. Par exemple, le chemin d’accès suivant, à l’aide de la variable d’environnement% windir%, localise le fichier journal dans le répertoire système dans le sous-dossier \System32\Logs (c’est-à-dire,%windir%\System32\Logs @ no__t-0.

- Le changement de format de fichier journal n’entraîne pas la création d’un nouveau journal. Si vous modifiez les formats de fichier journal, le fichier qui est actif au moment de la modification contient une combinaison des deux formats (les enregistrements au début du journal auront le format précédent, et les enregistrements à la fin du journal auront le nouveau format).

- Si la gestion des comptes RADIUS échoue en raison d’un lecteur de disque dur complet ou d’autres causes, le serveur NPS cesse de traiter les demandes de connexion, ce qui empêche les utilisateurs d’accéder aux ressources réseau.

- Le serveur NPS permet de se connecter à une base de données Microsoft® SQL Server™ en plus ou au lieu de la journalisation dans un fichier local.

Pour effectuer cette procédure, vous devez au minimum appartenir au groupe **Admins du domaine** .


### <a name="to-configure-nps-log-file-properties"></a>Pour configurer les propriétés du fichier journal NPS

1. Ouvrez la console NPS ou le composant logiciel enfichable MMC (Microsoft Management Console) NPS.
2. Dans l’arborescence de la console, cliquez sur **comptabilité**.
3. Dans le volet d’informations, dans **Propriétés du fichier journal**, cliquez sur **modifier les propriétés du fichier journal**. La boîte de dialogue **Propriétés du fichier journal** s’ouvre.
4. Dans **Propriétés du fichier journal**, sous l’onglet **paramètres** , dans **consigner les informations suivantes**dans le journal, veillez à enregistrer suffisamment d’informations pour atteindre vos objectifs de comptabilité. Par exemple, si vos journaux doivent effectuer la corrélation de session, activez toutes les cases à cocher.
5. Dans **action d’échec de journalisation**, sélectionnez **si la journalisation échoue, ignorer les demandes de connexion** si vous souhaitez que NPS arrête le traitement des messages de demande d’accès lorsque les fichiers journaux sont saturés ou indisponibles pour une raison quelconque. Si vous souhaitez que NPS continue de traiter les demandes de connexion en cas d’échec de la journalisation, n’activez pas cette case à cocher.
6. Dans la boîte de dialogue **Propriétés du fichier journal** , cliquez sur l’onglet **fichier journal** .
7. Sous l’onglet **fichier journal** , dans le **répertoire**, tapez l’emplacement où vous souhaitez stocker les fichiers journaux NPS. L’emplacement par défaut est le dossier systemroot\System32\LogFiles.<br>Si vous ne fournissez pas d’instruction de chemin d’accès complet dans le **répertoire du fichier journal**, le chemin d’accès par défaut est utilisé. Par exemple, si vous tapez **NPSLogFile** dans le **répertoire du fichier journal**, le fichier se trouve à l’emplacement suivant :%systemroot%\System32\NPSLogFile.
8. Dans **format**, cliquez sur **compatible DTS**. Si vous préférez, vous pouvez sélectionner à la place un format de fichier hérité, tel que **ODBC \(hérité\)**  ou **IAS \(hérité.\)**<br>Les types de fichiers hérités **ODBC** et **IAS** contiennent un sous-ensemble des informations que NPS envoie à sa base de données SQL Server. Le format XML du type de fichier **compatible DTS** est identique au format XML utilisé par le serveur NPS pour importer des données dans sa base de données SQL Server. Par conséquent, le format de fichier **compatible DTS** fournit un transfert plus efficace et plus efficace des données dans la base de données SQL Server standard pour NPS.
9. Dans **créer un nouveau fichier journal**, pour configurer NPS afin de démarrer de nouveaux fichiers journaux à intervalles spécifiés, cliquez sur l’intervalle que vous souhaitez utiliser :
    - Pour activité intensive du volume de transactions et de la journalisation, cliquez sur tous les **jours**.
    - Pour les volumes de transaction inférieurs et l’activité de journalisation, cliquez sur **hebdomadaire** ou **mensuel**.
    - Pour stocker toutes les transactions dans un fichier journal, cliquez sur **taille \(\)de fichier jamais illimitée**.
    - Pour limiter la taille de chaque fichier journal, cliquez sur **quand le fichier journal atteint cette taille**, puis tapez une taille de fichier, après laquelle un nouveau journal est créé. La taille par défaut est de 10 mégaoctets (Mo).
10. Si vous souhaitez que NPS supprime les anciens fichiers journaux afin de créer de l’espace disque pour les nouveaux fichiers journaux lorsque le disque dur est proche de la capacité, assurez-vous que l’option **lorsque le disque est saturé supprimer les anciens fichiers journaux** est sélectionnée. Toutefois, cette option n’est pas disponible si la valeur de **créer un nouveau fichier journal** n' **est \(jamais de taille\)de fichier illimitée**. En outre, si le fichier journal le plus ancien est le fichier journal actuel, il n’est pas supprimé.

## <a name="configure-nps-sql-server-logging"></a>Configurer la journalisation des SQL Server NPS

Vous pouvez utiliser cette procédure pour enregistrer les données de gestion des comptes RADIUS dans une base de données locale ou distante exécutant Microsoft SQL Server.

>[!NOTE]
>NPS met en forme les données de gestion sous forme de document XML qu’il envoie à la procédure stockée **report_event** dans la base de données SQL Server que vous désignez dans NPS. Pour SQL Server la journalisation pour fonctionner correctement, vous devez disposer d’une procédure stockée nommée **report_event** dans la base de données SQL Server qui peut recevoir et analyser les documents XML à partir du serveur NPS.

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe Admins du domaine ou à un groupe équivalent.

### <a name="to-configure-sql-server-logging-in-nps"></a>Pour configurer la journalisation SQL Server dans NPS

1. Ouvrez la console NPS ou le composant logiciel enfichable MMC (Microsoft Management Console) NPS.
2. Dans l’arborescence de la console, cliquez sur **comptabilité**.
3. Dans le volet d’informations, dans **SQL Server propriétés de journalisation**, cliquez sur **modifier les propriétés de journalisation SQL Server**. La boîte de dialogue **Propriétés de journalisation SQL Server** s’ouvre.
4. Dans **enregistrer les informations suivantes dans le journal**, sélectionnez les informations que vous souhaitez consigner : 
    - Pour consigner toutes les demandes de gestion des comptes, cliquez sur **demandes de gestion**.
    - Pour consigner les demandes d’authentification, cliquez sur **demandes d’authentification**.
    - Pour enregistrer l’état de la comptabilité périodique, cliquez sur **État de comptabilité périodique**.
    - Pour enregistrer l’état périodique, par exemple les demandes de gestion de comptes intermédiaires, cliquez sur **état périodique**.
5. Pour configurer le nombre de sessions simultanées autorisées entre le serveur NPS et le SQL Server, tapez un nombre dans **nombre maximal de sessions simultanées**.
6. Pour configurer la source de données SQL Server, dans **SQL Server la journalisation**, cliquez sur **configurer**. La boîte de dialogue **Propriétés des liaisons de données** s’ouvre. Sous l’onglet **connexion** , spécifiez les éléments suivants : 
    - Pour spécifier le nom du serveur sur lequel la base de données est stockée, tapez ou sélectionnez un nom dans **Sélectionner ou entrez un nom de serveur**.
    - Pour spécifier la méthode d’authentification avec laquelle se connecter au serveur, cliquez sur **utiliser la sécurité intégrée de Windows NT**. Sinon, cliquez sur **utiliser un nom d’utilisateur et un mot de passe spécifiques**, puis tapez les informations d’identification dans **nom d’utilisateur** et **mot de passe**.
    - Pour autoriser un mot de passe vide, cliquez sur **mot de passe vide**.
    - Pour stocker le mot de passe, cliquez sur **autoriser l’enregistrement du mot de passe**.
    - Pour spécifier la base de données à laquelle se connecter sur l’ordinateur exécutant SQL Server, cliquez sur **Sélectionner la base de données sur le serveur**, puis sélectionnez un nom de base de données dans la liste.
7. Pour tester la connexion entre NPS et SQL Server, cliquez sur **tester la connexion**. Cliquez sur **OK** pour fermer **Propriétés des liaisons de données**.
8. Dans **action d’échec de journalisation**, sélectionnez **activer la journalisation des fichiers texte pour le basculement** si vous souhaitez que NPS continue la journalisation des fichiers texte si SQL Server journalisation échoue. 
9. Dans **action d’échec de journalisation**, sélectionnez **si la journalisation échoue, ignorer les demandes de connexion** si vous souhaitez que NPS arrête le traitement des messages de demande d’accès lorsque les fichiers journaux sont saturés ou indisponibles pour une raison quelconque. Si vous souhaitez que NPS continue de traiter les demandes de connexion en cas d’échec de la journalisation, n’activez pas cette case à cocher.

## <a name="ping-user-name"></a>Ping-nom d’utilisateur

Certains serveurs proxy RADIUS et serveurs d’accès réseau envoient régulièrement des demandes d’authentification et de gestion des comptes (appelées requêtes ping) pour vérifier que le serveur NPS est présent sur le réseau. Ces demandes ping incluent des noms d’utilisateur fictifs. Lorsque NPS traite ces requêtes, les journaux des événements et de gestion des comptes sont remplis avec des enregistrements de refus d’accès, ce qui rend plus difficile le suivi des enregistrements valides.

Quand vous configurez une entrée de Registre pour **ping User-Name**, NPS fait correspondre la valeur de l’entrée de Registre à la valeur de nom d’utilisateur dans les demandes ping effectuées par d’autres serveurs. Une entrée de Registre **ping User-Name** spécifie le nom d’utilisateur fictif (ou un modèle de nom d’utilisateur, avec des variables, qui correspond au nom d’utilisateur fictif) envoyé par les serveurs proxy RADIUS et les serveurs d’accès réseau. Lorsque NPS reçoit des demandes ping correspondant à la valeur d’entrée de Registre **ping User-Name** , NPS rejette les demandes d’authentification sans traiter la demande. NPS n’enregistre pas les transactions impliquant le nom d’utilisateur fictif dans des fichiers journaux, ce qui facilite l’interprétation du journal des événements.

**Le nom d’utilisateur ping** n’est pas installé par défaut. Vous devez ajouter **ping User-Name** au registre. Vous pouvez ajouter une entrée au registre à l’aide de l’éditeur du Registre.

>[!CAUTION]
>Une modification incorrecte du Registre peut gravement endommager votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.

### <a name="to-add-ping-user-name-to-the-registry"></a>Pour ajouter le nom d’utilisateur ping au registre

Ping User-Name peut être ajouté à la clé de Registre suivante sous la forme d’une valeur de chaîne par un membre du groupe Administrateurs local :

`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\IAS\Parameters`

- **Nom**:`ping user-name`
- **Tapez**:`REG_SZ`
- **Données**:  *Nom d’utilisateur*

>[!TIP]
>Pour indiquer plusieurs noms d’utilisateur pour une valeur de **nom d’utilisateur ping** , entrez un modèle de nom, tel qu’un nom DNS, y compris des caractères génériques, dans les **données**.
