---
title: Créer une nouvelle association de cartes réseau sur un ordinateur hôte ou une machine virtuelle
description: Dans cette rubrique, vous allez créer une nouvelle association de cartes réseau sur un ordinateur hôte ou dans une machine virtuelle Hyper-V exécutant Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-nict
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.openlocfilehash: d552460a94f4278c32f57130d7973ca5bdedca08
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854772"
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>Créer une nouvelle association de cartes réseau sur un ordinateur hôte ou une machine virtuelle

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous allez créer une nouvelle association de cartes réseau sur un ordinateur hôte ou dans une machine virtuelle Hyper-V exécutant Windows Server 2016.  

## <a name="network-configuration-requirements"></a>Configuration réseau requise  
Avant de pouvoir créer une nouvelle association de cartes réseau, vous devez déployer un ordinateur hôte Hyper-V avec deux cartes réseau qui se connectent à différents commutateurs physiques. Vous devez également configurer les cartes réseau avec des adresses IP qui se trouvent dans la même plage d’adresses IP.  

Le commutateur physique, le commutateur virtuel Hyper-V, le réseau local (LAN) et les exigences d’association de cartes réseau pour la création d’une association de cartes réseau dans une machine virtuelle sont les suivants :  

-   L’ordinateur exécutant Hyper-V doit avoir au moins deux cartes réseau.  

-   En cas de connexion des cartes réseau à plusieurs commutateurs physiques, les commutateurs physiques doivent se trouver sur le même sous-réseau de couche 2.  

-   Vous devez utiliser le Gestionnaire Hyper-V ou Windows PowerShell pour créer deux commutateurs virtuels Hyper-V externes, chacun étant connecté à une autre carte réseau physique.  

-   La machine virtuelle doit se connecter aux commutateurs virtuels externes que vous créez.  

-   L’Association de cartes réseau, dans Windows Server 2016, prend en charge les équipes avec deux membres dans des machines virtuelles. Vous pouvez créer des équipes de plus grande taille, mais il n’existe aucune prise en charge.

-   Si vous configurez une association de cartes réseau dans une machine virtuelle, vous devez sélectionner un **mode d’association** _indépendant_ et un mode d' **équilibrage de charge** de hachage d' _adresse_. 

## <a name="step-1-configure-the-physical-and-virtual-network"></a>Étape 1. Configurer le réseau physique et le réseau virtuel  
Dans cette procédure, vous créez deux commutateurs virtuels Hyper-V externes, vous connectez une machine virtuelle aux commutateurs, puis vous configurez les connexions de machines virtuelles aux commutateurs.  

### <a name="prerequisites"></a>Composants requis

Vous devez être membre du groupe **administrateurs**ou d’un groupe équivalent.  

### <a name="procedure"></a>Procédure

1.  Sur l’hôte Hyper-V, ouvrez le Gestionnaire Hyper-V, puis sous actions, cliquez sur **Gestionnaire de commutateur virtuel**.  

   ![Gestionnaire de commutateur virtuel](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv.jpg)  

2.  Dans le gestionnaire de commutateur virtuel, assurez-vous que l’option **externe** est sélectionnée, puis cliquez sur **créer un commutateur virtuel**.  

   ![Créer un commutateur virtuel](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv_02.jpg)  

3.  Dans Propriétés du commutateur virtuel, tapez un **nom** pour le commutateur virtuel et ajoutez des **Remarques** si nécessaire.  

4.  Dans **type de connexion**, dans **réseau externe**, sélectionnez la carte réseau physique à laquelle vous souhaitez attacher le commutateur virtuel.  

5.  Configurez des propriétés de commutateur supplémentaires pour votre déploiement, puis cliquez sur **OK**.  

6.  Créez un second commutateur virtuel externe en répétant les étapes précédentes. Connectez le deuxième commutateur externe à une autre carte réseau.  

7.  Dans le Gestionnaire Hyper-V, sous **ordinateurs virtuels**, cliquez avec le bouton droit sur la machine virtuelle que vous souhaitez configurer, puis cliquez sur **paramètres**.  

   La boîte de dialogue **paramètres** de machine virtuelle s’ouvre.

8.  Vérifiez que la machine virtuelle n’est pas démarrée. S’il est démarré, effectuez un arrêt avant de configurer la machine virtuelle.  

8.  Dans **matériel**, cliquez sur **carte réseau**.  

   ![Carte réseau](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_01.jpg)  

9. Dans Propriétés de la **carte réseau** , sélectionnez l’un des commutateurs virtuels que vous avez créés lors des étapes précédentes, puis cliquez sur **appliquer**.  

    ![Sélectionner un commutateur virtuel](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_02.jpg)  

10. Dans **matériel**, cliquez pour développer le signe plus (+) en regard de **carte réseau**. 

