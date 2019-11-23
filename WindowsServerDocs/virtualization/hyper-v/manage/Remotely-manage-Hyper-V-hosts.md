---
title: Gérer à distance les ordinateurs hôtes Hyper-V
description: Décrit la compatibilité de version entre les ordinateurs hôtes Hyper-V et le Gestionnaire Hyper-V et comment se connecter aux hôtes distants dans différents environnements, y compris inter-domaines et autonomes.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d34e98c-6134-479b-8000-3eb360b8b8a3
author: KBDAzure
ms.author: kathydav
ms.date: 12/06/2016
ms.openlocfilehash: 677e054fe42978697ef786b73daac75069f0408f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392688"
---
# <a name="remotely-manage-hyper-v-hosts-with-hyper-v-manager"></a>Gérer à distance les ordinateurs hôtes Hyper-V à l’aide du Gestionnaire Hyper-V

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows 10 Windows 8.1

Cet article répertorie les combinaisons prises en charge des ordinateurs hôtes Hyper-V et du Gestionnaire Hyper-V et explique comment se connecter aux hôtes Hyper-V locaux et distants afin de pouvoir les gérer. 

Le Gestionnaire Hyper-V vous permet de gérer un petit nombre d’hôtes Hyper-V, à la fois distants et locaux. Il est installé lorsque vous installez les outils de gestion Hyper-V, ce qui peut être fait via une installation complète d’Hyper-V ou une installation d’outils uniquement. L’installation d’outils uniquement signifie que vous pouvez utiliser les outils sur des ordinateurs qui ne répondent pas à la configuration matérielle requise pour héberger Hyper-V. Pour plus d’informations sur le matériel pour les ordinateurs hôtes Hyper-V, voir [Configuration système requise](../System-requirements-for-Hyper-V-on-Windows.md).

