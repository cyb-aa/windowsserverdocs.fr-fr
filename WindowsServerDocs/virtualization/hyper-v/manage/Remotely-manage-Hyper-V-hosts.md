---
title: Gérer à distance des ordinateurs hôtes Hyper-V
description: Décrit la compatibilité de version entre les hôtes Hyper-V et le Gestionnaire Hyper-V et comment vous connecter à des hôtes distants dans différents environnements, y compris les domaines et autonome.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d34e98c-6134-479b-8000-3eb360b8b8a3
author: KBDAzure
ms.author: kathydav
ms.date: 12/06/2016
ms.openlocfilehash: df66f308ee7999f97fe7e57a8b52256f2561faa2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870230"
---
# <a name="remotely-manage-hyper-v-hosts-with-hyper-v-manager"></a>Gérer à distance des ordinateurs hôtes Hyper-V avec le Gestionnaire Hyper-V

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1

Cet article répertorie les combinaisons prises en charge des hôtes Hyper-V et les versions du Gestionnaire Hyper-V et explique comment se connecter aux ordinateurs hôtes Hyper-V locaux et distants afin que vous puissiez les gérer. 

Gestionnaire Hyper-V vous permet de gérer un petit nombre d’hôtes Hyper-V, locaux et distants. Il est installé lorsque vous installez les outils de gestion Hyper-V, vous pouvez faire via un intégral installation Hyper-V ou une installation des outils uniquement. Effectuer un moyen de l’installation d’outils vous pouvez utiliser les outils sur les ordinateurs qui ne répondent pas à la configuration matérielle requise pour l’hôte Hyper-V. Pour plus d’informations sur le matériel pour les ordinateurs hôtes Hyper-V, consultez [requise](..\System-requirements-for-Hyper-V-on-Windows.md).

