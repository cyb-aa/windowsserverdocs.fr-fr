---
title: Comment détecter, activer et désactiver SMBv1, SMBv2 et SMBv3 dans Windows
description: Décrit comment activer et désactiver le protocole SMB (Server Message Block) (SMBv1, SMBv2 et SMBv3) dans les environnements client et serveur Windows.
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 9da4d6f2b5616dc6f8aec3fefb1ae7141ed88b0b
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654390"
---
# <a name="how-to-detect-enable-and-disable-smbv1-smbv2-and-smbv3-in-windows"></a>Comment détecter, activer et désactiver SMBv1, SMBv2 et SMBv3 dans Windows

## <a name="summary"></a>Récapitulatif

Cet article décrit comment activer et désactiver le protocole SMB (Server Message Block) version 1 (SMBv1), SMB version 2 (SMBv2) et SMB version 3 (SMBv3) sur les composants client et serveur SMB. 

> [!IMPORTANT]
> Nous vous recommandons de ne **pas** désactiver SMBv2 ou SMBv3. Désactivez SMBv2 ou SMBv3 uniquement comme mesure de dépannage temporaire. Ne laissez pas SMBv2 ou SMBv3 désactivé.  

Dans Windows 7 et Windows Server 2008 R2, la désactivation de SMBv2 désactive les fonctionnalités suivantes : 
 
- Composition des demandes : permet d’envoyer plusieurs demandes SMB 2 en tant que demande réseau unique    
- Lectures et écritures plus grandes-meilleure utilisation de réseaux plus rapides    
- Mise en cache des propriétés de dossier et de fichier-les clients conservent des copies locales de dossiers et de fichiers    
- Handles durables-autoriser la connexion à se reconnecter en toute transparence au serveur en cas de déconnexion temporaire    
- Signature de message améliorée-HMAC SHA-256 remplace MD5 comme algorithme de hachage    
- Évolutivité améliorée pour le partage de fichiers : le nombre d’utilisateurs, de partages et de fichiers ouverts par serveur a considérablement augmenté    
- Prise en charge des liens symboliques    
- Modèle de bail oplock client : limite les données transférées entre le client et le serveur, ce qui améliore les performances sur les réseaux à latence élevée et l’évolutivité du serveur SMB.    
- Large prise en charge MTU : pour une utilisation complète de 10 Gigabye (Go) Ethernet    
- Efficacité énergétique améliorée : les clients qui ont des fichiers ouverts sur un serveur peuvent se mettre en veille    

Dans Windows 8, Windows 8.1, Windows 10, Windows Server 2012 et Windows Server 2016, la désactivation de SMBv3 désactive les fonctionnalités suivantes (et également la fonctionnalité SMBv2 décrite dans la liste précédente) : 
 
- Basculement transparent-les clients se reconnectent sans interruption aux nœuds de cluster pendant la maintenance ou le basculement    
- Scale Out : accès simultané aux données partagées sur tous les nœuds de cluster de fichiers     
- Multichannel-agrégation de la bande passante réseau et de la tolérance de panne si plusieurs chemins sont disponibles entre le client et le serveur  
- SMB direct : ajoute la prise en charge de la mise en réseau RDMA pour des performances très élevées, avec une faible latence et une faible utilisation du processeur    
- Chiffrement : fournit un chiffrement de bout en bout et protège contre les écoutes clandestines sur les réseaux non fiables    
- Leasing de répertoires : améliore les temps de réponse des applications dans les succursales grâce à la mise en cache    
- Optimisations des performances-optimisations pour les e/s de lecture/écriture aléatoires

##  <a name="more-information"></a>Plus d’informations

Le protocole SMBv2 a été introduit dans Windows Vista et Windows Server 2008.

Le protocole SMBv3 a été introduit dans Windows 8 et Windows Server 2012.

Pour plus d’informations sur les fonctionnalités des fonctionnalités SMBv2 et SMBv3, consultez les articles suivants :

[Vue d’ensemble du bloc de message serveur](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831795(v=ws.11))

