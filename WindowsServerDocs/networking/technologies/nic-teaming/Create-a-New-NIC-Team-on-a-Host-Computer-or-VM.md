---
title: Créer une association de cartes réseau sur un ordinateur hôte ou d’une machine virtuelle
description: Dans cette rubrique, vous créez une nouvelle association de cartes réseau sur un ordinateur hôte ou sur une machine de virtuelle (VM) Hyper-V exécutant Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 5380cb2007bab1a296e0facc12885d47c6afc708
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282095"
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>Créer une association de cartes réseau sur un ordinateur hôte ou d’une machine virtuelle

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous créez une nouvelle association de cartes réseau sur un ordinateur hôte ou sur une machine de virtuelle (VM) Hyper-V exécutant Windows Server 2016.  

## <a name="network-configuration-requirements"></a>Configuration réseau requise  
Avant de pouvoir créer une association de cartes réseau, vous devez déployer un hôte Hyper-V avec deux cartes réseau qui se connectent à des commutateurs physiques différents. Vous devez également configurer les cartes réseau avec des adresses IP qui sont à partir de la même plage d’adresses IP.  

Le commutateur physique, le commutateur virtuel Hyper-V, réseau local (LAN) et les exigences d’association de cartes réseau pour la création d’une association de cartes réseau dans une machine virtuelle sont :  

-   L’ordinateur exécutant Hyper-V doit avoir au moins deux cartes réseau.  

-   Si vous vous connectez les cartes réseau à plusieurs commutateurs physiques, les commutateurs physiques doivent être sur le même sous-réseau de couche 2.  

-   Vous devez utiliser le Gestionnaire Hyper-V ou Windows PowerShell pour créer deux commutateurs virtuels Hyper-V externe, chacune connectée à une carte réseau physique différente.  

-   La machine virtuelle doit se connecter pour les deux commutateurs virtuels externes que vous créez.  

-   Association de cartes réseau, dans Windows Server 2016, prend en charge les équipes avec deux membres dans les machines virtuelles. Vous pouvez créer des équipes plus grandes, mais il n’existe aucune prise en charge.

-   Si vous configurez une association de cartes réseau dans une machine virtuelle, vous devez sélectionner un **mode d’association** de _indépendant du commutateur_ et un **mode d’équilibrage de charge** de _hachage d’adresse_. 

## <a name="step-1-configure-the-physical-and-virtual-network"></a>Étape 1. Configurer le réseau physique et virtuel  
Dans cette procédure, vous créez deux commutateurs virtuels Hyper-V externe, connectez une machine virtuelle pour les commutateurs, puis configurez les connexions de la machine virtuelle pour les commutateurs.  

### <a name="prerequisites"></a>Prérequis

Vous devez être membre du **administrateurs**, ou équivalent.  

### <a name="procedure"></a>Procédure

1.  Sur l’hôte Hyper-V, ouvrez le Gestionnaire Hyper-V, sous Actions, cliquez sur **Gestionnaire de commutateur virtuel**.  

   ![Gestionnaire de commutateur virtuel](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv.jpg)  

2.  Dans le Gestionnaire de commutateur virtuel, assurez-vous que **externe** est sélectionnée, puis cliquez sur **créer un commutateur virtuel**.  

   ![Créer un commutateur virtuel](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv_02.jpg)  

3.  Dans les propriétés du commutateur virtuel, tapez un **nom** pour le commutateur virtuel et ajoutez **Notes** en fonction des besoins.  

4.  Dans **type de connexion**, dans **réseau externe**, sélectionnez la carte réseau physique à laquelle vous souhaitez attacher le commutateur virtuel.  

5.  Configurer les propriétés du commutateur supplémentaire pour votre déploiement, puis cliquez sur **OK**.  

6.  Créer un deuxième commutateur virtuel externe en répétant les étapes précédentes. Se connecter à la deuxième commutateur externe à une autre carte réseau.  

7.  Dans le Gestionnaire Hyper-V, sous **Machines virtuelles**, avec le bouton droit de la machine virtuelle que vous souhaitez configurer, puis cliquez sur **paramètres**.  

   La machine virtuelle **paramètres** boîte de dialogue s’ouvre.

8.  Assurez-vous que la machine virtuelle n’est pas démarrée. Si elle est démarrée, effectuer un arrêt avant de configurer la machine virtuelle.  

8.  Dans **matériel**, cliquez sur **carte réseau**.  

   ![Carte réseau](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_01.jpg)  

9. Dans **carte réseau** propriétés, sélectionnez un des commutateurs virtuels que vous avez créé dans les étapes précédentes, puis cliquez sur **appliquer**.  

    ![Sélectionnez un commutateur virtuel](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_02.jpg)  

