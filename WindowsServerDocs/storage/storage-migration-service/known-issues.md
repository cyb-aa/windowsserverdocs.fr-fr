---
title: Service de Migration de stockage problèmes connus
description: Problèmes connus et dépannage prise en charge du Service de Migration de stockage, notamment comment collecter des journaux pour le Support Microsoft.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 07/09/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 08156a09491d66016b5fcfe6056ed318d682b987
ms.sourcegitcommit: 514d659c3bcbdd60d1e66d3964ede87b85d79ca9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67735158"
---
# <a name="storage-migration-service-known-issues"></a>Service de Migration de stockage problèmes connus

Cette rubrique contient des réponses aux problèmes connus lors de l’utilisation [Service de Migration de stockage](overview.md) pour migrer des serveurs.

## <a name="collecting-logs"></a> Comment collecter des fichiers journaux lorsque vous travaillez avec le Support Microsoft

Le Service de Migration de stockage contient des journaux des événements pour le service Orchestrator et le Service de Proxy. Le serveur urchestrator contient toujours les deux journaux des événements, et les serveurs de destination avec le service de proxy installé contiennent les journaux de proxy. Ces journaux sont situés sous :

- Journaux des applications et Services \ Microsoft \ Windows \ StorageMigrationService
- Journaux des applications et Services \ Microsoft \ Windows \ StorageMigrationService-Proxy

