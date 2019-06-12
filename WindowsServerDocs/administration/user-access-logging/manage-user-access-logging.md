---
title: Gérer la journalisation des accès utilisateur
description: Décrit comment gérer la journalisation des accès utilisateur
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-user-access-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f039017-4152-47eb-838e-bb6ef730b638
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03bad9864f81cf75be13b4ca391fdcbc5f9dcb5c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435349"
---
# <a name="manage-user-access-logging"></a>Gérer la journalisation des accès utilisateur

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ce document explique comment gérer la journalisation des accès utilisateur (UAL, User Access Logging).  
  
La journalisation des accès utilisateur est une fonctionnalité qui peut aider les administrateurs de serveur à quantifier le nombre de demandes client uniques de rôles et services sur un serveur local.  
  
La journalisation des accès utilisateur est installée et activée par défaut et collecte les données quasiment en temps réel. Seules quelques options de configuration sont disponibles pour la journalisation des accès utilisateur. Ce document décrit ces options et leur effet escompté.  
  
Pour en savoir plus sur les avantages de la journalisation des accès utilisateur, consultez le [bien démarrer avec la journalisation des accès utilisateur](get-started-with-user-access-logging.md).  
  
**Dans ce document**  
  
Les options de configuration traitées dans ce document incluent :  
  
-   Désactivation et activation du service de journalisation des accès utilisateur  
  
-   Collecte et suppression de données  
  
-   Suppression des données consignées par la journalisation des accès utilisateur  
  
-   Gestion de la journalisation des accès utilisateur dans des environnements de grand volume  
  
-   Récupération à partir d’un état endommagé  
  
-   Activation du suivi des licences d’utilisation Dossiers de travail   
  
## <a name="BKMK_Step1"></a>Désactivation et activation du service de journalisation des accès utilisateur  
Journalisation des accès utilisateur est activé et s’exécute par défaut lorsqu’un ordinateur exécutant Windows Server 2012, ou une version ultérieure, sont installé et démarré pour la première fois. Les administrateurs peuvent arrêter et désactiver la journalisation des accès utilisateur pour respecter les exigences de confidentialité ou d’autres besoins opérationnels. Vous pouvez désactiver la journalisation des accès utilisateur à l’aide de la console Services, à partir de la ligne de commande, ou à l’aide des applets de commande PowerShell. Toutefois, pour vous assurer que journalisation des accès utilisateur ne s’exécute pas à nouveau la prochaine fois que l’ordinateur est démarré, vous devez également désactiver le service. Les procédures suivantes décrit comment arrêter et désactiver la journalisation des accès utilisateur.  
  
> [!NOTE]  
> Vous pouvez utiliser l’applet de commande PowerShell `Get-Service UALSVC` pour récupérer des informations sur le service de journalisation des accès utilisateur, y compris les informations indiquant si le service est en cours d’exécution ou arrêté, et s’il est activé ou désactivé.  
  
#### <a name="to-stop-and-disable-the-ual-service-by-using-the-services-console"></a>Pour arrêter et désactiver le service de journalisation des accès utilisateur à l’aide de la console Services  
  
1.  Connectez-vous au serveur avec un compte disposant des privilèges d’administrateur local.  
  
2.  Dans le Gestionnaire de serveur, pointez sur **Outils**, puis cliquez sur **Services**.  
  
3.  Faites défiler l’affichage et sélectionnez **Service de journalisation des accès utilisateur**. Cliquez sur **Arrêter le service**.  
  
4.  Droite\-cliquez sur le nom du service et sélectionnez **propriétés**. Dans l’onglet **Général**, modifiez le **Type de démarrage** en spécifiant **Désactivé**, puis cliquez sur **OK**.  
  
#### <a name="to-stop-and-disable-ual-from-the-command-line"></a>Pour arrêter et désactiver la journalisation des accès utilisateur à partir de la ligne de commande  
  
1.  Connectez-vous au serveur avec un compte disposant des privilèges d’administrateur local.  
  