Si le Gestionnaire Hyper-V n’est pas installé, consultez le [instructions](#install-hyper-v-manager) ci-dessous.

## <a name="supported-combinations-of-hyper-v-manager-and-hyper-v-host-versions"></a>Combinaisons prises en charge du Gestionnaire Hyper-V et les versions d’hôte Hyper-V

Dans certains cas, vous pouvez utiliser une version différente du Gestionnaire Hyper-V à la version d’Hyper-V sur l’hôte, comme indiqué dans le tableau. Lorsque vous effectuez cette opération, le Gestionnaire Hyper-V fournit les fonctionnalités disponibles pour la version d’Hyper-V sur l’hôte que vous gérez. Par exemple, si vous utilisez la version du Gestionnaire Hyper-V dans Windows Server 2012 R2 pour gérer à distance un ordinateur hôte exécutant Hyper-V dans Windows Server 2012, vous ne pourrez pas utiliser les fonctionnalités disponibles dans Windows Server 2012 R2 sur cet ordinateur hôte Hyper-V.

Le tableau suivant présente les versions d’un hôte Hyper-V, vous pouvez gérer à partir d’une version particulière du Gestionnaire Hyper-V. Uniquement pris en charge le système d’exploitation versions sont répertoriées. Pour plus d’informations sur l’état de prise en charge d’une version de système d’exploitation particulier, utilisez le **cycle de vie de produit recherche** bouton sur le [Microsoft Lifecycle Policy](https://support.microsoft.com/lifecycle) page. En règle générale, les versions antérieures du Gestionnaire Hyper-V peuvent uniquement gérer un hôte Hyper-V qui exécutent la même version ou la version de Windows Server comparable.

|Version du Gestionnaire Hyper-V | Version de l’hôte Hyper-V|
|---|---|
|Windows 2016, Windows 10|-Windows Server 2016, toutes les éditions et options d’installation, y compris Nano Server et la version correspondante du serveur Hyper-V <br>-Windows Server 2012 R2 : toutes les éditions et options d’installation et la version correspondante du serveur Hyper-V <br>-Windows Server 2012, toutes les éditions et options d’installation et la version correspondante du serveur Hyper-V <br> -Windows 10 <br> -Windows 8.1  |
| Windows Server 2012 R2, Windows 8.1 | -Windows Server 2012 R2 : toutes les éditions et options d’installation et la version correspondante du serveur Hyper-V <br>-Windows Server 2012, toutes les éditions et options d’installation et la version correspondante du serveur Hyper-V <br>-Windows 8.1
| Windows Server 2012 | -Windows Server 2012, toutes les éditions et options d’installation et la version correspondante du serveur Hyper-V
| Windows Server 2008 R2 Service Pack 1, Windows 7 Service Pack 1 | -Windows Server 2008 R2 : toutes les éditions et options d’installation et la version correspondante du serveur Hyper-V
| Windows Server 2008, Windows Vista Service Pack 2 | -Windows Server 2008, toutes les éditions et options d’installation et la version correspondante du serveur Hyper-V

>[!NOTE]
>Prise en charge de service pack pour Windows 8 pris fin le 12 janvier 2016. Pour plus d’informations, consultez le [le Forum aux questions sur Windows 8.1](https://support.microsoft.com/help/18581).

## <a name="connect-to-a-hyper-v-host"></a>Se connecter à un ordinateur hôte Hyper-V

Pour vous connecter à un ordinateur hôte Hyper-V à partir du Gestionnaire Hyper-V, avec le bouton droit **Gestionnaire Hyper-V** dans le volet gauche, puis cliquez sur **se connecter au serveur**.

## <a name="manage-hyper-v-on-a-local-computer"></a>Gérer Hyper-V sur un ordinateur local

Gestionnaire Hyper-V ne répertorie pas tous les ordinateurs qui hébergent Hyper-V jusqu'à ce que vous ajoutiez l’ordinateur, y compris un ordinateur local. Pour ce faire :

1. Dans le volet gauche, cliquez sur **Gestionnaire Hyper-V**.
2. Cliquez sur **se connecter au serveur**.
3. À partir de **sélectionner un ordinateur**, cliquez sur **ordinateur Local** puis cliquez sur **OK**.

Si vous ne pouvez pas vous connecter :

* Il est possible que seuls les outils Hyper-V sont installés. Pour vérifier que la plateforme Hyper-V est installé, recherchez le service de gestion d’ordinateurs virtuels. \(Ouvrez l’application de bureau de Services : cliquez sur **Démarrer**, cliquez sur le **rechercher** , tapez **services.msc**, puis appuyez sur **entrée**. Si le service de gestion d’ordinateurs virtuels n’est pas répertorié, installez la plate-forme Hyper-V en suivant les instructions dans [installer Hyper-V](..\get-started\Install-the-Hyper-V-role-on-Windows-Server.md).\)
* Vérifiez que le matériel répond à la configuration requise. Consultez [requise](..\System-requirements-for-Hyper-V-on-Windows.md).
* Vérifiez que votre compte d’utilisateur appartient au groupe Administrateurs ou du groupe Administrateurs Hyper-V.

## <a name="manage-hyper-v-hosts-remotely"></a>Gérer à distance des ordinateurs hôtes Hyper-V  

Pour gérer les hôtes Hyper-V à distance, activez la gestion à distance sur l’ordinateur local et l’hôte distant.

Sur Windows Server, ouvrez le Gestionnaire de serveur \> **serveur Local** \> **gestion à distance** puis cliquez sur **autoriser les connexions à distance à cet ordinateur**. 

Ou, à partir de ces systèmes d’exploitation, ouvrez PowerShell de Windows en tant qu’administrateur et exécutez : 

```
Enable-PSRemoting
```

### <a name="connect-to-hosts-in-the-same-domain"></a>Se connecter aux ordinateurs hôtes dans le même domaine

Pour Windows 8.1 et versions antérieures, la gestion à distance fonctionne uniquement lorsque l’hôte est dans le même domaine et votre compte d’utilisateur local est également sur l’hôte distant.

Pour ajouter un hôte Hyper-V distant au Gestionnaire Hyper-V, sélectionnez **un autre ordinateur** dans le **sélectionner un ordinateur** boîte de dialogue et tapez le nom d’hôte de l’hôte distant, nom NetBIOS ou le nom de domaine complet \(Nom de domaine complet\).

Gestionnaire Hyper-V dans Windows Server 2016 et Windows 10 propose plusieurs types de connexion à distance que les versions antérieures, décrites dans les sections suivantes.  

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-as-a-different-user"></a>Se connecter à un hôte distant Windows 2016 ou Windows 10 en tant qu’autre utilisateur

Cela vous permet de vous connecter à l’hôte Hyper-V lorsque vous n'êtes pas en cours d’exécution sur l’ordinateur local en tant qu’utilisateur qui est membre du groupe Administrateurs de Hyper-V ou du groupe Administrateurs sur l’hôte Hyper-V. Pour ce faire :

1. Dans le volet gauche, cliquez sur **Gestionnaire Hyper-V**.
1. Cliquez sur **se connecter au serveur**.
1. Sélectionnez **se connecter en tant qu’un autre utilisateur** dans le **sélectionner un ordinateur** boîte de dialogue.
1. Sélectionnez **définir l’utilisateur**.

>[!NOTE]
> Cela fonctionne uniquement pour Windows Server 2016 ou Windows 10 **distant** hôtes.

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-using-ip-address"></a>Se connecter à un hôte distant Windows 2016 ou Windows 10 à l’aide d’adresse IP

Pour ce faire :

1. Dans le volet gauche, cliquez sur **Gestionnaire Hyper-V**.
1. Cliquez sur **se connecter au serveur**.
1. Tapez l’adresse IP dans le **un autre ordinateur** champ de texte.

>[!NOTE]
> Cela fonctionne uniquement pour Windows Server 2016 ou Windows 10 **distant** hôtes.

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-outside-your-domain-or-with-no-domain"></a>Se connecter à un Windows 2016 ou Windows 10 hôte distant en dehors de votre domaine, ou sans domaine

Pour ce faire :

1. Sur l’hôte Hyper-V à gérer, ouvrez une session Windows PowerShell en tant qu’administrateur.

1. Créer les règles de pare-feu nécessaires pour les zones de réseau privé :   
   
   ```
   Enable-PSRemoting
   ```

2. Pour autoriser l’accès à distance sur les zones publiques, activez les règles de pare-feu pour CredSSP et WinRM :
  
   ```
   Enable-WSManCredSSP -Role server
   ```

    Pour plus d’informations, consultez [Enable-PSRemoting](https://technet.microsoft.com/library/hh849694.aspx) et [Enable-WSManCredSSP](https://technet.microsoft.com/library/hh849872.aspx).

Ensuite, configurez l’ordinateur que vous utiliserez pour gérer l’ordinateur hôte Hyper-V.

1. Ouvrez une session Windows PowerShell en tant qu’administrateur.
1. Exécutez les commandes suivantes :

     ```
     Set-Item WSMan:\localhost\Client\TrustedHosts -Value "fqdn-of-hyper-v-host"
     ```
     ```
     Enable-WSManCredSSP -Role client -DelegateComputer "fqdn-of-hyper-v-host"
     ```
1. Vous devrez peut-être également configurer la stratégie de groupe suivant : 
    * **Configuration de l’ordinateur** \> **modèles d’administration** \> **système** \> **dedélégationdesinformationsd’identification** \> **Autoriser la délégation de nouvelles informations d’identification avec l’authentification de serveur NTLM uniquement**
    * Cliquez sur **activer** et ajoutez *wsman/fqdn-de-hyper-v-host*.
1. Ouvrez **Gestionnaire Hyper-V**.
1. Dans le volet gauche, cliquez sur **Gestionnaire Hyper-V**.
1. Cliquez sur **se connecter au serveur**.

>[!NOTE]
> Cela fonctionne uniquement pour Windows Server 2016 ou Windows 10 **distant** hôtes.

Pour plus d’informations de l’applet de commande, consultez [Set-Item](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/set-item) et [Enable-WSManCredSSP](https://technet.microsoft.com/library/hh849872.aspx).

## <a name="install-hyper-v-manager"></a>Installer le Gestionnaire Hyper-V

Pour utiliser un outil d’interface utilisateur, choisissez celui approprié pour le système d’exploitation sur l’ordinateur où vous allez exécuter le Gestionnaire Hyper-V :

Sur Windows Server, ouvrez le Gestionnaire de serveur \> **gérer** \> **ajouter des rôles et fonctionnalités**. Déplacer vers le **fonctionnalités** page et développez **outils d’administration de serveur distant** \> **outils d’administration de rôles** \>  **Outils de gestion Hyper-V**. 

Sur Windows, le Gestionnaire Hyper-V est disponible sur [n’importe quel système d’exploitation Windows incluant Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility).

1. Sur le bureau Windows, cliquez sur le bouton Démarrer et commencez à taper **programmes et fonctionnalités**. 
1. Dans les résultats de recherche, cliquez sur **programmes et fonctionnalités**.
1. Dans le volet gauche, cliquez sur **ou désactiver des fonctionnalités Windows activer**.
1. Développez le dossier Hyper-V, et **cliquez sur Outils de gestion Hyper-V**.
1. Pour installer le Gestionnaire Hyper-V, cliquez sur **outils de gestion Hyper-V**. Si vous souhaitez également installer le module Hyper-V, cliquez sur cette option.

Pour utiliser Windows PowerShell, exécutez la commande suivante en tant qu’administrateur :

```
add-windowsfeature rsat-hyper-v-tools
```

## <a name="see-also"></a>Voir aussi  
 
[Installer Hyper-V](..\get-started\Install-the-Hyper-V-role-on-Windows-Server.md) 

