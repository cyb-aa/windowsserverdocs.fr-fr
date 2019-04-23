---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: Sécurisation des contrôleurs de domaine contre les attaques
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 06/18/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6be2899e85b68578518d9de1805c287367608163
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863090"
---
# <a name="securing-domain-controllers-against-attack"></a>Sécurisation des contrôleurs de domaine contre les attaques

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*La loi numéro 3 : Si une personne malintentionnée dispose d’un accès physique illimité à votre ordinateur, il n’est plus votre ordinateur.* - [Dix lois immuables de sécurité (Version 2.0)](https://technet.microsoft.com/security/hh278941.aspx)  
  
Contrôleurs de domaine fournissent le stockage physique pour la base de données AD DS, en plus de fournir les services et les données qui permettent aux entreprises de gérer leurs serveurs, les stations de travail, les utilisateurs et les applications. Si un accès privilégié à un contrôleur de domaine est obtenu par un utilisateur malveillant, cet utilisateur peut modifier, endommager ou détruire la base de données AD DS et, par extension, tous les systèmes et les comptes qui sont gérés par Active Directory.  
  
Étant donné que les contrôleurs de domaine peuvent lire et écrire à quoi que ce soit dans la base de données AD DS, la compromission d’un contrôleur de domaine signifie que votre forêt Active Directory peut jamais être considéré comme digne de confiance à nouveau, sauf si vous êtes en mesure de récupérer à l’aide d’une sauvegarde reconnue fiable et à Fermez les lacunes autorisée la compromission dans le processus.  
  
En fonction de l’attaquant de préparation, outils et compétences, modification ou des dommages irréparables même pour les services AD DS, base de données peut être effectuée en quelques minutes à quelques heures, pas les jours ou semaines. Ce qui est important n’est pas la durée pendant laquelle une personne malveillante a un accès privilégié à Active Directory, mais combien l’attaquant a prévu pour le moment lorsque de privilèges d’accès est obtenu. Compromettre un contrôleur de domaine peut fournir le chemin d’accès plus rapide à la propagation de grande échelle d’accès ou le chemin d’accès la plus directe pour la destruction des serveurs membres, les stations de travail et Active Directory. Pour cette raison, les contrôleurs de domaine doivent être sécurisées séparément et plus stricte que l’infrastructure générale de Windows.  

## <a name="physical-security-for-domain-controllers"></a>Sécurité physique des contrôleurs de domaine

Cette section fournit des informations sur la sécurisation physiquement les contrôleurs de domaine, si les contrôleurs de domaine sont physiques ou virtuels, dans les emplacements de centre de données, les succursales et les emplacements à distance avec uniquement les contrôles d’une infrastructure de base.  
  
### <a name="datacenter-domain-controllers"></a>Contrôleurs de domaine de centre de données  
  
#### <a name="physical-domain-controllers"></a>Contrôleurs de domaine physiques

Dans les centres de données, les contrôleurs de domaine physique doivent être installés dans les racks de sécurisé dédiés ou aux cages distincts de l’alimentation générale du serveur. Dans la mesure du possible, les contrôleurs de domaine doivent être configurés avec les processeurs de Module de plateforme sécurisée (TPM) et tous les volumes dans les serveurs de contrôleur de domaine doivent être protégées par le biais de chiffrement de lecteur BitLocker. BitLocker est généralement ajoute une surcharge de performances dans les pourcentages de chiffre, mais protège le répertoire contre toute compromission, même si les disques sont supprimés à partir du serveur. BitLocker peut également aider à protéger les systèmes contre les attaques telles que les rootkits, car la modification des fichiers de démarrage empêchera le serveur démarre en mode de récupération afin que les fichiers binaires d’origine peuvent être chargés. Si un contrôleur de domaine est configuré pour utiliser le logiciel RAID, serial attached SCSI, le stockage SAN/NAS, ou les volumes dynamiques, BitLocker ne peut pas être implémentés, par conséquent, stockage local (avec ou sans matériel RAID) doit être utilisé dans les contrôleurs de domaine chaque fois que possibles.  
  
#### <a name="virtual-domain-controllers"></a>Contrôleurs de domaine virtuel 

Si vous implémentez des contrôleurs de domaine virtuels, vous devez vous assurer que les contrôleurs de domaine exécutent sur des hôtes physiques distincts que les autres machines virtuelles dans l’environnement. Même si vous utilisez une plateforme de virtualisation de tiers, envisagez de déployer des contrôleurs de domaine virtuels sur le serveur Hyper-V dans Windows Server 2012 ou Windows Server 2008 R2, qui fournit une surface d’attaque minimale et peuvent être gérés avec les contrôleurs de domaine qu’il héberge et non gérés avec le reste des hôtes de virtualisation. Si vous implémentez System Center Virtual Machine Manager (SCVMM) pour la gestion de votre infrastructure de virtualisation, vous pouvez déléguer l’administration pour les hôtes physiques sur les machines virtuelles de contrôleur de domaine se trouvent et les contrôleurs de domaine eux-mêmes aux administrateurs autorisés. Vous devez également envisager de séparer le stockage des contrôleurs de domaine virtuel pour empêcher les administrateurs de stockage de l’accès aux fichiers de la machine virtuelle.  
  
### <a name="branch-locations"></a>Succursales  
  
#### <a name="physical-domain-controllers-in-branches"></a>Contrôleurs de domaine physiques dans les branches

Dans les emplacements dans lesquels plusieurs serveurs se trouvent, mais ne sont pas sécurisés physiquement au même niveau que les serveurs de centre de données sont sécurisées, les contrôleurs de domaine physiques doivent être configurées avec des processeurs TPM et le chiffrement de lecteur BitLocker pour tous les volumes du serveur. Si un contrôleur de domaine ne peut pas être stocké dans une pièce verrouillée dans les succursales, vous devez envisager de déployer des contrôleurs dans ces emplacements.  
  
#### <a name="virtual-domain-controllers-in-branches"></a>Contrôleurs de domaine virtuel dans les branches

Autant que possible, vous devez exécuter des contrôleurs de domaine virtuels au niveau des succursales sur des hôtes physiques distincts que les autres machines virtuelles dans le site. Dans les succursales dans lequel les contrôleurs de domaine virtuel ne peut pas exécuter sur des hôtes physiques distincts du reste de la population de serveur virtuel, vous devez implémenter les puces TPM et le chiffrement de lecteur BitLocker sur les hôtes sur lequel les contrôleurs de domaine virtuels exécutent au minimum, et tous les ordinateurs hôtes si possible. Selon la taille de la succursale et la sécurité des hôtes physiques, vous devez envisager de déployer des contrôleurs dans les succursales.  
  
### <a name="remote-locations-with-limited-space-and-security"></a>Emplacements distants avec un espace limité et la sécurité

Si votre infrastructure inclut les emplacements dans lesquels un seul serveur physique peut être installé, un serveur capable d’exécuter des charges de travail de virtualisation doit être installé dans l’emplacement distant, et le chiffrement de lecteur BitLocker doit être configuré pour protéger l’ensemble volumes dans le serveur. Un ordinateur virtuel sur le serveur doit s’exécuter sur un RODC, avec d’autres serveurs en cours d’exécution en tant que machines virtuelles distinctes sur l’ordinateur hôte. Informations sur la planification pour le déploiement de RODC est fourni dans le [Guide de déploiement et de planification de contrôleur de domaine en lecture seule](https://go.microsoft.com/fwlink/?LinkID=135993). Pour plus d’informations sur le déploiement et la sécurisation des contrôleurs de domaine virtualisés, consultez [en cours d’exécution de contrôleurs de domaine dans Hyper-V](https://technet.microsoft.com/library/dd363553(v=ws.10).aspx) sur le site Web TechNet. Pour plus d’informations pour le renforcement d’Hyper-V, délégation de la gestion de la machine virtuelle et la protection des machines virtuelles, consultez le [Guide de sécurité Hyper-V](https://www.microsoft.com/download/details.aspx?id=16650) accélérateur de Solution sur le site Web Microsoft.  
  
## <a name="domain-controller-operating-systems"></a>Systèmes d'exploitation de contrôleur de domaine

Vous devez exécuter tous les contrôleurs de domaine sur la version la plus récente de Windows Server qui est pris en charge au sein de votre organisation et de hiérarchiser la désaffectation des systèmes d’exploitation hérités de la population de contrôleur de domaine. En conservant des contrôleurs de domaine hérités actuelles et en éliminant vos contrôleurs de domaine, vous pouvez souvent tirer parti de nouvelles fonctionnalités et de sécurité qui ne peut-être pas être disponible dans les domaines ou forêts avec des contrôleurs de domaine exécutant le système d’exploitation hérité. Contrôleurs de domaine doivent être fraîchement installées et promues plutôt que mis à niveau à partir de systèmes d’exploitation précédents ou des rôles de serveur ; Autrement dit, n’effectuer des mises à niveau sur place des contrôleurs de domaine ni exécuter l’Assistant Installation d’Active Directory sur les serveurs sur lesquels le système d’exploitation n’est pas nouvellement installé. En implémentant des contrôleurs de domaine nouvellement installée, vous vous assurer que les paramètres et les anciens fichiers ne sont pas par inadvertance laissés sur les contrôleurs de domaine, et vous simplifier la mise en œuvre de la configuration du contrôleur de domaine cohérente et sécurisée.  
  
## <a name="secure-configuration-of-domain-controllers"></a>Configuration sécurisée des contrôleurs de domaine

De nombreux outils disponibles gratuitement, certains d'entre eux sont installés par défaut dans Windows, peut être utilisé pour créer une ligne de base de configuration de sécurité initiaux pour les contrôleurs de domaine qui peuvent ensuite être appliquées par stratégie de groupe. Ces outils sont décrits ici.  
  
### <a name="security-configuration-wizard"></a>Assistant Configuration de la sécurité  

Tous les contrôleurs de domaine doivent être verrouillées lors la génération initiale. Cela est possible à l’aide de l’Assistant de Configuration de sécurité qui est fourni en mode natif dans Windows Server pour configurer le service, paramètres du Registre, système et WFAS sur un contrôleur de domaine « build de base ». Paramètres peuvent être enregistrés et exportées vers un objet de stratégie de groupe qui peut être lié à l’UO de contrôleurs de domaine dans chaque domaine dans la forêt pour appliquer une configuration identique de contrôleurs de domaine. Si votre domaine contient plusieurs versions des systèmes d’exploitation de Windows, vous pouvez configurer des filtres Windows Management Instrumentation (WMI) pour appliquer la stratégie de groupe uniquement aux contrôleurs de domaine exécutant la version correspondante du système d’exploitation.  
  
### <a name="microsoft-security-compliance-toolkit"></a>Microsoft Security Compliance Toolkit

[Microsoft Security Compliance Toolkit](https://www.microsoft.com/download/details.aspx?id=55319) paramètres de contrôleur de domaine peuvent être combinés avec les paramètres de l’Assistant Configuration de sécurité pour produire des lignes de base de configuration complète pour les contrôleurs de domaine qui sont déployées et appliquées par la stratégie de groupe déployé au niveau de l’unité d’organisation des contrôleurs de domaine dans Active Directory.  
  
### <a name="rdp-restrictions"></a>Restrictions de RDP

Les objets de stratégie de groupe qui sont liées aux contrôleurs de domaine toutes les unités d’organisation dans une forêt doit être configurés pour autoriser des connexions RDP uniquement à partir de systèmes (par exemple, les serveurs jump) et les utilisateurs autorisés. Cela peut être obtenue via une combinaison de paramètres de droits d’utilisateur et la configuration de WFAS et doit être implémentée dans la stratégie de groupe afin que la stratégie est appliquée de manière cohérente. Si elle est ignorée, la prochaine actualisation de stratégie de groupe remet le système à sa configuration appropriée.  
  
### <a name="patch-and-configuration-management-for-domain-controllers"></a>Gestion de Configuration pour les contrôleurs de domaine et des correctifs

Bien que cela peut sembler illogique, vous devez envisager la mise à jour corrective de contrôleurs de domaine et d’autres composants d’infrastructure critiques séparément à partir de votre infrastructure générale de Windows. Si vous utilisez un logiciel de gestion de configuration enterprise pour tous les ordinateurs dans votre infrastructure, compromission du logiciel de gestion de systèmes permettre servir à compromettre ou détruire tous les composants d’infrastructure gérés par ce logiciel. En séparant la gestion des correctifs et des systèmes pour les contrôleurs de domaine à partir de la population en général, vous pouvez réduire la quantité de logiciels installés sur les contrôleurs de domaine, en plus de contrôler étroitement leur gestion.
  
### <a name="blocking-internet-access-for-domain-controllers"></a>Bloquant l’accès à Internet pour les contrôleurs de domaine  

Un des contrôles qui est effectuée dans le cadre d’une évaluation de sécurité Active Directory est l’utilisation et la configuration d’Internet Explorer sur les contrôleurs de domaine. Internet Explorer (ou tout autre navigateur web) ne doit pas être utilisé sur les contrôleurs de domaine, mais analyse de milliers de contrôleurs de domaine a révélé de nombreux cas dans lequel les utilisateurs disposant de privilèges utilisé Internet Explorer pour parcourir l’intranet de l’organisation ou le Internet.  
  
Comme décrit précédemment dans la section « Configuration incorrecte » de [voies de compromis](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md), navigation sur Internet (ou un intranet infecté) à partir d’un des ordinateurs plus puissants dans une infrastructure de Windows à l’aide de privilèges élevés compte (qui sont les seuls comptes autorisés à ouvrir une session localement aux contrôleurs de domaine par défaut) présente un risque exceptionnel à la sécurité d’une organisation. Que ce soit par un lecteur par téléchargement ou par téléchargement d’infectés « utilitaires », les attaquants peuvent accéder à tout ce que dont ils ont besoin pour complètement compromettre ou détruire l’environnement Active Directory.  
  
Bien que Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 et les versions actuelles d’Internet Explorer offrent un nombre de protections contre les téléchargements malveillants, dans la plupart des cas dans le domaine dans lequel les contrôleurs et les comptes privilégiés avaient été utilisés pour naviguer sur Internet, les contrôleurs de domaine ont été exécutant Windows Server 2003 ou les protections offertes par les navigateurs et systèmes d’exploitation plus récent avaient été intentionnellement désactivées.  
  
Lancement de navigateurs web sur les contrôleurs de domaine doit être interdit par la stratégie, mais par des contrôles techniques et les contrôleurs de domaine ne doivent pas être autorisés à accéder à Internet. Si vos contrôleurs de domaine est nécessaire pour la réplication entre des sites, vous devez implémenter des connexions sécurisées entre les sites. Bien que les instructions de configuration détaillées sont en dehors de la portée de ce document, vous pouvez implémenter un certain nombre de contrôles pour restreindre la capacité des contrôleurs de domaine pour être utilisés à mauvais escient ou mal configuré et compromis par la suite.  
  
### <a name="perimeter-firewall-restrictions"></a>Restrictions de pare-feu de périmètre

Pare-feu de périmètre doivent être configurés pour bloquer les connexions sortantes à partir de contrôleurs de domaine à Internet. Bien que les contrôleurs de domaine peuvent doivent communiquer au-delà des limites de site, les pare-feux de périmètre peuvent être configurés pour autoriser la communication intersite en suivant les instructions fournies dans [comment configurer un pare-feu pour les domaines et approbations](https://support.microsoft.com/kb/179442) sur le site Web de Support Microsoft.  
  
### <a name="dc-firewall-configurations"></a>Configurations de pare-feu de contrôleur de domaine  

Comme indiqué précédemment, vous devez utiliser l’Assistant de Configuration de sécurité pour capturer les paramètres de configuration du pare-feu Windows avec fonctions avancées de sécurité sur les contrôleurs de domaine. Vous devez examiner la sortie de l’Assistant de Configuration de sécurité pour vous assurer que les paramètres de configuration de pare-feu répondent aux besoins de votre organisation et ensuite utilisent des objets stratégie de groupe pour appliquer les paramètres de configuration.  
  
### <a name="preventing-web-browsing-from-domain-controllers"></a>Empêcher la navigation sur le Web à partir de contrôleurs de domaine

Vous pouvez utiliser une combinaison de configuration d’AppLocker, la configuration de proxy « trou noir » et configuration de WFAS pour empêcher les contrôleurs de domaine à partir de l’accès à Internet et pour empêcher l’utilisation des navigateurs web sur les contrôleurs de domaine.
