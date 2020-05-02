---
title: Événements du journal du système de clustering de basculement
description: Liste des événements de clustering de basculement dans le journal système Windows Server. Ces événements peuvent être utilisés pour dépanner un cluster.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
manager: lizross
ms.technology: storage-failover-clustering
ms.date: 01/14/2020
ms.openlocfilehash: eea98579a66f1db7f7ec873bda6a2c934841736f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720508"
---
# <a name="failover-clustering-system-log-events"></a>Événements du journal du système de clustering de basculement

> S'applique à : Windows Server 2019, Windows Server 2016

Cette rubrique répertorie les événements de clustering de basculement du journal système Windows Server (affichable dans observateur d’événements). Ces événements partagent tous la source d’événements de **FailoverClustering** et peuvent être utiles lors du dépannage d’un cluster.

## <a name="critical-events"></a>Événements critiques

### <a name="event-1000-unexpected_fatal_error"></a>Événement 1000 : UNEXPECTED_FATAL_ERROR

Service de cluster a subi une erreur irrécupérable inattendue à la ligne %1 du module source %2. Le code d’erreur était %3.

### <a name="event-1001-assertion_failure"></a>Événement 1001 : ASSERTION_FAILURE

Service de cluster échec d’un contrôle de validité sur la ligne %1 du module source %2. « %3 »

### <a name="event-1006-nm_event_membership_halt"></a>Événement 1006 : NM_EVENT_MEMBERSHIP_HALT

Service de cluster a été interrompu en raison d’une connectivité incomplète avec d’autres nœuds de cluster.

### <a name="event-1057-dm_database_corrupt_or_missing"></a>Événement 1057 : DM_DATABASE_CORRUPT_OR_MISSING

Impossible de charger la base de données du cluster. Le fichier est peut-être manquant ou endommagé.
Une réparation automatique peut être tentée.

### <a name="event-1070-nm_event_begin_join_failed"></a>Événement 1070 : NM_EVENT_BEGIN_JOIN_FAILED

Le nœud n’a pas pu joindre le cluster de basculement « %1 » en raison du code d’erreur « %2 ».

### <a name="event-1073-cs_event_inconsistency_halt"></a>Événement 1073 : CS_EVENT_INCONSISTENCY_HALT

Le service de cluster a été interrompu pour empêcher une incohérence au sein du cluster de basculement. Le code d’erreur était « %1 ».

### <a name="event-1080-cs_diskwrite_failure"></a>Événement 1080 : CS_DISKWRITE_FAILURE

La service de cluster n’a pas pu mettre à jour la base de données du cluster (code d’erreur « %1 »).
Causes possibles : espace disque insuffisant ou corruption du système de fichiers.

### <a name="event-1090-cs_event_reg_operation_failed"></a>Événement 1090 : CS_EVENT_REG_OPERATION_FAILED

Impossible de démarrer le service de cluster. Une tentative de lecture des données de configuration à partir du Registre Windows a échoué avec l’erreur « %1 ». Utilisez le composant logiciel enfichable Gestion du cluster de basculement pour vous assurer que cet ordinateur est membre d’un cluster.
Si vous envisagez d’ajouter cet ordinateur à un cluster existant, utilisez l’Assistant Ajout d’un nœud. Sinon, si cet ordinateur a été configuré en tant que membre d’un cluster, il est nécessaire de restaurer les données de configuration manquantes qui sont nécessaires au service de cluster pour identifier qu’il est membre d’un cluster.
Effectuez une restauration de l’état du système de cet ordinateur afin de restaurer les données de configuration.

### <a name="event-1092-nm_event_mm_form_failed"></a>Événement 1092 : NM_EVENT_MM_FORM_FAILED

Échec de la forme du cluster' %1 'avec le code d’erreur' %2 '. Le cluster de basculement ne sera pas disponible.

### <a name="event-1093-nm_event_node_not_member"></a>Événement 1093 : NM_EVENT_NODE_NOT_MEMBER

Le service de cluster ne peut pas identifier le nœud « %1 » en tant que membre du cluster de basculement « %2 ». Si le nom d’ordinateur de ce nœud a été modifié récemment, vous pouvez revenir au nom précédent. Vous pouvez également ajouter le nœud au cluster de basculement et, si nécessaire, réinstaller des applications en cluster.

### <a name="event-1105-cs_event_rpc_init_failed"></a>Événement 1105 : CS_EVENT_RPC_INIT_FAILED

Le service de cluster n’a pas pu démarrer car il n’a pas pu enregistrer les interfaces avec le service RPC. Le code d’erreur était « %1 ».

### <a name="event-1135-event_node_down"></a>Événement 1135 : EVENT_NODE_DOWN

Le nœud de cluster' %1 'a été supprimé de l’appartenance au cluster de basculement actif. Le service de cluster sur ce nœud a peut-être été arrêté. Cela peut également être dû au fait que le nœud a perdu la communication avec d’autres nœuds actifs dans le cluster de basculement.
Exécutez l’Assistant validation d’une configuration pour vérifier la configuration de votre réseau. Si la condition persiste, recherchez les erreurs matérielles ou logicielles liées aux cartes réseau sur ce nœud. Vérifiez également les défaillances dans les autres composants réseau auxquels le nœud est connecté, comme des concentrateurs, des commutateurs ou des ponts.

### <a name="event-1146-rcm_event_resmon_died"></a>Événement 1146 : RCM_EVENT_RESMON_DIED

Le processus de Sous-système d’hébergement de ressources du cluster (RHS) a été arrêté et sera redémarré. Cela est généralement associé à la détection et à la récupération d’intégrité du cluster d’une ressource. Reportez-vous au journal des événements système pour déterminer la ressource et la DLL de ressource à l’origine du problème.

### <a name="event-1177-mm_event_arbitration_failed"></a>Événement 1177 : MM_EVENT_ARBITRATION_FAILED

Le service de cluster s’arrête car le quorum a été perdu. Cela peut être dû à la perte de la connectivité réseau entre certains ou tous les nœuds du cluster, ou à un basculement du disque témoin.

Exécutez l’Assistant validation d’une configuration pour vérifier la configuration de votre réseau. Si la condition persiste, recherchez les erreurs matérielles ou logicielles liées à la carte réseau. Vérifiez également les défaillances dans les autres composants réseau auxquels le nœud est connecté, comme des concentrateurs, des commutateurs ou des ponts.

### <a name="event-1181-netres_resource_start_error"></a>Événement 1181 : NETRES_RESOURCE_START_ERROR

La ressource de cluster' %1 'ne peut pas être mise en ligne, car le service associé n’a pas pu démarrer. Le code de retour du service est' %2 '. Vérifiez si des événements supplémentaires sont associés au service et assurez-vous que le service démarre correctement.

### <a name="event-1247-cluster_event_invalid_service_sid_type"></a>Événement 1247 : CLUSTER_EVENT_INVALID_SERVICE_SID_TYPE

Le type d’identificateur de sécurité (SID) pour le service de cluster est configuré en tant que « %1 », mais le type de SID attendu est « illimité ». Le service de cluster modifie automatiquement sa configuration de type SID avec le gestionnaire de contrôle des services (SCM) et redémarre afin que cette modification prenne effet.

### <a name="event-1248-cluster_event_service_sid_missing"></a>Événement 1248 : CLUSTER_EVENT_SERVICE_SID_MISSING

L’identificateur de sécurité (SID) ' %1 'associé au service de cluster n’est pas présent dans le jeton de processus. Le service de cluster corrigera automatiquement ce problème et redémarrera.

### <a name="event-1282-sm_event_handshake_timeout"></a>Événement 1282 : SM_EVENT_HANDSHAKE_TIMEOUT

La négociation de sécurité entre les points de terminaison locaux et distants' %2 'ne s’est pas terminée dans' %1 'secondes, le nœud termine la connexion

### <a name="event-1542-service_prerestore_failed"></a>Événement 1542 : SERVICE_PRERESTORE_FAILED

La demande de restauration des données de configuration du cluster a échoué. Cette restauration a échoué lors de la phase de pré-restauration, ce qui indique généralement que certains nœuds contenant le cluster ne sont pas opérationnels actuellement. Assurez-vous que le service de cluster s’exécute correctement sur tous les nœuds qui composent ce cluster.

### <a name="event-1543-service_postrestore_failed"></a>Événement 1543 : SERVICE_POSTRESTORE_FAILED

L’opération de restauration des données de configuration du cluster a échoué. Cette restauration a échoué au cours de la phase de « postconnexion », ce qui indique généralement que certains nœuds qui composent le cluster ne sont pas opérationnels actuellement. Il est recommandé de remplacer le fichier de données de configuration de cluster en cours (ClusDB) par « %1 ».

### <a name="event-1546-service_form_version_incompatible"></a>Événement 1546 : SERVICE_FORM_VERSION_INCOMPATIBLE

Le nœud « %1 » n’a pas pu former un cluster de basculement. Cela est dû au fait qu’un ou plusieurs nœuds exécutent des versions incompatibles du logiciel de service de cluster. Si le nœud « %1 » ou un autre nœud du cluster a été récemment mis à niveau, vérifiez de nouveau que tous les nœuds exécutent des versions compatibles du logiciel de service de cluster.

### <a name="event-1547-service_connect_version_incompatible"></a>Événement 1547 : SERVICE_CONNECT_VERSION_INCOMPATIBLE

Le nœud « %1 » a tenté de rejoindre un cluster de basculement, mais a échoué en raison d’une incompatibilité entre les versions du logiciel de service de cluster. Si le nœud « %1 » ou un autre nœud du cluster a été récemment mis à niveau, vérifiez que le déploiement du cluster modifié avec différentes versions du logiciel de service de cluster est pris en charge.

### <a name="event-1553-service_no_network_connectivity"></a>Événement 1553 : SERVICE_NO_NETWORK_CONNECTIVITY

Ce nœud de cluster n’a pas de connectivité réseau. Elle ne peut pas participer au cluster tant que la connectivité n’est pas restaurée. Exécutez l’Assistant validation d’une configuration pour vérifier la configuration de votre réseau. Si la condition persiste, recherchez les erreurs matérielles ou logicielles liées à la carte réseau. Vérifiez également les défaillances dans les autres composants réseau auxquels le nœud est connecté, comme des concentrateurs, des commutateurs ou des ponts.

### <a name="event-1554-service_network_connectivity_lost"></a>Événement 1554 : SERVICE_NETWORK_CONNECTIVITY_LOST

Ce nœud de cluster a perdu toute connectivité réseau. Elle ne peut pas participer au cluster tant que la connectivité n’est pas restaurée. Le service de cluster sur ce nœud va être arrêté.

### <a name="event-1556-service_unhandled_exception_in_worker_thread"></a>Événement 1556 : SERVICE_UNHANDLED_EXCEPTION_IN_WORKER_THREAD

Le service de cluster a rencontré un problème inattendu et va s’arrêter. Le code d’erreur était « %2 ».

### <a name="event-1561-service_nonstorage_witness_better_tag"></a>Événement 1561 : SERVICE_NONSTORAGE_WITNESS_BETTER_TAG

Service de cluster n’a pas pu démarrer car ce nœud a détecté qu’il ne dispose pas de la dernière copie des données de configuration du cluster. Les modifications apportées au cluster s’est produite alors que ce nœud n’était pas membre et, par conséquent, n’a pas pu recevoir les mises à jour des données de configuration.

#### <a name="guidance"></a>Assistance

Essayez de démarrer le service de cluster sur tous les nœuds du cluster afin que les nœuds avec la dernière copie des données de configuration du cluster puissent tout d’abord former le cluster. Ce nœud pourra alors rejoindre le cluster et obtiendra automatiquement les données de configuration de cluster mises à jour. Si aucun nœud n’est disponible avec la dernière copie des données de configuration du cluster, exécutez l’applet de commande Windows PowerShell « Start-ClusterNode-FQ ». L’utilisation du paramètre ForceQuorum (FQ) permet de démarrer le service de cluster et de marquer la copie de ce nœud des données de configuration du cluster comme faisant autorité. Le quorum forcé sur un nœud avec une copie obsolète de la base de données du cluster peut entraîner des modifications de la configuration du cluster qui se sont produites alors que le nœud n’était pas impliqué dans le cluster à perdre.

### <a name="event-1564-res_fsw_arbitratefailure"></a>Événement 1564 : RES_FSW_ARBITRATEFAILURE

La ressource témoin de partage de fichiers « %1 » n’a pas pu arbitrer pour le partage de fichiers « %2 ».
Vérifiez que le partage de fichiers « %2 » existe et qu’il est accessible par le cluster.

### <a name="event-1570-service_connect_authentication_failure"></a>Événement 1570 : SERVICE_CONNECT_AUTHENTICATION_FAILURE

Le nœud « %1 » n’a pas réussi à établir une session de communication lors de la jonction au cluster.
Cela est dû à un échec d’authentification. Vérifiez que les nœuds exécutent des versions compatibles du logiciel de service de cluster.

### <a name="event-1571-service_connect_authorizaion_failure"></a>Événement 1571 : SERVICE_CONNECT_AUTHORIZAION_FAILURE

Le nœud « %1 » n’a pas réussi à établir une session de communication lors de la jonction au cluster.
Cela est dû à un échec d’autorisation. Vérifiez que les nœuds exécutent des versions compatibles du logiciel de service de cluster.

### <a name="event-1572-service_netft_route_initial_failure"></a>Événement 1572 : SERVICE_NETFT_ROUTE_INITIAL_FAILURE

Le nœud « %1 » n’a pas réussi à joindre le cluster, car il n’a pas pu envoyer et recevoir des messages réseau de détection des échecs avec d’autres nœuds de cluster. Exécutez l’Assistant validation d’une configuration pour vérifier les paramètres réseau. Vérifiez également les règles des clusters de basculement du pare-feu Windows.

### <a name="event-1574-dm_database_unload_failed"></a>Événement 1574 : DM_DATABASE_UNLOAD_FAILED

Impossible de décharger la base de données de cluster de basculement. Si le redémarrage du service de cluster ne résout pas le problème, redémarrez l’ordinateur.

### <a name="event-1575-dm_database_corrupt_or_missing_fixquorum"></a>Événement 1575 : DM_DATABASE_CORRUPT_OR_MISSING_FIXQUORUM

Une tentative de démarrage forcé du service de cluster a échoué, car les données de configuration du cluster sur ce nœud sont manquantes ou endommagées. Commencez par démarrer le service de cluster sur un autre nœud disposant d’une copie intacte et valide des données de configuration du cluster. Ensuite, retentez l’opération de démarrage sur ce nœud (qui essaiera d’obtenir automatiquement les informations de configuration valides mises à jour). Si aucun autre nœud n’est disponible, utilisez WBAdmin. msc pour effectuer une restauration de l’état du système de ce nœud afin de restaurer les données de configuration.

### <a name="event-1593-dm_could_not_discard_changes"></a>Événement 1593 : DM_COULD_NOT_DISCARD_CHANGES

La base de données de cluster de basculement n’a pas pu être déchargée et aucune modification potentiellement incorrecte de la mémoire n’a pu être ignorée. Le service de cluster tente de réparer la base de données en la récupérant à partir d’un autre nœud de cluster. Si le service de cluster n’est pas en ligne, redémarrez le service de cluster sur ce nœud. Si le redémarrage du service de cluster ne résout pas le problème, redémarrez l’ordinateur. Si le service de cluster ne parvient pas à se mettre en ligne après un redémarrage, restaurez la base de données de cluster à partir de la dernière sauvegarde. La base de données actuelle a été copiée dans' %1 '. Si aucune sauvegarde n’est disponible, copiez « %1 » vers « %2 » et essayez de démarrer le service de cluster. Si le service de cluster est ensuite en ligne sur ce nœud, certaines modifications de configuration du cluster peuvent être perdues et le cluster peut ne pas fonctionner correctement. Exécutez l’Assistant validation d’une configuration pour vérifier la configuration de votre cluster et vérifier que les applications et les services hébergés sont en ligne et fonctionnent correctement.

### <a name="event-1672-event_node_quarantined"></a>Événement 1672 : EVENT_NODE_QUARANTINED

Le nœud de cluster' %1 'a été mis en quarantaine. Le nœud a rencontré « %2 » défaillances consécutives dans un laps de temps et a été supprimé du cluster pour éviter d’autres interruptions. Le nœud sera mis en quarantaine jusqu’à « %3 », puis le nœud essaiera automatiquement de rejoindre le cluster.

Reportez-vous aux journaux des événements système et d’application pour déterminer les problèmes sur ce nœud. Lorsque le problème est résolu, la mise en quarantaine peut être désactivée manuellement pour permettre au nœud de rejoindre l’applet de commande Windows PowerShell « Start-ClusterNode – ClearQuarantine ».

Nom du nœud : %1

Le nombre de membres du cluster consécutifs perd : %2

L’heure de mise en quarantaine sera automatiquement désactivée : %3

## <a name="error-events"></a>Événements d’erreur

### <a name="event-1024-cp_reg_ckpt_restore_failed"></a>Événement 1024 : CP_REG_CKPT_RESTORE_FAILED

Impossible de restaurer le point de contrôle du Registre pour la ressource de cluster « %1 »\\dans la clé de Registre HKEY_LOCAL_MACHINE %2. La ressource peut ne pas fonctionner correctement.
Assurez-vous qu’aucun autre processus n’a ouvert des descripteurs sur les clés de Registre dans cette sous-arborescence du Registre.