2.  Appuyez sur les touches Windows + R et tapez **cmd** pour ouvrir une fenêtre d’invite de commandes.  
  
    > [!IMPORTANT]  
    > Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
3.  Tapez **net stop ualsvc**, et appuyez sur ENTRÉE.  
  
4.  Tapez **netsh ualsvc set opmode mode=disable**, puis appuyez sur ENTRÉE.  
   
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  

Vous pouvez également arrêter et désactiver la journalisation des accès utilisateur en utilisant les commandes Windows PowerShell Stop-service et Disable-Ual.  
  
```  
Stop-service ualsvc  
```  
  
```  
Disable-ual  
```  
  
Si ultérieurement vous souhaitez redémarrer et réactiver la journalisation des accès utilisateur vous pouvez le faire avec les procédures suivantes.  
  
#### <a name="to-start-and-enable-the-ual-service-by-using-the-services-console"></a>Pour démarrer et activer le service de journalisation des accès utilisateur à l’aide de la console Services  
  
1.  Connectez-vous au serveur avec un compte disposant des privilèges d’administrateur local.  
  
2.  Dans le Gestionnaire de serveur, pointez sur **Outils**, puis cliquez sur **Services**.  
  
3.  Faites défiler l’affichage et sélectionnez **Service de journalisation des accès utilisateur**. Cliquez sur **Démarrer le service**.  
  
4.  Cliquez avec le bouton droit sur le nom du service et sélectionnez **Propriétés**. Dans l’onglet **Général** , modifiez le **Type de démarrage** en spécifiant **Automatique**, puis cliquez sur **OK**.  
  
#### <a name="to-start-and-enable-ual-from-the-command-line"></a>Pour démarrer et activer la journalisation des accès utilisateur à partir de la ligne de commande  
  
1.  Connectez-vous au serveur avec des informations d’identification d’administrateur local.  
  
2.  Appuyez sur les touches Windows + R et tapez **cmd** pour ouvrir une fenêtre d’invite de commandes.  
  
    > [!IMPORTANT]  
    > Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
3.  Tapez **net start ualsvc**, puis appuyez sur ENTRÉE.  
  
4.  Tapez **netsh ualsvc set opmode mode=enable**, puis appuyez sur ENTRÉE.  

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.
  
Vous pouvez également démarrer et réactiver la journalisation des accès utilisateur en utilisant les commandes Windows PowerShell Start-service et Enable-Ual.  
  
```  
Enable-ual  
```  
  
```  
Start-service ualsvc  
```  
  
## <a name="BKMK_Step2"></a>Collecte des données de journalisation des accès utilisateur  
En plus des applets de commande PowerShell décrites dans la section précédente, 12 autres applets de commande peuvent être utilisés pour collecter des données de journalisation des accès utilisateur :  
  
-   **Get-UalOverview** : fournit des détails associés à la journalisation des accès utilisateur et l’historique des rôles et produits installés.  
  
-   **Get-UalServerUser** : fournit les données des accès des utilisateurs clients pour le serveur local ou ciblé.  
  
-   **Get-UalServerDevice** : fournit les données des accès des périphériques clients pour le serveur local ou ciblé.  
  
-   **Get-UalUserAccess** : fournit les données des accès des utilisateurs clients pour chaque rôle ou produit installé sur le serveur local ou ciblé.  
  
-   **Get-UalDeviceAccess** : fournit les données des accès des périphériques clients pour chaque rôle ou produit installé sur le serveur local ou ciblé.  
  
-   **Get-UalDailyUserAccess** : fournit les données des accès des utilisateurs clients pour chaque jour de l’année.  
  
-   **Get-UalDailyDeviceAccess** : fournit les données des accès des périphériques clients pour chaque jour de l’année.  
  
-   **Get-UalDailyAccess** : fournit à la fois les données des accès des périphériques et des utilisateurs clients pour chaque jour de l’année.  
  
-   **Get-UalHyperV** : fournit les données d’ordinateur virtuel qui se rapportent au serveur local ou ciblé.  
  
