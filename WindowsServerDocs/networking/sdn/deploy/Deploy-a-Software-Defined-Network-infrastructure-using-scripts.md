---
title: Déployer une Infrastructure de réseau définies de logiciels à l’aide de Scripts
description: Cette rubrique explique comment déployer une infrastructure de réseau de défini des logiciels Microsoft (SDN) à l’aide de scripts dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4428ad73ab8933510d5a759ec4fa7377ea222ebd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>Déployer une Infrastructure de réseau définies de logiciels à l’aide de Scripts

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique explique comment déployer une infrastructure de réseau de défini des logiciels Microsoft (SDN) à l’aide de scripts. L’infrastructure comprend un contrôleur de réseau (HA) hautement disponible, une haute disponibilité logiciel charge Équilibrage (SLB) / MUX, des réseaux virtuels et associé de listes de contrôle d’accès (ACL). En outre, un autre script déploie une charge de travail client pour valider votre infrastructure SDN.  
  
Si vous souhaitez que vos charges de travail clientes pour communiquer en dehors de leurs réseaux virtuels, vous pouvez définir des règles SLB NAT, des tunnels de passerelle de Site ou transfert de couche 3 pour acheminer entre les charges de travail physiques et virtuels.  
  
Vous pouvez également déployer une infrastructure SDN à l’aide de Virtual Machine Manager (VMM). Pour plus d’informations, voir [configurer une infrastructure logicielle définie réseau (SDN) dans l’infrastructure VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-overview).  
  
## <a name="pre-deployment"></a>Avant le déploiement  
  
> [!IMPORTANT]  
> Avant de commencer le déploiement, vous devez planifier et configurer vos ordinateurs hôtes et une infrastructure réseau physique. Pour plus d’informations, voir [planifier une Infrastructure de réseau défini par logiciel](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md).  
  
Tous les ordinateurs hôtes Hyper-V doivent disposer de Windows Server2016 installé.  
  