### <a name="event-1034-res_disk_missing"></a>Événement 1034 : RES_DISK_MISSING

Impossible de mettre en ligne la ressource de disque physique de cluster « %1 », car le disque associé est introuvable. La signature attendue du disque était « %2 ».
Si le disque a été remplacé ou restauré, dans le composant logiciel enfichable Gestionnaire du cluster de basculement, vous pouvez utiliser la fonction de réparation (dans la feuille de propriétés du disque) pour réparer le disque nouveau ou restauré. Si le disque ne sera pas remplacé, supprimez la ressource de disque associée.

### <a name="event-1035-res_disk_mount_failed"></a>Événement 1035 : RES_DISK_MOUNT_FAILED

La ressource de disque « %1 » a été mise en ligne, l’accès à un ou plusieurs volumes a échoué avec l’erreur « %2 ». Exécutez l’Assistant validation d’une configuration pour vérifier la configuration de votre stockage. Si vous le souhaitez, vous pouvez exécuter Chkdsk pour vérifier l’intégrité de tous les volumes de ce disque.

### <a name="event-1037-res_disk_filesystem_failed"></a>Événement 1037 : RES_DISK_FILESYSTEM_FAILED

Le système de fichiers pour une ou plusieurs partitions sur le disque pour la ressource « %1 » est peut-être endommagé. Exécutez l’Assistant validation d’une configuration pour vérifier la configuration de votre stockage. Si vous le souhaitez, vous pouvez exécuter Chkdsk pour vérifier l’intégrité de tous les volumes de ce disque.

### <a name="event-1038-res_disk_reservation_lost"></a>Événement 1038 : RES_DISK_RESERVATION_LOST

La propriété du disque de cluster « %1 » a été perdue de manière inattendue par ce nœud. Exécutez l’Assistant validation d’une configuration pour vérifier la configuration de votre stockage.

### <a name="event-1039-res_genapp_create_failed"></a>Événement 1039 : RES_GENAPP_CREATE_FAILED

L’application générique' %1 'n’a pas pu être mise en ligne (avec l’erreur' %2 ') lors d’une tentative de création du processus. Les causes possibles sont les suivantes : l’application n’est peut-être pas présente sur ce nœud, le nom du chemin d’accès a peut-être été spécifié de manière incorrecte, le nom binaire a peut-être été spécifié de manière incorrecte.

### <a name="event-1040-res_gensvc_open_failed"></a>Événement 1040 : RES_GENSVC_OPEN_FAILED

Le service générique « %1 » n’a pas pu être mis en ligne (avec l’erreur « %2 ») lors de la tentative d’ouverture du service. Les causes possibles sont les suivantes : le service n’est pas installé ou le nom de service spécifié n’est pas valide.

### <a name="event-1041-res_gensvc_start_failed"></a>Événement 1041 : RES_GENSVC_START_FAILED

Le service générique « %1 » n’a pas pu être mis en ligne (avec l’erreur « %2 ») lors d’une tentative de démarrage du service. Cause possible : les paramètres de service spécifiés ne sont peut-être pas valides.

### <a name="event-1042-res_gensvc_failed_after_start"></a>Événement 1042 : RES_GENSVC_FAILED_AFTER_START

Échec du service générique « %1 » avec l’erreur « %2 ». Examinez le journal des événements de l’application.

### <a name="event-1044-res_ipaddr_nbt_interface_create_failed"></a>Événement 1044 : RES_IPADDR_NBT_INTERFACE_CREATE_FAILED

Une erreur s’est produite lors de la tentative de création d’une interface NetBIOS pendant la mise en ligne de la ressource' %1 ' (code d’erreur : ' %2 '). Le nombre maximal de noms NetBIOS a peut-être été dépassé.

### <a name="event-1046-res_ipaddr_invalid_subnet"></a>Événement 1046 : RES_IPADDR_INVALID_SUBNET

La ressource d’adresse IP de cluster « %1 » ne peut pas être mise en ligne, car la valeur du masque de sous-réseau n’est pas valide. Vérifiez vos propriétés de ressource d’adresse IP.

### <a name="event-1047-res_ipaddr_invalid_address"></a>Événement 1047 : RES_IPADDR_INVALID_ADDRESS

La ressource d’adresse IP de cluster « %1 » ne peut pas être mise en ligne, car la valeur d’adresse n’est pas valide. Vérifiez vos propriétés de ressource d’adresse IP.

### <a name="event-1048-res_ipaddr_invalid_adapter"></a>Événement 1048 : RES_IPADDR_INVALID_ADAPTER

La ressource d’adresse IP de cluster « %1 » n’a pas pu être mise en ligne. Impossible de déterminer les données de configuration de la carte réseau correspondant à l’interface réseau de cluster « %2 » (code d’erreur : « %3 »). Vérifiez que la ressource d’adresse IP est configurée avec les propriétés d’adresse et de réseau appropriées.

### <a name="event-1049-res_ipaddr_in_use"></a>Événement 1049 : RES_IPADDR_IN_USE

La ressource d’adresse IP de cluster « %1 » ne peut pas être mise en ligne, car une adresse IP dupliquée « %2 » a été détectée sur le réseau. Vérifiez que toutes les adresses IP sont uniques.

### <a name="event-1050-res_netname_duplicate"></a>Événement 1050 : RES_NETNAME_DUPLICATE

La ressource de nom de réseau de cluster « %1 » ne peut pas être mise en ligne, car le nom « %2 » correspond au nom de ce nœud de cluster. Assurez-vous que les noms réseau sont uniques.

### <a name="event-1051-res_netname_no_ip_address"></a>Événement 1051 : RES_NETNAME_NO_IP_ADDRESS

La ressource de nom de réseau de cluster « %1 » ne peut pas être mise en ligne. Assurez-vous que les cartes réseau des ressources d’adresse IP dépendantes ont accès à au moins un serveur DNS. Vous pouvez également activer NetBIOS pour les adresses IP dépendantes.

### <a name="event-1052-res_netname_cant_add_name_status"></a>Événement 1052 : RES_NETNAME_CANT_ADD_NAME_STATUS

Impossible de mettre en ligne la ressource de nom de réseau de cluster « %1 », car le nom n’a pas pu être ajouté au système. Le code d’erreur associé est stocké dans la section de données.

### <a name="event-1053-res_smb_cant_create_share"></a>Événement 1053 : RES_SMB_CANT_CREATE_SHARE

Le partage de fichiers en cluster « %1 » ne peut pas être mis en ligne, car le partage n’a pas pu être créé.

### <a name="event-1054-res_smb_share_not_found"></a>Événement 1054 : RES_SMB_SHARE_NOT_FOUND

Le contrôle d’intégrité de la ressource de partage de fichiers « %1 » a échoué. La récupération des informations pour le partage « %2 » (dont l’étendue est le nom réseau %3) a retourné le code d’erreur « %4 ». Assurez-vous que le partage existe et qu’il est accessible.

### <a name="event-1055-res_smb_share_failed"></a>Événement 1055 : RES_SMB_SHARE_FAILED

Le contrôle d’intégrité de la ressource de partage de fichiers « %1 » a échoué. La récupération des informations pour le partage « %2 » (dont l’étendue est le nom réseau %3) indiquait que le partage n’existe pas (code d’erreur « %4 »). Vérifiez que le partage existe et qu’il est accessible.

### <a name="event-1069-rcm_resource_failure"></a>Événement 1069 : RCM_RESOURCE_FAILURE

Échec de la ressource de cluster' %1 'dans le rôle en cluster' %2 '.

### <a name="event-1069-rcm_resource_failure_with_typename"></a>Événement 1069 : RCM_RESOURCE_FAILURE_WITH_TYPENAME

Échec de la ressource de cluster' %1 'de type' %3 'dans le rôle en cluster' %2 '.<br><br>En fonction des stratégies d’échec pour la ressource et le rôle, le service de cluster peut essayer de mettre la ressource en ligne sur ce nœud ou de déplacer le groupe vers un autre nœud du cluster, puis de le redémarrer. Vérifiez l’état des ressources et des groupes à l’aide de Gestionnaire du cluster de basculement ou de l’applet de commande Windows PowerShell ClusterResource.

### <a name="event-1069-rcm_resource_failure_with_cause"></a>Événement 1069 : RCM_RESOURCE_FAILURE_WITH_CAUSE

Échec de la ressource de cluster' %1 'de type' %3 'dans le rôle en cluster' %2 '. Le code d’erreur était « %5 » (« %4 »).<br><br>En fonction des stratégies d’échec pour la ressource et le rôle, le service de cluster peut essayer de mettre la ressource en ligne sur ce nœud ou de déplacer le groupe vers un autre nœud du cluster, puis de le redémarrer. Vérifiez l’état des ressources et des groupes à l’aide de Gestionnaire du cluster de basculement ou de l’applet de commande Windows PowerShell ClusterResource.

### <a name="event-1069-rcm_resource_failure_with_error_code"></a>Événement 1069 : RCM_RESOURCE_FAILURE_WITH_ERROR_CODE

Échec de la ressource de cluster' %1 'de type' %3 'dans le rôle en cluster' %2 '. Le code d’erreur était « %4 ».<br><br>En fonction des stratégies d’échec pour la ressource et le rôle, le service de cluster peut essayer de mettre la ressource en ligne sur ce nœud ou de déplacer le groupe vers un autre nœud du cluster, puis de le redémarrer. Vérifiez l’état des ressources et des groupes à l’aide de Gestionnaire du cluster de basculement ou de l’applet de commande Windows PowerShell ClusterResource.

### <a name="event-1069-rcm_resource_failure_due_to_veto"></a>Événement 1069 : RCM_RESOURCE_FAILURE_DUE_TO_VETO

La ressource de cluster' %1 'de type' %3 'dans le rôle en cluster' %2 'a échoué en raison d’une tentative de blocage d’un changement d’État requis dans cette ressource de cluster.

### <a name="event-1077-res_ipaddr_ipv4_address_interface_failed"></a>Événement 1077 : RES_IPADDR_IPV4_ADDRESS_INTERFACE_FAILED

Échec du contrôle d’intégrité de l’interface IP « %1 » (adresse « %2 ») (État : « %3 »). Exécutez l’Assistant validation d’une configuration pour vérifier que la carte réseau fonctionne correctement.

### <a name="event-1078-res_ipaddr_wins_address_failed"></a>Événement 1078 : RES_IPADDR_WINS_ADDRESS_FAILED

Impossible de mettre en ligne la ressource d’adresse IP de cluster « %1 », car l’inscription WINS a échoué sur l’interface « %2 » avec l’erreur « %3 ». Assurez-vous qu’un serveur WINS valide et accessible a été spécifié.

### <a name="event-1121-cp_crypto_ckpt_restore_failed"></a>Événement 1121 : CP_CRYPTO_CKPT_RESTORE_FAILED

Les paramètres chiffrés pour la ressource de cluster « %1 » n’ont pas pu être appliqués correctement au nom de conteneur « %2 » sur ce nœud.

### <a name="event-1127-tm_event_cluster_netinterface_failed"></a>Événement 1127 : TM_EVENT_CLUSTER_NETINTERFACE_FAILED

Échec de l’interface réseau de cluster « %1 » pour le nœud de cluster « %2 » sur le réseau « %3 ». Exécutez l’Assistant validation d’une configuration pour vérifier la configuration de votre réseau. Si la condition persiste, recherchez les erreurs matérielles ou logicielles liées à la carte réseau. Vérifiez également les défaillances dans les autres composants réseau auxquels le nœud est connecté, comme des concentrateurs, des commutateurs ou des ponts.

### <a name="event-1129-tm_event_cluster_network_partitioned"></a>Événement 1129 : TM_EVENT_CLUSTER_NETWORK_PARTITIONED

Le réseau de cluster' %1 'est partitionné. Certains nœuds de cluster de basculement attachés ne peuvent pas communiquer entre eux via le réseau. Le cluster de basculement n’a pas pu déterminer l’emplacement de l’échec. Exécutez l’Assistant validation d’une configuration pour vérifier la configuration de votre réseau. Si la condition persiste, recherchez les erreurs matérielles ou logicielles liées à la carte réseau. Vérifiez également les défaillances dans les autres composants réseau auxquels le nœud est connecté, comme des concentrateurs, des commutateurs ou des ponts.

### <a name="event-1130-tm_event_cluster_network_down"></a>Événement 1130 : TM_EVENT_CLUSTER_NETWORK_DOWN

Le réseau de clusters « %1 » est inactif. Aucun des nœuds disponibles ne peut communiquer à l’aide de ce réseau. Exécutez l’Assistant validation d’une configuration pour vérifier la configuration de votre réseau. Si la condition persiste, recherchez les erreurs matérielles ou logicielles liées à la carte réseau. Vérifiez également les défaillances dans les autres composants réseau auxquels le nœud est connecté, comme des concentrateurs, des commutateurs ou des ponts.

### <a name="event-1137-rcm_drain_move_failed"></a>Événement 1137 : RCM_DRAIN_MOVE_FAILED

Impossible de terminer le déplacement du rôle de cluster « %1 » pour drain. L’opération a échoué avec le code d’erreur %2.

### <a name="event-1138-res_smb_cant_create_dfs_root"></a>Événement 1138 : RES_SMB_CANT_CREATE_DFS_ROOT

La ressource de partage de fichiers de cluster « %1 » ne peut pas être mise en ligne. Échec de la création de la racine de l’espace de noms DFS avec l’erreur « %3 ». Cela peut être dû à l’impossibilité de démarrer le service « espace de noms DFS » ou de créer la racine DFS-N pour le partage « %2 ».

### <a name="event-1141-res_smb_cant_init_dfs_svc"></a>Événement 1141 : RES_SMB_CANT_INIT_DFS_SVC

La ressource de partage de fichiers de cluster « %1 » ne peut pas être mise en ligne. La resynchronisation de la cible racine DFS « %2 » a échoué avec l’erreur « %3 ».

### <a name="event-1142-res_smb_cant_online_dfs_root"></a>Événement 1142 : RES_SMB_CANT_ONLINE_DFS_ROOT

Impossible de mettre en ligne la ressource de partage de fichiers de cluster « %1 » pour l’espace de noms DFS en raison de l’erreur « %2 ».

### <a name="event-1182-netres_resource_stop_error"></a>Événement 1182 : NETRES_RESOURCE_STOP_ERROR

La ressource de cluster' %1 'ne peut pas être mise en ligne, car le service associé n’a pas pu redémarrer. Le code d’erreur est « %2 ». Le service a requis un redémarrage afin de mettre à jour les paramètres. Toutefois, le service a échoué avant d’être arrêté et redémarré. Vérifiez si des événements supplémentaires sont associés au service et assurez-vous que le service fonctionne correctement.

### <a name="event-1183-res_disk_invalid_mp_source_not_clustered"></a>Événement 1183 : RES_DISK_INVALID_MP_SOURCE_NOT_CLUSTERED

La ressource de disque de cluster « %1 » contient un point de montage non valide. Les disques source et cible associés au point de montage doivent être des disques en cluster et doivent être membres du même groupe. <br>Le point de montage' %2 'pour le volume' %3 'fait référence à un disque source non valide. Vérifiez que le disque source est également un disque en cluster et dans le même groupe que le disque cible (hébergeant le point de montage).

### <a name="event-1191-res_netname_delete_computer_account_failed_status"></a>Événement 1191 : RES_NETNAME_DELETE_COMPUTER_ACCOUNT_FAILED_STATUS

La ressource de nom réseau de cluster « %1 » n’a pas pu supprimer son objet ordinateur associé dans le domaine « %2 ». Le code d’erreur était « %3 ». <br>Demandez à un administrateur de domaine de supprimer manuellement l’objet ordinateur du domaine Active Directory.

### <a name="event-1192-res_netname_delete_computer_account_failed"></a>Événement 1192 : RES_NETNAME_DELETE_COMPUTER_ACCOUNT_FAILED

La ressource de nom de réseau de cluster « %1 » n’a pas pu supprimer son objet ordinateur associé dans le domaine « %2 » pour la raison suivante :<br>%3. <br><br>Demandez à un administrateur de domaine de supprimer manuellement l’objet ordinateur du domaine Active Directory.

### <a name="event-1193-res_netname_add_computer_account_failed_status"></a>Événement 1193 : RES_NETNAME_ADD_COMPUTER_ACCOUNT_FAILED_STATUS

La ressource de nom de réseau de cluster « %1 » n’a pas pu créer l’objet ordinateur associé dans le domaine « %2 » pour la raison suivante : %3.<br><br>Le code d’erreur associé est : %5<br><br>
Collaborez avec votre administrateur de domaine pour vous assurer que :

- L’identité de cluster « %4 » peut créer des objets ordinateur. Par défaut, tous les objets ordinateur sont créés dans le conteneur « ordinateurs ». consultez l’administrateur de domaine si cet emplacement a été modifié.
- Le quota pour les objets ordinateur n’a pas été atteint.
- S’il existe un objet ordinateur existant, vérifiez que l’identité de cluster « %4 » possède l’autorisation « contrôle total » pour cet objet ordinateur à l’aide de l’outil Active Directory les utilisateurs et les ordinateurs.

