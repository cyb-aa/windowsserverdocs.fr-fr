---
title: Déployer une infrastructure réseau définie par logiciel à l’aide de scripts
description: Cette rubrique explique comment déployer une infrastructure SDN (Software Defined Network) à l’aide de scripts dans Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: lizross
author: eross-msft
ms.date: 08/23/2018
ms.openlocfilehash: 1a17d5f5fec0a05b4258b295eb37b6dc80cdaee1
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313052"
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>Déployer une infrastructure SDN (Software Defined Networking) avec des scripts

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous déployez une infrastructure SDN (Software Defined Network) à l’aide de scripts. L’infrastructure comprend un contrôleur de réseau à haute disponibilité (HA), un/MUX de Load Balancer logiciels à haute disponibilité (SLB), des réseaux virtuels et des listes de Access Control (ACL) associées. En outre, un autre script déploie une charge de travail de locataire pour vous assurer la validation de votre infrastructure SDN.  

Si vous souhaitez que les charges de travail de vos locataires communiquent en dehors de leurs réseaux virtuels, vous pouvez configurer des règles NAT SLB, des tunnels de passerelle de site à site ou le transfert de couche 3 vers le routage entre les charges de travail virtuelles et physiques.  

Vous pouvez également déployer une infrastructure SDN à l’aide de Virtual Machine Manager (VMM). Pour plus d’informations, consultez [configurer une infrastructure SDN (Software Defined Network) dans l’infrastructure VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-overview).  


## <a name="pre-deployment"></a>Pré-déploiement  

> [!IMPORTANT]  
> Avant de commencer le déploiement, vous devez planifier et configurer vos ordinateurs hôtes et votre infrastructure de réseau physique. Pour plus d’informations, consultez [Planifier une mise en réseau SDN (Software Defined Networking)](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md).  

Windows Server 2016 doit être installé sur tous les ordinateurs hôtes Hyper-V.  

## <a name="deployment-steps"></a>Étapes de déploiement  
Commencez par configurer le commutateur virtuel Hyper-V (serveurs physiques) de l’hôte Hyper-V et l’attribution d’adresses IP. Tout type de stockage compatible avec Hyper-V, partagé ou local, peut être utilisé.  

### <a name="install-host-networking"></a>Installer la mise en réseau des ordinateurs hôtes  