## <a name="deployment-steps"></a>Étapes de déploiement  
Commencez par configurer le commutateur virtuel Hyper-V de (serveurs physiques) et l’attribution d’adresses IP de l’ordinateur hôte Hyper-V. N’importe quel type de stockage est compatible avec Hyper-V, partagé ou local peut être utilisée.  
### <a name="install-host-networking"></a>Installer l’ordinateur hôte de la mise en réseau  
1. Installez les derniers pilotes réseau disponibles pour votre matériel de carte réseau.  
2. Installer le rôle Hyper-V sur tous les ordinateurs hôtes (pour plus d’informations, voir [prise en main Hyper-V sur Windows Server2016](https://technet.microsoft.com/en-us/library/mt126159.aspx).   
  
   À partir d’une invite de commandes avec élévation de privilèges PowerShellcommand Windows:  
   ``Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart``  
    
    un. Créer le commutateur virtuel Hyper-V (utilisez le même nom de commutateur pour tous les ordinateurs hôtes. Par exemple: **sdnSwitch**). Configurer au moins une carte réseau ou, si vous utilisez Switch Embedded Teaming, configurez au moins deux cartes réseau. Propagation entrant maximale se produit lors de l’utilisation de deux cartes réseau.  
 `` New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True``  
 
 >[!NOTE] 
 >Vous pouvez ignorer les étapes3 et 4 Si vous disposez de gestion de cartes réseau distinctes.

3. Reportez-vous à la rubrique Planification ([planifier une Infrastructure de réseau défini par logiciel](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) et fonctionnent avec votre administrateur réseau pour obtenir l’ID de VLAN de la gestion de VLAN. Attacher la carte réseau virtuelle de gestion du commutateur virtuel qui vient d’être créé pour le réseau local virtuel de gestion. Cette étape peut être omise si votre environnement n’utilise pas les étiquettes VLAN.  
 `` Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True``  
 
4. Reportez-vous à la rubrique Planification ([planifier une Infrastructure de réseau défini par logiciel](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) et fonctionnent avec votre administrateur réseau pour utiliser DHCP soit ou les affectations IP statiques pour attribuer une adresse IP à la carte réseau virtuelle de gestion du commutateur virtuel nouvellement créé. L’exemple suivant montre comment créer une adresse IP statique et l’affecter à la carte réseau virtuelle de gestion du commutateur virtuel:  
 ``New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>``  
      
5. [Facultatif] Déployer un ordinateur virtuel pour héberger des Services de domaine ActiveDirectory ([installer ActiveDirectory Domain Services (niveau 100)](https://technet.microsoft.com/library/hh472162.aspx) et un serveur DNS.  
   
    un. Se connecter à l’ordinateur virtuel ActiveDirectory/serveur DNS pour le réseau local virtuel de gestion:
    
            Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True  
   
   b. Installer DNS et les Services de domaine ActiveDirectory.  
      >[!NOTE]
      >Le contrôleur de réseau prend en charge les certificats X.509 et Kerberos pour l’authentification. Ce guide utilise les deux mécanismes d’authentification à des fins différentes (bien qu’une seule est nécessaire).  
        
6. Joindre tous les hôtes Hyper-V au domaine. Vérifiez que l’entrée du serveur DNS pour la carte réseau qui a une adresse IP assignée aux points de réseau de gestion vers un serveur DNS qui peut résoudre le nom de domaine. Par exemple:

        Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>  
   
   un. Avec le bouton droit **Démarrer**, cliquez sur **système**, puis cliquez sur **modifier les paramètres**.  
   b. Cliquez sur **modification**.  
   c. Cliquez sur **domaine** et spécifiez le nom de domaine.  
   d. Cliquez sur **OK**.  
   e. Tapez les nom et mot de passe informations d’identification utilisateur lorsque vous y êtes invité.  
   f. Redémarrez le serveur.  
  
### <a name="validation"></a>Validation  
Utilisez la procédure suivante pour valider cet hôte mise en réseau est configuré correctement.  
1. Assurez-vous que le commutateur de machine virtuelle a été créé avec succès:  
      
    ``Get-VMSwitch "<switch name>"``  
2. Vérifiez que la carte réseau virtuelle de gestion sur le commutateur de machine virtuelle est connecté à la gestion de réseau local virtuel:  
    >[!NOTE]
    >Uniquement si le trafic de client et de gestion partage la même carte réseau.    
      
    ``Get-VMNetworkAdapterIsolation -ManagementOS``  
3. Valider que tous les ordinateurs hôtes Hyper-V (et les ressources de gestion externe, par exemple: serveurs DNS) sont accessibles via une commande ping à l’aide de leur adresse IP de la gestion et/ou le nom de domaine complet (FQDN).   
      
   ``ping <Hyper-V Host IP>``  
   ``ping <Hyper-V Host FQDN>``  
4. Exécutez la commande suivante sur l’hôte de déploiement et spécifiez le nom de domaine complet de chaque ordinateur hôte Hyper-V pour vérifier les informations d’identification Kerberos utilisées fournit l’accès à tous les serveurs.  
      
   ``winrm id -r:<Hyper-V Host FQDN>``  
      
### <a name="nano-installation-requirements-and-notes"></a>Notes de publication Nano configuration requise et installation  
Si vous utilisez Nano en tant que vos ordinateurs hôtes Hyper-V (serveurs physiques) pour le déploiement, les conditions suivantes se supplémentaires:  
1. Tous les nœuds Nano doivent disposer du package DSC installé avec le pack de langue:  
   
   * Microsoft-NanoServer-DSC-Package.cab  
   * Microsoft-NanoServer-DSC-Package_en-us.cab
   
        ``dism /online /add-package /packagepath:<Path> /loglevel:4``  
2. Les scripts SDN Express doivent être exécutés à partir d’un ordinateur hôte non Nano (WindowsServerCore ou Windows Server avec une interface graphique utilisateur). Les Workflows PowerShell ne sont pas pris en charge sur Nano.  
3.  Appeler l’API northbound du contrôleur de réseau à l’aide de PowerShell ou les Wrappers reste CN (qui s’appuient sur Invoke-WebRequest et Invoke-RestMethod) doit être effectuée à partir d’un ordinateur hôte non Nano.  
   
         
### <a name="run-sdn-express-scripts"></a>Exécuter des Scripts Express SDN  
  
1.  Les fichiers d’installation sont trouvent sur GitHub. Téléchargez le fichier zip à partir de la [référentiel GitHub de SDN Microsoft](https://github.com/Microsoft/SDN.git). Sur la page de référentiel SDN Microsoft, cliquez sur **clonage ou téléchargement** puis cliquez sur **télécharger le ZIP**.  
  
2.  Désigner un ordinateur en tant qu’ordinateur de votre déploiement.  Cet ordinateur doit exécuter Windows Server2016. Développez le fichier zip et copie les **SDNExpress** dossier à l’ordinateur de déploiement `C:\`dossier.  
  
3.  Partage les `C:\SDNExpress`dossier en tant que «**SDNExpress**» avec une autorisation pour **tout le monde** à **en lecture/écriture **.  
  
4.  Accédez à la `C:\SDNExpress`dossier.

 Vous verrez les dossiers suivants:  

|Nom de dossier|Description|  
|---------------|---------------|  
|AgentConf|Contient de nouvelles copies des schémas OVSDB utilisés par l’Agent d’ordinateur hôte SDN sur chaque ordinateur hôte Windows Server2016 Hyper-V pour la stratégie du programme de réseau.|  
|Certificats|Emplacement partagé temporaire pour le fichier de certificat CN.|  
|Images|Vide, placez votre image vhdx de Windows Server2016 ici|  
|Outils|Utilitaires de résolution des problèmes et de débogage.  Copiés sur les ordinateurs hôtes et les ordinateurs virtuels.  Nous vous conseillons de placer le Moniteur réseau ou Wireshark ici afin qu’il soit disponible si nécessaire.|  
|Scripts|Scripts de déploiement.<br /><br />-   **SDNExpress.ps1**<br />    Déploie et configure l’infrastructure, y compris les ordinateurs virtuels du contrôleur réseau, SLB multiplexer des ordinateurs virtuels, les pools de passerelle et les ordinateurs virtuels de passerelle HNV correspondant aux pools de.<br />-   **FabricConfig.psd1**<br />    Un modèle de fichier de configuration pour le script SDNExpress.  Vous allez le personnaliser pour votre environnement.<br />-   **SDNExpressTenant.ps1**<br />    Déploie une charge de travail cliente exemple sur un réseau virtuel avec une adresse IP virtuelle d’équilibrage de charge.<br />    Configure également un ou plusieurs connexions réseau (VPN S2S de IPSec, GRE, L3) sur les passerelles de périmètre de fournisseur de service qui sont connectés à la charge de travail client créé précédemment. Les passerelles IPSec et GRE sont disponibles pour la connectivité sur l’adresse IP adresse IP virtuelle correspondante et la passerelle de transfert L3 sur le pool d’adresses correspondant.<br />    Ce script peut être utilisé pour supprimer la configuration correspondante avec une option Undo également.<br />-   **TenantConfig.psd1**<br />    Un fichier de configuration de modèle pour la charge de travail client et la configuration de la passerelle S2S.<br />-   **SDNExpressUndo.ps1**<br />    Nettoie l’environnement d’infrastructure et réinitialise à un état de départ.<br />-   **SDNExpressEnterpriseExample.ps1**<br />    Configure un ou plusieurs environnements site d’entreprise avec une passerelle d’accès à distance et (éventuellement) un ordinateur virtuel d’entreprise correspondant par site. Les passerelles entreprise IPSec ou GRE se connecte à l’adresse IP de l’adresse IP virtuelle correspondante de la passerelle du fournisseur de service pour établir les tunnels S2S. La passerelle de transfert L3 se connecte via l’adresse IP homologue correspondante. <br />            Ce script peut être utilisé pour supprimer la configuration correspondante avec une option Undo également.<br />-   **EnterpriseConfig.psd1**<br />    Un fichier de configuration de modèle pour la passerelle de site d’entreprise et la configuration d’ordinateur virtuel du Client.|  
|TenantApps|Fichiers utilisés pour déployer des charges de travail clientes exemple.|  
  
5.  Vérifiez le fichier VHDX de Windows Server2016 est dans le **Images** dossier.  
  
6. Personnaliser le fichier SDNExpress\scripts\FabricConfig.psd1 en modifiant le **<< remplacer >>** balises avec des valeurs spécifiques pour s’adapter à votre infrastructure du laboratoire, y compris les noms d’hôte, les noms de domaine, les noms d’utilisateur et mots de passe et les informations réseau pour les réseaux répertoriés dans la rubrique Planification de réseau.  
7. Créer un enregistrement hôte A dans DNS pour les NetworkControllerRestIP NetworkControllerRestName (FQDN).  
8. Exécutez le script en tant qu’utilisateur avec les informations d’identification d’administrateur de domaine:  
      
    ``SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
9.  Pour annuler toutes les opérations, exécutez la commande suivante:  
      
    ``SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
#### <a name="validation"></a>Validation  
En supposant que le script SDN Express s’est terminé sans signaler les erreurs, vous pouvez effectuer l’étape suivante pour vérifier les ressources d’infrastructure ont été déployés correctement et sont disponibles pour le déploiement du client.  

- Utilisez [des outils de Diagnostic](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack) pour vous assurer qu’aucune erreur sur toutes les ressources de fabric dans le contrôleur de réseau.  
      
    ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>``  
        
   
### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>Déployer une charge de travail cliente exemple avec l’équilibrage de charge logicielle  
    
Maintenant que les ressources d’infrastructure ont été déployées, vous pouvez valider votre SDN déploiement de bout en bout en déployant une charge de travail client exemple. Cette charge de travail client se compose de deux sous-réseaux virtuels (niveau web et le niveau de base de données) protégés par le biais des règles de liste de contrôle d’accès (ACL) à l’aide du pare-feu SDN distribué. Sous-réseau virtuel au niveau web est accessible via le SLB/MUX à l’aide d’une adresse IP virtuelle (VIP). Le script déploie deux machines virtuelles de niveau web et une base de données niveau virtuels automatiquement et connecte les sous-réseaux virtuels.  
  
1.  Personnaliser le fichier SDNExpress\scripts\TenantConfig.psd1 en modifiant le **<< remplacer >>** balises avec des valeurs spécifiques (par exemple: nom, nom du contrôleur de réseau reste, nom de commutateur virtuel, etc. précédemment définies dans le fichier FabricConfig.psd1 d’image disque dur virtuel)  
2.  Exécutez le script. Par exemple:  
``SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose``  
3.  Pour annuler la configuration, exécutez le script même avec le **Annuler** paramètre. Par exemple:  
``SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

#### <a name="validation"></a>Validation  
Pour valider que le déploiement du client a réussi, procédez comme suit:
1.  Connectez-vous à l’ordinateur virtuel de niveau de base de données et effectuez un test ping de l’adresse IP de l’un des ordinateurs virtuels niveau web (Vérifiez le que pare-feu Windows est désactivé dans les ordinateurs virtuels de la couche web).  
2.  Vérifiez les ressources de client de contrôleur de réseau pour les erreurs. Exécutez la commande suivante à partir de n’importe quel ordinateur hôte Hyper-V avec une connexion de couche 3 au contrôleur de réseau:  
      
    ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>``
3. Pour vérifier que l’équilibrage de charge s’exécute correctement, exécutez la commande suivante à partir de n’importe quel ordinateur hôte Hyper-V:
    
        wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing
   
   Où `<VIP IP address>`est l’adresse IP de l’adresse IP virtuelle configurée dans le fichier TenantConfig.psd1 du niveau web. Recherchez le `VIPIP`variable dans TenantConfig.psd1.

   Exécuter cette fois plusieurs pour voir l’équilibrage de charge de basculer entre les DIP disponibles. Vous pouvez également observer ce comportement à l’aide d’un navigateur web. Accédez à `<VIP IP address>/unique.htm`. Fermez le navigateur, ouvrez une nouvelle instance et accédez de nouveau. Vous verrez la page bleue et l’autre, sauf lorsque le navigateur met en cache la page avant l’expiration de la mémoire cache de page verte.