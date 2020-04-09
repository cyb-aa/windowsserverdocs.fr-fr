---
title: Gérer la journalisation des accès utilisateur
description: Décrit comment gérer la journalisation des accès utilisateur
ms.prod: windows-server
ms.technology: manage-user-access-logging
ms.topic: article
ms.assetid: 4f039017-4152-47eb-838e-bb6ef730b638
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ff33102a2197cc961a44290c5b7e4e3e40b0191
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851382"
---
# <a name="manage-user-access-logging"></a>Gérer la journalisation des accès utilisateur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ce document explique comment gérer la journalisation des accès utilisateur (UAL, User Access Logging).  
  
La journalisation des accès utilisateur est une fonctionnalité qui peut aider les administrateurs de serveur à quantifier le nombre de demandes client uniques de rôles et services sur un serveur local.  
  
La journalisation des accès utilisateur est installée et activée par défaut et collecte les données quasiment en temps réel. Seules quelques options de configuration sont disponibles pour la journalisation des accès utilisateur. Ce document décrit ces options et leur effet escompté.  
  
Pour en savoir plus sur les avantages de la [journalisation des accès utilisateur, consultez prise en main de la journalisation des accès utilisateur](get-started-with-user-access-logging.md).  
  
**Dans ce document**  
  
Les options de configuration traitées dans ce document incluent :  
  
-   Désactivation et activation du service de journalisation des accès utilisateur  
  
-   Collecte et suppression de données  
  
-   Suppression des données consignées par la journalisation des accès utilisateur  
  
-   Gestion de la journalisation des accès utilisateur dans des environnements de grand volume  
  
-   Récupération à partir d’un état endommagé  
  
-   Activation du suivi des licences d’utilisation Dossiers de travail   
  
## <a name="disabling-and-enabling-the-ual-service"></a><a name="BKMK_Step1"></a>Désactivation et activation du service de journalisation des accès aux services  
La journalisation des accès système est activée et s’exécute par défaut lorsqu’un ordinateur exécutant Windows Server 2012 ou version ultérieure est installé et démarré pour la première fois. Les administrateurs peuvent arrêter et désactiver la journalisation des accès utilisateur pour respecter les exigences de confidentialité ou d’autres besoins opérationnels. Vous pouvez désactiver cette option à l’aide de la console Services, à partir de la ligne de commande ou à l’aide des applets de commande PowerShell. Toutefois, pour s’assurer que la journalisation des accès système ne s’exécute pas à nouveau au prochain démarrage de l’ordinateur, vous devez également désactiver le service. Les procédures suivantes décrivent comment désactiver et désactiver la journalisation des éléments de journal.  
  
> [!NOTE]  
> Vous pouvez utiliser l’applet de commande PowerShell `Get-Service UALSVC` pour récupérer des informations sur le service de journalisation des accès utilisateur, y compris les informations indiquant si le service est en cours d’exécution ou arrêté, et s’il est activé ou désactivé.  
  
#### <a name="to-stop-and-disable-the-ual-service-by-using-the-services-console"></a>Pour arrêter et désactiver le service de journalisation des accès utilisateur à l’aide de la console Services  
  
1.  Connectez-vous au serveur avec un compte disposant des privilèges d’administrateur local.  
  
2.  Dans le Gestionnaire de serveur, pointez sur **Outils**, puis cliquez sur **Services**.  
  
3.  Faites défiler l’affichage et sélectionnez **Service de journalisation des accès utilisateur**. Cliquez sur **Arrêter le service**.  
  
4.  Cliquez avec le bouton droit\-sur le nom du service, puis sélectionnez **Propriétés**. Dans l’onglet **Général**, modifiez le **Type de démarrage** en spécifiant **Désactivé**, puis cliquez sur **OK**.  
  
#### <a name="to-stop-and-disable-ual-from-the-command-line"></a>Pour arrêter et désactiver la journalisation des accès utilisateur à partir de la ligne de commande  
  
1.  Connectez-vous au serveur avec un compte disposant des privilèges d’administrateur local.  
  
2.  Appuyez sur les touches Windows + R et tapez **cmd** pour ouvrir une fenêtre d’invite de commandes.  
  
    > [!IMPORTANT]  
    > Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
