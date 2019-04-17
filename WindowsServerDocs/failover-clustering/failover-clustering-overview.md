---
ms.assetid: c9844427-27cf-4d76-b5bb-e06368b092f7
title: Clustering de basculement
ms.prod: windows-server-threshold
layout: LandingPage
ms.topic: landing-page
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 03/08/2019
ms.localizationpriority: high
ms.openlocfilehash: 445de065ff5b68b83481ee5bd83ebf18fdd180a7
ms.sourcegitcommit: b0fece76b871da3fa9d6a996798a5008756f486b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "9178600"
---
# Clustering de basculement dans WindowsServer

> S'applique à: WindowsServer2019, WindowsServer2016, WindowsServer (canal semi-annuel)

>[!TIP]
> Vous recherchez des informations sur des versions plus anciennes de WindowsServer? Consultez nos autres [bibliothèques Windows Server](/previous-versions/windows/) sur docs.microsoft.com. Vous pouvez également [rechercher dans ce site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) des informations spécifiques.

<hr />

Un cluster de basculement est un groupe d’ordinateurs indépendants qui travaillent conjointement pour accroître la disponibilité et l’extensibilité des rôles en cluster (appelés précédemment « applications et services en cluster »). Les serveurs en cluster (appelés « nœuds ») sont connectés par des câbles physiques et par des logiciels. En cas de défaillance d’un ou plusieurs nœuds, d’autres nœuds prennent le relais pour fournir les services requis (processus appelé «basculement»). En outre, les rôles en cluster sont surveillés de manière proactive pour vérifier qu’ils fonctionnent correctement. S’ils ne fonctionnent pas, ils sont redémarrés ou déplacés vers un autre nœud.

Les clusters de basculement offrent également la fonctionnalité de volume partagé de cluster (CSV) qui procure un espace de noms distribué et cohérent que les rôles en cluster peuvent utiliser pour accéder au stockage partagé à partir de tous les nœuds. La fonctionnalité de clustering de basculement garantit ainsi aux utilisateurs des interruptions de service minimales.

Le Clustering de basculement a de nombreuses applications pratiques, notamment:
* Stockage de partage de fichiers hautement disponible ou disponible en continu destiné à des applications telles que Microsoft SQLServer et à des ordinateurs virtuels Hyper\-V.
* Rôles en cluster hautement disponibles exécutés sur des serveurs physiques ou des machines virtuelles installées sur des serveurs exécutant Hyper-V

<hr />

<ul class="cardsF panelContent">
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-whats-new.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h2><a href="whats-new-in-failover-clustering.md">Nouveautés du clustering de basculement</a></h2>
                                        </div>
                                    </div>
                                </div>
                             </div>
                          </a>
                        </li>
                     </ul>
<HR />

<ul class="cardsF panelContent">

<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Comprendre</h3>
<HR />
                                        <p><a href="sofs-overview.md">Serveur de fichiers avec montée en charge pour les données d'application</a></p>
<HR />
                                        <p><a href="../storage/storage-spaces/understand-quorum.md">Quorum de cluster et de pool</a></p>
<HR />
                                        <p><a href="fault-domains.md">Reconnaissance des domaines d'erreur</a></p>
<HR />
                                        <p><a href="smb-multichannel.md">Réseaux en cluster à plusieurs cartes réseau et SMB Multichannel simplifiés</a></p>
<HR />
                                        <p><a href="vm-load-balancing-overview.md">Équilibrage de charge des machines virtuelles</a></p>
<HR />
                                        <p><a href="../storage/storage-spaces/cluster-sets.md">Jeux de clusters</a></p>
<HR />
                                        <p><a href="cluster-affinity.md">Affinité de cluster</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>

<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Planification</h3>
<HR />
                                        <p><a href="clustering-requirements.md">Configuration matérielle requise pour le clustering de basculement et options de stockage</a></p>
<HR />
                                        <p><a href="failover-cluster-csvs.md">Utiliser les volumes partagés de cluster</a></p>               
<HR />
                                        <p><a href="../storage/storage-spaces/storage-spaces-direct-in-vm.md">Utilisation de clusters d'ordinateurs virtuels invités avec espaces de stockage direct</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Déploiement</a></h3> 
<HR />
                                        <p><a href="prestage-cluster-adds.md">Prédéfinir des objets ordinateur pour le cluster dans Active Directory Domain Services</a></p>
<HR />
                                        <p><a href="create-failover-cluster.md">Création d'un cluster de basculement</a></p> 
<HR />
                                        <p><a href="deploy-two-node-clustered-file-server.md">Déployer un serveur de fichiers à deux nœuds</a></p> 
<HR />
                                        <p><a href="manage-cluster-quorum.md">Gérer le quorum et les témoins</a></p> 
<HR />
                                        <p><a href="deploy-cloud-witness.md">Déployer un témoin cloud</a></p>
<HR />
                                        <p><a href="file-share-witness.md">Déployer un témoin de partage de fichiers</a></p>
<HR />
                                        <p><a href="cluster-operating-system-rolling-upgrade.md">Mises à niveau propagées de système d’exploitation de cluster</a></p> 
<HR />
                                        <p><a href="upgrade-option-same-hardware.md">Mise à niveau d’un cluster de basculement sur le même matériel</a></p>
<HR />
                                        <p><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970\(v%3dws.11\)">Déployer un cluster détaché d’Active Directory</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
                     </ul>
<HR />
<ul class="cardsF panelContent">
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Gérer</h3>
<HR />
                                        <p><a href="cluster-aware-updating.md">Mise à jour adaptée aux clusters</a></p> 
<HR />
                                        <p><a href="health-service-overview.md">Service de contrôle d’intégrité</a></p>
<HR />
                                        <p><a href="cluster-domain-migration.md">Migration d'un cluster vers un domaine</a></p>
<HR />
                                        <p><a href="troubleshooting-using-wer-reports.md">Résolution des problèmes à l'aide du Rapport d'erreurs Windows</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Outils et paramètres</a></h3>
<HR />
                                        <p><a href="https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps">Applets de commande PowerShell pour clustering de basculement</a></p> 
<HR />
                                        <p><a href="https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps">Applets de commande PowerShell pour la mise à jour adaptée aux clusters</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Ressources de la communauté</a></h3>
<HR />
                                        <p><a href="https://go.microsoft.com/fwlink/p/?LinkId=230641">Forum sur la haute disponibilité (clustering)</a></p> 
<HR />
                                        <p><a href="http://blogs.msdn.com/b/clustering/">Blog de l’équipe sur le clustering de basculement et l’équilibrage de la charge réseau</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
</ul>
