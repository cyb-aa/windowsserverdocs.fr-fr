---
title: Problèmes connus du service de migration du stockage
description: Problèmes connus et prise en charge de la résolution des problèmes pour Storage migration service, tels que la collecte des journaux pour Support Microsoft.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 02/10/2020
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.openlocfilehash: 92742929e3826fca3cf87cb84341d3aecec0d55d
ms.sourcegitcommit: 1c75e4b3f5895f9fa33efffd06822dca301d4835
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77517494"
---
# <a name="storage-migration-service-known-issues"></a>Problèmes connus du service de migration du stockage

Cette rubrique contient des réponses aux problèmes connus liés à l’utilisation de [Storage migration service](overview.md) pour migrer des serveurs.

Le service de migration de stockage est disponible en deux parties : le service dans Windows Server et l’interface utilisateur dans le centre d’administration Windows. Le service est disponible dans Windows Server, le canal de maintenance à long terme, ainsi que Windows Server, canal semi-annuel ; le centre d’administration Windows est disponible sous forme de téléchargement distinct. Nous incluons également périodiquement des modifications dans les mises à jour cumulatives pour Windows Server, publiées via Windows Update. 

Par exemple, Windows Server, version 1903 comprend de nouvelles fonctionnalités et des correctifs pour le service de migration de stockage, qui sont également disponibles pour Windows Server 2019 et Windows Server version 1809 en installant [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534).

## <a name="collecting-logs"></a>Comment collecter des fichiers journaux lors de l’utilisation de Support Microsoft

Le service de migration de stockage contient des journaux des événements pour le service Orchestrator et le service proxy. Le serveur Orchestrator contient toujours les journaux des événements, et les serveurs de destination sur lesquels le service proxy est installé contiennent les journaux du proxy. Ces journaux se trouvent sous :

- Journaux des applications et des services \ Microsoft \ Windows \ StorageMigrationService
- Journaux des applications et des services \ Microsoft \ Windows \ StorageMigrationService-proxy