### <a name="event-1194-res_netname_add_computer_account_failed"></a>Événement 1194 : RES_NETNAME_ADD_COMPUTER_ACCOUNT_FAILED

La ressource de nom de réseau de cluster « %1 » n’a pas pu créer l’objet ordinateur associé dans le domaine « %2 » pendant : %3.<br><br>Le texte du code d’erreur associé est : %4

Collaborez avec votre administrateur de domaine pour vous assurer que :

- L’identité de cluster « %5 » a des autorisations de création d’objets ordinateur. Par défaut, tous les objets ordinateur sont créés dans le même conteneur que l’identité de cluster « %5 ».
- Le quota pour les objets ordinateur n’a pas été atteint.
- S’il existe un objet ordinateur existant, vérifiez que l’identité de cluster « %5 » possède l’autorisation « contrôle total » pour cet objet ordinateur à l’aide de l’outil Active Directory des utilisateurs et des ordinateurs.

### <a name="event-1195-res_netname_dns_registration_failed_status"></a>Événement 1195 : RES_NETNAME_DNS_REGISTRATION_FAILED_STATUS

La ressource de nom de réseau de cluster « %1 » n’a pas pu inscrire un ou plusieurs noms DNS associés. Le code d’erreur était « %2 ». Assurez-vous que les cartes réseau associées aux ressources d’adresse IP dépendantes sont configurées avec un accès à au moins un serveur DNS.

### <a name="event-1196-res_netname_dns_registration_failed"></a>Événement 1196 : RES_NETNAME_DNS_REGISTRATION_FAILED

La ressource de nom de réseau de cluster « %1 » n’a pas pu inscrire un ou plusieurs noms DNS associés pour la raison suivante :<br>%2.<br><br>Assurez-vous que les cartes réseau associées aux ressources d’adresse IP dépendantes sont configurées avec au moins un serveur DNS accessible.

### <a name="event-1205-rcm_event_group_failed_online_offline"></a>Événement 1205 : RCM_EVENT_GROUP_FAILED_ONLINE_OFFLINE

La service de cluster n’a pas pu mettre le rôle en cluster' %1 'entièrement en ligne ou hors connexion. Une ou plusieurs ressources sont peut-être dans un état d’échec. Cela peut avoir un impact sur la disponibilité du rôle en cluster.

### <a name="event-1206-res_netname_update_computer_account_failed_status"></a>Événement 1206 : RES_NETNAME_UPDATE_COMPUTER_ACCOUNT_FAILED_STATUS

L’objet ordinateur associé à la ressource de nom de réseau de cluster « %1 » n’a pas pu être mis à jour dans le domaine « %2 ». Le code d’erreur était « %3 ». L’identité de cluster « %4 » peut ne pas avoir les autorisations nécessaires pour mettre à jour l’objet. Collaborez avec votre administrateur de domaine pour vous assurer que l’identité de cluster peut mettre à jour les objets ordinateur dans le domaine.

### <a name="event-1207-res_netname_update_computer_account_failed"></a>Événement 1207 : RES_NETNAME_UPDATE_COMPUTER_ACCOUNT_FAILED

L’objet ordinateur associé à la ressource de nom de réseau de cluster « %1 » n’a pas pu être mis à jour dans le domaine « %2 » pendant la <br>opération %3.<br><br>Le texte du code d’erreur associé est : %4<br> <br>L’identité de cluster « %5 » peut ne pas avoir les autorisations nécessaires pour mettre à jour l’objet. Collaborez avec votre administrateur de domaine pour vous assurer que l’identité de cluster peut mettre à jour les objets ordinateur dans le domaine.

### <a name="event-1208-res_disk_invalid_mp_target_not_clustered"></a>Événement 1208 : RES_DISK_INVALID_MP_TARGET_NOT_CLUSTERED

La ressource de disque de cluster « %1 » contient un point de montage non valide. Les disques source et cible associés au point de montage doivent être des disques en cluster et doivent être membres du même groupe. <br>Le point de montage' %2 'pour le volume' %3 'fait référence à un disque cible non valide. Vérifiez que le disque cible est également un disque en cluster et dans le même groupe que le disque source (hébergeant le point de montage).

### <a name="event-1211-res_netname_no_writeable_dc_status"></a>Événement 1211 : RES_NETNAME_NO_WRITEABLE_DC_STATUS

La ressource de nom de réseau de cluster « %1 » ne peut pas être mise en ligne. Échec de la tentative de localisation d’un contrôleur de domaine accessible en écriture (dans le domaine %2) pour créer ou mettre à jour un objet ordinateur associé à la ressource. Le code d’erreur était « %3 ».
Assurez-vous qu’un contrôleur de domaine accessible en écriture est accessible à ce nœud dans le domaine configuré. Assurez-vous également que le serveur DNS est en cours d’exécution afin de résoudre le nom du contrôleur de domaine.

### <a name="event-1212-res_netname_no_writeable_dc"></a>Événement 1212 : RES_NETNAME_NO_WRITEABLE_DC

La ressource de nom de réseau de cluster « %1 » ne peut pas être mise en ligne. La tentative de recherche d’un contrôleur de domaine accessible en écriture (dans le domaine %2) pour créer ou mettre à jour un objet ordinateur associé à la ressource a échoué pour la raison suivante :<br>%3.<br><br> Le code d’erreur était « %4 ». Assurez-vous qu’un contrôleur de domaine accessible en écriture est accessible à ce nœud dans le domaine configuré. Assurez-vous également que le serveur DNS est en cours d’exécution afin de résoudre le nom du contrôleur de domaine.

### <a name="event-1213-res_netname_rename_restore_failed"></a>Événement 1213 : RES_NETNAME_RENAME_RESTORE_FAILED

La ressource de nom de réseau de cluster « %1 » n’a pas pu renommer complètement l’objet ordinateur associé sur le contrôleur de domaine « %2 ». Échec de la tentative de renommage de l’objet ordinateur à partir du nouveau nom « %3 » vers son nom d’origine « %4 ». Le code d’erreur était' %5 '. Cela peut affecter la connectivité du client jusqu’à ce que le nom du réseau et son nom d’objet ordinateur associé soient cohérents. Contactez votre administrateur de domaine pour renommer manuellement l’objet ordinateur.

### <a name="event-1214-res_netname_cant_add_name2"></a>Événement 1214 : RES_NETNAME_CANT_ADD_NAME2

La ressource de nom réseau de cluster ne peut pas être inscrite avec NetBIOS. <br><br>Nom du réseau : « %3 » <br>Raison de l’échec : %2 <br>Nom de la ressource : « %1 » <br><br> Exécutez nbtstat pour le nom réseau pour vous assurer que le nom n’est pas déjà inscrit avec NetBIOS.

### <a name="event-1215-res_netname_not_registered_with_rdr"></a>Événement 1215 : RES_NETNAME_NOT_REGISTERED_WITH_RDR

Échec d’un contrôle d’intégrité de la ressource de nom réseau de cluster « %1 ». Le nom réseau' %2 'n’est plus inscrit sur ce nœud. Le code d’erreur était « %3 ». Recherchez les erreurs matérielles ou logicielles liées à la carte réseau. Vous pouvez également exécuter l’Assistant validation d’une configuration pour vérifier la configuration de votre réseau.

### <a name="event-1218-res_netname_online_rename_recovery_missing_account"></a>Événement 1218 : RES_NETNAME_ONLINE_RENAME_RECOVERY_MISSING_ACCOUNT

La ressource de nom réseau de cluster « %1 » n’a pas pu effectuer une opération de modification de nom (tentative de modification du nom d’origine « %3 » en nom « %4 »). L’objet ordinateur est introuvable sur le contrôleur de domaine' %2 ' (où il a été créé). Une tentative de recréation de l’objet ordinateur aura lieu lors de la prochaine mise en ligne de la ressource. En outre, contactez votre administrateur de domaine pour vous assurer que l’objet ordinateur existe dans le domaine.

### <a name="event-1219-res_netname_online_rename_dc_not_found"></a>Événement 1219 : RES_NETNAME_ONLINE_RENAME_DC_NOT_FOUND

La ressource de nom de réseau de cluster « %1 » n’a pas pu effectuer une opération de modification de nom.
Le contrôleur de domaine' %2 'pour lequel l’objet ordinateur' %3 'a été renommé n’a pas pu être contacté. Le code d’erreur était « %4 ». Assurez-vous qu’un contrôleur de domaine accessible en écriture est accessible et vérifiez tout problème de connectivité.

### <a name="event-1220-res_netname_online_rename_recovery_failed"></a>Événement 1220 : RES_NETNAME_ONLINE_RENAME_RECOVERY_FAILED

Le compte d’ordinateur pour la ressource « %1 » n’a pas pu être renommé de %2 en %3 à l’aide du contrôleur de domaine %4. Le code d’erreur associé est stocké dans la section de données.<br>
<br>Le compte d’ordinateur de cette ressource était en cours de renommage et ne s’est pas terminé. Cette opération a été détectée pendant le processus en ligne de cette ressource.
Pour pouvoir effectuer la récupération, le compte d’ordinateur doit être renommé en fonction de la valeur actuelle de la propriété Name, c.-à-d. le nom présenté sur le réseau.<br> <br>Le contrôleur de domaine sur lequel le changement de nom a été tenté n’est peut-être pas disponible. Si c’est le cas, attendez que le contrôleur de domaine soit à nouveau disponible. Le contrôleur de domaine peut refuser l’accès au compte. une fois l’accès résolu, réessayez de mettre le nom en ligne.<br> <br>Si ce n’est pas possible, désactivez et réactivez l’authentification Kerberos. une tentative de recherche du compte d’ordinateur sur un autre contrôleur de périphérique est effectuée. Vous devez modifier la propriété nom en %2 afin d’utiliser le compte d’ordinateur existant.

### <a name="event-1223-res_ipaddr_invalid_network_role"></a>Événement 1223 : RES_IPADDR_INVALID_NETWORK_ROLE

La ressource d’adresse IP de cluster « %1 » ne peut pas être mise en ligne, car le réseau de cluster « %2 » n’est pas configuré pour autoriser l’accès client. Utilisez l’composant logiciel enfichable Gestionnaire du cluster de basculement pour vérifier les propriétés configurées du réseau de clusters.

### <a name="event-1226-res_netname_tcb_not_held"></a>Événement 1226 : RES_NETNAME_TCB_NOT_HELD

La ressource de nom de réseau « %1 » (avec le nom réseau associé « %2 ») est activée pour la prise en charge de l’authentification Kerberos. Échec de l’ajout des informations d’identification requises au LSA-le code d’erreur associé « %3 » indique des privilèges insuffisants, normalement requis pour cette opération. Le privilège requis est « Trusted Computing base » et doit être activé localement sur chaque nœud contenant le cluster.

### <a name="event-1227-res_netname_lsa_error"></a>Événement 1227 : RES_NETNAME_LSA_ERROR

La ressource de nom de réseau « %1 » (avec le nom réseau associé « %2 ») est activée pour la prise en charge de l’authentification Kerberos. Échec de l’ajout des informations d’identification requises au LSA-le code d’erreur associé est « %3 ».

### <a name="event-1228-res_netname_clone_failure"></a>Événement 1228 : RES_NETNAME_CLONE_FAILURE

La ressource de nom réseau de cluster « %1 » a rencontré une erreur lors de l’activation du nom réseau sur ce nœud. La raison de l’échec est la suivante : <br> ' %2 '.<br><br> Le code d’erreur était « %3 ». <br><br> Vous pouvez mettre à nouveau la ressource de nom de réseau hors connexion et en ligne pour réessayer.

### <a name="event-1229-res_netname_no_ips_for_dns"></a>Événement 1229 : RES_NETNAME_NO_IPS_FOR_DNS

La ressource de nom de réseau de cluster « %1 » n’a pas pu identifier les adresses IP à inscrire auprès d’un serveur DNS. Assurez-vous qu’un ou plusieurs réseaux sont activés pour l’utilisation du cluster avec le paramètre « autoriser les clients à se connecter via ce réseau » et que chaque nœud possède une adresse IP valide configurée pour les réseaux.

### <a name="event-1230-rcm_deadlock_or_crash_detected"></a>Événement 1230 : RCM_DEADLOCK_OR_CRASH_DETECTED

Un composant sur le serveur n’a pas répondu dans le délai imparti. La ressource de cluster « %1 » (type de ressource « %2 », DLL « %3 ») dépasse son seuil de délai d’attente. Dans le cadre de la détection de l’intégrité du cluster, des actions de récupération sont effectuées.
Le cluster tentera d’effectuer une récupération automatique en terminant et en redémarrant le processus de Sous-système d’hébergement de ressources (RHS) qui exécute cette ressource. Vérifiez que l’infrastructure sous-jacente (par exemple, le stockage, la mise en réseau ou les services) qui sont associés à la ressource fonctionne correctement.

### <a name="event-1230-rcm_resource_control_deadlock_detected"></a>Événement 1230 : RCM_RESOURCE_CONTROL_DEADLOCK_DETECTED

Un composant sur le serveur n’a pas répondu dans le délai imparti. La ressource de cluster « %1 » (type de ressource « %2 », DLL « %3 ») a été dépassée pour dépasser le seuil de délai d’expiration lors du traitement du code de contrôle « %4 ; ». Dans le cadre de la détection de l’intégrité du cluster, des actions de récupération sont effectuées. Le cluster tentera d’effectuer une récupération automatique en terminant et en redémarrant le processus de Sous-système d’hébergement de ressources (RHS) qui exécute cette ressource. Vérifiez que l’infrastructure sous-jacente (par exemple, le stockage, la mise en réseau ou les services) qui sont associés à la ressource fonctionne correctement.

### <a name="event-1231-res_netname_logon_failure"></a>Événement 1231 : RES_NETNAME_LOGON_FAILURE

La ressource de nom réseau de cluster « %1 » a rencontré une erreur lors de l’ouverture de session sur le domaine. La raison de l’échec est la suivante : <br> ' %2 '.<br><br> Le code d’erreur était « %3 ».
<br><br> Assurez-vous qu’un contrôleur de domaine est accessible à ce nœud dans le domaine configuré.

### <a name="event-1232-res_genscript_timeout"></a>Événement 1232 : RES_GENSCRIPT_TIMEOUT

Le point d’entrée' %2 'de la ressource de script générique de cluster' %1 'n’a pas effectué l’exécution en temps opportun. Cela peut être dû à une boucle infinie ou à d’autres problèmes susceptibles de provoquer une attente infinie. La valeur de délai d’attente spécifiée peut également être trop petite pour cette ressource. Vérifiez le point d’entrée de script' %2 'pour vous assurer que toutes les attentes infinies dans le code de script ont été corrigées. Envisagez ensuite d’incrémenter la valeur du délai d’attente en attente si nécessaire.

### <a name="event-1233-res_genscript_hangmode"></a>Événement 1233 : RES_GENSCRIPT_HANGMODE

Ressource de script générique de cluster « %1 » : la demande d’exécution de l’opération « %2 » ne sera pas traitée. Cela est dû à une tentative précédente d’exécution du point d’entrée' %3 'en temps voulu. Vérifiez le code de script pour ce point d’entrée pour vous assurer qu’il n’existe pas de boucle infinie ou d’autres problèmes susceptibles de provoquer une attente infinie. Envisagez ensuite d’incrémenter la valeur du délai d’attente de la ressource si nécessaire.

### <a name="event-1234-cluster_event_account_missing_privs"></a>Événement 1234 : CLUSTER_EVENT_ACCOUNT_MISSING_PRIVS

Le service de cluster a détecté que son compte de service ne dispose pas d’un ou de plusieurs des privilèges requis. La liste des privilèges manquants est : « %1 » et n’est pas actuellement accordée au compte de service. Utilisez « SC. exe qprivs ClusSvc » pour vérifier les privilèges du service de cluster (ClusSvc). Vérifiez également les stratégies de sécurité ou de groupe dans Active Directory Domain Services qui peuvent avoir modifié les privilèges par défaut. Tapez la commande suivante pour accorder au service de cluster les privilèges nécessaires pour fonctionner correctement :

```
sc.exe privs
clussvc
SeBackupPrivilege/SeRestorePrivilege/SeIncreaseQuotaPrivilege/SeIncreaseBasePriorityPrivilege/SeTcbPrivilege/SeDebugPrivilege/SeSecurityPrivilege/SeAuditPrivilege/SeImpersonatePrivilege/SeChangeNotifyPrivilege/SeIncreaseWorkingSetPrivilege/SeManageVolumePrivilege/SeCreateSymbolicLinkPrivilege/SeLoadDriverPrivilege
```

### <a name="event-1242-res_ipaddr_lease_expired"></a>Événement 1242 : RES_IPADDR_LEASE_EXPIRED

Le bail de l’adresse IP « %2 » associé à la ressource d’adresse IP de cluster « %1 » a expiré ou est sur le lieu d’expirer et ne peut pas être renouvelé pour le moment. Assurez-vous que le serveur DHCP associé est accessible et correctement configuré pour renouveler le bail sur cette adresse IP.

### <a name="event-1245-res_ipaddr_lease_renewal_failed"></a>Événement 1245 : RES_IPADDR_LEASE_RENEWAL_FAILED

