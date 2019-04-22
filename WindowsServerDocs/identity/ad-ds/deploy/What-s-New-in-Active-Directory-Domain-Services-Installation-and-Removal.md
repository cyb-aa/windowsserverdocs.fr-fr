---
ms.assetid: ba7f2b9f-7351-4680-b7d8-a5f270614f1c
title: Nouveautés relatives à l’installation et à la suppression des services de domaine Active Directory
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 66455a9ec4eb8a6ff6bfcfa387aeb59acb3ddcc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821350"
---
# <a name="whats-new-in-active-directory-domain-services-installation-and-removal"></a>Nouveautés relatives à l’installation et à la suppression des services de domaine Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Déploiement d’Active Directory domaine Services (AD DS) dans Windows Server 2012 est plus simple et plus rapide que les versions antérieures de Windows Server. Le processus d’installation des services AD DS repose à présent sur Windows PowerShell et est intégré au Gestionnaire de serveur. Le nombre d’étapes requises pour introduire des contrôleurs de domaine dans un environnement Active Directory existant est en baisse, ce qui rend le processus de création d’un environnement Active Directory plus simple et plus efficace. Le nouveau processus de déploiement des services AD DS réduit au minimum le risque d’erreurs susceptibles de bloquer l’installation.  
  
Par ailleurs, vous pouvez installer les binaires du rôle serveur AD DS (c’est-à-dire le rôle serveur AD DS) sur plusieurs serveurs en même temps. Vous pouvez également exécuter l’Assistant Installation des services de domaine Active Directory à distance sur un serveur individuel. Ces améliorations offrent plus de souplesse pour le déploiement de contrôleurs de domaine qui exécutent Windows Server 2012, en particulier pour les déploiements à grande échelle, globales où plusieurs contrôleurs de domaine doivent être déployés vers des bureaux situés dans différentes régions.  
  
L’installation des services AD DS comprend les fonctionnalités suivantes :  
  
- **Intégration d’Adprep.exe dans le processus d’installation AD DS.** Certaines étapes jugées peu pratiques et qui étaient requises pour préparer un annuaire Active Directory existant, comme la nécessité d’utiliser une série d’informations d’identification différentes, la copie des fichiers Adprep.exe ou encore la connexion à des contrôleurs de domaine spécifiques, ont été simplifiées ou sont effectuées automatiquement. Il en résulte une réduction du temps nécessaire à l’installation des services AD DS et une réduction du risque d’erreurs susceptibles de bloquer la promotion du contrôleur de domaine.  

   Concernant les environnements dans lesquels il est préférable d’exécuter des commandes adprep.exe préalablement à l’installation d’un nouveau contrôleur de domaine, vous pouvez toujours exécuter les commandes adprep.exe séparément de l’installation des services AD DS. La version de Windows Server 2012 d’adprep.exe s’exécutant à distance, vous pouvez exécuter toutes les commandes nécessaires à partir d’un serveur qui exécute une version 64 bits de Windows Server 2008 ou version ultérieure.  

- **La nouvelle installation des services AD DS repose sur Windows PowerShell et peut être appelée à distance.** De par l’intégration de la nouvelle installation des services AD DS au Gestionnaire de serveur, vous pouvez utiliser la même interface pour installer les services AD DS que celle que vous utilisez pour installer d’autres rôles serveurs. Pour les utilisateurs Windows PowerShell, les applets de commande de déploiement des services AD DS offrent plus de fonctionnalités et une flexibilité accrue. Une parité fonctionnelle existe entre les options d’installation de ligne de commande et celles de l’interface graphique utilisateur.  
- **La nouvelle installation des services AD DS inclut la validation des conditions préalables.** Les erreurs potentielles sont identifiées avant le début de l’installation. Vous pouvez corriger les conditions d’erreur avant qu’elles ne se produisent et éviter ainsi les problèmes résultant d’une mise à niveau partiellement terminée. Par exemple, si la commande adprep /domainprep doit être exécutée, l’Assistant Installation vérifie que l’utilisateur dispose des droits suffisants pour exécuter l’opération.  
- **Les pages de configuration sont regroupées en une séquence qui reflète les exigences des options de promotion les plus courantes avec des options associées regroupées dans moins de pages de l’Assistant.** Cela offre un meilleur contexte pour les choix d’installation.  
- **Vous pouvez exporter un script Windows PowerShell qui contient toutes les options qui ont été spécifiées pendant l’installation graphique.** À la fin d’une installation ou d’une suppression, vous pouvez exporter les paramètres vers un script Windows PowerShell pour automatiser la même opération.  
- **Seule la réplication critique se produit avant le redémarrage.** Nouveau commutateur permettant la réplication des données non critiques avant le redémarrage. Pour plus d'informations, voir [ADDSDeployment cmdlet arguments](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params).  

