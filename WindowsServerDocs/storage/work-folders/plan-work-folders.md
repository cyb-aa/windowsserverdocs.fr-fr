---
ms.assetid: a7c39656-81ee-4c2b-80ef-4d017dd11b07
title: "Planification d’un déploiement de Dossiers de travail"
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
author: JasonGerend
manager: dongill
ms.author: jgerend
ms.date: 4/5/2017
description: "Comment planifier un déploiement de Dossiers de travail, y compris la configuration système requise et comment préparer votre environnement réseau."
ms.openlocfilehash: 877b418439e77e39cbdc6821808e296f977c26dd
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="planning-a-work-folders-deployment"></a>Planification d’un déploiement de Dossiers de travail

>S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, Windows10, Windows8.1, Windows7

Cette rubrique décrit le processus de conception d’une implémentation de Dossiers de travail et part du principe que vous disposez des connaissances suivantes:  
  
-   notions de base sur les Dossiers de travail (comme décrit dans [Dossiers de travail](work-folders-overview.md));  
  
-   notions de base sur les concepts des services de domaine Active Directory (AD DS);  
  
-   notions de base sur le partage de fichiers Windows et les technologies associées;  
  
-   notions de base sur l’utilisation du certificat SSL;  
  
-   notions de base sur l’activation de l’accès web aux ressources internes via un proxy inverse web.  
  
 Les sections suivantes vont vous aider à concevoir votre implémentation de Dossiers de travail. Le déploiement de Dossiers de travail est abordé dans la rubrique suivante, [Déploiement de Dossiers de travail](deploy-work-folders.md).  
  
##  <a name="BKMK_SOFT"></a> Configuration logicielle requise  

La fonctionnalité Dossiers de travail présente la configuration logicielle requise suivante pour les serveurs de fichiers et votre infrastructure réseau:  
  
-   Un serveur exécutant Windows Server2012R2 ou Windows Server2016 pour l’hébergement des partages de synchronisation avec des fichiers utilisateur  
  
-   Un volume formaté avec le système de fichiers NTFS pour le stockage des fichiers utilisateur  
  
-   Pour appliquer les stratégies de mot de passe sur les PC Windows7, vous devez utiliser des stratégies de mot de passe de stratégie de groupe. Vous devez également exclure les PC Windows7 des stratégies de mot de passe de Dossiers de travail (si vous les utilisez).

-   Un certificat de serveur pour chaque serveur de fichiers qui hébergera les dossiers de travail. Ces certificats doivent provenir d’une autorité de certification approuvée par vos utilisateurs, dans l’idéal une autorité de certification publique.

-   (Facultatif) Une forêt des services de domaine Active Directory avec des extensions de schéma dans Windows Server2012 R2 pour prendre en charge automatiquement les références des PC et appareils sur le serveur de fichiers approprié lors de l'utilisation de plusieurs serveurs de fichiers. 
  
Pour permettre aux utilisateurs d’effectuer la synchronisation sur Internet, il existe des exigences supplémentaires:  
  
-   La possibilité de rendre un serveur accessible à partir d’Internet en créant des règles de publication dans le proxy inverse ou la passerelle réseau de votre organisation  
  
-   (Facultatif) Un nom de domaine enregistré publiquement et la possibilité de créer d’autres enregistrements DNS publics pour le domaine  
  
-   (Facultatif) Une infrastructure des services de fédération Active Directory (AD FS) lors de l’utilisation de l’authentification ADFS  
  
La fonctionnalité Dossiers de travail nécessite la configuration logicielle suivante pour les ordinateurs clients:  
  
-   Les ordinateurs doivent exécuter l’un des systèmes d’exploitation suivants:  
  
    -   Windows10  
  
    -   Windows8.1  
  
    -   WindowsRT8.1  
  
    -   Windows7  
  
    -   Android4.4 KitKat et versions ultérieures  
  
    -   iOS10.2 et versions ultérieures  
  
-   Les PC Windows7 doivent exécuter l'une des éditions suivantes de Windows:  
  
    -   Windows7Professionnel  
  
    -   Windows7 Édition Intégrale  
  
    -   Windows7Entreprise  
  
-   Les PC Windows7 doivent être joints au domaine de votre organisation (ils ne peuvent pas être joints à un groupe de travail).  
  
