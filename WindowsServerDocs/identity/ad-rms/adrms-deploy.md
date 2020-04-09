---
ms.assetid: e6fa9069-ec9c-4615-b266-957194b49e11
title: Mise à niveau de AD RMS vers Windows Server 2016
author: msmbaldwin
ms.author: esaggese
ms.date: 05/30/2019
ms.prod: windows-server
ms.topic: article
ms.openlocfilehash: 88af85f8e670b9c23b503e0f79af2ce8f10d045e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854852"
---
# <a name="upgrading-ad-rms-to-windows-server-2016"></a>Mise à niveau de AD RMS vers Windows Server 2016

## <a name="introduction"></a>Introduction

Services AD RMS (Active Directory Rights Management Services) (AD RMS) est un service Microsoft qui protège les documents et e-mails sensibles. Contrairement aux méthodes de protection traditionnelles, telles que les pare-feu et les listes de contrôle d’accès, AD RMS le chiffrement et la protection sont persistants, quel que soit l’emplacement d’un fichier ou son mode de transport. 

Ce document fournit des conseils pour la migration de Windows Server 2012 R2 avec SQL Server 2012 vers Windows Server 2016 et SQL Server 2016. Le même processus peut être utilisé pour effectuer une migration à partir de versions antérieures mais prises en charge de AD RMS.
Notez que services AD RMS (Active Directory Rights Management Services) n’est plus en cours de développement actif et pour les dernières fonctionnalités, les clients doivent envisager la migration vers [Azure information protection](https://azure.microsoft.com/services/information-protection/), qui offre un ensemble de fonctionnalités bien plus complet avec une prise en charge des appareils et des applications plus complète. 

Pour plus d’informations sur la migration vers Azure Information Protection à partir de AD RMS sans avoir à protéger à nouveau votre contenu, consultez [la documentation sur la migration de Azure information protection](https://docs.microsoft.com/azure/information-protection/migrate-from-ad-rms-to-azure-rms).

## <a name="about-the-environment-used-in-this-guide"></a>À propos de l’environnement utilisé dans ce guide

AD FS est un composant facultatif d’une installation AD RMS. Dans ce guide, l’utilisation d’ADFS est supposée être utilisée. Si ADFS n’a pas été utilisé dans votre environnement pour prendre en charge les utilisateurs AD RMS, vous pouvez ignorer toutes les étapes qui font référence à ADFS.

Dans ce guide, SQL Server est mis à niveau vers SQL Server 2016 en effectuant une installation parallèle et en déplaçant les bases de données via une sauvegarde. Si vous pouvez également mettre à niveau vos serveurs de base de données AD RMS et ADFS vers SQL Server 2016 sur place, vous pouvez passer à la section suivante de ce document après avoir effectué cette opération sans avoir à suivre les étapes de cette section.  

## <a name="installation"></a>Installation

### <a name="configuring-sql-server-2016"></a>Configuration de SQL Server 2016

La section suivante détaille les tâches d’implémentation associées directement à la configuration SQL Server 2016. Ce guide se concentre sur l’utilisation de la Gestionnaire de serveur et des SQL Server Management Studio pour effectuer ces tâches.

Ces étapes doivent être effectuées sur une installation SQL Server 2016. Installez SQL Server 2016 sur un matériel approprié conformément aux pratiques et stratégies standard de votre organisation. 

#### <a name="preparing-the-sql-server"></a>Préparation de l’SQL Server

La section suivante explique comment préparer le SQL Server afin qu’il puisse être mis à niveau vers SQL Server 2016 avant de mettre à niveau d’autres services de la plateforme AD RMS pour utiliser Windows Server 2016.

##### <a name="adding-cname-for-sql-server-2016-to-dns"></a>Ajout de CNAMe pour SQL Server 2016 à DNS

L’enregistrement CNAME est utilisé pour garantir que le programme d’installation de Windows Server 2016 obtient les données appropriées, car il pointe vers le nouveau SQL Server 2016. **Remarque : Si vous utilisez déjà un enregistrement CNAME pour le service ADFS et AD RMS, vous pouvez passer aux étapes suivantes.**


**Pour ajouter un enregistrement CNAME pour SQL Server 2016 à DNS**

1.  Connectez-vous au contrôleur de domaine Windows Server 2012 R2 avec les informations d’identification d’administrateur de domaine.

2.  Ouvrez Gestionnaire de serveur.

3.  Cliquez sur **Outils** et sélectionnez **DNS** pour ouvrir le Gestionnaire DNS.

4.  Dans le volet de navigation gauche, développez le DC et ouvrez les **zones de recherche directe**.

5.  Ouvrez les ressources de domaine appropriées, cliquez avec le bouton droit dans le volet d’affichage de droite, puis sélectionnez **Nouvel alias (CNAME)** pour commencer à créer l’enregistrement CNAME.

6.  Pour le nom d’alias, entrez un nom logique pour le différencier des autres qui peuvent être présents (par exemple, SQLADRMS ou SQLADFS)

7.  Après avoir entré le nom, indiquez le nom de domaine complet de l’hôte cible qui sera le nouveau serveur SQL Server 2016. ex. SQL2016.contoso.com)

8.  Une fois toutes les informations entrées, cliquez sur **OK**.

##### <a name="backup-the-ad-rms-and-adfs-databases"></a>Sauvegarder les bases de données AD RMS et ADFS

Les bases de données AD RMS et ADFS contiennent les informations critiques nécessaires pour AD RMS, telles que la clé publique du certificat de licence serveur, les modèles de stratégie de droits, les données de configuration ADFS et les informations de journalisation. Sans ces bases de données, les clients ne peuvent pas émettre de licences pour consommer du contenu protégé, entre autres problèmes.

Parmi les bases de données, la base de données de configuration AD RMS est considérée comme la plus importante, car elle stocke les modèles de stratégie de licence, les modèles de stratégie de droits, les clés des utilisateurs et les informations de configuration. Par conséquent, bien qu’il soit conseillé de sauvegarder toutes les bases de données AD RMS et ADFS, vous devez planifier la sauvegarde régulière de la base de données de configuration.

La base de données de journalisation stocke les informations sur les demandes des utilisateurs auprès du cluster AD RMS pour les certificats et les licences d’utilisation. Votre stratégie de sauvegarde de cette base de données doit être basée sur la stratégie de l’entreprise pour conserver ce type d’informations.

La base de données des services d’annuaire n’est pas critique pour AD RMS fonctionnalité et, si les données les plus récentes sont perdues, la base de données est à nouveau remplie avec les informations lorsque le serveur AD RMS reçoit des demandes de certificats et utilise des licences. Vous n’avez pas besoin de sauvegarder cette base de données régulièrement, mais vous devez disposer au moins d’une copie de la base de données telle qu’elle a été configurée à l’origine après le déploiement de AD RMS.

**Pour sauvegarder une base de données AD RMS et/ou ADFS avec Microsoft SQL Server**

1.  Connectez-vous au serveur de base de données Windows Server 2012 R2 AD RMS avec SQL 2012.

2.  Cliquez sur **Démarrer**, sur **tous les programmes**, sur **Microsoft SQL Server**, puis sur **SQL Server Management Studio**.

3.  Dans la fenêtre **se connecter au serveur** , confirmez que le serveur hébergeant les bases de données AD RMS se trouve dans la zone **nom du serveur** , puis cliquez sur **se connecter**.

4.  Développez **bases de données**. Cliquez avec le bouton droit sur la base de données appropriée (**DRM** et **ADFS**), pointez sur **tâches**, puis sélectionnez **sauvegarde**.

5.  Répétez l’étape 4 pour les bases de données restantes.

6.  Assurez-vous que la sauvegarde des bases de données est accessible par d’autres ordinateurs sur le réseau ou à l’aide d’un dispositif de stockage, car ils seront nécessaires pour les étapes ultérieures pendant la migration.

Vous pouvez désormais stocker les copies de base de données dans un emplacement sécurisé. N’oubliez pas de sauvegarder régulièrement vos bases de données.

##### <a name="adding-domain-admin-sql-ad-rms-andor-adfs-service-account-to-sql-server-2016"></a>Ajout d’un compte de service d’administration de domaine, SQL, AD RMS et/ou ADFS à SQL Server 2016

Les étapes suivantes montrent comment ajouter les différents comptes de service à SQL Server 2016 pour faciliter la migration des données à partir de l’environnement Windows Server 2012 R2. Vous obtiendrez ainsi les autorisations adéquates lors de la tentative d’accès au contenu et de gestion des données.

**Pour ajouter le compte de service d’administration de domaine, SQL, AD RMS et/ou ADFS à SQL Server**

1.  Connectez-vous au serveur avec SQL Server 2016 en tant que compte d’administrateur local.

2.  Cliquez sur **Démarrer**, sur **tous les programmes**, sur **Microsoft SQL Server**, puis sur **SQL Server Management Studio**.

3.  Dans la fenêtre **se connecter au serveur** , confirmez que le serveur hébergeant les bases de données AD RMS se trouve dans la zone **nom du serveur** , puis pour authentification, cliquez sur le menu déroulant et sélectionnez **authentification SQL Server**.

4.  Dans le champ **connexion** , entrez le nom du compte d’administrateur local (par exemple, LocalAdmin), puis fournissez le mot de passe approprié, puis cliquez sur **se connecter**.

5.  Développez **sécurité** , cliquez avec le bouton droit sur **connexions** et sélectionnez **nouvelle connexion** dans le menu contextuel qui s’affiche.

6.  Une fois que la fenêtre apparaît dans le champ **nom de connexion** du compte d’administrateur de domaine (par exemple, Contoso\\ContosoAdmin)

7.  Dans le volet de navigation gauche, choisissez **rôles de serveur**.

8.  Activez ensuite la case à cocher **sysadmin** sous les rôles serveur, puis cliquez sur **OK**.

9.  Redémarrez la **gestion des SQL Server**.

10. Dans la fenêtre **se connecter au serveur** , confirmez que le serveur hébergeant les bases de données AD RMS se trouve dans la zone **nom du serveur** , puis pour authentification, cliquez sur le menu déroulant et sélectionnez **authentification Windows** , puis cliquez sur **se connecter**.

##### <a name="restoring-the-ad-rms-and-adfs-databases-to-sql-server-2016"></a>Restauration des bases de données AD RMS et ADFS sur SQL Server 2016

Les étapes suivantes illustrent la restauration des données de l’instance de SQL Server précédente vers la nouvelle instance 2016. Cela permettra à la nouvelle base de données SQL d’utiliser les données de configuration pertinentes des précédentes AD RMS et des bases de données ADFS.

**Pour restaurer les données de l’SQL Server précédent vers le nouveau SQL Server**

1.  Connectez-vous au serveur avec SQL Server 2016 avec le compte approprié.

2.  Dans le volet de navigation gauche, cliquez avec le bouton droit sur **bases de données** , puis sélectionnez **restaurer la base de données** pour commencer le processus de restauration.

3.  Sous **source** , choisissez **appareil** , puis recherchez l’emplacement où les fichiers de base de données ont été stockés dans les étapes précédentes.

4.  Une fois les fichiers sélectionnés, cliquez sur **OK**.

5.  Assurez-vous que tous les fichiers de base de données ont été ajoutés et terminez le processus en cliquant sur **OK**.

### <a name="configuring-windows-server-2016-active-directory-federation-services-ad-fs"></a>Configuration de Windows Server 2016 Services ADFS (AD FS)

AD FS a été déployé pour fournir un accès par authentification unique (SSO) à AD RMS en tant qu’application. Elle a également été configurée avec l’extension de périphérique mobile (MDE) AD RMS, qui permet la prise en charge des appareils Mac et des appareils mobiles pour les utilisateurs finaux.

Les sections suivantes fournissent des conseils sur les tâches opérationnelles que vous devrez peut-être effectuer sur votre déploiement AD FS.

#### <a name="adding-a-2016-ad-fs-server-to-the-farm"></a>Ajout d’un serveur de AD FS 2016 à la batterie de serveurs

Vous pouvez déployer des serveurs de AD FS supplémentaires pour prendre en charge le déploiement AD RMS. Vous pouvez choisir d’effectuer cette action en cas d’augmentation du trafic vers les serveurs AD RMS, ou vers des applications supplémentaires, ou si vous devez mettre hors service l’un des serveurs en cours d’utilisation pour AD FS.

**Pour ajouter le serveur de AD FS 2016 à la batterie de serveurs**

1.  Sur le serveur Azure AD Connect, double-cliquez sur l’icône **Azure ad Connect** pour lancer l’Assistant Azure ad Connect.

2.  Dans la page d’accueil, cliquez sur **configurer**.

3.  Dans la page tâches supplémentaires, cliquez sur **déployer un serveur de Fédération supplémentaire** , puis cliquez sur **suivant**.

4.  Dans la page se connecter à Azure AD, entrez le nom d’utilisateur et le mot de passe d’un compte disposant d’autorisations d’administrateur globales, puis cliquez sur **suivant**.

5.  Dans la page informations d’identification de l’administrateur de domaine, entrez le nom d’utilisateur et le mot de passe d’un compte disposant d’autorisations d’administrateur de domaine, puis cliquez sur **suivant**.

6.  Cliquez sur **Parcourir** et sélectionnez le fichier de certificat utilisé lors de la configuration de la batterie de AD FS à l’aide du Azure ad Connect.

7.  Cliquez sur **entrer le mot de passe** pour ouvrir la boîte de dialogue mot de passe du certificat.

8.  Entrez le mot de passe du certificat dans le champ mot de passe, puis cliquez sur **OK**.

9.  Cliquez sur **Suivant**.

10. Dans la page serveurs AD FS, entrez le nom ou l’adresse IP du nouveau serveur de AD FS, puis cliquez sur **Ajouter**.

11. Dans la page prêt à configurer, cliquez sur **installer**.

12. Dans la page installation terminée, cliquez sur **quitter**.

#### <a name="raising-the-adfs-farm-behavior-level"></a>Augmentation du niveau de comportement de la batterie de serveurs ADFS

Lors du déploiement d’un serveur ADFs qui dépasse le niveau d’environnement actuel, par exemple, si vous disposez d’un ADFS sur Windows Server 2012 R2 et que vous ajoutez ensuite un serveur Windows Server 2016 ADFS, le niveau de comportement de la batterie de serveurs doit être augmenté. Cela est nécessaire pour garantir que l’environnement utilise les informations et fonctions les plus récentes.

**Pour augmenter le niveau de comportement de la batterie de serveurs ADFS**

1.  Accédez au Windows Server 2016 ADFS.

2.  Ouvrez une session PowerShell d’administration.

3.  Entrez la commande suivante : **\$cred = obtient-Credential**

4.  Une fenêtre s’affiche pour vous demander les informations d’identification, entrez les informations d’identification d’administrateur de domaine.

5.  Ensuite, entrez la commande suivante : **Invoke-AdfsFarmBehaviorLevelRaise-Credential \$cred**

6.  Une invite s’affiche, **voulez-vous continuer cette opération ?** Ensuite, entrez **un** pour accepter l’invite.

7.  Une fois la commande terminée, le niveau de comportement de la batterie de serveurs sera configuré et prêt.

#### <a name="enabling-mobile-device-extension-logging"></a>Activation de la journalisation des extensions de périphérique mobile

L’extension d’appareil mobile peut consigner les requêtes qu’elle reçoit des appareils des utilisateurs finaux. La journalisation est désactivée par défaut et nous vous recommandons d’activer uniquement la journalisation dans un scénario de dépannage. Toutes les demandes, depuis les appareils mobiles et les ordinateurs de bureau, à la récupération d’amorçage ou à l’acquisition d’une licence d’utilisation finale sont journalisées dans la base de données de journalisation AD RMS ou le compte de stockage Azure. La journalisation MDE crée deux tables supplémentaires dans le SQL Server utilisé par AD RMS : la table du journal de débogage du client et la table du journal des performances du client.

**Pour activer la journalisation des extensions de périphérique mobile**

1.  À partir d’un serveur AD RMS, ouvrez Windows PowerShell en tant qu’administrateur.

2.  Tapez la commande suivante et appuyez sur **entrée**: **import-module ADRMSADMIN**

3.  Tapez la commande suivante et appuyez sur **entrée**: **New-PSDrive-Name AdrmsCluster-PSProvider AdRmsAdmin-root https://localhost**

4.  Tapez la commande suivante et appuyez sur **entrée**: **Set-ItemProperty-Path AdrmsCluster :\\-Name IsLoggingEnabled-value \$true**

Si vous utilisez la journalisation MDE pour la résolution des problèmes, nous vous recommandons de la désactiver après avoir résolu le problème.

**Pour désactiver la journalisation des extensions de périphérique mobile**

1.  À partir d’un serveur AD RMS, ouvrez Windows PowerShell en tant qu’administrateur.

2.  Tapez la commande suivante et appuyez sur **entrée**: **import-module ADRMSADMIN**

3.  Tapez la commande suivante et appuyez sur **entrée**: **New-PSDrive-Name AdrmsCluster-PSProvider AdRmsAdmin-root https://localhost**

4.  Tapez la commande suivante et appuyez sur **entrée**: **Set-ItemProperty-Path AdrmsCluster :\\-Name IsLoggingEnabled-value \$false**

### <a name="upgrading-ad-rms-to-windows-server-2016"></a>Mise à niveau de AD RMS vers Windows Server 2016

Les sections suivantes fournissent des conseils sur la façon d’ajouter un serveur de AD RMS Windows Server 2016 au cluster Windows Server 2012 R2 actuel. Le serveur sera ajouté au cluster et les informations seront répliquées vers ce dernier afin que le serveur de AD RMS précédent puisse être déprécié pour libérer des ressources.

Après l’ajout d’un serveur de AD RMS Windows Server 2016 qui a été ajouté à votre cluster AD RMS, tous les nœuds basés sur des versions antérieures de Windows deviennent inactifs. Une fois cette opération effectuée, vous pouvez annuler l’approvisionnement de ces serveurs (par exemple, arrêter, réaffecter ou réinstaller avec Windows Server 2016 pour rejoindre le cluster AD RMS). 

Vous pouvez déployer des serveurs de AD RMS supplémentaires sur le cluster pour prendre en charge la charge sur votre déploiement AD RMS. Vous pouvez également choisir d’effectuer cette action en cas d’augmentation du trafic vers les serveurs AD RMS.

Ce guide ne couvre pas les étapes nécessaires à la modification des mécanismes d’équilibrage de charge que vous utilisez dans votre environnement pour exclure les serveurs que vous désapprouvez et pour inclure ceux que vous ajoutez au cluster. 

#### <a name="adding-a-2016-ad-rms-server"></a>Ajout d’un serveur de AD RMS 2016

Si votre cluster AD RMS utilise un module de sécurité matériel au lieu d’une clé gérée de manière centralisée pour son certificat de licence serveur, vous devez installer le logiciel et d’autres artefacts HSM (par exemple, les fichiers Key et configuragtion) sur le serveur avant d’installer AD RMS. Vous devrez également connecter le HSM au serveur, que ce soit physiquement ou par le biais des configurations réseau appropriées. Suivez les instructions de votre module HSM pour suivre ces étapes. 

**Pour ajouter un serveur de AD RMS 2016**

1.  Installez le rôle AD RMS sur le déploiement Windows Server 2016 souhaité.

2.  Une fois l’installation terminée, sélectionnez le lien pour **effectuer une configuration supplémentaire**.

3.  Sélectionnez **joindre un cluster AD RMS existant** , puis cliquez sur **suivant**.

4.  Dans la page **Sélectionner la base de données de configuration** , entrez le CNAME spécifié dans le DNS pour le serveur SQL Server 2016 (nom de domaine complet).

5.  Cliquez sur **liste** sur la deuxième ligne et sélectionnez le **DefaultInstance** dans la liste déroulante.

6.  Sous **nom de la base de données de configuration**, sélectionnez le menu déroulant et choisissez la configuration DRM qui s’affiche. Ensuite, cliquez sur **Suivant**.

7.  Dans la page informations sur la **base de données** , entrez le mot de passe de clé de cluster dans le champ prévu à cet effet. Après cela, cliquez sur **suivant**.

8.  Dans la page suivante de l’Assistant, spécifiez le compte de service AD RMS et fournissez le mot de passe pour celui-ci, puis cliquez sur **suivant** une fois qu’il a été vérifié.

9.  Une fois la page **site Web du cluster** affichée, vérifiez simplement que le site Web approprié a été sélectionné, puis cliquez sur **suivant**.

10. Sur la page **choisir un certificat d’authentification serveur** , sélectionnez le certificat SSL importé, puis cliquez sur **suivant**.

11. Cliquez sur **Installer** pour commencer l'installation.

12. Une fois la configuration terminée, vous devez vous déconnecter et vous reconnecter pour administrer AD RMS.

13. Une fois que vous êtes connecté, ouvrez **Gestionnaire de serveur** sélectionnez **outils** , puis **Active Directory Rights Management**. La fenêtre de gestion doit s’afficher et indiquer que le cluster a le serveur supplémentaire dans le cluster.

14. Si l’extension d’appareil mobile AD RMS a été installée dans le cluster de AD RMS d’origine, vous devez également installer le fichier MDE dans les nœuds de cluster mis à jour. Suivez les instructions de la documentation MDE pour ajouter MDE à votre cluster AD RMS. À ce stade, vous pouvez réaffecter tous les nœuds préexistants ou les mettre à niveau vers Windows Server 2016 et les rejoindre au cluster AD RMS à l’aide du processus décrit ci-dessus. 

### <a name="configuring-windows-server-2016-web-application-proxy-wap"></a>Configuration du proxy d’application Web (WAP) Windows Server 2016

Les sections suivantes fournissent des conseils sur les tâches opérationnelles que vous devrez peut-être effectuer sur votre déploiement de proxy d’application Web. Il s’agit d’une étape facultative, qui n’est pas obligatoire si vous publiez des AD RMS sur Internet via d’autres mécanismes. 

#### <a name="adding-a-windows-server-2016-wap-server"></a>Ajout d’un serveur WAP Windows Server 2016

Vous pouvez déployer des serveurs proxy d’application Web supplémentaires pour prendre en charge le déploiement AD RMS. Vous pouvez choisir d’effectuer cette action en cas d’augmentation du trafic vers les serveurs AD RMS ou si vous devez mettre hors service l’un des serveurs en cours d’utilisation pour le proxy d’application Web.

**Pour ajouter un serveur proxy d’application Web 2016**

1.  À partir du serveur que vous souhaitez configurer en tant que proxy d’application Web, accédez à la console Gestionnaire de serveur, puis cliquez sur **Ajouter des rôles et des fonctionnalités**.

2.  Dans l' **Assistant Ajout de rôles et de fonctionnalités**, cliquez sur **suivant** jusqu’à ce que vous obteniez l’écran de sélection du rôle de serveur.

3.  Dans l’écran Sélectionner des rôles de serveurs, sélectionnez **accès à distance**, puis cliquez sur **suivant** jusqu’à ce que vous revenez à l’écran Sélectionner des rôles de serveurs.

4.  Dans l’écran Sélectionner des rôles de serveurs, sélectionnez **proxy d’application Web**, cliquez sur **Ajouter des fonctionnalités**, puis cliquez sur **suivant**.

5.  Dans l’écran confirmer les sélections d’installation, cliquez sur **installer**.

6.  Une fois l’installation terminée, cliquez sur **Fermer**.

7.  Il est maintenant temps de configurer le serveur. Pour ce faire, ouvrez la console de gestion de l’accès à distance sur le serveur proxy d’application Web. Ouvrez le menu **Démarrer** , tapez **RAMgmtUI. exe**, puis sélectionnez l’application.

8.  Dans le volet de navigation, cliquez sur **Proxy d'application web**.

9.  Dans la console Gestion de l’accès à distance, cliquez sur **exécuter l’Assistant Configuration du proxy d’application Web**. Une fois dans l’Assistant, cliquez sur **suivant**.

10. Dans l’écran serveur de Fédération, entrez le nom de domaine complet du serveur AD FS (par exemple, adfs.contoso.com), puis entrez les informations d’identification d’un administrateur sur le serveur de AD FS.

11. Dans l’écran AD FS certificat de proxy, dans la liste des certificats actuellement installés sur le serveur proxy d’application Web, sélectionnez un certificat à utiliser par le proxy d’application Web pour AD FS proxy, puis cliquez sur **suivant**.

12. Dans l’écran de confirmation, passez en revue les paramètres, puis cliquez sur **configurer**.

13. Une fois la configuration terminée, cliquez sur **Fermer**.

#### <a name="dns-configuration-for-2016-wap-server"></a>Configuration DNS pour le serveur WAP 2016

Une fois que le serveur proxy d’application Web Windows Server 2016 a été mis en place, certaines modifications DNS doivent être apportées. Cela nécessite l’utilisation d’un service tel que GoDaddy pour faire pointer les services ADFS et AD RMS sur le serveur WAP 2016.

**Pour faire pointer le DNS sur le serveur WAP**

1.  Accédez au site Web de votre fournisseur (par exemple, GoDaddy).

2.  Accédez à gestion de domaine, puis à gestion DNS.

3.  Localisez le service ADFS et AD RMS et remplacez les **points** par l’adresse IP publique du serveur WAP 2016 et **Enregistrez**.

4.  La propagation des modifications peut prendre du temps, mais une fois cette installation terminée, cette opération est effectuée.

#### <a name="enabling-debugging-logs"></a>Activation des journaux de débogage

Les informations de journalisation détaillées sont disponibles sur les serveurs proxy d’application Web. Vous pouvez configurer la journalisation avancée du débogage à l’aide de l’observateur d’événements. Vous pouvez également sélectionner des paramètres supplémentaires pour la taille des journaux afin de garantir que l’analyse est utile pour la visionneuse.

**Activation des journaux de débogage pour le proxy d’application Web**

1.  Ouvrez la console **Observateur d’événements** sur le proxy d’application Web.

2.  Développez le nœud **Microsoft** .

3.  Développez le nœud **Windows** .

4.  Ouvrez les journaux du **proxy d’application Web** .

5.  Vous pourrez alors ouvrir les journaux d' **administration** .

6.  Ouvrez le menu **action** , situé dans le coin supérieur gauche, puis sélectionnez **Propriétés**.

7.  Sous l’onglet **général** , choisissez l’option permettant d' **activer la journalisation**.

8.  Enfin, vous pouvez personnaliser la taille maximale du journal et ce qui se produit lorsque la taille maximale du journal des événements est atteinte.

### <a name="configuring-high-availability-for-windows-server-2016-services"></a>Configuration de la haute disponibilité pour les services Windows Server 2016

Les sections suivantes fournissent des conseils sur les tâches opérationnelles dont vous pouvez avoir besoin pour configurer votre environnement Windows Server 2016 en haute disponibilité.

#### <a name="adding-a-2016-ad-rms-server-for-high-availability"></a>Ajout d’un serveur de AD RMS 2016 pour la haute disponibilité

Vous pouvez déployer des serveurs de AD RMS supplémentaires pour configurer la haute disponibilité. Vous pouvez choisir d’effectuer cette action en cas d’augmentation du trafic vers les serveurs AD RMS.

**Pour ajouter un serveur de AD RMS 2016 pour la haute disponibilité**

1.  Installez le rôle AD RMS sur le déploiement Windows Server 2016 souhaité.

2.  Une fois l’installation terminée, sélectionnez le lien pour **effectuer une configuration supplémentaire**.

3.  Sélectionnez **joindre un cluster AD RMS existant** , puis cliquez sur **suivant**.

4.  Dans la page **Sélectionner la base de données de configuration** , entrez le CNAME spécifié dans le DNS pour le serveur SQL Server 2016 (nom de domaine complet).

5.  Cliquez sur **liste** sur la deuxième ligne et sélectionnez le **DefaultInstance** dans la liste déroulante.

6.  Sous **nom de la base de données de configuration**, sélectionnez le menu déroulant et choisissez la configuration DRM qui s’affiche. Ensuite, cliquez sur **Suivant**.

7.  Dans la page informations sur la **base de données** , entrez le mot de passe de clé de cluster dans le champ prévu à cet effet. Après cela, cliquez sur **suivant**.

8.  Dans la page suivante de l’Assistant, spécifiez le compte de service AD RMS et fournissez le mot de passe pour celui-ci, puis cliquez sur **suivant** une fois qu’il a été vérifié.

9.  Une fois la page **site Web du cluster** affichée, vérifiez simplement que le site Web approprié a été sélectionné, puis cliquez sur **suivant**.

10. Sur la page **choisir un certificat d’authentification serveur** , sélectionnez le certificat SSL importé, puis cliquez sur **suivant**.

11. Cliquez sur **Installer** pour commencer l'installation.

12. Une fois la configuration terminée, vous devez vous déconnecter et vous reconnecter pour administrer AD RMS.

13. Une fois que vous êtes connecté, ouvrez **Gestionnaire de serveur** sélectionnez **outils** , puis **Active Directory Rights Management**. La fenêtre de gestion doit s’afficher et indiquer que le cluster a le serveur supplémentaire dans le cluster.

14. Après avoir confirmé la configuration du serveur, configurez votre service d’équilibrage de charge pour équilibrer la charge entre les différents serveurs de AD RMS du cluster.

#### <a name="adding-a-windows-server-2016-ad-fs-server-for-high-availability"></a>Ajout d’un serveur de AD FS Windows Server 2016 pour la haute disponibilité

Vous pouvez déployer des serveurs de AD FS supplémentaires pour configurer la haute disponibilité. Vous pouvez choisir d’effectuer cette action en cas d’augmentation du trafic vers les serveurs AD FS. **Remarque : après avoir déclenché le niveau de comportement de la batterie de serveurs, une nouvelle entrée de base de données sera entrée dans le SQL Server 2016 (ADFS Configv3) et l’ancienne base de données de configuration doit être supprimée avant de poursuivre la procédure.**

**Pour ajouter le serveur de AD FS Windows Server 2016 pour la haute disponibilité**

1.  Installez le rôle AD RMS sur le déploiement Windows Server 2016 souhaité.

2.  Une fois l’installation terminée, sélectionnez le lien pour **configurer le service de Fédération sur ce serveur**.

3.  Dans la section Bienvenue de l’Assistant, choisissez l’option permettant d' **Ajouter un serveur de Fédération à une batterie de serveurs de Fédération** , puis cliquez sur **suivant**.

4.  Spécifiez le compte d’administrateur approprié, puis cliquez sur **suivant**.

5.  Dans la page **spécifier la batterie** , sélectionnez l' **emplacement de la base de données pour une batterie de serveurs existante à l’aide de SQL Server** puis entrez le CNAME du service SQL pour le nom d’hôte de la base de données, puis cliquez sur **suivant**.

6.  Dans la zone **spécifier un compte de service** de l’Assistant, entrez les informations d’identification du compte de service AD FS, puis cliquez sur **suivant**.

7.  Dans **options de révision**, cliquez sur **suivant**.

8.  Cliquez sur **configurer** lorsque le bouton devient disponible.

9.  Après la configuration, redémarrez l’ordinateur.

10. Après confirmation de la configuration du serveur, équilibrez la charge des serveurs AD FS en fonction des besoins.

#### <a name="adding-a-windows-server-2016-wap-server-for-high-availability"></a>Ajout d’un serveur WAP Windows Server 2016 pour la haute disponibilité

Vous pouvez déployer des serveurs WAP supplémentaires pour configurer la haute disponibilité. Vous pouvez choisir d’effectuer cette action en cas d’augmentation du trafic vers les serveurs AD RMS.

**Pour ajouter un serveur proxy d’application Web Windows Server 2016 pour la haute disponibilité**

1.  À partir du serveur que vous souhaitez configurer en tant que proxy d’application Web, accédez à la console Gestionnaire de serveur, puis cliquez sur **Ajouter des rôles et des fonctionnalités**.

2.  Dans l' **Assistant Ajout de rôles et de fonctionnalités**, cliquez sur **suivant** jusqu’à ce que vous obteniez l’écran de sélection du rôle de serveur.

3.  Dans l’écran Sélectionner des rôles de serveurs, sélectionnez **accès à distance**, puis cliquez sur **suivant** jusqu’à ce que vous revenez à l’écran Sélectionner des rôles de serveurs.

4.  Dans l’écran Sélectionner des rôles de serveurs, sélectionnez **proxy d’application Web**, cliquez sur **Ajouter des fonctionnalités**, puis cliquez sur **suivant**.

5.  Dans l’écran confirmer les sélections d’installation, cliquez sur **installer**.

6.  Une fois l’installation terminée, cliquez sur **Fermer**.

7.  Il est maintenant temps de configurer le serveur. Pour ce faire, ouvrez la console de gestion de l’accès à distance sur le serveur proxy d’application Web. Ouvrez le menu **Démarrer** , tapez **RAMgmtUI. exe**, puis sélectionnez l’application.

8.  Dans le volet de navigation, cliquez sur **Proxy d'application web**.

9.  Dans la console Gestion de l’accès à distance, cliquez sur **exécuter l’Assistant Configuration du proxy d’application Web**. Une fois dans l’Assistant, cliquez sur **suivant**.

10. Dans l’écran serveur de Fédération, entrez le nom de domaine complet du serveur AD FS (par exemple, adfs.contoso.com), puis entrez les informations d’identification d’un administrateur sur le serveur de AD FS.

11. Dans l’écran AD FS certificat de proxy, dans la liste des certificats actuellement installés sur le serveur proxy d’application Web, sélectionnez un certificat à utiliser par le proxy d’application Web pour AD FS proxy, puis cliquez sur **suivant**.

12. Dans l’écran de confirmation, passez en revue les paramètres, puis cliquez sur **configurer**.

13. Une fois la configuration terminée, cliquez sur **Fermer**.

14. Après confirmation de la configuration du serveur, équilibrez la charge des serveurs WAP dans la zone DMZ.

#### <a name="adding-a-sql-server-2016-node-for-always-on-high-availability"></a>Ajout d’un nœud SQL Server 2016 pour la haute disponibilité Always On

Vous pouvez déployer des serveurs SQL supplémentaires pour configurer Always On haute disponibilité. Vous pouvez choisir d’effectuer cette action en cas d’augmentation du trafic vers les serveurs AD RMS. **Remarque : Assurez-vous que le port entrant 5022 est ouvert sur les deux serveurs SQL.**

**Pour ajouter un serveur SQL Server 2016 pour la haute disponibilité Always On**

1.  À partir du serveur que vous souhaitez configurer en tant que serveur SQL Server 2016 supplémentaire, accédez à la console Gestionnaire de serveur, puis cliquez sur **Ajouter des rôles et des fonctionnalités**.

2.  Cliquez sur **suivant** jusqu’à la boîte de dialogue **Sélectionner des fonctionnalités** .

3.  Activez la case à cocher **clustering avec basculement** . **Remarque : suivez cette étape pour le serveur SQL Server 2016 d’origine afin que les deux serveurs SQL disposent de la fonctionnalité de clustering de basculement.**

4.  Cliquez sur **installer** pour installer la fonctionnalité de clustering de basculement.

5.  À présent, ouvrez **Gestionnaire de serveur** , sélectionnez **outils** , puis **Gestionnaire du cluster de basculement**.

6.  Dans le volet du menu de gauche, cliquez avec le bouton droit sur **Gestionnaire du cluster de basculement** , puis sélectionnez **créer un cluster** .

7.  L' **Assistant Création**d’un cluster s’ouvre.

8.  Recherchez les serveurs SQL Server 2016 qui seront utilisés pour Always On haute disponibilité et entrez-les dans, puis cliquez sur **suivant**.

9.  Vous recevrez un avertissement de validation. Sélectionnez **Oui** pour valider les nœuds de cluster, puis cliquez sur **suivant**.

10. Sous la page **options de test** , sélectionnez l’option **exécuter tous les tests** , puis cliquez sur **suivant.**

11. **Remarque : l’Assistant validation de cluster est supposé renvoyer plusieurs messages d’avertissement, en particulier si vous n’utilisez pas de stockage partagé. En revanche, si vous trouvez des messages d’erreur, vous devez les corriger avant de créer le cluster de basculement Windows Server**.

12. Dans la boîte de dialogue **point d’accès pour l’administration du cluster** , entrez le nom du cluster et l’adresse IP virtuelle du cluster de basculement Windows Server, puis cliquez sur **suivant**.

13. Vérifiez que la configuration a réussi en **Résumé** , puis cliquez sur **Terminer**.

14. Dans le **Gestionnaire du cluster de basculement,** cliquez avec le bouton droit sur votre cluster et sélectionnez **autres actions** , puis choisissez **configurer les paramètres de quorum du cluster** .

15. Cliquez sur **suivant** , puis choisissez l’option pour **Sélectionner le témoin de quorum** , puis appuyez de nouveau sur **suivant** .

16. Dans la page **Sélectionner le témoin de quorum** , sélectionnez l’option **configurer un témoin de partage de fichiers** . Ensuite, cliquez sur **Suivant**.

17. Sélectionnez **Parcourir** et recherchez le chemin d’accès du partage de fichiers que vous souhaitez utiliser dans la boîte de dialogue chemin d’accès au partage de fichiers. Cliquez sur **Suivant**.

18. Dans la page confirmation, cliquez sur **suivant**.

19. Sur la page Résumé, cliquez sur **Terminer**.

20. À présent, ouvrez le menu **Démarrer** et recherchez **Gestionnaire de configuration SQL Server**.

21. Cliquez avec le bouton droit sur le nom du SQL Server et choisissez **Propriétés**.

22. Dans la boîte de dialogue Propriétés, sélectionnez l’onglet **haute disponibilité AlwaysOn** . activez la case à cocher **activer la groupes de disponibilité AlwaysOn** . Cliquez sur **OK**. **Remarque : procédez de la même façon sur les deux serveurs SQL Server 2016.**

23. Ensuite, redémarrez le service SQL Server.

24. À présent, ouvrez le menu **Démarrer** et recherchez **SQL Server Management Studio** et dans le volet de navigation gauche, cliquez avec le bouton droit sur **groupes de disponibilité** , puis cliquez sur **Assistant Nouveau groupe de disponibilité** , puis cliquez sur **suivant**.

25. Dans la page **spécifier le nom du groupe de disponibilité** , choisissez un nom de groupe (par exemple, SQLAvailabilityGroup2016). Ensuite, cliquez sur **Suivant**.

26. Dans la section **Sélectionner des bases de données** , spécifiez les bases de données. Cliquez ensuite sur Suivant. **Remarque : il se peut que certaines bases de données doivent être sauvegardées à nouveau ou mises en mode de récupération complète**.

27. Une fois sur la page **spécifier les réplicas** , cliquez sur le bouton **Ajouter un réplica** et sélectionnez les autres SQL Server 2016.

28. Après avoir ajouté l’autre serveur, activez les cases à cocher et configurez le serveur secondaire de façon à ce qu’il soit accessible en lecture.

29. Accédez à l’onglet **points de terminaison** et cliquez sur l’option **Actualiser** . Bien qu’ici, faites défiler la page et vérifiez que le même compte de service se trouve sur le nœud principal et le nœud secondaire.

30. À présent, choisissez l’onglet **Préférences de sauvegarde** et sélectionnez l’option **préférer secondaire** .

31. Passez à l’onglet **écouteur** .

32. Spécifiez un nom (par exemple, SQLListener) et assurez-vous que le port est **1433** , puis cliquez sur **suivant**.

33. Dans la page **Sélectionner la synchronisation de données initiale** de l’Assistant, choisissez l’option **complète** et spécifiez l’emplacement réseau accessible par tous les serveurs SQL, puis cliquez sur **suivant**.

34. Enfin, cliquez sur **Terminer** pour terminer le processus.

### <a name="decommission-windows-server-2012-r2-nodes"></a>Désactiver les nœuds Windows Server 2012 R2

Les sections suivantes fournissent des conseils sur les tâches opérationnelles dont vous pouvez avoir besoin pour supprimer vos serveurs Windows Server 2012 R2 après avoir correctement mis à niveau le cluster AD RMS vers Windows Server 2016.

#### <a name="removing-a-windows-server-2012-r2-ad-rms-server"></a>Suppression d’un serveur de AD RMS Windows Server 2012 R2

Vous pouvez supprimer les serveurs de AD RMS inutiles après une mise à niveau. Vous pouvez choisir d’effectuer cette action lorsqu’elle est nécessaire pour mettre hors service AD RMS serveurs.

**Pour supprimer un** **serveur de AD RMS Windows Server 2012 R2**

1.  Sur le serveur Windows Server 2012 R2 AD RMS dans Gestionnaire de serveur, sélectionnez **gérer** dans les menus en haut à droite, puis choisissez **supprimer des rôles et des fonctionnalités**.

2.  L' **Assistant Suppression de rôles et de fonctionnalités** s’ouvre et dans l’écran **avant de commencer** , cliquez sur **suivant**.

3.  Dans l’écran **sélection du serveur** , cliquez sur **suivant**.

4.  Dans l’écran **rôles de serveurs** , désactivez la case à cocher en regard de **services AD RMS (Active Directory Rights Management Services)** , puis cliquez sur **suivant**.

5.  Dans l’écran **fonctionnalités** , cliquez sur **suivant**.

6.  Dans l’écran **confirmation** , cliquez sur **supprimer**.

7.  Une fois cette opération terminée, redémarrez le serveur.

8.  Vous pouvez maintenant arrêter ce serveur et réallouer les ressources en fonction des besoins.
