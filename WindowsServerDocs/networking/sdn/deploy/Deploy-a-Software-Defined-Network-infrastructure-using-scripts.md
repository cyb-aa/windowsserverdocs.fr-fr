---
title: Déployer une Infrastructure de réseau à définition logicielle à l’aide de Scripts
description: Cette rubrique explique comment déployer une infrastructure Microsoft réseau SDN (Software Defined) à l’aide de scripts dans Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: dabfe3de4cc307723ff7e614fb73e3903e74aeb2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844620"
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>Déployer une infrastructure SDN (Software Defined Networking) avec des scripts

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous déployez une infrastructure Microsoft réseau SDN (Software Defined) à l’aide de scripts. L’infrastructure comprend un contrôleur de réseau à haute disponibilité (HA), une haute disponibilité logiciel équilibreur de charge (SLB) / MUX, réseaux virtuels et associés des listes de contrôle d’accès (ACL). En outre, un autre script déploie une charge de travail cliente vous permet de valider votre infrastructure SDN.  
  
Si vous souhaitez que vos charges de travail client de communiquer en dehors de leurs réseaux virtuels, vous pouvez configurer des règles NAT de SLB, tunnels de passerelle Site à Site ou de couche 3 transférer le routage entre les charges de travail virtuels et physiques.  
  
Vous pouvez également déployer une infrastructure SDN à l’aide de Virtual Machine Manager (VMM). Pour plus d’informations, consultez [configurer une infrastructure de réseau SDN (Software Defined) dans l’infrastructure VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-overview).  

  
## <a name="pre-deployment"></a>Avant le déploiement  
  
> [!IMPORTANT]  
> Avant de commencer le déploiement, vous devez planifier et configurer vos ordinateurs hôtes et l’infrastructure réseau physique. Pour plus d’informations, voir [Planifier une mise en réseau SDN (Software Defined Networking)](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md).  
  
Tous les hôtes Hyper-V doivent avoir installé Windows Server 2016.  
  
## <a name="deployment-steps"></a>Étapes de déploiement  
Commencez par configurer le commutateur virtuel de Hyper-V (serveurs physiques) et attribution d’adresses IP de l’hôte Hyper-V. N’importe quel type de stockage qui est compatible avec Hyper-V, partagé ou local peut être utilisé.  

### <a name="install-host-networking"></a>Installer la mise en réseau hôte  