La ressource d’adresse IP de cluster « %1 » n’a pas pu renouveler le bail pour l’adresse IP « %2 ».
Assurez-vous que le serveur DHCP est accessible et correctement configuré pour renouveler le bail sur cette adresse IP.

### <a name="event-1250-rcm_resource_embedded_failure"></a>Événement 1250 : RCM_RESOURCE_EMBEDDED_FAILURE

La ressource de cluster' %1 'dans le rôle en cluster' %2 'a reçu une notification d’état critique. Pour une machine virtuelle, cela indique qu’une application ou un service à l’intérieur de l’ordinateur virtuel se trouve dans un État non sain. Vérifiez la fonctionnalité du service ou de l’application surveillés au sein de la machine virtuelle.

### <a name="event-1254-rcm_group_terminal_failure"></a>Événement 1254 : RCM_GROUP_TERMINAL_FAILURE

Le rôle en cluster « %1 » a dépassé son seuil de basculement. Il a épuisé le nombre de tentatives de basculement configurées dans la période de basculement allouée et reste dans un état d’échec. Aucune tentative supplémentaire n’est effectuée pour mettre le rôle en ligne ou le basculer vers un autre nœud du cluster.
Vérifiez les événements associés à l’échec. Une fois que les problèmes à l’origine de l’échec sont résolus, le rôle peut être mis en ligne manuellement ou le cluster peut tenter de le remettre en ligne après le délai de redémarrage.

### <a name="event-1255-rcm_resource_network_failure"></a>Événement 1255 : RCM_RESOURCE_NETWORK_FAILURE

La ressource de cluster' %1 'dans le rôle en cluster' %2 'a reçu une notification d’état critique. Pour une machine virtuelle, cela indique qu’un réseau critique de l’ordinateur virtuel se trouve dans un État non sain. Vérifiez la connectivité réseau de l’ordinateur virtuel et les réseaux virtuels que l’ordinateur virtuel est configuré pour utiliser.

### <a name="event-1256-res_netname_dns_registration_failed_dynamic_dns_zone"></a>Événement 1256 : RES_NETNAME_DNS_REGISTRATION_FAILED_DYNAMIC_DNS_ZONE

La ressource de nom réseau de cluster n’a pas pu inscrire un ou plusieurs noms DNS associés, car la zone DNS correspondante n’accepte pas les mises à jour dynamiques.<br><br>Nom du réseau de clusters : « %1 »<br>Zone DNS : « %2 »

#### <a name="guidance"></a>Assistance

Assurez-vous que le DNS est configuré en tant que zone DNS dynamique. Si le serveur DNS n’accepte pas les mises à jour dynamiques, décochez les cases « enregistrer les adresses de cette connexion dans DNS » dans les propriétés de la carte réseau.

### <a name="event-1257-res_netname_dns_registration_failed_secure_dns_zone"></a>Événement 1257 : RES_NETNAME_DNS_REGISTRATION_FAILED_SECURE_DNS_ZONE

La ressource de nom réseau de cluster n’a pas pu inscrire un ou plusieurs noms DNS associés, car l’accès à la mise à jour de la zone DNS sécurisée a été refusé.<br><br>Nom du réseau de clusters : « %1 »<br>Zone DNS : « %2 »<br><br>Assurez-vous que l’objet nom de cluster (CNO) reçoit des autorisations sur la zone DNS sécurisée.

### <a name="event-1258-res_netname_dns_registration_failed_timeout"></a>Événement 1258 : RES_NETNAME_DNS_REGISTRATION_FAILED_TIMEOUT

La ressource de nom réseau de cluster n’a pas pu inscrire un ou plusieurs noms DNS associés, car le serveur DNS n’a pas pu être atteint.<br><br>Nom du réseau de clusters : « %1 »<br>Zone DNS : « %2 »<br>Serveur DNS : « %3 »<br><br>Assurez-vous que les cartes réseau associées aux ressources d’adresse IP dépendantes sont configurées avec au moins un serveur DNS accessible.

### <a name="event-1259-res_netname_dns_registration_failed_cleanup"></a>Événement 1259 : RES_NETNAME_DNS_REGISTRATION_FAILED_CLEANUP

La ressource de nom réseau de cluster n’a pas pu inscrire un ou plusieurs noms DNS associés, car le service de cluster a échoué au nettoyage des enregistrements existants correspondant au nom de réseau.<br><br>Nom du réseau de clusters : « %1 »<br>Zone DNS : « %2 »<br><br>Assurez-vous que l’objet nom de cluster (CNO) reçoit des autorisations sur la zone DNS sécurisée.

### <a name="event-1260-res_netname_dns_registration_modify_failed"></a>Événement 1260 : RES_NETNAME_DNS_REGISTRATION_MODIFY_FAILED

La ressource de nom réseau de cluster n’a pas pu modifier l’inscription DNS.<br><br>Nom du réseau de clusters : « %1 »<br>Code d’erreur : ' %2 '

#### <a name="guidance"></a>Assistance

Assurez-vous que les cartes réseau associées aux ressources d’adresse IP dépendantes sont configurées avec un accès à au moins un serveur DNS.

### <a name="event-1261-res_netname_dns_registration_modify_failed_status"></a>Événement 1261 : RES_NETNAME_DNS_REGISTRATION_MODIFY_FAILED_STATUS

La ressource de nom réseau de cluster n’a pas pu modifier l’inscription DNS.<br><br>Nom du réseau de clusters : « %1 »<br>Raison : « %2 »

#### <a name="guidance"></a>Assistance

Assurez-vous que les cartes réseau associées aux ressources d’adresse IP dépendantes sont configurées avec un accès à au moins un serveur DNS.

### <a name="event-1262-res_netname_dns_registration_publish_ptr_failed"></a>Événement 1262 : RES_NETNAME_DNS_REGISTRATION_PUBLISH_PTR_FAILED

La ressource de nom réseau de cluster n’a pas pu publier l’enregistrement PTR dans la zone de recherche inversée DNS.<br><br>Nom du réseau de clusters : « %1 »<br>Code d’erreur : ' %2 '

#### <a name="guidance"></a>Assistance

Assurez-vous que les cartes réseau associées aux ressources d’adresse IP dépendantes sont configurées avec un accès à au moins un serveur DNS et que la zone de recherche inversée DNS existe.

### <a name="event-1264-res_netname_dns_registration_publish_ptr_failed_status"></a>Événement 1264 : RES_NETNAME_DNS_REGISTRATION_PUBLISH_PTR_FAILED_STATUS

La ressource de nom réseau de cluster n’a pas pu publier l’enregistrement PTR dans la zone de recherche inversée DNS.<br><br>Nom du réseau de clusters : « %1 »<br>Raison : « %2 »

#### <a name="guidance"></a>Assistance

Assurez-vous que les cartes réseau associées aux ressources d’adresse IP dépendantes sont configurées avec un accès à au moins un serveur DNS et que la zone de recherche inversée DNS existe.

### <a name="event-1265-res_type_control_timed_out"></a>Événement 1265 : RES_TYPE_CONTROL_TIMED_OUT

Le type de ressource de cluster « %1 » a dépassé le délai d’attente lors du traitement du code de contrôle %2. Le cluster tentera d’effectuer une récupération automatique en terminant et en redémarrant le processus de Sous-système d’hébergement de ressources (RHS) qui traitait l’appel.

### <a name="event-1289-netft_adapter_not_found"></a>Événement 1289 : NETFT_ADAPTER_NOT_FOUND

Le service de cluster n’a pas pu accéder à la carte réseau « %1 ». Vérifiez que les autres cartes réseau fonctionnent correctement et recherchez dans le gestionnaire de périphériques les erreurs associées à l’adaptateur' %1 '. Si la configuration de l’adaptateur' %1 'a été modifiée, il peut être nécessaire de réinstaller la fonctionnalité de clustering de basculement sur cet ordinateur.

### <a name="event-1360-res_ipaddr_invalid_network"></a>Événement 1360 : RES_IPADDR_INVALID_NETWORK

La ressource d’adresse IP de cluster « %1 » n’a pas pu être mise en ligne. Vérifiez que la propriété de réseau « %2 » correspond à un nom réseau de cluster ou que la propriété d’adresse « %3 » correspond à l’un des sous-réseaux d’un réseau de clusters. S’il s’agit d’un type d’adresse IPv6, vérifiez que le réseau de cluster correspondant à cette ressource a au moins un préfixe IPv6 qui n’est pas de type lien local ou tunnel.

### <a name="event-1361-res_ipaddr_missing_dependant"></a>Événement 1361 : RES_IPADDR_MISSING_DEPENDANT

La ressource d’adresse de tunnel IPv6 « %1 » n’a pas pu être mise en ligne, car elle ne dépend pas d’une ressource d’adresse IP (IPv4). La dépendance sur au moins une ressource d’adresse IP (IPv4) est requise.

### <a name="event-1362-res_ipaddr_missing_data"></a>Événement 1362 : RES_IPADDR_MISSING_DATA

La ressource d’adresse IP de cluster « %1 » n’a pas pu être mise en ligne, car la propriété « %2 » n’a pas pu être lue. Vérifiez que la ressource est correctement configurée.

### <a name="event-1363-res_ipaddr_no_isatap_support"></a>Événement 1363 : RES_IPADDR_NO_ISATAP_SUPPORT

La ressource d’adresse de tunnel IPv6 « %1 » n’a pas pu être mise en ligne. Le réseau de cluster « %2 » associé à la ressource d’adresse IP dépendante (IPv4) « %3 » ne prend pas en charge le tunneling ISATAP. Vérifiez que le réseau de clusters prend en charge le tunneling ISATAP.

### <a name="event-1540-service_backup_noquorum"></a>Événement 1540 : SERVICE_BACKUP_NOQUORUM

L’opération de sauvegarde des données de configuration du cluster a été abandonnée car le quorum du cluster n’a pas encore été atteint. Renouvelez cette opération de sauvegarde une fois que le cluster a atteint le quorum.

### <a name="event-1554-service_restore_invaliduser"></a>Événement 1554 : SERVICE_RESTORE_INVALIDUSER

L’opération de restauration des données de configuration du cluster a échoué. Cela était dû à des privilèges insuffisants associés au compte d’utilisateur effectuant la restauration. Vérifiez que le compte d’utilisateur dispose de privilèges d’administrateur local.

### <a name="event-1557-service_witness_attach_failed"></a>Événement 1557 : SERVICE_WITNESS_ATTACH_FAILED

Service de cluster n’a pas pu mettre à jour les données de configuration du cluster sur la ressource témoin. Vérifiez que la ressource témoin est en ligne et accessible.

### <a name="event-1559-res_witness_new_node_conflict"></a>Événement 1559 : RES_WITNESS_NEW_NODE_CONFLICT

Le partage de fichiers « %1 » associé à la ressource témoin de partage de fichiers est actuellement hébergé par le serveur « %2 ». Ce serveur « %2 » vient d’être ajouté en tant que nouveau nœud dans le cluster de basculement. L’hébergement du témoin de partage de fichiers par n’importe quel nœud comprenant le même cluster n’est pas recommandé. Choisissez un témoin de partage de fichiers qui n’est hébergé par aucun nœud dans le même cluster et modifiez les paramètres de la ressource témoin de partage de fichiers en conséquence.

### <a name="event-1560-res_smb_share_conflict"></a>Événement 1560 : RES_SMB_SHARE_CONFLICT

La ressource de partage de fichiers de cluster « %1 » a détecté des conflits de dossiers partagés. Par conséquent, certains de ces dossiers partagés peuvent ne pas être accessibles. Pour résoudre ce problème, assurez-vous que plusieurs dossiers partagés n’ont pas le même nom de partage.

### <a name="event-1563-res_fsw_onlinefailure"></a>Événement 1563 : RES_FSW_ONLINEFAILURE

La ressource témoin de partage de fichiers « %1 » n’a pas pu être mise en ligne. Vérifiez que le partage de fichiers « %2 » existe et qu’il est accessible par le cluster.

### <a name="event-1566-res_netname_timedout"></a>Événement 1566 : RES_NETNAME_TIMEDOUT

La ressource de nom de réseau de cluster « %1 » ne peut pas être mise en ligne. La ressource de nom de réseau a été arrêtée par le sous-système de l’hôte de ressources, car elle n’a pas effectué une opération dans un délai acceptable. L’opération a expiré lors de l’exécution de :<br> ' %2 '

### <a name="event-1567-service_failed_to_change_log_size"></a>Événement 1567 : SERVICE_FAILED_TO_CHANGE_LOG_SIZE

Service de cluster n’a pas pu modifier la taille du journal des traces. Vérifiez le paramètre ClusterLogSize à l’aide de l’applet de \| commande PowerShell « \*obtient-cluster format-list ». Utilisez également le composant logiciel enfichable analyseur de performances pour vérifier les paramètres de session de suivi d’événements pour FailoverClustering.

### <a name="event-1567-res_vipaddr_address_interface_failed"></a>Événement 1567 : RES_VIPADDR_ADDRESS_INTERFACE_FAILED

Échec du contrôle d’intégrité de l’interface IP « %1 » (adresse « %2 ») (État : « %3 »). Recherchez les erreurs matérielles ou logicielles liées aux cartes réseau physiques ou virtuelles.

### <a name="event-1568-res_cloud_witness_cant_communicate_to_azure"></a>Événement 1568 : RES_CLOUD_WITNESS_CANT_COMMUNICATE_TO_AZURE

La ressource témoin Cloud n’a pas pu atteindre Microsoft Azure services de stockage.<br><br>Ressource de cluster : %1 <br>Nœud de cluster : %2 

#### <a name="guidance"></a>Assistance

Cela peut être dû à une communication réseau entre le nœud de cluster et le service de Microsoft Azure bloqué. Vérifiez la connectivité Internet du nœud pour Microsoft Azure. Connectez-vous au Portail Microsoft Azure et vérifiez que le compte de stockage existe.

### <a name="event-1569-service_using_restricted_network"></a>Événement 1569 : SERVICE_USING_RESTRICTED_NETWORK

Le réseau « %1 » qui a été désactivé pour l’utilisation du cluster de basculement est le seul réseau actuellement possible que le nœud « %2 » peut utiliser pour communiquer avec d’autres nœuds du cluster. Cela peut avoir un impact sur la capacité du nœud à participer au cluster. Vérifiez la connectivité réseau du nœud « %2 » et activez au moins un réseau pour la communication avec le cluster. Exécutez l’Assistant validation d’une configuration pour vérifier la configuration de votre réseau.

### <a name="event-1569-res_cloud_witness_token_expired"></a>Événement 1569 : RES_CLOUD_WITNESS_TOKEN_EXPIRED

La ressource de témoin Cloud n’a pas pu s’authentifier auprès des services de stockage Microsoft Azure. Une erreur de refus d’accès a été renvoyée lors de la tentative de contact du compte de stockage Microsoft Azure. <br><br>Ressource de cluster : %1 

#### <a name="guidance"></a>Assistance

La clé d’accès du compte de stockage n’est peut-être plus valide. Utilisez l’Assistant Configuration de quorum du cluster dans le Gestionnaire du cluster de basculement ou l’applet de commande Windows PowerShell Set-ClusterQuorum pour configurer la ressource de témoin Cloud avec la clé d’accès de compte de stockage mise à jour.

### <a name="event-1573-service_form_witness_failed"></a>Événement 1573 : SERVICE_FORM_WITNESS_FAILED

Le nœud « %1 » n’a pas pu former un cluster. Cela était dû au fait que le témoin n’était pas accessible. Vérifiez que la ressource témoin est en ligne et disponible.

### <a name="event-1580-res_netname_dns_registration_secure_zone_failed"></a>Événement 1580 : RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_FAILED

La ressource de nom de réseau de cluster « %1 » n’a pas pu inscrire le nom « %2 » sur l’adaptateur « %4 » dans une zone DNS sécurisée. Cela était dû à un enregistrement existant portant ce nom et l’identité du cluster ne dispose pas des privilèges suffisants pour mettre à jour cet enregistrement. Le code d’erreur était « %3 ». Contactez l’administrateur de votre serveur DNS pour vérifier que l’identité du cluster dispose des autorisations sur l’enregistrement DNS' %2 '.

### <a name="event-1580-res_netname_dns_registration_secure_zone_failed"></a>Événement 1580 : RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_FAILED

La ressource de nom de réseau de cluster « %1 » n’a pas pu inscrire le nom « %2 » sur l’adaptateur « %4 » dans une zone DNS sécurisée. Cela était dû à un enregistrement existant portant ce nom et l’identité du cluster ne dispose pas des privilèges suffisants pour mettre à jour cet enregistrement. Le code d’erreur était « %3 ». Contactez l’administrateur de votre serveur DNS pour vérifier que l’identité du cluster dispose des autorisations sur l’enregistrement DNS' %2 '.

### <a name="event-1585-res_fileserver_fscheck_srvsvc_stopped"></a>Événement 1585 : RES_FILESERVER_FSCHECK_SRVSVC_STOPPED

La ressource du serveur de fichiers en cluster « %1 » n’a pas pu vérifier l’intégrité. Cela est dû au fait que le service serveur n’a pas été démarré. Utilisez Gestionnaire de serveur pour confirmer l’état du service serveur sur ce nœud de cluster.