-   **Get-UalDns** : fournit les données spécifiques de client DNS du serveur DNS local ou ciblé.  
  
-   **Get-UalSystemId** : fournit les données spécifiques au système permettant d’identifier de façon unique le serveur local ou ciblé.  
  
L’applet de commande `Get-UalSystemId` est censée fournir un profil unique d’un serveur pour toutes les autres données issues de ce serveur à mettre en corrélation.  Si un serveur connaît une modification quelconque d’un des paramètres de `Get-UalSystemId` un nouveau profil est créé.  `Get-UalOverview` est censée fournir à l’administrateur la liste des rôles installés et en cours d’utilisation sur le serveur.  
  
> [!NOTE]  
> Fonctionnalités de base de l’impression et les Services de Document et les Services de fichiers sont installées par défaut. Par conséquent, les administrateurs peuvent s’attendre à toujours voir les informations relatives à ces fonctionnalités affichées comme si les rôles complets étaient installés. Des applets de commande de journalisation des accès utilisateur distinctes sont incluses pour Hyper-V et DNS en raison des données uniques que la journalisation des accès utilisateur collecte pour ces rôles serveur.  
  
Un cas d’usage standard des applets de commande de journalisation des accès utilisateur correspond au scénario dans lequel un administrateur interroge le service de journalisation des accès utilisateur au sujet des accès client uniques sur une plage de dates donnée. Cette opération peut être réalisée de différentes façons. La méthode ci-dessous est recommandée pour obtenir les accès par des périphériques uniques sur une plage de dates.  
  
```  
PS C:\Windows\system32>Gwmi -Namespace "root\AccessLogging" -query "SELECT * FROM MsftUal_DeviceAccess WHERE LastSeen >='1/01/2013' and LastSeen <='3/31/2013'"  
  
```  
  
Ceci retourne une liste détaillée de tous les périphériques client uniques, classés par adresse IP, qui ont effectué des demandes auprès du serveur dans cette plage de dates.  
  
‘ActivityCount’ pour chaque client unique est limité à 65 535 par jour. En outre, l’appel à WMI à partir de PowerShell est requis uniquement lorsque vous effectuez une requête par date.  Tous les autres paramètres des applets de commande de journalisation des accès utilisateur peuvent être utilisés au sein des requêtes PowerShell comme prévu, comme dans l’exemple suivant :  
  
```  
PS C:\Windows\system32> Get-UalDeviceAccess -IPAddress "10.36.206.112"  
  
ActivityCount    : 1  
FirstSeen        : 6/23/2012 5:06:50 AM  
IPAddress        : 10.36.206.112  
LastSeen         : 6/23/2012 5:06:50 AM  
ProductName      : Windows Server 2012 Datacenter  
RoleGuid         : 10a9226f-50ee-49d8-a393-9a501d47ce04  
RoleName         : File Server  
TenantIdentifier : 00000000-0000-0000-0000-000000000000  
PSComputerName  
  
```  
  
La journalisation des accès utilisateur conserve jusqu’à deux années d’historique. Pour permettre à un administrateur de récupérer les données de journalisation des accès utilisateur lorsque le service est en cours d’exécution, la journalisation des accès utilisateur effectue une copie du fichier de la base de données active, current.mdb, dans un fichier nommé *GUID.mdb* toutes les 24 heures, à l’usage du fournisseur WMI.  
  
Le premier jour de l’année, la journalisation des accès utilisateur cré un nouveau fichier *GUID.mdb*. L’ancien fichier *GUID.mdb* est conservé sous forme d’archive à l’usage du fournisseur.  Au bout de deux ans, le fichier *GUID.mdb* original est remplacé.  
  
> [!IMPORTANT]  
> La procédure suivante doit être exécutée uniquement par un utilisateur avancé et serait communément utilisée par un développeur testant sa propre instrumentation des interfaces de programmation d’application de journalisation des accès utilisateur...  
  
#### <a name="to-adjust-the-default-24-hour-interval-to-make-data-visible-to-the-wmi-provider"></a>Pour ajuster l’intervalle par défaut de 24 heures pour rendre les données visibles au fournisseur WMI  
  
