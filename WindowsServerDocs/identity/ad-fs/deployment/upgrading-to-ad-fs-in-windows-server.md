---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: Mise à niveau vers ADFS dans Windows Server2016
description: ''
author: billmath
manager: femila
ms.date: 04/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 428e35524fbcfe5177b544e1c6cc6fa32ec32056
ms.sourcegitcommit: 4a03f263952c993dfdf339dd3491c73719854aba
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74791371"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>Mise à niveau vers AD FS dans Windows Server 2016 à l'aide d'une base de données WID


> [!NOTE]
> Commencez une mise à niveau avec un intervalle de temps définitif planifié pour l’achèvement. Il n’est pas recommandé de conserver AD FS dans un État en mode mixte pendant une période prolongée, car laisser des AD FS en mode mixte peut entraîner des problèmes avec la batterie de serveurs.

## <a name="upgrading-a-windows-server-2012-r2-or-2016-ad-fs-farm-to-windows-server-2019"></a>Mise à niveau d’une batterie de AD FS Windows Server 2012 R2 ou 2016 vers Windows Server 2019
Le document suivant décrit comment mettre à niveau votre batterie de AD FS vers AD FS dans Windows Server 2019 lorsque vous utilisez une base de données WID.

### <a name="ad-fs-farm-behavior-levels-fbl"></a>AD FS les niveaux de comportement de la batterie de serveurs (FBL)
Dans AD FS pour Windows Server 2016, le niveau de comportement de la batterie de serveurs (FBL) a été introduit. Il s’agit d’un paramètre à l’ensemble de la batterie qui détermine les fonctionnalités que le AD FS batterie de serveurs peut utiliser.

Le tableau suivant répertorie les valeurs FBL par version de Windows Server :

| Version de Windows Server  | FBL | Nom de la base de données de configuration AD FS |
| ------------- | ------------- | ------------- |
| 2012 R2  | 1  | AdfsConfiguration |
| 2016  | 3  | AdfsConfigurationV3 |
| 2019  | 4  | AdfsConfigurationV4 |

> [!NOTE]
> La mise à niveau de FBL crée une base de données de configuration AD FS.  Consultez le tableau ci-dessus pour connaître les noms de la base de données de configuration pour chaque version de AD FS Windows Server et la valeur FBL

### <a name="new-vs-upgraded-farms"></a>Nouvelles batteries de serveurs vs mises à niveau
Par défaut, le FBL dans une nouvelle batterie de serveurs AD FS correspond à la valeur de la version Windows Server du premier nœud de la batterie de serveurs installée.

Un serveur AD FS d’une version ultérieure peut être joint à une batterie de serveurs AD FS 2012 R2 ou 2016, et la batterie de serveurs fonctionnera sur le même FBL que le (s) nœud (s) existant (s). Si plusieurs versions de Windows Server se trouvent dans la même batterie de serveurs à la valeur FBL de la version la plus basse, votre batterie de serveurs est dite « mixte ». Toutefois, vous ne pourrez pas tirer parti des fonctionnalités des versions ultérieures tant que FBL n’est pas déclenché. Avec une batterie de serveurs mixte :

- Les administrateurs peuvent ajouter de nouveaux serveurs de Fédération Windows Server 2019 à une batterie de serveurs Windows Server 2012 R2 ou 2016 existante. Par conséquent, la batterie de serveurs est en mode mixte et fonctionne au même niveau de comportement de la batterie de serveurs que la batterie d’origine. Pour garantir un comportement cohérent au sein de la batterie, les fonctionnalités des versions plus récentes de Windows Server AD FS ne peuvent pas être configurées ou utilisées.

- Avant que le FBL puisse être déclenché, les administrateurs doivent supprimer les AD FS nœuds des versions précédentes de Windows Server de la batterie de serveurs.  Dans le cas d’une batterie de serveurs WID, Notez que cela nécessite que l’un des nouveaux serveurs de Fédération Windows Server 2019 soit promu au rôle de nœud principal dans la batterie.

- Une fois que tous les serveurs de Fédération de la batterie de serveurs ont la même version de Windows Server, le FBL peut être déclenché.  Par conséquent, les nouvelles fonctionnalités de AD FS Windows Server 2019 peuvent ensuite être configurées et utilisées.

N’oubliez pas qu’en mode batterie mixte, la batterie de AD FS n’est pas compatible avec les nouvelles fonctionnalités ou fonctionnalités introduites dans AD FS dans Windows Server 2019. Cela signifie que les organisations qui souhaitent essayer les nouvelles fonctionnalités ne peuvent pas effectuer cette opération tant que le FBL n’est pas généré. Par conséquent, si votre organisation cherche à tester les nouvelles fonctionnalités avant de déclencher l’FBL, vous devrez déployer une batterie de serveurs distincte pour effectuer cette opération.

