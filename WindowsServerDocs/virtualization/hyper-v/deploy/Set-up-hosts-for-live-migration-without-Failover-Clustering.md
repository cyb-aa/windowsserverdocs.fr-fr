---
title: Configurer des hôtes pour la migration dynamique sans Clustering de basculement
description: Fournit des instructions pour la configuration d’une migration dynamique dans un environnement non cluster
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5e3c405-cb76-4ff2-8042-c2284448c435
author: KBDAzure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: 49e36d7fae5eec07772fd6f82c5a5d69838351d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821990"
---
# <a name="set-up-hosts-for-live-migration-without-failover-clustering"></a>Configurer des hôtes pour la migration dynamique sans Clustering de basculement

>S'applique à : Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Cet article vous montre comment configurer des hôtes qui ne sont pas en cluster afin de vous pouvez migrations en direct entre eux. Utilisez ces instructions si vous n’avez pas défini la migration dynamique lorsque vous avez installé Hyper-V, ou si vous souhaitez modifier les paramètres. Pour configurer les ordinateurs hôtes en cluster, utilisez les outils pour le Clustering de basculement.  
  
## <a name="requirements-for-setting-up-live-migration"></a>Conditions requises pour configurer la migration dynamique  
  
Pour configurer les hôtes non cluster pour la migration dynamique, vous devez :   
  
-  Un compte d’utilisateur avec l’autorisation d’effectuer les différentes étapes. L’appartenance au groupe local Administrateurs Hyper-V ou du groupe Administrateurs sur les ordinateurs source et de destination répond à cette exigence, sauf si vous configurez la délégation contrainte. L’appartenance au groupe Administrateurs du domaine est requis pour configurer la délégation contrainte.  
  