[Nouveautés du système de noms de domaine (SMB)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff625695(v=ws.10))  

## <a name="how-to-gracefully-remove-smb-v1-in-windows-81-windows-10-windows-2012-r2-and-windows-server-2016"></a>Comment supprimer de manière appropriée SMB v1 dans Windows 8.1, Windows 10, Windows 2012 R2 et Windows Server 2016

#### <a name="windows-server-2012-r2--2016-powershell-methods"></a>Windows Server 2012 R2 & 2016 : méthodes PowerShell

##### <a name="smb-v1"></a>SMB v1

- Identifie 

  ```PowerShell
  Get-WindowsFeature FS-SMB1
  ```

- Désactive 

  ```PowerShell
  Disable-WindowsOptionalFeature -Online -FeatureName smb1protocol
  ```

- Activez : 

  ```PowerShell
  Enable-WindowsOptionalFeature -Online -FeatureName smb1protocol
  ```

##### <a name="smb-v2v3"></a>SMB v2/v3

- Identifie
  
  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB2Protocol
  ```

- Désactive

  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $false
  ```

- Activez :

  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $true 
  ```

#### <a name="windows-server-2012-r2-and-windows-server-2016-server-manager-method-for-disabling-smb"></a>Windows Server 2012 R2 et Windows Server 2016 : méthode Gestionnaire de serveur pour la désactivation de SMB

##### <a name="smb-v1"></a>SMB v1

![Gestionnaire de serveur-Dashboard, méthode](media/detect-enable-and-disable-smbv1-v2-v3-1.png)

#### <a name="windows-81-and-windows-10-powershell-method"></a>Windows 8.1 et Windows 10 : méthode PowerShell

##### <a name="smb-v1-protocol"></a>Protocole SMB v1

- Identifie 
  
  ```PowerShell
  Get-WindowsOptionalFeature –Online –FeatureName SMB1Protocol
  ```

- Désactive 

  ```PowerShell
  Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
  ```

- Activez : 

  ```PowerShell
  Enable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
  ```

##### <a name="smb-v2v3protocol"></a>Protocole SMB v2/v3

- Identifie 
  
  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB2Protocol
  ```

- Désactive 
  
  ```PowerShell
  Set-SmbServerConfiguration –EnableSMB2Protocol $false
  ```

- Activez :

  ```PowerShell
  Set-SmbServerConfiguration –EnableSMB2Protocol $true
  ```

#### <a name="windows-81-and-windows-10-add-or-remove-programs-method"></a>Windows 8.1 et Windows 10 : méthode ajout/suppression de programmes

![Méthode cliente Add-Remove Programs](media/detect-enable-and-disable-smbv1-v2-v3-2.png)

## <a name="how-to-detect-status-enable-and-disable-smb-protocols-on-the-smb-server"></a>Comment détecter l’État, activer et désactiver les protocoles SMB sur le serveur SMB

### <a name="for-windows-8-and-windows-server-2012"></a>Pour Windows 8 et Windows Server 2012

Windows 8 et Windows Server 2012 présentent la nouvelle applet de commande Windows PowerShell **Set-SMBServerConfiguration** . L’applet de commande vous permet d’activer ou de désactiver les protocoles SMBv1, SMBv2 et SMBv3 sur le composant serveur.  

> [!NOTE]   
> Lorsque vous activez ou désactivez SMBv2 dans Windows 8 ou Windows Server 2012, SMBv3 est également activé ou désactivé. Ce comportement se produit parce que ces protocoles partagent la même pile.     

Vous n’avez pas besoin de redémarrer l’ordinateur après l’exécution de l’applet de commande **Set-SMBServerConfiguration** . 

##### <a name="smb-v1-on-smb-server"></a>SMB v1 sur le serveur SMB

- Identifie 
  
  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB1Protocol
  ```

- Désactive 

  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB1Protocol $false
  ```