1.  Connectez-vous au serveur avec un compte disposant des privilèges d’administrateur local.  
  
2.  Appuyez sur les touches Windows + R et tapez **cmd** pour ouvrir une fenêtre d’invite de commandes.  
  
3.  Ajoutez la valeur de Registre :  **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\WMI\AutoLogger\Sum\PollingInterval (REG_DWORD)** .  
  
    > [!WARNING]  
    > Une modification incorrecte du Registre peut endommager gravement votre système. Avant d’apporter des modifications au Registre, il est conseillé de sauvegarder les données importantes stockées sur l’ordinateur.  
  
    L’exemple suivant montre comment ajouter un intervalle de deux minutes (non recommandé comme état d’exécution à long terme) : **REG ADD HKLM\System\CurrentControlSet\Control\WMI\\AutoLogger\Sum /v PollingInterval /t REG\_DWORD /d 120000 /F**  
  
    Les valeurs temporelles sont exprimées en millisecondes. La valeur minimale est de 60 secondes, la valeur maximale de sept jours et la valeur par défaut de 24 heures.  
  
4.  Utilisez la console Services pour arrêter et redémarrer le service de journalisation des accès utilisateur.  
  
## <a name="deleting-data-logged-by-ual"></a>Suppression des données consignées par la journalisation des accès utilisateur  
La journalisation des accès utilisateur n’a pas pour vocation d’être un composant essentiel. Elle a été conçue pour affecter le moins possible les opérations système locales tout en maintenant un haut niveau de fiabilité. Cela permet également l’administrateur de supprimer manuellement la base de données de journalisation des accès utilisateur et les fichiers associés (chaque fichier dans le répertoire de \Windows\System32\LogFilesSUM\) pour répondre aux besoins opérationnels.  
  
#### <a name="to-delete-data-logged-by-ual"></a>Pour supprimer les données consignées par la journalisation des accès utilisateur  
  
1. Arrêtez le service de journalisation des accès utilisateur.  
  
2. Ouvrez l’Explorateur Windows.  
  
3. Accédez à **\Windows\System32\Logfiles\SUM\\** .  
  
4. Supprimez tous les fichiers de ce dossier.  
  
## <a name="managing-ual-in-high-volume-environments"></a>Gestion de la journalisation des accès utilisateur dans des environnements de grand volume  
Cette section décrit ce à quoi un administrateur peut s’attendre lorsque la journalisation des accès utilisateur est utilisée sur un serveur à grand volume client :  
  
Le nombre maximal d’accès pouvant être consignés par le biais de la journalisation des accès utilisateur est de 65 535 par jour.  Il n’est pas recommandé d’utiliser la journalisation des accès utilisateur sur des serveurs connectés directement à Internet, tels que des serveurs Web connectés directement à Internet, ni dans des scénarios où des performances extrêmement élevées constituent l’objectif principal du serveur (par exemple, dans les environnements HPC). Journalisation des accès utilisateur sont principalement destinée aux petites, moyennes et scénarios intranet d’entreprise où un volume élevé est attendu, mais pas aussi élevé autant déploiement qui font Office de volume de trafic sur Internet de manière régulière.  
  
**Journalisation des accès utilisateur dans la mémoire** : comme la journalisation des accès utilisateur utilise la méthode ESE (Extensible Storage Engine), la mémoire requise augmentera au fil du temps (ou en fonction de la quantité des demandes client). En revanche, la mémoire sera libérée lorsque le système en aura besoin pour réduire l’impact sur les performances système.  
  
**Journalisation des accès utilisateur sur le disque** : les capacités approximatives de disque dur requises par la journalisation des accès utilisateur sont indiquées ci-dessous :  
  
-   0 enregistrement client unique : 22 Mo  
  
-   50 000 enregistrements client unique : 80 Mo  
  
-   500 000 enregistrements client unique : 384 Mo  
  
-   1 000 000 enregistrements client unique : 729 Mo  
  