### <a name="event-1586-res_fileserver_fscheck_scoped_name_not_registered"></a>Événement 1586 : RES_FILESERVER_FSCHECK_SCOPED_NAME_NOT_REGISTERED

La ressource du serveur de fichiers en cluster « %1 » n’a pas pu vérifier l’intégrité. Cela était dû au fait que certains de ses dossiers partagés étaient inaccessibles. Vérifiez que les dossiers sont accessibles à partir des clients. En outre, confirmez l’état du service serveur sur ce nœud de cluster à l’aide de Gestionnaire de serveur et recherchez d’autres événements liés au service serveur sur ce nœud de cluster. Il peut être nécessaire de redémarrer la ressource de nom de réseau « %2 » dans ce rôle en cluster.

### <a name="event-1587-res_fileserver_fscheck_failed"></a>Événement 1587 : RES_FILESERVER_FSCHECK_FAILED

La ressource du serveur de fichiers en cluster « %1 » n’a pas pu vérifier l’intégrité. Cela était dû au fait que certains de ses dossiers partagés étaient inaccessibles. Vérifiez que les dossiers sont accessibles à partir des clients. En outre, confirmez l’état du service serveur sur ce nœud de cluster à l’aide de Gestionnaire de serveur et recherchez d’autres événements liés au service serveur sur ce nœud de cluster.

### <a name="event-1588-res_fileserver_share_cant_add"></a>Événement 1588 : RES_FILESERVER_SHARE_CANT_ADD

Impossible de mettre en ligne la ressource de serveur de fichiers en cluster « %1 ». La ressource n’a pas pu créer le partage de fichiers « %2 » associé au nom réseau « %3 ». Le code d’erreur était « %4 ». Vérifiez que les dossiers existent et sont accessibles. En outre, confirmez l’état du service serveur sur ce nœud de cluster à l’aide de Gestionnaire de serveur et recherchez d’autres événements connexes sur ce nœud de cluster. Il peut être nécessaire de redémarrer la ressource de nom de réseau « %3 » dans ce rôle en cluster.

### <a name="event-1600-clusapi_create_cannot_set_ad_dacl"></a>Événement 1600 : CLUSAPI_CREATE_CANNOT_SET_AD_DACL

Service de cluster n’a pas pu définir les autorisations sur l’objet d’ordinateur de cluster' %1 '. Contactez votre administrateur réseau pour vérifier le descripteur de sécurité du cluster de l’objet ordinateur dans Active Directory, vérifiez que la liste DACL n’est pas trop volumineuse et supprimez les ACE supplémentaires inutiles sur l’objet, si nécessaire.

### <a name="event-1603-res_fileserver_clone_failed"></a>Événement 1603 : RES_FILESERVER_CLONE_FAILED

Le serveur de fichiers n’a pas pu démarrer car la dépendance attendue sur la ressource’nom réseau’est introuvable ou n’a pas été configurée correctement. Erreur = 0x %2.

### <a name="event-1606-res_disk_cno_check_failed"></a>Événement 1606 : RES_DISK_CNO_CHECK_FAILED

La ressource de disque de cluster « %1 » contient un volume protégé par BitLocker, « %2 », mais pour ce volume, le Active Directory compte de nom de cluster (également appelé objet nom de cluster ou CNO) n’est pas un protecteur BitLocker pour le volume. Cela est nécessaire pour les volumes protégés par BitLocker. Pour corriger cela, supprimez d’abord le disque du cluster. Ensuite, utilisez l’outil en ligne de commande Manage-bde. exe pour ajouter le nom du cluster en tant que protecteur ADAccountOrGroup\\,\$ en utilisant le format de domaine ClusterName pour le nom du cluster. Rajoutez ensuite le disque au cluster. Pour plus d’informations, consultez la documentation de Manage-bde. exe.

### <a name="event-1607-res_disk_cno_unlock_failed"></a>Événement 1607 : RES_DISK_CNO_UNLOCK_FAILED

La ressource de disque de cluster « %1 » n’a pas pu déverrouiller le volume protégé par BitLocker « %2 ». L’objet nom de cluster (CNO) n’est pas configuré pour être un protecteur BitLocker valide pour ce volume. Pour corriger cela, supprimez le disque du cluster. Utilisez ensuite l’outil en ligne de commande Manage-bde. exe pour ajouter le nom du cluster en tant que protecteur ADAccountOrGroup\\,\$en utilisant le format de domaine ClusterName, puis rajoutez le disque au cluster. Pour plus d’informations, consultez la documentation de Manage-bde. exe.

### <a name="event-1608-res_fileserver_leader_failed"></a>Événement 1608 : RES_FILESERVER_LEADER_FAILED

Le serveur de fichiers n’a pas pu démarrer car la dépendance attendue sur la ressource’nom réseau’est introuvable ou n’a pas été configurée correctement. Erreur = 0x %2.

### <a name="event-1609-res_soda_fileserver_leader_failed"></a>Événement 1609 : RES_SODA_FILESERVER_LEADER_FAILED

Scale Out serveur de fichiers n’a pas pu démarrer, car la dépendance attendue sur la ressource « nom du réseau distribué » est introuvable ou n’a pas été configurée correctement. Erreur = 0x %2.

### <a name="event-1632-clusapi_create_mismatched_ou"></a>Événement 1632 : CLUSAPI_CREATE_MISMATCHED_OU

Échec de la création du cluster. Impossible de créer l’objet de nom de cluster « %1 » dans l’unité d’organisation Active Directory « %2 ». L’objet existe déjà dans l’unité d’organisation « %3 ». Vérifiez que le chemin d’accès du nom unique spécifié et l’objet nom de cluster sont corrects. Si le chemin d’accès du nom unique n’est pas spécifié, l’objet ordinateur existant « %1 » sera utilisé.

### <a name="event-1652-service_tcp_connection_failure"></a>Événement 1652 : SERVICE_TCP_CONNECTION_FAILURE

Le nœud de cluster' %1 'n’a pas pu joindre le cluster. Une connexion TCP n’a pas pu être établie sur le (s) nœud (s) « %2 ». Vérifiez la connectivité réseau et la configuration de tous les pare-feu réseau.

### <a name="event-1652-service_udp_connection_failure"></a>Événement 1652 : SERVICE_UDP_CONNECTION_FAILURE

Le nœud de cluster' %1 'n’a pas pu joindre le cluster. Impossible d’établir une connexion UDP au (x) nœud (s) ' %2 '. Vérifiez la connectivité réseau et la configuration de tous les pare-feu réseau.

### <a name="event-1652-service_virtual_tcp_connection_failure"></a>Événement 1652 : SERVICE_VIRTUAL_TCP_CONNECTION_FAILURE

Le nœud de cluster' %1 'n’a pas pu joindre le cluster. Une connexion TCP utilisant la carte virtuelle de cluster de basculement Microsoft n’a pas pu être établie sur le (s) nœud (s) « %2 ». Vérifiez la connectivité réseau et la configuration de tous les pare-feu réseau.

### <a name="event-1653-service_no_connectivity"></a>Événement 1653 : SERVICE_NO_CONNECTIVITY

Le nœud de cluster' %1 'n’a pas pu rejoindre le cluster, car il n’a pas pu communiquer sur le réseau avec un autre nœud du cluster. Vérifiez la connectivité réseau et la configuration de tous les pare-feu réseau.

### <a name="event-1654-res_vipaddr_invalid_adaptername"></a>Événement 1654 : RES_VIPADDR_INVALID_ADAPTERNAME

La ressource d’adresse IP disjointe de cluster « %1 » n’a pas pu être mise en ligne. Impossible de déterminer les données de configuration de la carte réseau correspondant à la carte réseau « %2 » (code d’erreur : « %3 »). Vérifiez que la ressource d’adresse IP est configurée avec les propriétés d’adresse et de réseau appropriées.

### <a name="event-1655-res_vipaddr_invalid_vsid"></a>Événement 1655 : RES_VIPADDR_INVALID_VSID

La ressource d’adresse IP disjointe de cluster « %1 » n’a pas pu être mise en ligne. Impossible de déterminer les données de configuration de la carte réseau correspondant à l’ID de sous-réseau virtuel « %2 » et à l’ID de domaine de routage « %3 » (code d’erreur : « %4 »). Vérifiez que la ressource d’adresse IP est configurée avec les propriétés d’adresse et de réseau appropriées.

### <a name="event-1656-res_vipaddr_address_create_failed"></a>Événement 1656 : RES_VIPADDR_ADDRESS_CREATE_FAILED

Échec de l’ajout de l’adresse IP « %2 » pour la ressource d’adresse IP disjointe « %1 » (code d’erreur : « %3 »). Recherchez les erreurs matérielles ou logicielles liées aux cartes réseau physiques ou virtuelles.

### <a name="event-1664-cluster_upgrade_incomplete"></a>Événement 1664 : CLUSTER_UPGRADE_INCOMPLETE

Échec de la mise à niveau du niveau fonctionnel du cluster. Vérifiez que tous les nœuds du cluster sont en cours d’exécution et qu’ils sont de la même version de Windows Server, puis exécutez à nouveau l’applet de commande Windows PowerShell Update-ClusterFunctionalLevel.

### <a name="event-1676-event_local_node_quarantined"></a>Événement 1676 : EVENT_LOCAL_NODE_QUARANTINED

Le nœud de cluster local a été mis en quarantaine par « %1 ». Le nœud sera mis en quarantaine jusqu’à « %2 », puis le nœud essaiera automatiquement de rejoindre le cluster.
<br><br>Reportez-vous aux journaux des événements système et d’application pour déterminer les problèmes sur ce nœud. Lorsque le problème est résolu, la mise en quarantaine peut être désactivée manuellement pour permettre au nœud de rejoindre l’applet de commande Windows PowerShell « Start-ClusterNode – ClearQuarantine ».<br><br>QuarantineType : mis en quarantaine par %1<br>L’heure de mise en quarantaine sera automatiquement désactivée : %2

### <a name="event-1677-event_node_drain_failed"></a>Événement 1677 : EVENT_NODE_DRAIN_FAILED

Échec de l’écoulement du nœud sur le nœud de cluster %1. <br><br>Référencez les journaux des événements système et d’application du nœud et les journaux du cluster pour rechercher la cause de l’échec de l’écoulement. Une fois le problème résolu, vous pouvez retenter l’opération de drainage.

### <a name="event-1683-res_netname_computer_account_no_dc"></a>Événement 1683 : RES_NETNAME_COMPUTER_ACCOUNT_NO_DC

Le service de cluster n’a pas pu atteindre un contrôleur de domaine disponible sur le domaine. Cela peut avoir un impact sur les fonctionnalités qui dépendent de l’authentification du nom du réseau de clusters.<br><br>Serveur de contrôleur de périphérique : %1 

#### <a name="guidance"></a>Assistance

Vérifiez que les contrôleurs de domaine sont accessibles sur le réseau aux nœuds du cluster.

### <a name="event-1684-res_netname_computer_object_vco_not_found"></a>Événement 1684 : RES_NETNAME_COMPUTER_OBJECT_VCO_NOT_FOUND

La ressource de nom réseau de cluster n’a pas pu trouver l’objet ordinateur associé dans Active Directory. Cela peut avoir un impact sur les fonctionnalités qui dépendent de l’authentification du nom du réseau de clusters.<br><br>Nom réseau : %1<br>Unité d’organisation : %2

#### <a name="guidance"></a>Assistance

Restaurez l’objet ordinateur pour le nom réseau à partir de la corbeille de Active Directory. Vous pouvez également utiliser la ressource de nom réseau de cluster en mode hors connexion et exécuter l’action de réparation pour recréer l’objet ordinateur dans Active Directory.

### <a name="event-1685-res_netname_computer_object_cno_not_found"></a>Événement 1685 : RES_NETNAME_COMPUTER_OBJECT_CNO_NOT_FOUND

La ressource de nom réseau de cluster n’a pas pu trouver l’objet ordinateur associé dans Active Directory. Cela peut avoir un impact sur les fonctionnalités qui dépendent de l’authentification du nom du réseau de clusters.<br><br>Nom réseau : %1<br>Unité d’organisation : %2
#### <a name="guidance"></a>Assistance

Restaurez l’objet ordinateur pour le nom réseau à partir de la corbeille de Active Directory.

### <a name="event-1686-res_netname_computer_object_vco_disabled"></a>Événement 1686 : RES_NETNAME_COMPUTER_OBJECT_VCO_DISABLED

La ressource de nom réseau de cluster a trouvé l’objet ordinateur associé dans Active Directory à désactiver. Cela peut avoir un impact sur les fonctionnalités qui dépendent de l’authentification du nom du réseau de clusters.<br><br>Nom réseau : %1<br>Unité d’organisation : %2

#### <a name="guidance"></a>Assistance

Activez l’objet ordinateur pour le nom réseau dans Active Directory.

### <a name="event-1687-res_netname_computer_object_cno_disabled"></a>Événement 1687 : RES_NETNAME_COMPUTER_OBJECT_CNO_DISABLED

La ressource de nom réseau de cluster a trouvé l’objet ordinateur associé dans Active Directory à désactiver. Cela peut avoir un impact sur les fonctionnalités qui dépendent de l’authentification du nom du réseau de clusters.<br><br>Nom réseau : %1<br>Unité d’organisation : %2

#### <a name="guidance"></a>Assistance

Activez l’objet ordinateur pour le nom réseau dans Active Directory. Vous pouvez également mettre hors connexion la ressource de nom de réseau du cluster et exécuter l’action de réparation pour activer l’objet ordinateur dans Active Directory.

### <a name="event-1688-res_netname_computer_object_failed"></a>Événement 1688 : RES_NETNAME_COMPUTER_OBJECT_FAILED

La ressource de nom réseau de cluster a détecté que l’objet ordinateur associé dans Active Directory a été désactivé et sa tentative d’activation a échoué. Cela peut avoir un impact sur les fonctionnalités qui dépendent de l’authentification du nom du réseau de clusters.<br><br>Nom réseau : %1<br>Unité d’organisation : %2

#### <a name="guidance"></a>Assistance

Activez l’objet ordinateur pour le nom réseau dans Active Directory.

### <a name="event-4608-nodecleanup_get_clustered_disks_failed"></a>Événement 4608 : NODECLEANUP_GET_CLUSTERED_DISKS_FAILED

Service de cluster n’a pas pu récupérer la liste des disques en cluster lors de la destruction du cluster. Le code d’erreur était « %1 ». Si ces disques ne sont pas accessibles, exécutez l’applet de commande PowerShell « Clear-ClusterDiskReservation ».

### <a name="event-4611-nodecleanup_release_clustered_disks_from_partmgr_failed"></a>Événement 4611 : NODECLEANUP_RELEASE_CLUSTERED_DISKS_FROM_PARTMGR_FAILED

Le disque en cluster avec l’ID « %2 » n’a pas été libéré par le gestionnaire de partition lors de la destruction du cluster. Le code d’erreur était « %1 ». Le redémarrage de l’ordinateur permet de s’assurer que le disque est libéré par le gestionnaire de partition.

### <a name="event-4613-nodecleanup_clear_clusdisk_database_failed"></a>Événement 4613 : NODECLEANUP_CLEAR_CLUSDISK_DATABASE_FAILED

Le service de cluster n’a pas réussi à nettoyer correctement un disque en cluster avec l’ID « %2 » lors de la destruction du cluster. Le code d’erreur était « %1 ». Vous ne pouvez pas accéder à ce disque tant que le nettoyage n’a pas abouti. Pour le nettoyage manuel, supprimez la valeur « AttachedDisks » de la\\clé\\«\\HKEY_LOCAL_MACHINE\\System\\CurrentControlSet Services Clusdisk Parameters » dans le Registre Windows.

### <a name="event-4615-nodecleanup_disable_cluster_service_failed"></a>Événement 4615 : NODECLEANUP_DISABLE_CLUSTER_SERVICE_FAILED

Le service de cluster a été arrêté et défini comme étant désactivé dans le cadre du nettoyage du nœud de cluster.

### <a name="event-4616-nodecleanup_disable_cluster_service_timeout"></a>Événement 4616 : NODECLEANUP_DISABLE_CLUSTER_SERVICE_TIMEOUT

L’arrêt du service de cluster pendant le nettoyage du nœud de cluster n’est pas terminé dans le délai imparti. Redémarrez cet ordinateur pour vous assurer que le service de cluster n’est plus en cours d’exécution.

### <a name="event-4618-nodecleanup_reset_cluster_registry_entries_failed"></a>Événement 4618 : NODECLEANUP_RESET_CLUSTER_REGISTRY_ENTRIES_FAILED

Échec de la réinitialisation des entrées de Registre du service de cluster lors du nettoyage du nœud de cluster.
Le code d’erreur était « %1 ». Il se peut que vous ne puissiez pas créer ou joindre un cluster à cet ordinateur tant que le nettoyage n’a pas abouti. Pour un nettoyage manuel, exécutez l’applet de commande PowerShell’Clear-ClusterNode’sur cet ordinateur.

### <a name="event-4620-nodecleanup_unload_cluster_hive_failed"></a>Événement 4620 : NODECLEANUP_UNLOAD_CLUSTER_HIVE_FAILED

