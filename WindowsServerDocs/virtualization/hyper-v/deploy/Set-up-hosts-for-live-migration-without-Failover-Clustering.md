---
title: Configurer des hôtes pour la migration dynamique sans clustering de basculement
description: Fournit des instructions sur la configuration de la migration dynamique dans un environnement non-cluster
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5e3c405-cb76-4ff2-8042-c2284448c435
author: KBDAzure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: 3f0c13ba44eb498635b9b0c049b2921776049840
ms.sourcegitcommit: 9a6a692a7b2a93f52bb9e2de549753e81d758d28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2019
ms.locfileid: "72591065"
---
# <a name="set-up-hosts-for-live-migration-without-failover-clustering"></a>Configurer des hôtes pour la migration dynamique sans clustering de basculement

>S’applique à : Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Cet article explique comment configurer des ordinateurs hôtes qui ne sont pas en cluster afin de pouvoir effectuer des migrations dynamiques entre eux. Utilisez ces instructions si vous n’avez pas configuré la migration dynamique lorsque vous avez installé Hyper-V ou si vous souhaitez modifier les paramètres. Pour configurer des ordinateurs hôtes en cluster, utilisez outils pour le clustering de basculement.

## <a name="requirements-for-setting-up-live-migration"></a>Conditions requises pour la configuration de la migration dynamique

Pour configurer des ordinateurs hôtes non-cluster pour la migration dynamique, vous avez besoin des éléments suivants :

-  Un compte d’utilisateur autorisé à effectuer les différentes étapes. L’appartenance au groupe administrateurs Hyper-V local ou au groupe Administrateurs sur les ordinateurs source et de destination répond à cette exigence, sauf si vous configurez la délégation contrainte. L’appartenance au groupe administrateurs de domaine est requise pour configurer la délégation avec restriction.