1. Installez les derniers pilotes réseau disponibles pour votre matériel de carte réseau.  
2. Installer le rôle Hyper-V sur tous les ordinateurs hôtes (pour plus d’informations, consultez [prise en main d’Hyper-v sur Windows Server 2016](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/Get-started-with-Hyper-V-on-Windows).   

   ```PowerShell
   Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart
   ```  

3. Créez le commutateur virtuel Hyper-V.<p>Utilisez le même nom de commutateur pour tous les ordinateurs hôtes, par exemple **sdnSwitch**. Configurez au moins une carte réseau ou, si vous utilisez SET, configurez au moins deux cartes réseau. Le nombre maximal de diffusions entrantes se produit lors de l’utilisation de deux cartes réseau.  

   ```PowerShell
   New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True
   ```  
   >[!TIP] 
   >Vous pouvez ignorer les étapes 4 et 5 Si vous avez des cartes réseau de gestion distinctes.

3. Reportez-vous à la rubrique planification ([planifier une infrastructure réseau définie par un logiciel](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) et contactez votre administrateur réseau pour obtenir l’ID du réseau local virtuel de gestion. Attachez le carte réseau virtuelle de gestion du commutateur virtuel nouvellement créé au réseau local virtuel de gestion. Vous pouvez omettre cette étape si votre environnement n’utilise pas de balises VLAN.  

   ```PowerShell
   Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True
   ```  

4. Reportez-vous à la rubrique planification ([planifier une infrastructure réseau à définition logicielle](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) et contactez votre administrateur réseau pour qu’il utilise des attributions d’adresses IP statiques ou DHCP pour attribuer une adresse IP au carte réseau virtuelle de gestion du vswitch nouvellement créé. L’exemple suivant montre comment créer une adresse IP statique et l’assigner au carte réseau virtuelle de gestion du vSwitch :  

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>
   ```  

5. Facultatif Déployez une machine virtuelle pour héberger des Active Directory Domain Services ([installer Active Directory Domain Services (niveau 100)](https://technet.microsoft.com/library/hh472162.aspx) et un serveur DNS.  

    a. Connectez la machine virtuelle du serveur Active Directory/DNS au réseau local virtuel de gestion :

       ```PowerShell
       Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True  
       ```   

   b. Installez Active Directory Domain Services et DNS.  

   >[!NOTE]
   >Le contrôleur de réseau prend en charge les certificats Kerberos et X. 509 pour l’authentification. Ce guide utilise les deux mécanismes d’authentification à des fins différentes (même si un seul est requis).  

6. Joignez tous les ordinateurs hôtes Hyper-V au domaine. Vérifiez que l’entrée de serveur DNS pour la carte réseau qui a une adresse IP affectée au réseau de gestion pointe vers un serveur DNS capable de résoudre le nom de domaine. 

   ```PowerShell   
   Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>  
   ```

   a. Cliquez avec le bouton droit sur **Démarrer**, cliquez sur **système**, puis sur **modifier les paramètres**.  
   b. Cliquez sur **Modifier**.  
   c. Cliquez sur **domaine** et spécifiez le nom de domaine.  
   d. Cliquez sur **OK**.  
   e. Lorsque vous y êtes invité, tapez les informations d’identification du nom d’utilisateur et du mot de passe.  
   f. Redémarrez le serveur.  

### <a name="validation"></a>Validation  
Procédez comme suit pour vérifier que la mise en réseau de l’ordinateur hôte est correctement configurée.  

1. Vérifiez que le commutateur d’ordinateur virtuel a été créé avec succès :  

   ```PowerShell
   Get-VMSwitch "<switch name>"
   ```  

2. Vérifiez que le carte réseau virtuelle de gestion sur le commutateur de machine virtuelle est connecté au réseau local virtuel de gestion :  

   >[!NOTE]
   >Concerne uniquement si la gestion et le trafic de locataire partagent la même carte réseau.    

   ```PowerShell
   Get-VMNetworkAdapterIsolation -ManagementOS
   ```

3. Validez tous les ordinateurs hôtes Hyper-V et les ressources de gestion externes, par exemple, les serveurs DNS.<p>Vérifiez qu’ils sont accessibles via la commande ping à l’aide de leur adresse IP de gestion et/ou nom de domaine complet (FQDN).   

   ``ping <Hyper-V Host IP>``  
   ``ping <Hyper-V Host FQDN>``  

4. Exécutez la commande suivante sur l’hôte de déploiement et spécifiez le nom de domaine complet de chaque hôte Hyper-V pour vous assurer que les informations d’identification Kerberos utilisées permettent d’accéder à tous les serveurs.  

   ``winrm id -r:<Hyper-V Host FQDN>``  

### <a name="nano-installation-requirements-and-notes"></a>Remarques et conditions requises pour l’installation de nano  

Si vous utilisez Nano comme hôtes Hyper-V (serveurs physiques) pour le déploiement, les conditions suivantes sont requises :  

1. Pour tous les nœuds nano, le package DSC doit être installé avec le module linguistique :  

   - Microsoft-NanoServer-DSC-Package. cab  
   - Microsoft-serveur-DSC-Package_en-US. cab

     ``dism /online /add-package /packagepath:<Path> /loglevel:4``  

2. Les scripts SDN Express doivent être exécutés à partir d’un ordinateur hôte non nano (Windows Server Core ou Windows Server avec interface graphique utilisateur). Les workflows PowerShell ne sont pas pris en charge sur nano.  

3. L’appel de l’API NorthBound du contrôleur de réseau à l’aide des wrappers PowerShell ou REST de NC (qui reposent sur Invoke-WebRequest et Invoke-RestMethod) doit être effectué à partir d’un hôte non nano.  


### <a name="run-sdn-express-scripts"></a>Exécuter des scripts SDN Express  

1. Accédez au [dépôt github Microsoft SDN](https://github.com/Microsoft/SDN.git) pour les fichiers d’installation.

2. Téléchargez les fichiers d’installation à partir du référentiel vers l’ordinateur de déploiement désigné. Cliquez sur **cloner ou télécharger** , puis sur Télécharger le fichier **zip**.  

   >[!NOTE]
   >L’ordinateur de déploiement désigné doit exécuter Windows Server 2016 ou une version ultérieure.

3. Développez le fichier zip et copiez le dossier **SDNExpress** dans le dossier `C:\` de l’ordinateur de déploiement.  

4. Partagez le dossier `C:\SDNExpress` en tant que «**SDNExpress**» avec l’autorisation de **lecture/écriture**pour **tout le monde** .  

5. Accédez au dossier `C:\SDNExpress`.<p>Les dossiers suivants s’affichent :  


   | Folder Name |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
   |-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |  AgentConf  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   Contient des copies récentes des schémas OVSDB utilisés par l’agent hôte SDN sur chaque hôte Hyper-V de Windows Server 2016 pour programmer la stratégie réseau.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
   |    Certificats    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         Emplacement partagé temporaire pour le fichier de certificat NC.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |   Images    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         Vide, placez votre image vhdx Windows Server 2016 ici                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
   |    Outils    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Utilitaires pour le dépannage et le débogage.  Copié sur les ordinateurs hôtes et les ordinateurs virtuels.  Nous vous recommandons de placer Moniteur réseau ou Wireshark ici afin qu’il soit disponible si nécessaire.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
   |   scripts ;   | Scripts de déploiement.<br /><br />-   **SDNExpress. ps1**<br />    Déploie et configure l’infrastructure, y compris les machines virtuelles du contrôleur de réseau, les machines virtuelles SLB MUX, les pools de passerelle et les machines virtuelles de la passerelle HNV correspondant au (x) pool (s).<br />-   **FabricConfig. psd1**<br />    Modèle de fichier de configuration pour le script SDNExpress.  Vous allez personnaliser cela pour votre environnement.<br />-   **SDNExpressTenant. ps1**<br />    Déploie un exemple de charge de travail de locataire sur un réseau virtuel avec une adresse IP virtuelle à charge équilibrée.<br />    Configure également une ou plusieurs connexions réseau (VPN S2S IPSec, GRE, L3) sur les passerelles de périphérie du fournisseur de services qui sont connectées à la charge de travail du locataire précédemment créée. Les passerelles IPSec et GRE sont disponibles pour la connectivité sur l’adresse IP VIP correspondante et la passerelle de transfert L3 sur le pool d’adresses correspondant.<br />    Ce script peut être utilisé pour supprimer la configuration correspondante avec une option Annuler.<br />-   **TenantConfig. psd1**<br />    Fichier de configuration de modèle pour la charge de travail du locataire et la configuration de passerelle S2S.<br />-   **SDNExpressUndo. ps1**<br />    Nettoie l’environnement d’infrastructure et le réinitialise à l’état de démarrage.<br />-   **SDNExpressEnterpriseExample. ps1**<br />    Provisionne un ou plusieurs environnements de site d’entreprise avec une seule passerelle d’accès à distance et (éventuellement) une machine virtuelle d’entreprise correspondante par site. Les passerelles d’entreprise IPSec ou GRE se connectent à l’adresse IP VIP correspondante de la passerelle du fournisseur de services pour établir les tunnels S2S. La passerelle de transfert L3 se connecte à l’adresse IP homologue correspondante. <br />            Ce script peut être utilisé pour supprimer la configuration correspondante avec une option Annuler.<br />-   **EnterpriseConfig. psd1**<br />    Fichier de configuration de modèle pour la passerelle de site à site d’entreprise et la configuration de machine virtuelle cliente. |
   | TenantApps  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             Fichiers utilisés pour déployer des exemples de charges de travail de locataire.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

   ---

6. Vérifiez que le fichier VHDX Windows Server 2016 se trouve dans le dossier **images** .  

7. Personnalisez le fichier SDNExpress\scripts\FabricConfig.psd1 en modifiant le **< < remplacez >** balises > par des valeurs spécifiques pour les adapter à votre infrastructure de laboratoire, y compris les noms d’hôte, les noms de domaine, les noms d’utilisateur et les mots de passe, ainsi que les informations réseau pour les réseaux listés dans la rubrique réseau de planification.  

8. Créez un enregistrement A hôte A dans DNS pour NetworkControllerRestName (FQDN) et NetworkControllerRestIP.  

9. Exécutez le script en tant qu’utilisateur avec les informations d’identification d’administrateur de domaine :  

   ``SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  

10. Pour annuler toutes les opérations, exécutez la commande suivante :  

    ``SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  

#### <a name="validation"></a>Validation  

En supposant que le script SDN Express s’est exécuté jusqu’à la fin sans signaler d’erreurs, vous pouvez effectuer l’étape suivante pour vous assurer que les ressources de l’infrastructure ont été déployées correctement et sont disponibles pour le déploiement du locataire.  

Utilisez [outils de diagnostic](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack) pour vous assurer qu’il n’y a aucune erreur sur les ressources de l’infrastructure dans le contrôleur de réseau.  

   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>``  


### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>Déployer un exemple de charge de travail de locataire avec l’équilibreur de charge logiciel  

Maintenant que les ressources d’infrastructure ont été déployées, vous pouvez valider votre déploiement SDN de bout en bout en déployant un exemple de charge de travail de locataire. Cette charge de travail de locataire se compose de deux sous-réseaux virtuels (couche Web et couche de base de données) protégés Access Control via des règles de liste de contrôle d’accès (ACL) à l’aide du pare-feu distribué SDN. Le sous-réseau virtuel de la couche Web est accessible via le SLB/MUX à l’aide d’une adresse IP virtuelle (VIP). Le script déploie automatiquement deux machines virtuelles de niveau Web et une machine virtuelle de niveau base de données et les connecte aux sous-réseaux virtuels.  

1.  Personnalisez le fichier SDNExpress\scripts\TenantConfig.psd1 en modifiant le **< < remplacez >** balises > par des valeurs spécifiques (par exemple : nom de l’image de disque dur virtuel, nom du contrôleur de réseau, nom du vswitch, etc. comme précédemment défini dans le fichier FabricConfig. psd1)  

2.  Exécutez le script. Par exemple :  

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

3.  Pour annuler la configuration, exécutez le même script avec le paramètre **Undo** . Par exemple :  

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

#### <a name="validation"></a>Validation  

Pour vérifier que le déploiement du client a réussi, procédez comme suit :

1. Connectez-vous à la machine virtuelle de la couche base de données et essayez d’exécuter une commande ping sur l’adresse IP de l’une des machines virtuelles de la couche Web (Vérifiez que le pare-feu Windows est désactivé sur les machines virtuelles de niveau Web).  

2. Recherchez des erreurs dans les ressources du client du contrôleur de réseau. Exécutez la commande suivante à partir de n’importe quel ordinateur hôte Hyper-V avec une connectivité de couche 3 vers le contrôleur de réseau :  

   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>``

3. Pour vérifier que l’équilibreur de charge s’exécute correctement, exécutez la commande suivante à partir de n’importe quel ordinateur hôte Hyper-V :

   ``wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing``

   où `<VIP IP address>` est l’adresse IP VIP du niveau Web que vous avez configurée dans le fichier TenantConfig. psd1. 

   >[!TIP]
   >Recherchez la variable `VIPIP` dans TenantConfig. psd1.

   Exécutez ce plusieurs fois pour voir le basculement de l’équilibreur de charge entre les DIP disponibles. Vous pouvez également observer ce comportement à l’aide d’un navigateur Web. Accédez à `<VIP IP address>/unique.htm`. Fermez le navigateur et ouvrez une nouvelle instance et parcourez à nouveau. La page bleue et la page verte s’affichent, sauf lorsque le navigateur met en cache la page avant que le cache n’expire.

---