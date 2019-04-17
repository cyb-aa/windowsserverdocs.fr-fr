---
title: Stockage
description: ''
author: JasonGerend
manager: elizapo
layout: LandingPage
ms.prod: windows-server-threshold
ms.technology: storage
ms.assetid: 6b74bc7c-a58d-4915-af8e-2cc27f2c4726
ms.topic: landing-page
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 03/08/2019
ms.openlocfilehash: d83d51ebf56d38f93c176d403ea5c6c14f625ee2
ms.sourcegitcommit: 419bf9d6d18e7a32cba2cbcd98587b81a79adc51
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/11/2019
ms.locfileid: "9152176"
---
# Stockage

>S'applique à: WindowsServer2019, WindowsServer2016, WindowsServer (canal semi-annuel)

>[!TIP]
> Vous recherchez des informations sur des versions plus anciennes de WindowsServer? Consultez nos autres [bibliothèques Windows Server](/previous-versions/windows/) sur docs.microsoft.com. Vous pouvez également [rechercher dans ce site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) des informations spécifiques.

<hr />
Le stockage dans WindowsServer propose des fonctionnalités nouvelles et améliorées pour les clients de centres de données définis par logiciel (SDDC) privilégiant les charges de travail virtualisées. WindowsServer assure aussi une prise en charge étendue pour les clients d’entreprise qui utilisent des serveurs de fichiers comprenant des charges de travail existantes.

<hr />
<ul class="cardsF panelContent">
<li>
 <a href="whats-new-in-storage.md">
                            <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-whats-new.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                            <h2>Quelles sont les nouveautés?</h2>
                                            <p>Découvrir les nouveautés du stockage Windows Server</p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
</ul>
<hr />
<ul class="cardsF panelContent">
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Stockage défini par logiciel pour les charges de travail virtualisées</h3>
<HR />
                        <p><h3><a href="storage-spaces/storage-spaces-direct-overview.md">Espaces de stockage direct</a></h3> Attachés directement le stockage local, y compris les périphériques SATA et NVME, pour optimiser l’utilisation des disques après avoir ajouté de nouveaux disques physiques et de réparer les heures pour accélérer le disque virtuel. Consultez également les <a href="storage-spaces/overview.md">Espaces de stockage</a> pour des informations sur les espaces de stockage SAS partagé et autonome.</p>
<HR />
                        <p><h3><a href="storage-replica/storage-replica-overview.md">Réplica de stockage</a></h3> Indépendante du stockage, au niveau du bloc une réplication synchrone entre les clusters ou les serveurs de préparation aux situations d’urgence et de récupération, aussi bien un étirement d’un cluster de basculement sur plusieurs sites de la haute disponibilité. La réplication synchrone permet la mise en miroir des données dans des sites physiques avec des volumes cohérents en cas d’incident, ce qui garantit aucune perte de données au niveau du système de fichiers.</p>
<HR />
                        <p><h3><a href="storage-qos/storage-qos-overview.md">Qualité de service de stockage</a></h3> Surveiller et gérer les performances de stockage pour les machines virtuelles à l’aide d’Hyper-V et les rôles de serveur de fichiers avec montée en automatiquement de manière centralisée improveing équité des ressources de stockage entre plusieurs machines virtuelles à l’aide de la même cluster de serveurs de fichiers.</p>
<HR />
                        <p><h3><a href="data-deduplication/overview.md">Déduplication des données</a></h3> Optimise l’espace libre sur un volume en examinant les données sur le volume d’être dupliqué. Une fois identifiées, les parties dupliquées du jeu de données du volume sont stockées une seule fois et sont (éventuellement) compressées pour économiser encore davantage. La déduplication des données permet d’optimiser les redondances sans compromettre la fidélité ou l’intégrité des données.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Serveurs de fichiers à usage général</h3>
<HR />
                        <p><h3><a href="storage-migration-service/overview.md">Service de migration du stockage</a></h3>Migrer des serveurs à une version plus récente de Windows Server à l’aide d’un outil graphique qui répertorie les données sur les serveurs, transfère les données et la configuration de serveurs récents et éventuellement déplace ensuite les identités des anciens serveurs vers les nouveaux serveurs ainsi que les applications et les utilisateurs n’avez pas à modifier quoi que ce soit.</p>
<HR />
                        <p><h3><a href="work-folders/work-folders-overview.md">Dossiers de travail</a></h3> Windows Store et l’accès fichiers de travail sur les ordinateurs personnels et les périphériques, souvent appelés apportez votre propre appareil (BYOD), en plus des PC d’entreprise. Les utilisateurs bénéficient ainsi d’un emplacement pratique pour stocker les fichiers de travail et y accéder en tout lieu. Les organisations gardent la mainmise sur les données d’entreprise en stockant les fichiers sur des serveurs de fichiers gérés de manière centralisée et en spécifiant éventuellement des stratégies de périphérique utilisateur (par exemple, mots de passe de chiffrement et d’écran de verrouillage).</p>
<HR />
                        <p><h3><a href="folder-redirection/folder-redirection-rup-overview.md">Fichiers hors connexion et Redirection de dossiers</a></h3> Rediriger le chemin d’accès des dossiers locaux (par exemple, le dossier Documents) vers un emplacement réseau, tout le contenu localement pour augmenter la vitesse et la disponibilité de la mise en cache.</p>
