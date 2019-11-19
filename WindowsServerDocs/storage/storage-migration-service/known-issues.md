---
title: Problèmes connus du service de migration du stockage
description: Problèmes connus et prise en charge de la résolution des problèmes pour Storage migration service, tels que la collecte des journaux pour Support Microsoft.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 10/09/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.openlocfilehash: e20913b1245ce7e453b87e9b88a7a418a5c71de2
ms.sourcegitcommit: b60fdd2efa57ff23834a324b75de8fe245a7631f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74166171"
---
# <a name="storage-migration-service-known-issues"></a>Problèmes connus du service de migration du stockage

Cette rubrique contient des réponses aux problèmes connus liés à l’utilisation de [Storage migration service](overview.md) pour migrer des serveurs.

Le service de migration de stockage est disponible en deux parties : le service dans Windows Server et l’interface utilisateur dans le centre d’administration Windows. Le service est disponible dans Windows Server, le canal de maintenance à long terme, ainsi que Windows Server, canal semi-annuel ; le centre d’administration Windows est disponible sous forme de téléchargement distinct. Nous incluons également périodiquement des modifications dans les mises à jour cumulatives pour Windows Server, publiées via Windows Update. 

Par exemple, Windows Server, version 1903 comprend de nouvelles fonctionnalités et des correctifs pour le service de migration de stockage, qui sont également disponibles pour Windows Server 2019 et Windows Server version 1809 en installant [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534).

## <a name="collecting-logs"></a>Comment collecter des fichiers journaux lors de l’utilisation de Support Microsoft

Le service de migration de stockage contient des journaux des événements pour le service Orchestrator et le service proxy. Le serveur urchestrator contient toujours les journaux des événements, et les serveurs de destination sur lesquels le service proxy est installé contiennent les journaux du proxy. Ces journaux se trouvent sous :

- Journaux des applications et des services \ Microsoft \ Windows \ StorageMigrationService
- Journaux des applications et des services \ Microsoft \ Windows \ StorageMigrationService-proxy