3.  Tapez **net stop ualsvc**, et appuyez sur ENTRÉE.  
  
4.  Tapez **netsh ualsvc set opmode mode=disable**, puis appuyez sur ENTRÉE.  
   
La ou les applets de commande Windows PowerShell suivantes ont la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles apparaissent ici sur plusieurs lignes en raison de contraintes de mise en forme.  

Vous pouvez également arrêter et désactiver la journalisation des accès utilisateur en utilisant les commandes Windows PowerShell Stop-service et Disable-Ual.  
  
```  
Stop-service ualsvc  
```  
  
```  
Disable-ual  
```  
  
Si vous souhaitez ultérieurement redémarrer et réactiver la journalisation des e/r, vous pouvez le faire avec les procédures suivantes.  
  
#### <a name="to-start-and-enable-the-ual-service-by-using-the-services-console"></a>Pour démarrer et activer le service de journalisation des accès utilisateur à l’aide de la console Services  
  
1.  Connectez-vous au serveur avec un compte disposant des privilèges d’administrateur local.  
  
2.  Dans le Gestionnaire de serveur, pointez sur **Outils**, puis cliquez sur **Services**.  
  
3.  Faites défiler l’affichage et sélectionnez **Service de journalisation des accès utilisateur**. Cliquez sur **Démarrer le service** .  
  
4.  Cliquez avec le bouton droit sur le nom du service et sélectionnez **Propriétés**. Dans l’onglet **Général**, modifiez le **Type de démarrage** en spécifiant **Automatique**, puis cliquez sur **OK**.  
  
#### <a name="to-start-and-enable-ual-from-the-command-line"></a>Pour démarrer et activer la journalisation des accès utilisateur à partir de la ligne de commande  
  
1.  Connectez-vous au serveur avec des informations d’identification d’administrateur local.  
  
2.  Appuyez sur les touches Windows + R et tapez **cmd** pour ouvrir une fenêtre d’invite de commandes.  
  
    > [!IMPORTANT]  
    > Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
3.  Tapez **net start ualsvc**, puis appuyez sur ENTRÉE.  
  
4.  Tapez **netsh ualsvc set opmode mode=enable**, puis appuyez sur ENTRÉE.  

La ou les applets de commande Windows PowerShell suivantes ont la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles apparaissent ici sur plusieurs lignes en raison de contraintes de mise en forme.
  
Vous pouvez également démarrer et réactiver la journalisation des accès utilisateur en utilisant les commandes Windows PowerShell Start-service et Enable-Ual.  
  
```  
Enable-ual  
```  
  
```  
Start-service ualsvc  
```  
  
## <a name="collecting-ual-data"></a><a name="BKMK_Step2"></a>Collecte des données de journalisation des informations  
En plus des applets de commande PowerShell décrites dans la section précédente, 12 applets de commande supplémentaires peuvent être utilisées pour collecter des données de journalisation des informations de bout en bout :  
  
-   **Get-UalOverview**: fournit des détails associés à la journalisation des accès utilisateur et l’historique des rôles et produits installés.  
  
-   **Get-UalServerUser** : fournit les données des accès des utilisateurs clients pour le serveur local ou ciblé.  
  
-   **Get-UalServerDevice** : fournit les données des accès des périphériques clients pour le serveur local ou ciblé.  
  
-   **Get-UalUserAccess** : fournit les données des accès des utilisateurs clients pour chaque rôle ou produit installé sur le serveur local ou ciblé.  
  
-   **Get-UalDeviceAccess** : fournit les données des accès des périphériques clients pour chaque rôle ou produit installé sur le serveur local ou ciblé.  
  
-   **Get-UalDailyUserAccess** : fournit les données des accès des utilisateurs clients pour chaque jour de l’année.  
  
-   **Get-UalDailyDeviceAccess**: fournit les données des accès des périphériques clients pour chaque jour de l’année.  
  
-   **Get-UalDailyAccess**: fournit à la fois les données des accès des périphériques et des utilisateurs clients pour chaque jour de l’année.  
  
-   **Get-UalHyperV** : fournit les données d’ordinateur virtuel qui se rapportent au serveur local ou ciblé.  
  
-   **Get-UalDns** : fournit les données spécifiques de client DNS du serveur DNS local ou ciblé.  
  
