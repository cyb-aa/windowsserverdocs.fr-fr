---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: Sécurisation des contrôleurs de domaine contre les attaques
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 06/18/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: b91164e14f3ae94f6f7d01c62125f45124df0907
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367620"
---
# <a name="securing-domain-controllers-against-attack"></a>Sécurisation des contrôleurs de domaine contre les attaques

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Loi n ° 3 : si une personne malintentionnée dispose d’un accès physique illimité à votre ordinateur, ce n’est plus votre ordinateur.* - [dix lois immuables de sécurité (Version 2,0)](https://technet.microsoft.com/security/hh278941.aspx)  
  
Les contrôleurs de domaine fournissent le stockage physique pour la base de données AD DS, en plus de fournir les services et les données qui permettent aux entreprises de gérer efficacement leurs serveurs, stations de travail, utilisateurs et applications. Si un accès privilégié à un contrôleur de domaine est obtenu par un utilisateur malveillant, cet utilisateur peut modifier, corrompre ou détruire la base de données AD DS et, par extension, tous les systèmes et comptes gérés par Active Directory.  
  
Dans la mesure où les contrôleurs de domaine peuvent lire et écrire dans la base de données AD DS, la compromission d’un contrôleur de domaine signifie que votre forêt Active Directory ne peut jamais être considérée comme digne de confiance, sauf si vous êtes en mesure de récupérer à l’aide d’une sauvegarde correcte connue et de Fermez les lacunes qui ont permis de compromettre le processus.  
  
Selon la préparation, les outils et les compétences d’un pirate, la modification ou même les dommages irréparables de la base de données de AD DS peuvent être effectués en quelques minutes, voire en quelques heures, et non en plusieurs jours ou semaines. Il ne s’agit pas de la durée pendant laquelle une personne malveillante dispose d’un accès privilégié à Active Directory, mais le niveau de la personne malveillante a prévu le moment où un accès privilégié est obtenu. Compromettre un contrôleur de domaine peut fournir le chemin le plus rapide pour la propagation d’accès à grande échelle, ou le chemin le plus direct à la destruction des serveurs membres, des stations de travail et des Active Directory. Pour cette raison, les contrôleurs de domaine doivent être sécurisés séparément et de manière plus stricte que l’infrastructure Windows générale.  

## <a name="physical-security-for-domain-controllers"></a>Sécurité physique pour les contrôleurs de domaine

Cette section fournit des informations sur la sécurisation physique des contrôleurs de domaine, si les contrôleurs de domaine sont des machines physiques ou virtuelles, dans des emplacements de centres de données, des succursales et même des emplacements distants avec des contrôles d’infrastructure de base uniquement.  
  
### <a name="datacenter-domain-controllers"></a>Contrôleurs de domaine Datacenter  
  
#### <a name="physical-domain-controllers"></a>Contrôleurs de domaine physiques

Dans les centres de centres, les contrôleurs de domaine physiques doivent être installés dans des racks sécurisés dédiés ou des cages distinctes de la population générale du serveur. Lorsque cela est possible, les contrôleurs de domaine doivent être configurés avec des puces de Module de plateforme sécurisée (TPM) (TPM) et tous les volumes des serveurs de contrôleur de domaine doivent être protégés via Chiffrement de lecteur BitLocker. BitLocker ajoute généralement une surcharge de performance dans les pourcentages à un seul chiffre, mais protège le répertoire contre toute compromission, même si les disques sont supprimés du serveur. BitLocker peut également aider à protéger les systèmes contre les attaques telles que les rootkits, car la modification des fichiers de démarrage entraîne le démarrage du serveur en mode de récupération afin que les binaires d’origine puissent être chargés. Si un contrôleur de domaine est configuré pour utiliser un RAID logiciel, un SCSI attaché en série, un stockage SAN/NAS ou des volumes dynamiques, BitLocker ne peut pas être implémenté, de sorte que le stockage attaché localement (avec ou sans RAID matériel) doit être utilisé dans les contrôleurs de domaine dans la mesure du possible.  
  
#### <a name="virtual-domain-controllers"></a>Contrôleurs de domaine virtuels 

Si vous implémentez des contrôleurs de domaine virtuels, vous devez vous assurer que les contrôleurs de domaine s’exécutent sur des hôtes physiques distincts des autres ordinateurs virtuels de l’environnement. Même si vous utilisez une plateforme de virtualisation tierce, envisagez de déployer des contrôleurs de domaine virtuels sur le serveur Hyper-V dans Windows Server 2012 ou Windows Server 2008 R2, ce qui constitue une surface d’attaque minimale et peut être géré avec les contrôleurs de domaine qu’il héberge. plutôt que d’être gérées avec le reste des hôtes de virtualisation. Si vous implémentez System Center Virtual Machine Manager (SCVMM) pour la gestion de votre infrastructure de virtualisation, vous pouvez déléguer l’administration pour les hôtes physiques sur lesquels résident les machines virtuelles de contrôleur de domaine et les contrôleurs de domaine. eux-mêmes aux administrateurs autorisés. Vous devez également envisager de séparer le stockage des contrôleurs de domaine virtuels pour empêcher les administrateurs de stockage d’accéder aux fichiers de l’ordinateur virtuel.  
  
### <a name="branch-locations"></a>Emplacements des branches  
  
#### <a name="physical-domain-controllers-in-branches"></a>Contrôleurs de domaine physiques dans les branches

Dans les emplacements où résident plusieurs serveurs, mais qui ne sont pas physiquement sécurisés au niveau de la sécurisation des serveurs de centres de donnés, les contrôleurs de domaine physiques doivent être configurés avec des puces TPM et des Chiffrement de lecteur BitLocker pour tous les volumes de serveur. Si un contrôleur de domaine ne peut pas être stocké dans une salle verrouillée dans les filiales, vous devez envisager de déployer des contrôleurs de domaine en lecture seule à ces emplacements.  
  
#### <a name="virtual-domain-controllers-in-branches"></a>Contrôleurs de domaine virtuels dans les branches

Dans la mesure du possible, vous devez exécuter les contrôleurs de domaine virtuels dans les filiales situées sur des hôtes physiques distincts des autres machines virtuelles du site. Dans les filiales où les contrôleurs de domaine virtuels ne peuvent pas s’exécuter sur des hôtes physiques distincts du reste de la population de serveurs virtuels, vous devez implémenter des puces TPM et des Chiffrement de lecteur BitLocker sur les hôtes sur lesquels les contrôleurs de domaine virtuels s’exécutent au minimum, et tous les ordinateurs hôtes, si possible. En fonction de la taille de la succursale et de la sécurité des hôtes physiques, vous devez envisager de déployer des RODC dans des succursales.  
  
### <a name="remote-locations-with-limited-space-and-security"></a>Emplacements distants avec un espace et une sécurité limités

Si votre infrastructure comprend des emplacements dans lesquels un seul serveur physique peut être installé, un serveur capable d’exécuter des charges de travail de virtualisation doit être installé à l’emplacement distant, et Chiffrement de lecteur BitLocker doivent être configurés pour protéger tout volumes du serveur. Un ordinateur virtuel sur le serveur doit exécuter un RODC, avec d’autres serveurs qui s’exécutent en tant qu’ordinateurs virtuels distincts sur l’ordinateur hôte. Vous trouverez des informations sur la planification du déploiement de RODC dans le [Guide de planification et de déploiement du contrôleur de domaine en lecture seule](https://go.microsoft.com/fwlink/?LinkID=135993). Pour plus d’informations sur le déploiement et la sécurisation des contrôleurs de domaine virtualisés, voir [exécution de contrôleurs de domaine dans Hyper-V](https://technet.microsoft.com/library/dd363553(v=ws.10).aspx) sur le site Web TechNet. Pour obtenir des conseils plus détaillés sur la sécurisation de Hyper-V, la délégation de la gestion des ordinateurs virtuels et la protection des machines virtuelles, consultez le [Guide de sécurité Hyper-v](https://www.microsoft.com/download/details.aspx?id=16650) sur le site Web de Microsoft.  
  
## <a name="domain-controller-operating-systems"></a>Systèmes d'exploitation de contrôleur de domaine

Vous devez exécuter tous les contrôleurs de domaine sur la version la plus récente de Windows Server prise en charge au sein de votre organisation et définir la priorité de désaffectation des systèmes d’exploitation hérités dans la population du contrôleur de domaine. En maintenant vos contrôleurs de domaine à jour et en éliminant les contrôleurs de domaine hérités, vous pouvez souvent tirer parti de nouvelles fonctionnalités et de la sécurité qui ne sont peut-être pas disponibles dans les domaines ou les forêts avec les contrôleurs de domaine exécutant le système d’exploitation hérité. Les contrôleurs de domaine doivent être installés et promus au lieu d’être mis à niveau à partir de systèmes d’exploitation ou de rôles de serveur antérieurs. autrement dit, n’effectuez pas de mises à niveau sur place de contrôleurs de domaine ou exécutez l’Assistant Installation de AD DS sur des serveurs sur lesquels le système d’exploitation n’est pas installé. En implémentant les contrôleurs de domaine récemment installés, vous vous assurez que les fichiers et les paramètres hérités ne sont pas laissés par inadvertance sur les contrôleurs de domaine et que vous simplifiez l’application d’une configuration de contrôleur de domaine sécurisée et cohérente.  
  
## <a name="secure-configuration-of-domain-controllers"></a>Configuration sécurisée des contrôleurs de domaine

Un certain nombre d’outils disponibles gratuitement, dont certains sont installés par défaut dans Windows, peuvent être utilisés pour créer une ligne de base de configuration de sécurité initiale pour les contrôleurs de domaine qui peuvent être appliqués par la suite par des objets de stratégie de groupe. Ces outils sont décrits ici.  
  
### <a name="security-configuration-wizard"></a>Assistant Configuration de la sécurité  

Tous les contrôleurs de domaine doivent être verrouillés lors de la génération initiale. Pour ce faire, utilisez l’Assistant Configuration de la sécurité qui est fourni en mode natif dans Windows Server pour configurer les paramètres de service, de Registre, de système et de WFAS sur un contrôleur de domaine « Build de base ». Les paramètres peuvent être enregistrés et exportés vers un objet de stratégie de groupe qui peut être lié à l’unité d’organisation contrôleurs de domaine dans chaque domaine de la forêt pour appliquer une configuration cohérente des contrôleurs de domaine. Si votre domaine contient plusieurs versions des systèmes d’exploitation Windows, vous pouvez configurer des filtres d’Windows Management Instrumentation (WMI) pour appliquer des objets de stratégie de groupe uniquement aux contrôleurs de domaine qui exécutent la version correspondante du système d’exploitation.  
  
### <a name="microsoft-security-compliance-toolkit"></a>Microsoft Security Compliance Toolkit

Les paramètres du contrôleur de domaine [Microsoft Security Compliance Toolkit](https://www.microsoft.com/download/details.aspx?id=55319) peuvent être combinés avec les paramètres de l’Assistant Configuration de la sécurité pour produire des lignes de base de configuration complètes pour les contrôleurs de domaine déployés et appliqués par les objets de stratégie de groupe déployés dans l’unité d’organisation des contrôleurs de domaine dans Active Directory.  
  
### <a name="rdp-restrictions"></a>Restrictions RDP

Stratégie de groupe objets qui sont liés à toutes les unités d’organisation des contrôleurs de domaine dans une forêt doivent être configurés pour autoriser les connexions RDP uniquement à partir des utilisateurs et des systèmes autorisés (par exemple, les serveurs de sauts). Pour ce faire, vous pouvez utiliser une combinaison de paramètres de droits d’utilisateur et de configuration WFAS et l’implémenter dans les objets de stratégie de groupe afin que la stratégie soit appliquée de manière cohérente. Si elle est ignorée, la prochaine actualisation stratégie de groupe rétablit le système à sa configuration appropriée.  
  
### <a name="patch-and-configuration-management-for-domain-controllers"></a>Gestion des correctifs et des configurations pour les contrôleurs de domaine

Bien qu’il puisse paraître non intuitifs, vous devez envisager de mettre à jour les contrôleurs de domaine et d’autres composants d’infrastructure critiques séparément de votre infrastructure Windows générale. Si vous utilisez un logiciel de gestion de la configuration d’entreprise pour tous les ordinateurs de votre infrastructure, une compromission du logiciel de gestion des systèmes peut être utilisée pour compromettre ou détruire tous les composants d’infrastructure gérés par ce logiciel. En séparant la gestion des correctifs et des systèmes pour les contrôleurs de domaine de la population générale, vous pouvez réduire la quantité de logiciels installés sur les contrôleurs de domaine, en plus de contrôler étroitement leur gestion.
  
### <a name="blocking-internet-access-for-domain-controllers"></a>Blocage de l’accès Internet pour les contrôleurs de domaine  

L’une des vérifications effectuées dans le cadre d’une évaluation de la sécurité de Active Directory est l’utilisation et la configuration d’Internet Explorer sur les contrôleurs de domaine. Internet Explorer (ou tout autre navigateur Web) ne doit pas être utilisé sur les contrôleurs de domaine, mais l’analyse de milliers de contrôleurs de domaine a révélé de nombreux cas dans lesquels les utilisateurs privilégiés utilisaient Internet Explorer pour parcourir l’intranet de l’organisation ou le Internet.  
  
Comme décrit précédemment dans la section « Configuration inutilisable » de [avenues pour compromettre](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md), parcourir Internet (ou un intranet infecté) à partir de l’un des ordinateurs les plus puissants dans une infrastructure Windows à l’aide d’un compte doté de privilèges élevés (qui sont les seuls comptes autorisés à ouvrir une session localement sur les contrôleurs de domaine par défaut) présente un risque exceptionnel pour la sécurité d' Que ce soit via un lecteur par téléchargement ou par téléchargement d’utilitaires malveillants, les attaquants peuvent accéder à tout ce dont ils ont besoin pour compromettre ou détruire complètement l’environnement Active Directory.  
  
Bien que Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 et les versions actuelles d’Internet Explorer offrent un certain nombre de protections contre les téléchargements malveillants, dans la plupart des cas, les contrôleurs de domaine et les comptes privilégiés ont été utilisés pour Parcourez Internet, les contrôleurs de domaine exécutant Windows Server 2003 ou les protections offertes par les systèmes d’exploitation et les navigateurs plus récents ont été désactivés intentionnellement.  
  
Le lancement de navigateurs Web sur des contrôleurs de domaine doit être interdit non seulement par la stratégie, mais par les contrôles techniques, et les contrôleurs de domaine ne doivent pas être autorisés à accéder à Internet. Si vos contrôleurs de domaine doivent être répliqués sur plusieurs sites, vous devez implémenter des connexions sécurisées entre les sites. Bien que les instructions de configuration détaillées n’entrent pas dans le cadre de ce document, vous pouvez implémenter un certain nombre de contrôles pour limiter la capacité des contrôleurs de domaine à être mal utilisés ou mal configurés et par la suite compromis.  
  
### <a name="perimeter-firewall-restrictions"></a>Restrictions de pare-feu de périmètre

Les pare-feu de périmètre doivent être configurés pour bloquer les connexions sortantes entre les contrôleurs de domaine et Internet. Bien que les contrôleurs de domaine aient besoin de communiquer au-delà des limites du site, les pare-feu de périmètre peuvent être configurés pour autoriser la communication intersite en suivant les instructions fournies dans [Comment configurer un pare-feu pour les domaines et les approbations](https://support.microsoft.com/kb/179442) sur le site Web Support Microsoft.  
  
### <a name="dc-firewall-configurations"></a>Configurations de pare-feu DC  

Comme décrit précédemment, vous devez utiliser l’Assistant Configuration de la sécurité pour capturer les paramètres de configuration du pare-feu Windows avec fonctions avancées de sécurité sur les contrôleurs de domaine. Vous devez examiner la sortie de l’Assistant Configuration de la sécurité pour vous assurer que les paramètres de configuration du pare-feu répondent aux besoins de votre organisation, puis utiliser les objets de stratégie de groupe pour appliquer les paramètres de configuration.  
  
### <a name="preventing-web-browsing-from-domain-controllers"></a>Empêcher la navigation Web à partir de contrôleurs de domaine

Vous pouvez utiliser une combinaison de configuration AppLocker, de configuration de proxy « trou noir » et de configuration WFAS pour empêcher les contrôleurs de domaine d’accéder à Internet et empêcher l’utilisation de navigateurs Web sur les contrôleurs de domaine.