## <a name="BKMK_ADConfigurationWizard"></a>L’Assistant de Configuration de Services de domaine Active Directory

À compter de Windows Server 2012, l’Assistant Configuration des Services de domaine Active Directory remplace l’hérité Active Directory domaine Services Assistant Installation sert d’option d’interface utilisateur pour spécifier les paramètres lorsque vous installez un contrôleur de domaine. L’Assistant Configuration des services de domaine Active Directory est lancé au terme de l’exécution de l’Assistant Ajout de rôles.  

> [!WARNING]  
> Le hérité Active Directory domaine Services Assistant Installation (dcpromo.exe) est déconseillé à compter de Windows Server 2012.  

Dans [installer Active Directory Domain Services &#40;niveau 100&#41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md), les procédures de l’interface utilisateur indiquent comment démarrer l’Assistant Ajout de rôles pour installer le serveur AD DS binaires du rôle, puis exécutez les Services de domaine Active Directory Assistant de configuration pour terminer l’installation de contrôleur de domaine. Les exemples Windows PowerShell montrent comment effectuer les deux étapes à l’aide d’une applet de commande de déploiement AD DS.  
  
## <a name="BKMK_NewAdprep"></a>Intégration d’Adprep.exe

À compter de Windows Server 2012, il existe une seule version d’Adprep.exe (il n’existe aucune version 32 bits, adprep32.exe). Commandes adprep sont exécutées automatiquement en fonction des besoins lorsque vous installez un contrôleur de domaine qui exécute Windows Server 2012 à une forêt ou un domaine Active Directory existant.  
  
Bien que les opérations adprep soient exécutées automatiquement, vous pouvez exécuter Adprep.exe séparément. Par exemple, si l’utilisateur qui installe les services AD DS n’est pas membre du groupe Administrateurs de l’entreprise (une condition requise à l’exécution d’Adprep /forestprep), vous devrez peut-être exécuter la commande séparément. Mais vous devez uniquement exécuter adprep.exe si vous prévoyez de mise à niveau sur place votre premier contrôleur de domaine Windows Server 2012 (en d’autres termes, vous prévoyez d’effectuer sur place mettre à niveau le système d’exploitation d’un contrôleur de domaine qui exécute Windows Server 2012).  
  
Adprep.exe se trouve dans le dossier \support\adprep du disque d’installation de Windows Server 2012. La version de Windows Server 2012 d’adprep peut être exécutée à distance.  
  
La version de Windows Server 2012 d’adprep.exe peut s’exécuter sur n’importe quel serveur qui exécute une version 64 bits de Windows Server 2008 ou version ultérieure. Le serveur doit bénéficier d’une connectivité réseau au contrôleur de schéma pour la forêt et au maître d’infrastructure sur lequel vous voulez ajouter un contrôleur de domaine. Si l’un de ces rôles est hébergé sur un serveur Windows Server 2003, adprep doit être exécuté à distance. Le serveur sur lequel vous exécutez adprep ne doit pas forcément être un contrôleur de domaine. Il peut soit être joint à un domaine, soit se trouver dans un groupe de travail.  

> [!NOTE]  
> Si vous essayez d’exécuter la version de Windows Server 2012 d’adprep.exe sur un serveur qui exécute Windows Server 2003, l’erreur suivante s’affiche :  
>   
> Adprep.exe n’est pas une application Win32 valide.  

![Nouveautés](media/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal/AdprepNotValid.gif)  

Pour plus d’informations sur la résolution d’autres erreurs retournées par Adprep.exe, voir [Problèmes connus](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues).  

### <a name="group-membership-check-against-windows-server-2003-operations-master-roles"></a>Vérification de l’appartenance aux groupes par rapport aux rôles de maître d’opérations Windows Server 2003

Pour chaque commande (/forestprep, /domainprep ou /rodcprep), Adprep vérifie l’appartenance aux groupes pour déterminer si les informations d’identification spécifiées représentent un compte dans certains groupes. Pour effectuer cette vérification, Adprep contacte le propriétaire du rôle de maître d’opérations. Si le maître d’opérations exécute Windows Server 2003 et que vous exécutez Adprep.exe, vous devez spécifier les paramètres de ligne de commande /user et /userdomain pour vous assurer que la vérification de l’appartenance aux groupes est effectuée dans tous les cas.  
  
/User et /userdomain sont de nouveaux paramètres pour Adprep.exe dans Windows Server 2012. Ces paramètres spécifient respectivement le nom du compte et le domaine de l’utilisateur qui exécute la commande adprep. L’utilitaire de ligne de commande Adprep.exe vous empêche de ne spécifier qu’un seul des deux paramètres (/userdomain ou /user).  
  
Toutefois, les opérations Adprep peuvent également être exécutées dans le cadre d’une installation des services AD DS à l’aide de Windows PowerShell ou du Gestionnaire de serveur. Ces expériences partagent la même implémentation sous-jacente (adprep.dll) qu’adprep.exe. L’entrée des informations d’identification, qui s’effectue séparément pour Windows PowerShell et le Gestionnaire de serveur, n’impose pas les mêmes conditions que celles requises par adprep.exe. Si vous utilisez Windows PowerShell ou le Gestionnaire de serveur, il est possible de passer une valeur pour /user mais pas /userdomain à adprep.dll. Si /user est spécifié mais que /userdomain n’est pas spécifié, le domaine de l’ordinateur local est utilisé pour effectuer la vérification. Si l’ordinateur n’est pas joint à un domaine, l’appartenance aux groupes ne peut pas être vérifiée.  
  
Lorsque l’appartenance aux groupes ne peut pas être vérifiée, Adprep affiche un message d’avertissement dans les fichiers journaux adprep et continue :  

```
Adprep was unable to check the specified user's group membership. This could happen if the FSMO role owner <DNS host name of operations master> is running Windows Server 2003 or lower version of Windows.  
```

Si vous exécutez Adprep.exe sans spécifier les paramètres /user et /userdomain et que le maître d’opérations exécute Windows Server 2003, Adprep.exe contacte un contrôleur de domaine dans le domaine de l’utilisateur actuellement connecté. Si l’utilisateur actuellement connecté n’est pas un compte de domaine, Adprep.exe ne peut pas effectuer la vérification de l’appartenance aux groupes. Il en va de même si des informations d’identification de carte à puce sont utilisées, et ce même si vous spécifiez /user et /userdomain.  
  
Si l’exécution d’Adprep se termine correctement, aucune action n’est requise. En cas d’échec de l’exécution d’Adprep avec des erreurs d’accès, spécifiez un compte avec une appartenance correcte. Pour plus d’informations, voir [Informations d’identification requises pour exécuter Adprep.exe et installer les services de domaine Active Directory](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
### <a name="syntax-for-adprep-in-windows-server-2012"></a>Syntaxe pour Adprep dans Windows Server 2012

Utilisez la syntaxe suivante pour exécuter adprep séparément d’une installation des services AD DS :  

```
Adprep.exe /forestprep /forest <forest name> /userdomain <user domain name> /user <user name> /password *  
```

Utilisez /logdsid dans la commande afin de générer une journalisation plus détaillée. Le journal adprep.log se trouve à l’emplacement suivant : %windir%\System32\Debug\Adprep\Logs.  

### <a name="running-adprep-using-smartcard"></a>Exécution d’adprep à l’aide d’une carte à puce

La version de Windows Server 2012 d’adprep.exe fonctionne à l’aide de la carte à puce comme informations d’identification, mais il n’existe aucun moyen facile pour spécifier les informations d’identification de carte à puce via la ligne de commande. Une façon d’y parvenir consiste à d’obtenir les informations d’identification de carte à puce par le biais de l’applet de commande PowerShell Get-Credential. Ensuite, il suffit d’utiliser le nom d’utilisateur de l’objet PSCredential retourné, celui-ci apparaissant sous la forme `@@...`. Le mot de passe est le code confidentiel de la carte à puce.  

Adprep.exe requiert /userdomain si /user est spécifié. Pour les informations d’identification de carte à puce , /userdomain doit être le domaine du compte d’utilisateur sous-jacent représenté par la carte à puce.  

### <a name="adprep-domainprep-gpprep-command-is-not-run-automatically"></a>La commande adprep /domainprep /gpprep n’est pas exécutée automatiquement.

La commande adprep /domainprep /gpprep n’est pas exécutée dans le cadre de l’installation des services AD DS. Cette commande définit les autorisations requises pour le mode de planification du jeu de stratégie résultant (RSoP). Pour plus d’informations sur cette commande, voir l’[article 324392 de la Base de connaissances Microsoft](https://support.microsoft.com/kb/324392). Si la commande doit être exécutée dans votre domaine Active Directory, vous pouvez l’exécuter séparément de l’installation des services AD DS. Si la commande a déjà été exécutée en préparation du déploiement de contrôleurs de domaine exécutant Windows Server 2003 SP1 ou version ultérieure, il est inutile de réexécuter la commande.  

Vous pouvez ajouter en toute sécurité des contrôleurs de domaine qui exécutent Windows Server 2012 à un domaine existant sans adprep /domainprep /gpprep en cours d’exécution, mais le mode de planification RSOP ne fonctionnera pas correctement.  

## <a name="BKMK_PrereqCheck"></a>Validation des conditions préalables AD DS installation

L’Assistant Installation des services de domaine Active Directory vérifie que les conditions préalables suivantes sont remplies avant de démarrer l’installation. Cela vous donne la possibilité de corriger les problèmes susceptibles de bloquer l’installation.  
  
Parmi les conditions préalables liées à Adprep, citons les suivantes :  

- Vérification des informations d’identification Adprep : si l’exécution d’Adprep s’avère nécessaire, l’Assistant Installation vérifie que l’utilisateur dispose de droits suffisants pour exécuter les opérations Adprep requises.  
- Vérification de la disponibilité du contrôleur de schéma : si l’Assistant Installation détermine que la commande adprep /forestprep doit être exécutée, il vérifie que le contrôleur de schéma est en ligne ; sinon, il échoue.  
- Vérification de la disponibilité du maître d’infrastructure : si l’Assistant Installation détermine que la commande adprep /domainprep doit être exécutée, il vérifie que le maître d’infrastructure est en ligne ; sinon, il échoue.

Voici d’autres vérifications des conditions préalables qui sont issues de l’Assistant Installation des services de domaine Active Directory hérité (dcpromo.exe) :  

- Vérification du nom de la forêt : vérifie que le nom de la forêt est valide et qu’il n’est pas déjà utilisé.  
- Vérification du nom NetBIOS : vérifie que le nom NetBIOS fourni est valide et qu’il n’est pas en conflit avec des noms existants.  
- Vérification des chemins d’accès des composants : vérifie que les chemins d’accès pour la base de données Active Directory, les journaux et SYSVOL sont valides et que l’espace disque nécessaire pour les prendre en charge est suffisant.  
- Vérification du nom du domaine enfant : vérifie que les noms du parent et du nouveau domaine enfant sont valides et qu’ils ne sont pas en conflit avec des domaines existants.  
- Vérification du nom du domaine de l’arborescence : vérifie que le nom de l’arborescence spécifié est valide et qu’il n’existe pas actuellement.  

## <a name="BKMK_SystemReqs"></a>Configuration système requise

Configuration système requise pour Windows Server 2012 est identiques à Windows Server 2008 R2. Pour plus d’informations, consultez [Windows Server 2008 R2 avec SP1 requise](https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx) (https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx).  

Il est possible que des conditions supplémentaires soient associées à certaines fonctionnalités. Par exemple, la fonctionnalité de clonage de contrôleur de domaine virtuel exige que l’émulateur de contrôleur de domaine principal exécutant Windows Server 2012 et un ordinateur exécutant Windows Server 2012 avec le rôle Hyper-V installé.  

## <a name="BKMK_KnownIssues"></a>Problèmes connus

Cette section répertorie certains des problèmes connus qui affectent l’installation AD DS dans Windows Server 2012. Pour découvrir d’autres problèmes connus, voir [Résolution des problèmes de déploiement de contrôleur de domaine](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md).  

- Si l’accès WMI au maître de schéma est bloqué par le Pare-feu Windows lorsque vous exécutez à distance adprep /forestprep, l’erreur suivante est enregistrée dans le journal adprep à l’emplacement %systemroot%\system32\debug\adprep :  

   ```
   Adprep encountered a Win32 error.
   Error code: 0x6ba Error message: The RPC server is unavailable.  
   ```

   Dans ce cas, pour contourner cette erreur, vous pouvez soit exécuter adprep /forestprep directement sur le contrôleur de schéma, soit exécuter l’une des commandes suivantes pour autoriser le trafic de WMI à travers le Pare-feu Windows.

   Pour Windows Server 2008 ou version ultérieure :

   ```
   netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes
   ```

   Pour Windows Server 2003 :

   ```
   netsh firewall set service RemoteAdmin enable
   ```

   Une fois adprep terminé, vous pouvez exécuter l’une des commandes suivantes pour bloquer de nouveau le trafic WMI :

   ```
   netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=no
   ```

   ```
   netsh firewall set service remoteadmin disable
   ```

- Vous pouvez appuyer sur CTRL+C pour annuler l’applet de commande Install-ADDSForest. L’annulation arrête l’installation, et toutes les modifications apportées à l’état du serveur sont annulées. Toutefois, après l’émission de la commande d’annulation, le contrôle n’est pas retourné à Windows PowerShell et l’applet de commande peut se bloquer indéfiniment.  
- **Installation du contrôleur de domaine supplémentaire à l’aide des informations d’identification de carte à puce échoue si le serveur cible n’est pas joint au domaine avant l’installation.**  

   Le message d’erreur retourné dans ce cas est le suivant :  

   Impossible de se connecter au contrôleur de domaine source de réplication *nom du contrôleur de domaine source*. (Exception : (Échec de l’ouverture de session : nom d’utilisateur inconnu ou mot de passe incorrect)  

   Si vous joignez le serveur cible au domaine, puis effectuez l’installation à l’aide d’une carte à puce, l’installation aboutit.  
  
- **Le module ADDSDeployment ne s’exécute pas sous les processus 32 bits.** Si vous automatisez le déploiement et la configuration de Windows Server 2012 à l’aide d’un script qui inclut une applet de commande ADDSDeployment et toute autre applet de commande ne prend pas en charge les processus 64 bits natifs, le script peut échouer avec une erreur qui indique la ADDSDeployment applet de commande est introuvable.  

   Dans ce cas, vous devez exécuter l’applet de commande ADDSDeployment séparément de l’applet de commande qui ne prend pas en charge les processus 64 bits natifs.  

- Il est un nouveau système de fichiers dans Windows Server 2012, nommé Resilient File System. Ne stockez pas la base de données Active Directory, les fichiers journaux ou SYSVOL sur un volume de données au format ReFS. Pour plus d’informations sur ReFS, consultez [création du système de fichier prochaine génération pour Windows : ReFS](http://blogs.msdn.com/b/b8/archive/2012/01/16/building-the-next-generation-file-system-for-windows-refs.aspx).  
- Dans le Gestionnaire de serveur, les serveurs qui exécutent les services AD DS ou autres rôles de serveur sur une installation Server Core et qui ont été mis à niveau vers Windows Server 2012, le rôle de serveur peut apparaître avec un état rouge, même si les événements et l’état sont collectés comme prévu. Serveurs qui exécutent une installation minimale d’une version préliminaire que Windows Server 2012 peuvent également être touchés.  

### <a name="active-directory-domain-services-installation-hangs-if-an-error-prevents-critical-replication"></a>L’installation des services de domaine Active Directory se bloque si une erreur empêche une réplication critique.

Si l’installation des services AD DS se heurte à une erreur lors de la phase de réplication critique, l’installation peut se bloquer indéfiniment. Par exemple, si des erreurs réseau empêchent l’achèvement de la réplication critique, l’installation ne continue pas.  
  
Si vous effectuez l’installation à l’aide du Gestionnaire de serveur, il est possible que la page de progression de l’installation reste ouverte sans qu’aucune erreur ne soit signalée à l’écran et que la progression reste inchangée pendant environ 15 minutes. Si vous utilisez Windows PowerShell, la progression affichée dans la fenêtre Windows PowerShell n’évolue pas pendant plus de 15 minutes.  
  
Si vous rencontrez ce problème, examinez le fichier dcpromo.log dans le dossier %systemroot%\debug sur le serveur cible. Le fichier journal indique généralement des échecs répétés à répliquer. Parmi les causes connues de ce problème, citons les suivantes :  

- Des problèmes réseau empêchent la réplication critique entre le serveur cible qui est promu et le contrôleur de domaine source de réplication.  

   Par exemple, le journal dcpromo.log affiche ce qui suit :  

   ```  
   05/02/2012 14:16:46 [INFO] EVENTLOG (Error): NTDS Replication / DS RPC Client : 1963  
   Internal event: The following local directory service received an exception from a remote procedure call (RPC) connection. Extensive RPC information was requested. This is intermediate information and might not contain a possible cause.  
   Process ID:   
   500  
   Reported error information:  
   Error value:   
   Could not find the domain controller for this domain. (1908)  
   directory service:   
   <domain>.com  
   Extensive error information:  
   Error value:   
   A security package specific error occurred. 1825  
   directory service:   
   <DC Name>  
   ```  

   Étant donné que le processus d’installation retente la réplication critique indéfiniment, l’installation du contrôleur de domaine se poursuit si les problèmes réseau sous-jacents sont résolus. Étudiez le problème réseau à l’aide d’outils tels qu’ipconfig, nslookup et netmon, si nécessaire. Vérifiez l’existence d’une connectivité entre le contrôleur de domaine que vous promouvez et le partenaire de réplication sélectionné pendant l’installation des services AD DS. Vérifiez également que la résolution de noms fonctionne.  

   La configuration requise pour l’installation des services AD DS pour la connectivité réseau et la résolution de noms est validée pendant la vérification de la configuration requise, avant le début de l’installation. Certaines conditions d’erreur peuvent néanmoins se produire entre le moment de la validation préalable et la fin de l’installation, par exemple si le partenaire de réplication devient indisponible pendant l’installation.  

- Au cours de l’installation du contrôleur de domaine réplica, le compte d’administrateur local du serveur cible est spécifié pour les informations d’identification d’installation et le mot de passe du compte d’administrateur local correspond au mot de passe d’un compte d’administrateur de domaine. Dans ce cas, vous pouvez terminer l’Assistant installation et commencer l’installation avant de rencontrer l’erreur « Accès refusé ».  

   Par exemple, le journal dcpromo.log affiche ce qui suit :  

   ```  
   03/30/2012 11:36:51 [INFO] Creating the NTDS Settings object for this Active Directory Domain Controller on the remote AD DC DC2.contoso.com...  
   03/30/2012 11:36:51 [INFO] EVENTLOG (Error): NTDS Replication / DS RPC Client : 1963Internal event: The following local directory service received an exception from a remote procedure call (RPC) connection. Extensive RPC information was requested. This is intermediate information and might not contain a possible cause.  
   Process ID:   
   508  
   Reported error information:  
   Error value:   
   Access is denied. (5)  
   directory service:   
   DC2.contoso.com  
   ```  

   Si l’erreur est causée par la spécification d’un compte et d’un mot de passe d’administrateur local, vous devez, pour corriger le problème, réinstaller le système d’exploitation, [effectuer un nettoyage des métadonnées](https://technet.microsoft.com/library/cc816907(WS.10).aspx) du compte pour le contrôleur de domaine qui n’a pas réussi à effectuer l’installation, puis réessayer l’installation des services de domaine Active Directory à l’aide d’informations d’identification de l’administrateur de domaine. Le redémarrage du serveur ne corrige pas cette condition d’erreur, car le serveur indique que les services AD DS est installé même si l’installation ne s’est pas terminée correctement.  

### <a name="BKMK_nonnormalDNSNameWarning"></a>Assistant Configuration de Services de domaine Active Directory vous avertit lorsqu’un nom DNS non normalisé est spécifié

Si vous créez un domaine ou une forêt et que vous spécifiez un nom de domaine DNS contenant des caractères internationaux qui ne sont pas normalisés, l’Assistant Configuration des services de domaine Active Directory affiche alors un avertissement indiquant l’échec possible des requêtes DNS pour le nom. Bien que le nom de domaine DNS soit spécifié dans la page Configuration de déploiement, l’avertissement apparaît dans la page Vérification de la configuration requise plus loin dans l’Assistant.  

Si un nom de domaine DNS est spécifié à l’aide d’un nom non normalisé comme .com füßball.com ou « ΣΤ » (les versions normalisées sont : füssball.com et βστα.com), les applications clientes qui tentent d’y accéder avec WinHTTP normalise le nom avant d’appeler les API de résolution de nom. Si l’utilisateur tape « « ΣΤ » .com » dans une boîte de dialogue, la requête DNS est envoyée comme « βστα.com » et aucun serveur DNS seront en correspondance avec un enregistrement de ressource pour « .com « ΣΤ » ». L’utilisateur ne sera pas en mesure de résoudre le nom.  

L’exemple suivant explique l’un des problèmes pouvant se produire lors de l’utilisation d’un nom IDN qui n’est pas normalisé :  

1. Le domaine à l’aide d’un nom non normalisée est créé et enregistré sur le serveur dns : füßball.com  
2. Machine « nps » est joint au domaine et obtient son nom enregistré : nps.füßball.com  
3. Une application cliente essaie de se connecter à la nps.füßball.com de serveur  
4. L’application cliente tente de résoudre le nps.füßball.com nom appelant les API de résolution de nom.  
5. En raison de la normalisation, le nom est converti en nps.füssball.com et est interrogé sur le réseau en tant que nps.füßball.com  
6. L’application cliente ne peut pas résoudre le nom dans la mesure où le nom inscrit est nps.füßball.com  

Si l’avertissement apparaît dans la page Vérification de la configuration requise de l’Assistant Configuration des services de domaine Active Directory, retournez à la page Configuration de déploiement et spécifiez un nom de domaine DNS normalisé. Si vous installez un nouveau domaine à l’aide de Windows PowerShell, spécifiez un nom DNS normalisé pour l’option -DomainName.  

Pour plus d’informations sur les noms IDN, voir [Traitement des noms de domaines internationaux](https://msdn.microsoft.com/library/windows/desktop/dd318142(v=vs.85).aspx).  
