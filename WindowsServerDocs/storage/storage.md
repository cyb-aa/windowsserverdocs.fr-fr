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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833800"
---
# <a name="storage"></a>Stockage

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel)

>[!TIP]
> Vous recherchez des informations sur des versions plus anciennes de Windows Server ? Consultez nos autres [bibliothèques Windows Server](/previous-versions/windows/) sur docs.microsoft.com. Vous pouvez également [rechercher dans ce site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) des informations spécifiques.

<hr />
Le stockage dans Windows Server propose des fonctionnalités nouvelles et améliorées pour les clients de centres de données définis par logiciel (SDDC) privilégiant les charges de travail virtualisées. Windows Server assure aussi une prise en charge étendue pour les clients d’entreprise qui utilisent des serveurs de fichiers comprenant des charges de travail existantes.

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
                                            <h2>Quelles sont les nouveautés ?</h2>
                                            <p>Découvrez quelles sont les nouveautés dans le stockage de Windows Server</p>
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
                        <p><h3><a href="storage-spaces/storage-spaces-direct-overview.md">Espaces de stockage Direct</a></h3> Attaché directement le stockage local, y compris les appareils SATA et NVME, pour optimiser l’utilisation du disque après l’ajout de nouveaux disques physiques et plus rapidement le disque virtuel réparer fois. Consultez également les <a href="storage-spaces/overview.md">Espaces de stockage</a> pour des informations sur les espaces de stockage SAS partagé et autonome.</p>
<HR />
                        <p><h3><a href="storage-replica/storage-replica-overview.md">Réplica de stockage</a></h3> Indépendante du stockage, au niveau du bloc réplication synchrone entre des clusters ou serveurs pour la préparation aux situations d’urgence et de récupération, ainsi étirer un cluster de basculement entre des sites pour la haute disponibilité. La réplication synchrone permet la mise en miroir des données dans des sites physiques avec des volumes cohérents en cas d’incident, ce qui garantit aucune perte de données au niveau du système de fichiers.</p>
<HR />
                        <p><h3><a href="storage-qos/storage-qos-overview.md">Qualité de Service (QoS) de stockage</a></h3> Surveiller et gérer les performances de stockage pour les machines virtuelles à l’aide de Hyper-V et les rôles de serveur de fichiers avec montée en puissance automatiquement de manière centralisée improveing équité des ressources de stockage entre plusieurs machines virtuelles avec le même cluster de serveur de fichiers.</p>
<HR />
                        <p><h3><a href="data-deduplication/overview.md">Déduplication des données</a></h3> Optimise l’espace libre sur un volume en examinant les données sur le volume pour la duplication. Une fois identifiées, les parties dupliquées du jeu de données du volume sont stockées une seule fois et sont (éventuellement) compressées pour économiser encore davantage. La déduplication des données permet d’optimiser les redondances sans compromettre la fidélité ou l’intégrité des données.</p>
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
                        <p><h3><a href="storage-migration-service/overview.md">Service de Migration de stockage</a></h3>Migrer des serveurs à une version plus récente de Windows Server à l’aide d’un outil graphique qui inventorie les données sur les serveurs, transfère les données et la configuration sur de nouveaux serveurs et éventuellement déplace ensuite les identités des anciens serveurs vers les nouveaux serveurs ainsi que les applications et les utilisateurs sans avoir à modifier quoi que ce soit.</p>
<HR />
                        <p><h3><a href="work-folders/work-folders-overview.md">Dossiers de travail</a></h3> Store et l’accès des fichiers sur les ordinateurs personnels et les périphériques, communément appelée apportez votre propre appareil (BYOD), en plus des PC d’entreprise de travail. Les utilisateurs bénéficient ainsi d’un emplacement pratique pour stocker les fichiers de travail et y accéder en tout lieu. Les organisations gardent la mainmise sur les données d’entreprise en stockant les fichiers sur des serveurs de fichiers gérés de manière centralisée et en spécifiant éventuellement des stratégies de périphérique utilisateur (par exemple, mots de passe de chiffrement et d’écran de verrouillage).</p>
<HR />
                        <p><h3><a href="folder-redirection/folder-redirection-rup-overview.md">Redirection de dossiers et fichiers hors ligne</a></h3> Rediriger le chemin d’accès de dossiers locaux (tels que le dossier Documents) vers un emplacement réseau, lors de la mise en cache le contenu localement pour une vitesse accrue et la disponibilité.</p>
<HR />
                        <p><h3><a href="folder-redirection/deploy-roaming-user-profiles.md">Les profils utilisateur itinérants</a></h3> Rediriger un profil utilisateur vers un emplacement réseau.</p>