Échec du déchargement de la ruche du Registre du service de cluster lors du nettoyage du nœud de cluster.
Le code d’erreur était « %1 ». Il se peut que vous ne puissiez pas créer ou joindre un cluster à cet ordinateur tant que le nettoyage n’a pas abouti. Pour un nettoyage manuel, exécutez l’applet de commande PowerShell’Clear-ClusterNode’sur cet ordinateur.

### <a name="event-4622-nodecleanup_errors"></a>Événement 4622 : NODECLEANUP_ERRORS

La service de cluster a rencontré une erreur lors du nettoyage du nœud. Il se peut que vous ne puissiez pas créer ou joindre un cluster à cet ordinateur tant que le nettoyage n’a pas abouti. Utilisez l’applet de commande PowerShell’Clear-ClusterNode’sur ce nœud.

### <a name="event-4624-nodecleanup_reset_nlbsflags_failed"></a>Événement 4624 : NODECLEANUP_RESET_NLBSFLAGS_FAILED

Échec de la réinitialisation de la valeur de registre de l’Association de sécurité IPSec lors du nettoyage du nœud de cluster. Le code d’erreur était « %1 ». Pour un nettoyage manuel, exécutez l’applet de commande PowerShell’Clear-ClusterNode’sur cet ordinateur. Vous pouvez également réinitialiser le délai d’expiration de l’Association de sécurité IPSec en supprimant la valeur' %2 'et la valeur' %3 'de HKEY_LOCAL_MACHINE dans le Registre Windows.

### <a name="event-4627-nodecleanup_delete_cluster_tasks_failed"></a>Événement 4627 : NODECLEANUP_DELETE_CLUSTER_TASKS_FAILED

Échec de la suppression des tâches en cluster lors du nettoyage du nœud. Le code d’erreur était « %1 ».
Utilisez Windows Planificateur de tâches pour supprimer toutes les tâches en cluster restantes.

### <a name="event-4629-nodecleanup_delete_local_account_failed"></a>Événement 4629 : NODECLEANUP_DELETE_LOCAL_ACCOUNT_FAILED

Lors du nettoyage du nœud, le compte d’utilisateur local qui est géré par le cluster n’a pas été supprimé. Le code d’erreur était « %1 ». Ouvrez utilisateurs et groupes locaux (lusrmgr. msc) pour supprimer le compte.

### <a name="event-4864-res_vsstask_open_failed"></a>Événement 4864 : RES_VSSTASK_OPEN_FAILED

Échec de la création de la ressource de tâche du service de cliché instantané des volumes « %1 ». Le code d’erreur était « %2 ».

### <a name="event-4865-res_vsstask_terminate_task_failed"></a>Événement 4865 : RES_VSSTASK_TERMINATE_TASK_FAILED

Échec de la ressource de tâche du service de cliché instantané des volumes « %1 ». Le code d’erreur était « %2 ».
Cela est dû au fait que la tâche associée n’a pas pu être arrêtée dans le cadre d’une opération hors connexion. Vous devrez peut-être l’arrêter manuellement à l’aide du composant logiciel enfichable Planificateur de tâches.

### <a name="event-4866-res_vsstask_delete_task_failed"></a>Événement 4866 : RES_VSSTASK_DELETE_TASK_FAILED

Échec de la ressource de tâche du service de cliché instantané des volumes « %1 ». Le code d’erreur était « %2 ».
Cela est dû au fait que la tâche associée n’a pas pu être supprimée dans le cadre d’une opération hors connexion. Vous devrez peut-être le supprimer manuellement à l’aide du composant logiciel enfichable Planificateur de tâches.

### <a name="event-4867-res_vsstask_online_failed"></a>Événement 4867 : RES_VSSTASK_ONLINE_FAILED

Échec de la ressource de tâche du service de cliché instantané des volumes « %1 ». Le code d’erreur était « %2 ».
Cela est dû au fait que la tâche associée n’a pas pu être ajoutée dans le cadre d’une opération en ligne. Utilisez le composant logiciel enfichable Planificateur de tâches pour vous assurer que vos tâches sont correctement configurées.

### <a name="event-4868-unable_to_start_autologger"></a>Événement 4868 : UNABLE_TO_START_AUTOLOGGER

Service de cluster n’a pas pu démarrer la session de trace du journal du cluster. Le code d’erreur était « %2 ». Le cluster fonctionne correctement, mais les informations de journalisation supplémentaires sont manquantes. Utilisez le composant logiciel enfichable analyseur de performances pour vérifier les paramètres de canal d’événements pour « %1 ».

### <a name="event-4869-netft_watchdog_process_hung"></a>Événement 4869 : NETFT_WATCHDOG_PROCESS_HUNG

Le contrôle d’intégrité du mode utilisateur a détecté que le système ne répond pas. La carte virtuelle de cluster de basculement a perdu le contact avec le processus « %1 » avec l’ID de processus « %2 », pour « %3 » secondes. Utilisez l’analyseur de performances pour évaluer l’intégrité du système et déterminer quel processus peut avoir un impact négatif sur le système.

### <a name="event-4870-netft_watchdog_process_terminated"></a>Événement 4870 : NETFT_WATCHDOG_PROCESS_TERMINATED

Le contrôle d’intégrité du mode utilisateur a détecté que le système ne répond pas. La carte virtuelle de cluster de basculement a perdu le contact avec le processus « %1 » avec l’ID de processus « %2 », pour « %3 » secondes. Une action de récupération sera effectuée.

### <a name="event-4871-netft_miniport_initialization_failure"></a>Événement 4871 : NETFT_MINIPORT_INITIALIZATION_FAILURE

Le service de cluster n’a pas pu démarrer. Cela s’est dû au fait que la carte miniport n’a pas pu être initialisée par l’adaptateur virtuel de cluster de basculement. Le code d’erreur était « %1 ». Vérifiez que les autres cartes réseau fonctionnent correctement et recherchez les erreurs dans le gestionnaire de périphériques. Si la configuration a été modifiée, il peut être nécessaire de réinstaller la fonctionnalité de clustering de basculement sur cet ordinateur.

### <a name="event-4872-netft_missing_datalink_address"></a>Événement 4872 : NETFT_MISSING_DATALINK_ADDRESS

La carte virtuelle de cluster de basculement n’a pas pu générer une adresse MAC unique.
Soit il n’a pas pu trouver d’adaptateur Ethernet physique à partir duquel générer une adresse unique, soit l’adresse générée est en conflit avec une autre carte de cet ordinateur. Exécutez l’Assistant validation d’une configuration pour vérifier la configuration de votre réseau.

### <a name="event-5122-dcm_event_root_creation_failed"></a>Événement 5122 : DCM_EVENT_ROOT_CREATION_FAILED

Service de cluster n’a pas pu créer le répertoire racine des volumes partagés de cluster' %2 '.
Le message d’erreur était « %1 ».

### <a name="event-5142-dcm_volume_no_access"></a>Événement 5142 : DCM_VOLUME_NO_ACCESS

Volume partagé de cluster « %1 » (« %2 ») n’est plus accessible à partir de ce nœud de cluster en raison de l’erreur « %3 ». Résolvez les problèmes de connectivité de ce nœud au périphérique de stockage et à la connectivité réseau.

### <a name="event-5143-dcm_veto_resource_move_due_to_cc"></a>Événement 5143 : DCM_VETO_RESOURCE_MOVE_DUE_TO_CC

Le déplacement du disque (« %2 ») est refusé en fonction de l’état actuel du gestionnaire de cache sur le nœud « %1 » pour empêcher un blocage potentiel. « Le gestionnaire de cache seuils des pages incorrectes » est %3 et « pages de modifications du gestionnaire de cache » est %4. Le déplacement est autorisé si les « pages de modifications du gestionnaire de cache » sont inférieures à 70% de « pages de modifications du gestionnaire de cache seuils » ou si « pages de modifications du gestionnaire de cache seuils » moins « pages de modifications du gestionnaire de cache » est supérieure à 128000 pages (environ 500 Mo si une taille de page est de 4096 octets).
Transfert de ressources de refus de cluster pour empêcher un blocage potentiel en raison des écritures de mise en mémoire tampon du gestionnaire de cache pendant l’interruption des volumes partagés de cluster sur ce disque.

### <a name="event-5144-dcm_snapshot_diff_area_failure"></a>Événement 5144 : DCM_SNAPSHOT_DIFF_AREA_FAILURE

Lors de l’ajout du disque (« %1 ») aux volumes partagés de cluster, la définition de l’Association de zone diff snapshot explicite pour le volume (« %2 ») a échoué avec l’erreur « %3 ». La seule association de zone diff de l’instantané logiciel prise en charge pour les volumes partagés de cluster est à proprement dit.

### <a name="event-5145-dcm_snapshot_diff_area_delete_failure"></a>Événement 5145 : DCM_SNAPSHOT_DIFF_AREA_DELETE_FAILURE

La ressource de disque de cluster « %1 » n’a pas pu supprimer un instantané logiciel. Impossible de dissocier la zone diff du volume' %3 'du volume' %2 '. Cela peut être dû à des captures instantanées actives. Les volumes partagés de cluster nécessitent que l’instantané logiciel se trouve sur le même disque.

### <a name="event-5146-dcm_veto_resource_move_due_to_dismount"></a>Événement 5146 : DCM_VETO_RESOURCE_MOVE_DUE_TO_DISMOUNT

Le déplacement de la ressource de Volume partagé de cluster' %1 'est refusé car l’un des volumes appartenant à la ressource est dans un état démonté. Réessayez l’action une fois l’opération de démontage terminée.

### <a name="event-5147-dcm_veto_resource_move_due_to_snapshot"></a>Événement 5147 : DCM_VETO_RESOURCE_MOVE_DUE_TO_SNAPSHOT

Le déplacement de la ressource de Volume partagé de cluster' %1 'est refusé car l’un des volumes appartenant à la ressource est dans un état démonté. Réessayez l’action une fois l’opération de démontage terminée.

### <a name="event-5148-dcm_veto_resource_move_due_to_io_mode_change"></a>Événement 5148 : DCM_VETO_RESOURCE_MOVE_DUE_TO_IO_MODE_CHANGE

Le déplacement de la ressource de Volume partagé de cluster' %1 'est refusé, car une opération de modification en mode e/s (e/s directe vers des e/s redirigées ou vice versa) est en cours sur l’un des volumes appartenant à la ressource. Réessayez l’action une fois l’opération terminée.

### <a name="event-5150-dcm_set_resource_in_failed_state"></a>Événement 5150 : DCM_SET_RESOURCE_IN_FAILED_STATE

La ressource de disque physique de cluster « %1 » a échoué. La Volume partagé de cluster a été placée dans un état d’échec avec l’erreur suivante : « %2 »

### <a name="event-5200-cam_cannot_create_cno_token"></a>Événement 5200 : CAM_CANNOT_CREATE_CNO_TOKEN

Service de cluster n’a pas pu créer un jeton d’identité de cluster pour les volumes partagés de cluster. Le code d’erreur était « %1 ». Assurez-vous que le contrôleur de domaine est accessible et vérifiez les problèmes de connectivité. Tant que la connexion au contrôleur de domaine n’a pas été récupérée, certaines opérations sur ce nœud sur les volumes partagés de cluster peuvent échouer.

### <a name="event-5216-csv_sw_snapshot_failed"></a>Événement 5216 : CSV_SW_SNAPSHOT_FAILED

Échec de la création de l’instantané logiciel sur le Volume partagé de cluster' %1 ' (' %2 ') avec l’erreur %3. La ressource doit être en ligne pour prendre en charge la création d’instantanés. Vérifiez l’état de la ressource.

### <a name="event-5217-csv_sw_snapshot_set_failed"></a>Événement 5217 : CSV_SW_SNAPSHOT_SET_FAILED

Échec de la création de l’instantané logiciel sur Volume partagé de cluster (s) (« %1 ») avec l’ID du jeu d’instantanés « %2 » avec l’erreur « %3 ». Vérifiez l’état des ressources CSV et les événements système des nœuds propriétaires de ressources.

### <a name="event-5219-csv_register_snapshot_prov_with_vss_failed"></a>Événement 5219 : CSV_REGISTER_SNAPSHOT_PROV_WITH_VSS_FAILED

Service de cluster n’a pas pu inscrire le fournisseur de capture instantanée des volumes partagés de cluster avec le service VSS (Volume Shadow service). Cela peut être dû au fait que le service VSS s’arrête ou peut être confronté à un problème avec le service VSS ayant un problème qui l’empêche d’accepter les demandes entrantes. <br>Erreur : %1

### <a name="event-5377-operation_exceeded_timeout"></a>Événement 5377 : OPERATION_EXCEEDED_TIMEOUT

Une opération de service de cluster interne a dépassé le seuil défini de « %2 » secondes. La service de cluster a été arrêtée pour la récupération. Le gestionnaire de contrôle des services va redémarrer le service de cluster et le nœud rejoindra le cluster.

### <a name="event-5396-two_partitions_have_quorum"></a>Événement 5396 : TWO_PARTITIONS_HAVE_QUORUM

L’service de cluster sur ce nœud est en cours d’arrêt, car il a détecté qu’il existe d’autres nœuds de cluster dotés du quorum. Cela se produit lorsque le service de cluster détecte un autre nœud qui a été démarré avec le commutateur de quorum de force (/FQ). Le nœud qui a été démarré avec le commutateur forcer le quorum restera en cours d’exécution. Utilisez Gestionnaire du cluster de basculement pour vérifier que ce nœud rejoint automatiquement le cluster lors du redémarrage du service de cluster.

### <a name="event-5397-rlua_account_failed"></a>Événement 5397 : RLUA_ACCOUNT_FAILED

La ressource de cluster « %1 » n’a pas pu créer ou modifier le compte d’utilisateur local répliqué « %2 » sur ce nœud. Pour plus d’informations, consultez les journaux du cluster.

### <a name="event-5398-nm_event_cluster_failed_to_form"></a>Événement 5398 : NM_EVENT_CLUSTER_FAILED_TO_FORM

Le cluster n’a pas pu démarrer. La dernière copie des données de configuration de cluster n’était pas disponible dans l’ensemble des nœuds tentant de démarrer le cluster. Les modifications apportées au cluster s’est produite alors que l’ensemble de nœuds n’était pas dans l’appartenance et n’a donc pas pu recevoir les mises à jour des données de configuration. .<br><br>Votes requis pour démarrer le cluster : %1<br>Votes disponibles : %2<br>Nœuds avec votes : %3

#### <a name="guidance"></a>Assistance

Essayez de démarrer le service de cluster sur tous les nœuds du cluster afin que les nœuds avec la dernière copie des données de configuration du cluster puissent tout d’abord former le cluster. Le cluster pourra démarrer et les nœuds obtiendront automatiquement les données de configuration de cluster mises à jour. Si aucun nœud n’est disponible avec la dernière copie des données de configuration du cluster, exécutez l’applet de commande Windows PowerShell « Start-ClusterNode-FQ ». L’utilisation du paramètre ForceQuorum (FQ) permet de démarrer le service de cluster et de marquer la copie de ce nœud des données de configuration du cluster comme faisant autorité. Le quorum forcé sur un nœud avec une copie obsolète de la base de données du cluster peut entraîner des modifications de la configuration du cluster qui se sont produites alors que le nœud n’était pas impliqué dans le cluster à perdre.

## <a name="warning-events"></a>Événements d’avertissement

### <a name="event-1011-nm_node_evicted"></a>Événement 1011 : NM_NODE_EVICTED

Le nœud de cluster %1 a été supprimé du cluster de basculement.

### <a name="event-1045-res_ipaddr_ipv4_address_create_failed"></a>Événement 1045 : RES_IPADDR_IPV4_ADDRESS_CREATE_FAILED

Aucune interface réseau correspondante n’a été trouvée pour l’adresse IP « %2 » de la ressource « %1 » (code de retour : « %3 »). Si vos nœuds de cluster s’étendent sur des sous-réseaux différents, cela peut être normal.

### <a name="event-1066-res_disk_corrupt_disk"></a>Événement 1066 : RES_DISK_CORRUPT_DISK

La ressource de disque de cluster « %1 » indique une altération pour le volume « %2 ». CHKDSK est en cours d’exécution pour résoudre les problèmes. Le disque sera indisponible jusqu’à ce que CHKDSK se termine.
La sortie CHKDSK sera enregistrée dans le fichier « %3 ».<br> CHKDSK peut également écrire des informations dans le journal des événements de l’application.

### <a name="event-1068-res_smb_share_cant_add"></a>Événement 1068 : RES_SMB_SHARE_CANT_ADD

La ressource de partage de fichiers de cluster « %1 » ne peut pas être mise en ligne. La création du partage de fichiers' %2 ' (avec le nom de réseau %3) a échoué en raison de l’erreur' %4 '. Cette opération sera automatiquement retentée.

### <a name="event-1071-rcm_resource_online_blocked_by_locked_mode"></a>Événement 1071 : RCM_RESOURCE_ONLINE_BLOCKED_BY_LOCKED_MODE

L’opération tentée sur la ressource de cluster' %1 'de type' %3 'dans le rôle en cluster' %2 'n’a pas pu être effectuée parce que la ressource ou l’un de ses fournisseurs a l’état verrouillé.

### <a name="event-1071-rcm_resource_offline_blocked_by_locked_mode"></a>Événement 1071 : RCM_RESOURCE_OFFLINE_BLOCKED_BY_LOCKED_MODE