10. Dans **matériel**, cliquez pour développer le signe plus (+) à côté **carte réseau**. 

11. Cliquez sur **fonctionnalités avancées** pour activer l’association de cartes réseau à l’aide de l’interface utilisateur graphique. 

    >[!TIP]
    >Vous pouvez également activer l’association de cartes réseau avec une commande Windows PowerShell : 
    >
    >```PowerShell
    >Set-VMNetworkAdapter -VMName <VMname> -AllowTeaming On
    >```

    a. Sélectionnez **dynamique** adresse MAC. 

    b. Cliquez pour sélectionner **protégé réseau**. 

    c. Cliquez pour sélectionner **activer cette carte réseau faire partie d’une équipe dans le système d’exploitation invité**. 

    d. Cliquez sur **OK**.  

    ![Ajouter une carte réseau à une équipe](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_05.jpg)  

13. Pour ajouter une deuxième carte réseau, dans le Gestionnaire Hyper-V, dans **Machines virtuelles**, avec le bouton droit de la même machine virtuelle, puis cliquez sur **paramètres**.  

    La machine virtuelle **paramètres** boîte de dialogue s’ouvre.

14. Dans **Ajout de matériel**, cliquez sur **carte réseau**, puis cliquez sur **ajouter**.  

   ![Ajouter une carte réseau](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_06.jpg)  

15. Dans **carte réseau** propriétés, sélectionnez le deuxième commutateur virtuel que vous avez créé dans les étapes précédentes, puis cliquez sur **appliquer**.  

   ![Appliquer un commutateur virtuel](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_07.jpg)  

16. Dans **matériel**, cliquez pour développer le signe plus (+) à côté **carte réseau**. 

17. Cliquez sur **fonctionnalités avancées**, faites défiler jusqu'à **association de cartes réseau**, puis cliquez pour sélectionner **activer cette carte réseau faire partie d’une équipe dans le système d’exploitation invité**. 

18. Cliquez sur **OK**.  

_**Félicitations !** _  Vous avez configuré le réseau physique et virtuel.  Vous pouvez maintenant passer à la création d’une association de cartes réseau.  

## <a name="step-2-create-a-new-nic-team"></a>Étape 2. Créer une nouvelle association de cartes réseau

Lorsque vous créez une nouvelle association de cartes réseau, vous devez configurer les propriétés de l’association de cartes réseau :  

-   Nom de l’équipe  

-   Cartes membres  

-   Mode d’association  

-   Mode d’équilibrage de charge  

-   Adaptateur de secours  

Vous pouvez également configurer l’interface de l’équipe principale et configurer un nombre LAN (VLAN) virtuel.  

Pour plus d’informations sur ces paramètres, consultez [paramètres d’association de cartes réseau](nic-teaming-settings.md).

### <a name="prerequisites"></a>Prérequis

Vous devez être membre du **administrateurs**, ou équivalent.  

### <a name="procedure"></a>Procédure

1. Dans le Gestionnaire de serveur, cliquez sur **Serveur local**.  

2. Dans le **propriétés** volet, dans la première colonne, recherchez **association de cartes réseau**, puis cliquez sur le **désactivé** lien.  

   Le **association de cartes réseau** boîte de dialogue s’ouvre.

   ![Boîte de dialogue Association de cartes réseau](../../media/Create-a-New-NIC-Team/nict_02_nicteaming.jpg)

3. Dans **adaptateurs et les Interfaces**, sélectionnez l’une ou plusieurs cartes réseau que vous souhaitez ajouter à une association de cartes réseau.  

4. Cliquez sur **tâches**, puis cliquez sur **ajouter à la nouvelle équipe**.  

   Le **nouvelle équipe** boîte de dialogue s’ouvre et affiche les cartes réseau et les membres de l’équipe.

5. Dans **nom de l’équipe**, tapez un nom pour l’association de cartes réseau, puis cliquez sur **des propriétés supplémentaires**.  