<HR />
                        <p><h3><a href="dfs-namespaces/dfs-overview.md">Espaces de noms DFS</a></h3> Regrouper des dossiers partagés qui sont trouvent sur des serveurs différents en un ou plusieurs espaces de noms logiquement structurés. Pour les utilisateurs, chaque espace de noms apparaît sous la forme d’un dossier partagé unique avec une série de sous-dossiers. Toutefois, la structure sous-jacente de l’espace de noms peut être composée de nombreux partages de fichiers situés sur différents serveurs, dans plusieurs sites.</p>
<HR />
                        <p><h3><a href="dfs-replication/dfsr-overview.md">Réplication DFS</a></h3> Répliquer les dossiers (y compris ceux indiqués dans un chemin d’espace de noms DFS) sur plusieurs serveurs et sites. La réplication DFS utilise un algorithme de compression appelé compression différentielle à distance (RDC). L’algorithme RDC détecte les changements de données dans un fichier et permet à la réplication DFS de répliquer uniquement les blocs de fichiers modifiés à la place du fichier entier.</p>
<HR />
                        <p><h3><a href="fsrm/fsrm-overview.md">File Server Resource Manager</a></h3> Gérer et classer les données stockées sur les serveurs de fichiers.<p>
<HR />
                        <p><h3><a href="iscsi/iscsi-target-server.md">Serveur cible iSCSI</a></h3> Donne à d’autres serveurs et applications sur le réseau la possibilité de prendre en charge le stockage par blocs à l’aide de la norme iSCSI (Internet SCSI).</p>
<HR />
                       <p><h3><a href="iscsi/iscsi-boot-overview.md">Serveur cible iSCSI</a></h3> Vous pouvez démarrer des centaines d’ordinateurs à partir d’une image de système d’exploitation unique qui est stocké dans un emplacement centralisé. Cela contribue à améliorer l’efficacité, la simplicité de gestion, la disponibilité et la sécurité.</p>
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
                        <p><h3><a href="refs/refs-overview.md">ReFS</a></h3> Un système de fichiers résilient qui optimise la disponibilité des données, s’adapter efficacement aux jeux de données très volumineux dans diverses charges de travail et fournit l’intégrité des données au moyen d’une résilience aux dommages (quelles que soient les défaillances logicielles ou matérielles).<p>
<HR />
                        <p><h3><a href="file-server/file-server-smb-overview.md">Protocole de Server Message Block (SMB)</a></h3> Un fichier réseau partage de protocole qui permet aux applications sur un ordinateur pour lire et écrire dans des fichiers et pour demander des services à partir de programmes serveur sur un réseau d’ordinateurs. Le protocole SMB peut être utilisé en plus du protocole TCP/IP ou d‘autres protocoles réseau. Grâce à lui, une application (ou bien l’utilisateur d’une application) peut accéder à des fichiers ou d’autres ressources sur un serveur distant. Les applications peuvent ainsi lire, créer et mettre à jour des fichiers sur le serveur distant. Il permet aussi de communiquer avec n’importe quel programme serveur configuré pour recevoir une demande de clientSMB.<p>
<HR />
                        <p><h3><a href="storage-spaces/Storage-class-memory-health.md">Mémoire de classe de stockage</a></h3> Fournit des performances similaires à la mémoire de l’ordinateur (rapide), mais avec la persistance des données des disques de stockage normal. Windows traite la mémoire de classe stockage de la même manière que les lecteurs classiques (avec simplement plus de rapidité), mais il existe quelques différences dans la façon dont l’intégrité des appareils est gérée.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/cc766295(v=ws.10).aspx">Chiffrement de lecteur BitLocker</a></h3> Stocke les données sur les volumes dans un format chiffré, même si l’ordinateur est trafiqué lorsque le système d’exploitation n’est pas en cours d’exécution. Cela permet de se prémunir contre les attaques dites hors connexion, qui résultent d’une désactivation ou d’un contournement du système d’exploitation installé ou découlent du retrait physique du disque dur en vue de s’attaquer aux données séparément.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/dn466522(v=ws.11).aspx">NTFS</a></h3> Le système de fichiers principal pour les versions récentes de Windows et Windows Server, fournit un ensemble complet de fonctionnalités, notamment les descripteurs de sécurité, le chiffrement, les quotas de disque et des métadonnées enrichies et peut être utilisé avec des Volumes CSV (Cluster Shared) pour fournir en permanence volumes disponibles qui peuvent accéder simultanément à partir de plusieurs nœuds d’un Cluster de basculement.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/jj592688(v=ws.11).aspx">Système de fichiers réseau (NFS)</a></h3> Fournit une solution pour les entreprises qui disposent d’environnements hétérogènes constitués de Windows et les ordinateurs non Windows de partage de fichiers.<p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

---


## <a name="in-azure"></a>Dans Azure

* [Stockage Azure](https://azure.microsoft.com/documentation/services/storage/)
* [Azure StorSimple](https://www.microsoft.com/en-us/cloud-platform/azure-storsimple)