L’opération tentée sur la ressource de cluster' %1 'de type' %3 'dans le rôle en cluster' %2 'n’a pas pu être effectuée parce que la ressource ou l’une de ses dépendances a un état verrouillé.

### <a name="event-1094-sm_invalid_security_level"></a>Événement 1094 : SM_INVALID_SECURITY_LEVEL

La propriété commune SecurityLevel du cluster ne peut pas être modifiée sur ce cluster. Impossible de modifier le niveau de sécurité du cluster, car le cluster est actuellement configuré pour le mode aucune authentification.

### <a name="event-1119-res_netname_dns_registration_missing"></a>Événement 1119 : RES_NETNAME_DNS_REGISTRATION_MISSING

La ressource de nom de réseau de cluster « %1 » n’a pas pu inscrire le nom DNS « %2 » sur l’adaptateur « %4 » pour la raison suivante : <br><br>« %3 »

### <a name="event-1125-tm_event_cluster_netinterface_unreachable"></a>Événement 1125 : TM_EVENT_CLUSTER_NETINTERFACE_UNREACHABLE

L’interface réseau de cluster « %1 » pour le nœud de cluster « %2 » sur le réseau « %3 » est inaccessible par au moins un autre nœud de cluster attaché au réseau. Le cluster de basculement n’a pas pu déterminer l’emplacement de l’échec. Exécutez l’Assistant validation d’une configuration pour vérifier la configuration de votre réseau. Si la condition persiste, recherchez les erreurs matérielles ou logicielles liées à la carte réseau. Vérifiez également les défaillances dans les autres composants réseau auxquels le nœud est connecté, comme des concentrateurs, des commutateurs ou des ponts.

### <a name="event-1149-res_netname_cant_delete_dns_records"></a>Événement 1149 : RES_NETNAME_CANT_DELETE_DNS_RECORDS

Les enregistrements de l’hôte DNS (A) et du pointeur (PTR) associés à la ressource de cluster' %1 'n’ont pas été supprimés du serveur DNS associé à la ressource. Si nécessaire, vous pouvez les supprimer manuellement. Contactez votre administrateur DNS pour faciliter ce travail.

### <a name="event-1150-res_netname_dns_ptr_record_delete_failed"></a>Événement 1150 : RES_NETNAME_DNS_PTR_RECORD_DELETE_FAILED

La suppression de l’enregistrement de pointeur DNS (PTR) « %2 » pour l’hôte « %3 » qui est associé à la ressource de nom de réseau de cluster « %1 » a échoué avec l’erreur « %4 ».
Si nécessaire, l’enregistrement peut être supprimé manuellement. Contactez votre administrateur DNS pour obtenir de l’aide.

### <a name="event-1151-res_netname_dns_a_record_delete_failed"></a>Événement 1151 : RES_NETNAME_DNS_A_RECORD_DELETE_FAILED

La suppression de l’enregistrement d’hôte DNS (A) « %2 » associé à la ressource de nom de réseau de cluster « %1 » a échoué avec l’erreur « %3 ». Si nécessaire, l’enregistrement peut être supprimé manuellement. Contactez votre administrateur DNS pour obtenir de l’aide.

### <a name="event-1155-rcm_event_exited_queuing"></a>Événement 1155 : RCM_EVENT_EXITED_QUEUING

Le déplacement en attente pour le rôle' %1 'ne s’est pas terminé.

### <a name="event-1197-res_netname_delete_disable_failed"></a>Événement 1197 : RES_NETNAME_DELETE_DISABLE_FAILED

La ressource de nom de réseau de cluster « %1 » n’a pas pu supprimer ou désactiver l’objet ordinateur associé « %2 » pendant la suppression des ressources. Le code d’erreur était « %3 ». <br>Vérifiez que le site est en lecture seule. Assurez-vous que l’objet nom de cluster dispose des autorisations appropriées sur l’objet « %2 » dans Active Directory.

### <a name="event-1198-res_netname_delete_vco_guid_failed"></a>Événement 1198 : RES_NETNAME_DELETE_VCO_GUID_FAILED

La ressource de nom de réseau de cluster « %1 » n’a pas pu supprimer l’objet ordinateur avec le GUID « %2 ». Le code d’erreur était « %3 ». <br>Vérifiez que le site est en lecture seule. Assurez-vous que l’objet nom de cluster dispose des autorisations appropriées sur l’objet « %2 » dans Active Directory.

### <a name="event-1216-service_netname_change_warning"></a>Événement 1216 : SERVICE_NETNAME_CHANGE_WARNING

Une opération de modification de nom sur la ressource de NetName principale du cluster a échoué.
La tentative de restauration du nom d’origine de l’opération de changement de nom a échoué. Le code d’erreur était « %1 ». Vous ne pourrez peut-être pas gérer à distance le cluster à l’aide du nom du cluster tant que cette situation n’aura pas été corrigée manuellement.

### <a name="event-1221-res_netname_rename_out_of_synch_with_compobj"></a>Événement 1221 : RES_NETNAME_RENAME_OUT_OF_SYNCH_WITH_COMPOBJ

La ressource de nom de réseau de cluster « %1 » a un nom « %2 » qui ne correspond pas au nom d’objet d’ordinateur correspondant « %3 ». Il est probable qu’un changement de nom précédent de l’objet ordinateur n’ait pas été répliqué sur tous les contrôleurs de domaine du domaine. Vous ne pouvez pas renommer la ressource de nom de réseau tant que les noms ne sont pas cohérents. Si l’objet ordinateur n’a pas été récemment modifié, contactez votre administrateur de domaine pour renommer l’objet ordinateur et le rendre cohérent. En outre, assurez-vous que la réplication entre les contrôleurs de domaine s’est correctement déroulée.

### <a name="event-1222-res_netname_set_permissions_failed"></a>Événement 1222 : RES_NETNAME_SET_PERMISSIONS_FAILED

L’objet ordinateur associé à la ressource de nom réseau de cluster « %1 » n’a pas pu être mis à jour.<br><br>Le texte du code d’erreur associé est : %2<br> <br>L’identité de cluster « %3 » peut ne pas avoir les autorisations nécessaires pour mettre à jour l’objet. Collaborez avec votre administrateur de domaine pour vous assurer que l’identité de cluster peut mettre à jour les objets ordinateur dans le domaine.

### <a name="event-1240-res_ipaddr_obtain_lease_failed"></a>Événement 1240 : RES_IPADDR_OBTAIN_LEASE_FAILED

La ressource d’adresse IP de cluster « %1 » n’a pas pu obtenir une adresse IP louée.

### <a name="event-1243-res_ipaddr_release_lease_failed"></a>Événement 1243 : RES_IPADDR_RELEASE_LEASE_FAILED

La ressource d’adresse IP de cluster « %1 » n’a pas pu libérer le bail pour l’adresse IP « %2 ».

### <a name="event-1251-rcm_group_preempted"></a>Événement 1251 : RCM_GROUP_PREEMPTED

Le rôle en cluster' %2 'a été mis hors connexion. Ce rôle a été devancé par le rôle en cluster « %1 » avec une priorité plus élevée. Le service de cluster tentera de mettre le rôle en cluster' %2 'en ligne plus tard lorsque les ressources système seront disponibles.

### <a name="event-1544-service_vss_onabort"></a>Événement 1544 : SERVICE_VSS_ONABORT

L’opération de sauvegarde des données de configuration du cluster a été annulée. L’enregistreur du Service VSS du cluster (VSS) a reçu une demande d’abandon.

### <a name="event-1548-service_connect_version_compatible"></a>Événement 1548 : SERVICE_CONNECT_VERSION_COMPATIBLE

Le nœud « %1 » a établi la communication avec le nœud « %2 » et a détecté qu’il exécute une version différente, mais compatible, du système d’exploitation. Nous recommandons que tous les nœuds exécutent la même version du système d’exploitation. Une fois que tous les nœuds ont été mis à niveau, exécutez l’applet de commande Windows PowerShell Update-ClusterFunctionalLevel pour terminer la mise à niveau du cluster.

### <a name="event-1550-service_connect_novercheck"></a>Événement 1550 : SERVICE_CONNECT_NOVERCHECK

Le nœud « %1 » a établi une session de communication avec le nœud « %2 » sans effectuer de vérification de compatibilité de version, car la vérification de la compatibilité des versions est désactivée. La désactivation de la vérification de la compatibilité des versions n’est pas prise en charge.

### <a name="event-1551-service_form_novercheck"></a>Événement 1551 : SERVICE_FORM_NOVERCHECK

Le nœud' %1 'a formé un cluster de basculement sans effectuer de vérification de compatibilité de version, car la vérification de compatibilité de version est désactivée. La désactivation de la vérification de la compatibilité des versions n’est pas prise en charge.

### <a name="event-1555-service_netft_disable_autoconfig_failed"></a>Événement 1555 : SERVICE_NETFT_DISABLE_AUTOCONFIG_FAILED

Échec de la tentative d’utilisation d’IPv4 pour la carte réseau « %1 ». Cela est dû à l’échec de la désactivation de la configuration automatique IPv4 et de DHCP. Cela peut se produire si le service client DHCP est déjà désactivé. IPv6 sera utilisé s’il est activé ; sinon, ce nœud ne pourra peut-être pas participer à un cluster de basculement.

### <a name="event-1558-service_witness_failover_attempt"></a>Événement 1558 : SERVICE_WITNESS_FAILOVER_ATTEMPT

Le service de cluster a détecté un problème avec la ressource témoin. La ressource témoin sera basculée vers un autre nœud au sein du cluster pour tenter de rétablir l’accès aux données de configuration du cluster.

### <a name="event-1562-res_fsw_alivefailure"></a>Événement 1562 : RES_FSW_ALIVEFAILURE

La ressource témoin de partage de fichiers « %1 » n’a pas pu procéder à un contrôle d’intégrité périodique sur le partage de fichiers « %2 ». Vérifiez que le partage de fichiers « %2 » existe et qu’il est accessible par le cluster.

### <a name="event-1576-res_netname_dns_registration_secure_zone_refused"></a>Événement 1576 : RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_REFUSED

La ressource de nom de réseau de cluster « %1 » n’a pas pu inscrire le nom « %2 » sur l’adaptateur « %4 » dans une zone DNS sécurisée. Cela était dû à un enregistrement existant portant ce nom et l’identité du cluster ne dispose pas des privilèges suffisants pour mettre à jour cet enregistrement. Le code d’erreur était « %3 ». Contactez l’administrateur de votre serveur DNS pour vérifier que l’identité du cluster dispose des autorisations sur l’enregistrement DNS' %2 '.

### <a name="event-1576-res_netname_dns_registration_secure_zone_refused"></a>Événement 1576 : RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_REFUSED

La ressource de nom de réseau de cluster « %1 » n’a pas pu inscrire le nom « %2 » sur l’adaptateur « %4 » dans une zone DNS sécurisée. Cela était dû à un enregistrement existant portant ce nom et l’identité du cluster ne dispose pas des privilèges suffisants pour mettre à jour cet enregistrement. Le code d’erreur était « %3 ». Contactez l’administrateur de votre serveur DNS pour vérifier que l’identité du cluster dispose des autorisations sur l’enregistrement DNS' %2 '.

### <a name="event-1577-res_netname_dns_server_could_not_be_contacted"></a>Événement 1577 : RES_NETNAME_DNS_SERVER_COULD_NOT_BE_CONTACTED

La ressource de nom de réseau de cluster « %1 » n’a pas pu inscrire le nom « %2 » sur l’adaptateur « %4 ». Impossible de contacter le serveur DNS. Le code d’erreur était « %3 ». Assurez-vous qu’un serveur DNS est accessible à partir de ce nœud de cluster. L’inscription DNS sera retentée ultérieurement.

### <a name="event-1577-res_netname_dns_server_could_not_be_contacted"></a>Événement 1577 : RES_NETNAME_DNS_SERVER_COULD_NOT_BE_CONTACTED

La ressource de nom de réseau de cluster « %1 » n’a pas pu inscrire le nom « %2 » sur l’adaptateur « %4 ». Impossible de contacter le serveur DNS. Le code d’erreur était « %3 ». Assurez-vous qu’un serveur DNS est accessible à partir de ce nœud de cluster. L’inscription DNS sera retentée ultérieurement.

### <a name="event-1578-res_netname_dns_test_for_dynamic_update_failed"></a>Événement 1578 : RES_NETNAME_DNS_TEST_FOR_DYNAMIC_UPDATE_FAILED

La ressource de nom de réseau de cluster « %1 » n’a pas pu inscrire les mises à jour dynamiques pour le nom « %2 » sur l’adaptateur « %4 ». Le serveur DNS n’est peut-être pas configuré pour accepter les mises à jour dynamiques. Le code d’erreur était « %3 ». Veuillez contacter l’administrateur de votre serveur DNS pour vérifier que le serveur DNS est disponible et configuré pour les mises à jour dynamiques.<br><br>Vous pouvez également désactiver les mises à jour DNS dynamiques en désactivant le paramètre « enregistrer les adresses de cette connexion dans le système DNS » dans les paramètres TCP/IP avancés pour l’adaptateur « %4 » sous l’onglet DNS.

### <a name="event-1578-res_netname_dns_test_for_dynamic_update_failed"></a>Événement 1578 : RES_NETNAME_DNS_TEST_FOR_DYNAMIC_UPDATE_FAILED

La ressource de nom de réseau de cluster « %1 » n’a pas pu inscrire les mises à jour dynamiques pour le nom « %2 » sur l’adaptateur « %4 ». Le serveur DNS n’est peut-être pas configuré pour accepter les mises à jour dynamiques. Le code d’erreur était « %3 ». Veuillez contacter l’administrateur de votre serveur DNS pour vérifier que le serveur DNS est disponible et configuré pour les mises à jour dynamiques.<br><br>Vous pouvez également désactiver les mises à jour DNS dynamiques en désactivant le paramètre « enregistrer les adresses de cette connexion dans le système DNS » dans les paramètres TCP/IP avancés pour l’adaptateur « %4 » sous l’onglet DNS.

### <a name="event-1579-res_netname_dns_record_update_failed"></a>Événement 1579 : RES_NETNAME_DNS_RECORD_UPDATE_FAILED

La ressource de nom de réseau de cluster « %1 » n’a pas pu mettre à jour l’enregistrement DNS pour le nom « %2 » sur l’adaptateur « %4 ». Le code d’erreur était « %3 ». Assurez-vous qu’un serveur DNS est accessible à partir de ce nœud de cluster et contactez l’administrateur de votre serveur DNS pour vérifier que l’identité du cluster peut mettre à jour l’enregistrement DNS' %2 '.

### <a name="event-1579-res_netname_dns_record_update_failed"></a>Événement 1579 : RES_NETNAME_DNS_RECORD_UPDATE_FAILED

La ressource de nom de réseau de cluster « %1 » n’a pas pu mettre à jour l’enregistrement DNS pour le nom « %2 » sur l’adaptateur « %4 ». Le code d’erreur était « %3 ». Assurez-vous qu’un serveur DNS est accessible à partir de ce nœud de cluster et contactez l’administrateur de votre serveur DNS pour vérifier que l’identité du cluster peut mettre à jour l’enregistrement DNS' %2 '.

### <a name="event-1581-clussvc_unable_to_move_hive_to_safe_file"></a>Événement 1581 : CLUSSVC_UNABLE_TO_MOVE_HIVE_TO_SAFE_FILE

La demande de restauration des données de configuration du cluster n’a pas pu effectuer une copie du fichier de données de configuration de cluster existant (ClusDB). Lors de la tentative de conservation de la configuration existante, l’opération de restauration n’a pas pu créer de copie à l’emplacement « %1 ». Cela peut se produire si le fichier de données de configuration existant est endommagé. L’opération de restauration s’est poursuivie mais les tentatives de restauration de la configuration de cluster existante ne sont peut-être pas possibles.

### <a name="event-1582-clussvc_unable_to_move_restored_hive_to_current"></a>Événement 1582 : CLUSSVC_UNABLE_TO_MOVE_RESTORED_HIVE_TO_CURRENT

Le service de cluster n’a pas pu déplacer la ruche de cluster restaurée de « %1 » vers « %2 ». Cela peut empêcher l’opération de restauration de réussir correctement. Si la configuration du cluster n’a pas été correctement restaurée, réessayez l’action de restauration.

### <a name="event-1583-clussvc_netft_disable_connectionsecurity_failed"></a>Événement 1583 : CLUSSVC_NETFT_DISABLE_CONNECTIONSECURITY_FAILED

Service de cluster n’a pas pu désactiver la sécurité du protocole Internet (IPsec) sur la carte virtuelle de cluster de basculement « %1 ». Cela peut avoir un impact négatif sur les performances de communication du cluster. Si ce problème persiste, vérifiez que vos stratégies de sécurité de connexion locale et de domaine s’appliquent à IPSec et au pare-feu Windows. Vérifiez également les événements liés au service de moteur de filtrage de base.

### <a name="event-1584-shared_volume_not_ready_for_snapshot"></a>Événement 1584 : SHARED_VOLUME_NOT_READY_FOR_SNAPSHOT

