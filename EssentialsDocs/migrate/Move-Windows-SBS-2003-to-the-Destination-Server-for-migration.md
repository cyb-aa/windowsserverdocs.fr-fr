---
title: Déplacer les paramètres et données de Windows SBS 2003 vers le serveur de destination pour la migration vers Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67087ccb-d820-4642-8ca2-7d2d38714014
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9fd9cdfaea641a0aee615befb5d400fa45160d97
ms.sourcegitcommit: 9a4ab3a0d00b06ff16173aed616624c857589459
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66828553"
---
# <a name="move-windows-sbs-2003-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Déplacer les paramètres et données de Windows SBS 2003 vers le serveur de destination pour la migration vers Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Déplacez les paramètres et les données vers le serveur de destination comme suit :

1. [Copier des données vers le serveur de Destination](#copy-data-to-the-destination-server)

2. [Importer des comptes d’utilisateur Active Directory à bord Windows Server Essentials (facultatif)](#import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard)

3. [Supprimer les anciens scripts d’ouverture de session (facultatifs)](#remove-old-logon-scripts)

4. [Supprimer les anciens Active Directory objets stratégie de groupe (facultatif)](#remove-legacy-active-directory-group-policy-objects) 

5. [Configurer le réseau](#configure-the-network) 

6. [Mapper les ordinateurs autorisés aux comptes d’utilisateurs](#map-permitted-computers-to-user-accounts)

## <a name="copy-data-to-the-destination-server"></a>Copier les données vers le serveur de destination
Avant de copier des données du serveur source vers le serveur de destination, effectuez les tâches suivantes : 

- Passez en revue la liste des dossiers partagés sur le serveur source, y compris les autorisations de chaque dossier. Créez ou personnalisez les dossiers sur le serveur de destination afin qu'ils correspondent à la structure de dossiers que vous migrez depuis le serveur source. 

- Passez en revue la taille de chaque dossier et vérifiez que le serveur de destination dispose de suffisamment d'espace de stockage. 

- Mettez en lecture seule les dossiers partagés sur le serveur source pour tous les utilisateurs afin qu'aucune écriture ne puisse être réalisée sur le lecteur pendant que vous copiez les fichiers vers le serveur de destination. 

#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Pour copier les données du serveur source vers le serveur de destination 

1. Ouvrez une session sur le serveur de destination en tant qu'administrateur de domaine. 

2. Cliquez sur **Démarrer**, tapez **cmd** dans la zone de recherche, puis appuyez sur Entrée. 

3. À l'invite de commandes, tapez la commande suivante et appuyez sur Entrée : 

    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt` 

Où :
 - \<Nom_serveur_source\> est le nom du serveur Source
 - \<Nomdossiersourcepartagé\> est le nom du dossier partagé sur le serveur Source
 - \<NomServeurDestination\> est le nom du serveur de Destination,
 - \<Nomdossierdestinationpartagé\> est le dossier partagé sur le serveur de Destination vers lequel les données seront copiées. 

4. Répétez l'étape précédente pour chaque dossier partagé que vous migrez depuis le serveur source.

## <a name="import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard"></a>Importer des comptes d’utilisateurs Active Directory dans le tableau de bord Windows Server Essentials
 Par défaut, tous les comptes d’utilisateur créés sur le serveur Source migrent automatiquement vers le tableau de bord dans Windows Server Essentials. Toutefois, la migration automatique d'un compte d'utilisateur Active Directory échouera si certaines propriétés ne répondent pas aux exigences de migration. Vous pouvez utiliser l'applet de commande Windows PowerShell suivante pour importer les utilisateurs Active Directory.

#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Pour importer un compte d’utilisateur Active Directory dans le tableau de bord Windows Server Essentials

1. Ouvrez une session sur le serveur de destination en tant qu'administrateur de domaine.

2. Ouvrez Windows PowerShell en tant qu'administrateur.

3. Exécutez l’applet de commande suivante, où `[AD username]` est le nom du compte d’utilisateur Active Directory que vous souhaitez importer :

    `Import-WssUser SamAccountName [AD username]`

## <a name="remove-old-logon-scripts"></a>Supprimer les anciens scripts d’ouverture de session
Windows SBS 2003 utilise des scripts d'ouverture de session pour des tâches telles que l'installation du logiciel et la personnalisation des postes de travail. Windows Server Essentials remplace les scripts d’ouverture de session Windows SBS 2003 par une combinaison de scripts d’ouverture de session et des objets de stratégie de groupe.

> [!NOTE]
> Si vous avez modifié les scripts d'ouverture de session Windows SBS 2003, vous devez renommer les scripts pour conserver vos personnalisations.
>
> Les scripts d'ouverture de session Windows SBS 2003 s'appliquent uniquement aux comptes d'utilisateurs qui ont été ajoutés à l'aide de l' **Assistant Ajout de nouveaux utilisateurs**.

#### <a name="to-remove-the-windows-sbs-2003-logon-scripts"></a>Pour supprimer les scripts d'ouverture de session Windows SBS 2003 

1. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Utilisateurs et ordinateurs Active Directory**.

2. Dans **Utilisateurs et ordinateurs Active Directory**, étendez votre réseau, puis cliquez sur **Utilisateurs**.

3. Cliquez avec le bouton droit sur un nom d'utilisateur, cliquez sur **Propriétés**, puis sur l'onglet **Profil** .

4. Supprimez le contenu de la zone de texte **Script d'ouverture de session**, puis cliquez sur **OK**.

5. Répétez les étapes 3 et 4 pour chaque utilisateur.

## <a name="remove-legacy-active-directory-group-policy-objects"></a>Supprimer les anciens objets de stratégie de groupe de répertoire Active
Les objets de stratégie de groupe (GPO) sont mis à jour pour Windows Server Essentials. Ils représentent un sur-ensemble des objets de stratégie de groupe Windows SBS 2003. Pour Windows Server Essentials, un nombre de filtres de la stratégie de groupe Windows SBS 2003 et Windows Management Instrumentation (WMI) doit être supprimé manuellement pour éviter tout conflit avec les stratégie de groupe Windows Server Essentials et de filtres WMI. 

> [!NOTE]
> Si vous avez modifié les objets de stratégie de groupe Windows SBS 2003 d'origine, enregistrez-en des copies à un autre emplacement, puis supprimez-les de Windows SBS 2003.

#### <a name="to-remove-old-group-policy-objects-from-windows-sbs-2003"></a>Pour supprimer les anciens objets de stratégie de groupe de Windows SBS 2003 

1. Ouvrez une session sur le serveur source avec un compte d'administrateur. 

2. Cliquez sur **Démarrer**, puis sur **Gestion de serveur**. 

3. Dans le volet de navigation, cliquez sur **gestion avancée**, cliquez sur **Group Policy Management**, puis cliquez sur **forêt : *** < nom_domaine\>* . 

4. Cliquez sur **domaines**, cliquez sur *< nom_domaine\>* , puis cliquez sur **les objets de stratégie de groupe**. 

5. Cliquez avec le bouton droit sur **Stratégie d'audit de domaine Small Business Server**, sur **Supprimer**, puis sur **OK**. 

6. Répétez l'étape 5 pour supprimer les objets de stratégie de groupe suivants qui s'appliquent à votre réseau : 

 - Ordinateur client Small Business Server 

 - Stratégie de mot de passe de domaine Small Business Server 

Nous vous recommandons de que configurer la stratégie de mot de passe dans Windows Server Essentials pour appliquer des mots de passe forts. Pour configurer la stratégie de mot de passe, utilisez le tableau de bord, qui écrit la configuration dans la stratégie de domaine par défaut. La configuration de la stratégie de mot de passe n'est pas écrite dans l'objet de stratégie de mot de passe de domaine Small Business Server, comme c'était le cas dans Windows SBS 2003. 

 - Pare-feu de connexion Internet Small Business Server 

 - Stratégie de verrouillage Small Business Server 

 - Stratégie d'assistance à distance Small Business Server 

 - Pare-feu Windows Small Business Server 

 - Stratégie de l'ordinateur client Small Business Server Update Services 

 Cet objet de stratégie de groupe sera présent si vous effectuez une migration depuis Windows SBS 2003 R2. 

 - Stratégie des paramètres communs Small Business Server Update Services 

 Cet objet de stratégie de groupe sera présent si vous effectuez une migration depuis Windows SBS 2003 R2. 
 
 - Stratégie de l'ordinateur serveur Small Business Server Update Services 
 
 Cet objet de stratégie de groupe sera présent si vous effectuez une migration depuis Windows SBS 2003 R2.

7. Vérifiez que tous les objets de stratégie de groupe sont supprimés.

#### <a name="to-remove-wmi-filters-from-windows-sbs-2003"></a>Pour supprimer les filtres WMI de Windows SBS 2003

1. Ouvrez une session sur le serveur source avec un compte d'administrateur.

2. Cliquez sur **Démarrer**, puis sur **Gestion de serveur**.

3. Dans le volet de navigation, cliquez sur **gestion avancée**, cliquez sur **Group Policy Management**, puis cliquez sur **forêt : *** < Nomdomainedevotreréseau\>*

4. Cliquez sur **domaines**, cliquez sur *< Nomdomainedevotreréseau\>* , puis cliquez sur **filtres WMI**.

5. Cliquez avec le bouton droit sur **PostSP2**, cliquez sur **Supprimer**, puis sur **Oui**.

6. Cliquez avec le bouton droit sur **PreSP2**, cliquez sur **Supprimer**, puis cliquez sur **Oui**.

7. Confirmez la suppression de ces trois filtres WMI.

## <a name="configure-the-network"></a>Configurer le réseau

#### <a name="to-configure-the-network"></a>Pour configurer le réseau 

1. Sur le serveur de destination, ouvrez le tableau de bord. 

2. Dans la page d'**accueil** du tableau de bord, cliquez sur **CONFIGURATION**, sur **Configurer l'Accès en tout lieu**, puis choisissez l'option **Cliquez pour configurer l'Accès en tout lieu**. 

3. Suivez les instructions de l'Assistant **Configurer l'Accès en tout lieu** pour configurer votre routeur et votre nom de domaine.

 Si votre routeur ne prend pas en charge l'infrastructure UPnP, ou si l'infrastructure UPnP est désactivée, une icône d'avertissement jaune peut apparaître en regard du nom du routeur. Vérifiez que les ports suivants sont ouverts et qu'ils sont dirigés vers l'adresse IP du serveur de destination :

- Port 80 : Trafic Web HTTP

- Port 443 : Trafic Web HTTPS

> [!NOTE]
> Si vous avez configuré un serveur Exchange local sur un second serveur, vous devez vérifier que le port 25 (SMTP) est également ouvert et qu'il est redirigé vers l'adresse IP du serveur Exchange local.

## <a name="map-permitted-computers-to-user-accounts"></a>Mapper les ordinateurs autorisés aux comptes d'utilisateur
 Dans Windows SBS 2003, si un utilisateur se connecte à l'accès web à distance, tous les ordinateurs du réseau sont affichés. Cela peut inclure des ordinateurs auxquels l'utilisateur n'est pas autorisé à accéder. Dans Windows Server Essentials, un utilisateur doit être explicitement affecté à un ordinateur pour qu’il s’affiche dans l’accès Web à distance. Chaque compte d'utilisateur qui est migré à partir de Windows SBS 2003 doit être mappé à un ou plusieurs ordinateurs. 

#### <a name="to-map-user-accounts-to-computers"></a>Pour mapper les comptes d'utilisateur aux ordinateurs 

1. Sur le serveur de Destination, ouvrez le tableau de bord Windows Server Essentials. 

2. Dans la barre de navigation, cliquez sur **Utilisateurs**. 

3. Dans la liste des comptes d'utilisateur, cliquez avec le bouton droit sur un compte d'utilisateur, puis cliquez sur **Afficher les propriétés du compte**. 

4. Cliquez sur l'onglet **Accès en tout lieu**, puis sur **Autoriser l'accès web à distance et l'accès aux applications de services Web**. 

5. Cliquez sur **Dossiers partagés**, sur **Ordinateurs**, sur **Liens vers la page d'accueil**, puis sur **Appliquer**. 

6. Cliquez sur l'onglet **Accès à l'ordinateur** , puis sur le nom de l'ordinateur auquel vous souhaitez autoriser l'accès. 

7. Répétez les étapes 3, 4, 5 et 6 pour chaque compte d'utilisateur. 

> [!NOTE]
> Vous n'avez pas besoin de modifier la configuration de l'ordinateur client. Il est configuré automatiquement.
>
> Après avoir effectué la migration, si vous rencontrez un problème quand vous créez le premier compte d'utilisateur sur le serveur de destination, supprimez le compte d'utilisateur que vous avez ajouté, puis recréez-le.
