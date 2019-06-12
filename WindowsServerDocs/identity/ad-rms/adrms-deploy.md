---
ms.assetid: e6fa9069-ec9c-4615-b266-957194b49e11
title: La mise à niveau d’AD RMS pour Windows Server 2016
description: ''
author: msmbaldwin
ms.author: esaggese
ms.date: 05/30/2019
ms.topic: article
ms.openlocfilehash: ce058a2885315c84d2c1c6701ad2801790d3c590
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66814072"
---
# <a name="upgrading-ad-rms-to-windows-server-2016"></a>La mise à niveau d’AD RMS pour Windows Server 2016

## <a name="introduction"></a>Introduction

Active Directory Rights Management Services (AD RMS) est un service de Microsoft qui protège les e-mails et documents sensibles. Contrairement aux méthodes de protection traditionnelle, telles que les pare-feux et les ACL, protection et chiffrement AD RMS sont persistants, quel que soit l’emplacement d’un fichier ou le mode de transport. 

Ce document fournit des conseils pour la migration de Windows Server 2012 R2 avec SQL Server 2012 vers Windows Server 2016 et SQL Server 2016. Le même processus peut être utilisé pour migrer des versions antérieures mais pris en charge des services AD RMS.
Veuillez noter que Active Directory Rights Management Services n’est plus en cours de développement, et pour les dernières fonctionnalités clients doivent envisager de migrer vers [Azure Information Protection](https://azure.microsoft.com/services/information-protection/), qui offre une bien plus encore ensemble complet de fonctionnalités avec les appareils plus complète et prise en charge de l’application. 

Pour plus d’informations sur la migration vers Azure Information Protection à partir d’AD RMS, sans avoir à nouveau protéger votre contenu, consultez [la documentation de migration Azure Information Protection](https://docs.microsoft.com/azure/information-protection/migrate-from-ad-rms-to-azure-rms).

## <a name="about-the-environment-used-in-this-guide"></a>À propos de l’environnement utilisé dans ce guide

AD FS est un composant facultatif d’une installation AD RMS. Dans ce guide, l’utilisation d’ADFS est supposée. Si AD FS n’a pas été utilisé dans votre environnement pour prendre en charge des utilisateurs AD RMS, vous pouvez ignorer toutes les étapes qui font référence à AD FS.

Dans ce guide, SQL Server est mis à niveau vers SQL Server 2016 en effectuant une installation parallèle et de basculer les bases de données via une sauvegarde. Vous pouvez également, si vous pouvez mettre à niveau vos serveurs de base de données AD RMS et ADFS pour SQL Server 2016 sur place, vous pouvez déplacer à la section suivante dans ce document après avoir effectué cette tâche sans avoir à suivre les étapes décrites dans cette section.  

## <a name="installation"></a>Installation

### <a name="configuring-sql-server-2016"></a>Configuration de SQL Server 2016

Section Détails de l’implémentation suit directement liées à la configuration de SQL Server 2016. Ce guide se concentre sur l’utilisation du Gestionnaire de serveur et de SQL Server Management Studio pour effectuer ces tâches.

Ces étapes doivent être effectuées sur une installation de SQL Server 2016. Installer SQL Server 2016 sur du matériel approprié selon les stratégies et les méthodes standard de votre organisation. 

#### <a name="preparing-the-sql-server"></a>Préparation du serveur SQL

La section suivante décrit en détail comment préparer le serveur SQL Server afin qu’il peut être mis à niveau vers SQL Server 2016 avant la mise à niveau d’autres services dans la plateforme d’AD RMS à utiliser Windows Server 2016.

##### <a name="adding-cname-for-sql-server-2016-to-dns"></a>Ajoute l’enregistrement CNAME pour SQL Server 2016 au serveur DNS

L’enregistrement CNAME est utilisé pour garantir que le programme d’installation de Windows Server 2016 obtiendra les données appropriées dans la mesure où il sera pointer vers le nouveau SQL Server 2016. **Remarque : Si vous utilisez déjà un enregistrement CNAME pour le service RMS AD FS et AD, vous pouvez passer aux étapes suivantes.**


**Pour ajouter un enregistrement CNAME pour SQL Server 2016 à DNS**

1.  Ouvrez une session le contrôleur de domaine Windows Server 2012 R2 avec les informations d’identification administrateur de domaine.

2.  Ouvrez le Gestionnaire de serveur.

3.  Cliquez sur **outils** et sélectionnez **DNS** pour ouvrir le Gestionnaire DNS.

4.  Dans le volet de navigation de gauche, développez le contrôleur de domaine et ouvrez **Zones de recherche directe**.

5.  Ouvrir les ressources de domaine approprié, puis cliquez avec le bouton droit dans le volet de droite et sélectionnez **nouvel Alias (CNAME)** pour commencer à créer l’enregistrement CNAME.

6.  Pour, entrez le nom d’alias dans un nom logique pour le différencier des autres qui peut-être être présent (Ex. SQLADRMS ou SQLADFS)

7.  Après avoir entré le nom, indiquez le nom de domaine complet pour l’hôte cible qui sera le nouveau serveur SQL Server 2016. (par ex. SQL2016.contoso.com)

8.  Une fois toutes les informations entrées, cliquez sur **OK**.

##### <a name="backup-the-ad-rms-and-adfs-databases"></a>Les services AD RMS et ADFS de bases de données de sauvegarde

Les bases de données AD RMS et ADFS contiennent des informations critiques nécessaires aux services AD RMS, telles que la clé publique du certificat de licence serveur, les modèles de stratégie de droits, les données de configuration AD FS, les informations de journalisation. Sans ces bases de données, les clients ne peuvent pas émettre des licences pour consommer du contenu protégé, entre autres.

Les bases de données, la base de données de configuration AD RMS est considérée comme la plus importante, car il stocke le SLC, droits des modèles de stratégie, leurs clés et informations de configuration. Par conséquent, bien que vous devez prendre soin de sauvegarder toutes les bases de données AD RMS et ADFS, pensez à sauvegarder régulièrement de la base de données de configuration.

La base de données de journalisation stocke des informations sur les demandes utilisateur vers le cluster AD RMS pour les certificats et les licences d’utilisation. Votre stratégie de sauvegarde de cette base de données doit être basée sur la stratégie d’entreprise en conservant ce type d’information.

La base de données des services de répertoire n’est pas critique pour les fonctionnalités d’AD RMS et, en cas de perte de données les plus récentes, la base de données sera remplir à nouveau avec des informations comme le serveur AD RMS reçoit des demandes de certificats et les licences d’utilisation. Vous n’avez pas besoin de sauvegarder cette base de données régulièrement, mais vous n’avez pas besoin d’avoir au moins une copie de la base de données tel qu’il a été initialement configuré après le déploiement d’AD RMS.

**Pour sauvegarder une base de données AD RMS et/ou ADFS avec Microsoft SQL Server**

1.  Ouvrez une session le serveur de base de données de Windows Server 2012 R2 AD RMS avec SQL 2012.

2.  Cliquez sur **Démarrer**, cliquez sur **tous les programmes**, cliquez sur **Microsoft SQL Server**, puis cliquez sur **SQL Server Management Studio**.

3.  Dans le **se connecter au serveur** fenêtre, vérifiez le serveur qui héberge les bases de données AD RMS est dans le **nom du serveur** , puis cliquez sur **Connect**.

4.  Développez **bases de données**. Cliquez avec le bouton droit sur la base de données approprié (**DRM** et **Adfs**), pointez sur **tâches**, puis sélectionnez **sauvegarde**.

5.  Répétez l’étape 4 pour les bases de données.

6.  Assurez-vous que la sauvegarde des bases de données est accessible par d’autres ordinateurs sur le réseau ou à l’aide d’un périphérique de stockage comme ils vous seront utiles pour les étapes ultérieures pendant la migration.

Vous pouvez désormais stocker les copies de base de données dans un emplacement sécurisé. Pensez à sauvegarder vos bases de données fréquemment.

##### <a name="adding-domain-admin-sql-ad-rms-andor-adfs-service-account-to-sql-server-2016"></a>Ajout d’administrateur de domaine, SQL, AD RMS ou ADFS Service compte vers SQL Server 2016

Les étapes suivantes montrent comment ajouter les différents comptes de Service pour SQL Server 2016 afin de faciliter la migration des données à partir de l’environnement Windows Server 2012 R2. Cela donnera les autorisations appropriées lorsque vous tentez d’accéder au contenu et gérer les données.

**Pour ajouter l’administrateur de domaine, SQL, AD RMS, et/ou compte de Service AD FS vers SQL Server**

1.  Ouvrez une session en tant que compte d’administrateur Local le serveur avec SQL Server 2016.

2.  Cliquez sur **Démarrer**, cliquez sur **tous les programmes**, cliquez sur **Microsoft SQL Server**, puis cliquez sur **SQL Server Management Studio**.

3.  Dans le **se connecter au serveur** fenêtre, vérifiez le serveur qui héberge les bases de données AD RMS est dans le **nom du serveur** zone puis pour l’authentification, cliquez sur le menu déroulant et sélectionnez **SQL Server Authentification**.

4.  Dans le **connexion** champ Entrez le nom du compte d’administrateur Local (par exemple, LocalAdmin) et fournir le mot de passe approprié, puis cliquez sur **Connect**.

5.  Développez **sécurité** , cliquez sur **connexions** et sélectionnez **nouvelle connexion** dans le menu contextuel qui s’affiche.

6.  Une fois que la fenêtre s’affiche à entrer dans le compte d’administrateur de domaine dans le **nom de connexion** champ (par exemple, Contoso\\ContosoAdmin)

7.  Dans le volet de navigation gauche, choisissez **rôles de serveur**.

8.  Puis cochez la case à cocher pour **sysadmin** sous rôles de serveur et cliquez sur **OK**.

9.  Redémarrez **SQL Server Management**.

10. Dans le **se connecter au serveur** fenêtre, vérifiez le serveur qui héberge les bases de données AD RMS est dans le **nom du serveur** zone puis pour l’authentification, cliquez sur le menu déroulant et sélectionnez **Windows Authentification** et cliquez sur **Connect**.

##### <a name="restoring-the-ad-rms-and-adfs-databases-to-sql-server-2016"></a>Restauration des services AD RMS et ADFS de bases de données vers SQL Server 2016

Les étapes suivantes montrent comment restaurer les données à partir de l’instance de SQL Server précédente vers la nouvelle instance de 2016. Ainsi, le nouveau SQL d’utiliser les données de configuration pertinentes à partir de bases de données AD RMS et ADFS précédentes.

**Pour restaurer les données à partir du serveur SQL précédent vers le nouveau serveur SQL**

1.  Ouvrez une session le serveur avec SQL Server 2016 avec le compte approprié.

2.  Dans le volet de navigation de gauche, droit **bases de données** et sélectionnez **Restore Database** pour commencer le processus de restauration.

3.  Sous **Source** choisissez **appareil** et puis naviguez jusqu'à l’emplacement où les fichiers de base de données ont été stockés dans les étapes précédentes.

4.  Une fois que les fichiers ont été sélectionnés, cliquez sur **OK**.

5.  Assurez-vous que tous les fichiers de base de données ont été ajoutés et terminer le processus en cliquant sur **OK**.

### <a name="configuring-windows-server-2016-active-directory-federation-services-ad-fs"></a>Configuration d’Active Directory Federation Services (AD FS) de Windows Server 2016

AD FS a été déployée pour fournir l’authentification accès unique (SSO) aux services AD RMS en tant qu’application. Il a également été configurée avec l’AD RMS Mobile Device Extension (MDE), ce qui permet la prise en charge de l’appareil mobile pour les utilisateurs finaux et de Mac.

Les sections suivantes fournissent des conseils sur les tâches opérationnelles, que vous devrez peut-être effectuer sur votre déploiement AD FS.

#### <a name="adding-a-2016-ad-fs-server-to-the-farm"></a>Ajout d’un serveur ADFS 2016 à la batterie de serveurs

Vous pouvez déployer des serveurs AD FS supplémentaires pour prendre en charge le déploiement d’AD RMS. Vous pouvez choisir d’effectuer cette action en cas d’augmentation du trafic vers les serveurs AD RMS, ou des applications supplémentaires, ou si vous avez besoin de mettre hors service un des serveurs en cours d’utilisation pour AD FS.

**Pour ajouter le serveur 2016 AD FS à la batterie de serveurs**

1.  À partir du serveur Azure AD Connect, double-cliquez sur le **Azure AD Connect** icône pour lancer l’Assistant Azure AD Connect.

2.  Dans la page d’accueil, cliquez sur **configurer**.

3.  Dans la page tâches supplémentaires, cliquez sur **déployer un serveur de fédération supplémentaire** puis cliquez sur **suivant**.

4.  Dans la page connexion à Azure AD, entrez le nom d’utilisateur et le mot de passe d’un compte disposant d’autorisations administratives globales et puis cliquez sur **suivant**.

5.  Dans la page informations d’identification administrateur de domaine, entrez le nom d’utilisateur et le mot de passe d’un compte disposant d’autorisations d’administrateur de domaine et cliquez sur **suivant**.

6.  Cliquez sur **Parcourir** et sélectionnez le fichier de certificat utilisée lorsque configuration de la batterie de serveurs AD FS à l’aide d’Azure AD Connect.

7.  Cliquez sur **entrer le mot de passe** pour ouvrir la boîte de dialogue de mot de passe de certificat.

8.  Entrez le mot de passe du certificat dans le champ de mot de passe, puis activez **OK**.

9.  Cliquez sur **Suivant**.

10. Dans la page serveurs AD FS, entrez le nom ou l’adresse IP du nouveau serveur AD FS et cliquez sur **ajouter**.

11. Dans la page prêt à configurer, cliquez sur **installer**.

12. Dans la page Installation terminée, cliquez sur **Exit**.

#### <a name="raising-the-adfs-farm-behavior-level"></a>Augmenter le niveau de comportement de la batterie de serveurs AD FS

Lorsque vous déployez un serveur ADFs qui dépasse le niveau de l’environnement par exemple, avoir un AD FS sur Windows Server 2012 R2, puis en ajoutant qu'une ADFS Windows Server 2016, le niveau de comportement de batterie de serveurs doivent être augmentées. Cela est nécessaire pour vous assurer que l’environnement utilise les informations et les fonctions plus récentes.

**Pour augmenter le niveau de comportement de batterie de serveurs AD FS**

1.  Accédez à ADFS Windows Server 2016.

2.  Ouvrez une session PowerShell d’administrateur.

3.  Entrez la commande suivante :  **\$cred = Get-Credential**

4.  Une fenêtre s’affiche vous demandant des informations d’identification, entrez les informations d’identification administrateur de domaine.

5.  Puis entrez la commande suivante : **AdfsFarmBehaviorLevelRaise appeler-informations d’identification \$cred**

6.  Une invite apparaît pour vous demande, **voulez-vous continuer cette opération ?** Puis entrez **un** accepte l’invite.

7.  Une fois la commande terminée, le niveau de comportement de batterie de serveurs sera être configuré et prêt.

#### <a name="enabling-mobile-device-extension-logging"></a>Activation de l’enregistrement d’Extension de périphérique Mobile

L’Extension d’appareil Mobile peut journaliser les requêtes qu’il reçoit à partir d’appareils de l’utilisateur final. La journalisation est désactivée par défaut et nous ne recommandons que l’activation de la journalisation dans un scénario de dépannage. Toutes les demandes, des périphériques mobiles et ordinateurs de bureau, à démarrer ou d’acquérir une licence d’utilisation de fin sont enregistrés dans la base de données de journalisation AD RMS ou d’un compte de stockage Azure. Journalisation de MDE crée deux tables supplémentaires à SQL Server utilisés par AD RMS : le client déboguer la table du journal et la table de journal de performances client.

**Pour activer la journalisation de l’Extension d’appareils mobiles**

1.  À partir d’un serveur AD RMS, ouvrez Windows PowerShell en tant qu’administrateur.

2.  Tapez la commande suivante et la presse **entrée**: **Import-Module AdRmsAdmin**

3.  Tapez la commande suivante et la presse **entrée**: **New-PSDrive -Name AdrmsCluster -PsProvider AdRmsAdmin -Root https://localhost**

4.  Tapez la commande suivante et la presse **entrée**: **Set-ItemProperty-Path AdrmsCluster :\\ -IsLoggingEnabled de nom-valeur \$true**

Si vous utilisez la journalisation MDE pour la résolution des problèmes, nous vous recommandons de le désactiver après avoir corrigé le problème.

**Pour désactiver la journalisation de l’Extension d’appareils mobiles**

1.  À partir d’un serveur AD RMS, ouvrez Windows PowerShell en tant qu’administrateur.

2.  Tapez la commande suivante et la presse **entrée**: **Import-Module AdRmsAdmin**

3.  Tapez la commande suivante et la presse **entrée**: **New-PSDrive -Name AdrmsCluster -PsProvider AdRmsAdmin -Root https://localhost**

4.  Tapez la commande suivante et la presse **entrée**: **Set-ItemProperty-Path AdrmsCluster :\\ -IsLoggingEnabled de nom-valeur \$false**

### <a name="upgrading-ad-rms-to-windows-server-2016"></a>La mise à niveau d’AD RMS pour Windows Server 2016

Les sections suivantes fournissent des conseils sur l’ajout d’un serveur basé sur Windows Server 2016 AD RMS dans le cluster Windows Server 2012 R2 en cours. Le serveur sera ajouté dans le cluster et les informations seront répliquées à ce dernier afin que le serveur AD RMS précédent peut être déconseillé pour libérer des ressources.

Après avoir ajouté un serveur basé sur Windows Server 2016 AD RMS a été ajouté à votre cluster AD RMS, tous les nœuds selon les versions antérieures de Windows devient inactives. Après que cela, vous pouvez annuler l’approvisionnement de ces serveurs (par exemple, arrêter, réaffecter ou réinstaller Windows Server 2016 à rejoindre le cluster AD RMS). 

Vous pouvez déployer des serveurs AD RMS supplémentaires au cluster pour prendre en charge de la charge sur votre déploiement AD RMS. Vous pouvez également choisir d’effectuer cette action en cas d’augmentation du trafic vers les serveurs AD RMS.

Ce guide ne couvre pas les étapes nécessaires pour modifier la mécanismes que vous pouvez utiliser dans votre environnement pour exclure les serveurs que sont de dépréciation et incluent ceux que vous ajoutez au cluster d’équilibrage de charge. 

#### <a name="adding-a-2016-ad-rms-server"></a>Ajout d’un serveur 2016 AD RMS

Si votre cluster AD RMS utilise un Module de sécurité matériel plutôt qu’une clé gérée de façon centralisée pour son certificat de licence serveur, vous devez installer le logiciel et autres artefacts de module de sécurité matériel (par exemple, les fichiers de clé et la configuration) sur le serveur avant d’installer AD RMS. Vous devrez également les connectez au serveur, physiquement ou via les configurations de réseau approprié. Suivez les conseils de votre HSM pour ces étapes. 

**Pour ajouter un serveur RMS de 2016 AD**

1.  Installez le rôle AD RMS sur le déploiement de Windows Server 2016 souhaité.

2.  Une fois l’installation terminée, sélectionnez le lien vers **effectuer une configuration supplémentaire**.

3.  Sélectionnez **rejoindre un cluster AD RMS existant** et cliquez sur **suivant**.

4.  Sur le **sélectionner la base de données de Configuration** , entrez le nom canonique spécifié dans le DNS du serveur de SQL Server 2016 (FQDN).

5.  Cliquez sur **liste** sur la deuxième ligne et sélectionnez le **DefaultInstance** à partir de la liste déroulante.

6.  Sous **nom de base de données de Configuration**, sélectionnez le menu déroulant et choisissez la configuration de DRM qui s’affiche. Ensuite, cliquez sur **Suivant**.

7.  Sur le **les informations de base de données** , entrez le mot de passe de clé de cluster dans le champ fourni. Après cela, cliquez sur **suivant**.

8.  Dans la page suivante de l’Assistant, spécifiez le compte de service AD RMS et fournir le mot de passe pour celle-ci, puis cliquez sur **suivant** une fois qu’il a été vérifié.

9.  Une fois le **Site Web de Cluster** page s’affiche, simplement vous assurer que le site web approprié a été sélectionné et cliquez sur **suivant**.

10. Sur le **choisir un certificat d’authentification serveur** page, sélectionnez le certificat SSL importé, puis cliquez sur **suivant**.

11. Cliquez sur **Installer** pour commencer l'installation.

12. Une fois la configuration terminée, vous devez se déconnectent puis se reconnectent pour administrer AD RMS.

13. Une fois ouvert une session, ouvrez **le Gestionnaire de serveur** sélectionnez **outils** , puis **Active Directory Rights Management**. La fenêtre de gestion doit apparaître et indiquer que le cluster possède le serveur supplémentaire dans le cluster.

14. 14. Si l’Extension d’appareil Mobile AD RMS a été installée dans le cluster AD RMS d’origine, vous devez également installer le fichier MDE dans les nœuds de cluster mis à jour. Suivez les instructions dans la documentation MDE ajouter MDE à votre cluster AD RMS. À ce stade, réaffecter tous les nœuds préexistants ou les mettre à niveau vers Windows Server 2016 et participer au cluster AD RMS en utilisant le même processus présentée ci-dessus. 

### <a name="configuring-windows-server-2016-web-application-proxy-wap"></a>Configuration du Proxy d’Application Windows Server 2016 Web (WAP)

Les sections suivantes fournissent des conseils sur les tâches opérationnelles, que vous devrez peut-être effectuer sur votre déploiement de Proxy d’Application Web. Il s’agit d’une étape facultative, ne pas nécessaire si vous publiez les services AD RMS à Internet via d’autres mécanismes. 

#### <a name="adding-a-windows-server-2016-wap-server"></a>Ajout d’un serveur de proxy d’application Web de Windows Server 2016

Vous pouvez déployer des serveurs Proxy d’Application Web supplémentaires pour prendre en charge le déploiement d’AD RMS. Vous pouvez choisir d’effectuer cette action en cas d’augmentation du trafic vers les serveurs AD RMS ou si vous avez besoin de mettre hors service un des serveurs en cours d’utilisation pour le Proxy d’Application Web.

**Pour ajouter un serveur de Proxy d’Application Web 2016**

1.  À partir du serveur que vous souhaitez le programme d’installation comme un Proxy d’Application Web, accédez à la console du Gestionnaire de serveur, puis cliquez sur **ajouter des rôles et fonctionnalités**.

2.  Dans le **Assistant Ajouter des rôles et fonctionnalités**, cliquez sur **suivant** jusqu'à ce que vous arriviez à l’écran de sélection du rôle de serveur.

3.  Dans l’écran Sélectionner des rôles de serveur, sélectionnez **accès à distance**, puis cliquez sur **suivant** jusqu'à ce que vous êtes dans l’écran Sélectionner des rôles de serveur.

4.  Dans l’écran Sélectionner des rôles de serveur, sélectionnez **Proxy d’Application Web**, cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant**.

5.  Dans l’écran Confirmer les sélections d’Installation, cliquez sur **installer**.

6.  Une fois l’installation terminée, cliquez sur **fermer**.

7.  Il est maintenant temps de configurer le serveur. Pour ce faire, ouvrez la console de gestion de l’accès à distance sur le serveur de Proxy d’Application Web. Ouvrez le **Démarrer** menu, tapez **RAMgmtUI.exe**, puis sélectionnez l’application.

8.  Dans le volet de navigation, cliquez sur **Proxy d’Application Web**.

9.  Dans la console de gestion de l’accès à distance, cliquez sur **exécuter l’Assistant de Configuration de Proxy d’Application Web**. Une fois dans l’Assistant, cliquez sur **suivant**.

10. Dans l’écran de serveur de fédération, entrez le nom de domaine complet du serveur AD FS (Ex. ADFS.contoso.com), puis entrez les informations d’identification pour un administrateur sur le serveur AD FS.

11. Dans l’écran de certificat de Proxy AD FS, dans la liste des certificats actuellement installés sur le serveur Proxy d’Application Web, sélectionnez un certificat à utiliser pour le proxy AD FS par Proxy d’Application Web, puis cliquez sur **suivant**.

12. Dans l’écran de Confirmation, passez en revue les paramètres, puis cliquez sur **configurer**.

13. Une fois la configuration est terminée, cliquez sur **fermer**.

#### <a name="dns-configuration-for-2016-wap-server"></a>Configuration DNS pour 2016 serveur WAP

Une fois que le serveur de Proxy d’Application Web Windows Server 2016 a été mis en place, certaines modifications DNS devrez être apportées. Vous devrez à l’aide d’un service tel que GoDaddy pour pointer ADFS et les services AD RMS au niveau du serveur WAP de 2016.

**Pour faire pointer le DNS sur le serveur WAP**

1.  Accédez au site Web de votre fournisseur (par ex. GoDaddy).

2.  Accédez à la gestion de domaine, puis sur gestion du service DNS.

3.  Recherchez le service AD FS et AD RMS et remplacez le **pointe vers** partie avec l’adresse IP publique du serveur WAP 2016 et **enregistrer**.

4.  Les modifications peuvent prendre de temps pour se propager, mais une fois qu’ils font ce programme d’installation sera terminée.

#### <a name="enabling-debugging-logs"></a>L’activation des journaux de débogage

Les informations de journalisation détaillées sont disponibles sur les serveurs Proxy d’Application Web. Vous pouvez configurer la journalisation de débogage avancée à l’aide de l’Observateur d’événements. Paramètres supplémentaires peuvent également être activées pour la taille des journaux afin de garantir que l’analytique est utiles pour la visionneuse.

**L’activation des journaux de débogage pour le Proxy d’Application Web**

1.  Ouvrez le **Observateur d’événements** console sur le Proxy d’Application Web.

2.  Développez le **Microsoft** nœud.

3.  Développez le **Windows** nœud.

4.  Ouvrez le **Proxy d’Application Web** journaux.

5.  Vous serez ensuite en mesure d’ouvrir le **administrateur** journaux.

6.  Ouvrez le **Action** menu, situé dans le coin supérieur gauche, puis sélectionnez **propriétés**.

7.  Sous le **général** onglet, choisissez l’option à **activer la journalisation**.

8.  Enfin, vous êtes en mesure de personnaliser la taille maximale du journal et que se passe-t-il lorsque la taille du journal des événements maximale est atteinte.

### <a name="configuring-high-availability-for-windows-server-2016-services"></a>Configuration de haute disponibilité pour les Services de Windows Server 2016

Les sections suivantes fournissent des conseils sur les tâches opérationnelles, que vous devrez peut-être configurer votre environnement Windows Server 2016 dans une haute disponibilité.

#### <a name="adding-a-2016-ad-rms-server-for-high-availability"></a>Ajout d’un serveur de 2016 AD RMS pour la haute disponibilité

Vous pouvez déployer des serveurs AD RMS supplémentaires pour configurer la haute disponibilité. Vous pouvez choisir d’effectuer cette action en cas d’augmentation du trafic vers les serveurs AD RMS.

**Pour ajouter un serveur RMS de 2016 AD pour la haute disponibilité**

1.  Installez le rôle AD RMS sur le déploiement de Windows Server 2016 souhaité.

2.  Une fois l’installation terminée, sélectionnez le lien vers **effectuer une configuration supplémentaire**.

3.  Sélectionnez **rejoindre un cluster AD RMS existant** et cliquez sur **suivant**.

4.  Sur le **sélectionner la base de données de Configuration** , entrez le nom canonique spécifié dans le DNS du serveur de SQL Server 2016 (FQDN).

5.  Cliquez sur **liste** sur la deuxième ligne et sélectionnez le **DefaultInstance** à partir de la liste déroulante.

6.  Sous **nom de base de données de Configuration**, sélectionnez le menu déroulant et choisissez la configuration de DRM qui s’affiche. Ensuite, cliquez sur **Suivant**.

7.  Sur le **les informations de base de données** , entrez le mot de passe de clé de cluster dans le champ fourni. Après cela, cliquez sur **suivant**.

8.  Dans la page suivante de l’Assistant, spécifiez le compte de service AD RMS et fournir le mot de passe pour celle-ci, puis cliquez sur **suivant** une fois qu’il a été vérifié.

9.  Une fois le **Site Web de Cluster** page s’affiche, simplement vous assurer que le site web approprié a été sélectionné et cliquez sur **suivant**.

10. Sur le **choisir un certificat d’authentification serveur** page, sélectionnez le certificat SSL importé, puis cliquez sur **suivant**.

11. Cliquez sur **Installer** pour commencer l'installation.

12. Une fois la configuration terminée, vous devez se déconnectent puis se reconnectent pour administrer AD RMS.

13. Une fois ouvert une session, ouvrez **le Gestionnaire de serveur** sélectionnez **outils** , puis **Active Directory Rights Management**. La fenêtre de gestion doit apparaître et indiquer que le cluster possède le serveur supplémentaire dans le cluster.

14. Après avoir confirmé la configuration du serveur, configurer votre service d’équilibrage de charge pour équilibrer la charge entre les différents serveurs AD RMS dans le cluster.

#### <a name="adding-a-windows-server-2016-ad-fs-server-for-high-availability"></a>Ajout d’un serveur Windows Server 2016 AD FS pour la haute disponibilité

Vous pouvez déployer des serveurs AD FS supplémentaires pour configurer la haute disponibilité. Vous pouvez choisir d’effectuer cette action en cas d’augmentation du trafic vers les serveurs AD FS. **Remarque : après avoir déclenché le niveau de comportement de batterie de serveurs, une nouvelle entrée de base de données sera entrée dans le SQL Server 2016 (Adfs Configv3) et l’ancienne base de données de configuration doit être supprimé avant de poursuivre cette procédure.**

**Pour ajouter le serveur de Windows Server 2016 AD FS pour la haute disponibilité**

1.  Installez le rôle AD RMS sur le déploiement de Windows Server 2016 souhaité.

2.  Une fois l’installation terminée, sélectionnez le lien vers **configurer le service de fédération sur ce serveur**.

3.  Dans la section de bienvenue de l’Assistant, choisissez l’option à **ajouter un serveur de fédération à une batterie de serveurs de fédération** puis cliquez sur **suivant**.

4.  Spécifiez le compte de l’administrateur approprié, cliquez sur **suivant**.

5.  Sur le **spécifier la batterie de serveurs** , sélectionnez le **spécifier l’emplacement de la base de données pour une batterie existante à l’aide de SQL Server** Entrez l’enregistrement CNAME pour le service SQL pour le nom d’hôte de base de données, puis cliquez sur **suivant**.

6.  Sous le **spécifier un compte de Service** zone de l’Assistant, entrez les informations d’identification du compte de service AD FS et puis cliquez sur **suivant**.

7.  Dans **examiner les Options**, cliquez sur **suivant**.

8.  Cliquez sur **configurer** lorsque le bouton devient disponible.

9.  Après la configuration, redémarrez l’ordinateur.

10. Après la confirmation de l’installation de serveur, équilibrage de charge les serveurs AD FS en fonction des besoins.

#### <a name="adding-a-windows-server-2016-wap-server-for-high-availability"></a>Ajout d’un serveur de proxy d’application Web de Windows Server 2016 pour la haute disponibilité

Vous pouvez déployer des serveurs WAP supplémentaires pour configurer la haute disponibilité. Vous pouvez choisir d’effectuer cette action en cas d’augmentation du trafic vers les serveurs AD RMS.

**Pour ajouter un serveur de Proxy d’Application Web Windows Server 2016 pour la haute disponibilité**

1.  À partir du serveur que vous souhaitez le programme d’installation comme un Proxy d’Application Web, accédez à la console du Gestionnaire de serveur, puis cliquez sur **ajouter des rôles et fonctionnalités**.

2.  Dans le **Assistant Ajouter des rôles et fonctionnalités**, cliquez sur **suivant** jusqu'à ce que vous arriviez à l’écran de sélection du rôle de serveur.

3.  Dans l’écran Sélectionner des rôles de serveur, sélectionnez **accès à distance**, puis cliquez sur **suivant** jusqu'à ce que vous êtes dans l’écran Sélectionner des rôles de serveur.

4.  Dans l’écran Sélectionner des rôles de serveur, sélectionnez **Proxy d’Application Web**, cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant**.

5.  Dans l’écran Confirmer les sélections d’Installation, cliquez sur **installer**.

6.  Une fois l’installation terminée, cliquez sur **fermer**.

7.  Il est maintenant temps de configurer le serveur. Pour ce faire, ouvrez la console de gestion de l’accès à distance sur le serveur de Proxy d’Application Web. Ouvrez le **Démarrer** menu, tapez **RAMgmtUI.exe**, puis sélectionnez l’application.

8.  Dans le volet de navigation, cliquez sur **Proxy d’Application Web**.

9.  Dans la console de gestion de l’accès à distance, cliquez sur **exécuter l’Assistant de Configuration de Proxy d’Application Web**. Une fois dans l’Assistant, cliquez sur **suivant**.

10. Dans l’écran de serveur de fédération, entrez le nom de domaine complet du serveur AD FS (Ex. ADFS.contoso.com), puis entrez les informations d’identification pour un administrateur sur le serveur AD FS.

11. Dans l’écran de certificat de Proxy AD FS, dans la liste des certificats actuellement installés sur le serveur Proxy d’Application Web, sélectionnez un certificat à utiliser pour le proxy AD FS par Proxy d’Application Web, puis cliquez sur **suivant**.

12. Dans l’écran de Confirmation, passez en revue les paramètres, puis cliquez sur **configurer**.

13. Une fois la configuration est terminée, cliquez sur **fermer**.

14. Après avoir confirmé la configuration du serveur, équilibrez la charge les serveurs WAP dans la zone DMZ.

#### <a name="adding-a-sql-server-2016-node-for-always-on-high-availability"></a>Ajout d’un nœud de SQL Server 2016 pour la haute disponibilité Always On

Vous pouvez déployer des serveurs SQL Server pour configurer la haute disponibilité Always On. Vous pouvez choisir d’effectuer cette action en cas d’augmentation du trafic vers les serveurs AD RMS. **Remarque : vous assurer que le port entrant 5022 ouvert sur les deux serveurs SQL.**

**Pour ajouter un serveur SQL server 2016 pour la haute disponibilité Always On**

1.  À partir du serveur que vous souhaitez le programme d’installation en tant qu’un serveur SQL Server 2016 supplémentaire, accédez à la console du Gestionnaire de serveur, puis cliquez sur **ajouter des rôles et fonctionnalités**.

2.  Cliquez sur **suivant** jusqu'à ce que le **sélectionner des fonctionnalités** boîte de dialogue.

3.  Sélectionnez le **le Clustering de basculement** case à cocher. **Remarque : suivez cette étape pour le d’origine serveur SQL server 2016 ainsi afin que les deux serveurs SQL aient la fonctionnalité de Clustering de basculement.**

4.  Cliquez sur **installer** pour installer la fonctionnalité Clustering avec basculement.

5.  À présent, ouvrez **le Gestionnaire de serveur** et sélectionnez **outils** puis **Gestionnaire du Cluster de basculement**.

6.  Dans le volet de menu de gauche, droit **Gestionnaire du Cluster de basculement** et sélectionnez **création d’un Cluster**

7.  Cette opération ouvre le **Assistant Création d’un Cluster**.

8.  Recherchez les serveurs SQL server 2016, qui sera utilisé pour la haute disponibilité Always On et entrez-les dans puis cliquez sur **suivant**.

9.  Vous recevrez un avertissement de validation. Sélectionnez **Oui** à valider les nœuds de Cluster, puis cliquez sur **suivant**.

10. Sous le **Options de test** page, sélectionnez l’option **exécuter tous les tests** et cliquez sur **suivant.**

11. **Remarque : L’Assistant de Validation de Cluster est censé renvoyer plusieurs messages d’avertissement, en particulier si vous n’utiliserez pas un stockage partagé. Autrement, si vous trouvez des messages d’erreur vous devez les corriger avant de créer le Cluster de basculement Windows Server**.

12. Dans le **Point d’accès pour administrer le Cluster** boîte de dialogue, entrez le nom du cluster et l’adresse IP virtuelle pour le Cluster de basculement Windows Server, puis cliquez sur **suivant**.

13. Vérifiez que la configuration a réussi dans **Résumé** et cliquez sur **Terminer**.

14. Dans le **Gestionnaire du Cluster de basculement,** avec le bouton droit sur votre cluster, puis sélectionnez **autres Actions** puis choisissez **configurer les paramètres de Cluster Quorum**

15. Cliquez sur **suivant** , puis choisissez l’option pour **sélectionner le témoin de quorum** puis appuyez sur **suivant** à nouveau.

16. Dans le **sélectionner le témoin de Quorum** , sélectionnez le **configurer un témoin de partage de fichiers** option. Ensuite, cliquez sur **Suivant**.

17. Sélectionnez **Parcourir** et recherchez le chemin d’accès du partage de fichiers que vous souhaitez utiliser dans la boîte de dialogue chemin de partage de fichier. Cliquez sur **Suivant**.

18. Dans la page de Confirmation, cliquez sur **suivant**.

19. Dans la page Résumé, cliquez sur **Terminer**.

20. À présent, ouvrez le **Démarrer** menu et recherchez **Gestionnaire de Configuration SQL Server**.

21. Cliquez sur le nom de SQL Server et choisissez **propriétés**.

22. Dans la boîte de dialogue Propriétés, sélectionnez le **haute disponibilité AlwaysOn** onglet. Vérifier le **activer les groupes de disponibilité AlwaysOn** case à cocher. Cliquez sur **OK**. **Remarque : cela sur SQL server 2016 serveurs.**

23. Puis redémarrez le service SQL Server.

24. À présent, ouvrez le **Démarrer** menu et recherchez **SQL Server Management Studio** et dans le volet de navigation de gauche, droit **groupes de disponibilité** et cliquez sur **Assistant Nouveau groupe de disponibilité** puis cliquez sur **suivant**.

25. Dans le **spécifier un nom de groupe de disponibilité** page Choisir un nom de groupe (Ex.SQLAvailabilityGroup2016). Ensuite, cliquez sur **Suivant**.

26. Sous le **sélectionner les bases de données** section, spécifiez les bases de données. Puis cliquez sur Suivant. **Remarque : une base de données devrez à nouveau sauvegardés ou à mettre en mode de récupération complète**.

27. Une fois sur le **spécifier les réplicas** , cliquez sur le **ajouter un réplica** bouton et choisissez vos autres 2016 SQL Server.

28. Après avoir ajouté l’autre serveur, cliquez sur les cases à cocher et configurer le serveur secondaire pour être un secondaire lisible.

29. Accédez à la **points de terminaison** onglet et cliquez sur le **Actualiser** option. Lors de l’également ici, faites défiler et assurez-vous que le même compte de service est sur le nœud principal et secondaire.

30. Maintenant, choisissez le **préférences de sauvegarde** onglet et sélectionnez le **préférer secondaire** option.

31. Passez à la **écouteur** onglet.

32. Spécifiez un nom (par exemple SQLListener) et assurez-vous que le port est **1433** puis cliquez sur **suivant**.

33. Dans le **sélectionner la synchronisation initiale des données** page de l’Assistant, choisissez le **complète** et spécifier un emplacement réseau accessible par tous les serveurs SQL, puis cliquez sur **suivant**.

34. Enfin, cliquez sur **Terminer** et le processus se termine.

### <a name="decommission-windows-server-2012-r2-nodes"></a>Désactiver les nœuds de Windows Server 2012 R2

Les sections suivantes fournissent des conseils sur les tâches opérationnelles, que vous devrez peut-être supprimer vos serveurs Windows Server 2012 R2 après avoir mis à niveau le cluster AD RMS pour Windows Server 2016.

#### <a name="removing-a-windows-server-2012-r2-ad-rms-server"></a>Suppression d’un serveur Windows Server 2012 R2 AD RMS

Vous pouvez supprimer des serveurs AD RMS inutiles après une mise à niveau. Vous pouvez choisir d’effectuer cette action lorsqu’il devienne nécessaire de mettre hors service des serveurs AD RMS.

**Pour supprimer un** **server de Windows Server 2012 R2 AD RMS**

1.  Sur le serveur Windows Server 2012 R2 AD RMS dans le Gestionnaire de serveur, sélectionnez **gérer** à partir du haut menus avec le bouton droit, puis choisissez **supprimer des rôles et fonctionnalités**.

2.  Le **Assistant supprimer des rôles et fonctionnalités** s’ouvre en haut et sur le **avant de commencer** , cliquez sur **suivant**.

3.  Sur le **sélection du serveur** écran, cliquez sur **suivant**.

4.  Sur le **rôles serveur** écran, décochez la case à côté **Active Directory Rights Management Services** et cliquez sur **suivant**.

5.  Sur le **fonctionnalités** écran, cliquez sur **suivant**.

6.  Sur le **Confirmation** écran, cliquez sur **supprimer**.

7.  Une fois cette opération terminée, redémarrez le serveur.

8.  Vous pouvez maintenant fermer ce serveur et réaffecter les ressources en fonction des besoins.