-   **Get-UalSystemId** : fournit les données spécifiques au système permettant d’identifier de façon unique le serveur local ou ciblé.  
  
L’applet de commande `Get-UalSystemId` est censée fournir un profil unique d’un serveur pour toutes les autres données issues de ce serveur à mettre en corrélation.  Si un serveur subit une modification dans l’un des paramètres de `Get-UalSystemId` un nouveau profil est créé.  `Get-UalOverview` est censée fournir à l’administrateur la liste des rôles installés et en cours d’utilisation sur le serveur.  
  
> [!NOTE]  
> Les fonctionnalités de base des Services d’impression et de numérisation de document et des services de fichiers sont installées par défaut. Par conséquent, les administrateurs peuvent s’attendre à toujours voir les informations relatives à ces fonctionnalités affichées comme si les rôles complets étaient installés. Des applets de commande de journalisation des e/d distinctes sont incluses pour Hyper-V et DNS en raison des données uniques collectées par la journalisation des informations pour ces rôles serveur.  
  
Un cas d’usage standard des applets de commande de journalisation des accès utilisateur correspond au scénario dans lequel un administrateur interroge le service de journalisation des accès utilisateur au sujet des accès client uniques sur une plage de dates donnée. Cela peut être effectué de différentes façons. La méthode suivante est recommandée pour interroger des accès d’appareil uniques sur une plage de dates.  
  
```  
PS C:\Windows\system32>Gwmi -Namespace "root\AccessLogging" -query "SELECT * FROM MsftUal_DeviceAccess WHERE LastSeen >='1/01/2013' and LastSeen <='3/31/2013'"  
  
```  
  
Ceci retourne une liste détaillée de tous les périphériques client uniques, classés par adresse IP, qui ont effectué des demandes auprès du serveur dans cette plage de dates.  
  
« ActivityCount » pour chaque client unique est limité à 65 535 par jour. En outre, l’appel à WMI à partir de PowerShell est requis uniquement lorsque vous interrogez par date.  Tous les autres paramètres d’applet de commande de journalisation des éléments de journal peuvent être utilisés dans les requêtes PS comme prévu, comme dans l’exemple suivant :  
  
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
  
La journalisation des accès d’inscription conserve jusqu’à deux années d’histoire. Pour autoriser la récupération des données de base par un administrateur lorsque le service est en cours d’exécution, la journalisation des accès d’ordinateur crée une copie du fichier de base de données actif, Current. mdb, dans un fichier nommé *GUID. mdb* toutes les 24 heures pour l’utilisation du fournisseur WMI.  
  
Le premier jour de l’année, la journalisation des accès utilisateur cré un nouveau fichier *GUID.mdb*. L’ancien *GUID. mdb* est conservé en tant qu’Archive pour l’utilisation du fournisseur.  Au bout de deux ans, le fichier *GUID.mdb* original est remplacé.  
  
> [!IMPORTANT]  
> La procédure suivante doit être exécutée uniquement par un utilisateur avancé et serait communément utilisée par un développeur testant sa propre instrumentation des interfaces de programmation d’application de journalisation des accès utilisateur...  
  
#### <a name="to-adjust-the-default-24-hour-interval-to-make-data-visible-to-the-wmi-provider"></a>Pour ajuster l’intervalle par défaut de 24 heures pour rendre les données visibles au fournisseur WMI  
  
1.  Connectez-vous au serveur avec un compte disposant des privilèges d’administrateur local.  
  
2.  Appuyez sur les touches Windows + R et tapez **cmd** pour ouvrir une fenêtre d’invite de commandes.  
  
3.  Ajoutez la valeur de Registre :  **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\WMI\AutoLogger\Sum\PollingInterval (REG_DWORD)** .  
  
    > [!WARNING]  
    > Une modification incorrecte du Registre peut sérieusement endommager votre système. Avant d’apporter des modifications au Registre, il est conseillé de sauvegarder les données importantes stockées sur l’ordinateur.  
  
    L’exemple suivant montre comment ajouter un intervalle de deux minutes (non recommandé comme état d’exécution à long terme) : **reg Add HKLM\System\CurrentControlSet\Control\WMI\\AutoLogger\Sum/V PollingInterval/T REG\_DWORD/d 120000/f**  
  
    Les valeurs temporelles sont exprimées en millisecondes. La valeur minimale est de 60 secondes, la valeur maximale de sept jours et la valeur par défaut de 24 heures.  
  
