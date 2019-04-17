---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: "Sécurisation des contrôleurs de domaine contre les attaques"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fc40d0ac536b128b799006214e360fa4d991ee44
ms.sourcegitcommit: 06a84f5caeab49b8480d6eed037aebbb59c77a9f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2018
---
# <a name="securing-domain-controllers-against-attack"></a>Sécurisation des contrôleurs de domaine contre les attaques

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

*La loi du nombre de trois: si une personne malintentionnée dispose d’un accès physique illimité à votre ordinateur, il n’est pas votre ordinateur plus.* - [Dix immuables de sécurité (Version2.0)](https://technet.microsoft.com/security/hh278941.aspx)  
  
Contrôleurs de domaine fournissent le stockage physique de la base de données ADDS, en plus de fournir les services et les données qui permettent aux entreprises de gérer leurs serveurs, stations de travail, les utilisateurs et des applications. Si l’accès privilégié à un contrôleur de domaine est obtenu par un utilisateur malveillant, cet utilisateur peut modifier, endommager ou détruire la base de données ADDS et, par extension, tous les systèmes et les comptes qui sont gérés par ActiveDirectory.  
  
Dans la mesure où les contrôleurs de domaine peuvent lire et écrire à n’importe où dans la base de données ADDS, la compromission d’un contrôleur de domaine signifie que votre forêt ActiveDirectory peut jamais être considéré comme digne de confiance à nouveau, sauf si vous pouvez récupérer à l’aide d’une sauvegarde correcte et fermer les lacunes permettant la compromission du processus.  
  
En fonction de la personne malveillante préparation, outils et compétences, modification ou des dommages irréparables même pour les services ADDS, base de données peut être effectuée en quelques minutes à quelques heures, pas jours ou semaines. Les questions n’est pas la durée pendant laquelle une personne malveillante a un accès privilégié à ActiveDirectory, mais combien la personne malveillante a prévu pour l’instant lorsque de privilèges d’accès est obtenu. Compromettre un contrôleur de domaine peut fournir le chemin d’accès plus rapide à la propagation de grande échelle d’accès, ou le chemin d’accès plus directe à la destruction des serveurs membres, stations de travail et ActiveDirectory. Pour cette raison, les contrôleurs de domaine doivent être sécurisés séparément et de façon plus stricte que l’infrastructure générale de Windows.  

  
## <a name="physical-security-for-domain-controllers"></a>Sécurité physique des contrôleurs de domaine  
Cette section fournit des informations sur la sécurisation des contrôleurs de domaine, si les contrôleurs de domaine sont physiques ou virtuels, dans les emplacements de centre de données, les succursales et emplacements à distance avec les contrôles d’infrastructure de base uniquement physiquement.  
  
### <a name="datacenter-domain-controllers"></a>Contrôleurs de domaine centre de données  
  
#### <a name="physical-domain-controllers"></a>Contrôleurs de domaine physiques  
Dans les centres de données, les contrôleurs de domaine physiques doivent être installés dans les racks sécurisés dédiés ou des racks dédiés distincts à partir de la population générale du serveur. Lorsque cela est possible, les contrôleurs de domaine doivent être configurées avec les puces du Module de plateforme sécurisée (TPM) et tous les volumes sur les serveurs de contrôleur de domaine doivent être protégés par le chiffrement de lecteur BitLocker. BitLocker est généralement ajoute des performances de charge dans les pourcentages chiffre, mais protège le répertoire contre toute compromission, même si les disques sont supprimés à partir du serveur. BitLocker peut également aider à protéger les systèmes contre les attaques, telles que les rootkits, car la modification des fichiers de démarrage entraîne le serveur démarrer en mode de récupération afin que les fichiers binaires d’origine peuvent être chargés. Si un contrôleur de domaine est configuré pour utiliser les volumes RAID, SCSI attaché en série, stockage SAN/NAS, logiciels ou les volumes dynamiques, BitLocker ne peut pas être implémentés, par conséquent, connecté localement de stockage (avec ou sans matériel RAID) doit être utilisé dans les contrôleurs de domaine chaque fois que possible.  
  
#### <a name="virtual-domain-controllers"></a>Contrôleurs de domaine virtuel  
Si vous implémentez des contrôleurs de domaine virtuel, vous devez vous assurer que les contrôleurs de domaine s’exécuter sur des ordinateurs hôtes physiques distincts à d’autres ordinateurs virtuels dans l’environnement. Même si vous utilisez une plate-forme de virtualisation tiers, envisagez de déployer des contrôleurs de domaine virtuels sur le serveur Hyper-V dans Windows Server2012 ou Windows Server2008R2, qui fournit une surface d’attaque minimale et peuvent être gérés avec les contrôleurs de domaine qu’il héberge au lieu d’être gérés avec le reste des ordinateurs hôtes de virtualisation. Si vous implémentez SystemCenter Virtual Machine Manager (SCVMM) pour la gestion de votre infrastructure de virtualisation, vous pouvez déléguer l’administration des ordinateurs hôtes physiques sur les ordinateurs virtuels du contrôleur domaine résident et les contrôleurs de domaine eux-mêmes aux administrateurs autorisés. Vous devez également envisager de séparer le stockage des contrôleurs de domaine virtuel pour empêcher les administrateurs de stockage de l’accès aux fichiers d’ordinateur virtuel.  
  
### <a name="branch-locations"></a>Bureaux des filiales  
  
#### <a name="physical-domain-controllers"></a>Contrôleurs de domaine physiques  
Dans les emplacements dans lesquels plusieurs serveurs se trouvent, mais ne sont pas physiquement sécurisés au niveau des serveurs de centre de données sécurisés, les contrôleurs de domaine physiques doivent être configurés avec les puces TPM et le chiffrement de lecteur BitLocker pour tous les volumes du serveur. Si un contrôleur de domaine ne peuvent pas être stocké dans une pièce verrouillée dans les succursales, vous devez envisager de déployer RODC dans ces emplacements.  
  
#### <a name="virtual-domain-controllers"></a>Contrôleurs de domaine virtuel  
Dès que possible, vous devez exécuter des contrôleurs de domaine virtuel dans les filiales sur des ordinateurs hôtes physiques distincts que les autres ordinateurs virtuels dans le site. Dans les filiales dans lequel les contrôleurs de domaine virtuel ne peut pas s’exécutent sur des hôtes physiques distincts du reste de la population du serveur virtuel, vous devez implémenter des puces TPM et le chiffrement de lecteur BitLocker sur les ordinateurs hôtes sur lesquels les contrôleurs de domaine virtuels exécutent au minimum et tous les ordinateurs hôtes si possible. Selon la taille de la filiale et la sécurité des hôtes physiques, vous devez envisager de déployer RODC dans les succursales.  
  
### <a name="remote-locations-with-limited-space-and-security"></a>Emplacements distants avec un espace limité et de sécurité  
Si votre infrastructure inclut les emplacements dans lesquels un seul serveur physique peut être installé, un serveur capable d’exécuter des charges de travail de virtualisation doit être installé dans l’emplacement distant, et le chiffrement de lecteur BitLocker doit être configuré pour protéger tous les volumes sur le serveur. Un ordinateur virtuel sur le serveur doit s’exécuter sur un RODC, avec d’autres serveurs en cours d’exécution en tant que machines virtuelles distinctes sur l’ordinateur hôte. Informations sur la planification de déploiement de RODC est fourni dans le [Guide de déploiement et de planification de contrôleur de domaine en lecture seule](https://go.microsoft.com/fwlink/?LinkID=135993). Pour plus d’informations sur le déploiement et la sécurisation des contrôleurs de domaine virtualisés, voir [en cours d’exécution de contrôleurs de domaine dans Hyper-V](https://technet.microsoft.com/library/dd363553(v=ws.10).aspx) sur le site Web TechNet. Pour plus d’instructions pour la sécurisation renforcée Hyper-V, délégation de la gestion de l’ordinateur virtuel et la protection des machines virtuelles, consultez le [Guide de sécurité Hyper-V](https://www.microsoft.com/download/details.aspx?id=16650) accélérateur de solutions sur le site Web Microsoft.  
  
## <a name="domain-controller-operating-systems"></a>Systèmes d’exploitation de contrôleur de domaine  
Vous devez exécuter tous les contrôleurs de domaine sur la version la plus récente de Windows Server qui est pris en charge au sein de votre organisation et de hiérarchiser la désaffectation d’anciens systèmes d’exploitation de la population de contrôleur de domaine. En conservant des contrôleurs de domaine hérités actuelles et en éliminant vos contrôleurs de domaine, vous pouvez souvent tirer parti des nouvelles fonctionnalités et de sécurité peut-être pas disponible dans les domaines ou forêts avec les contrôleurs de domaine exécutant le système d’exploitation existant. Contrôleurs de domaine doivent être vient d’être installés et promus plutôt que mis à niveau à partir de systèmes d’exploitation antérieurs ou des rôles de serveurs; autrement dit, n’effectuer des mises à niveau sur place des contrôleurs de domaine ni exécuter l’Assistant d’Installation ADDS sur des serveurs sur lesquels le système d’exploitation n’est pas vient d’être installé. En implémentant les contrôleurs de domaine nouvellement installé, vous vérifiez que les fichiers hérités et les paramètres ne sont pas par inadvertance laissés sur les contrôleurs de domaine, et vous simplifiez la mise en œuvre de la configuration du contrôleur de domaine cohérent et sécurisé.  
  
## <a name="secure-configuration-of-domain-controllers"></a>Configuration sécurisée des contrôleurs de domaine  
De nombreux outils disponibles gratuitement, certains d'entre eux sont installés par défaut dans Windows, peut servir à créer une ligne de base de configuration initiale de sécurité pour les contrôleurs de domaine qui peuvent ensuite être appliquées par la stratégie de groupe. Ces outils sont décrits ici.  
  
### <a name="security-configuration-wizard"></a>Assistant Configuration de sécurité  

Tous les contrôleurs de domaine doivent être verrouillés sur la génération initiale. Cela peut être obtenue à l’aide de l’Assistant Configuration de sécurité qui est fourni en mode natif dans Windows Server pour configurer le service, le Registre, système et paramètres WFAS sur un contrôleur de domaine «version de base». Paramètres peuvent être enregistrés et exportés vers un objet de stratégie de groupe qui peut être lié vers l’UO de contrôleurs de domaine dans chaque domaine dans la forêt pour appliquer une configuration cohérente des contrôleurs de domaine. Si votre domaine contient plusieurs versions de systèmes d’exploitation Windows, vous pouvez configurer des filtres WindowsManagementInstrumentation (WMI) pour appliquer les GPO uniquement aux contrôleurs de domaine exécutant la version correspondante du système d’exploitation.  
  
### <a name="microsoft-security-compliance-manager"></a>MicrosoftSecurity Compliance Manager  
[MicrosoftSecurity Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx) paramètres du contrôleur de domaine peuvent être combinées avec les paramètres de l’Assistant Configuration de sécurité pour produire des lignes de base de configuration complète pour les contrôleurs de domaine qui sont déployés et appliquées par la stratégie de groupe déployée à l’unité d’organisation des contrôleurs de domaine dans ActiveDirectory.  
  
### <a name="applocker"></a>AppLocker  
AppLocker ou un outil de création de listes autorisées application tierce doit être utilisé pour configurer les services et applications qui sont autorisées à s’exécuter sur les contrôleurs de domaine, et ces services et applications autorisées doivent être uniquement composés de ce qui est requis pour l’ordinateur hôte, les services ADDS et éventuellement DNS, ainsi que n’importe quel logiciel de sécurité système tels que les logiciels antivirus. À la liste approuvée d’applications autorisée sur des contrôleurs de domaine, une couche supplémentaire de sécurité est ajoutée afin que même si une application non autorisée est installée sur un contrôleur de domaine, l’application ne peut pas s’exécuter.  
  
### <a name="rdp-restrictions"></a>Restrictions de RDP  
Les objets de stratégie de groupe qui sont liées aux contrôleurs de domaine toutes les unités d’organisation dans une forêt doit être configurés pour autoriser les connexions RDP uniquement à partir de systèmes (par exemple, les serveurs de renvoi) et les utilisateurs autorisés. Cela peut être obtenue via une combinaison de paramètres de droits d’utilisateur et de configuration WFAS et doit être implémentée dans des objets stratégie de groupe afin que la stratégie est appliquée de manière cohérente. Si elle est ignorée, la prochaine actualisation de stratégie de groupe renvoie le système à sa configuration correcte.  
  
### <a name="patch-and-configuration-management-for-domain-controllers"></a>Correctifs et gestion de la Configuration des contrôleurs de domaine  
Bien que cela peut sembler illogique, vous devez envisager la mise à jour corrective contrôleurs de domaine et d’autres composants d’infrastructure critiques séparément de votre infrastructure générale de Windows. Si vous utilisez un logiciel de gestion de configuration entreprise pour tous les ordinateurs dans votre infrastructure, compromission du logiciel de gestion de systèmes permettre servir à compromettre ou détruire tous les composants d’infrastructure gérés par ce logiciel. En séparant les correctifs et systèmes de gestion pour les contrôleurs de domaine de la population en général, vous pouvez réduire la quantité de logiciels installés sur les contrôleurs de domaine, en plus de contrôler étroitement leur gestion.  
  
### <a name="blocking-internet-access-for-domain-controllers"></a>Blocage de l’accès Internet pour les contrôleurs de domaine  

Un des contrôles qui est effectuée dans le cadre d’une évaluation de sécurité ActiveDirectory est l’utilisation et la configuration d’Internet Explorer sur les contrôleurs de domaine. Internet Explorer (ou un autre navigateur web) ne doit pas être utilisé sur les contrôleurs de domaine, mais l’analyse de milliers de contrôleurs de domaine a révélé de nombreux cas dans lesquels les utilisateurs privilégiés permettant de Internet Explorer pour parcourir intranet de l’entreprise ou à Internet.  
  
Comme décrit précédemment dans la section «Configuration» de [voies de compromis](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md), navigation sur Internet (ou un intranet infecté) à partir d’un des ordinateurs plus puissants dans une infrastructure Windows à l’aide d’un compte à privilèges élevés (qui sont les seuls comptes autorisés à se connecter localement aux contrôleurs de domaine par défaut) présente un risque d’exceptionnel à la sécurité d’une organisation. Si via un lecteur en téléchargement ou en téléchargement utilitaires «infectés par des programmes malveillants», les personnes malveillantes peuvent accéder à tout ce dont ils ont besoin pour complètement compromettre ou détruire l’environnement ActiveDirectory.  
  
Bien que les versions actuelles de MicrosoftInternet Explorer, Windows Server2008, Windows Server2012 et Windows Server2008R2 offrent un certain nombre de dispositifs de protection contre les téléchargements malveillants, dans la plupart des cas, dans le domaine dans lequel les contrôleurs et les comptes privilégiés avaient été utilisées pour naviguer sur Internet, les contrôleurs de domaine ont été exécutant Windows Server2003 ou protections offertes par les plus récentes des systèmes d’exploitation et des navigateurs avaient été volontairement désactivées.  
  
Lancement de navigateurs web sur les contrôleurs de domaine doit être interdite par la stratégie, mais par des contrôles techniques et les contrôleurs de domaine ne doivent pas être autorisés à accéder à Internet. Si vos contrôleurs de domaine devant répliquer sur plusieurs sites, vous devez implémenter des connexions sécurisées entre les sites. Bien que les instructions de configuration détaillées sont en dehors de la portée de ce document, vous pouvez implémenter un certain nombre de contrôles pour limiter la capacité des contrôleurs de domaine pour être utilisées à mauvais escient ou mal configuré et compromise par la suite.  
  
### <a name="perimeter-firewall-restrictions"></a>Restrictions de pare-feu de périmètre  
Pare-feu de périmètre doivent être configurés pour bloquer les connexions sortantes à partir de contrôleurs de domaine à Internet. Bien que les contrôleurs de domaine peuvent doivent communiquer au-delà des limites de site, les pare-feu de périmètre peuvent être configurés pour autoriser la communication intersite en suivant les instructions fournies dans [comment configurer un pare-feu pour les domaines et approbations](https://support.microsoft.com/kb/179442) sur le site Web de Support Microsoft.  
  
### <a name="dc-firewall-configurations"></a>Configurations de pare-feu du contrôleur de domaine  

Comme indiqué précédemment, vous devez utiliser l’Assistant Configuration de sécurité pour capturer les paramètres de configuration du pare-feu Windows avec fonctions avancées de sécurité sur les contrôleurs de domaine. Vous devez passer en revue la sortie de l’Assistant Configuration de sécurité pour vous assurer que les paramètres de configuration de pare-feu répondent aux exigences de votre organisation et ensuite utilisent des objets stratégie de groupe pour appliquer les paramètres de configuration.  
  
### <a name="preventing-web-browsing-from-domain-controllers"></a>Empêcher la navigation sur le Web à partir de contrôleurs de domaine  
Vous pouvez utiliser une combinaison de configuration AppLocker, la configuration de proxy «trou noir» et configuration WFAS pour empêcher les contrôleurs de domaine d’accéder à Internet et pour empêcher l’utilisation des navigateurs web sur les contrôleurs de domaine.  
  