Si vous devez rassembler ces journaux pour une consultation hors connexion ou pour les envoyer à Support Microsoft, un script PowerShell Open source est disponible sur GitHub :

 [Programme d’assistance de Storage migration service](https://aka.ms/smslogs) 

Consultez le fichier Lisez-moi pour plus d’utilisation.

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>Le service de migration de stockage n’apparaît pas dans le centre d’administration Windows, sauf si vous gérez Windows Server 2019

Lorsque vous utilisez la version 1809 du centre d’administration Windows pour gérer un Windows Server 2019 Orchestrator, vous ne voyez pas l’option outil pour le service de migration de stockage. 

L’extension du service de migration de stockage du centre d’administration Windows est liée à une version pour gérer uniquement les systèmes d’exploitation Windows Server 2019 version 1809 ou ultérieures. Si vous l’utilisez pour gérer des systèmes d’exploitation Windows Server plus anciens ou des versions préliminaires Insider, l’outil ne s’affiche pas. Ce comportement est lié à la conception. 

Pour résoudre, utiliser ou effectuer une mise à niveau vers Windows Server 2019 Build 1809 ou version ultérieure.

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>La validation du basculement du service de migration du stockage échoue avec l’erreur « Accès refusé pour la stratégie de filtre de jeton sur l’ordinateur de destination »

Lors de l’exécution de la validation du basculement, vous recevez l’erreur « échec : l’accès est refusé pour la stratégie de filtre de jeton sur l’ordinateur de destination ». Cela se produit même si vous avez fourni des informations d’identification d’administrateur local correctes pour les ordinateurs source et de destination.

Ce problème a été résolu dans la mise à jour [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) . 

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-or-windows-server-2019-essentials-edition"></a>Storage migration service n’est pas inclus dans Windows Server 2019 Evaluation ou Windows Server 2019 Essentials Edition

Lorsque vous utilisez le centre d’administration Windows pour vous connecter à une [version d’évaluation de Windows server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) ou à l’édition windows server 2019 Essentials, il n’est pas possible de gérer le service de migration de stockage. Le service de migration de stockage n’est pas non plus inclus dans les rôles et les fonctionnalités.

Ce problème est dû à un problème de maintenance dans le support d’évaluation de Windows Server 2019 et Windows Server 2019 Essentials. 

Pour contourner ce problème à des fins d’évaluation, installez une version commerciale, MSDN, OEM ou de licence en volume de Windows Server 2019 et ne l’activez pas. Sans activation, toutes les éditions de Windows Server fonctionnent en mode d’évaluation pendant 180 jours. 

Nous avons résolu ce problème dans une version ultérieure de Windows Server.  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>Le service de migration du stockage expire le téléchargement de l’erreur de transfert CSV

Lorsque vous utilisez le centre d’administration Windows ou PowerShell pour télécharger le journal CSV d’opérations de transfert (messages détaillés uniquement), vous recevez l’erreur suivante :

 >   Journal de transfert-Vérifiez que le partage de fichiers est autorisé dans votre pare-feu. : Cette opération de demande envoyée à net. TCP : biais : 28940/SMS/service/1/Transfer n’a pas reçu de réponse dans le délai d’expiration configuré (00:01:00). Le temps alloué à cette opération fait peut-être partie d'un délai d'attente plus long. Ceci peut être dû au fait que le service est toujours en cours de traitement de l'opération ou qu'il n'a pas pu envoyer un message de réponse. Envisagez d’incrémenter le délai d’expiration de l’opération (en convertissant le canal/proxy vers IContextChannel et en définissant la propriété OperationTimeout) et vérifiez que le service est en mesure de se connecter au client.

Ce problème est dû à un très grand nombre de fichiers transférés qui ne peuvent pas être filtrés dans le délai d’attente d’une minute par défaut autorisé par le service de migration de stockage. 

Pour contourner ce problème :

1. Sur l’ordinateur Orchestrator, modifiez le fichier *%systemroot%\SMS\Microsoft.StorageMigration.service.exe.config* à l’aide de Notepad. exe pour remplacer la valeur par défaut « sendTimeout » par 10 minutes.

   ```
     <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:01:00"
   ```

2. Redémarrez le service de migration de stockage sur l’ordinateur Orchestrator. 
3. Sur l’ordinateur Orchestrator, démarrez regedit. exe.
4. Localisez la sous-clé de Registre suivante, puis cliquez dessus : 

   `HKEY_LOCAL_MACHINE\Software\Microsoft\SMSPowershell`

5. Dans le menu Edition, pointez sur Nouveau, puis cliquez sur DWORD Value. 
6. Tapez « WcfOperationTimeoutInMinutes » pour le nom de la valeur DWORD, puis appuyez sur entrée.
7. Cliquez avec le bouton droit sur « WcfOperationTimeoutInMinutes », puis cliquez sur modifier. 
8. Dans la zone données de base, cliquez sur « décimal ».
9. Dans la zone données de la valeur, tapez « 10 », puis cliquez sur OK.
10. Quittez l’éditeur du Registre.
11. Essayez à nouveau de télécharger le fichier CSV d’erreurs uniquement. 

Nous avons l’intention de modifier ce comportement dans une version ultérieure de Windows Server 2019.  

## <a name="validation-warnings-for-destination-proxy-and-credential-administrative-privileges"></a>Avertissements de validation pour le proxy de destination et les privilèges d’administration des informations d’identification

Lors de la validation d’une tâche de transfert, les avertissements suivants s’affichent :

 > **Les informations d’identification ont des privilèges d’administrateur.**
 > AVERTISSEMENT : l’action n’est pas disponible à distance.
 > **Le proxy de destination est inscrit.**
 > AVERTISSEMENT : le proxy de destination est introuvable.

Si vous n’avez pas installé le service proxy de service de migration de stockage sur l’ordinateur de destination Windows Server 2019, ou si l’ordinateur de destination est Windows Server 2016 ou Windows Server 2012 R2, ce comportement est normal. Nous vous recommandons de migrer vers un ordinateur Windows Server 2019 avec le proxy installé pour améliorer considérablement les performances de transfert.  

## <a name="certain-files-do-not-inventory-or-transfer-error-5-access-is-denied"></a>Certains fichiers ne sont pas inventaires ou transférés, erreur 5 « accès refusé »

Lors de l’inventaire ou du transfert de fichiers d’un ordinateur source vers un ordinateur de destination, la migration des fichiers à partir desquels un utilisateur a supprimé les autorisations du groupe administrateurs échoue. Examen du service de migration de stockage-débogage du proxy :

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          2/26/2019 9:00:04 AM
    Event ID:      10000
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      srv1.contoso.com
    Description:

    02/26/2019-09:00:04.860 [Error] Transfer error for \\srv1.contoso.com\public\indy.png: (5) Access is denied.
    Stack Trace:
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.OpenFile(String fileName, DesiredAccess desiredAccess, ShareMode shareMode, CreationDisposition creationDisposition, FlagsAndAttributes flagsAndAttributes)
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile(String path)
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile(FileInfo file)
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.InitializeSourceFileInfo()
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.Transfer()
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.TryTransfer()   

Ce problème est dû à une erreur de code dans le service de migration de stockage où le privilège de sauvegarde n’a pas été appelé. 

Pour résoudre ce problème, installez [Windows Update le 2 avril 2019 — KB4490481 (version 17763,404 du système d’exploitation)](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481) sur l’ordinateur Orchestrator et l’ordinateur de destination si le service proxy y est installé. Assurez-vous que le compte d’utilisateur de migration source est un administrateur local sur l’ordinateur source et le service de migration de stockage Orchestrator. Assurez-vous que le compte d’utilisateur de migration de destination est un administrateur local sur l’ordinateur de destination et le service de migration de stockage Orchestrator. 

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>Incompatibilité de hachages DFSR lors de l’utilisation du service de migration de stockage pour préamorcer des données

Lorsque vous utilisez le service de migration de stockage pour transférer des fichiers vers une nouvelle destination, en configurant le réplication DFS (DFSR) pour répliquer ces données avec un serveur DFSR existant via la réplication prédéfinie ou le clonage de base de données DFSR, tous les fichiers rencontrent un hachage discordance et sont à nouveau répliquées. Les flux de données, les flux de sécurité, les tailles et les attributs semblent être parfaitement mis en correspondance après l’utilisation de SMS pour les transférer. L’examen des fichiers avec ICACLS ou le journal de débogage de clonage de base de données DFSR révèle :

Fichier source :

  icacls d:\test\Source :

  icacls d:\test\thatcher.png/Save out. txt/t Thatcher. png D :AI (A ;; FA ;;; BA) (A ;; 0 x1200a9 ;;;D D) (A ;; 0 x1301bf ;;;D U) (A ; ID ; FA ;;; BA) (A ; ID ; FA ;;; SY) (A ; ID ; ACE 0x1200a9 ;;; BU

Fichier de destination :

  icacls d:\test\thatcher.png/Save out. txt/t Thatcher. png D :AI (A ;; FA ;;; BA) (A ;; 0 x1301bf ;;;D U) (A ;; 0 x1200a9 ;;;D D) (A ; ID ; FA ;;; BA) (A ; ID ; FA ;;; SY) (A ; ID ; ACE 0x1200a9 ;;; BU)**S : PAINO_ACCESS_CONTROL**

Journal de débogage DFSR :

    20190308 10:18:53.116 3948 DBCL  4045 [WARN] DBClone::IDTableImportUpdate Mismatch record was found. 

    Local ACL hash:1BCDFE03-A18BCE01-D1AE9859-23A0A5F6 
    LastWriteTime:20190308 18:09:44.876 
    FileSizeLow:1131654 
    FileSizeHigh:0 
    Attributes:32 

    Clone ACL hash:**DDC4FCE4-DDF329C4-977CED6D-F4D72A5B** 
    LastWriteTime:20190308 18:09:44.876 
    FileSizeLow:1131654 
    FileSizeHigh:0 
    Attributes:32 

Ce problème est résolu par la mise à jour [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transferring-from-windows-server-2008-r2"></a>Erreur « Impossible de transférer le stockage sur l’un des points de terminaison » lors du transfert à partir de Windows Server 2008 R2

Lorsque vous tentez de transférer des données à partir d’un ordinateur source Windows Server 2008 R2, aucun transfert de données ne s’affiche et vous recevez l’erreur suivante :  

    Couldn't transfer storage on any of the endpoints.
    0x9044

Cette erreur est attendue si votre ordinateur Windows Server 2008 R2 n’est pas entièrement corrigé avec toutes les mises à jour critiques et importantes de Windows Update. Quel que soit le service de migration de stockage, nous vous recommandons de toujours mettre à jour un ordinateur Windows Server 2008 R2 à des fins de sécurité, car ce système d’exploitation ne contient pas les améliorations de sécurité des versions plus récentes de Windows Server.

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-and-check-if-the-source-device-is-online---we-couldnt-access-it"></a>Erreur « Impossible de transférer le stockage sur l’un des points de terminaison » et « vérifiez si l’appareil source est en ligne, nous n’avons pas pu y accéder ».

Lorsque vous tentez de transférer des données à partir d’un ordinateur source, certains ou tous les partages ne sont pas transférés, avec une erreur de Résumé :

    Couldn't transfer storage on any of the endpoints.
    0x9044

L’examen des détails du transfert SMB affiche l’erreur suivante :

    Check if the source device is online - we couldn't access it.

L’examen du journal des événements StorageMigrationService/admin affiche :

    Couldn't transfer storage.

    Job: Job1
    ID:  
    State: Failed
    Error: 36931
    Error Message: 

   Guide : Vérifiez l’erreur détaillée et assurez-vous que les conditions de transfert sont remplies. La tâche de transfert n’a pas pu transférer les ordinateurs source et de destination. Cela peut être dû au fait que l’ordinateur Orchestrator n’a pu atteindre aucun ordinateur source ou de destination, peut-être en raison d’une règle de pare-feu ou d’autorisations manquantes.

L’examen du journal StorageMigrationService-proxy/Debug affiche :

    07/02/2019-13:35:57.231 [Error] Transfer validation failed. ErrorCode: 40961, Source endpoint is not reachable, or doesn't exist, or source credentials are invalid, or authenticated user doesn't have sufficient permissions to access it.
    at Microsoft.StorageMigration.Proxy.Service.Transfer.TransferOperation.Validate()
    at Microsoft.StorageMigration.Proxy.Service.Transfer.TransferRequestHandler.ProcessRequest(FileTransferRequest fileTransferRequest, Guid operationId)    

Il s’agit d’une erreur de code qui se manifesterait si votre compte de migration n’a pas au moins des autorisations de lecture sur les partages SMB. Ce problème a été corrigé pour la première fois dans la mise à jour cumulative [4520062](https://support.microsoft.com/help/4520062/windows-10-update-kb4520062). 

## <a name="error-0x80005000-when-running-inventory"></a>Erreur 0x80005000 lors de l’exécution de l’inventaire

Après l’installation de [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) et la tentative d’exécution de l’inventaire, l’inventaire échoue avec des erreurs :

    EXCEPTION FROM HRESULT: 0x80005000
  
    Log Name:      Microsoft-Windows-StorageMigrationService/Admin
    Source:        Microsoft-Windows-StorageMigrationService
    Date:          9/9/2019 5:21:42 PM
    Event ID:      2503
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      FS02.TailwindTraders.net
    Description:
    Couldn't inventory the computers.
    Job: foo2
    ID: 20ac3f75-4945-41d1-9a79-d11dbb57798b
    State: Failed
    Error: 36934
    Error Message: Inventory failed for all devices
    Guidance: Check the detailed error and make sure the inventory requirements are met. The job couldn't inventory any of the specified source computers. This could be because the orchestrator computer couldn't reach it over the network, possibly due to a firewall rule or missing permissions.
  
    Log Name:      Microsoft-Windows-StorageMigrationService/Admin
    Source:        Microsoft-Windows-StorageMigrationService
    Date:          9/9/2019 5:21:42 PM
    Event ID:      2509
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      FS02.TailwindTraders.net
    Description:
    Couldn't inventory a computer.
    Job: foo2
    Computer: FS01.TailwindTraders.net
    State: Failed
    Error: -2147463168
    Error Message: 
    Guidance: Check the detailed error and make sure the inventory requirements are met. The inventory couldn't determine any aspects of the specified source computer. This could be because of missing permissions or privileges on the source or a blocked firewall port.
  
    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          2/14/2020 1:18:21 PM
    Event ID:      10000
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      2019-rtm-orc.ned.contoso.com
    Description:
    02/14/2020-13:18:21.097 [Erro] Failed device discovery stage SystemInfo with error: (0x80005000) Unknown error (0x80005000)   
  
Cette erreur est due à un défaut de code dans le service de migration de stockage lorsque vous fournissez des informations d’identification de migration sous la forme d’un nom d’utilisateur principal (UPN), tel que «meghan@contoso.com». Le service d’analyse du service de migration du stockage ne parvient pas à analyser ce format correctement, ce qui provoque un échec dans une recherche de domaine qui a été ajoutée pour la prise en charge de la migration de cluster dans KB4512534 et 19H1.

Pour contourner ce problème, fournissez les informations d’identification au format domaine\utilisateur, par exemple « Contoso\Meghan ».

## <a name="error-serviceerror0x9006-or-the-proxy-isnt-currently-available-when-migrating-to-a-windows-server-failover-cluster"></a>Erreur « ServiceError0x9006 » ou « le proxy n’est pas disponible actuellement ». lors de la migration vers un cluster de basculement Windows Server

Lorsque vous tentez de transférer des données sur un serveur de fichiers en cluster, vous recevez des erreurs telles que : 

    Make sure the proxy service is installed and running, and then try again. The proxy isn't currently available.
    0x9006
    ServiceError0x9006,Microsoft.StorageMigration.Commands.UnregisterSmsProxyCommand

Cette erreur est attendue si la ressource de serveur de fichiers a été déplacée de son nœud propriétaire de cluster Windows Server 2019 d’origine vers un nouveau nœud et que la fonctionnalité de proxy de service de migration de stockage n’a pas été installée sur ce nœud.

Pour résoudre ce problème, replacez la ressource du serveur de fichiers de destination sur le nœud de cluster propriétaire d’origine qui était utilisé lorsque vous avez configuré pour la première fois les jumelages de transfert.

Une autre solution de contournement consiste à :

1. Installez la fonctionnalité de proxy du service de migration de stockage sur tous les nœuds d’un cluster.
2. Exécutez la commande PowerShell de service de migration de stockage suivante sur l’ordinateur Orchestrator : 

   ```PowerShell
   Register-SMSProxy -ComputerName *destination server* -Force
   ```
## <a name="error-dll-was-not-found-when-running-inventory-from-a-cluster-node"></a>Erreur « dll introuvable » lors de l’exécution de l’inventaire à partir d’un nœud de cluster

Lorsque vous tentez d’exécuter un inventaire avec le service de migration de stockage et que vous ciblez une source de serveur de fichiers d’utilisation générale du cluster de basculement Windows Server, vous recevez les erreurs suivantes :

    DLL not found
    [Error] Failed device discovery stage VolumeInfo with error: (0x80131524) Unable to load DLL 'Microsoft.FailoverClusters.FrameworkSupport.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)   

Pour contourner ce problème, installez les outils d’administration de cluster de basculement (RSAT-clustering-Mgmt) sur le serveur qui exécute le service de migration de stockage Orchestrator. 

## <a name="error-there-are-no-more-endpoints-available-from-the-endpoint-mapper-when-running-inventory-against-a-windows-server-2003-source-computer"></a>Erreur « aucun point de terminaison n’est disponible à partir du mappeur de point de terminaison » lors de l’inventaire sur un ordinateur source Windows Server 2003

Lorsque vous tentez d’exécuter un inventaire avec le serveur Orchestrator de migration de stockage corrigé avec la mise à jour cumulative [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) ou une version ultérieure, vous recevez l’erreur suivante :

    There are no more endpoints available from the endpoint mapper  

Pour contourner ce problème, désinstallez temporairement la mise à jour cumulative KB4512534 (et toute autre priorité) à partir de l’ordinateur d’Orchestrator du service de migration du stockage. Une fois la migration terminée, réinstallez la dernière mise à jour cumulative.  

Notez que, dans certaines circonstances, la désinstallation de KB4512534 ou de ses mises à jour de remplacement peut empêcher le démarrage du service de migration de stockage. Pour résoudre ce problème, vous pouvez sauvegarder et supprimer la base de données du service de migration de stockage :

1.  Ouvrez une invite de commandes avec élévation de privilèges, dans laquelle vous êtes membre des administrateurs sur le serveur du service de migration de stockage, puis exécutez la commande suivante :

     ```
     TAKEOWN /d y /a /r /f c:\ProgramData\Microsoft\StorageMigrationService
     
     MD c:\ProgramData\Microsoft\StorageMigrationService\backup

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService\* /grant Administrators:(GA)

     XCOPY c:\ProgramData\Microsoft\StorageMigrationService\* .\backup\*

     DEL c:\ProgramData\Microsoft\StorageMigrationService\* /q

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService  /GRANT networkservice:F /T /C

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService /GRANT networkservice:(GA) /T /C
     ```
   
2.  Démarrez le service de migration du stockage, qui créera une nouvelle base de données.

## <a name="error-clusctl_resource_netname_repair_vco-failed-against-netname-resource-and-windows-server-2008-r2-cluster-cutover-fails"></a>Erreur « CLUSCTL_RESOURCE_NETNAME_REPAIR_VCO échec sur la ressource de nom de serveur » et le basculement de cluster Windows Server 2008 R2 échoue

Lorsque vous tentez d’exécuter la fenêtre couper sur une source de cluster Windows Server 2008 R2, la coupure est bloquée lors de la phase « modification du nom de l’ordinateur source... » et vous recevez l’erreur suivante :

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          10/17/2019 6:44:48 PM
    Event ID:      10000
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      WIN-RNS0D0PMPJH.contoso.com
    Description:
    10/17/2019-18:44:48.727 [Erro] Exception error: 0x1. Message: Control code CLUSCTL_RESOURCE_NETNAME_REPAIR_VCO failed against netName resource 2008r2FS., stackTrace:    at Microsoft.FailoverClusters.Framework.ClusterUtils.NetnameRepairVCO(SafeClusterResourceHandle netNameResourceHandle, String netName)
       at Microsoft.FailoverClusters.Framework.ClusterUtils.RenameFSNetName(SafeClusterHandle ClusterHandle, String clusterName, String FsResourceId, String NetNameResourceId, String newDnsName, CancellationToken ct)
       at Microsoft.StorageMigration.Proxy.Cutover.CutoverUtils.RenameFSNetName(NetworkCredential networkCredential, Boolean isLocal, String clusterName, String fsResourceId, String nnResourceId, String newDnsName, CancellationToken ct)    [d:\os\src\base\dms\proxy\cutover\cutoverproxy\CutoverUtils.cs::RenameFSNetName::1510]

Ce problème est dû à l’absence d’API dans les versions antérieures de Windows Server. Actuellement, il n’existe aucun moyen de migrer les clusters Windows Server 2008 et Windows Server 2003. Vous pouvez effectuer un inventaire et un transfert sans problème sur les clusters Windows Server 2008 R2, puis effectuer manuellement le basculement en modifiant manuellement l’adresse IP et le nom d’accès de la ressource du serveur de fichiers source du cluster, puis en modifiant le nom et l’adresse IP du cluster de destination. adresse qui correspond à la source d’origine. 

## <a name="cutover-hangs-on-38-mapping-network-interfaces-on-the-source-computer"></a>Le basculement se bloque sur « 38% mappage des interfaces réseau sur l’ordinateur source... » 

Lorsque vous tentez d’exécuter la fonction couper au-dessus d’un ordinateur source, si vous avez défini l’ordinateur source pour qu’il utilise une nouvelle adresse IP statique (non DHCP) sur une ou plusieurs interfaces réseau, la coupure est bloquée à la phase « 38% mappage des interfaces réseau sur le comnputer source... » et vous recevez l’erreur suivante dans le journal des événements SMS :

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Admin
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          11/13/2019 3:47:06 PM
    Event ID:      20494
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      orc2019-rtm.corp.contoso.com
    Description:
    Couldn't set the IP address on the network adapter.

    Computer: fs12.corp.contoso.com
    Adapter: microsoft hyper-v network adapter
    IP address: 10.0.0.99
    Network mask: 16
    Error: 40970
    Error Message: Unknown error (0xa00a)

    Guidance: Confirm that the Netlogon service on the computer is reachable through RPC and that the credentials provided are correct.

L’examen de l’ordinateur source montre que l’adresse IP d’origine ne peut pas changer. 

Ce problème ne se produit pas si vous avez sélectionné l’option « utiliser DHCP » dans l’écran du centre d’administration Windows « configurer le basculement », uniquement si vous spécifiez une nouvelle adresse IP statique, un sous-réseau et une passerelle. 

Ce problème est dû à une régression dans la mise à jour [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) . Il existe actuellement deux solutions de contournement pour ce problème :

  - Avant le basculement : au lieu de définir une nouvelle adresse IP statique lors du basculement, sélectionnez « utiliser DHCP » et assurez-vous qu’une étendue DHCP couvre ce sous-réseau. SMS configure l’ordinateur source pour qu’il utilise DHCP sur les interfaces de l’ordinateur source et le découpage se poursuivra normalement. 
  
  - Si le basculement est déjà bloqué : Connectez-vous à l’ordinateur source et activez DHCP sur ses interfaces réseau, après avoir vérifié qu’une étendue DHCP couvre ce sous-réseau. Lorsque l’ordinateur source acquiert une adresse IP fournie par DHCP, SMS poursuit la découpe normalement.
  
Dans les deux solutions de contournement, une fois la coupure terminée, vous pouvez définir une adresse IP statique sur l’ancien ordinateur source comme vous le voyez et l’arrêter à l’aide de DHCP.   

## <a name="slower-than-expected-re-transfer-performance"></a>Ralentissement des performances de retransfert ATTENDU

À l’issue d’un transfert, lors de l’exécution d’un nouveau transfert des mêmes données, vous risquez de ne pas voir une grande amélioration du temps de transfert, même si les données ont été modifiées pendant l’intervalle de temps sur le serveur source.

Il s’agit du comportement attendu lors du transfert d’un très grand nombre de fichiers et de dossiers imbriqués. La taille des données n’est pas pertinente. Nous avons d’abord apporté des améliorations à ce comportement dans [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) et nous continuons à optimiser les performances de transfert. Pour affiner les performances, passez en revue [optimisation des performances d’inventaire et de transfert](https://docs.microsoft.com/windows-server/storage/storage-migration-service/faq#optimizing-inventory-and-transfer-performance).

## <a name="data-does-not-transfer-user-renamed-when-migrating-to-or-from-a-domain-controller"></a>Les données ne sont pas transférées, l’utilisateur a été renommé lors de la migration vers ou à partir d’un contrôleur de domaine

Après le démarrage du transfert à partir de ou vers un contrôleur de domaine :

 1. Aucune donnée n’est migrée et aucun partage n’est créé sur la destination.
 2. Un symbole d’erreur rouge s’affiche dans le centre d’administration Windows sans message d’erreur
 3. Un ou plusieurs utilisateurs AD et groupes locaux de domaine ont leur nom et/ou l’attribut de connexion antérieur à Windows 2000 modifié
 4. Vous voyez l’événement 3509 sur SMS Orchestrator :
 
        Log Name:      Microsoft-Windows-StorageMigrationService/Admin
        Source:        Microsoft-Windows-StorageMigrationService
        Date:          1/10/2020 2:53:48 PM
        Event ID:      3509
        Task Category: None
        Level:         Error
        Keywords:      
        User:          NETWORK SERVICE
        Computer:      orc2019-rtm.corp.contoso.com
        Description:
        Couldn't transfer storage for a computer.

        Job: dctest3
        Computer: dc02-2019.corp.contoso.com
        Destination Computer: dc03-2019.corp.contoso.com
        State: Failed
        Error: 53251
        Error Message: Local accounts migration failed with error System.Exception: -2147467259
           at Microsoft.StorageMigration.Service.DeviceHelper.MigrateSecurity(IDeviceRecord sourceDeviceRecord, IDeviceRecord destinationDeviceRecord, TransferConfiguration config, Guid proxyId, CancellationToken cancelToken)

Il s’agit du comportement attendu si vous tentez de migrer à partir de ou vers un contrôleur de domaine avec le service de migration de stockage et d’utiliser l’option « migrer les utilisateurs et les groupes » pour renommer ou réutiliser des comptes. au lieu de sélectionner « ne pas transférer les utilisateurs et les groupes ». La migration du contrôleur [de stockage n’est pas prise en charge avec Storage migration service](faq.md). Étant donné qu’un contrôleur de service ne possède pas de véritables utilisateurs et groupes locaux, le service de migration de stockage traite ces principaux de sécurité comme c’est le cas lors de la migration entre deux serveurs membres et tente d’ajuster les ACL comme indiqué, ce qui aboutit aux erreurs et aux comptes déformés ou copiés. 

Si vous avez déjà exécuté le transfert un ou plusieurs fois :

 1. Utilisez la commande AD PowerShell suivante sur un contrôleur de domaine pour localiser les utilisateurs ou les groupes modifiés (en modifiant SearchBase pour qu’il corresponde à votre nom de dinstringuished de domaine) : 

    ```PowerShell
    Get-ADObject -Filter 'Description -like "*storage migration service renamed*"' -SearchBase 'DC=<domain>,DC=<TLD>' | ft name,distinguishedname
    ```
   
 2. Pour tous les utilisateurs retournés avec leur nom d’origine, modifiez leur « nom d’ouverture de session utilisateur (antérieur à Windows 2000) » pour supprimer le suffixe de caractère aléatoire ajouté par Storage migration service, afin que ce perdant puisse ouvrir une session.
 3. Pour tous les groupes retournés avec leur nom d’origine, modifiez leur nom de groupe (antérieur à Windows 2000) pour supprimer le suffixe de caractère aléatoire ajouté par Storage migration service.
 4. Pour tous les utilisateurs ou groupes désactivés dont le nom contient désormais un suffixe ajouté par Storage migration service, vous pouvez supprimer ces comptes. Vous pouvez vérifier que les comptes d’utilisateur ont été ajoutés ultérieurement, car ils ne contiendront que le groupe utilisateurs du domaine et auront une date/heure de création correspondant à l’heure de début du transfert du service de migration du stockage.
 
 Si vous souhaitez utiliser le service de migration de stockage avec les contrôleurs de domaine à des fins de transfert, veillez à toujours sélectionner « ne pas transférer les utilisateurs et les groupes » dans la page Paramètres de transfert du centre d’administration Windows.
 
 ## <a name="error-53-failed-to-inventory-all-specified-devices-when-running-inventory"></a>Erreur 53 « échec de l’inventaire de tous les périphériques spécifiés » lors de l’inventaire, 

Lorsque vous tentez d’exécuter un inventaire, vous recevez :

    Failed to inventory all specified devices 
    
    Log Name:      Microsoft-Windows-StorageMigrationService/Admin
    Source:        Microsoft-Windows-StorageMigrationService
    Date:          1/16/2020 8:31:17 AM
    Event ID:      2516
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      ned.corp.contoso.com
    Description:
    Couldn't inventory files on the specified endpoint.
    Job: ned1
    Computer: ned.corp.contoso.com
    Endpoint: hithere
    State: Failed
    File Count: 0
    File Size in KB: 0
    Error: 53
    Error Message: Endpoint scan failed
    Guidance: Check the detailed error and make sure the inventory requirements are met. This could be because of missing permissions on the source computer.

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          1/16/2020 8:31:17 AM
    Event ID:      10004
    Task Category: None
    Level:         Critical
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      ned.corp.contoso.com
    Description:
    01/16/2020-08:31:17.031 [Crit] Consumer Task failed with error:The network path was not found.
    . StackTrace=   at Microsoft.Win32.RegistryKey.Win32ErrorStatic(Int32 errorCode, String str)
       at Microsoft.Win32.RegistryKey.OpenRemoteBaseKey(RegistryHive hKey, String machineName, RegistryView view)
       at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetEnvironmentPathFolders(String ServerName, Boolean IsServerLocal)
       at Microsoft.StorageMigration.Proxy.Service.Discovery.ScanUtils.<ScanSMBEndpoint>d__3.MoveNext()
       at Microsoft.StorageMigration.Proxy.EndpointScanOperation.Run()
       at Microsoft.StorageMigration.Proxy.Service.Discovery.EndpointScanRequestHandler.ProcessRequest(EndpointScanRequest scanRequest, Guid operationId)
       at Microsoft.StorageMigration.Proxy.Service.Discovery.EndpointScanRequestHandler.ProcessRequest(Object request)
       at Microsoft.StorageMigration.Proxy.Common.ProducerConsumerManager`3.Consume(CancellationToken token)    
       
    01/16/2020-08:31:10.015 [Erro] Endpoint Scan failed. Error: (53) The network path was not found.
    Stack trace:
       at Microsoft.Win32.RegistryKey.Win32ErrorStatic(Int32 errorCode, String str)
       at Microsoft.Win32.RegistryKey.OpenRemoteBaseKey(RegistryHive hKey, String machineName, RegistryView view)

À ce niveau, Storage migration service Orchestrator tente des lectures du Registre à distance pour déterminer la configuration de l’ordinateur source, mais est rejetée par le serveur source, indiquant que le chemin d’accès au registre n’existe pas. Les raisons de ce problème peuvent être les suivantes :

 - Le service d’accès à distance au registre n’est pas en cours d’exécution sur l’ordinateur source.
 - le pare-feu n’autorise pas les connexions à distance au serveur source à partir de l’orchestrateur.
 - Le compte de migration source ne dispose pas des autorisations de Registre à distance pour se connecter à l’ordinateur source.
 - Le compte de migration source ne dispose pas des autorisations de lecture dans le registre de l’ordinateur source, sous « HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows NT\CurrentVersion » ou sous «HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\ LanManServer

## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble de Storage migration service](overview.md)