- Activez : 
  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB1Protocol $true
  ```

Pour plus d’informations, consultez [stockage serveur chez Microsoft](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Stop-using-SMB1/ba-p/425858). 
##### <a name="smb-v2v3-on-smb-server"></a>SMB v2/v3 sur le serveur SMB

- Identifie

  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB2Protocol
  ```

- Désactive 
  
  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $false
  ```

- Activez :
  
  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $true
  ```

### <a name="for-windows-7-windows-server-2008-r2-windows-vista-and-windows-server-2008"></a>Pour Windows 7, Windows Server 2008 R2, Windows Vista et Windows Server 2008

Pour activer ou désactiver les protocoles SMB sur un serveur SMB qui est exécutant Windows 7, Windows Server 2008 R2, Windows Vista ou Windows Server 2008, utilisez Windows PowerShell ou l’éditeur du Registre. 

#### <a name="powershell-methods"></a>Méthodes PowerShell

> [!NOTE]
> Cette méthode nécessite PowerShell 2,0 ou une version ultérieure de PowerShell.

##### <a name="smb-v1-on-smb-server"></a>SMB v1 sur le serveur SMB

Identifie

```PowerShell
Get-Item HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters | ForEach-Object {Get-ItemProperty $_.pspath}
```

Configuration par défaut = activé (aucune clé de Registre n’est créée), donc aucune valeur SMB1 n’est retournée

Désactive

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB1 -Type DWORD -Value 0 –Force
```

Activez :  

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB1 -Type DWORD -Value 1 –Force
```  

