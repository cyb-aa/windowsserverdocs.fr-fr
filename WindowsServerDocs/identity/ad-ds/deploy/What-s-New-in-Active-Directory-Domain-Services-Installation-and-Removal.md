---
ms.assetid: ba7f2b9f-7351-4680-b7d8-a5f270614f1c
title: 'Quelle & #39; Installation et suppression des Services de s de domaine Active Directory'
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 814a6f1af9144db28e81163b21cb5da4b6e51b3b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="what39s-new-in-active-directory-domain-services-installation-and-removal"></a>Quelle & #39; Installation et suppression des Services de s de domaine Active Directory

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique couvre:  
  
-   [L’Assistant de Configuration de Services de domaine Active Directory](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_ADConfigurationWizard)  
  
-   [Intégration d’Adprep.exe](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_NewAdprep)  
  
-   [Validation des conditions préalables AD DS installation](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_PrereqCheck)  
  
-   [Configuration système requise](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_SystemReqs)  
  
-   [Problèmes connus](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues)  
  
Déploiement d’Active Directory des Services de domaine (AD DS) dans Windows Server 2012 est plus simple et plus rapide que les versions précédentes de Windows Server. Le processus d’installation AD DS s’appuie sur Windows PowerShell et est intégré au Gestionnaire de serveur. Le nombre d’étapes requises pour introduire des contrôleurs de domaine dans un environnement Active Directory existant est réduit. Cela rend le processus de création d’un environnement Active Directory plus simple et plus efficace. Le nouveau processus de déploiement d’AD DS réduit le risque d’erreurs susceptibles de bloquer l’installation.  
  