Si vous avez besoin pour collecter ces journaux hors connexion ou à envoyer au Support Microsoft, il est un script PowerShell disponible sur GitHub, open source :

 [Application auxiliaire Service de Migration de stockage](https://aka.ms/smslogs) 

Passez en revue le fichier Lisezmoi pour l’utilisation.

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>Service de Migration de stockage n’apparaît pas dans Windows Admin Center, sauf si la gestion de Windows Server 2019

Lorsque vous utilisez la version 1809 de Windows Admin Center pour gérer un orchestrateur de Windows Server 2019, vous ne voyez pas l’option d’outil pour le Service de Migration de stockage. 

L’extension de Service de Migration de stockage Windows Admin Center est lié aux version pour gérer uniquement la version de Windows Server 2019 1809 ou des systèmes d’exploitation ultérieurs. Si vous l’utilisez pour gérer les systèmes d’exploitation Windows Server ou des versions préliminaires insider, l’outil ne s’affiche pas. Ce comportement est normal. 

Pour résoudre, utilisez ou mettre à niveau vers Windows Server 2019 build 1809 ou version ultérieure.

## <a name="storage-migration-service-doesnt-let-you-choose-static-ip-on-cutover"></a>Service de Migration de stockage ne vous permettent de choisir une adresse IP statique sur le basculement

Lorsque vous utilisez la version 0,57 la Migration du Service de stockage extension dans Windows Admin Center et vous atteindre la phase de basculement, vous ne pouvez pas sélectionner une adresse IP statique pour une adresse. Vous êtes obligé d’utiliser le protocole DHCP.

Pour résoudre ce problème, dans Windows Admin Center, regardez sous **paramètres** > **Extensions** pour une alerte indiquant que la version mise à jour Service de Migration de stockage 0.57.2 peut être installée. Vous devrez peut-être redémarrer votre onglet de navigateur pour le centre d’administration de Windows.

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>Validation de basculement de Service de Migration de stockage échoue avec l’erreur « Accès refusé pour la stratégie de jeton de filtre sur l’ordinateur de destination »

Lors de l’exécution de la validation de basculement, vous recevez l’erreur « Échec : Accès refusé pour la stratégie de jeton de filtre sur l’ordinateur de destination. » Cela se produit même si vous avez fourni les informations d’identification d’administrateur local approprié pour les ordinateurs source et de destination.

Ce problème est dû à une erreur de code dans Windows Server 2019. Le problème se produit lorsque vous utilisez l’ordinateur de destination en tant qu’un orchestrateur de Service de Migration de stockage.

Pour contourner ce problème, installez le Service de Migration de stockage sur un ordinateur Windows Server 2019 qui n’est pas la destination de migration souhaité, puis se connecter à ce serveur avec Windows Admin Center et effectuer la migration.

Nous avons résolu ce problème dans une version ultérieure de Windows Server. Ouvrez une demande de support via [Support Microsoft](https://support.microsoft.com) pour demander un rétroporter de ce correctif soit créé.

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-edition"></a>Service de Migration de stockage n’est pas inclus dans l’édition d’évaluation de Windows Server 2019

Lorsque vous utilisez Windows Admin Center pour vous connecter à un [version d’évaluation de Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) il n’est pas une option permettant de gérer le Service de Migration de stockage. Service de Migration de stockage n’est pas également inclus dans les rôles et fonctionnalités.

Ce problème est dû à un problème de maintenance dans le support d’évaluation de Windows Server 2019. 

Pour contourner ce problème, installez une vente au détail, de la version MSDN, OEM ou licence en Volume de Windows Server 2019 et ne pas l’activer. Sans activation, toutes les éditions de Windows Server fonctionnent en mode d’évaluation de 180 jours. 

Nous avons résolu ce problème dans une version ultérieure de Windows Server 2019.  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>Service de Migration de stockage expire de téléchargement de l’erreur de transfert CSV

Lorsque vous utilisez Windows Admin Center ou PowerShell pour télécharger l’opérations détaillées erreurs uniquement CSV journal de transfert, erreur :

 >   Journal de transfert, vérifiez que le partage de fichiers est autorisé dans votre pare-feu. : Cette opération de demande envoyée à NET.TCP://localhost : 28940/sms/service/1/transfert n’a pas reçu de réponse dans le délai imparti (00 : 01:00). Le temps alloué à cette opération peut avoir fait partie d’un délai d’expiration plus long. Cela peut être, car le service traite toujours l’opération ou le service n’a pas pu envoyer un message de réponse. Veuillez, envisagez d’augmenter le délai d’expiration de l’opération (en castant le canal/proxy vers IContextChannel et en définissant la propriété OperationTimeout) et vous assurer que le service est en mesure de se connecter au client.

Ce problème est dû à un très grand nombre de fichiers transférés qui ne peuvent pas être filtrées dans le délai d’une minute par défaut autorisé par le Service de Migration de stockage. 

Pour contourner ce problème :

1. Sur l’ordinateur orchestrator, modifiez le *%SYSTEMROOT%\SMS\Microsoft.StorageMigration.Service.exe.config* fichier à l’aide de Notepad.exe pour modifier le paramètre « sendTimeout » à partir de sa valeur par défaut de 1 minute à 10 minutes

   ```
     <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:01:00"
   ```

2. Redémarrez le service « Service de Migration de stockage » sur l’ordinateur orchestrator. 
3. Sur l’ordinateur orchestrator, lancez Regedit.exe
4. Localisez la sous-clé de Registre suivante, puis cliquez dessus : 

   `HKEY_LOCAL_MACHINE\\Software\\Microsoft\\SMSPowershell`

5. Dans le menu Edition, pointez sur Nouveau, puis cliquez sur DWORD Value. 
6. Tapez « WcfOperationTimeoutInMinutes » pour le nom de la valeur DWORD, puis appuyez sur ENTRÉE.
7. Cliquez sur « WcfOperationTimeoutInMinutes », puis cliquez sur Modifier. 
8. Dans la zone de la Base de données, cliquez sur « Décimal »
9. Dans la zone de données de valeur, tapez « 10 », puis cliquez sur OK.
10. Quittez l’Éditeur du Registre.
11. Essayez à nouveau de télécharger le fichier CSV erreurs uniquement. 

Nous avons l’intention de modifier ce comportement dans une version ultérieure de Windows Server 2019.  

## <a name="cutover-fails-when-migrating-between-networks"></a>Le basculement échoue lors de la migration entre les réseaux

Lorsque vous migrez vers un ordinateur de destination en cours d’exécution dans un autre réseau que la source, par exemple une instance Azure IaaS, le basculement échoue lorsque la source utilisait une adresse IP statique. 

Ce comportement est normal, afin d’éviter les problèmes de connectivité après la migration à partir des utilisateurs, des applications et des scripts pour se connecter via l’adresse IP. Lorsque l’adresse IP est déplacée à partir de l’ancien ordinateur source vers la nouvelle cible de destination, il ne correspondra pas les nouvelles informations de sous-réseau de réseau et de peut-être DNS et de WINS.

Pour contourner ce problème, effectuez une migration vers un ordinateur sur le même réseau. Déplacer cet ordinateur vers un nouveau réseau, puis réaffecter ses informations IP. Par exemple, si une migration vers Azure IaaS, tout d’abord migrer vers une machine virtuelle locale, puis utiliser Azure Migrate pour déplacer la machine virtuelle vers Azure.  

Nous avons résolu ce problème dans une version ultérieure de Windows Admin Center. Nous allons permettent à présent vous permettent de spécifier des migrations qui ne pas modifier les paramètres de réseau du serveur de destination. L’extension de mise à jour s’afficheront ici lors de la publication. 

## <a name="validation-warnings-for-destination-proxy-and-credential-administrative-privileges"></a>Avertissements de validation pour les privilèges d’administration de destination proxy et les informations d’identification

Lors de la validation d’une tâche de transfert, vous consultez les avertissements suivants :

 > **Les informations d’identification a des privilèges d’administrateur.**
 > Avertissement : Action n’est pas disponible à distance.
 > **Le proxy de destination est inscrit.**
 > Avertissement : Le proxy de destination n’a pas été trouvé.

Si vous n’avez pas installé le service de Proxy de Service de Migration de stockage sur l’ordinateur de destination Windows Server 2019, ou l’ordinateur de destination est Windows Server 2016 ou Windows Server 2012 R2, ce comportement est normal. Nous vous recommandons de migrer vers un ordinateur Windows Server 2019 avec le proxy est installé pour les performances de transfert considérablement améliorées.  

## <a name="certain-files-do-not-inventory-or-transfer-error-5-access-is-denied"></a>Certains fichiers ne pas d’inventaire ou de transfert, l’erreur 5 « accès refusé »

Lors de l’inventaire ou de transfert de fichiers à partir de la source vers les ordinateurs de destination, les fichiers à partir de laquelle un utilisateur a supprimé les autorisations du groupe administrateurs ne pas migrer. Examen du débogage de Proxy du Service de stockage Migration montre :

  Nom du journal :      Source de Microsoft-Windows-StorageMigrationService-Proxy/Debug :        Date de Microsoft-Windows-StorageMigrationService-Proxy :          26/2/2019 ID d’événement 9 h 00:04 :      Catégorie de tâche 10000 : Aucun niveau :         Erreur mots-clés :      
  Utilisateur :          Ordinateur de SERVICE de réseau : Description de le srv1.contoso.com :

  Erreur de transfert 02/26/2019-09:00:04.860 [erreur] pour \\srv1.contoso.com\public\indy.png : (5) l’accès est refusé.
Trace de la pile : Au Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.OpenFile (String fileName, DesiredAccess desiredAccess, ShareMode shareMode, CreationDisposition creationDisposition, FlagsAndAttributes flagsAndAttributes) à Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile (chemin d’accès de la chaîne) à Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile (fichier FileInfo) à Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.InitializeSourceFileInfo() à Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.Transfer() à Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.TryTransfer() [d:\os\src\base\dms\proxy\transfer\transferproxy\FileTransfer.cs::TryTransfer::55]


Ce problème est dû à une erreur de code dans le Service de Migration de stockage où le privilège de sauvegarde a été appelé pas. 

Pour résoudre ce problème, installez [mise à jour de Windows le 2 avril 2019 — KB4490481 (17763.404 de Build du système d’exploitation)](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481) sur l’ordinateur orchestrator et de l’ordinateur de destination, si le service de proxy est installé il. Assurez-vous que le compte d’utilisateur de migration source est un administrateur local sur l’ordinateur source et l’orchestrateur de Service de Migration de stockage. Assurez-vous que le compte d’utilisateur de migration destination est un administrateur local sur l’ordinateur de destination et l’orchestrateur de Service de Migration de stockage. 

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>Incompatibilité de hachages DFSR lors de l’utilisation du Service de Migration de stockage à prédéfini données

Lorsque vous utilisez le Service de Migration de stockage pour transférer des fichiers vers une nouvelle destination, puis en configurant la réplication DFS (DFSR) pour répliquer ces données avec un serveur existant de DFSR via presseded réplication DFSR base de données ou le clonage, tous les fichiers experiemce un hachage incompatibilité et sont ré-répliquées. Les flux de données, flux de sécurité, des tailles et attributs apparaissent à mettre en correspondance parfaitement après l’utilisation de SMS pour les transférer. L’examen de le, les fichiers avec ICACLS ou le journal de débogage de clonage de la base de données DFSR révèle :

Fichier source :

  icacls d:\test\Source :

  icacls d:\test\thatcher.png /save out.txt /t thatcher.png D:AI(A;;FA;;;BA)(A;;0x1200a9;;;DD)(A;;0x1301bf;;;DU)(A;ID;FA;;;BA)(A;ID;FA;;;SY)(A;ID;0x1200a9;;;BU)

Fichier de destination :

  icacls d:\test\thatcher.png/enregistrer out.txt /t thatcher.png D:AI(A;; FA ; ; BA) (A ; 0 x1301bf ; ; D U) (A ; 0 x1200a9 ; ; D (D) (A ; ID ; FA ; ; BA) (A ; ID ; FA ; ; SY) (A ; ID ; 0X1200A9 ; ; BU)**S:PAINO_ACCESS_CONTROL**

Journal de débogage DFSR :

  Enregistrement de l’incompatibilité de DBClone::IDTableImportUpdate DBCL 4045 [avertissement] 3948 20190308 10:18:53.116 a été trouvé. 

  Hachage d’ACL local : 1BCDFE03-A18BCE01-D1AE9859-23A0A5F6 LastWriteTime:20190308 18:09:44.876 FileSizeLow:1131654 FileSizeHigh:0 Attributs : 32 

  Cloner le hachage de l’ACL :**DDC4FCE4-DDF329C4-977CED6D-F4D72A5B** LastWriteTime:20190308 18:09:44.876 FileSizeLow:1131654 FileSizeHigh:0 Attributs : 32 

Ce problème est dû à une erreur de code dans une bibliothèque utilisée par le Service de Migration de stockage pour définir l’audit de sécurité ACL (SACL). Une liste SACL non null est involontairement définie lorsque la liste SACL était vide, DFSR pour identifier correctement une incompatibilité de hachage de début. 

Pour contourner ce problème, passez à l’aide de Robocopy pour [DFSR préamorçage et les opérations de clonage de la base de données DFSR](../dfs-replication/preseed-dfsr-with-robocopy.md) plutôt qu’au Service de Migration de stockage. Nous étudions actuellement ce problème et que vous avez l’intention résoudre ce problème dans une version plus récente de Windows Server et éventuellement une rétroportée mise à jour de Windows. 

## <a name="error-404-when-downloading-csv-logs"></a>Journaux d’erreur 404 lors du téléchargement CSV

Lorsque vous tentez de télécharger les journaux de transfert ou une erreur à la fin d’une opération de transfert, erreur :

  $jobname : Journal de transfert : erreur ajax 404

Cette erreur est attendue si vous n’avez pas activé la règle de pare-feu « Partage de fichiers et imprimantes (SMB-In) » sur le serveur orchestrator. Téléchargements de fichiers Windows Admin Center nécessitent le port TCP/445 (SMB) sur des ordinateurs connectés.  

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transfering-from-windows-server-2008-r2"></a>Erreur » n’a pas pu transférer le stockage sur des points de terminaison » lorsque lors du transfert à partir de Windows Server 2008 R2

Lorsque vous tentez de transférer des données à partir d’un ordinateur source de Windows Server 2008 R2, aucun transfert de données et vous l’erreur :  

  N’a pas pu transférer le stockage sur des points de terminaison.
0x9044

Cette erreur est attendue si votre ordinateur Windows Server 2008 R2 n’est pas entièrement corrigé avec toutes les critiques et importantes mises à jour Windows Update. Quel que soit le Service de Migration de stockage, nous recommandons toujours mise à jour corrective d’un ordinateur Windows Server 2008 R2 pour des raisons de sécurité, car ce système d’exploitation ne contient pas les améliorations de sécurité des versions plus récentes de Windows Server.

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-and-check-if-the-source-device-is-online---we-couldnt-access-it"></a>Erreur » n’a pas pu transférer le stockage sur des points de terminaison » et « Vérifier si l’appareil source est en ligne - nous n’avons pas pu y accéder. »

Lorsque vous tentez de transférer des données à partir d’un ordinateur source, certains ou tous les partages ne sont pas transférés, avec un résumé des erreurs :

   N’a pas pu transférer le stockage sur des points de terminaison.
0x9044

Examiner les détails de transfert SMB affiche Erreur :

   Vérifiez que si l’appareil source est en ligne - nous n’avons pas pu y accéder.

Examiner le journal des événements StorageMigrationService/Admin montre :

   Impossible de transférer le stockage.

   Tâche : ID de Job1 :  
   État : Erreur : Message d’erreur 36931 : 

   Instructions : Examinez l’erreur détaillée et assurez-vous que le transfert sont satisfaites. La tâche de transfert n’a pas pu transférer tous les ordinateurs source et de destination. Il peut s’agir, car l’ordinateur orchestrator n’a pas pu atteindre tous les ordinateurs source ou destination, probablement en raison d’une règle de pare-feu, ou les autorisations manquantes.

Examiner le journal StorageMigrationService-Proxy/Debug affiche :

   Échec de la validation de transfert 07/02/2019-13:35:57.231 [erreur]. Code d’erreur : 40961, point de terminaison source n’est pas accessible ou n’existe pas, ou informations d’identification de la source ne sont pas valides ou utilisateur authentifié ne dispose pas des autorisations suffisantes pour y accéder.
à Microsoft.StorageMigration.Proxy.Service.Transfer.TransferOperation.Validate() à Microsoft.StorageMigration.Proxy.Service.Transfer.TransferRequestHandler.ProcessRequest (FileTransferRequest fileTransferRequest, Guid operationId)    [d:\os\src\base\dms\proxy\transfer\transferproxy\TransferRequestHandler.cs ::

Cette erreur est attendue si votre compte de migration n’a pas au moins des autorisations d’accès en lecture aux partages SMB. Pour contourner cette erreur, ajoutez un groupe de sécurité qui contient le compte de source de migration pour les partages SMB sur l’ordinateur source et lui accorder en lecture, modification ou contrôle total. Une fois la migration terminée, vous pouvez supprimer ce groupe. Une version ultérieure de Windows Server peut modifier ce comportement pour n’ont plus besoin des autorisations explicites pour les partages sources.

## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble du Service de Migration de stockage](overview.md)