Si vous devez rassembler ces journaux pour une consultation hors connexion ou pour les envoyer à Support Microsoft, un script PowerShell Open source est disponible sur GitHub :

 [Programme d’assistance de Storage migration service](https://aka.ms/smslogs) 

Consultez le fichier Lisez-moi pour plus d’utilisation.

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>Le service de migration de stockage n’apparaît pas dans le centre d’administration Windows, sauf si vous gérez Windows Server 2019

Lorsque vous utilisez la version 1809 du centre d’administration Windows pour gérer un Windows Server 2019 Orchestrator, vous ne voyez pas l’option outil pour le service de migration de stockage. 

L’extension du service de migration de stockage du centre d’administration Windows est liée à une version pour gérer uniquement les systèmes d’exploitation Windows Server 2019 version 1809 ou ultérieures. Si vous l’utilisez pour gérer des systèmes d’exploitation Windows Server plus anciens ou des versions préliminaires Insider, l’outil ne s’affiche pas. Ce comportement est normal. 

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

 >   Journal de transfert-Vérifiez que le partage de fichiers est autorisé dans votre pare-feu. : Cette opération de demande envoyée à net. TCP : biais : 28940/SMS/service/1/Transfer n’a pas reçu de réponse dans le délai d’expiration configuré (00:01:00). Le temps alloué à cette opération est peut-être une partie d’un délai d’expiration plus long. Cela peut être dû au fait que le service traite toujours l’opération ou parce que le service n’a pas pu envoyer un message de réponse. Envisagez d’incrémenter le délai d’expiration de l’opération (en convertissant le canal/proxy vers IContextChannel et en définissant la propriété OperationTimeout) et vérifiez que le service est en mesure de se connecter au client.

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

  Nom du journal : Microsoft-Windows-StorageMigrationService-proxy/débogage source : Microsoft-Windows-StorageMigrationService-proxy Date : 2/26/2019 9:00:04 AM ID d’événement : 10000 tâche catégorie : aucun niveau : Mots clés d’erreur :      
  Utilisateur : ordinateur de SERVICE réseau : srv1.contoso.com Description :

  02/26/2019-09:00:04.860 [erreur] erreur de transfert pour \\SRV1. contoso. com\public\indy.png : (5) l’accès est refusé.
Trace de la pile : au niveau de Microsoft. StorageMigration. proxy. service. Transfer. FileDirUtils. OpenFile (String fileName, DesiredAccess desiredAccess, ShareMode shareMode, CreationDisposition creationDisposition, FlagsAndAttributes flagsAndAttributes) à l’adresse Microsoft. StorageMigration. proxy. service. Transfer. FileDirUtils. GetTargetFile (chemin d’accès de chaîne) au niveau de Microsoft. StorageMigration. proxy. service. Transfer. FileDirUtils. GetTargetFile (fichier FileInfo) à l’adresse Microsoft. StorageMigration. proxy. service. Transfer. FileTransfer. InitializeSourceFileInfo () à Microsoft. StorageMigration. proxy. service. Transfer. FileTransfer. Transfer () à l’adresse Microsoft. StorageMigration. proxy. service. Transfer. FileTransfer. TryTransfer () [d:\os\src\base\dms\proxy\transfer\transferproxy\FileTransfer.cs :: TryTransfer :: 55]


Ce problème est dû à une erreur de code dans le service de migration de stockage où le privilège de sauvegarde n’a pas été appelé. 

Pour résoudre ce problème, installez [Windows Update le 2 avril 2019 — KB4490481 (version 17763,404 du système d’exploitation)](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481) sur l’ordinateur Orchestrator et l’ordinateur de destination si le service proxy y est installé. Assurez-vous que le compte d’utilisateur de migration source est un administrateur local sur l’ordinateur source et le service de migration de stockage Orchestrator. Assurez-vous que le compte d’utilisateur de migration de destination est un administrateur local sur l’ordinateur de destination et le service de migration de stockage Orchestrator. 

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>Incompatibilité de hachages DFSR lors de l’utilisation du service de migration de stockage pour préamorcer des données

Lorsque vous utilisez le service de migration de stockage pour transférer des fichiers vers une nouvelle destination, en configurant le réplication DFS (DFSR) pour répliquer ces données avec un serveur DFSR existant via la réplication prédéfinie ou le clonage de base de données DFSR, tous les fichiers experiemce un hachage discordance et sont à nouveau répliquées. Les flux de données, les flux de sécurité, les tailles et les attributs semblent être parfaitement mis en correspondance après l’utilisation de SMS pour les transférer. L’examen des fichiers avec ICACLS ou le journal de débogage de clonage de base de données DFSR révèle :

Fichier source :

  icacls d:\test\Source :

  icacls d:\test\thatcher.png/Save out. txt/t Thatcher. png D :AI (A ;; FA ;;; BA) (A ;; 0 x1200a9 ;;;D D) (A ;; 0 x1301bf ;;;D U) (A ; ID ; FA ;;; BA) (A ; ID ; FA ;;; SY) (A ; ID ; ACE 0x1200a9 ;;; BU

Fichier de destination :

  icacls d:\test\thatcher.png/Save out. txt/t Thatcher. png D :AI (A ;; FA ;;; BA) (A ;; 0 x1301bf ;;;D U) (A ;; 0 x1200a9 ;;;D D) (A ; ID ; FA ;;; BA) (A ; ID ; FA ;;; SY) (A ; ID ; ACE 0x1200a9 ;;; BU)**S : PAINO_ACCESS_CONTROL**

Journal de débogage DFSR :

  20190308 10:18:53.116 3948 DBCL 4045 [WARN] DBClone :: IDTableImportUpdate enregistrement incompatibilité a été trouvé. 

  Hachage de la liste de contrôle d’accès local : 1BCDFE03-A18BCE01-D1AE9859-23A0A5F6 LastWriteTime : 20190308 18:09:44.876 FileSizeLow : 1131654 FileSizeHigh : 0 Attributs : 32 

  Clone ACL Hash :**DDC4FCE4-DDF329C4-977CED6D-F4D72A5B** LastWriteTime : 20190308 18:09:44.876 FileSizeLow : 1131654 FileSizeHigh : 0 Attributs : 32 

Ce problème est résolu par la mise à jour [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transferring-from-windows-server-2008-r2"></a>Erreur « Impossible de transférer le stockage sur l’un des points de terminaison » lors du transfert à partir de Windows Server 2008 R2

Lorsque vous tentez de transférer des données à partir d’un ordinateur source Windows Server 2008 R2, aucun transfert de données ne s’affiche et vous recevez l’erreur suivante :  

  Impossible de transférer le stockage sur l’un des points de terminaison.
0x9044

Cette erreur est attendue si votre ordinateur Windows Server 2008 R2 n’est pas entièrement corrigé avec toutes les mises à jour critiques et importantes de Windows Update. Quel que soit le service de migration de stockage, nous vous recommandons de toujours mettre à jour un ordinateur Windows Server 2008 R2 à des fins de sécurité, car ce système d’exploitation ne contient pas les améliorations de sécurité des versions plus récentes de Windows Server.

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-and-check-if-the-source-device-is-online---we-couldnt-access-it"></a>Erreur « Impossible de transférer le stockage sur l’un des points de terminaison » et « vérifiez si l’appareil source est en ligne, nous n’avons pas pu y accéder ».

Lorsque vous tentez de transférer des données à partir d’un ordinateur source, certains ou tous les partages ne sont pas transférés, avec une erreur de Résumé :

   Impossible de transférer le stockage sur l’un des points de terminaison.
0x9044

L’examen des détails du transfert SMB affiche l’erreur suivante :

   Vérifiez si l’appareil source est en ligne, nous n’avons pas pu y accéder.

L’examen du journal des événements StorageMigrationService/admin affiche :

   Impossible de transférer le stockage.

   Tâche : ID Job1 :  
   État : échec de l’erreur : 36931 message d’erreur : 

   Guide : Vérifiez l’erreur détaillée et assurez-vous que les conditions de transfert sont remplies. La tâche de transfert n’a pas pu transférer les ordinateurs source et de destination. Cela peut être dû au fait que l’ordinateur Orchestrator n’a pu atteindre aucun ordinateur source ou de destination, peut-être en raison d’une règle de pare-feu ou d’autorisations manquantes.

L’examen du journal StorageMigrationService-proxy/Debug affiche :

   07/02/2019-13:35:57.231 [erreur] échec de la validation du transfert. ErrorCode : 40961, le point de terminaison source n’est pas accessible ou n’existe pas, les informations d’identification source ne sont pas valides, ou l’utilisateur authentifié ne dispose pas des autorisations suffisantes pour y accéder.
à Microsoft. StorageMigration. proxy. service. Transfer. TransferOperation. Validate () à Microsoft. StorageMigration. proxy. service. Transfer. TransferRequestHandler. ProcessRequest (FileTransferRequest fileTransferRequest, Guid operationId)    [d:\os\src\base\dms\proxy\transfer\transferproxy\TransferRequestHandler.cs::

Cette erreur est attendue si votre compte de migration ne dispose pas au minimum d’autorisations d’accès en lecture aux partages SMB. Pour contourner cette erreur, ajoutez un groupe de sécurité contenant le compte de migration source aux partages SMB sur l’ordinateur source et accordez-lui l’autorisation lecture, modification ou contrôle total. Une fois la migration terminée, vous pouvez supprimer ce groupe.

## <a name="error-0x80005000-when-running-inventory"></a>Erreur 0x80005000 lors de l’exécution de l’inventaire

Après l’installation de [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) et la tentative d’exécution de l’inventaire, l’inventaire échoue avec des erreurs :

  EXCEPTION de HRESULT : 0x80005000
  
  Nom du journal : Microsoft-Windows-StorageMigrationService/admin source : Microsoft-Windows-StorageMigrationService Date : 9/9/2019 5:21:42 PM ID d’événement : 2503 tâche catégorie : aucun niveau : Mots clés d’erreur :      
  Utilisateur : ordinateur de SERVICE réseau : FS02. Description de TailwindTraders.net : impossible d’inventorier les ordinateurs.
Tâche : ID de foo2 : état 20ac3f75-4945-41d1-9a79-d11dbb57798b : erreur échec : 36934 message d’erreur : échec de l’inventaire pour tous les appareils : Vérifiez l’erreur détaillée et assurez-vous que les conditions d’inventaire sont remplies. Le travail n’a pas pu inventorier les ordinateurs source spécifiés. Cela peut être dû au fait que l’ordinateur Orchestrator n’a pas pu l’atteindre sur le réseau, peut-être en raison d’une règle de pare-feu ou d’autorisations manquantes.
  
  Nom du journal : Microsoft-Windows-StorageMigrationService/admin source : Microsoft-Windows-StorageMigrationService Date : 9/9/2019 5:21:42 PM ID d’événement : 2509 tâche catégorie : aucun niveau : Mots clés d’erreur :      
  Utilisateur : ordinateur de SERVICE réseau : FS02. Description de TailwindTraders.net : impossible d’inventorier un ordinateur.
Travail : ordinateur foo2 : FS01. État de TailwindTraders.net : erreur d’échec :-2147463168 message d’erreur : aide : Vérifiez l’erreur détaillée et assurez-vous que les conditions d’inventaire sont remplies. L’inventaire n’a pas pu déterminer les aspects de l’ordinateur source spécifié. Cela peut être dû à des autorisations ou des privilèges manquants sur la source ou sur un port de pare-feu bloqué.
  
Cette erreur est due à un défaut de code dans le service de migration de stockage lorsque vous fournissez des informations d’identification de migration sous la forme d’un nom d’utilisateur principal (UPN), tel que « meghan@contoso.com ». Le service d’analyse du service de migration du stockage ne parvient pas à analyser ce format correctement, ce qui provoque un échec dans une recherche de domaine qui a été ajoutée pour la prise en charge de la migration de cluster dans KB4512534 et 19H1.

Pour contourner ce problème, fournissez les informations d’identification au format domaine\utilisateur, par exemple « Contoso\Meghan ».

## <a name="error-serviceerror0x9006-or-the-proxy-isnt-currently-available-when-migrating-to-a-windows-server-failover-cluster"></a>Erreur « ServiceError0x9006 » ou « le proxy n’est pas disponible actuellement ». lors de la migration vers un cluster de basculement Windows Server

Lorsque vous tentez de transférer des données sur un serveur de fichiers en cluster, vous recevez des erreurs telles que : 

   Assurez-vous que le service proxy est installé et en cours d’exécution, puis réessayez. Le proxy n’est pas disponible actuellement.
0x9006 ServiceError0x9006, Microsoft. StorageMigration. Commands. UnregisterSmsProxyCommand

Cette erreur est attendue si la ressource de serveur de fichiers a été déplacée de son nœud propriétaire de cluster Windows Server 2019 d’origine vers un nouveau nœud et que la fonctionnalité de proxy de service de migration de stockage n’a pas été installée sur ce nœud.

Pour résoudre ce problème, replacez la ressource du serveur de fichiers de destination sur le nœud de cluster propriétaire d’origine qui était utilisé lorsque vous avez configuré pour la première fois les jumelages de transfert.

Une autre solution de contournement consiste à :

1. Installez la fonctionnalité de proxy du service de migration de stockage sur tous les nœuds d’un cluster.
2. Exécutez la commande PowerShell de service de migration de stockage suivante sur l’ordinateur Orchestrator : 

   ```PowerShell
   Register-SMSProxy -ComputerName *destination server* -Force
   ```
## <a name="error-dll-was-not-found-when-running-inventory-from-a-cluster-node"></a>Erreur « dll introuvable » lors de l’exécution de l’inventaire à partir d’un nœud de cluster

Lorsque vous tentez d’exécuter un inventaire avec l’installation de Storage migration service Orchestrator sur un nœud de cluster de basculement Windows Server 2019 et en ciblant une source de serveur de fichiers d’utilisation générale du cluster de basculement Windows Server, vous recevez l’erreur suivante :

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

## <a name="cutover-hangs-on-38-mapping-network-interfaces-on-the-source-comnputer"></a>Le basculement se bloque sur « 38% mappage des interfaces réseau sur le comnputer source... » 

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

Examinining l’ordinateur source indique que l’adresse IP d’origine ne peut pas changer. 

Ce problème ne se produit pas si vous avez sélectionné l’option « utiliser DHCP » dans l’écran du centre d’administration Windows « configurer le basculement », uniquement si vous spécifiez une nouvelle adresse IP statique, un sous-réseau et une passerelle. 

Ce problème est dû à une régression dans la mise à jour [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) . Il existe actuellement deux solutions de contournement pour ce problème :

  - Avant le basculement : au lieu de définir une nouvelle adresse IP statique lors du basculement, sélectionnez « utiliser DHCP » et assurez-vous qu’une étendue DHCP couvre ce sous-réseau. SMS configure l’ordinateur source pour qu’il utilise DHCP sur les interfaces de l’ordinateur source et le découpage se poursuivra normalement. 
  
  - Si le basculement est déjà bloqué : Connectez-vous à l’ordinateur source et activez DHCP sur ses interfaces réseau, après avoir vérifié qu’une étendue DHCP couvre ce sous-réseau. Lorsque l’ordinateur source acquiert une adresse IP fournie par DHCP, SMS poursuit la découpe normalement.
  
Dans les deux solutions de contournement, une fois la coupure terminée, vous pouvez définir une adresse IP statique sur l’ancien ordinateur source comme vous le voyez et l’arrêter à l’aide de DHCP.   

## <a name="see-also"></a>Articles associés

- [Vue d’ensemble de Storage migration service](overview.md)