Si le Gestionnaire Hyper-V n’est pas installé, consultez les [instructions](#install-hyper-v-manager) ci-dessous.

## <a name="supported-combinations-of-hyper-v-manager-and-hyper-v-host-versions"></a>Combinaisons prises en charge du Gestionnaire Hyper-V et des versions de l’hôte Hyper-V

Dans certains cas, vous pouvez utiliser une version différente du Gestionnaire Hyper-V que la version Hyper-V sur l’ordinateur hôte, comme indiqué dans le tableau. Dans ce cas, le Gestionnaire Hyper-V fournit les fonctionnalités disponibles pour la version d’Hyper-V sur l’hôte que vous gérez. Par exemple, si vous utilisez la version du Gestionnaire Hyper-V dans Windows Server 2012 R2 pour gérer à distance un ordinateur hôte exécutant Hyper-V dans Windows Server 2012, vous ne pouvez pas utiliser les fonctionnalités disponibles dans Windows Server 2012 R2 sur cet hôte Hyper-V.

Le tableau suivant répertorie les versions d’un hôte Hyper-V que vous pouvez gérer à partir d’une version particulière du Gestionnaire Hyper-V. Seules les versions de système d’exploitation prises en charge sont répertoriées. Pour plus d’informations sur l’état de prise en charge d’une version particulière du système d’exploitation, utilisez le bouton Rechercher dans le **vie du produit** sur la page stratégie de [cycle de vie Microsoft](https://support.microsoft.com/lifecycle) . En général, les versions antérieures du Gestionnaire Hyper-V peuvent uniquement gérer un ordinateur hôte Hyper-V exécutant la même version ou la version de Windows Server comparable.

|Version du Gestionnaire Hyper-V | Version de l’hôte Hyper-V|
|---|---|
|Windows 2016, Windows 10|-Windows Server 2016 : toutes les éditions et options d’installation, y compris nano Server, et la version correspondante du serveur Hyper-V <br>-Windows Server 2012 R2 : toutes les éditions et options d’installation, et la version correspondante du serveur Hyper-V <br>-Windows Server 2012 : toutes les éditions et options d’installation, et la version correspondante du serveur Hyper-V <br> -Windows 10 <br> -Windows 8.1  |
| Windows Server 2012 R2, Windows 8.1 | -Windows Server 2012 R2 : toutes les éditions et options d’installation, et la version correspondante du serveur Hyper-V <br>-Windows Server 2012 : toutes les éditions et options d’installation, et la version correspondante du serveur Hyper-V <br>-Windows 8.1
| Windows Server 2012 | -Windows Server 2012 : toutes les éditions et options d’installation, et la version correspondante du serveur Hyper-V
| Windows Server 2008 R2 Service Pack 1, Windows 7 Service Pack 1 | -Windows Server 2008 R2 : toutes les éditions et options d’installation, et la version correspondante du serveur Hyper-V
| Windows Server 2008, Windows Vista Service Pack 2 | -Windows Server 2008 : toutes les éditions et options d’installation, et la version correspondante du serveur Hyper-V

>[!NOTE]
>La prise en charge du Service Pack s’est terminée pour Windows 8 le 12 janvier 2016. Pour plus d’informations, consultez le [Forum aux questions Windows 8.1](https://support.microsoft.com/help/18581).

## <a name="connect-to-a-hyper-v-host"></a>Se connecter à un hôte Hyper-V

Pour vous connecter à un hôte Hyper-v à partir du Gestionnaire Hyper-V, cliquez avec le bouton droit sur **Gestionnaire Hyper-v** dans le volet gauche, puis cliquez sur **se connecter au serveur**.

## <a name="manage-hyper-v-on-a-local-computer"></a>Gérer Hyper-V sur un ordinateur local

Le Gestionnaire Hyper-V ne répertorie pas les ordinateurs qui hébergent Hyper-V tant que vous n’avez pas ajouté l’ordinateur, y compris un ordinateur local. Pour ce faire :

1. Dans le volet gauche, cliquez avec le bouton droit sur **Gestionnaire Hyper-V**.
2. Cliquez sur **se connecter au serveur**.
3. Dans **Sélectionner un ordinateur**, cliquez sur **ordinateur local** , puis sur **OK**.

Si vous ne pouvez pas vous connecter :

* Il est possible que seuls les outils Hyper-V soient installés. Pour vérifier que la plateforme Hyper-V est installée, recherchez le service de gestion d’ordinateurs virtuels. /(Ouvrez l’application de bureau Services : cliquez sur **Démarrer**, cliquez sur la zone **Rechercher** , tapez **services. msc**, puis appuyez sur **entrée**. Si le service de gestion d’ordinateurs virtuels n’est pas listé, installez la plateforme Hyper-V en suivant les instructions de la procédure d' [installation de Hyper-v](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).
* Vérifiez que votre matériel répond à la configuration requise. Consultez [Configuration système requise](../System-requirements-for-Hyper-V-on-Windows.md).
* Vérifiez que votre compte d’utilisateur appartient au groupe administrateurs ou au groupe administrateurs Hyper-V.

## <a name="manage-hyper-v-hosts-remotely"></a>Gérer les ordinateurs hôtes Hyper-V à distance  

Pour gérer les ordinateurs hôtes Hyper-V distants, activez la gestion à distance sur l’ordinateur local et l’hôte distant.

Sur Windows Server, ouvrez Gestionnaire de serveur \>**serveur Local** \>**administration à distance** , puis cliquez sur **autoriser les connexions à distance à cet ordinateur**. 

Ou, à partir de l’un des systèmes d’exploitation, ouvrez Windows PowerShell en tant qu’administrateur et exécutez : 

```
Enable-PSRemoting
```

### <a name="connect-to-hosts-in-the-same-domain"></a>Se connecter à des hôtes dans le même domaine

Pour Windows 8.1 et versions antérieures, la gestion à distance fonctionne uniquement lorsque l’ordinateur hôte se trouve dans le même domaine et que votre compte d’utilisateur local est également sur l’hôte distant.

Pour ajouter un hôte Hyper-V distant au Gestionnaire Hyper-V, sélectionnez **un autre ordinateur** dans la boîte de dialogue **Sélectionner un ordinateur** , puis tapez le nom d’hôte de l’hôte distant, le nom NetBIOS ou le nom de domaine complet \(\)de noms de domaine complet.

Le Gestionnaire Hyper-V dans Windows Server 2016 et Windows 10 offre plus de types de connexion à distance que les versions précédentes, décrits dans les sections suivantes.  

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-as-a-different-user"></a>Se connecter à un hôte distant Windows 2016 ou Windows 10 en tant qu’utilisateur différent

Cela vous permet de vous connecter à l’hôte Hyper-V lorsque vous n’êtes pas en cours d’exécution sur l’ordinateur local en tant qu’utilisateur membre du groupe administrateurs Hyper-V ou administrateurs sur l’hôte Hyper-V. Pour ce faire :

1. Dans le volet gauche, cliquez avec le bouton droit sur **Gestionnaire Hyper-V**.
1. Cliquez sur **se connecter au serveur**.
1. Sélectionnez **se connecter en tant qu’autre utilisateur** dans la boîte de dialogue **Sélectionner un ordinateur** .
1. Sélectionnez **définir l’utilisateur**.

>[!NOTE]
> Cela ne fonctionne que pour les hôtes **distants** windows server 2016 ou Windows 10.

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-using-ip-address"></a>Se connecter à un hôte distant Windows 2016 ou Windows 10 à l’aide d’une adresse IP

Pour ce faire :

1. Dans le volet gauche, cliquez avec le bouton droit sur **Gestionnaire Hyper-V**.
1. Cliquez sur **se connecter au serveur**.
1. Tapez l’adresse IP dans le champ de texte **autre ordinateur** .

>[!NOTE]
> Cela ne fonctionne que pour les hôtes **distants** windows server 2016 ou Windows 10.

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-outside-your-domain-or-with-no-domain"></a>Connectez-vous à un hôte distant Windows 2016 ou Windows 10 en dehors de votre domaine, ou sans domaine

Pour ce faire :

1. Sur l’hôte Hyper-V à gérer, ouvrez une session Windows PowerShell en tant qu’administrateur.

1. Créez les règles de pare-feu nécessaires pour les zones de réseau privé :   
   
   ```
   Enable-PSRemoting
   ```

2. Pour autoriser l’accès à distance sur les zones publiques, activez les règles de pare-feu pour CredSSP et WinRM :
  
   ```
   Enable-WSManCredSSP -Role server
   ```

    Pour plus d’informations, consultez [Enable-PSRemoting](https://technet.microsoft.com/library/hh849694.aspx) et [Enable-WSManCredSSP](https://technet.microsoft.com/library/hh849872.aspx).

Ensuite, configurez l’ordinateur que vous utiliserez pour gérer l’hôte Hyper-V.

1. Ouvrez une session Windows PowerShell en tant qu’administrateur.
1. Exécutez les commandes suivantes :

     ```
     Set-Item WSMan:\localhost\Client\TrustedHosts -Value "fqdn-of-hyper-v-host"
     ```
     ```
     Enable-WSManCredSSP -Role client -DelegateComputer "fqdn-of-hyper-v-host"
     ```
1. Vous devrez peut-être également configurer la stratégie de groupe suivante : 
    * **Configuration** de l’ordinateur \> **modèles d’administration** \> la **délégation des informations d’identification** du **système** \> \> **autoriser la délégation de nouvelles informations d’identification avec l’authentification du serveur NTLM uniquement**
    * Cliquez sur **activer** et ajoutez *WSMan/FQDN-of-Hyper-v-Host*.
1. Ouvrez **le Gestionnaire Hyper-V**.
1. Dans le volet gauche, cliquez avec le bouton droit sur **Gestionnaire Hyper-V**.
1. Cliquez sur **se connecter au serveur**.

>[!NOTE]
> Cela ne fonctionne que pour les hôtes **distants** windows server 2016 ou Windows 10.

Pour plus d’informations sur les applets de commande, consultez [Set-Item](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/set-item) et [Enable-WSManCredSSP](https://technet.microsoft.com/library/hh849872.aspx).

## <a name="install-hyper-v-manager"></a>Installer le Gestionnaire Hyper-V

Pour utiliser un outil d’interface utilisateur, choisissez celui qui convient au système d’exploitation sur l’ordinateur sur lequel vous allez exécuter le Gestionnaire Hyper-V :

Sur Windows Server, ouvrez Gestionnaire de serveur \> **gérer** \> **Ajouter des rôles et des fonctionnalités**. Accédez à la page **fonctionnalités** et développez **Outils d’administration de serveur distant** \> outils d' **administration de rôle** \> **outils de gestion Hyper-V**. 

Sur Windows, le Gestionnaire Hyper-V est disponible sur [n’importe quel système d’exploitation Windows incluant Hyper-v](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility).

1. Sur le bureau Windows, cliquez sur le bouton Démarrer et commencez à taper **programmes et fonctionnalités**. 
1. Dans les résultats de la recherche, cliquez sur **programmes et fonctionnalités**.
1. Dans le volet gauche, cliquez sur **activer ou désactiver des fonctionnalités Windows**.
1. Développez le dossier Hyper-V, puis **cliquez sur outils de gestion Hyper-v**.
1. Pour installer le Gestionnaire Hyper-V, cliquez sur **outils de gestion Hyper-v**. Si vous souhaitez également installer le module Hyper-V, cliquez sur cette option.

Pour utiliser Windows PowerShell, exécutez la commande suivante en tant qu’administrateur :

```
add-windowsfeature rsat-hyper-v-tools
```

## <a name="see-also"></a>Voir également  
 
[Installer Hyper-V](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md) 