Une application de sauvegarde a initié un instantané VSS sur le Volume partagé de cluster « %1 » (« %3 ») sans préparer correctement le volume de l’instantané. Cet instantané n’est peut-être pas valide et la sauvegarde n’est peut-être pas utilisable pour les opérations de restauration. Veuillez contacter le fournisseur de l’application de sauvegarde pour vérifier la compatibilité avec les volumes partagés de cluster.

### <a name="event-1589-res_netname_dns_returning_ip_that_is_not_provider"></a>Événement 1589 : RES_NETNAME_DNS_RETURNING_IP_THAT_IS_NOT_PROVIDER

La ressource de nom de réseau de cluster « %1 » a trouvé une ou plusieurs adresses IP associées au nom DNS « %2 » qui ne sont pas des ressources d’adresse IP dépendantes. Les adresses supplémentaires trouvées sont « %3 ». Cela peut affecter la connectivité du client jusqu’à ce que le nom du réseau et ses enregistrements DNS associés soient cohérents. Contactez l’administrateur de votre serveur DNS pour vérifier les enregistrements associés au nom « %2 ».

### <a name="event-1604-res_disk_chkdsk_spotfix_needed"></a>Événement 1604 : RES_DISK_CHKDSK_SPOTFIX_NEEDED

La ressource de disque de cluster' %1 'a détecté une altération pour le volume' %2 '. Spotfix CHKDSK est nécessaire pour résoudre les problèmes.

### <a name="event-1605-res_disk_spotfix_performed"></a>Événement 1605 : RES_DISK_SPOTFIX_PERFORMED

La ressource de disque de cluster' %1 'a terminé l’exécution de ChkDsk. exe/spotfix sur le volume' %2 '.
Code de retour : ' %4 '. La sortie de ChkDsk a été enregistrée dans le fichier « %3 ».<br>
Consultez le journal des événements d’application pour obtenir des informations supplémentaires à partir de ChkDsk.

### <a name="event-1671-res_disk_online_set_attributes_completed_failure"></a>Événement 1671 : RES_DISK_ONLINE_SET_ATTRIBUTES_COMPLETED_FAILURE

Impossible de mettre en ligne la ressource de disque physique de cluster.<br><br>Nom de la ressource de disque physique : %1<br>Code d’erreur : %2<br>Temps écoulé (secondes) : %3

#### <a name="guidance"></a>Assistance

Exécutez l’Assistant validation d’une configuration pour vérifier la configuration de votre stockage. Si le code d’erreur a été ERROR_CLUSTER_SHUTDOWN, l’État en attente en ligne a été annulé par un administrateur. S’il s’agit d’un volume répliqué, cela peut être dû à un échec de définition des attributs de disque. Pour plus d’informations, consultez les événements de réplication du stockage.

### <a name="event-1673-cluster_node_entered_isolated_state"></a>Événement 1673 : CLUSTER_NODE_ENTERED_ISOLATED_STATE

Le nœud de cluster' %1 'est passé à l’état isolé.

### <a name="event-1675-event_joining_node_quarantined"></a>Événement 1675 : EVENT_JOINING_NODE_QUARANTINED

Le nœud de cluster « %1 » a été mis en quarantaine par « %2 » et ne peut pas rejoindre le cluster. Le nœud sera mis en quarantaine jusqu’à « %3 », puis le nœud essaiera automatiquement de rejoindre le cluster. <br><br>Reportez-vous aux journaux des événements système et d’application pour déterminer les problèmes sur ce nœud. Lorsque le problème est résolu, la mise en quarantaine peut être désactivée manuellement pour permettre au nœud de rejoindre l’applet de commande Windows PowerShell « Start-ClusterNode – ClearQuarantine ».<br><br>Nom du nœud : %1<br>QuarantineType : mise en quarantaine par %2<br>L’heure de mise en quarantaine sera automatiquement désactivée : %3

### <a name="event-1681-cluster_groups_unmonitored_on_node"></a>Événement 1681 : CLUSTER_GROUPS_UNMONITORED_ON_NODE

Les ordinateurs virtuels sur le nœud « %1 » sont entrés dans un État non surveillé. L’intégrité des machines virtuelles est de nouveau analysée une fois que le nœud est retourné à partir d’un état isolé ou peut basculer si le nœud ne retourne pas. L’ordinateur virtuel n’est plus analysé : %2.

### <a name="event-1689-event_disable_and_stop_other_service"></a>Événement 1689 : EVENT_DISABLE_AND_STOP_OTHER_SERVICE

Le service de cluster a détecté un service qui n’est pas compatible avec le clustering de basculement. Le service a été désactivé pour éviter d’éventuels problèmes liés au cluster de basculement.<br><br>Nom du service : ' %1 '

### <a name="event-4625-nodecleanup_reset_nlbsflags_preserved"></a>Événement 4625 : NODECLEANUP_RESET_NLBSFLAGS_PRESERVED

Échec de la réinitialisation de la valeur de registre de l’Association de sécurité IPSec lors du nettoyage du nœud de cluster. Cela est dû au fait que le délai d’expiration de l’Association de sécurité IPSec a été modifié après la configuration de cet ordinateur en tant que membre d’un cluster. Pour un nettoyage manuel, exécutez l’applet de commande PowerShell’Clear-ClusterNode’sur cet ordinateur. Vous pouvez également réinitialiser le délai d’expiration de l’Association de sécurité IPSec en supprimant la valeur « %1 » et la valeur « %2 » de HKEY_LOCAL_MACHINE dans le Registre Windows.

### <a name="event-4873-netft_clussvc_terminate_because_of_watchdog"></a>Événement 4873 : NETFT_CLUSSVC_TERMINATE_BECAUSE_OF_WATCHDOG

Le service de cluster a détecté que la carte virtuelle de cluster de basculement s’est arrêtée. Cela est attendu lorsque l’UC de remplacement à chaud est exécutée sur ce nœud.
Service de cluster s’arrête et doit redémarrer automatiquement une fois l’opération terminée. Recherchez d’autres événements associés au service et assurez-vous que le service de cluster a été redémarré sur ce nœud.

### <a name="event-5120-dcm_volume_auto_pause_after_failure"></a>Événement 5120 : DCM_VOLUME_AUTO_PAUSE_AFTER_FAILURE

Volume partagé de cluster' %1 ' (' %2 ') est passé à l’état suspendu en raison de' %3 '.
Toutes les e/s seront temporairement mises en file d’attente jusqu’à ce qu’un chemin d’accès au volume soit rétabli.

### <a name="event-5123-dcm_event_root_rename_success"></a>Événement 5123 : DCM_EVENT_ROOT_RENAME_SUCCESS

Le répertoire racine des volumes partagés de cluster' %1 'existe déjà. Le répertoire « %1 » a été renommé en « %2 ». Vérifiez que les applications qui accèdent aux données à cet emplacement ont été mises à jour si nécessaire.

### <a name="event-5124-dcm_unsafe_filters_found"></a>Événement 5124 : DCM_UNSAFE_FILTERS_FOUND

Volume partagé de cluster « %1 » (« %3 ») a identifié un ou plusieurs pilotes de filtre actifs sur cette pile d’appareils qui peuvent interférer avec les opérations CSV. L’accès aux e/s sera redirigé vers le périphérique de stockage sur le réseau par le biais d’un autre nœud de cluster. Cela peut entraîner une dégradation des performances. Contactez le fournisseur du pilote de filtre pour vérifier l’interopérabilité avec les volumes partagés de cluster. <br><br>Pilotes de filtre actifs détectés :<br>%2

### <a name="event-5125-dcm_unsafe_volfilter_found"></a>Événement 5125 : DCM_UNSAFE_VOLFILTER_FOUND

Volume partagé de cluster « %1 » (« %3 ») a identifié un ou plusieurs pilotes de volume actifs sur cette pile d’appareils qui peuvent interférer avec les opérations CSV. L’accès aux e/s sera redirigé vers le périphérique de stockage sur le réseau par le biais d’un autre nœud de cluster. Cela peut entraîner une dégradation des performances. Contactez le fournisseur du pilote de volume pour vérifier l’interopérabilité avec les volumes partagés de cluster. <br><br>Pilotes de volume actifs détectés :<br>%2

### <a name="event-5126-dcm_event_cannot_disable_short_names"></a>Événement 5126 : DCM_EVENT_CANNOT_DISABLE_SHORT_NAMES

La ressource de disque physique « %1 » n’autorise pas la désactivation de la génération de noms courts. Cela peut entraîner des problèmes de compatibilité des applications. Utilisez « fsutil 8dot3name Set 2 » pour autoriser la désactivation de la génération de noms courts, puis hors connexion/en ligne de la ressource.

### <a name="event-5128-dcm_event_cannot_disable_short_names"></a>Événement 5128 : DCM_EVENT_CANNOT_DISABLE_SHORT_NAMES

La ressource de disque physique « %1 » n’autorise pas la désactivation de la génération de noms courts. Cela peut entraîner des problèmes de compatibilité des applications. Utilisez « fsutil 8dot3name Set 2 » pour autoriser la désactivation de la génération de noms courts, puis hors connexion/en ligne de la ressource.

### <a name="event-5133-dcm_cannot_restore_drive_letters"></a>Événement 5133 : DCM_CANNOT_RESTORE_DRIVE_LETTERS

Le disque de cluster « %1 » a été supprimé et remis dans le groupe de clusters « stockage disponible ». Au cours de ce processus, une tentative de restauration de la ou des lettres de lecteur d’origine a pris plus de temps que prévu, peut-être parce que ces lettres de lecteur étaient déjà en cours d’utilisation.

### <a name="event-5134-dcm_cannot_set_acl_on_root"></a>Événement 5134 : DCM_CANNOT_SET_ACL_ON_ROOT

Service de cluster n’a pas pu définir les autorisations (ACL) sur le répertoire racine des volumes partagés de cluster' %1 '. L’erreur était « %2 ».

### <a name="event-5135-dcm_cannot_set_acl_on_volume_folder"></a>Événement 5135 : DCM_CANNOT_SET_ACL_ON_VOLUME_FOLDER

Service de cluster n’a pas pu définir les autorisations sur le répertoire Volume partagé de cluster' %1 ' (' %2 '). L’erreur était « %3 ».

### <a name="event-5136-dcm_csv_into_redirected_mode"></a>Événement 5136 : DCM_CSV_INTO_REDIRECTED_MODE

L’accès Redirigé de l’Volume partagé de cluster « %1 » (« %2 ») a été activé. L’accès au dispositif de stockage est redirigé sur le réseau à partir de tous les nœuds de cluster qui accèdent à ce volume. Cela peut entraîner une dégradation des performances. Désactivez l’accès Redirigé pour ce volume pour reprendre les opérations normales.

### <a name="event-5149-dcm_csv_block_cache_resized"></a>Événement 5149 : DCM_CSV_BLOCK_CACHE_RESIZED

La taille du cache a été redimensionnée à' %1 'en fonction de la mémoire remplie, la configuration de l’utilisateur est trop élevée.

### <a name="event-5156-dcm_volume_auto_pause_after_snapshot_failure"></a>Événement 5156 : DCM_VOLUME_AUTO_PAUSE_AFTER_SNAPSHOT_FAILURE

Volume partagé de cluster' %1 ' (' %2 ') est passé à l’état suspendu en raison de' %3 '.
Cette erreur se produit lorsque les instantanés de Volsnap sous-jacents au volume CSV sont supprimés en dehors d’une demande de l’utilisateur. Les causes possibles de la suppression des captures instantanées sont le manque d’espace dans le volume pour les captures instantanées, ou l’échec d’e/s lors de la tentative de mise à jour des données d’instantané. Toutes les e/s seront temporairement mises en file d’attente jusqu’à ce que l’état de la capture instantanée soit synchronisé avec Volsnap.

### <a name="event-5157-dcm_volume_auto_pause_after_failure_expected"></a>Événement 5157 : DCM_VOLUME_AUTO_PAUSE_AFTER_FAILURE_EXPECTED

Volume partagé de cluster' %1 ' (' %2 ') est passé à l’état suspendu en raison de' %3 '.
Toutes les e/s seront temporairement mises en file d’attente jusqu’à ce qu’un chemin d’accès au volume soit rétabli.
Cette erreur est généralement causée par une défaillance de l’infrastructure. Par exemple, la perte de connectivité au stockage ou au nœud possédant le Volume partagé de cluster supprimé de l’appartenance au cluster actif.

### <a name="event-5394-pool_online_warnings"></a>Événement 5394 : POOL_ONLINE_WARNINGS

Le service de cluster a rencontré des erreurs de stockage en tentant de mettre en ligne le pool de stockage. Exécutez la validation de stockage du cluster pour résoudre le problème.

### <a name="event-5395-rcm_group_auto_move_storage_pool"></a>Événement 5395 : RCM_GROUP_AUTO_MOVE_STORAGE_POOL

Le cluster déplace le groupe pour le pool de stockage « %1 », car le nœud actuel « %3 » ne dispose pas d’une connectivité optimale aux disques physiques du pool de stockage.

## <a name="informational-events"></a>Événements d’information

### <a name="event-1592-clussvc_tcp_reconnect_connection_reestablished"></a>Événement 1592 : CLUSSVC_TCP_RECONNECT_CONNECTION_REESTABLISHED

Le nœud de cluster' %1 'a perdu la communication avec le nœud de cluster' %2 '. La communication réseau a été rétablie. Cela peut être dû au fait que la communication est temporairement bloquée par un pare-feu ou une mise à jour de stratégie de sécurité de connexion. Si le problème persiste et que la communication réseau n’est pas rétablie, le service de cluster sur un ou plusieurs nœuds s’arrête. Si cela se produit, exécutez l’Assistant validation d’une configuration pour vérifier la configuration de votre réseau. En outre, recherchez les erreurs matérielles ou logicielles liées aux cartes réseau sur ce nœud, puis vérifiez les défaillances dans les autres composants réseau auxquels le nœud est connecté, par exemple des concentrateurs, des commutateurs ou des ponts.

### <a name="event-1594-cluster_functional_level_upgrade_complete"></a>Événement 1594 : CLUSTER_FUNCTIONAL_LEVEL_UPGRADE_COMPLETE

Service de cluster correctement mis à niveau le niveau fonctionnel du cluster. <br><br>
Niveau fonctionnel : %1 <br> Version de mise à niveau : %2

### <a name="event-1635-rcm_resource_failure_info_with_typename"></a>Événement 1635 : RCM_RESOURCE_FAILURE_INFO_WITH_TYPENAME

Échec de la ressource de cluster' %1 'de type' %3 'dans le rôle en cluster' %2 '.

### <a name="event-1636-clussvc_password_changed"></a>Événement 1636 : CLUSSVC_PASSWORD_CHANGED

Le service de cluster a modifié le mot de passe du compte « %1 » sur le nœud « %2 ».

### <a name="event-1680-cluster_node_exited_isolated_state"></a>Événement 1680 : CLUSTER_NODE_EXITED_ISOLATED_STATE

Le nœud de cluster' %1 'a quitté l’état isolé.

### <a name="event-1682-cluster_group_moved_no_longer_unmonitored"></a>Événement 1682 : CLUSTER_GROUP_MOVED_NO_LONGER_UNMONITORED

La machine virtuelle « %2 » qui n’a pas été analysée sur le nœud isolé « %1 » a été basculée vers un autre nœud. L’intégrité de l’ordinateur virtuel est de nouveau analysée.

### <a name="event-1682-cluster_multiple_groups_no_longer_unmonitored"></a>Événement 1682 : CLUSTER_MULTIPLE_GROUPS_NO_LONGER_UNMONITORED

Le nœud « %1 » a rejoint le cluster et les ordinateurs virtuels suivants en cours d’exécution sur ce nœud ont de nouveau l’état d’intégrité analysé : %2.

### <a name="event-4621-nodecleanup_success"></a>Événement 4621 : NODECLEANUP_SUCCESS

Ce nœud a été correctement supprimé du cluster.

### <a name="event-5121-dcm_volume_no_direct_io_due_to_failure"></a>Événement 5121 : DCM_VOLUME_NO_DIRECT_IO_DUE_TO_FAILURE

Volume partagé de cluster « %1 » (« %2 ») n’est plus directement accessible à partir de ce nœud de cluster. L’accès aux e/s sera redirigé vers le périphérique de stockage sur le réseau vers le nœud propriétaire du volume. Si cela entraîne une dégradation des performances, corrigez la connectivité de ce nœud au périphérique de stockage et les e/s reprendront à un état sain une fois la connectivité au périphérique de stockage rétablie.

### <a name="event-5218-csv_old_sw_snapshot_deleted"></a>Événement 5218 : CSV_OLD_SW_SNAPSHOT_DELETED

La ressource de disque physique de cluster « %1 » a supprimé un instantané logiciel. L’instantané logiciel sur le Volume partagé de cluster' %2 'a été supprimé, car il était antérieur à' %3 'jour (s). L’ID d’instantané était « %4 » et a été créé à partir du nœud « %5 » à « %6 ».
Il est supposé que les instantanés sont supprimés par une application de sauvegarde après la fin d’un travail de sauvegarde. Cet instantané a dépassé le temps attendu pour l’existence d’un instantané. Vérifiez auprès de l’application de sauvegarde que les travaux de sauvegarde se terminent correctement.

## <a name="see-also"></a>Voir aussi

-   [Informations détaillées sur les événements pour les composants de clustering de basculement dans Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753362(v%3dws.10))