**Remarque** Vous devez redémarrer l’ordinateur après avoir apporté ces modifications. Pour plus d’informations, consultez [stockage serveur chez Microsoft](https://blogs.technet.microsoft.com/filecab/2016/09/16/stop-using-smb1/). 
##### <a name="smb-v2v3-on-smb-server"></a>SMB v2/v3 sur le serveur SMB

Identifie  

```PowerShell
Get-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters | ForEach-Object {Get-ItemProperty $_.pspath} 
```

Désactive

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB2 -Type DWORD -Value 0 –Force  
```

Activez :

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB2 -Type DWORD -Value 1 –Force 
```

> [!NOTE]
> Vous devez redémarrer l’ordinateur après avoir apporté ces modifications. 

#### <a name="registry-editor"></a>Éditeur du Registre

> [!IMPORTANT]
> Suivez attentivement les étapes décrites dans cette section. De graves problèmes peuvent se produire si vous modifiez le registre de façon incorrecte. Avant de le modifier, [sauvegardez le Registre afin de pouvoir le restaurer](https://support.microsoft.com/help/322756) en cas de problème.
 
Pour activer ou désactiver SMBv1 sur le serveur SMB, configurez la clé de Registre suivante :

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters**

```
Registry entry: SMB1
REG_DWORD: 0 = Disabled
REG_DWORD: 1 = Enabled
Default: 1 = Enabled (No registry key is created)
```

Pour activer ou désactiver SMBv2 sur le serveur SMB, configurez la clé de Registre suivante : 

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters**

```
Registry entry: SMB2
REG_DWORD: 0 = Disabled
REG_DWORD: 1 = Enabled
Default: 1 = Enabled (No registry key is created) 
```

> [!NOTE]
> vous devez redémarrer l’ordinateur après avoir apporté ces modifications. 

## <a name="how-to-detect-status-enable-and-disable-smb-protocols-on-the-smb-client"></a>Comment détecter l’État, activer et désactiver les protocoles SMB sur le client SMB

### <a name="for-windows-vista-windows-server-2008-windows-7-windows-server-2008-r2-windows-8-and-windows-server-2012"></a>Pour Windows Vista, Windows Server 2008, Windows 7, Windows Server 2008 R2, Windows 8 et Windows Server 2012

> [!NOTE]
> Lorsque vous activez ou désactivez SMBv2 dans Windows 8 ou Windows Server 2012, SMBv3 est également activé ou désactivé. Ce comportement se produit parce que ces protocoles partagent la même pile. 

##### <a name="smb-v1-on-smb-client"></a>SMB v1 sur un client SMB

- Détecter
  
  ```cmd
  sc.exe qc lanmanworkstation
  ```

- Désactive
  
  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb20/nsi
  sc.exe config mrxsmb10 start= disabled
  ```

- Activez :

  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb10/mrxsmb20/nsi
  sc.exe config mrxsmb10 start= auto
  ```

Pour plus d’informations, consultez [stockage serveur chez Microsoft](https://blogs.technet.microsoft.com/filecab/2016/09/16/stop-using-smb1/) 
##### <a name="smb-v2v3-on-smb-client"></a>SMB v2/v3 sur le client SMB

- Identifie

  ```cmd
  sc.exe qc lanmanworkstation
  ```

- Désactive

  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb10/nsi
  sc.exe config mrxsmb20 start= disabled 
  ```

- Activez :

  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb10/mrxsmb20/nsi
  sc.exe config mrxsmb20 start= auto
  ```

> [!NOTE]
> - Vous devez exécuter ces commandes à partir d’une invite de commandes avec élévation de privilèges.    
> - Vous devez redémarrer l’ordinateur après avoir apporté ces modifications.    
 

## <a name="disable-smbv1-server-with-group-policy"></a>Désactiver le serveur SMBv1 avec stratégie de groupe

Cette procédure configure le nouvel élément suivant dans le registre :

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters** 

- Entrée de Registre : **SMB1** 
- REG_DWORD : **0** = désactivé   

Pour configurer ce à l’aide de stratégie de groupe, procédez comme suit :
 
1. Ouvrez la **Console de gestion des stratégies de groupe**. Cliquez avec le bouton droit sur l’objet de stratégie de groupe (GPO) qui doit contenir le nouvel élément de préférence, puis cliquez sur **Modifier**.

2. Dans l’arborescence de la console, sous **Configuration ordinateur**, développez le dossier **Préférences** , puis développez le dossier **Paramètres Windows** .

3. Cliquez avec le bouton droit sur le nœud **Registre**, pointez sur **Nouveau** et sélectionnez **Élément Registre**.

   ![Élément Registry-New-Registry](media/detect-enable-and-disable-smbv1-v2-v3-3.png)    
 
Dans la boîte de dialogue **Propriétés du nouveau registre**, sélectionnez les éléments suivants : 
 
- **Action**: créer    
- **Hive**: HKEY_LOCAL_MACHINE    
- **Chemin**de la clé : SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters    
- **Nom**de la valeur : SMB1    
- **Type de valeur**: REG_DWORD    
- **Données**de la valeur : 0    
 
![Nouvelles propriétés de Registre-général](media/detect-enable-and-disable-smbv1-v2-v3-4.png)

Les composants du serveur SMBv1 sont alors désactivés. Cette stratégie de groupe doit être appliquée à tous les postes de travail, serveurs et contrôleurs de domaine nécessaires dans le domaine.

> [!NOTE]
>Les  [filtres WMI](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc947846(v=ws.10)) peuvent également être définis pour exclure les systèmes d’exploitation non pris en charge ou les exclusions sélectionnées, telles que Windows XP.

> [!IMPORTANT]
> Soyez prudent lorsque vous apportez ces modifications sur les contrôleurs de domaine sur lesquels Windows XP hérité ou des systèmes tiers Linux et tiers (qui ne prennent pas en charge SMBv2 ou SMBv3) requièrent l’accès à SYSVOL ou à d’autres partages de fichiers dans lesquels SMB v1 est désactivé.     

## <a name="disable-smbv1-client-with-group-policy"></a>Désactiver le client SMBv1 avec stratégie de groupe

Pour désactiver le client SMBv1, la clé de Registre services doit être mise à jour pour désactiver le démarrage de **MRxSMB10** , puis la dépendance sur **MRxSMB10** doit être supprimée de l’entrée pour **LanmanWorkstation** , afin qu’elle puisse démarrer normalement sans nécessiter le premier démarrage de **MRxSMB10** .

Cela permet de mettre à jour et de remplacer les valeurs par défaut dans les 2 éléments suivants du Registre : 

**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\services\mrxsmb10** 

Entrée de Registre : **Start** REG_DWORD : **4**= Disabled

**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\LanmanWorkstation** 

Entrée de Registre : **DependOnService** REG_MULTI_SZ : **« Bowser », « MRxSmb20 », « NSI »**   

> [!NOTE]
> le MRxSMB10 inclus par défaut, qui est maintenant supprimé en tant que dépendance.

Pour configurer ce à l’aide de stratégie de groupe, procédez comme suit :
 
1. Ouvrez la **Console de gestion des stratégies de groupe**. Cliquez avec le bouton droit sur l’objet de stratégie de groupe (GPO) qui doit contenir le nouvel élément de préférence, puis cliquez sur **Modifier**.

2. Dans l’arborescence de la console, sous **Configuration ordinateur**, développez le dossier **Préférences** , puis développez le dossier **Paramètres Windows** .

3. Cliquez avec le bouton droit sur le nœud **Registre**, pointez sur **Nouveau** et sélectionnez **Élément Registre**.    

4. Dans la boîte de dialogue **Propriétés du nouveau registre** , sélectionnez les éléments suivants : 
 
   - **Action**: mettre à jour
   - **Hive**: HKEY_LOCAL_MACHINE
   - **Chemin**de la clé : SYSTEM\CurrentControlSet\services\mrxsmb10
   - **Nom**de la valeur : début
   - **Type de valeur**: REG_DWORD
   - **Données**de la valeur : 4
 
   ![Propriétés de démarrage-général](media/detect-enable-and-disable-smbv1-v2-v3-5.png)

5. Supprimez ensuite la dépendance sur le **MRxSMB10** qui vient d’être désactivé.

   Dans la boîte de dialogue **Propriétés du nouveau registre** , sélectionnez les éléments suivants : 
 
   - **Action**: remplacer
   - **Hive**: HKEY_LOCAL_MACHINE
   - **Chemin**de la clé : SYSTEM\CurrentControlSet\Services\LanmanWorkstation
   - **Nom**de la valeur : DependOnService
   - **Type de valeur**: REG_MULTI_SZ 
   - **Données de la valeur** :
      - Bowser
      - MRxSmb20
      - NSI
 
   > [!NOTE]
   > Ces trois chaînes ne comporteront pas de puces (voir la capture d’écran suivante).

   ![Propriétés DependOnService](media/detect-enable-and-disable-smbv1-v2-v3-6.png) 

   La valeur par défaut comprend des **MRxSMB10** dans de nombreuses versions de Windows. par conséquent, si vous les remplacez par cette chaîne à valeurs multiples, vous supprimez **MRxSMB10** en tant que dépendance pour **LanmanServer** et en allant de quatre valeurs par défaut à ces trois valeurs ci-dessus.

   > [!NOTE]
   > Lorsque vous utilisez Console de gestion des stratégies de groupe, vous n’êtes pas obligé d’utiliser des guillemets ou des virgules. Tapez simplement chaque entrée sur des lignes individuelles.

6. Redémarrez les systèmes ciblés pour terminer la désactivation de SMB v1.

### <a name="summary"></a>Récapitulatif

Si tous les paramètres se trouvent dans le même objet de stratégie de groupe (GPO), la gestion des stratégie de groupe affiche les paramètres suivants.

![Éditeur de gestion des stratégies de groupe-Registre](media/detect-enable-and-disable-smbv1-v2-v3-7.png) 

### <a name="testing-and-validation"></a>Test et validation

Une fois ces derniers configurés, autorisez la réplication et la mise à jour de la stratégie. Si nécessaire pour le test, exécutez **gpupdate/force** à partir d’une invite de commandes, puis examinez les ordinateurs cibles pour vous assurer que les paramètres du Registre sont appliqués correctement. Assurez-vous que SMB v2 et SMB v3 fonctionnent pour tous les autres systèmes de l’environnement.

> [!NOTE]
> N’oubliez pas de redémarrer les systèmes cibles.
