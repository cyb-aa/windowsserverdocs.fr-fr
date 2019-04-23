---
title: Gérer la stratégie de QoS
description: Cette rubrique fournit des instructions sur la façon de créer et gérer la stratégie de qualité de Service (QoS) dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 04fdfa54-6600-43d4-8945-35f75e15275a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 94e5a1832a6c1e160b9cc338d50636026a5eb751
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851670"
---
# <a name="manage-qos-policy"></a>Gérer la stratégie de QoS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur l’utilisation de l’Assistant Stratégie de QoS pour créer, modifier ou supprimer une stratégie de QoS.

>[!NOTE]
>  Outre cette rubrique, la documentation de gestion de stratégie de QoS suivante est disponible.
> 
>  - [Erreurs et événements de stratégie de QoS](qos-policy-errors.md)

Dans les systèmes d’exploitation Windows, la stratégie QoS combine les fonctionnalités de QoS basée sur les normes avec la facilité de gestion de stratégie de groupe. Configuration de cette combinaison rend simplifie l’application des stratégies de QoS aux objets de stratégie de groupe. Windows inclut un Assistant de stratégie de QoS pour vous aider à effectuer les tâches suivantes.

-  [Créer une stratégie de QoS](#bkmk_createpolicy)

-  [Afficher, modifier ou supprimer une stratégie de QoS](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>Créer une stratégie de QoS

Avant de créer une stratégie de QoS, il est important de comprendre les deux contrôles de qualité de service clés qui sont utilisées pour gérer le trafic réseau :

- Valeur DSCP

-   Taux d’accélération

### <a name="prioritizing-traffic-with-dscp"></a>Hiérarchiser le trafic avec DSCP

Comme indiqué dans l’exemple d’application de line-of-business précédent, vous pouvez définir la priorité du trafic réseau sortant à l’aide de **spécifier la valeur DSCP** pour configurer une stratégie de QoS avec une valeur DSCP spécifique. 

Comme décrit dans la RFC 2474, DSCP autorise les valeurs comprises entre 0 et 63 pour spécifier le champ TOS d’un paquet IPv4 et dans le champ de classe de trafic IPv6. Les routeurs réseau utilisent la valeur DSCP pour classer les paquets réseau et en file d’attente en conséquence.
  
> [!NOTE]
>  Par défaut, le trafic Windows a la valeur DSCP 0.
  
Le nombre de files d’attente et la définition de leur priorité doivent être décidés dans le cadre de la stratégie de QoS de votre organisation. Par exemple, votre organisation peut choisir d’avoir cinq files d’attente : trafic sensible à la latence, le trafic de contrôle, trafic critique pour l’entreprise, meilleur effort le trafic et le trafic de transfert de données en bloc.  
  
### <a name="throttling-traffic"></a>Limitation du trafic

Ainsi que les valeurs DSCP, la limitation est un autre contrôle de clé pour la gestion de la bande passante réseau. Comme mentionné précédemment, vous pouvez utiliser la **spécifier le taux d’accélération** paramètre pour configurer une stratégie de QoS avec un taux d’accélération spécifique pour le trafic sortant. À l’aide de la limitation, une stratégie de QoS limite le trafic réseau sortant à un taux d’accélération spécifié. Le marquage DSCP et la limitation de bande passante peuvent être utilisés conjointement pour gérer efficacement le trafic.

>[!NOTE]
>Par défaut, la case à cocher **Spécifier le taux d’accélération** n’est pas activée.

Pour créer une stratégie de QoS, modifiez les paramètres d’un objet (stratégie de groupe) à partir de dans l’outil de la Console de gestion des stratégies de groupe (GPMC). La console GPMC puis ouvre l’éditeur d’objets de stratégie de groupe.

Les noms des stratégies de QoS doivent être uniques. Comment les stratégies sont appliquées aux serveurs et les utilisateurs finaux dépend de la stratégie QoS de stockage de dans l’éditeur d’objet de stratégie de groupe :

- Une stratégie de QoS dans la configuration d’ordinateur, Settings\QoS stratégie s’applique aux ordinateurs, quel que soit l’utilisateur actuellement connecté. Les stratégies de QoS basées sur un ordinateur sont généralement utilisées pour les ordinateurs serveurs.

- Une stratégie de QoS dans utilisateur configuration Settings\QoS stratégie s’applique aux utilisateurs une fois qu’ils sont connectés, quel que soit l’ordinateur sur lequel ils sont connectés.

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>Pour créer une nouvelle stratégie de QoS avec l’Assistant Stratégie de QoS

-   Dans l’éditeur d’objets de stratégie de groupe, cliquez sur un de le **stratégie QoS** nœuds, puis cliquez sur **créer une nouvelle stratégie**.

### <a name="wizard-page-1---policy-profile"></a>Page de l’Assistant 1 - profil de stratégie

Sur la première page de l’Assistant Stratégie de QoS, vous pouvez spécifier un nom de stratégie et configurer la QoS contrôle le trafic réseau sortant.

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>Pour configurer la page Profil de stratégie de l’Assistant QoS basée sur la stratégie

1. Dans **Nom de la stratégie**, tapez le nom de la stratégie de QoS. Le nom doit identifier la stratégie.

2. Si vous le souhaitez, utiliser **spécifier la valeur DSCP** pour activer le marquage DSCP, puis configurez une valeur DSCP comprise entre 0 et 63.

3. Vous pouvez éventuellement utiliser **Spécifier le taux d’accélération** pour activer le limitation du trafic et configurer le taux d’accélération. La valeur de taux de limitation doit être supérieure à 1 et vous pouvez spécifier des unités de kilo-octets par seconde \(Kbits/s\) ou mégaoctets par seconde \(Mbits/s\).

4. Cliquez sur **Suivant**.

### <a name="wizard-page-2---application-name"></a>Page de l’Assistant 2 - nom de l’Application

Dans la deuxième page de l’Assistant Stratégie de QoS, vous pouvez appliquer la stratégie à toutes les applications, à une application spécifique tel qu’identifié par son nom d’exécutable, un chemin d’accès et le nom de l’application, ou pour les applications de serveur HTTP qui gèrent les demandes pour une URL spécifique.

- **Toutes les applications** Spécifie que les paramètres de gestion de trafic sur la première page de l’Assistant Stratégie de QoS s’appliquent à toutes les applications.

- **Uniquement les applications possédant ce nom d’exécutable** Spécifie que les paramètres de gestion de trafic sur la première page de l’Assistant Stratégie de QoS sont pour une application spécifique. Le nom du fichier exécutable doit se terminer par l’extension de nom de fichier .exe.

- **Seules les applications de serveur HTTP répond aux demandes pour cette URL** Spécifie que les paramètres de gestion de trafic de la première page de l’Assistant Stratégie de QoS s’appliquent à certaines applications de serveur HTTP uniquement.

Vous pouvez éventuellement entrer le chemin d’accès de l’application. Pour spécifier le chemin d’accès à une application, indiquez le chemin d’accès en plus du nom de l’application. Le chemin d’accès peut inclure des variables d’environnement. Par exemple, %ProgramFiles%\Chemin_De_Mon_Application\MonApp.exe, ou c:\program files\chemin_de_mon_application\monapp.exe.

>[!NOTE]
>Le chemin d’accès de l’application ne peut pas inclure un chemin d’accès qui correspond à un lien symbolique.

L’URL doit être conforme à [RFC 1738](https://tools.ietf.org/html/rfc1738), sous la forme de `http[s]://<hostname\>:<port\>/<url-path>`. Vous pouvez utiliser un caractère générique, `‘*’`, pour `<hostname>` et/ou `<port>`, par exemple `https://training.\*/, https://\*.\*`, mais le caractère générique ne peut pas indiquer une sous-chaîne de `<hostname>` ou `<port>`.

En d’autres termes, aucun `https://my\*site/` ni `https://\*training\*/` est valide. 

Si vous le souhaitez, vous pouvez vérifier **inclure les sous-répertoires et fichiers** à mettre en correspondance tous les sous-répertoires et fichiers de la suite d’une URL. Par exemple, si cette option est activée et que l’URL est `https://training`, stratégie de QoS va prendre en compte les demandes de` https://training/video` une meilleure correspondance.

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>Pour configurer la page Nom de l’Application de l’Assistant Stratégie de QoS

1. Dans **cette stratégie de QoS s’applique à**, sélectionnez **toutes les applications** ou **uniquement les applications possédant ce nom d’exécutable**.

2. Si vous sélectionnez **Uniquement les applications possédant ce nom d’exécutable**, spécifiez un nom d’exécutable se terminant par l’extension de nom de fichier .exe.

3. Cliquez sur **Suivant**.

### <a name="wizard-page-3---ip-addresses"></a>Assistant, page 3 - adresses IP

Dans la troisième page de l’Assistant Stratégie de QoS, vous pouvez spécifier les conditions d’adresse IP pour la stratégie de QoS, notamment les suivantes :

- Toutes les adresses IPv4 ou IPv6 sources ou des adresses IPv4 ou IPv6 sources spécifiques.

- Toutes les adresses IPv4 ou IPv6 de destination ou les adresses IPv4 ou IPv6 de destination spécifique

Si vous sélectionnez **Uniquement pour l’adresse ou le préfixe IP source suivant** ou **Uniquement pour l’adresse ou le préfixe IP de destination suivant**, vous devez taper l’un des éléments suivants :

- Adresse IPv4, par exemple `192.168.1.1`

- Un préfixe d’adresse IPv4 à l’aide de notation de préfixe réseau, tel que `192.168.1.0/24`

- Adresse d’une adresse IPv6, par exemple `3ffe:ffff::1`

- Comme préfixe, une adresse IPv6 d’adresse `3ffe:ffff::/48`

Si vous sélectionnez à la fois **uniquement pour l’adresse IP source suivant** et **uniquement pour l’adresse IP de destination à l’adresse suivante**, adresses ou préfixes d’adresse doivent être soit IPv4 ou IPv6 basés sur.

Si vous avez spécifié l’URL pour les applications de serveur HTTP dans la page précédente de l’Assistant, vous remarquerez que l’adresse IP source pour la stratégie de QoS sur cette page d’Assistant apparaît en grisé. 

Cela est vrai, car l’adresse IP source est l’adresse du serveur HTTP et il n’est pas configurable ici. En revanche, vous pouvez toujours personnaliser la stratégie en spécifiant l’adresse IP de destination. Cela rend possible de créer différentes stratégies pour différents clients en utilisant les mêmes applications de serveur HTTP.

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>Pour configurer la page adresses IP de l’Assistant Stratégie de QoS

1. Dans **cette stratégie de QoS s’applique à** (source), sélectionnez **une adresse IP source** ou **uniquement pour l’adresse IP suivante adresse source**.

2. Si vous avez sélectionné **uniquement le suivant adresse IP source**, spécifiez une adresse IPv4 ou IPv6 ou le préfixe.

3. Dans **cette stratégie de QoS s’applique à** (destination), sélectionnez **n’importe quelle adresse de destination** ou **uniquement pour l’adresse IP de destination.**

4. Si vous avez sélectionné **uniquement pour l’adresse IP de destination**, spécifiez une adresse IPv4 ou IPv6 ou un préfixe qui correspond au type d’adresse ou le préfixe spécifié pour l’adresse source.

5.  Cliquez sur **Suivant**.  

### <a name="wizard-page-4---protocols-and-ports"></a>Page 4 - protocoles et Ports de l’Assistant

Dans la quatrième page de l’Assistant Stratégie de QoS, vous pouvez spécifier les types de trafic et les ports qui sont contrôlés par les paramètres sur la première page de l’Assistant. Vous pouvez spécifier les éléments suivants :  
-   trafic TCP, trafic UDP ou les deux ;  

-   tous les ports sources, une plage de ports sources ou un port source spécifique ;

-   Tous les ports de destination, une plage de ports de destination ou un port de destination spécifique  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>Pour configurer la page protocoles et Ports de l’Assistant Stratégie de QoS

1. Dans **Sélectionnez le protocole auquel s’applique cette stratégie de QoS**, sélectionnez **TCP**, **UDP** ou **TCP et UDP**.

2. Dans **Spécifiez le numéro de port source**, sélectionnez **À partir d’un port source quelconque** ou **À partir de ce numéro de port ou plage source**.

3. Si vous avez sélectionné **à partir de ce numéro de port source**, tapez un numéro de port compris entre 1 et 65535.

     Si vous le souhaitez, vous pouvez spécifier une plage de ports, au format «*faible*:*haute*, « où *faible* et *haute* représentent les limites inférieures et les limites supérieures du port, sont compris. *Faible* et *haute* chacune doit être un nombre compris entre 1 et 65535. Aucun espace n’est autorisé entre les deux-points (:) et les nombres.

4. Dans **Spécifiez le numéro de port de destination**, sélectionnez **Vers un port de destination quelconque** ou **Vers ce numéro de port ou plage de destination**.

5. Si vous avez sélectionné **Vers ce numéro de port ou plage de destination** à l’étape précédente, tapez un numéro de port compris entre 1 et 65535.

Pour terminer la création de la nouvelle stratégie de QoS, cliquez sur **Terminer** sur le **protocoles et Ports** page de l’Assistant Stratégie de QoS. Une fois terminé, la nouvelle stratégie de QoS est répertoriée dans le volet de détails de l’éditeur d’objet de stratégie de groupe.  
  
Pour appliquer les paramètres de stratégie de QoS aux utilisateurs ou ordinateurs, liez l’objet de stratégie de groupe dans lequel les stratégies de QoS sont situés dans un conteneur de Services de domaine Active Directory, comme un domaine, un site ou une unité d’organisation (UO).  
  
##  <a name="bkmk_editpolicy"></a>Afficher, modifier ou supprimer une stratégie de QoS

Précédemment, les pages de la stratégie de QoS Assistant décrit correspondent aux pages de propriétés qui sont affichent lorsque vous affichez ou modifiez les propriétés d’une stratégie.  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>Pour afficher les propriétés d’une stratégie de QoS  
  
-   Cliquez sur le nom de stratégie dans le volet de détails de l’éditeur d’objet de stratégie de groupe, puis cliquez sur **propriétés**.  
  
     L’éditeur d’objets de stratégie de groupe affiche la page de propriétés avec les onglets suivants :  
  
    -   Profil de stratégie  
  
    -   Nom de l’application  
  
    -   Adresses IP  
  
    -   Protocoles et ports  
  
### <a name="to-edit-a-qos-policy"></a>Pour modifier une stratégie de QoS  
  
-   Cliquez sur le nom de stratégie dans le volet de détails de l’éditeur d’objet de stratégie de groupe, puis cliquez sur **modifier la stratégie existante**.  
  
     L’éditeur d’objets de stratégie de groupe affiche les **modifier une stratégie de QoS existante** boîte de dialogue.  
  
### <a name="to-delete-a-qos-policy"></a>Pour supprimer une stratégie de QoS  
  
-   Cliquez sur le nom de stratégie dans le volet de détails de l’éditeur d’objet de stratégie de groupe, puis cliquez sur **supprimer la stratégie**.  
  
### <a name="qos-policy-gpmc-reporting"></a>Rapports de GPMC soient stratégie QoS 

Une fois que vous avez appliqué un nombre de stratégies de QoS au sein de votre organisation, il peut être utile ou nécessaire de consulter régulièrement la façon dont les stratégies sont appliquées. Un résumé des stratégies de QoS pour un utilisateur spécifique ou un ordinateur peut être affiché à l’aide de rapports de GPMC soient.  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>Pour exécuter l’Assistant résultats de stratégie de groupe pour un rapport des stratégies QoS  
  
-   Dans la console GPMC, cliquez sur le **les résultats de stratégie de groupe** nœud et sélectionnez le menu option **Assistant résultats de stratégie de groupe.**  
  
Une fois les résultats de stratégie de groupe sont générés, cliquez sur le **paramètres** onglet. Sur le **paramètres** onglet, les stratégies de QoS sont accessibles sous les nœuds « Stratégie d’ordinateur Configuration ordinateur\Paramètres Settings\QoS » et « Configuration ordinateur\Paramètres Settings\QoS de stratégie ».  
  
Sur le **paramètres** , les stratégies de QoS sont répertoriés par leur nom de stratégie de QoS avec leur valeur DSCP, les taux d’accélération, les conditions de stratégie, puis gagnante GPO répertoriés dans la même ligne...  
  
L’affichage de résultats de stratégie de groupe identifie de façon unique l’objet de stratégie de groupe gagnant. Lorsque plusieurs objets de stratégie de groupe ont des stratégies de qualité de service portant le même nom de stratégie de QoS, avec la priorité la plus élevée de la stratégie de groupe est appliqué. Il s’agit de l’objet de stratégie de groupe gagnant. Stratégies en conflit QoS (identifiés par le nom de la stratégie) qui sont attachés à une priorité plus faible GPO ne sont pas appliquées. Notez que les priorités de GPO définissent quelles stratégies de QoS sont déployées dans le site, le domaine ou l’unité d’organisation, comme il convient. Après le déploiement, à un niveau utilisateur ou ordinateur, le [règles de priorité de stratégie de QoS](#BKMK_precedencerules) déterminer quel trafic est autorisé et bloqué.  
  
La stratégie de QoS valeur DSCP, le taux d’accélération et les conditions de stratégie sont également visibles dans Éditeur du objet stratégie de groupe (GPOE)  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>Paramètres avancés pour les utilisateurs itinérants et à distance  
Avec la stratégie de QoS, vise à gérer le trafic sur un réseau d’entreprise. Dans les scénarios mobiles, les utilisateurs peuvent envoyer le trafic ou de désactiver le réseau d’entreprise. Étant donné que les stratégies de QoS ne sont pas pertinentes du réseau de l’entreprise, les stratégies de QoS sont activées uniquement sur des interfaces réseau qui sont connectés à l’entreprise pour Windows 8, Windows 7 ou Windows Vista.  
  
Par exemple, un utilisateur peut se connecter son ordinateur portable au réseau de son entreprise via le réseau privé virtuel (VPN) depuis un café. Pour les VPN, l’interface réseau physique (par exemple, sans fil) n’aura pas stratégies QoS sont appliquées. Toutefois, l’interface VPN auront des stratégies QoS sont appliquées, car il se connecte à l’entreprise. Si l’utilisateur entre ultérieurement le réseau d’une autre entreprise qui ne dispose pas d’une relation d’approbation AD DS, les stratégies QoS ne seront pas activées.  
  
Notez que ces scénarios mobiles ne s’appliquent pas aux charges de travail de serveur. Par exemple, un serveur avec plusieurs cartes réseau peut-être se trouvent sur le bord d’un réseau d’entreprise. Le service informatique peut choisir d’avoir QoS stratégies limiter le trafic egresses de l’entreprise ; Toutefois, cette carte réseau qui envoie ce trafic sortant ne se connecte pas nécessairement vers le réseau d’entreprise. Pour cette raison, les stratégies de QoS sont toujours activées sur toutes les interfaces réseau d’un ordinateur exécutant Windows Server 2012.  
  
> [!NOTE]
>  Activation sélective s’applique uniquement aux stratégies de QoS et pas pour les paramètres avancés de QoS abordée plus loin dans ce document.  
  
### <a name="advanced-qos-settings"></a>Paramètres avancés de QoS

Paramètres avancés de QoS fournissent des options supplémentaires pour les administrateurs informatiques à gérer la consommation réseau ordinateur et des marquages DSCP. Paramètres avancés de QoS s’appliquent uniquement au niveau de l’ordinateur, tandis que les stratégies de qualité de service peuvent être appliquées aux niveaux ordinateur et utilisateur.

#### <a name="to-configure-advanced-qos-settings"></a>Pour configurer les paramètres de QoS avancés

1.  Cliquez sur **Configuration ordinateur**, puis cliquez sur **Windows des paramètres de stratégie de groupe**.
  
2.  Avec le bouton droit **stratégie QoS**, puis cliquez sur **paramètres avancés de QoS**.

     La figure suivante illustre que les deux avancées des onglets de paramètres de qualité de service : **Le trafic TCP entrant** et **le remplacement de marquage DSCP**.
  
> [!NOTE]
>  Paramètres avancés de QoS sont des paramètres de stratégie de groupe au niveau de l’ordinateur.
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>Les paramètres de QoS avancés : le trafic TCP entrant

**Le trafic TCP entrant** contrôle la consommation de bande passante TCP sur le côté du récepteur, tandis que les stratégies de QoS affectent le trafic TCP et UDP sortant. 

En définissant un débit inférieur au niveau de la **du trafic TCP entrant** onglet, TCP limitera fenêtre de réception de la taille de son TCP publié. L’effet de ce paramètre sera taux d’augmentation du débit et l’utilisation pour les connexions TCP avec des bandes passantes plus élevées ou les latences (produit du délai de la bande passante) des liaisons. Par défaut, les ordinateurs exécutant Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 et Windows Vista sont définies au niveau de débit maximal.
  
La réception TCP fenêtre a changé dans Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 et Windows Vista à partir de versions précédentes de Windows. Les versions précédentes de Windows limité la fenêtre de réception TCP à un maximum de 64 kilo-octets (Ko), tandis que Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 et Windows Vista dynamiquement taille de la fenêtre de réception jusqu'à 16 mégaoctets (Mo ). Dans le contrôle de trafic TCP entrant, vous pouvez contrôler le niveau de débit entrant en définissant la valeur maximale que peut atteindre la fenêtre de réception TCP. Les niveaux correspondent aux valeurs maximum suivantes. 
  
|Niveau de débit entrant|Durée maximum|  
|------------------------|-------|  
|0|64 KO|
|1|256 KO|
|2|1 Mo|
|3|16 MO|

La taille de fenêtre réelle peut être une valeur égale ou inférieure à la valeur maximale, en fonction des conditions de réseau.

###### <a name="to-set-the-tcp-receive-side-window"></a>Pour définir la fenêtre côté réception de TCP

1. Dans l’éditeur d’objets de stratégie de groupe, cliquez sur **stratégie ordinateur Local**, cliquez sur **Windows paramètres**, avec le bouton droit cliquez sur **stratégie QoS**, puis cliquez sur **avancés de QoS Paramètres**.
  
2. Dans **le débit de réception TCP**, sélectionnez **configurer le débit TCP réception**, puis sélectionnez le niveau de débit que vous souhaitez.

3.  Liez l’objet de stratégie de groupe à l’unité d’organisation.

#### <a name="advanced-qos-settings-dscp-marking-override"></a>Paramètres QoS avancés : Remplacer le marquage DSCP

Remplacer le marquage DSCP restreint la capacité des applications pour spécifier, ou « marquer » — DSCP valeurs autres que celles spécifiées dans les stratégies de QoS. En spécifiant que les applications sont autorisées à définir des valeurs DSCP, les applications peuvent définir des valeurs DSCP non nulles. 

En spécifiant **ignorer**, les applications qui utilisent QoS APIs auront leurs valeurs DSCP définies sur zéro, et seules les stratégies QoS peuvent définir des valeurs DSCP. 

Par défaut, les ordinateurs exécutant Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 et Windows Vista permettent aux applications de spécifier les valeurs DSCP ; applications et les périphériques qui n’utilisent pas les APIs QoS ne sont pas remplacées.

##### <a name="wireless-multimedia-and-dscp-values"></a>Valeurs DSCP et des applications multimédias sans fil

Le [Wi-Fi Alliance](https://go.microsoft.com/fwlink/?LinkId=160769) a établi une certification pour le sans fil multimédia \(WMM\) qui définit quatre catégories d’accès \(WMM_AC\) pour hiérarchiser le trafic réseau transmis sur un Wi\-réseau sans fil Fi. Incluent les catégories d’accès \(par ordre de priorité la plus élevée-à-le plus faible\): voix, vidéo, meilleur effort et arrière-plan ; respectivement l’abréviation VO, VI, BE et BK. La spécification WMM définit quels DSCP valeurs correspondent à chacune des quatre accès catégories :
  
|Valeur DSCP|Catégorie d’accès WMM|
|----------|-------------------|
|48-63|Voix (VO)|
|32-47|Vidéo (VI)|
|24-31, 0-7|Meilleur effort (BE)|
|8-23|En arrière-plan (noir)|

Vous pouvez créer des stratégies de QoS qui utilisent ces valeurs DSCP pour vous assurer que portable ordinateurs avec Wi\-Fi Certified™ pour les cartes sans fil WMM de réception par ordre de priorité la gestion des cas d’association à Wi\-Fi certifié pour WMM points d’accès.
  
### <a name="BKMK_precedencerules"></a>Règles de priorité de stratégie de QoS

Comme pour les priorités du GPO, stratégies de qualité de service ont des règles de priorité pour résoudre les conflits lorsque plusieurs stratégies de QoS s’appliquent à un ensemble spécifique de trafic. Pour le trafic TCP ou UDP sortant, qu’une seule stratégie de QoS peut être appliquée à la fois, ce qui signifie que les stratégies de QoS n’ont pas d’effet cumulatif, par exemple où les taux de limitation serait être additionnés.

En règle générale, la stratégie de QoS avec le plus les conditions de correspondance wins. Quand plusieurs stratégies de QoS s’appliquent, les règles se répartissent en trois catégories : niveau de l’utilisateur et au niveau de l’ordinateur ; application par rapport à la quintuple réseau ; et entre le réseau quintuple.

Par *réseau quintuple*, nous entendons par là l’adresse IP source, adresse IP de destination, port source, port de destination et de protocole \(TCP/UDP\).  

 **Stratégie de QoS au niveau de l’utilisateur est prioritaire sur la stratégie de QoS au niveau de l’ordinateur**

Cette règle facilite grandement la gestion les administrateurs réseau de qualité de service de stratégie de groupe, en particulier pour les stratégies basées sur le groupe d’utilisateur. Par exemple, si l’administrateur réseau souhaite définir une stratégie de QoS pour un groupe d’utilisateurs, ils peuvent uniquement créer et distribuer un objet de stratégie de groupe à ce groupe. Ils n’aient problème sur les ordinateurs, ces utilisateurs sont connectés à et si ces ordinateurs a des stratégies de qualité de service en conflit définis, car, si un conflit existe, la stratégie au niveau de l’utilisateur est toujours prioritaire.

> [!NOTE]
>  Une stratégie de QoS au niveau de l’utilisateur s’applique uniquement au trafic généré par cet utilisateur. Autres utilisateurs d’un ordinateur spécifique et l’ordinateur, ne seront pas soumis à toutes les stratégies de QoS qui sont définis pour cet utilisateur.

 **Spécificité de l’application et étant prioritaires sur cinq fois de réseau**

Quand plusieurs stratégies de qualité de service correspond à un trafic spécifique, la stratégie plus spécifique est appliquée. Parmi les stratégies qui identifient des applications, une stratégie qui inclut le chemin d’accès du fichier de l’application émettrice est considéré comme plus spécifique qu’une autre stratégie qui identifie uniquement le nom de l’application (aucun chemin d’accès). Si plusieurs stratégies avec les applications s’appliquent toujours, les règles de priorité utilisent le réseau quintuple pour trouver la meilleure correspondance.

Vous pouvez également plusieurs stratégies QoS peuvent s’appliquer au trafic même en spécifiant des conditions sans chevauchement. Entre les conditions des applications et le quintuple de réseau, la stratégie qui spécifie l’application est considérée comme plus spécifique et est appliquée. 

Par exemple, stratégie spécifie uniquement un nom d’application (app.exe) et policy_B spécifie la destination IP adresse 192.168.1.0/24. Lorsque ces stratégies QoS sont en conflit \(app.exe envoie le trafic vers une adresse IP dans la plage de 192.168.4.0/24\), stratégie est appliquée.

 **Plus grande spécificité prioritaire au sein du réseau quintuple**

Pour les conflits de stratégie dans les cinq fois réseau, la stratégie avec les conditions de correspondance plus est prioritaire. Par exemple, supposons policy_C Spécifie l’adresse IP source « tout », adresse IP de destination 10.0.0.1, le port source « tout », « tout » de port de destination et protocole « TCP ». 

Ensuite, supposons policy_D Spécifie l’adresse IP source « tout », adresse IP de destination « any », port de destination 80 d’adresses 10.0.0.1, port source et protocole « TCP ». Puis policy_C et policy_D correspond à connexions pour 10.0.0.1 : 80 de destination. Étant donné que la stratégie de QoS s’applique la stratégie avec les conditions de correspondance la plus spécifiques, policy_D est prioritaire dans cet exemple.  
  
Toutefois, les stratégies QoS peuvent avoir un nombre égal de conditions. Par exemple, plusieurs stratégies peuvent chacun spécifier qu’un seul (mais pas dans le même) morceau du quintuple de réseau. Parmi les cinq fois réseau, l’ordre suivant provient d’une version ultérieure pour diminuer la priorité :

- Adresse IP source

- Adresse IP de destination

- Port source

- Port de destination

- Protocole (TCP ou UDP)

Au sein d’une condition spécifique, telles que l’adresse IP, une adresse IP plus spécifique est traitée avec une priorité plus élevée ; par exemple, une adresse IP 192.168.4.1 est plus spécifique que 192.168.4.0/24.

Concevez vos stratégies de QoS aussi précisément que possible simplifier la capacité de votre organisation pour comprendre quelles stratégies sont appliquées.

Pour la rubrique suivante de ce guide, consultez [erreurs et événements de stratégie de QoS](qos-policy-errors.md).

Pour la première rubrique de ce guide, consultez [stratégie de qualité de Service (QoS)](qos-policy-top.md).