-   Un espace libre suffisant sur un lecteur local au format NTFS pour stocker tous les fichiers utilisateur dans Dossiers de travail, plus 6Go supplémentaires d’espace libre si Dossiers de travail se trouve sur le lecteur système, son emplacement par défaut. Dossiers de travail utilise l’emplacement suivant par défaut: **%USERPROFILE%\Dossiers de travail**  
  
     Toutefois, les utilisateurs peuvent modifier l’emplacement lors de l’installation (les cartes microSD et les lecteurs USB formatés avec le système de fichiers NTFS sont des emplacements pris en charge, même si la synchronisation s’arrête si les lecteurs sont supprimés).  
  
     La taille maximale pour les fichiers individuels est de 10Go par défaut. Il n’existe pas de limite de stockage par utilisateur, même si les administrateurs peuvent utiliser la fonctionnalité des quotas du Gestionnaire de ressources du serveur de fichiers pour implémenter les quotas.  
  
-   La fonctionnalité Dossiers de travail ne prend pas en charge la restauration de l’état des ordinateurs virtuels clients. Effectuez plutôt des opérations de sauvegarde et de restauration à partir de l’ordinateur virtuel client en utilisant Sauvegarde d’image système ou une autre application de sauvegarde.  
  
> [!NOTE]
>  Veillez à installer le correctif cumulatif de disponibilité générale de Windows8.1 et de Windows Server2012R2 sur tous les serveurs Dossiers de travail et tous les ordinateurs clients exécutant Windows8.1 ou Windows Server2012R2. Pour plus d’informations, voir l’article [2883200](http://support.microsoft.com/kb/2883200) dans la Base de connaissances Microsoft.  
  
## <a name="deployment-scenarios"></a>Scénarios de déploiement  
 La fonctionnalité Dossiers de travail peut être implémentée sur un nombre quelconque de serveurs de fichiers dans un environnement client. Cela permet d’adapter les implémentations de Dossiers de travail en fonction des besoins des clients et peut aboutir à des déploiements hautement individualisés. Toutefois, la plupart des déploiements entrent dans l’un des trois scénarios de base suivants.  
  
### <a name="single-site-deployment"></a>Déploiement sur un site unique  
 Dans un déploiement sur un site unique, les serveurs de fichiers sont hébergés sur un site central dans l’infrastructure du client. Ce type de déploiement est le plus souvent utilisé dans les clients avec une infrastructure hautement centralisée ou avec des succursales plus petites qui ne gèrent pas les serveurs de fichiers locaux. Ce modèle de déploiement peut être plus facile à administrer pour le personnel informatique, car toutes les ressources du serveur sont locales et les entrées/sorties Internet sont également susceptibles d’être centralisées à cet emplacement. Toutefois, ce modèle de déploiement repose aussi sur une connectivité au réseau étendu (WAN) fiable entre le site central et les succursales, et les utilisateurs dans les succursales sont vulnérables à une interruption de service causée par les conditions réseau.  
  
### <a name="multiple-site-deployment"></a>Déploiement sur plusieurs sites  
 Dans un déploiement sur plusieurs sites, les serveurs de fichiers sont hébergés à plusieurs emplacements dans l’infrastructure du client. Cela peut représenter plusieurs centres de données ou signifier que les succursales gèrent des serveurs de fichiers individuels. Ce type de déploiement est le plus souvent utilisé dans les environnements clients plus importants ou dans les clients qui disposent de plusieurs succursales de plus grande taille et qui gèrent les ressources du serveur locales. Ce modèle de déploiement est plus difficile à administrer pour le personnel informatique et repose sur une coordination minutieuse du stockage de données et de la maintenance des services de domaine Active Directory (ADDS) pour garantir que les utilisateurs emploient le serveur de synchronisation correct pour la fonctionnalité Dossiers de travail.  
  
### <a name="hosted-deployment"></a>Déploiement hébergé  
 Dans un déploiement hébergé, les serveurs de synchronisation sont déployés dans une solution IAAS (Infrastructure-as-a-service), telle que Microsoft Azure VM. Cette méthode de déploiement présente l’avantage de rendre la disponibilité des serveurs de fichiers moins dépendante de la connectivité au réseau étendu (WAN) dans les activités du client. Si un appareil est en mesure de se connecter à Internet, il peut accéder à son serveur de synchronisation. Toutefois, les serveurs déployés dans l’environnement hébergé doivent quand même pouvoir atteindre le domaine Active Directory de l’organisation pour authentifier les utilisateurs. Quant au client, il échange une configuration requise de l’infrastructure sur site contre une gestion plus complexe de cette connexion.  
  
## <a name="deployment-technologies"></a>Technologies de déploiement  
 Les déploiements de Dossiers de travail reposent sur plusieurs technologies qui fonctionnent ensemble pour offrir un service aux appareils à la fois sur les réseaux internes et externes. Avant de concevoir un déploiement de Dossiers de travail, les clients doivent connaître les exigences de chacune des technologies suivantes.  
  
### <a name="active-directory-domain-services"></a>Services de domaine Active Directory  
 Les services ADDS fournissent deux services importants dans un déploiement de Dossiers de travail. Tout d’abord, en tant que technologie principale pour l’authentification Windows, ils offrent les services de sécurité et d’authentification utilisés pour accorder l’accès aux données utilisateur. S’il est impossible d’atteindre un contrôleur de domaine, un serveur de fichiers ne pourra pas authentifier une demande entrante et l’appareil ne pourra accéder à aucune donnée stockée dans le partage de synchronisation de ce serveur de fichiers.  
  
 Ensuite, les services ADDS (avec la mise à jour de schéma Windows Server2012 R2) gèrent l'attribut msDS-SyncServerURL sur chaque utilisateur, qui permet de diriger automatiquement les utilisateurs vers le serveur de synchronisation approprié.  
  
### <a name="file-servers"></a>Serveurs de fichiers  
 Les serveurs de fichiers exécutant WindowsServer2012R2 ou WindowsServer2016 hébergent le service de rôle Dossiers de travail ainsi que les partages de synchronisation qui stockent les données Dossiers de travail des utilisateurs. Les serveurs de fichiers peuvent également héberger des données stockées par d’autres technologies fonctionnant sur le réseau interne (par exemple, des partages de fichiers) et peuvent être mis en cluster pour assurer la tolérance de pannes pour les données utilisateur.  
  
###  <a name="GroupPolicy"></a> Stratégie de groupe  
 Si vous avez des PC Windows7 dans votre environnement, nous vous conseillons les actions suivantes:  
  
-   Utilisez la stratégie de groupe pour contrôler les stratégies de mot de passe pour tous les PC appartenant à un domaine qui utilisent Dossiers de travail.  
  
-   Utilisez la stratégie **Verrouiller automatiquement l'écran et exiger un mot de passe** de Dossiers de travail sur les PC qui ne sont pas joints à votre domaine.  
  
 Vous pouvez aussi utiliser une stratégie de groupe pour indiquer un serveur Dossiers de travail pour les PC appartenant à un domaine. L’installation de Dossiers de travail est ainsi quelque peu simplifiée: les utilisateurs devraient sinon entrer leur adresse de messagerie professionnelle pour rechercher les paramètres (en supposant que Dossiers de travail est correctement configuré) ou l’URL de Dossiers de travail que vous leur avez fournie de façon explicite par courrier électronique ou un autre moyen de communication.  
  
 Vous pouvez également utiliser une stratégie de groupe pour installer de force Dossiers de travail en fonction de l’utilisateur ou de l’ordinateur, bien que cette opération provoque la synchronisation de Dossiers de travail sur chaque PC auquel un utilisateur se connecte (lors de l’utilisation du paramètre de stratégie par utilisateur) et empêche les utilisateurs de spécifier un autre emplacement pour Dossiers de travail sur leur PC (tel qu’une carte microSD pour économiser l’espace sur le lecteur principal). Nous vous conseillons d’évaluer soigneusement les besoins de l’utilisateur avant de forcer l’installation automatique.  
  
### <a name="windows-intune"></a>WindowsIntune  
 WindowsIntune fournit également une couche de sécurité et une facilité de gestion pour les appareils n’appartenant pas à un domaine qui seraient autrement absents. Vous pouvez utiliser Windows Intune pour configurer et gérer les appareils personnels des utilisateurs, tels que les tablettes qui se connectent à Dossiers de travail depuis Internet. Windows Intune peut fournir des appareils avec l’URL du serveur de synchronisation à utiliser; les utilisateurs doivent sinon entrer leur adresse de messagerie professionnelle pour rechercher les paramètres (si vous publiez une URL de Dossiers de travail publique sous la forme https://workfolders.*contoso.com*), ou entrer directement l’URL du serveur de synchronisation.  
  
 Sans un déploiement Windows Intune, les utilisateurs doivent configurer manuellement des appareils externes, ce qui peut augmenter le nombre de demandes auprès du personnel du support technique d’un client.  
  
 Vous pouvez également utiliser Windows Intune pour effacer de manière sélective les données de Dossiers de travail sur l’appareil d’un utilisateur sans conséquences sur le reste des données, ce qui est pratique si un utilisateur quitte votre organisation ou si son appareil est volé.  
  
### <a name="web-application-proxyazure-ad-application-proxy"></a>Proxy d'application web/proxy d’application Azure AD  
 La fonctionnalité Dossiers de travail est conçue autour du concept autorisant les appareils connectés à Internet à récupérer en toute sécurité des données d’entreprise à partir du réseau interne, ce qui permet aux utilisateurs d’«emporter leurs données avec eux» sur leurs tablettes et appareils qui ne pourraient normalement pas accéder aux fichiers de travail. Pour ce faire, un proxy inverse doit être utilisé pour publier les URL des serveurs de synchronisation et les rendre disponibles pour les clients Internet. 
 
La fonctionnalité Dossiers de travail prend en charge l'utilisation du proxy d’application web, du proxy d’application Azure AD ou de solutions de proxy inverse tierces: 

-  Le proxy d’application web est une solution de proxy inverse locale. Pour en savoir plus, voir [Proxy d’application web dans Windows Server2016](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server).  
  
-  Le proxy d’application Azure AD est une solution de proxy inverse cloud. Pour en savoir plus, voir [Comment fournir un accès distant sécurisé aux applications locales](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-get-started)

## <a name="additional-design-considerations"></a>Considérations de conception supplémentaires  
 Outre la compréhension de chacun des composants indiqués ci-dessus, les clients doivent prendre du temps lors de la conception pour réfléchir au nombre de partages et de serveurs de synchronisation à utiliser, et s’il convient ou non de tirer parti du clustering de basculement pour assurer la tolérance de pannes sur ces serveurs de synchronisation  
  
### <a name="number-of-sync-servers"></a>Nombre de serveurs de synchronisation  
 Il est possible pour un client de faire fonctionner plusieurs serveurs de synchronisation dans un environnement. Cette configuration peut être souhaitable pour plusieurs raisons:  
  
-   répartition géographique des utilisateurs: par exemple, des serveurs de fichiers de succursales ou des centres de données régionaux;  
  
-   besoins en matière de stockage des données: certains services de l’entreprise peuvent avoir des besoins spécifiques en matière de gestion ou de stockage des données qui sont plus simples à traiter avec un serveur dédié;  
  
-   équilibrage de la charge: dans les environnements de grande taille, le stockage des données utilisateur sur plusieurs serveurs peut augmenter le temps d’activité et les performances des serveurs.  
  
 Pour plus d’informations sur les performances et l’adaptation du serveur Dossiers de travail, voir [Considérations sur les performances pour les déploiements de Dossiers de travail](http://blogs.technet.com/b/filecab/archive/2013/11/01/performance-considerations-for-large-scale-work-folders-deployments.aspx).  
  
> [!NOTE]
>  Lors de l’utilisation de plusieurs serveurs de synchronisation, il est conseillé de configurer la découverte automatique de serveurs pour les utilisateurs. Ce processus repose sur la configuration d’un attribut sur chaque compte d’utilisateur dans les services de domaine Active Directory. Cet attribut se nomme **msDS-SyncServerURL** et devient disponible sur les comptes d'utilisateurs après l'ajout d'un contrôleur de domaine Windows Server2012 R2 au domaine ou l'application des mises à jour du schéma Active Directory. Cet attribut doit être défini pour chaque utilisateur afin de garantir que tous se connectent au serveur de synchronisation approprié. La découverte automatique de serveurs permet aux organisations de publier la fonctionnalité Dossiers de travail derrière une URL «conviviale», telle que *https://workfolders.contoso.com*, quel que soit le nombre de serveurs de synchronisation en fonctionnement.  
  
### <a name="number-of-sync-shares"></a>Nombre de partages de synchronisation  
 Des serveurs de synchronisation individuels peuvent gérer plusieurs partages de synchronisation. Cela peut s’avérer utile pour les raisons suivantes:  
  
-   Exigences en matière d’audit et de sécurité: si des données utilisées par un certain service doivent être auditées de façon plus poussée ou conservées pendant une plus longue période, des partages de synchronisation distincts peuvent aider les administrateurs à conserver les dossiers utilisateur en séparant les différents niveaux d’audit.  
  
-   Quotas différents ou filtres de fichiers: si vous voulez définir des limites ou quotas de stockage différents selon lesquels les types de fichiers sont autorisés dans Dossiers de travail (filtres de fichiers) pour plusieurs groupes d’utilisateurs, des partages de synchronisation distincts peuvent s’avérer utiles.  
  
-   Contrôle des services: si des tâches administratives sont réparties par service, l’utilisation de partages distincts pour des services différents peut aider les administrateurs à appliquer des quotas ou d’autres stratégies.  
  
-   Stratégies de périphériques différentes: si une organisation doit gérer plusieurs stratégies de périphériques (telles que le chiffrement de Dossiers de travail) pour des groupes d’utilisateurs divers, elle peut le faire à l’aide de plusieurs partages.  
  
-   Capacité de stockage: si un serveur de fichiers dispose de plusieurs volumes, des partages supplémentaires permettent de tirer parti de ces volumes. Un partage individuel a accès uniquement au volume sur lequel il est hébergé et ne peut pas profiter des autres volumes sur un serveur de fichiers.  
  
#### <a name="access-to-sync-shares"></a>Accès aux partages de synchronisation  

Même si le serveur de synchronisation auquel un utilisateur accède est déterminé par l’URL entrée sur le client (ou l’URL publiée pour cet utilisateur dans les services de domaine Active Directory lors de l’utilisation de la découverte automatique de serveurs), l’accès aux partages de synchronisation individuels est déterminé par les autorisations présentes sur le partage.  
  
Par conséquent, si un client héberge plusieurs partages de synchronisation sur le même serveur, des précautions doivent être prises pour s’assurer que les utilisateurs individuels disposent des autorisations pour accéder à un seul de ces partages. Dans le cas contraire, lorsque des utilisateurs se connectent au serveur, leur client peut se connecter au partage incorrect. Pour ce faire, il suffit de créer un groupe de sécurité séparé pour chaque partage de synchronisation.  
  
En outre, l’accès au dossier d’un utilisateur au sein d’un partage de synchronisation est déterminé par les droits de propriété sur le dossier. Lors de la création d’un partage de synchronisation, la fonctionnalité Dossiers de travail accorde par défaut aux utilisateurs un accès exclusif à leurs fichiers (en désactivant l’héritage et en les rendant propriétaires de leurs dossiers individuels).  
  
## <a name="design-checklist"></a>Liste de vérification de la conception  

L’ensemble suivant de questions relatives à la conception est conçu pour aider les clients lors de la conception d’une implémentation de Dossiers de travail la plus utile possible à leur environnement. Les clients doivent passer cette liste de vérification en revue avant de tenter de déployer des serveurs.  
  
-   Utilisateurs visés  
  
    -   Quels utilisateurs emploieront la fonctionnalité Dossiers de travail?  
  
    -   Comment les utilisateurs sont-ils organisés? (géographiquement, par bureau, par service, etc.)  
  
    -   Est-ce que les utilisateurs ont des besoins spéciaux en termes de stockage des données, de sécurité ou de rétention?  
  
    -   Est-ce que les utilisateurs ont des besoins spécifiques en termes de stratégies de périphériques, telles que le chiffrement?  
  
    -   Quels appareils et ordinateurs clients devez-vous prendre en charge? (Windows8.1, WindowsRT8.1, Windows7)  
  
         Si vous prenez en charge les PC Windows7 et voulez utiliser des stratégies de mot de passe, excluez le domaine stockant leurs comptes d'ordinateurs de la stratégie de mot de passe de Dossiers de travail et utilisez à la place des stratégies de mot de passe de stratégie de groupe pour les PC appartenant à ce domaine.  
  
    -   Avez-vous besoin d’interagir avec d’autres solutions de gestion des données utilisateur, telles que la redirection de dossiers, ou de migrer à partir de ces solutions?  
  
    -   Est-ce que les utilisateurs de plusieurs domaines doivent effectuer la synchronisation sur Internet avec un seul serveur?  
  
    -   Devez-vous prendre en charge des utilisateurs qui ne sont pas membres du groupe Administrateurs local sur leur PC appartenant à un domaine? (Si tel est le cas, vous devez exclure les domaines pertinents des stratégies de périphériques de Dossiers de travail, par exemple les stratégies de chiffrement et de mot de passe.)  
  
-   Infrastructure et planification de la capacité  
  
    -   Sur quels sites doivent être situés les serveurs de synchronisation sur le réseau?  
  
    -   Est-ce que l’un des serveurs de synchronisation sera hébergé par un fournisseur IaaS (Infrastructure-as-a-service) comme dans Azure VM?  
  
    -   Est-ce que des serveurs dédiés seront nécessaires pour des groupes d’utilisateurs spécifiques et, si oui, combien d’utilisateurs pour chaque serveur dédié?  
  
    -   Où sont situés les points d’entrée et de sortie Internet sur le réseau?  
  
    -   Est-ce que les serveurs de synchronisation seront mis en cluster pour assurer la tolérance de pannes?  
  
    -   Est-ce que les serveurs de synchronisation devront gérer plusieurs volumes de données pour héberger les données utilisateur?  
  
-   Sécurité des données  
  
    -   Est-ce que plusieurs partages de synchronisation devront être créés sur les serveurs de synchronisation?  
  
    -   Quels groupes seront utilisés pour fournir l’accès aux partages de synchronisation?  
  
    -   Si vous utilisez plusieurs serveurs de synchronisation, quel groupe de sécurité allez-vous créer pour déléguer la possibilité de modifier la propriété msDS-SyncServerURL des objets utilisateur?  
  
    -   Existe-t-il des exigences spéciales en matière d’audit ou de sécurité pour les partages de synchronisation individuels?  
  
    -   Est-ce que l’authentification multifacteur est requise?  
  
    -   Avez-vous besoin de pouvoir effacer à distance les données de Dossiers de travail des PC et appareils?  
  
-   Accès des appareils  
  
    -   Quelle URL sera utilisée pour fournir l’accès aux appareils basés sur Internet (*l’URL par défaut qui est requise pour la découverte automatique de serveurs basée sur le courrier électronique est workfolders.domainname*)?  
  
    -   Comment l’URL sera-t-elle publiée sur Internet?  
  
    -   Est-ce que la découverte automatique de serveurs sera utilisée?  
  
    -   Est-ce qu’une stratégie de groupe sera utilisée pour configurer les PC appartenant à un domaine?  
  
    -   Est-ce que Windows Intune sera utilisé pour configurer les appareils externes?  
  
    -   Est-ce que le service DRS (Device Registration Service) sera requis pour la connexion des appareils?  
  
## <a name="next-steps"></a>Étapes suivantes  
 Après la conception de votre implémentation de Dossiers de travail, l’heure est au déploiement. Pour plus d’informations, voir [Déploiement de Dossiers de travail](deploy-work-folders.md).  
  
## <a name="see-also"></a>Voir également  
 Pour plus d’informations connexes, voir les ressources suivantes.  
  
|Type de contenu|Références|  
|------------------|----------------|  
|**Évaluation du produit**|-   [Dossiers de travail](work-folders-overview.md)<br />-   [Dossiers de travail pour Windows7](http://blogs.technet.com/b/filecab/archive/2014/04/24/work-folders-for-windows-7.aspx) (billet de blog)|  
|**Déploiement**|-   [Conception d’une implémentation de Dossiers de travail](plan-work-folders.md)<br />-   [Déploiement de Dossiers de travail](deploy-work-folders.md)<br />-   [Déploiement de Dossiers de travail avec ADFS et le proxy d'application web](deploy-work-folders-adfs-overview.md)<br />- [Déploiement de Dossiers de travail avec le Proxy d'application Azure AD](https://blogs.technet.microsoft.com/filecab/2017/05/31/enable-remote-access-to-work-folders-using-azure-active-directory-application-proxy/)<br />-   [Considérations sur les performances pour les déploiements de Dossiers de travail](http://blogs.technet.com/b/filecab/archive/2013/11/01/performance-considerations-for-large-scale-work-folders-deployments.aspx)<br />-   [Dossiers de travail pour Windows7 (téléchargement 64bits)](http://www.microsoft.com/download/details.aspx?id=42558)<br />-   [Dossiers de travail pour Windows7 (téléchargement 32bits)](http://www.microsoft.com/download/details.aspx?id=42559)<br />-   [Déploiement de laboratoire de test de Dossiers de travail](http://blogs.technet.com/b/filecab/archive/2013/07/10/work-folders-test-lab-deployment.aspx) (billet de blog)|