<HR />
                        <p><h3><a href="folder-redirection/deploy-roaming-user-profiles.md">Les profils utilisateur itinérants</a></h3> Rediriger un profil utilisateur vers un emplacement réseau.</p>
<HR />
                        <p><h3><a href="dfs-namespaces/dfs-overview.md">Espaces de nomsDFS</a></h3> Grouper des dossiers partagés qui sont trouvent sur des serveurs différents en un ou plusieurs espaces de noms logiquement structurés. Pour les utilisateurs, chaque espace de noms apparaît sous la forme d’un dossier partagé unique avec une série de sous-dossiers. Toutefois, la structure sous-jacente de l’espace de noms peut être composée de nombreux partages de fichiers situés sur différents serveurs, dans plusieurs sites.</p>
<HR />
                        <p><h3><a href="dfs-replication/dfsr-overview.md">Réplication DFS</a></h3> Répliquer des dossiers (y compris ceux indiqués dans un chemin d’espace de noms DFS) sur plusieurs serveurs et sites. La réplication DFS utilise un algorithme de compression appelé compression différentielle à distance (RDC). L’algorithme RDC détecte les changements de données dans un fichier et permet à la réplication DFS de répliquer uniquement les blocs de fichiers modifiés à la place du fichier entier.</p>
<HR />
                        <p><h3><a href="fsrm/fsrm-overview.md">Gestionnaire de ressources du serveur de fichiers</a></h3> Gérer et de classer les données stockées sur des serveurs de fichiers.<p>
<HR />
                        <p><h3><a href="iscsi/iscsi-target-server.md">Serveur cible iSCSI</a></h3> Fournit le stockage par blocs à d’autres serveurs et applications sur le réseau à l’aide de la norme Internet SCSI (iSCSI).</p>
<HR />
                       <p><h3><a href="iscsi/iscsi-boot-overview.md">Serveur cible iSCSI</a></h3> Peut démarrer des centaines d’ordinateurs à partir d’une image de système d’exploitation unique qui est stocké dans un emplacement centralisé. Cela contribue à améliorer l’efficacité, la simplicité de gestion, la disponibilité et la sécurité.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Systèmes de fichiers, protocoles, etc.</h3>
<HR />
                        <p><h3><a href="refs/refs-overview.md">ReFS</a></h3> Un système de fichiers résilient qui optimise la disponibilité des données, s’adapte efficacement aux jeux de données très volumineux sur diverses charges de travail et garantit l’intégrité des données au moyen de résilience aux altérations (quelles que soient les défaillances logicielles ou matérielles).<p>
<HR />
                        <p><h3><a href="file-server/file-server-smb-overview.md">Protocole de Server Message Block (SMB)</a></h3> Un protocole qui permet aux applications sur un ordinateur pour lire et écrire des fichiers et de solliciter des services auprès de programmes serveur sur un réseau informatique de partage de fichiers réseau. Le protocole SMB peut être utilisé en plus du protocole TCP/IP ou d‘autres protocoles réseau. Grâce à lui, une application (ou bien l’utilisateur d’une application) peut accéder à des fichiers ou d’autres ressources sur un serveur distant. Les applications peuvent ainsi lire, créer et mettre à jour des fichiers sur le serveur distant. Il permet aussi de communiquer avec n’importe quel programme serveur configuré pour recevoir une demande de clientSMB.<p>
<HR />
                        <p><h3><a href="storage-spaces/Storage-class-memory-health.md">Mémoire de classe stockage</a></h3> Fournit des performances comparables à la mémoire de l’ordinateur (en termes de rapidité), mais avec la persistance des données des lecteurs de capacité de stockage. Windows traite la mémoire de classe stockage de la même manière que les lecteurs classiques (avec simplement plus de rapidité), mais il existe quelques différences dans la façon dont l’intégrité des appareils est gérée.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/cc766295(v=ws.10).aspx">Chiffrement de lecteur BitLocker</a></h3> Stocke les données sur les volumes dans un format chiffré, même si l’ordinateur est falsifié ou que le système d’exploitation n’est pas en cours d’exécution. Cela permet de se prémunir contre les attaques dites hors connexion, qui résultent d’une désactivation ou d’un contournement du système d’exploitation installé ou découlent du retrait physique du disque dur en vue de s’attaquer aux données séparément.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/dn466522(v=ws.11).aspx">NTFS</a></h3> Le système de fichiers principal des versions récentes de Windows et Windows Server, offre un ensemble complet de fonctionnalités, notamment des descripteurs de sécurité, le chiffrement, les quotas de disque et des métadonnées enrichies et peut être utilisé avec Cluster Shared Volumes (CSV) pour fournir en permanence volumes disponibles qui sont accessibles simultanément à partir de plusieurs nœuds d’un Cluster de basculement.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/jj592688(v=ws.11).aspx">Système de fichiers réseau (NFS)</a></h3> Fournit une solution pour les entreprises qui disposent d’environnements hétérogènes constitués Windows et des ordinateurs non-Windows de partage de fichiers.<p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

---


## Dans Azure

* [Stockage Azure](https://azure.microsoft.com/documentation/services/storage/)
* [Azure StorSimple](https://www.microsoft.com/en-us/cloud-platform/azure-storsimple)