- Le rôle Hyper-V dans Windows Server 2016 ou Windows Server 2012 R2 est installé sur les serveurs source et de destination. Vous pouvez effectuer une migration dynamique entre des ordinateurs hôtes exécutant Windows Server 2016 et Windows Server 2012 R2 si la machine virtuelle est au moins la version 5. <br>Pour obtenir des instructions de mise à niveau de version, consultez [mettre à niveau la version de machine virtuelle dans Hyper-V sur Windows 10 ou Windows Server 2016](Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Pour obtenir des instructions d’installation, consultez [installer le rôle Hyper-V sur Windows Server](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).

- Ordinateurs source et de destination qui appartiennent au même domaine de Active Directory ou qui appartiennent à des domaines qui s’approuvent mutuellement.
- Les outils de gestion Hyper-V installés sur un ordinateur exécutant Windows Server 2016 ou Windows 10, à moins que les outils ne soient installés sur le serveur source ou de destination, et que vous exécutiez les outils à partir du serveur.

## <a name="consider-options-for-authentication-and-networking"></a>Envisagez les options d’authentification et de mise en réseau

Réfléchissez à la façon dont vous souhaitez configurer les éléments suivants :

-  **Authentification**: quel protocole sera utilisé pour authentifier le trafic de migration dynamique entre les serveurs source et de destination ? Le choix détermine si vous devez vous connecter au serveur source avant de démarrer une migration dynamique :
   - Kerberos vous permet d’éviter d’avoir à vous connecter au serveur, mais requiert la configuration de la délégation avec restriction. Pour obtenir des instructions, voir ci-dessous.
   - CredSSP vous permet d’éviter de configurer la délégation avec restriction, mais vous devez vous connecter au serveur source. Vous pouvez le faire via une session de console locale, une session Bureau à distance ou une session Windows PowerShell distante.

      CredSPP nécessite une connexion pour les situations qui peuvent ne pas être évidentes. Par exemple, si vous vous connectez à TestServer01 pour déplacer un ordinateur virtuel vers TestServer02, puis que vous souhaitez déplacer l’ordinateur virtuel vers TestServer01, vous devez vous connecter à TestServer02 avant de réessayer de déplacer la machine virtuelle vers TestServer01. Si vous ne le faites pas, la tentative d’authentification échoue, une erreur se produit et le message suivant s’affiche :

      «Échec de l’opération de migration d’ordinateur virtuel au niveau de la source de migration.
      Échec de l’établissement d’une connexion avec le nom de l' *ordinateur*hôte : aucune information d’identification n’est disponible dans le package de sécurité 0x8009030E.

-   **Performances**: est-il judicieux de configurer les options de performances ? Ces options peuvent réduire l’utilisation du réseau et du processeur, ainsi que accélérer les migrations dynamiques. Prenez en compte vos besoins et votre infrastructure, et testez différentes configurations pour vous aider à décider. Les options sont décrites à la fin de l’étape 2.

-  **Préférences réseau**: allez-vous autoriser le trafic de migration dynamique via tout réseau disponible ou isoler le trafic vers des réseaux spécifiques ? Pour des raisons de sécurité, il est conseillé d’isoler le trafic sur des réseaux privés approuvés, car le trafic de migration dynamique n’est pas chiffré lorsqu’il est envoyé sur le réseau. L’isolation du réseau peut être obtenue via un réseau isolé physiquement ou une autre technologie de mise en réseau approuvée comme les réseaux locaux virtuels.

## <a name="BKMK_Step1"></a>Étape 1 : configurer la délégation avec restriction (facultatif)
Si vous avez décidé d’utiliser Kerberos pour authentifier le trafic de migration dynamique, configurez la délégation avec restriction à l’aide d’un compte membre du groupe administrateurs de domaine.

### <a name="use-the-users-and-computers-snap-in-to-configure-constrained-delegation"></a>Utiliser le composant logiciel enfichable utilisateurs et ordinateurs pour configurer la délégation avec restriction

1.  Ouvrez le composant logiciel enfichable Utilisateurs et ordinateurs Active Directory. (Dans Gestionnaire de serveur, sélectionnez le serveur s’il n’est pas sélectionné, cliquez sur **outils**  >> **Active Directory utilisateurs et ordinateurs**).

2.  Dans le volet de navigation de **Active Directory utilisateurs et ordinateurs**, sélectionnez le domaine et double-cliquez sur le dossier **ordinateurs** .

3.  Dans le dossier **ordinateurs** , cliquez avec le bouton droit sur le compte d’ordinateur du serveur source, puis cliquez sur **Propriétés**.

4.  Dans **Propriétés**, cliquez sur l’onglet **délégation** .

5.  Sous l’onglet délégation, sélectionnez n' **approuver cet ordinateur que pour la délégation aux services spécifiés** , puis sélectionnez **utiliser n’importe quel protocole d’authentification**.

6.  Cliquez sur **Ajouter**.

7.  Dans **Ajouter des services**, cliquez sur **utilisateurs ou ordinateurs**.

8.  Dans **Sélectionner des utilisateurs ou des ordinateurs**, tapez le nom du serveur de destination. Cliquez sur **vérifier les noms** pour le vérifier, puis cliquez sur **OK**.

9. Dans **Ajouter des services**, dans la liste des services disponibles, effectuez les opérations suivantes, puis cliquez sur **OK**:

    -   Pour déplacer le stockage de l’ordinateur virtuel, sélectionnez **cifs**. Cela est nécessaire si vous souhaitez déplacer le stockage avec l’ordinateur virtuel, ainsi que si vous souhaitez déplacer uniquement le stockage d’une machine virtuelle. Si le serveur est configuré pour utiliser le stockage SMB pour Hyper-V, cela doit être déjà sélectionné.

    -   Pour déplacer les ordinateurs virtuels, sélectionnez **Service de migration de système virtuel Microsoft**.

10. Dans l’onglet **Délégation** de la boîte de dialogue Propriétés, vérifiez que les services que vous avez sélectionnés à l’étape précédente sont répertoriés comme les services auxquels l’ordinateur de destination peut présenter les informations d’identification déléguées. Cliquez sur **OK**.

11. Dans le dossier **Computers** , sélectionnez le compte d’ordinateur du serveur de destination et répétez le processus. Dans la boîte de dialogue **Sélectionner des utilisateurs ou des ordinateurs**, veillez à spécifier le nom du serveur source.

Les modifications de configuration prennent effet après les deux opérations suivantes :

  -  Les modifications sont répliquées sur les contrôleurs de domaine sur lesquels les serveurs exécutant Hyper-V sont connectés.
  -  Le contrôleur de domaine émet un nouveau ticket Kerberos.

## <a name="BKMK_Step2"></a>Étape 2 : configurer les ordinateurs source et de destination pour la migration dynamique
Cette étape consiste à choisir les options d’authentification et de mise en réseau. En guise de meilleure pratique de sécurité, nous vous recommandons de sélectionner des réseaux spécifiques à utiliser pour le trafic de migration dynamique, comme indiqué ci-dessus. Cette étape vous montre également comment choisir l’option de performance.

### <a name="use-hyper-v-manager-to-set-up-the-source-and-destination-computers-for-live-migration"></a>Utiliser le Gestionnaire Hyper-V pour configurer les ordinateurs source et de destination pour la migration dynamique

1.  Ouvrez le Gestionnaire Hyper-V. (À partir de Gestionnaire de serveur, cliquez sur **outils**  >>**Gestionnaire Hyper-V**.)

2.  Dans le volet de navigation, sélectionnez l’un des serveurs. (S’il n’est pas listé, cliquez avec le bouton droit sur **Gestionnaire Hyper-V**, cliquez sur **se connecter au serveur**, tapez le nom du serveur, puis cliquez sur **OK**. Répétez l’opération pour ajouter d’autres serveurs.)

3.  Dans le volet **action** , cliquez sur **paramètres Hyper-V**  >> des**migrations dynamiques**.

4.  Dans le volet **Migrations dynamiques** , sélectionnez **Activer les migrations dynamiques entrantes et sortantes**.

5.  Sous **migrations dynamiques simultanées**, spécifiez un autre numéro si vous ne souhaitez pas utiliser la valeur par défaut de 2.

6.  Sous **Migrations dynamiques entrantes**, si vous voulez utiliser des connexions réseau spécifiques pour accepter le trafic de migration dynamique, cliquez sur **Ajouter** pour taper les informations d’adresse IP. Dans le cas contraire, cliquez sur **Utiliser n’importe quel réseau disponible pour la migration dynamique**. Cliquez sur **OK**.

7.  Pour choisir Kerberos et les options de performances, développez **migrations dynamiques** , puis sélectionnez **fonctionnalités avancées**.

    - Si vous avez configuré la délégation avec restriction, sous **protocole d’authentification**, sélectionnez **Kerberos**.
    - Sous **options de performances**, passez en revue les détails et choisissez une autre option si elle convient à votre environnement.

8. Cliquez sur **OK**.

9. Sélectionnez l’autre serveur dans le Gestionnaire Hyper-V et répétez les étapes.

### <a name="use-windows-powershell-to-set-up-the-source-and-destination-computers-for-live-migration"></a>Utiliser Windows PowerShell pour configurer les ordinateurs source et de destination pour la migration dynamique

Trois applets de commande sont disponibles pour la configuration de la migration dynamique sur des ordinateurs hôtes non-cluster : [Enable-VMMigration](https://technet.microsoft.com/library/hh848544.aspx), [Set-VMMigrationNetwork](https://technet.microsoft.com/library/hh848467.aspx)et [Set-vmhost](https://technet.microsoft.com/library/hh848524.aspx). Cet exemple utilise les trois et effectue les opérations suivantes :
  - Configure la migration dynamique sur l’hôte local
  - Autorise le trafic de migration entrant uniquement sur un réseau spécifique
  - Choisit Kerberos comme protocole d’authentification

Chaque ligne représente une commande distincte.

```PowerShell
PS C:\> Enable-VMMigration

PS C:\> Set-VMMigrationNetwork 192.168.10.1

PS C:\> Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos
```

Set-VMHost vous permet également de choisir une option de performance (et de nombreux autres paramètres d’hôte). Par exemple, pour choisir SMB mais conserver le protocole d’authentification défini sur la valeur par défaut CredSSP, tapez :

```PowerShell
PS C:\> Set-VMHost -VirtualMachineMigrationPerformanceOption SMB
```

Ce tableau décrit le fonctionnement des options de performances.

|Option|Description|
|----------|---------------|
    |TCP/IP|Copie la mémoire de l’ordinateur virtuel sur le serveur de destination via une connexion TCP/IP.|
    |compression ;|Compresse le contenu de la mémoire de l’ordinateur virtuel avant de le copier sur le serveur de destination via une connexion TCP/IP. **Remarque :** Il s’agit du paramètre **par défaut** .|
    |SMB|Copie la mémoire de l’ordinateur virtuel sur le serveur de destination via une connexion SMB 3,0.<br /><br />-SMB direct est utilisé lorsque les fonctionnalités d’accès direct à la mémoire à distance (RDMA) sont activées pour les cartes réseau sur les serveurs source et de destination.<br />-SMB Multichannel détecte et utilise automatiquement plusieurs connexions quand une configuration SMB Multichannel appropriée est identifiée.<br /><br />Pour plus d’informations, voir [Améliorer les performances d’un serveur de fichiers avec SMB Direct](https://technet.microsoft.com/library/jj134210(WS.11).aspx).|

 ## <a name="next-steps"></a>Étapes suivantes

 Une fois les ordinateurs hôtes configurés, vous êtes prêt à effectuer une migration dynamique. Pour obtenir des instructions, consultez [utiliser la migration dynamique sans clustering de basculement pour déplacer un ordinateur virtuel](../manage/Use-live-migration-without-Failover-Clustering-to-move-a-virtual-machine.md).