6. Dans **des propriétés supplémentaires**, sélectionnez des valeurs pour :

   - **Mode d’association**. Les options de mode d’association sont **indépendant du commutateur** et **dépendants du commutateur**. Le commutateur dépendants mode inclut **association statique** et **protocole LACP (Link Aggregation contrôle Protocol)** . 

     - **Indépendant du commutateur.** [!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)]

     - **Dépendants du commutateur.** [!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)] 


       |                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
       |----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
       |              **Association statique**              |                                                                                                                                              Vous devez configurer manuellement le commutateur et l’hôte pour identifier le liens forment l’association. Comme il s’agit d’une solution configurée de manière statique, il n’existe aucun protocole supplémentaire afin de faciliter le commutateur et l’hôte pour identifier de manière incorrecte sur secteur câbles ou autres erreurs susceptibles de l’équipe ne parviennent pas à effectuer. Ce mode est généralement pris en charge par les commutateurs préconisés pour les serveurs.                                                                                                                                              |
       | **Link Aggregation Control Protocol (LACP)** | Contrairement à une association statique, mode d’association LACP identifie dynamiquement des liens qui sont connectés entre l’hôte et le commutateur. Cette connexion dynamique permet la création automatique d’une équipe et, en théorie, mais rarement dans la pratique, la réduction d’une association et l’extension simplement par la transmission ou la réception des paquets LACP à partir de l’entité pair. Tous les commutateurs de classe du serveur prend en charge le protocole LACP et requièrent tous l’opérateur de réseau à activer de manière administrative LACP sur le port de commutateur. Lorsque vous configurez un mode d’association du protocole LACP, association de cartes réseau fonctionne toujours en mode actif du protocole LACP avec un minuteur court.  Aucune option n’est actuellement disponible pour modifier la minuterie ou de modifier le mode LACP. |

       ---

   - **Mode d’équilibrage de charge**. Les options de mode de distribution d’équilibrage de charge sont **hachage d’adresse**, **Port Hyper-V**, et **dynamique**.

     - **Hachage d’adresse.** [!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]

     - **Port Hyper-V.** [!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]

     - **Dynamique.** [!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]

   - **Adaptateur de secours**. Les options de carte de mise en veille sont **None (toutes les cartes actif)** ou votre sélection d’une carte réseau spécifique dans l’association de cartes réseau qui agit comme une carte de mise en veille.

   > [!TIP]  
   > Si vous configurez une association de cartes réseau dans une machine virtuelle (VM), vous devez sélectionner un **mode d’association** de _indépendant du commutateur_ et un **mode d’équilibrage de charge** de  _Hachage d’adresses_.  

7. Si vous souhaitez configurer le nom de l’interface équipe principale ou affecter un numéro de réseau local virtuel à l’association de cartes réseau, cliquez sur le lien à droite de **interface d’équipe principale**.  

    Le **nouvelle interface d’équipe** boîte de dialogue s’ouvre.

    ![Nouvelle interface d’équipe](../../media/Create-a-New-NIC-Team/nict_newteaminterface.jpg)

8. Selon vos besoins, effectuez l’une des opérations suivantes :  

   -   Fournir un nom de l’interface tNIC.  

   -   Configurer l’appartenance au VLAN : cliquez sur **VLAN spécifique** et tapez les informations de réseau local virtuel. Par exemple, si vous souhaitez ajouter cette association de cartes réseau pour le réseau local virtuel comptabilité numéro 44, 44 de gestion des comptes de Type - VLAN.   

9. Cliquez sur **OK**.  

_**Félicitations !** _  Vous avez créé une association de cartes réseau sur un ordinateur hôte ou d’une machine virtuelle.

## <a name="related-topics"></a>Rubriques connexes

- [Association de cartes réseau](NIC-Teaming.md): Dans cette rubrique, nous vous donner une vue d’ensemble de l’association de carte d’Interface réseau (NIC) dans Windows Server 2016. Association de cartes réseau vous permet de regrouper entre 1 et 32 de cartes réseau Ethernet physiques dans un ou plusieurs adaptateurs de réseau virtuel basé sur le logiciel. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.   

- [Utilisation des adresses MAC de l’association de cartes réseau et la gestion](NIC-Teaming-MAC-Address-Use-and-Management.md): Lorsque vous configurez une association de cartes réseau avec le mode indépendant du commutateur et de hachage d’adresse ou distribution de charge dynamique, l’équipe utilise que l’accès au média (adresse MAC control) du membre principal association de cartes réseau sur le trafic sortant. Le membre d’association de cartes réseau principal est une carte réseau sélectionnée par le système d’exploitation à partir de l’ensemble initial de membres de l’équipe.

- [Paramètres d’association de cartes réseau](nic-teaming-settings.md): Dans cette rubrique, nous vous donner une vue d’ensemble des propriétés d’association de cartes réseau telles que l’association de cartes et modes d’équilibrage de charge. Nous vous donnons également plus d’informations sur le paramètre de carte de mise en veille et de la propriété d’interface équipe principale. Si vous avez au moins deux cartes réseau dans une association de cartes réseau, il est inutile de désigner une carte de mise en veille pour une tolérance de panne.

- [Résolution des problèmes d’association de cartes réseau](Troubleshooting-NIC-Teaming.md): Dans cette rubrique, nous abordons les façons de résoudre les problèmes d’association de cartes réseau, telles que le matériel, les titres de commutateur physique et la désactivation ou l’activation des cartes réseau à l’aide de Windows PowerShell. 

---