1. Installez les derniers pilotes réseau disponibles pour votre matériel de carte réseau.  
2. Installer le rôle Hyper-V sur tous les ordinateurs hôtes (pour plus d’informations, consultez [bien démarrer avec Hyper-V sur Windows Server 2016](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/Get-started-with-Hyper-V-on-Windows).   
  
   ```PowerShell
   Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart
   ```  
    
3. Créez le commutateur virtuel Hyper-V.<p>Utiliser le même nom de commutateur pour tous les ordinateurs hôtes, par exemple, **sdnSwitch**. Configurez au moins une carte réseau ou, si vous utilisez ensemble, configurez au moins deux cartes réseau. Maximale entrant se produit lors de l’utilisation de deux cartes réseau.  

   ```PowerShell
   New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True
   ```  
   >[!TIP] 
   >Vous pouvez ignorer les étapes 4 et 5 si vous avez des cartes réseau de gestion distinct.

3. Reportez-vous à la rubrique Planification ([planifier une Infrastructure de réseau défini par logiciel](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) et fonctionnent avec votre administrateur réseau pour obtenir l’ID de VLAN du VLAN de gestion. Attacher la carte réseau virtuelle de gestion du commutateur virtuel nouvellement créé pour le réseau local virtuel de gestion. Cette étape peut être omise si votre environnement n’utilise pas les étiquettes VLAN.  
   
   ```PowerShell
   Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True
   ```  
 
4. Reportez-vous à la rubrique Planification ([planifier une Infrastructure de réseau défini par logiciel](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) et fonctionnent avec votre administrateur réseau pour utiliser le protocole DHCP soit ou les affectations d’adresses IP statiques pour affecter une adresse IP à la carte réseau virtuelle de gestion de nouvellement créé commutateur virtuel. L’exemple suivant montre comment créer une adresse IP statique et l’assigner à la carte réseau virtuelle de gestion du commutateur virtuel :  
 
   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>
   ```  
      
5. [Facultatif] Déployer une machine virtuelle pour héberger les Services de domaine Active Directory ([installer Active Directory Domain Services (niveau 100)](https://technet.microsoft.com/library/hh472162.aspx) et un serveur DNS.  
   
    a. Se connecter à la machine virtuelle Active Directory/DNS Server pour le réseau local virtuel de gestion :
    
       ```PowerShell
       Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True  
       ```   

   b. Installer DNS et Active Directory Domain Services.  

   >[!NOTE]
   >Le contrôleur de réseau prend en charge les certificats Kerberos et X.509 pour l’authentification. Ce guide utilise les deux mécanismes d’authentification à des fins différentes (même si seul l’un est obligatoire).  
        
6. Joignez tous les hôtes Hyper-V au domaine. Vérifiez que l’entrée de serveur DNS pour la carte réseau qui a une adresse IP assignée aux points de réseau de gestion à un serveur DNS qui peut résoudre le nom de domaine. 

   ```PowerShell   
   Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>  
   ```
   
   a. Avec le bouton droit **Démarrer**, cliquez sur **système**, puis cliquez sur **modifier les paramètres**.  
   b. Cliquez sur **Modifier**.  
   c. Cliquez sur **domaine** et spécifiez le nom de domaine.  
   d. Cliquez sur **OK**.  
   e. Tapez les nom et mot de passe informations d’identification utilisateur lorsque vous y êtes invité.  
   f. Redémarrez le serveur.  
  
### <a name="validation"></a>Validation  
Utilisez les étapes suivantes pour valider la mise en réseau de cet hôte est configuré correctement.  

1. Vérifiez que le commutateur de machine virtuelle a été créé avec succès :  
      
   ```PowerShell
   Get-VMSwitch "<switch name>"
   ```  

2. Vérifiez que la carte réseau virtuelle de gestion sur le commutateur de machine virtuelle est connectée au VLAN de gestion :  

   >[!NOTE]
   >Pertinent uniquement si le trafic de client et de gestion partage la même carte réseau.    
      
   ```PowerShell
   Get-VMNetworkAdapterIsolation -ManagementOS
   ```

3. Valider tous les hôtes Hyper-V et les ressources de gestion externe, par exemple, les serveurs DNS.<p>Assurez-vous qu’ils sont accessibles via le ping à l’aide de leur adresse IP de gestion et/ou le nom de domaine complet (FQDN).   
      
   ``ping <Hyper-V Host IP>``  
   ``ping <Hyper-V Host FQDN>``  

4. Exécutez la commande suivante sur l’hôte de déploiement et spécifiez le nom de domaine complet de chaque hôte Hyper-V pour assurer les informations d’identification Kerberos utilisées fournit l’accès à tous les serveurs.  
      
   ``winrm id -r:<Hyper-V Host FQDN>``  
      
### <a name="nano-installation-requirements-and-notes"></a>Notes de publication et les exigences d’installation Nano  

Si vous utilisez Nano en tant que vos hôtes Hyper-V (serveurs physiques) pour le déploiement, les éléments suivants sont des exigences supplémentaires :  

1. Tous les nœuds Nano nécessaire pour que le package DSC installé avec le module linguistique :  
   
    - Microsoft-NanoServer-DSC-Package.cab  
    - Microsoft-NanoServer-DSC-Package_en-us.cab
   
    ``dism /online /add-package /packagepath:<Path> /loglevel:4``  

2. Les scripts SDN Express doivent être exécutés à partir d’un hôte non Nano (Windows Server Core ou Windows Server avec l’interface graphique utilisateur). Flux de travail PowerShell ne sont pas pris en charge sur Nano.  

3.  Appeler l’API NorthBound du contrôleur de réseau à l’aide de PowerShell ou les Wrappers de REST NC (qui s’appuient sur Invoke-WebRequest et Invoke-RestMethod) doit être effectuée à partir d’un hôte non Nano.  
   
         
### <a name="run-sdn-express-scripts"></a>Exécuter des scripts SDN Express  
  
1. Accédez à la [dépôt Microsoft SDN GitHub](https://github.com/Microsoft/SDN.git) pour les fichiers d’installation.

2. Télécharger les fichiers d’installation à partir du référentiel sur l’ordinateur de déploiement désigné. Cliquez sur **cloner ou télécharger** puis cliquez sur **Download ZIP**.  
 
   >[!NOTE]
   >L’ordinateur désigné déploiement doit exécuter Windows Server 2016 ou version ultérieure.
 
3. Développez le fichier zip et copie le **SDNExpress** dossier sur l’ordinateur de déploiement `C:\` dossier.  
  
4. Partage le `C:\SDNExpress` dossier en tant que «**SDNExpress**» avec l’autorisation de **tout le monde** à **en lecture/écriture**.  
  
5. Accédez à la `C:\SDNExpress` dossier.<p>Vous consultez les dossiers suivants :  

   |Nom du dossier|Description|  
   |---------------|---------------|  
   |AgentConf|Contient de nouvelles copies des schémas OVSDB utilisés par l’Agent hôte de SDN sur chaque hôte de Windows Server 2016 Hyper-V pour la stratégie de réseau de programme.|  
   |Certificats|Emplacement partagé temporaire pour le fichier de certificat de contrôleur de réseau.|  
   |Images|Vide, ici, placez l’image de vhdx Windows Server 2016|  
   |Outils|Utilitaires de dépannage et débogage.  Copiés vers les hôtes et les machines virtuelles.  Nous vous recommandons de que placer le Moniteur réseau ou Wireshark ici afin qu’il soit disponible si nécessaire.|  
   |scripts ;|Scripts de déploiement.<br /><br />-   **SDNExpress.ps1**<br />    Déploie et configure l’infrastructure, y compris les machines virtuelles de contrôleur de réseau, machines virtuelles SLB Mux, passerelle ou les pools et les machines virtuelles de passerelle HNV correspondant aux ou les pools.<br />-   **FabricConfig.psd1**<br />    Un modèle de fichier de configuration pour le script SDNExpress.  Vous allez le personnaliser pour votre environnement.<br />-   **SDNExpressTenant.ps1**<br />    Déploie une charge de travail du locataire exemple sur un réseau virtuel avec une adresse IP virtuelle à charge équilibrée.<br />    Configure également un ou plusieurs connexions réseau (IPSec S2S VPN, GRE, L3) sur les passerelles de périmètre de fournisseur de service qui sont connectés à la charge de travail client créé précédemment. Les passerelles IPSec et GRE sont disponibles pour la connectivité via l’adresse IP adresse IP virtuelle correspondante et la passerelle de transfert de L3 via le pool d’adresses correspondant.<br />    Ce script peut être utilisé pour supprimer la configuration correspondante avec également une option d’annulation.<br />-   **TenantConfig.psd1**<br />    Un fichier de configuration de modèle pour la charge de travail de locataire et la configuration de la passerelle S2S.<br />-   **SDNExpressUndo.ps1**<br />    Nettoie l’environnement d’infrastructure et une remise à un état de départ.<br />-   **SDNExpressEnterpriseExample.ps1**<br />    Configure un ou plusieurs environnements site d’entreprise avec une passerelle d’accès à distance et (éventuellement) une machine virtuelle d’entreprise correspondante par site. Les passerelles d’entreprise IPSec ou GRE se connecte à l’adresse IP virtuelle correspondante de la passerelle du fournisseur de service pour établir les tunnels S2S. La passerelle de transfert L3 se connecte via l’adresse IP homologue correspondante. <br />            Ce script peut être utilisé pour supprimer la configuration correspondante avec également une option d’annulation.<br />-   **EnterpriseConfig.psd1**<br />    Un fichier de configuration de modèle pour la passerelle de site à site d’entreprise et la configuration de machine virtuelle du Client.|  
   |TenantApps|Fichiers utilisés pour déployer des charges de travail clientes exemple.|  
   ---
  
6. Vérifiez que le fichier VHDX de Windows Server 2016 est dans le **Images** dossier.  
  
7. Personnaliser le fichier SDNExpress\scripts\FabricConfig.psd1 en modifiant le **<< remplacer >>** balises avec les valeurs spécifiques en fonction de votre infrastructure du laboratoire, y compris les noms d’hôte, les noms de domaine, les noms d’utilisateur et mots de passe, et informations de réseau pour les réseaux répertoriés dans la rubrique Planification réseau.  

8. Créer un enregistrement hôte A dans DNS pour les NetworkControllerRestIP NetworkControllerRestName (FQDN).  

9. Exécutez le script en tant qu’utilisateur avec les informations d’identification d’administrateur de domaine :  
      
   ``SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
10. Pour annuler toutes les opérations, exécutez la commande suivante :  
      
    ``SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
#### <a name="validation"></a>Validation  

En supposant que le script SDN Express s’est terminé sans signaler des erreurs, vous pouvez effectuer l’étape suivante pour vérifier les ressources d’infrastructure ont été déployés correctement et sont disponibles pour le déploiement du client.  

Utilisez [outils de Diagnostic](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack) pour vous assurer qu’aucune erreur sur les ressources d’infrastructure dans le contrôleur de réseau.  
      
   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>``  
        
   
### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>Déployer une charge de travail du locataire exemple avec l’équilibrage de charge logiciel  
    
Maintenant que les ressources d’infrastructure ont été déployés, vous pouvez valider votre SDN déploiement end-to-end en déployant une charge de travail de locataire exemple. Cette charge de travail client se compose de deux sous-réseaux virtuels (couche web et couche de base de données) protégées à l’aide des règles de liste de contrôle d’accès (ACL) à l’aide du pare-feu SDN distribué. Sous-réseau virtuel de la couche web est accessible via le SLB/MUX à l’aide d’une adresse IP virtuelle (VIP). Le script déploie deux machines virtuelles de niveau web et une machine virtuelle de niveau une base de données automatiquement et connecte à des sous-réseaux virtuels.  
  
1.  Personnaliser le fichier SDNExpress\scripts\TenantConfig.psd1 en modifiant le **<< remplacer >>** balises avec des valeurs spécifiques (par exemple : Nom d’image de disque dur virtuel, nom du contrôleur de réseau reste, nom de commutateur virtuel, etc. précédemment définies dans le fichier FabricConfig.psd1)  

2.  Exécutez le script. Exemple :  

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

3.  Pour annuler la configuration, exécutez le même script avec le **Annuler** paramètre. Exemple :  

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

#### <a name="validation"></a>Validation  

Pour valider que le déploiement de client a réussi, procédez comme suit :

1. Connectez-vous à la machine virtuelle de niveau base de données et effectuez un test ping de l’adresse IP d’un des machines virtuelles de niveau web (Vérifiez que le pare-feu de Windows est désactivée dans les machines virtuelles de niveau web).  

2. Consultez les ressources de locataire de contrôleur de réseau pour toutes les erreurs. Exécutez la commande suivante à partir de n’importe quel hôte Hyper-V avec connectivité de couche 3 pour le contrôleur de réseau :  
      
   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>``

3. Pour vérifier que l’équilibrage de charge s’exécute correctement, exécutez la commande suivante à partir de n’importe quel hôte Hyper-V :
    
   ``wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing``
   
   où `<VIP IP address>` est l’adresse IP virtuelle que vous avez configuré dans le fichier TenantConfig.psd1 du niveau web. 

   >[!TIP]
   >Recherchez le `VIPIP` variable TenantConfig.psd1.

   Exécutez cette fois plusieurs pour voir l’équilibreur de charge de basculer entre les adresses IP dynamiques disponibles. Vous pouvez également observer ce comportement à l’aide d’un navigateur web. Accédez à `<VIP IP address>/unique.htm`. Fermez le navigateur et ouvrez une nouvelle instance et accédez de nouveau. Vous verrez la page bleue et la page verte autre, sauf lorsque le navigateur met en cache la page avant l’expiration de la mémoire cache.

---