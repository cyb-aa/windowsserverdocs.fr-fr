---
title: Gérer la stratégie QoS
description: Cette rubrique fournit des instructions sur la création et la gestion de la stratégie de qualité de service (QoS) dans Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 04fdfa54-6600-43d4-8945-35f75e15275a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ac717555d1ab751600527e294d32f10d1f05bfa5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395877"
---
# <a name="manage-qos-policy"></a>Gérer la stratégie QoS

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur l’utilisation de l’Assistant stratégie de QoS pour créer, modifier ou supprimer une stratégie de QoS.

>[!NOTE]
>  En plus de cette rubrique, la documentation suivante sur la gestion des stratégies de QoS est disponible.
> 
>  - [Événements et erreurs de stratégie QoS](qos-policy-errors.md)

Dans les systèmes d’exploitation Windows, la stratégie de qualité de service combine les fonctionnalités de la qualité de service basée sur les normes avec la facilité de gestion de stratégie de groupe. La configuration de cette combinaison simplifie l’application des stratégies QoS aux objets stratégie de groupe. Windows comprend un Assistant stratégie de QoS qui vous aide à effectuer les tâches suivantes.

-  [Créer une stratégie de QoS](#bkmk_createpolicy)

-  [Afficher, modifier ou supprimer une stratégie de QoS](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>Créer une stratégie de QoS

Avant de créer une stratégie de qualité de service, il est important de comprendre les deux principaux contrôles QoS utilisés pour gérer le trafic réseau :

- Valeur DSCP

-   Taux d’accélération

### <a name="prioritizing-traffic-with-dscp"></a>Définition de la priorité du trafic avec DSCP

Comme indiqué dans l’exemple d’application sectorielle précédente, vous pouvez définir la priorité du trafic réseau sortant à l’aide de l’indication de la **valeur DSCP** pour configurer une stratégie de QoS avec une valeur DSCP spécifique. 

Comme décrit dans la norme RFC 2474, DSCP autorise la spécification de valeurs comprises entre 0 et 63 dans le champ OT d’un paquet IPv4 et dans le champ de classe de trafic dans IPv6. Les routeurs réseau utilisent la valeur DSCP pour classer les paquets réseau et les faire défiler de manière appropriée.
  
> [!NOTE]
>  Par défaut, le trafic Windows a une valeur DSCP égale à 0.
  
Le nombre de files d’attente et la définition de leur priorité doivent être décidés dans le cadre de la stratégie de QoS de votre organisation. Par exemple, votre organisation peut choisir d’avoir cinq files d’attente : le trafic sensible à la latence, le trafic de contrôle, le trafic critique pour l’entreprise, le trafic du meilleur effort et le trafic de transfert de données en bloc.  
  
### <a name="throttling-traffic"></a>Limitation du trafic

Outre les valeurs DSCP, la limitation est un autre contrôle clé pour la gestion de la bande passante réseau. Comme mentionné précédemment, vous pouvez utiliser le paramètre **spécifier le taux d’accélération** pour configurer une stratégie de QoS avec un taux de limitation spécifique pour le trafic sortant. En utilisant la limitation, une stratégie de QoS limite le trafic réseau sortant à un taux d’accélération spécifié. Le marquage DSCP et la limitation de bande passante peuvent être utilisés conjointement pour gérer efficacement le trafic.

>[!NOTE]
>Par défaut, la case à cocher **Spécifier le taux d’accélération** n’est pas activée.

Pour créer une stratégie de QoS, modifiez les paramètres d’un objet stratégie de groupe (GPO) à partir de l’outil Console de gestion des stratégies de groupe (GPMC). Ensuite, la console GPMC ouvre l’éditeur d’objets stratégie de groupe.

Les noms des stratégies de QoS doivent être uniques. La façon dont les stratégies sont appliquées aux serveurs et aux utilisateurs finaux dépend de l’emplacement de stockage de la stratégie de QoS dans l’éditeur d’objets stratégie de groupe :

- Une stratégie de QoS dans la stratégie de configuration de Settings\QoS de l’ordinateur est appliquée aux ordinateurs, quel que soit l’utilisateur actuellement connecté. Les stratégies de QoS basées sur un ordinateur sont généralement utilisées pour les ordinateurs serveurs.

- Une stratégie de QoS dans la stratégie d’Settings\QoS de configuration utilisateur s’applique aux utilisateurs une fois qu’ils sont connectés, quel que soit l’ordinateur sur lequel ils se sont connectés.

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>Pour créer une nouvelle stratégie de QoS avec l’Assistant stratégie de QoS

-   Dans stratégie de groupe éditeur d’objets, cliquez avec le bouton droit sur l’un des nœuds de la **stratégie de QoS** , puis cliquez sur **créer une nouvelle stratégie**.

### <a name="wizard-page-1---policy-profile"></a>Page 1 de l’Assistant-Profil de stratégie

Sur la première page de l’Assistant stratégie de QoS, vous pouvez spécifier un nom de stratégie et configurer la manière dont la QoS contrôle le trafic réseau sortant.

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>Pour configurer la page Profil de stratégie de l’Assistant QoS basée sur la stratégie

1. Dans **Nom de la stratégie**, tapez le nom de la stratégie de QoS. Le nom doit identifier la stratégie de façon unique.

2. Si vous le souhaitez, utilisez **spécifier la valeur DSCP** pour activer le marquage DSCP, puis configurez une valeur DSCP comprise entre 0 et 63.

3. Vous pouvez éventuellement utiliser **Spécifier le taux d’accélération** pour activer le limitation du trafic et configurer le taux d’accélération. La valeur du taux d’accélération doit être supérieure à 1 et vous pouvez spécifier des unités de kilo- \(octets\) par seconde ou\)mégaoctets \(par seconde.

4. Cliquez sur **Suivant**.

### <a name="wizard-page-2---application-name"></a>Page 2 de l’Assistant-nom de l’application

Dans la deuxième page de l’Assistant stratégie de QoS, vous pouvez appliquer la stratégie à toutes les applications, à une application spécifique identifiée par son nom d’exécutable, à un chemin d’accès et un nom d’application, ou aux applications serveur HTTP qui traitent les demandes pour une URL spécifique.

- **Toutes les applications** spécifie que les paramètres de gestion du trafic sur la première page de l’Assistant stratégie de QoS s’appliquent à toutes les applications.

- **Seules les applications avec ce nom d’exécutable** spécifient que les paramètres de gestion du trafic sur la première page de l’Assistant stratégie de QoS sont pour une application spécifique. Le nom du fichier exécutable doit se terminer par l’extension de nom de fichier .exe.

- **Seules les applications serveur http répondant aux demandes pour cette URL** spécifient que les paramètres de gestion du trafic sur la première page de l’Assistant stratégie de QoS s’appliquent uniquement à certaines applications serveur http.

Vous pouvez éventuellement entrer le chemin d’accès de l’application. Pour spécifier le chemin d’accès à une application, indiquez le chemin d’accès en plus du nom de l’application. Le chemin d’accès peut inclure des variables d’environnement. Par exemple, %ProgramFiles%\Chemin_De_Mon_Application\MonApp.exe, ou c:\program files\chemin_de_mon_application\monapp.exe.

>[!NOTE]
>Le chemin d’accès de l’application ne peut pas inclure un chemin d’accès qui correspond à un lien symbolique.

L’URL doit être conforme à la [norme RFC 1738](https://tools.ietf.org/html/rfc1738), sous `http[s]://<hostname\>:<port\>/<url-path>`la forme. Vous pouvez utiliser un caractère générique `‘*'`,, `<hostname>` pour `https://training.\*/, https://\*.\*`et/ `<port>`ou, par exemple,, mais le caractère générique ne peut pas désigner `<hostname>` une `<port>`sous-chaîne de ou.

En d’autres termes, `https://my\*site/` ni `https://\*training\*/` ni n’est valide. 

Si vous le souhaitez, vous pouvez cocher **inclure les sous-répertoires et les fichiers** pour effectuer la correspondance sur tous les sous-répertoires et fichiers qui suivent une URL. Par exemple, si cette option est activée et que l’URL `https://training`est, la stratégie de QoS prend` https://training/video` en compte les demandes pour une bonne correspondance.

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>Pour configurer la page nom de l’application de l’Assistant stratégie de QoS

1. Dans **cette stratégie de QoS s’applique à**, sélectionnez **toutes les applications** ou **uniquement les applications avec ce nom d’exécutable**.

2. Si vous sélectionnez **Uniquement les applications possédant ce nom d’exécutable**, spécifiez un nom d’exécutable se terminant par l’extension de nom de fichier .exe.

3. Cliquez sur **Suivant**.

### <a name="wizard-page-3---ip-addresses"></a>Page 3 de l’Assistant-adresses IP

Dans la troisième page de l’Assistant stratégie de QoS, vous pouvez spécifier des conditions d’adresse IP pour la stratégie QoS, y compris les suivantes :

- Toutes les adresses IPv4 ou IPv6 sources ou des adresses IPv4 ou IPv6 sources spécifiques.

- Toutes les adresses IPv4 ou IPv6 de destination ou des adresses IPv4 ou IPv6 de destination spécifiques

Si vous sélectionnez **Uniquement pour l’adresse ou le préfixe IP source suivant** ou **Uniquement pour l’adresse ou le préfixe IP de destination suivant**, vous devez taper l’un des éléments suivants :

- Une adresse IPv4, telle que`192.168.1.1`

- Préfixe d’adresse IPv4 utilisant la notation de longueur de préfixe réseau, tel que`192.168.1.0/24`

- Une adresse IPv6, telle que`3ffe:ffff::1`

- Préfixe d’adresse IPv6, tel que`3ffe:ffff::/48`

Si vous sélectionnez les deux **uniquement pour l’adresse IP source suivante** et **uniquement pour l’adresse IP de destination suivante**, les adresses ou les préfixes d’adresse doivent être basés sur IPv4 ou IPv6.

Si vous avez spécifié l’URL pour les applications serveur HTTP dans la page précédente de l’Assistant, vous remarquerez que l’adresse IP source de la stratégie QoS sur cette page de l’Assistant est grisée. 

Cela est vrai parce que l’adresse IP source est l’adresse du serveur HTTP et qu’elle n’est pas configurable ici. En revanche, vous pouvez toujours personnaliser la stratégie en spécifiant l’adresse IP de destination. Vous pouvez ainsi créer des stratégies différentes pour différents clients en utilisant les mêmes applications serveur HTTP.

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>Pour configurer la page adresses IP de l’Assistant stratégie de QoS

1. Dans **cette stratégie de QoS s’applique à** (source), sélectionnez **n’importe quelle adresse IP source** ou **uniquement pour l’adresse IP source suivante**.

2. Si vous avez sélectionné **uniquement l’adresse IP source suivante**, spécifiez une adresse ou un préfixe IPv4 ou IPv6.

3. Dans **cette stratégie de QoS s’applique à** (destination), sélectionnez **n’importe quelle adresse de destination** ou **uniquement pour l’adresse IP de destination suivante.**

4. Si vous avez sélectionné **uniquement pour l’adresse IP de destination suivante**, spécifiez une adresse ou un préfixe IPv4 ou IPv6 qui correspond au type d’adresse ou de préfixe spécifié pour l’adresse source.

5.  Cliquez sur **Suivant**.  

### <a name="wizard-page-4---protocols-and-ports"></a>Page 4 de l’Assistant-protocoles et ports

Dans la quatrième page de l’Assistant stratégie de QoS, vous pouvez spécifier les types de trafic et les ports qui sont contrôlés par les paramètres sur la première page de l’Assistant. Vous pouvez spécifier les éléments suivants :  
-   trafic TCP, trafic UDP ou les deux ;  

-   tous les ports sources, une plage de ports sources ou un port source spécifique ;

-   Tous les ports de destination, une plage de ports de destination ou un port de destination spécifique  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>Pour configurer la page protocoles et ports de l’Assistant stratégie de QoS

1. Dans **Sélectionnez le protocole auquel s’applique cette stratégie de QoS**, sélectionnez **TCP**, **UDP** ou **TCP et UDP**.

2. Dans **Spécifiez le numéro de port source**, sélectionnez **À partir d’un port source quelconque** ou **À partir de ce numéro de port ou plage source**.

3. Si vous avez sélectionné **à partir de ce numéro de port source**, tapez un numéro de port compris entre 1 et 65535.

     Si vous le souhaitez, vous pouvez spécifier une plage de ports, au format «*faible*:*élevé*», où *basse* et *haute* représentent les limites inférieures et les limites supérieures de la plage de ports, inclus. *Low* et *High* doivent être un nombre compris entre 1 et 65535. Aucun espace n’est autorisé entre les deux-points (:) et les nombres.

4. Dans **Spécifiez le numéro de port de destination**, sélectionnez **Vers un port de destination quelconque** ou **Vers ce numéro de port ou plage de destination**.

5. Si vous avez sélectionné **Vers ce numéro de port ou plage de destination** à l’étape précédente, tapez un numéro de port compris entre 1 et 65535.

Pour terminer la création de la nouvelle stratégie de QoS, cliquez sur **Terminer** dans la page **protocoles et ports** de l’Assistant stratégie de QoS. Une fois l’opération terminée, la nouvelle stratégie QoS est indiquée dans le volet d’informations de l’éditeur d’objets stratégie de groupe.  
  
Pour appliquer les paramètres de stratégie de qualité de service aux utilisateurs ou aux ordinateurs, liez l’objet de stratégie de groupe dans lequel les stratégies de QoS sont situées dans un conteneur de Active Directory Domain Services, tel qu’un domaine, un site ou une unité d’organisation (UO).  
  
##  <a name="bkmk_editpolicy"></a>Afficher, modifier ou supprimer une stratégie de QoS

Les pages de l’Assistant stratégie de QoS décrites précédemment correspondent aux pages de propriétés qui s’affichent lorsque vous affichez ou modifiez les propriétés d’une stratégie.  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>Pour afficher les propriétés d’une stratégie de QoS  
  
-   Cliquez avec le bouton droit sur le nom de la stratégie dans le volet d’informations de l’éditeur d’objets stratégie de groupe, puis cliquez sur **Propriétés**.  
  
     L’éditeur d’objets stratégie de groupe affiche la page Propriétés avec les onglets suivants :  
  
    -   Profil de stratégie  
  
    -   Nom de l’application  
  
    -   Adresses IP  
  
    -   Protocoles et ports  
  
### <a name="to-edit-a-qos-policy"></a>Pour modifier une stratégie de QoS  
  
-   Cliquez avec le bouton droit sur le nom de la stratégie dans le volet d’informations de l’éditeur d’objets stratégie de groupe, puis cliquez sur **modifier la stratégie existante**.  
  
     L’éditeur d’objets stratégie de groupe affiche la boîte de dialogue **modifier une stratégie de QoS existante** .  
  
### <a name="to-delete-a-qos-policy"></a>Pour supprimer une stratégie de QoS  
  
-   Cliquez avec le bouton droit sur le nom de la stratégie dans le volet d’informations de l’éditeur d’objets stratégie de groupe, puis cliquez sur **Supprimer la stratégie**.  
  
### <a name="qos-policy-gpmc-reporting"></a>Rapports GPMC de stratégie QoS 

Une fois que vous avez appliqué un certain nombre de stratégies de QoS au sein de votre organisation, il peut être utile ou nécessaire de vérifier régulièrement comment les stratégies sont appliquées. Un résumé des stratégies QoS pour un utilisateur ou un ordinateur spécifique peut être affiché à l’aide de la fonctionnalité de création de rapports de la console GPMC.  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>Pour exécuter l’Assistant stratégie de groupe des résultats pour un rapport de stratégies QoS  
  
-   Dans la console GPMC, cliquez avec le bouton droit sur le nœud résultats de la **stratégie de groupe** , puis sélectionnez l’option de menu pour **stratégie de groupe Assistant résultats.**  
  
Une fois stratégie de groupe résultats générés, cliquez sur l’onglet **paramètres** . Dans l’onglet **paramètres** , les stratégies QoS se trouvent sous les nœuds « ordinateur \ configuration de la stratégie Settings\QoS de l’ordinateur » et « configuration utilisateur \ stratégie Settings\QoS ».  
  
Dans l’onglet **paramètres** , les stratégies de QoS sont répertoriées par leurs noms de stratégie de qualité de service (QoS), leur valeur DSCP, le taux de limitation, les conditions de stratégie et l’objet de stratégie de groupe gagnant figurant sur la même ligne.  
  
L’affichage des résultats de stratégie de groupe identifie de façon unique l’objet de stratégie de groupe gagnant. Lorsque plusieurs objets de stratégie de groupe ont des stratégies de QoS avec le même nom de stratégie QoS, l’objet de stratégie de groupe avec la priorité la plus élevée est appliqué. Il s’agit de l’objet de stratégie de groupe gagnant. Les stratégies de QoS (identifiées par un nom de stratégie) qui sont associées à un objet de stratégie de groupe de priorité inférieure ne sont pas appliquées. Notez que les priorités des objets de stratégie de groupe définissent les stratégies de QoS qui sont déployées dans le site, le domaine ou l’unité d’organisation, le cas échéant. Après le déploiement, au niveau de l’utilisateur ou de l’ordinateur, les règles de priorité de la [stratégie de qualité](#BKMK_precedencerules) de service déterminent le trafic autorisé et bloqué.  
  
La valeur DSCP, le taux de limitation et les conditions de stratégie de la stratégie QoS sont également visibles dans l’éditeur d’objets de stratégie de groupe (GPOE)  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>Paramètres avancés pour l’itinérance et les utilisateurs distants  
Avec la stratégie de qualité de service, l’objectif est de gérer le trafic sur le réseau d’une entreprise. Dans les scénarios mobiles, les utilisateurs peuvent envoyer du trafic sur le réseau de l’entreprise. Étant donné que les stratégies de QoS ne sont pas pertinentes en dehors du réseau de l’entreprise, les stratégies de QoS sont activées uniquement sur les interfaces réseau qui sont connectées à l’entreprise pour Windows 8, Windows 7 ou Windows Vista.  
  
Par exemple, un utilisateur peut connecter son ordinateur portable au réseau de son entreprise via un réseau privé virtuel (VPN) à partir d’un café. Pour VPN, aucune stratégie de QoS n’est appliquée à l’interface réseau physique (par exemple, sans fil). Toutefois, les stratégies de QoS sont appliquées à l’interface VPN, car elle se connecte à l’entreprise. Si, par la suite, l’utilisateur entre un réseau d’entreprise qui ne dispose pas d’une relation d’approbation AD DS, les stratégies de QoS ne sont pas activées.  
  
Notez que ces scénarios mobiles ne s’appliquent pas aux charges de travail du serveur. Par exemple, un serveur avec plusieurs cartes réseau peut se trouver à la périphérie du réseau d’une entreprise. Le service informatique peut choisir d’avoir des stratégies de QoS qui limitent le trafic qui egresses l’entreprise ; Toutefois, cette carte réseau qui envoie ce trafic de sortie ne se connecte pas nécessairement au réseau de l’entreprise. Pour cette raison, les stratégies de QoS sont toujours activées sur toutes les interfaces réseau d’un ordinateur exécutant Windows Server 2012.  
  
> [!NOTE]
>  L’activation sélective s’applique uniquement aux stratégies de QoS et non aux paramètres de QoS avancés décrits plus loin dans ce document.  
  
### <a name="advanced-qos-settings"></a>Paramètres de QoS avancés

Les paramètres de QoS avancés fournissent des contrôles supplémentaires permettant aux administrateurs informatiques de gérer la consommation du réseau informatique et les marquages DSCP. Les paramètres de QoS avancés s’appliquent uniquement au niveau de l’ordinateur, alors que les stratégies de QoS peuvent être appliquées aux niveaux ordinateur et utilisateur.

#### <a name="to-configure-advanced-qos-settings"></a>Pour configurer les paramètres de QoS avancés

1.  Cliquez sur Configuration de l' **ordinateur**, puis sur **paramètres Windows dans stratégie de groupe**.
  
2.  Cliquez avec le bouton droit sur **stratégie QoS**, puis cliquez sur **paramètres de QoS avancés**.

     L’illustration suivante montre les deux onglets paramètres de QoS avancés : **Le trafic TCP entrant et le** **marquage DSCP remplacent**.
  
> [!NOTE]
>  Les paramètres de QoS avancés sont des paramètres de stratégie de groupe au niveau de l’ordinateur.
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>Paramètres de QoS avancés : trafic TCP entrant

Le **trafic TCP entrant** contrôle la consommation de bande passante TCP côté récepteur, tandis que les stratégies de QoS affectent le trafic TCP et UDP sortant. 

En définissant un niveau de débit inférieur sous l’onglet **trafic TCP entrant** , TCP limite la taille de sa fenêtre de réception TCP publiée. L’effet de ce paramètre est l’augmentation des taux de débit et de l’utilisation de la liaison pour les connexions TCP avec des bandes passantes ou des latences plus élevées (produit de délai de bande passante). Par défaut, les ordinateurs exécutant Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 et Windows Vista sont définis sur le niveau de débit maximal.
  
La fenêtre de réception TCP a été modifiée dans Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 et Windows Vista à partir des versions précédentes de Windows. Dans les versions précédentes de Windows, la fenêtre côté réception TCP était limitée à un maximum de 64 kilo-octets (Ko), alors que Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 et Windows Vista redimensionnent dynamiquement la fenêtre côté réception jusqu’à 16 mégaoctets (Mo ). Dans le contrôle du trafic TCP entrant, vous pouvez contrôler le niveau de débit entrant en définissant la valeur maximale que peut atteindre la fenêtre de réception TCP. Les niveaux correspondent aux valeurs maximales suivantes. 
  
|Niveau de débit entrant|Maximale|  
|------------------------|-------|  
|0|64 KO|
|1|256 KO|
|2|1 Mo|
|3|16 MO|

La taille réelle de la fenêtre peut être une valeur inférieure ou égale à la valeur maximale, selon les conditions du réseau.

###### <a name="to-set-the-tcp-receive-side-window"></a>Pour définir la fenêtre côté réception TCP

1. Dans stratégie de groupe éditeur d’objets, cliquez sur stratégie de l' **ordinateur local**, cliquez sur **Paramètres Windows**, cliquez avec le bouton droit sur **stratégie QoS**, puis cliquez sur **paramètres de QoS avancés**.
  
2. Dans **débit de réception TCP**, sélectionnez **configurer le débit de réception TCP**, puis sélectionnez le niveau de débit souhaité.

3.  Liez l’objet de stratégie de groupe à l’unité d’organisation.

#### <a name="advanced-qos-settings-dscp-marking-override"></a>Paramètres de QoS avancés : Remplacement du marquage DSCP

Le remplacement du marquage DSCP restreint la possibilité pour les applications de spécifier (ou « marquer ») des valeurs DSCP autres que celles spécifiées dans les stratégies QoS. En spécifiant que les applications sont autorisées à définir des valeurs DSCP, les applications peuvent définir des valeurs DSCP non nulles. 

En spécifiant **Ignorer**, les valeurs DSCP des applications qui utilisent des API QoS sont définies sur zéro, et seules les stratégies QoS peuvent définir des valeurs DSCP. 

Par défaut, les ordinateurs qui exécutent Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 et Windows Vista autorisent les applications à spécifier des valeurs DSCP. les applications et les appareils qui n’utilisent pas les API QoS ne sont pas substitués.

##### <a name="wireless-multimedia-and-dscp-values"></a>Valeurs multimédias et DSCP sans fil

[Wi-Fi Alliance](https://go.microsoft.com/fwlink/?LinkId=160769) a établi une certification pour Wireless Multimedia \(WMM @ no__t-2 qui définit quatre catégories d’accès \(WMM_AC @ no__t-4 pour définir la priorité du trafic réseau transmis sur un réseau sans fil Wi-No__t-5Fi. Les catégories d’accès \(incluent dans l’ordre de priorité\)la plus élevée à la plus basse : voix, vidéo, meilleur effort et arrière-plan ; respectivement, les abréviations VO, VI, is et BK. La spécification WMM définit les valeurs DSCP qui correspondent à chacune des quatre catégories d’accès :
  
|Valeur DSCP|Catégorie d’accès WMM|
|----------|-------------------|
|48-63|Voix (VO)|
|32-47|Vidéo (VI)|
|24-31, 0-7|Meilleur effort (a)|
|8-23|Arrière-plan (BK)|

Vous pouvez créer des stratégies de qualité de service qui utilisent ces valeurs DSCP pour vous assurer\-que les ordinateurs portables dotés de™ certifiés Wi-Fi pour les\-adaptateurs sans fil WMM reçoivent une gestion hiérarchisée lorsqu’ils sont associés à des points d’accès certifiés Wi-Fi pour WMM.
  
### <a name="BKMK_precedencerules"></a>Règles de priorité de la stratégie QoS

À l’instar des priorités des objets de stratégie de groupe, les stratégies de QoS ont des règles de précédence pour résoudre les conflits lorsque plusieurs stratégies de QoS s’appliquent à un ensemble spécifique de trafic. Pour le trafic TCP ou UDP sortant, une seule stratégie de qualité de service peut être appliquée à la fois, ce qui signifie que les stratégies de QoS n’ont pas d’effet cumulatif, par exemple la somme des taux de limitation.

En général, la stratégie de QoS avec les conditions les plus identiques gagne. Lorsque plusieurs stratégies de QoS s’appliquent, les règles se répartissent en trois catégories : au niveau de l’utilisateur et au niveau de l’ordinateur ; application et quintuple réseau ; et entre le réseau quintuple.

Par *réseau quintuple*, nous entendons l’adresse IP source, l’adresse IP de destination, le port source, le \(port de destination\)et le protocole TCP/UDP.  

 **La stratégie QoS au niveau de l’utilisateur est prioritaire sur la stratégie QoS au niveau de l’ordinateur**

Cette règle simplifie la gestion des objets de stratégie de groupe QoS pour les administrateurs réseau, en particulier pour les stratégies basées sur les groupes d’utilisateurs. Par exemple, si l’administrateur réseau souhaite définir une stratégie de QoS pour un groupe d’utilisateurs, il peut simplement créer et distribuer un objet de stratégie de groupe à ce groupe. Ils n’ont pas à se soucier des ordinateurs sur lesquels ces utilisateurs sont connectés et s’ils ont des stratégies de QoS en conflit définies, car, en cas de conflit, la stratégie de niveau utilisateur est toujours prioritaire.

> [!NOTE]
>  Une stratégie de qualité de service au niveau de l’utilisateur est uniquement applicable au trafic généré par cet utilisateur. Les autres utilisateurs d’un ordinateur spécifique et l’ordinateur lui-même ne sont soumis à aucune stratégie de QoS définie pour cet utilisateur.

 **Spécificité des applications et priorité sur les quintuple réseau**

Lorsque plusieurs stratégies de QoS correspondent au trafic spécifique, la stratégie plus spécifique est appliquée. Parmi les stratégies qui identifient les applications, une stratégie qui comprend le chemin d’accès au fichier de l’application émettrice est considérée comme plus spécifique qu’une autre stratégie qui identifie uniquement le nom de l’application (aucun chemin d’accès). Si plusieurs stratégies avec des applications s’appliquent toujours, les règles de précédence utilisent le quintuple réseau pour trouver la meilleure correspondance.

En guise d’alternative, plusieurs stratégies de QoS peuvent s’appliquer au même trafic en spécifiant des conditions qui ne se chevauchent pas. Entre les conditions des applications et le réseau quintuple, la stratégie qui spécifie l’application est considérée comme plus spécifique et est appliquée. 

Par exemple, policy_A spécifie uniquement un nom d’application (App. exe) et policy_B spécifie l’adresse IP de destination 192.168.1.0/24. Lorsque ces stratégies de QoS sont en conflit @no__t -0app. exe envoie le trafic à une adresse IP dans la plage de 192.168.4.0/24 @ no__t-1, policy_A est appliqué.

 **Une plus grande spécificité est prioritaire dans le quintuple réseau**

Pour les conflits de stratégie au sein du réseau quintuple, la stratégie avec le plus de conditions de correspondance est prioritaire. Par exemple, supposons que policy_C spécifie l’adresse IP source « any », l’adresse IP de destination 10.0.0.1, le port source « any », le port de destination « any » et le protocole « TCP ». 

Ensuite, supposons que policy_D spécifie l’adresse IP source « any », l’adresse IP de destination 10.0.0.1, le port source « any », le port de destination 80 et le protocole « TCP ». Ensuite, policy_C et policy_D correspondent aux connexions à la destination 10.0.0.1:80. Étant donné que la stratégie de QoS applique la stratégie avec les conditions de correspondance les plus spécifiques, policy_D est prioritaire dans cet exemple.  
  
Toutefois, les stratégies de QoS peuvent avoir un nombre égal de conditions. Par exemple, plusieurs stratégies peuvent spécifier chacune une seule partie (mais pas la même) du réseau quintuple. Parmi les quintuple réseau, l’ordre suivant passe de la priorité la plus élevée à la plus faible :

- Adresse IP source

- Adresse IP de destination

- Port source

- Port de destination

- Protocole (TCP ou UDP)

Dans une condition spécifique, telle que l’adresse IP, une adresse IP plus spécifique est traitée avec une priorité plus élevée. par exemple, une adresse IP 192.168.4.1 est plus spécifique que 192.168.4.0/24.

Concevez vos stratégies de qualité de service aussi spécifiquement que possible pour simplifier la capacité de votre organisation à comprendre les stratégies en vigueur.

Pour la rubrique suivante de ce guide, consultez [événements et erreurs de stratégie QoS](qos-policy-errors.md).

Pour obtenir la première rubrique de ce guide, consultez [stratégie de qualité de service (QoS)](qos-policy-top.md).