- Le rôle Hyper-V dans Windows Server 2016 ou Windows Server 2012 R2 est installé sur les serveurs source et de destination. Vous pouvez effectuer une migration dynamique entre les ordinateurs hôtes exécutant Windows Server 2016 et Windows Server 2012 R2, si la machine virtuelle est au moins la version 5. <br>Pour obtenir des instructions de mise à niveau de version, consultez [version de mise à niveau machine virtuelle dans Hyper-V sur Windows 10 ou Windows Server 2016](Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Pour obtenir des instructions d’installation, consultez [installer le rôle Hyper-V sur Windows Server](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  
  
- Ordinateurs source et de destination qui appartiennent au même domaine Active Directory, ou appartenant à des domaines qui approuvent mutuellement.    
- Les outils de gestion Hyper-V installés sur un ordinateur exécutant Windows Server 2016 ou Windows 10, à moins que les outils sont installés sur le serveur source ou destination et que vous allez exécuter les outils à partir du serveur.  
  
## <a name="consider-options-for-authentication-and-networking"></a>Envisagez les options pour l’authentification et de mise en réseau  
  
Prenez en compte la façon dont vous souhaitez configurer les éléments suivants :  
  
-  **Authentification**: Le protocole est utilisé pour authentifier le trafic de migration en direct entre les serveurs source et de destination ? Le choix détermine si vous devez vous connecter au serveur source avant de commencer une migration dynamique :   
   - Kerberos permet d’éviter d’avoir à se connecter au serveur, mais requiert une délégation à configurer. Pour obtenir des instructions, voir ci-dessous.  
   - CredSSP vous permet d’éviter de configurer la délégation contrainte, mais nécessite que vous vous connectez au serveur source. Pour cela, via une session de console locale, une session Bureau à distance ou une session Windows PowerShell à distance.  
  
      CredSPP requiert une connexion pour les situations qui peuvent ne pas être évidentes. Par exemple, si vous vous connectez à TestServer01 pour déplacer un ordinateur virtuel vers TestServer02 et que vous voulez ensuite ramener l’ordinateur virtuel vers TestServer01, vous devrez vous connectiez à TestServer02 avant d’essayer de ramener l’ordinateur virtuel vers TestServer01. Si vous ne le faites pas, la tentative d’authentification échoue, une erreur se produit, et le message suivant s’affiche :  
    
      « Échec de l’opération de migration de machine virtuelle à la Source de la migration.  
      Impossible d’établir une connexion avec l’hôte *nom de l’ordinateur*: Aucune information d’identification n’est disponibles dans le package de sécurité 0x8009030E. »
  
-   **Performances**: Il est judicieux de configurer les options de performances ? Ces options peuvent réduire l’utilisation du processeur et réseau, mais aussi rendre les migrations dynamiques accéder plus rapidement. Prendre en compte vos besoins et votre infrastructure et des configurations différentes pour vous aider à décider de test. Les options sont décrites à la fin de l’étape 2.  
  
-  **Réseau de préférence**: Souhaitez-vous autoriser le trafic de migration dynamique à traverser tout réseau disponible ou voulez-vous isoler le trafic dans des réseaux spécifiques ? Pour des raisons de sécurité, il est conseillé d’isoler le trafic sur des réseaux privés approuvés, car le trafic de migration dynamique n’est pas chiffré lorsqu’il est envoyé sur le réseau. L’isolation du réseau peut être obtenue via un réseau isolé physiquement ou une autre technologie de mise en réseau approuvée comme les réseaux locaux virtuels.  
  
## <a name="BKMK_Step1"></a>Étape 1 : Configurer la délégation contrainte (facultatif)  
Si vous avez décidé d’utiliser Kerberos pour authentifier le trafic de migration en direct, configurer la délégation contrainte à l’aide d’un compte qui est membre du groupe Administrateurs de domaine.  
  
### <a name="use-the-users-and-computers-snap-in-to-configure-constrained-delegation"></a>Utilisez le composant logiciel enfichable Utilisateurs et ordinateurs pour configurer la délégation contrainte  
  
1.  Ouvrez le composant logiciel enfichable Utilisateurs et ordinateurs Active Directory. (Dans le Gestionnaire de serveur, sélectionnez le serveur si elle n’est pas sélectionnée, cliquez sur **outils** >> **Active Directory Users and Computers**).  
  
2.  Dans le volet de navigation de **Active Directory Users and Computers**, sélectionnez le domaine et double-cliquez sur le **ordinateurs** dossier.  
  
3.  À partir de la **ordinateurs** dossier, cliquez sur le compte d’ordinateur du serveur source, puis cliquez sur **propriétés**.  
  
4.  À partir de **propriétés**, cliquez sur le **délégation** onglet.  
  
5.  Sous l’onglet Délégation, sélectionnez **n’approuver cet ordinateur pour la délégation aux services spécifiés uniquement** , puis sélectionnez **utiliser tout protocole d’authentification**.  
  
6.  Cliquez sur **Ajouter**.  
  
7.  À partir de **ajouter des Services**, cliquez sur **utilisateurs ou ordinateurs**.  
  
8.  À partir de **sélectionner des utilisateurs ou ordinateurs**, tapez le nom du serveur de destination. Cliquez sur **vérifier les noms** à vérifier, puis cliquez sur **OK**.  
  
9. À partir de **ajouter des Services**, dans la liste des services disponibles, procédez comme suit, puis sur **OK**:  
  
    -   Pour déplacer le stockage de l’ordinateur virtuel, sélectionnez **cifs**. Cela est nécessaire si vous souhaitez déplacer le stockage, ainsi que la machine virtuelle, ainsi que si vous souhaitez déplacer uniquement un stockage d’ordinateur virtuel. Si le serveur est configuré pour utiliser le stockage SMB pour Hyper-V, cela doit être déjà sélectionné.  
  
    -   Pour déplacer les ordinateurs virtuels, sélectionnez **Service de migration de système virtuel Microsoft**.  
  
10. Dans l’onglet **Délégation** de la boîte de dialogue Propriétés, vérifiez que les services que vous avez sélectionnés à l’étape précédente sont répertoriés comme les services auxquels l’ordinateur de destination peut présenter les informations d’identification déléguées. Cliquez sur **OK**.  
  
11. Dans le dossier **Computers** , sélectionnez le compte d’ordinateur du serveur de destination et répétez le processus. Dans la boîte de dialogue **Sélectionner des utilisateurs ou des ordinateurs**, veillez à spécifier le nom du serveur source.  
  
Les modifications de configuration prennent effet une fois que les actions suivantes se produisent :  
  
  -  Les modifications sont répliquées sur les contrôleurs de domaine auxquels les serveurs exécutant Hyper-V sont connectés.  
  -  Le contrôleur de domaine émet un nouveau ticket Kerberos.  
  
## <a name="BKMK_Step2"></a>Étape 2 : Configurer les ordinateurs source et de destination pour la migration dynamique  
Cette étape inclut la sélection des options pour l’authentification et de mise en réseau. Comme une meilleure pratique de sécurité, nous vous recommandons de sélectionner des réseaux spécifiques à utiliser pour le trafic de migration en direct, comme indiqué ci-dessus. Cette étape vous montre également comment choisir l’option de performances.   
  
### <a name="use-hyper-v-manager-to-set-up-the-source-and-destination-computers-for-live-migration"></a>Utilisez le Gestionnaire Hyper-V pour configurer les ordinateurs source et de destination pour la migration dynamique  
  
1.  Ouvrez le Gestionnaire Hyper-V. (Dans le Gestionnaire de serveur, cliquez sur **outils** >>**Gestionnaire Hyper-V**.)  
  
2.  Dans le volet de navigation, sélectionnez un des serveurs. (Si elle n’est pas répertoriée, cliquez sur **Gestionnaire Hyper-V**, cliquez sur **se connecter au serveur**, tapez le nom du serveur, cliquez sur **OK**. Répétez l’opération pour ajouter d’autres serveurs.)  
  
3.  Dans le **Action** volet, cliquez sur **paramètres Hyper-V** >>**Migrations dynamiques**.  
  
4.  Dans le volet **Migrations dynamiques** , sélectionnez **Activer les migrations dynamiques entrantes et sortantes**.  
  
5.  Sous **migrations dynamiques simultanées**, spécifiez un chiffre différent si vous ne souhaitez pas utiliser la valeur par défaut de 2.  
  
6.  Sous **Migrations dynamiques entrantes**, si vous voulez utiliser des connexions réseau spécifiques pour accepter le trafic de migration dynamique, cliquez sur **Ajouter** pour taper les informations d’adresse IP. Dans le cas contraire, cliquez sur **Utiliser n’importe quel réseau disponible pour la migration dynamique**. Cliquez sur **OK**.  
  
7.  Pour choisir les options de Kerberos et les performances, développez **Migrations dynamiques** , puis sélectionnez **fonctionnalités avancées**.  
  
    - Si vous avez configuré la délégation contrainte, sous **protocole d’authentification**, sélectionnez **Kerberos**.  
    - Sous **les options de performances**, passez en revue les détails et choisir une autre option, si c’est approprié pour votre environnement.   
  
8. Cliquez sur **OK**.  
  
9. Sélectionnez l’autre serveur dans le Gestionnaire Hyper-V et répétez les étapes.  
  
### <a name="use-windows-powershell-to-set-up-the-source-and-destination-computers-for-live-migration"></a>Utiliser Windows PowerShell pour configurer les ordinateurs source et de destination pour la migration dynamique  
  
Trois applets de commande sont disponibles pour la configuration de la migration dynamique sur des ordinateurs hôtes non cluster : [Enable-VMMigration](https://technet.microsoft.com/library/hh848544.aspx), [Set-VMMigrationNetwork](https://technet.microsoft.com/library/hh848467.aspx), et [Set-VMHost](https://technet.microsoft.com/library/hh848524.aspx). Cet exemple utilise trois et effectue les opérations suivantes :   
  - Configure la migration dynamique sur l’hôte local  
  - Autorise le trafic de migration entrant uniquement sur un réseau spécifique  
  - Choisit Kerberos comme protocole d’authentification   
  
Chaque ligne représente une commande distincte.  
  
```  
PS C:\> Enable-VMMigration  
  
PS C:\> Set-VMMigrationNetwork 192.168.10.1  
  
PS C:\> Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos  
  
```  
Set-VMHost vous permet également de choisir une option de performances (et de nombreux autres paramètres de l’hôte). Par exemple, choisissez SMB tout en conservant le protocole d’authentification a la valeur par défaut CredSSP, tapez :  
  
```  
PS C:\> Set-VMHost -VirtualMachineMigrationPerformanceOption SMBTransport  
  
```  
  
Ce tableau décrit comment les options de performances fonctionnent.  
  
|Option|Description|  
|----------|---------------|  
    |TCP/IP|Copie la mémoire de l’ordinateur virtuel sur le serveur de destination via une connexion TCP/IP.|  
    |compression ;|Compresse le contenu de la mémoire de l’ordinateur virtuel avant de le copier sur le serveur de destination via une connexion TCP/IP. **Remarque :** Il s’agit du **par défaut** paramètre.|  
    |SMB|Copie la mémoire de l’ordinateur virtuel sur le serveur de destination via une connexion SMB 3.0.<br /><br />-SMB Direct est utilisé lorsque les cartes réseau sur les serveurs source et de destination ont des fonctionnalités d’accès de mémoire Direct à distance (RDMA) activées.<br />-SMB Multichannel détecte automatiquement et utilise plusieurs connexions lorsqu’une configuration SMB Multichannel correcte est identifiée.<br /><br />Pour plus d’informations, voir [Améliorer les performances d’un serveur de fichiers avec SMB Direct](https://technet.microsoft.com/library/jj134210(WS.11).aspx).|  
      
 ## <a name="next-steps"></a>Étapes suivantes
 
 Après avoir configuré les ordinateurs hôtes, vous êtes prêt à effectuer une migration en direct. Pour obtenir des instructions, consultez [migration dynamique sans Clustering de basculement permet de déplacer un ordinateur virtuel](../manage/Use-live-migration-without-Failover-Clustering-to-move-a-virtual-machine.md). 
    