11. Cliquez sur **fonctionnalités avancées** pour activer l’Association de cartes réseau à l’aide de l’interface utilisateur graphique. 

    >[!TIP]
    >Vous pouvez également activer l’Association de cartes réseau avec une commande Windows PowerShell : 
    >
    >```PowerShell
    >Set-VMNetworkAdapter -VMName <VMname> -AllowTeaming On
    >```

    a. Sélectionnez **dynamique** pour adresse Mac. 

    b. Cliquez pour sélectionner le **réseau protégé**. 

    c. Cliquez pour sélectionner **activer cette carte réseau pour faire partie d’une équipe dans le système d’exploitation invité**. 

    d. Cliquez sur **OK**.  

    ![Ajouter une carte réseau à une équipe](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_05.jpg)  

13. Pour ajouter une deuxième carte réseau, dans le Gestionnaire Hyper-V, dans **ordinateurs virtuels**, cliquez avec le bouton droit sur la même machine virtuelle, puis cliquez sur **paramètres**.  

    La boîte de dialogue **paramètres** de machine virtuelle s’ouvre.

14. Dans **Ajouter un matériel**, cliquez sur **carte réseau**, puis sur **Ajouter**.  

   ![Ajouter une carte réseau](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_06.jpg)  

15. Dans Propriétés de la **carte réseau** , sélectionnez le deuxième commutateur virtuel que vous avez créé au cours des étapes précédentes, puis cliquez sur **appliquer**.  

   ![Appliquer un commutateur virtuel](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_07.jpg)  

16. Dans **matériel**, cliquez pour développer le signe plus (+) en regard de **carte réseau**. 

17. Cliquez sur **fonctionnalités avancées**, faites défiler jusqu’à **Association de cartes**réseau, puis cliquez pour sélectionner **activer cette carte réseau pour faire partie d’une équipe dans le système d’exploitation invité**. 

18. Cliquez sur **OK**.  

_**Félicitations!**_  Vous avez configuré le réseau physique et le réseau virtuel.  Vous pouvez maintenant passer à la création d’une nouvelle association de cartes réseau.  

## <a name="step-2-create-a-new-nic-team"></a>Étape 2. Créer une association de cartes réseau

Lorsque vous créez une nouvelle association de cartes réseau, vous devez configurer les propriétés de l’Association de cartes réseau :  

-   Nom de l’équipe  

-   Adaptateurs membres  

-   Mode d’association  

-   Mode d’équilibrage de charge  

-   Adaptateur de secours  

Vous pouvez également configurer l’interface d’équipe principale et configurer un numéro de réseau local virtuel (VLAN).  

Pour plus d’informations sur ces paramètres, consultez [paramètres d’association de cartes réseau](nic-teaming-settings.md).

### <a name="prerequisites"></a>Composants requis

Vous devez être membre du groupe **administrateurs**ou d’un groupe équivalent.  

### <a name="procedure"></a>Procédure

1. Dans le Gestionnaire de serveur, cliquez sur **Serveur local**.  

2. Dans le volet **Propriétés** , dans la première colonne, recherchez **Association de cartes réseau**, puis cliquez sur le lien **désactivé** .  

   La boîte de dialogue **Association de cartes réseau** s’ouvre.

   ![Boîte de dialogue Association de cartes réseau](../../media/Create-a-New-NIC-Team/nict_02_nicteaming.jpg)

3. Dans **adaptateurs et interfaces**, sélectionnez une ou plusieurs cartes réseau que vous souhaitez ajouter à une association de cartes réseau.  

4. Cliquez sur **tâches**, puis sur **Ajouter à la nouvelle équipe**.  

   La boîte **de dialogue nouvelle équipe** s’ouvre et affiche les cartes réseau et les membres de l’équipe.

5. Dans **nom**de l’équipe, tapez un nom pour la nouvelle association de cartes réseau, puis cliquez sur **propriétés supplémentaires**.  