En outre, vous pouvez installer les binaires du rôle serveur AD DS (c'est-à-dire le rôle de serveur AD DS) sur plusieurs serveurs en même temps. Vous pouvez également exécuter l’Assistant d’installation AD DS à distance sur un serveur individuel. Ces améliorations offrent plus de flexibilité pour le déploiement de contrôleurs de domaine qui exécutent Windows Server 2012, en particulier pour les déploiements à grande échelle, globales où plusieurs contrôleurs de domaine doivent être déployés dans les bureaux dans différentes régions.  
  
Installation du service d’annuaire Active Directory comprend les fonctionnalités suivantes:  
  
-   **Intégration d’Adprep.exe dans le processus d’installation AD DS.** Les étapes jugées peu pratiques requises pour préparer un annuaire Active Directory existant, comme la nécessité d’utiliser une série d’informations d’identification différentes, la copie des fichiers Adprep.exe ou ouvrir une session sur les contrôleurs de domaine spécifiques, ont été simplifiées ou sont effectuées automatiquement. Cela réduit le temps nécessaire pour installer les services AD DS et le risque d’erreurs susceptibles de bloquer la promotion de contrôleur de domaine.  
  
    Pour les environnements où il est préférable d’exécuter des commandes adprep.exe préalablement une nouvelle installation de contrôleur de domaine, vous pouvez toujours exécuter commandes adprep.exe séparément de l’installation des services AD DS. La version de Windows Server 2012 d’adprep.exe s’exécute à distance, vous pouvez exécuter toutes les commandes nécessaires à partir d’un serveur qui exécute une version 64 bits de Windows Server 2008 ou version ultérieure.  
  
-   **La nouvelle installation des services AD DS repose sur Windows PowerShell et peut être appelée à distance.** La nouvelle installation des services AD DS est intégrée au Gestionnaire de serveur, vous pouvez utiliser la même interface pour installer les services AD DS que vous utilisez lors de l’installation d’autres rôles de serveur. Pour les utilisateurs de Windows PowerShell, les applets de commande de déploiement AD DS fournissent plus de fonctionnalités et de flexibilité. Il existe une parité fonctionnelle entre la ligne de commande et les options d’installation graphique.  
  
-   **La nouvelle installation des services AD DS inclut la validation des conditions préalables.** Les erreurs potentielles sont identifiées avant le début de l’installation. Vous pouvez corriger les conditions d’erreur avant qu’ils se produisent et éviter ainsi les problèmes résultant d’une mise à niveau partiellement terminée. Par exemple, si la commande adprep /domainprep doit être exécutée, l’Assistant installation vérifie que l’utilisateur dispose de droits suffisants pour exécuter l’opération.  
  
-   **Pages de configuration sont regroupées en une séquence qui reflète les exigences des options de promotion plus courantes avec des options associées regroupées dans moins de pages Assistant.** Cela offre un meilleur contexte pour faire des choix de l’installation.  
  
-   **Vous pouvez exporter un script Windows PowerShell qui contient toutes les options qui ont été spécifiées pendant l’installation graphique.** À la fin d’une installation ou la suppression, vous pouvez exporter les paramètres vers un script Windows PowerShell pour automatiser la même opération.  
  
-   **Seule la réplication critique se produit avant le redémarrage.** Nouveau commutateur permettant la réplication des données non critiques avant le redémarrage. Pour plus d’informations, voir [arguments de l’applet de commande ADDSDeployment](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params).  
  
### <a name="BKMK_ADConfigurationWizard"></a>L’Assistant de Configuration de Services de domaine Active Directory  
À compter de Windows Server 2012, l’Assistant Configuration des Services de domaine Active Directory remplace le hérités Active Directory domaine Assistant Installation des Services comme option d’interface utilisateur pour spécifier les paramètres lorsque vous installez un contrôleur de domaine. L’Assistant Configuration des Services de domaine Active Directory commence une fois l’Assistant Ajout de rôles terminé.  
  
> [!WARNING]  
> Le hérités Active Directory domaine Assistant Installation des Services (dcpromo.exe) est déconseillé à compter de Windows Server 2012.  
  
Dans [installer les Services de domaine Active Directory & #40; Niveau 100 & #41; ](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md), les procédures d’interface utilisateur montrent comment démarrer l’Assistant Ajout de rôles pour installer le serveur AD DS binaires du rôle, puis exécutez l’Assistant Configuration des Services de domaine Active Directory pour terminer l’installation du contrôleur de domaine. Les exemples Windows PowerShell montrent comment effectuer les deux étapes à l’aide d’une applet de commande de déploiement AD DS.  
  
### <a name="BKMK_NewAdprep"></a>Intégration d’Adprep.exe  
À compter de Windows Server 2012, il n'existe qu’une seule version d’Adprep.exe (il n’existe aucune version 32 bits, adprep32.exe). Commandes adprep sont exécutées automatiquement en fonction des besoins lorsque vous installez un contrôleur de domaine qui exécute Windows Server 2012 à un domaine Active Directory existant ou une forêt.  
  
Bien que les opérations adprep soient exécutées automatiquement, vous pouvez exécuter Adprep.exe séparément. Par exemple, si l’utilisateur qui installe les services AD DS n’est pas membre du groupe Administrateurs de l’entreprise, qui est requis pour exécuter Adprep /forestprep, vous devrez peut-être exécuter la commande séparément. Mais, vous devez uniquement exécuter adprep.exe si vous envisagez de mise à niveau sur place votre premier contrôleur de domaine Windows Server 2012 (en d’autres termes, vous avez l’intention sur place mettre à niveau le système d’exploitation d’un contrôleur de domaine qui exécute Windows Server 2012).  
  
Adprep.exe se trouve dans le dossier \support\adprep du disque d’installation Windows Server2012. La version de Windows Server2012 d’adprep est capable d’exécuter à distance.  
  
La version de Windows Server 2012 d’adprep.exe peut s’exécuter sur n’importe quel serveur qui exécute une version 64 bits de Windows Server 2008 ou version ultérieure. Le serveur a besoin d’une connectivité réseau avec le contrôleur de schéma pour la forêt et le maître d’infrastructure du domaine dans lequel vous souhaitez ajouter un contrôleur de domaine. Si une de ces rôles est hébergé sur un serveur qui exécute Windows Server 2003, adprep doit être exécutée à distance. Le serveur sur lequel vous exécutez adprep n’a pas besoin être un contrôleur de domaine. Il peut être domaine joint ou dans un groupe de travail.  
  
> [!NOTE]  
> Si vous essayez d’exécuter la version de Windows Server 2012 d’adprep.exe sur un serveur qui exécute Windows Server 2003, l’erreur suivante s’affiche:  
>   
> Adprep.exe n’est pas une application Win32 valide.  
  
![Quelles sont les nouveautés](media/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal/AdprepNotValid.gif)  
  
Pour plus d’informations sur la résolution d’autres erreurs retournées par Adprep.exe, voir [problèmes connus](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues).  
  
#### <a name="group-membership-check-against-windows-server-2003-operations-master-roles"></a>Vérification de l’appartenance au groupe contre les rôles de maître d’opérations Windows Server 2003  
Pour chaque commande (/ forestprep, /domainprep ou /rodcprep), Adprep effectue une vérification de l’appartenance au groupe pour déterminer si les informations d’identification spécifiée représentent un compte dans certains groupes. Pour effectuer cette vérification, Adprep contacte le propriétaire de rôle de maître d’opérations. Si le maître d’opérations exécute Windows Server 2003, vous devez spécifier les paramètres /user et /userdomain les paramètres de ligne de commande si vous exécutez Adprep.exe pour garantir que la vérification de l’appartenance au groupe est effectuée dans tous les cas.  
  
/User et /userdomain sont de nouveaux paramètres pour Adprep.exe dans Windows Server 2012. Ces paramètres spécifient respectivement le nom de compte d’utilisateur et le domaine de l’utilisateur qui exécute la commande adprep. L’utilitaire de ligne de commande Adprep.exe bloque spécifier qu’un seul des /userdomain et /user mais en omettant l’autre.  
  
Toutefois, les opérations Adprep peuvent également être exécutées dans le cadre d’une installation d’AD DS à l’aide de Windows PowerShell ou le Gestionnaire de serveur. Ces expériences partagent la même implémentation sous-jacente (adprep.dll) qu’adprep.exe. Les expériences de Windows PowerShell et le Gestionnaire de serveur ont leurs informations d’identification distinctes d’entrée, qui n’impose pas les mêmes exigences que par adprep.exe. À l’aide de Windows PowerShell ou le Gestionnaire de serveur, il est possible de passer une valeur pour /user mais pas /userdomain à adprep.dll. Si /user est spécifié, mais que /userdomain n’est pas spécifié, le domaine de l’ordinateur local est utilisé pour effectuer la vérification. Si l’ordinateur n’est pas joint au domaine, l’appartenance au groupe ne peut pas être vérifiée.  
  
Lorsque l’appartenance au groupe ne peut pas être vérifiée, Adprep affiche un message d’avertissement dans les fichiers journaux adprep et continue:  
  
```  
Adprep was unable to check the specified user's group membership. This could happen if the FSMO role owner <DNS host name of operations master> is running Windows Server 2003 or lower version of Windows.  
```  
  
Si vous exécutez Adprep.exe sans spécifier les paramètres /user et /UserDomain et que le maître d’opérations exécute Windows Server 2003, Adprep.exe contacte un contrôleur de domaine dans le domaine de l’utilisateur d’ouverture de session actuel. Si l’utilisateur d’ouverture de session actuel n’est pas un compte de domaine, Adprep.exe ne peut pas effectuer la vérification de l’appartenance au groupe. Adprep.exe également ne peut pas effectuer la vérification de l’appartenance au groupe si les informations d’identification de carte à puce sont utilisées, même si vous spécifiez /user et /userdomain.  
  
Si Adprep se termine correctement, aucune action n’est requise. Si Adprep échoue en cours d’exécution avec des erreurs d’accès, spécifiez un compte avec appartenance correcte. Pour plus d’informations, voir [des informations d’identification requises pour exécuter Adprep.exe et installer les Services de domaine ActiveDirectory](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
#### <a name="syntax-for-adprep-in-windows-server-2012"></a>Syntaxe pour Adprep dans Windows Server 2012  
Utilisez la syntaxe suivante pour exécuter adprep séparément d’une installation d’AD DS:  
  
```  
Adprep.exe /forestprep /forest <forest name> /userdomain <user domain name> /user <user name> /password *  
```  
  
Utilisez /logdsid dans la commande afin de générer une journalisation plus détaillée. Le journal adprep.log se trouve dans % windir%\System32\Debug\Adprep\Logs.  
  
#### <a name="running-adprep-using-smartcard"></a>Exécution d’adprep à l’aide de la carte à puce  
La version de Windows Server 2012 d’adprep.exe fonctionne à l’aide de la carte à puce en tant qu’informations d’identification, mais il n’existe aucun moyen simple pour spécifier les informations d’identification de carte à puce par le biais de la ligne de commande. Une façon de le faire consiste à obtenir les informations d’identification de carte à puce via l’applet de commande PowerShell Get-Credential. Puis utilisez le nom d’utilisateur de l’objet PSCredential retourné, qui apparaît sous la forme `@@...`. Le mot de passe est le code confidentiel de la carte à puce.  
  
Adprep.exe requiert /userdomain si /user est spécifié. Informations d’identification de carte à puce, /userdomain doit être le domaine du compte d’utilisateur sous-jacent représenté par la carte à puce.  
  
#### <a name="adprep-domainprep-gpprep-command-is-not-run-automatically"></a>Commande adprep /domainprep /gpprep n’est pas exécutée automatiquement  
La commande adprep /domainprep /gpprep n’est pas exécutée dans le cadre de l’installation des services AD DS. Cette commande définit les autorisations qui sont requises pour jeu de stratégie résultant (RSOP) en mode de planification. Pour plus d’informations sur cette commande, voir [l’article 324392 de la Base de connaissances Microsoft](https://support.microsoft.com/kb/324392). Si la commande doit être exécutée dans votre domaine Active Directory, vous pouvez l’exécuter séparément de l’installation des services AD DS. Si la commande a déjà été exécutée en préparation du déploiement de contrôleurs de domaine qui exécutent Windows Server 2003 SP1 ou version ultérieure, la commande est inutile de réexécuter.  
  
Vous pouvez ajouter en toute sécurité des contrôleurs de domaine qui exécutent Windows Server 2012 à un domaine existant sans en cours d’exécution d’adprep /domainprep /gpprep, mais le mode de planification RSOP ne fonctionnera pas correctement.  
  
### <a name="BKMK_PrereqCheck"></a>Validation des conditions préalables AD DS installation  
L’Assistant d’installation de domaine Active Directory vérifie que les conditions préalables suivantes sont remplies avant le début de l’installation. Cela vous donne une chance de corriger les problèmes susceptibles de bloquer l’installation.  
  
Par exemple, liées à Adprep les conditions préalables sont les suivantes:  
  
-   Vérification des informations d’identification Adprep: Si adprep doit être exécutée, l’Assistant installation vérifie que l’utilisateur dispose de droits suffisants pour exécuter les opérations Adprep requises.  
  
-   Contrôle de disponibilité du contrôleur de schéma: si l’Assistant installation détermine qu’adprep /forestprep doit être exécutée, il vérifie que le contrôleur de schéma est en ligne échoue dans le cas contraire.  
  
-   Vérification de disponibilité du maître d’infrastructure: si l’Assistant installation détermine que la commande adprep /domainprep doit être exécutée, il vérifie que le maître d’infrastructure est en ligne échoue dans le cas contraire.  
  
Autres vérifications sont issues de l’Assistant de Installation d’Active Directory hérité (dcpromo.exe) sont les suivantes:  
  
-   Vérification du nom de la forêt: vérifie que le nom de la forêt est valide et qu’il n’existe pas actuellement.  
  
-   Vérification du nom NetBIOS: vérifie que fourni le nom NetBIOS est valide et qu’il n’est pas en conflit avec des noms existants.  
  
-   Vérification de chemin d’accès des composants: vérifie que les chemins d’accès pour la base de données Active Directory, les journaux et SYSVOL sont valides et que suffisamment d’espace disque est disponible pour eux.  
  
-   Vérification du nom du domaine enfant: vérifie que le parent et les nouveaux noms de domaine enfant sont valides et qu’ils ne portent pas des domaines existants.  
  
-   Vérification du nom du domaine d’arborescence: vérifie que le nom de l’arborescence spécifié est valide et qu’il n’existe pas actuellement.  
  
## <a name="BKMK_SystemReqs"></a>Configuration système requise  
Configuration système requise pour Windows Server 2012 est identiques à Windows Server 2008 R2. Pour plus d’informations, voir [Windows Server2008R2 avec SP1 requise](https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx) (https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx).  
  
Certaines fonctionnalités peuvent avoir des exigences supplémentaires. Par exemple, la fonctionnalité de clonage de contrôleur de domaine virtuel requiert que l’émulateur de contrôleur de domaine principal exécutant Windows Server 2012 et un ordinateur exécutant Windows Server 2012 avec le rôle Hyper-V installé.  
  
## <a name="BKMK_KnownIssues"></a>Problèmes connus  
Cette section répertorie certains problèmes connus qui affectent l’installation des services AD DS dans Windows Server 2012. Pour plus d’autres problèmes connus, consultez [Troubleshooting Domain Controller Deployment](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md).  
  
-   Si l’accès WMI au contrôleur de schéma est bloqué par le pare-feu Windows lorsque vous exécutez à distance adprep /forestprep, l’erreur suivante est enregistrée dans le journal adprep à systemroot%\system32\debug\adprep:  
  
    ```  
    Adprep encountered a Win32 error.   
    Error code: 0x6ba Error message: The RPC server is unavailable.  
    ```  
  
    Dans ce cas, vous pouvez contourner cette erreur en soit exécuter adprep /forestprep directement sur le contrôleur de schéma, ou vous pouvez exécuter une des commandes suivantes pour autoriser le trafic WMI via le pare-feu Windows.  
  
    Pour Windows Server 2008 ou version ultérieure:  
  
    ```  
    netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes  
    ```  
  
    Pour Windows Server 2003:  
  
    ```  
    netsh firewall set service RemoteAdmin enable  
    ```  
  
    Une fois adprep terminé, vous pouvez exécuter une des commandes suivantes pour bloquer le trafic WMI à nouveau:  
  
    ```  
    netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=no  
    ```  
  
    ```  
    netsh firewall set service remoteadmin disable  
    ```  
  
-   Vous pouvez taper Ctrl + C pour annuler l’applet de commande Install-ADDSForest. L’annulation arrête l’installation et toutes les modifications qui ont été apportées à l’état du serveur sont annulées. Toutefois, une fois la commande d’annulation est émise, contrôle n’est pas retourné à Windows PowerShell et l’applet de commande peut se bloquer indéfiniment.  
  
-   **Installation d’un contrôleur de domaine supplémentaire à l’aide des informations d’identification de carte à puce échoue si le serveur cible n’est pas joint au domaine avant l’installation.**  
  
    Le message d’erreur retourné est dans ce cas:  
  
    Impossible de se connecter au contrôleur de domaine source réplication *nom de contrôleur de domaine source*. (Exception: Logonfailure: nom d’utilisateur inconnu ou mot de passe incorrect)  
  
    Si vous joignez le serveur cible au domaine et puis effectuez l’installation à l’aide d’une carte à puce, l’installation réussit.  
  
-   **Le module ADDSDeployment ne s’exécute pas sous les processus 32 bits.** Si vous automatisez le déploiement et la configuration de Windows Server 2012 à l’aide d’un script qui inclut une applet de commande ADDSDeployment et toute autre applet de commande qui ne prend pas en charge les processus 64 bits natifs, le script peut échouer avec une erreur indiquant que l’applet de commande ADDSDeployment est introuvable.  
  
    Dans ce cas, vous devez exécuter l’applet de commande ADDSDeployment séparément à partir de l’applet de commande qui ne prend pas en charge les processus 64 bits natifs.  
  
-   Il existe un nouveau système de fichiers dans Windows Server 2012 nommé Resilient File System. Ne stockez pas la base de données Active Directory, les fichiers journaux ou SYSVOL sur un volume de données formaté avec le système de fichiers résilient (ReFS). Pour plus d’informations sur ReFS, voir [création du système de fichiers prochaine génération pour Windows: ReFS](http://blogs.msdn.com/b/b8/archive/2012/01/16/building-the-next-generation-file-system-for-windows-refs.aspx).  
  
-   Dans le Gestionnaire de serveur, les serveurs qui exécutent des services AD DS ou autres rôles de serveur sur une installation Server Core et ont été mis à niveau vers Windows Server 2012, le rôle de serveur peut apparaître avec l’état rouge, même si les événements et l’état sont collectés comme prévu. Serveurs qui exécutent une installation minimale d’une version préliminaire que Windows Server 2012 peuvent également être touchés.  
  
### <a name="active-directory-domain-services-installation-hangs-if-an-error-prevents-critical-replication"></a>L’installation des Services de domaine Active Directory se bloque si une erreur empêche la réplication critique  
Si l’installation des services AD DS rencontre une erreur lors de la phase de la réplication critique, l’installation peut se bloquer indéfiniment. Par exemple, si les erreurs réseau empêchent l’achèvement de la réplication critique, l’installation ne continue pas.  
  
Si vous installez à l’aide du Gestionnaire de serveur, vous pouvez voir la page de progression de l’installation reste ouverte, mais il n’existe aucune erreur signalée à l’écran et la progression reste inchangée pendant environ 15 minutes. Si vous utilisez Windows PowerShell, la progression affichée dans la fenêtre Windows PowerShell ne changera pas pendant plus de 15 minutes.  
  
Si vous rencontrez ce problème, vérifiez le fichier dcpromo.log dans le dossier %systemroot%\debug sur le serveur cible. Le fichier journal indiquent des échecs répétés à répliquer. Certaines causes connues de ce problème sont:  
  
-   Des problèmes réseau empêchent la réplication critique entre le serveur cible qui est promu et le contrôleur de domaine de source de réplication.  
  
    Par exemple, le fichier dcpromo.log affiche:  
  
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
  
    Étant donné que le processus d’installation retente la réplication critique indéfiniment, l’installation du contrôleur de domaine se poursuit si les problèmes réseau sous-jacents sont résolus. Examiner le problème de mise en réseau à l’aide d’outils tels qu’ipconfig, nslookup et netmon en fonction des besoins. Garantir la connectivité entre le contrôleur de domaine que vous promouvez et le partenaire de réplication sélectionné pendant l’installation des services AD DS. Veillez également à la résolution de noms fonctionne.  
  
    Exigences d’installation AD DS pour la résolution de nom et de connectivité réseau sont validées lors de la vérification de la configuration requise avant le début de l’installation. Toutefois, certaines conditions d’erreur peuvent se produire dans le temps après validation préalable et avant la fin de l’installation, par exemple si le partenaire de réplication devient indisponible pendant l’installation.  
  
-   Pendant l’installation du contrôleur de domaine réplica, le compte administrateur local du serveur cible est spécifié pour les informations d’identification de l’installation et le mot de passe du compte d’administrateur local correspond au mot de passe d’un compte d’administrateur de domaine. Dans ce cas, vous pouvez exécuter l’Assistant installation et commencer l’installation avant que vous rencontrez l’erreur «Accès refusé».  
  
    Par exemple, le fichier dcpromo.log affiche:  
  
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
  
    Si l’erreur est provoquée par la spécification d’un compte d’administrateur local et le mot de passe, afin de récupérer les vous devez réinstaller le système d’exploitation, [effectuer un nettoyage des métadonnées](https://technet.microsoft.com/library/cc816907(WS.10).aspx) du compte du contrôleur de domaine qui n’a pas pu terminer l’installation et relancez l’installation d’AD DS à l’aide des informations d’identification d’administrateur de domaine. Le redémarrage du serveur ne corrige pas cette condition d’erreur, car le serveur indique que les services AD DS est installé même si l’installation ne s’est pas terminée correctement.  
  
### <a name="BKMK_nonnormalDNSNameWarning"></a>Assistant Configuration de Services de domaine Active Directory vous avertit lorsqu’un nom DNS non normalisé est spécifié.  
Si vous créez un nouveau domaine ou forêt et que vous spécifiez un nom de domaine DNS qui inclut des caractères internationaux qui ne sont pas normalisés, l’Assistant Configuration des Services de domaine Active Directory affiche un avertissement indiquant que les requêtes DNS pour le nom peuvent échouer. Bien que le nom de domaine DNS est spécifié dans la page de Configuration de déploiement, l’avertissement apparaît dans la page Vérification de la configuration requise plus loin dans l’Assistant.  
  
Si un nom de domaine DNS est spécifié à l’aide d’un nom non normalisé tels que .com füßball.com ou «ΣΤ» (les versions normalisées sont: füssball.com et βστα.com), les applications clientes qui tentent d’accéder avec WinHTTP normalise le nom avant d’appeler des API de résolution de nom. Si l’utilisateur tape «.com 'ΣΤ'» dans une boîte de dialogue, la requête DNS est envoyée comme «βστα.com» et aucun serveur DNS ne seront en correspondance avec un enregistrement de ressource pour «.com 'ΣΤ'». L’utilisateur ne pourra pas résoudre le nom.  
  
L’exemple suivant explique un des problèmes qui peuvent se produire lorsque vous utilisez un nom IDN qui n’est pas normalisé:  
  
1.  Le domaine à l’aide d’un nom non normalisé est créé et enregistré sur le serveur dns: füßball.com  
  
2.  Ordinateur «nps» est joint au domaine et qu’il obtient son nom enregistré: nps.füßball.com  
  
3.  Une application cliente essaie de se connecter à la nps.füßball.com de serveur  
  
4.  L’application cliente tente de résoudre le nps.füßball.com nom de l’appel d’API de résolution de nom.  
  
5.  En raison de normalisation, le nom converti en nps.füssball.com et est interrogé via le réseau en tant que nps.füßball.com  
  
6.  L’application cliente ne peut pas résoudre le nom dans la mesure où le nom enregistré est nps.füßball.com  
  
Si l’avertissement apparaît dans la page Vérification de la configuration requise dans l’Assistant Configuration des Services de domaine Active Directory, retournez à la page de Configuration de déploiement et spécifiez un nom de domaine DNS normalisé. Si vous installez un nouveau domaine à l’aide de Windows PowerShell, spécifiez un nom DNS normalisé pour l’option - DomainName.  
  
Pour plus d’informations sur les noms IDN, voir [gestion des noms de domaine internationaux (noms de domaines internationaux)](https://msdn.microsoft.com/library/windows/desktop/dd318142(v=vs.85).aspx).  
  