4.  Utilisez la console Services pour arrêter et redémarrer le service de journalisation des accès utilisateur.  
  
## <a name="deleting-data-logged-by-ual"></a>Suppression des données consignées par la journalisation des accès utilisateur  
La journalisation des accès utilisateur n’a pas pour vocation d’être un composant essentiel. Elle a été conçue pour affecter le moins possible les opérations système locales tout en maintenant un haut niveau de fiabilité. Cela permet également à l’administrateur de supprimer manuellement la base de données de base de données et les fichiers de prise en charge (chaque fichier dans le répertoire \Windows\System32\LogFilesSUM\) pour répondre aux besoins opérationnels.  
  
#### <a name="to-delete-data-logged-by-ual"></a>Pour supprimer les données consignées par la journalisation des accès utilisateur  
  
1. Arrêtez le service de journalisation des accès utilisateur.  
  
2. Ouvrez l’Explorateur Windows.  
  
3. Accédez à **\Windows\System32\Logfiles\SUM\\** .  
  
4. Supprimez tous les fichiers de ce dossier.  
  
## <a name="managing-ual-in-high-volume-environments"></a>Gestion de la journalisation des accès utilisateur dans des environnements de grand volume  
Cette section décrit ce à quoi un administrateur peut s’attendre lorsque la journalisation des accès utilisateur est utilisée sur un serveur à grand volume client :  
  
Le nombre maximal d’accès pouvant être consignés par le biais de la journalisation des accès utilisateur est de 65 535 par jour.  La journalisation des connexions réseau n’est pas recommandée pour une utilisation sur des serveurs connectés directement à Internet, tels que des serveurs Web connectés directement à Internet ou dans des scénarios où des performances extrêmement élevées constituent la fonction principale du serveur (par exemple, dans les environnements de charge de travail HPC). La journalisation des accès aux entreprises est principalement conçue pour les scénarios intranet de petite, moyenne et entreprise où un volume élevé est prévu, mais pas aussi élevé que le nombre de déploiements qui servent le volume de trafic Internet régulièrement.  
  
**Mémoire disponible : dans**la mesure où la journalisation des applications utilise le moteur ESE (Extensible Storage Engine), les besoins en mémoire de la journalisation des e/d augmentent au fil du temps (ou par quantité de demandes client). En revanche, la mémoire sera libérée lorsque le système en aura besoin pour réduire l’impact sur les performances système.  
  
Journalisation **sur disque**: la configuration requise pour le disque dur de la journalisation des éléments est approximativement la suivante :  
  
-   0 enregistrement client unique : 22 Mo  
  
-   50 000 enregistrements client unique : 80 Mo  
  
-   500 000 enregistrements client unique : 384 Mo  
  
-   1 000 000 enregistrements client unique : 729 Mo  
  
## <a name="recovering-from-a-corrupt-state"></a>Récupération à partir d’un état endommagé  
Cette section traite de l’utilisation de la journalisation du moteur de stockage extensible (ESE) à un niveau élevé et de ce qu’un administrateur peut faire si les données de journalisation des éléments de journal sont endommagées ou irrécupérables.  
  
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
  
Une fois la journalisation activée, 2 événements à caractère informatif sont consignés dans le canal Journaux Windows\Application chaque fois qu’un client se connecte au serveur. Pour Dossiers de travail, chaque utilisateur peut avoir un ou plusieurs périphériques clients se connectant au serveur et recherchant des mises à jour de données toutes les 10 minutes. Dans le cas d’un serveur gérant 1 000 utilisateurs, dotés de 2 périphériques chacun, les journaux d’applications sont remplacés toutes les 70 minutes, ce qui complique la résolution des problèmes non associés. Pour éviter ce risque, vous pouvez désactiver temporairement le service de journalisation des accès utilisateur ou augmenter la taille du canal Windows Windows\application du serveur.  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Voir aussi  

- [Prise en main de la journalisation des accès utilisateur](get-started-with-user-access-logging.md)
  