6. Dans **propriétés supplémentaires**, sélectionnez les valeurs pour :

   - **Mode d’association**. Les options pour le mode d’association sont **indépendantes du commutateur** et dépendent du **commutateur**. Le mode dépendant du commutateur comprend l' **Association statique** et le **protocole LACP (Link Aggregation Control Protocol)** . 

     - **Indépendant du commutateur.** [!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)]

     - **Dépend du commutateur.** [!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)] 


       |                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
       |----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
       |              **Association statique**              |                                                                                                                                              Exige que vous configuriez manuellement le commutateur et l’hôte pour identifier les liens qui forment l’équipe. Étant donné qu’il s’agit d’une solution configurée de manière statique, il n’existe aucun protocole supplémentaire pour aider le commutateur et l’hôte à identifier les câbles mal branchés ou d’autres erreurs susceptibles de provoquer l’échec de l’équipe. Ce mode est généralement pris en charge par les commutateurs préconisés pour les serveurs.                                                                                                                                              |
       | **Protocole LACP (Link Aggregation Control Protocol)** | Contrairement à l’Association statique, le mode d’association LACP identifie de manière dynamique les liens qui sont connectés entre l’hôte et le commutateur. Cette connexion dynamique permet la création automatique d’une équipe et, en théorie, mais rarement en pratique, le développement et la réduction d’une équipe simplement par la transmission ou la réception de paquets LACP à partir de l’entité homologue. Tous les commutateurs de classe de serveur prennent en charge le protocole LACP, et tous requièrent que l’opérateur de réseau active à la fois le protocole LACP sur le port commuté. Quand vous configurez le mode d’association du protocole LACP, l’Association de cartes réseau fonctionne toujours en mode actif du protocole LACP.  Par défaut, l’Association de cartes réseau utilise un minuteur (3 secondes), mais vous pouvez configurer un minuteur long (90 secondes) avec `Set-NetLbfoTeam`. |

       ---

   - **Mode d’équilibrage de charge**. Les options pour le mode de distribution d’équilibrage de charge sont **hachage d’adresse**, **port Hyper-V**et **dynamique**.

     - **Hachage d’adresse.** [!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]

     - **Port Hyper-V.** [!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]

     - **Dynamique.** [!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]

   - **Adaptateur de secours**. Les options de l’adaptateur de secours sont **None (toutes les cartes actives)** ou votre sélection d’une carte réseau spécifique dans l’Association de cartes réseau qui agit comme un adaptateur de secours.

   > [!TIP]  
   > Si vous configurez une association de cartes réseau dans une machine virtuelle, vous devez sélectionner un **mode d’association** _indépendant_ et un mode d' **équilibrage de charge** de hachage d' _adresse_.  

7. Si vous souhaitez configurer le nom de l’interface d’équipe principale ou affecter un numéro de réseau local virtuel à l’Association de cartes réseau, cliquez sur le lien situé à droite de interface de l' **équipe principale**.  

    La boîte **de dialogue nouvelle interface d’équipe** s’ouvre.

    ![Nouvelle interface d’équipe](../../media/Create-a-New-NIC-Team/nict_newteaminterface.jpg)

8. Selon vos besoins, effectuez l’une des opérations suivantes :  

   -   Fournissez un nom d’interface tNIC.  

   -   Configurer l’appartenance au réseau local virtuel : cliquez sur un **réseau local virtuel spécifique** et tapez les informations du réseau local virtuel. Par exemple, si vous souhaitez ajouter cette association de cartes réseau au numéro de réseau local virtuel de comptabilité 44, tapez comptabilité 44-VLAN.   

9. Cliquez sur **OK**.  

_**Félicitations!**_  Vous avez créé une nouvelle association de cartes réseau sur un ordinateur hôte ou une machine virtuelle.

## <a name="related-topics"></a>Rubriques connexes

- [Association de cartes](NIC-Teaming.md)réseau : dans cette rubrique, nous vous proposons une vue d’ensemble de l’Association de cartes d’interface réseau (NIC) dans Windows Server 2016. L’Association de cartes réseau vous permet de grouper entre une et 32 cartes réseau Ethernet physiques dans une ou plusieurs cartes réseau virtuelles basées sur le logiciel. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.   

- [Utilisation et gestion de l’Association de cartes réseau avec l’adresse Mac](NIC-Teaming-MAC-Address-Use-and-Management.md): quand vous configurez une association de cartes réseau avec le mode indépendant du commutateur et l’adresse de hachage d’adresse ou de charge dynamique, l’équipe utilise l’adresse Mac (Media Access Control) du membre de l’équipe de cartes réseau principales sur le trafic sortant. Le membre de l’Association de cartes réseau principale est une carte réseau sélectionnée par le système d’exploitation à partir de l’ensemble initial des membres de l’équipe.

- [Paramètres d’association de cartes réseau](nic-teaming-settings.md): dans cette rubrique, nous vous offrons une vue d’ensemble des propriétés de l’équipe de cartes réseau, telles que les modes d’association et d’équilibrage de charge. Nous vous fournissons également des détails sur le paramètre de l’adaptateur de secours et la propriété de l’interface d’équipe principale. Si vous disposez d’au moins deux cartes réseau dans une association de cartes réseau, vous n’avez pas besoin de désigner une carte de secours pour la tolérance de panne.

- [Résolution des problèmes d’association de cartes](Troubleshooting-NIC-Teaming.md)réseau : dans cette rubrique, nous expliquons comment résoudre les problèmes liés à l’Association de cartes réseau, telles que le matériel, les titres de commutateur physique et la désactivation ou l’activation de cartes réseau à l’aide de Windows PowerShell. 

---