Le reste du document contient les étapes permettant d’ajouter un serveur de Fédération Windows Server 2019 à un environnement Windows Server 2016 ou 2012 R2, puis de déclencher l’FBL vers Windows Server 2019. Ces étapes ont été effectuées dans un environnement de test décrit par le diagramme architectural ci-dessous.

> [!NOTE]
> Pour pouvoir passer à AD FS dans Windows Server 2019 FBL, vous devez supprimer tous les nœuds Windows Server 2016 ou 2012 R2. Vous ne pouvez pas simplement mettre à niveau un système d’exploitation Windows Server 2016 ou 2012 R2 vers Windows Server 2019 et le faire devenir un nœud 2019. Vous devez le supprimer et le remplacer par un nouveau nœud 2019.

##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2019-farm-behavior-level"></a>Pour mettre à niveau votre batterie de serveurs AD FS au niveau du comportement de la batterie de serveurs Windows Server 2019

1. À l’aide de Gestionnaire de serveur, installez le rôle Services ADFS sur Windows Server 2019

2. À l’aide de l’Assistant Configuration de AD FS, joignez le nouveau serveur Windows Server 2019 à la batterie de AD FS existante.

![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)

3. Sur le serveur de Fédération Windows Server 2019, ouvrez gestion des AD FS. Notez que les fonctionnalités de gestion ne sont pas disponibles, car ce serveur de Fédération n’est pas le serveur principal.

![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)

4. Sur le serveur Windows Server 2019, ouvrez une fenêtre de commande PowerShell avec élévation de privilèges et exécutez l’applet de commande suivante :

```PowerShell
Set-AdfsSyncProperties -Role PrimaryComputer
```

![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)

5. Sur le serveur AD FS qui a été précédemment configuré comme principal, ouvrez une fenêtre de commande PowerShell avec élévation de privilèges et exécutez l’applet de commande suivante :

```PowerShell
Set-AdfsSyncProperties -Role SecondaryComputer -PrimaryComputerName {FQDN}
```

![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)

6. Maintenant, sur le serveur de Fédération Windows Server 2016, ouvrez la gestion de AD FS. Notez que toutes les fonctionnalités d’administration s’affichent, car le rôle principal a été transféré vers ce serveur.

![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)

7. Si vous mettez à niveau une batterie de serveurs AD FS 2012 R2 vers 2016 ou 2019, la mise à niveau de la batterie de serveurs nécessite que le schéma AD soit au moins au niveau 85.  Pour mettre à niveau le schéma, à l’aide du support d’installation de Windows Server 2016, ouvrez une invite de commandes et accédez au répertoire support\adprep Exécutez la commande suivante : `adprep /forestprep`

![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)

Une fois l’exécution terminée `adprep/domainprep`

> [!NOTE]
> Avant d’exécuter l’étape suivante, assurez-vous que Windows Server est en cours d’exécution en exécutant Windows Update à partir de paramètres. Continuez ce processus jusqu’à ce qu’il n’y ait plus de mises à jour nécessaires.

![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)

8. Maintenant, sur le serveur Windows Server 2016, ouvrez PowerShell et exécutez l’applet de commande suivante :

> [!NOTE]
> Tous les serveurs 2012 R2 doivent être supprimés de la batterie de serveurs avant d’exécuter l’étape suivante.

```PowerShell
Invoke-AdfsFarmBehaviorLevelRaise
```

![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)

9. Lorsque vous y êtes invité, tapez o. Cela commencera à augmenter le niveau. Une fois cette opération terminée, vous avez correctement déclenché le FBL.

![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)

10. Maintenant, si vous accédez à AD FS Management, vous verrez que les nouvelles fonctionnalités ont été ajoutées pour la version AD FS ultérieure

![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)

11. De même, vous pouvez utiliser l’applet de commande PowerShell : `Get-AdfsFarmInformation` pour afficher le FBL actuel.

![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)

12. Pour mettre à niveau les serveurs WAP au niveau le plus récent, sur chaque proxy d’application Web, reconfigurez le WAP en exécutant l’applet de commande PowerShell suivante dans une fenêtre avec élévation de privilèges :

```PowerShell
$trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred
```

Supprimez les anciens serveurs du cluster et conservez uniquement les serveurs WAP qui exécutent la dernière version du serveur, qui a été reconfigurée ci-dessus, en exécutant l’applet de commande PowerShell suivante.

```PowerShell
Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
```

Vérifiez la configuration du WAP en exécutant l’applet de commande WebApplicationProxyConfiguration. Le ConnectedServersName reflète le serveur exécuté à partir de la commande précédente.

```PowerShell
Get-WebApplicationProxyConfiguration
```
Pour mettre à niveau les ConfigurationVersion des serveurs WAP, exécutez la commande PowerShell suivante.

```PowerShell
Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
```

Cela terminera la mise à niveau des serveurs WAP.