## <a name="recovering-from-a-corrupt-state"></a>Récupération à partir d’un état endommagé  
Cette section traite de l’utilisation par la journalisation des accès utilisateur de la méthode ESE (Extensible Storage Engine) à un niveau élevé et de ce que peut faire un administrateur si les données de journalisation des accès utilisateur sont endommagées ou irrécupérables.  
  
La journalisation des accès utilisateur utilise ESE pour optimiser l’utilisation des ressources système et pour renforcer leur résistance à l’endommagement.  Pour plus d’informations sur les avantages que procure ESE, voir [ESE (Extensible Storage Engine)](https://msdn.microsoft.com/library/windows/desktop/gg269259(v=exchg.10).aspx) sur MSDN.  
  
Chaque fois que le service de journalisation des accès utilisateur démarre, ESE effectue une récupération logicielle. Pour plus d’informations, voir [Fichiers ESE (Extensible Storage Engine)](https://msdn.microsoft.com/library/windows/desktop/gg294069(v=exchg.10).aspx) sur MSDN.  
  
En cas de problème lié à la récupération logicielle, ESE effectue une récupération sur incident. Pour plus d’informations, voir [Fonction JetInit](https://msdn.microsoft.com/library/windows/desktop/gg294068(v=exchg.10).aspx) sur MSDN.  
  
Si la journalisation des accès utilisateur ne peut toujours pas démarrer avec l’ensemble existant de fichiers ESE, elle supprimera tous les fichiers présents dans le répertoire \Windows\System32\LogFiles\SUM\. Une fois ces fichiers supprimés, le service de journalisation des accès utilisateur redémarrera et de nouveaux fichiers seront créés. Le service de journalisation des accès utilisateur reprendra son exécution comme s’il se trouvait sur un ordinateur nouvellement configuré.  
  
> [!IMPORTANT]  
> Les fichiers présents dans le répertoire de la base de données de journalisation des accès utilisateur ne doivent jamais être déplacés ni modifiés. Tout manquement à cette règle initiera la procédure de récupération, dans laquelle s’inscrit la routine de nettoyage décrite dans cette section. Si des sauvegardes du répertoire \Windows\System32\LogFiles\SUM\ sont requises, tous les fichiers de ce répertoire doivent être sauvegardés ensemble pour qu’une opération de restauration fonctionne comme prévu.  
  
## <a name="enable-work-folders-usage-license-tracking"></a>Activation du suivi des licences d’utilisation Dossiers de travail  
Le serveur Dossiers de travail peut utiliser la journalisation des accès utilisateur pour faire état de l’utilisation du client. Contrairement à la journalisation des accès utilisateur, la journalisation des dossiers de travail n’est pas activée par défaut. Vous pouvez l’activer en modifiant la clé de Registre suivante :  
  
```  
Reg add HKLM\Software\Microsoft\Windows\CurrentVersion\SyncShareSrv /v EnableWorkFoldersUAL /t REG_DWORD /d 1  
```  
  
Après avoir ajouté la clé de Registre, vous devez redémarrer le service SyncShareSvc sur le serveur, pour activer la journalisation.  
  
Une fois la journalisation activée, 2 événements à caractère informatif sont consignés dans le canal Journaux Windows\Application chaque fois qu’un client se connecte au serveur. Pour Dossiers de travail, chaque utilisateur peut avoir un ou plusieurs périphériques clients se connectant au serveur et recherchant des mises à jour de données toutes les 10 minutes. Dans le cas d’un serveur gérant 1 000 utilisateurs, dotés de 2 périphériques chacun, les journaux d’applications sont remplacés toutes les 70 minutes, ce qui complique la résolution des problèmes non associés. Pour éviter ce problème, vous pouvez désactiver le service de journalisation des accès utilisateur temporairement ou augmenter la taille du canal de Windows Windows\Application du serveur.  
  
## <a name="BKMK_Links"></a>Voir aussi  

- [Prise en main utilisateur journalisation des accès](get-started-with-user-access-logging.md)
  

