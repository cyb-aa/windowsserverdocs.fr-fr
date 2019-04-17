---
title: Gérer la stratégie de QoS
description: Cette rubrique fournit des instructions sur la création et gestion de la stratégie de qualité de Service (QoS) dans Windows Server2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 04fdfa54-6600-43d4-8945-35f75e15275a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5baa83e41c79a558f5bd32f77a234367726592c7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="manage-qos-policy"></a>Gérer la stratégie de QoS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur l’utilisation de l’Assistant Stratégie de QoS pour créer, modifier ou supprimer une stratégie de QoS.

>[!NOTE]
>  Outre cette rubrique, la documentation de gestion de stratégie de QoS suivante est disponible.
> 
>  - [Erreurs et événements de stratégie de QoS](qos-policy-errors.md)

Dans les systèmes d’exploitation Windows, stratégie de QoS combine les fonctionnalités de QoS basée sur des normes avec la facilité de gestion de stratégie de groupe. Configuration de cette combinaison permet simplifie l’application des stratégies de QoS aux objets de stratégie de groupe. Windows inclut un Assistant de stratégie de QoS pour vous aider à effectuer les tâches suivantes.

-  [Créer une stratégie de QoS](#bkmk_createpolicy)

-  [Afficher, modifier ou supprimer une stratégie de QoS](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>Créer une stratégie de QoS

Avant de créer une stratégie de QoS, il est important de comprendre les deux contrôles QoS clés qui sont utilisées pour gérer le trafic réseau:

- Valeur DSCP

-   Taux d’accélération

### <a name="prioritizing-traffic-with-dscp"></a>Définition des priorités de trafic avec DSCP

Comme indiqué dans l’exemple d’application métier du précédent, vous pouvez définir la priorité du trafic réseau sortant à l’aide de **spécifier la valeur DSCP** pour configurer une stratégie de QoS avec une valeur DSCP spécifique. 

Comme décrit dans la RFC2474, DSCP autorise les valeurs comprises entre 0 et 63 être spécifié dans le champ TOS d’un paquet IPv4 et dans le champ classe de trafic IPv6. Les routeurs réseau utilisent la valeur DSCP pour classer les paquets réseau et en file d’attente en conséquence.
  
> [!NOTE]
>  Par défaut, le trafic de Windows a la valeur DSCP 0.
  
Le nombre de files d’attente et leur comportement de la définition des priorités doive être décidés dans le cadre de la stratégie de QoS de votre organisation. Par exemple, votre organisation peut choisir d’avoir cinq files d’attente: trafic sensible à la latence, le trafic de contrôle, trafic critique d’entreprise, le trafic au mieux et le trafic de transfert de données en bloc.  
  
### <a name="throttling-traffic"></a>Limitation du trafic

Ainsi que les valeurs DSCP, la limitation est un autre contrôle de clé pour la gestion de la bande passante réseau. Comme mentionné précédemment, vous pouvez utiliser le **spécifier le taux d’accélération** paramètre pour configurer une stratégie de QoS avec un taux d’accélération spécifiques pour le trafic sortant. À l’aide de la limitation, une stratégie de QoS limite le trafic réseau sortant à un taux d’accélération spécifié. Le marquage DSCP et la limitation peut être utilisés conjointement pour gérer efficacement le trafic.

>[!NOTE]
>Par défaut, le **spécifier le taux d’accélération** case à cocher n’est pas activée.

Pour créer une stratégie de QoS, modifiez les paramètres d’un objet (stratégie de groupe) à partir de dans l’outil de la Console de gestion des stratégies de groupe (GPMC). La console GPMC puis ouvre l’éditeur d’objets de stratégie de groupe.

Noms de stratégie de QoS doivent être uniques. Comment les stratégies sont appliquées aux serveurs et les utilisateurs finaux dépend d’où la stratégie de QoS est stockée dans l’éditeur d’objets de stratégie de groupe:

- Une stratégie de QoS dans Configuration ordinateur\Paramètres Settings\QoS de stratégie s’applique aux ordinateurs, quel que soit l’utilisateur qui est actuellement connecté. Vous utilisez généralement des stratégies de QoS basée sur l’ordinateur pour les ordinateurs serveurs.

- Une stratégie de QoS dans utilisateur configuration Settings\QoS stratégie s’applique aux utilisateurs une fois qu’ils sont connectés, quel que soit l’ordinateur sur lequel ils sont connectés.

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>Pour créer une nouvelle stratégie de QoS avec l’Assistant Stratégie de QoS

-   Dans l’éditeur d’objets de stratégie de groupe, cliquez sur une du **stratégie de QoS** nœuds, puis cliquez sur **créer une nouvelle stratégie **.

### <a name="wizard-page-1---policy-profile"></a>Page de l’Assistant 1 - profil de stratégie

Sur la première page de l’Assistant Stratégie de QoS, vous pouvez spécifier un nom de la stratégie et configurer la qualité de service de contrôle du trafic réseau sortant.

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>Pour configurer la page de profil de stratégie de l’Assistant Stratégie de QoS

1. Dans **nom de la stratégie**, tapez un nom pour la stratégie de QoS. Le nom doit identifier la stratégie.

2. Vous pouvez éventuellement utiliser **spécifier la valeur DSCP** pour activer le marquage DSCP, puis configurez une valeur DSCP comprise entre 0 et 63.

3. Vous pouvez éventuellement utiliser **spécifier le taux d’accélération** pour activer la limitation du trafic et configurer le taux d’accélération. La valeur du taux d’accélération doit être supérieure à 1, et vous pouvez spécifier des unités de kilo-octets par seconde \(KBps\) ou en mégaoctets par seconde \(MBps\).

4. Cliquez sur **suivant**.

### <a name="wizard-page-2---application-name"></a>Page de l’Assistant 2 - nom de l’Application

Dans la deuxième page de l’Assistant Stratégie de QoS, vous pouvez appliquer la stratégie à toutes les applications, à une application spécifique identifiée par son nom d’exécutable, un chemin d’accès et le nom de l’application, ou pour les applications serveur HTTP qui gèrent les demandes pour une URL spécifique.

- **Toutes les applications** Spécifie que les paramètres de gestion du trafic sur la première page de l’Assistant Stratégie de QoS s’appliquent à toutes les applications.

- **Uniquement les applications possédant ce nom d’exécutable** Spécifie que les paramètres de gestion du trafic sur la première page de l’Assistant Stratégie de QoS pour une application spécifique. Le fichier exécutable de nom et doit se terminer par l’extension de nom de fichier .exe.

- **Seules les applications serveur HTTP répond aux demandes pour cette URL** Spécifie que les paramètres de gestion du trafic sur la première page de l’Assistant Stratégie de QoS s’appliquent à certaines applications serveur HTTP uniquement.

Si vous le souhaitez, vous pouvez entrer le chemin d’accès de l’application. Pour spécifier un chemin d’accès de l’application, incluez le chemin d’accès avec le nom de l’application. Le chemin d’accès peut inclure des variables d’environnement. Par exemple, %ProgramFiles%\My Path\MyApp.exe ou c:\program files\my application path\myapp.exe.

>[!NOTE]
>Le chemin d’accès de l’application ne peut pas inclure un chemin d’accès correspond à un lien symbolique.

L’URL doit être conforme à [RFC1738](http://tools.ietf.org/html/rfc1738), sous la forme de `http[s]://<hostname\>:<port\>/<url-path>`. Vous pouvez utiliser un caractère générique, `‘*’`, pour `<hostname>`et/ou `<port>`, par exemple, `http://training.\*/, https://\*.\*`, mais le caractère générique ne peut pas indiquer une sous-chaîne de `<hostname>`ou `<port>`.

En d’autres termes, ni `http://my\*site/`ni `https://\*training\*/`est valide. 

Si vous le souhaitez, vous pouvez vérifier **incluent les sous-répertoires et fichiers** pour effectuer une correspondance sur tous les sous-répertoires et fichiers après une URL. Par exemple, si cette option est cochée et l’URL est `http://training`, stratégie de QoS prendra en compte les demandes de` http://training/video` une bonne correspondance.

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>Pour configurer la page Nom de l’Application de l’Assistant Stratégie de QoS

1. Dans **cette stratégie de QoS s’applique à**, sélectionnez **toutes les applications** ou **uniquement les applications possédant ce nom d’exécutable **.

2. Si vous sélectionnez **uniquement les applications possédant ce nom d’exécutable**, spécifiez un nom d’exécutable se terminant par l’extension de nom de fichier .exe.

3. Cliquez sur **suivant**.

### <a name="wizard-page-3---ip-addresses"></a>Page de l’Assistant 3 - adresses IP

Dans la troisième page de l’Assistant Stratégie de QoS, vous pouvez spécifier les conditions d’adresse IP de la stratégie de qualité de service, notamment les suivantes:

- Toutes les adresses IPv4 ou IPv6 ou les adresses IPv4 ou IPv6 sources spécifiques de la source

- Toutes les adresses IPv4 ou IPv6 de destination ou les adresses IPv4 ou IPv6 de destination spécifique

Si vous sélectionnez **uniquement pour l’adresse IP source suivant** ou **uniquement pour l’adresse IP de destination à l’adresse suivante**, vous devez taper une des opérations suivantes:

- Adresse IPv4, telles que `192.168.1.1`

- Un préfixe d’adresse IPv4 utilisant la notation de préfixe réseau, tels que `192.168.1.0/24`

- Par exemple, d’adresses d’IPv6 `3ffe:ffff::1`

- Comme préfixe, d’IPv6 d’adresses `3ffe:ffff::/48`

Si vous sélectionnez les deux **uniquement pour l’adresse IP source suivant** et **uniquement pour l’adresse IP de destination à l’adresse suivante**, adresses ou préfixes d’adresses doivent être soit IPv4 ou IPv6 basée sur.

Si vous avez spécifié l’URL pour les applications de serveur HTTP dans la page précédente de l’Assistant, vous remarquerez que l’adresse IP source pour la stratégie de QoS sur cette page de l’Assistant est grisée. 

Cela est vrai, car l’adresse IP source est l’adresse du serveur HTTP et il n’est pas configurable ici. En revanche, vous pouvez toujours personnaliser la stratégie en spécifiant l’adresse IP de destination. Cela permet de créer des stratégies différentes pour différents clients à l’aide des mêmes applications serveur HTTP.

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>Pour configurer la page adresses IP de l’Assistant Stratégie de QoS

1. Dans **cette stratégie de QoS s’applique à** (source), sélectionnez **une adresse IP source** ou **uniquement pour l’adresse IP suivante adresse source **.

2. Si vous avez sélectionné **uniquement l’adresse IP source**, spécifiez une adresse IPv4 ou IPv6 ou un préfixe.

3. Dans **cette stratégie de QoS s’applique à** (destination), sélectionnez **n’importe quelle adresse de destination** ou **uniquement pour l’adresse IP de destination.**

4. Si vous avez sélectionné **uniquement pour l’adresse IP de destination**, spécifiez une adresse IPv4 ou IPv6 ou un préfixe qui correspond au type d’adresse ou le préfixe spécifié pour l’adresse source.

5.  Cliquez sur **suivant**.  

### <a name="wizard-page-4---protocols-and-ports"></a>Page de l’Assistant 4 - protocoles et Ports

Sur la quatrième page de l’Assistant Stratégie de QoS, vous pouvez spécifier les types de trafic et les ports qui sont contrôlés par les paramètres sur la première page de l’Assistant. Vous pouvez spécifier:  
-   Le trafic TCP, trafic UDP ou les deux  

-   Tous les ports, une plage de ports source ou un port source spécifique de la source

-   Tous les ports de destination, une plage de ports de destination ou un port de destination spécifique  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>Pour configurer la page protocoles et Ports de l’Assistant Stratégie de QoS

1. Dans **sélectionner le protocole de cette stratégie de QoS s’applique à**, sélectionnez **TCP**, **UDP**, ou **TCP et UDP **.

2. Dans **spécifier le numéro de port source**, sélectionnez **à partir de n’importe quel port source** ou **à partir de ce numéro de port source **.

3. Si vous avez sélectionné **à partir de ce numéro de port source**, tapez un numéro de port compris entre 1 et 65535.

     Si vous le souhaitez, vous pouvez spécifier une plage de ports, au format «*faible*:*haute*, «où *faible* et *haute* représentent les limites inférieure et supérieure de la plage de ports (inclus). *Faible* et *haute* chacun doit être un nombre compris entre 1 et 65535. Aucun espace n’est autorisé entre le signe deux-points (:)) et les nombres.

4. Dans **spécifier le numéro de port de destination**, sélectionnez **n’importe quel port de destination** ou **vers ce numéro de port de destination**.

5. Si vous avez sélectionné **vers ce numéro de port de destination** à l’étape précédente, tapez un numéro de port compris entre 1 et 65535.

Pour terminer la création de la nouvelle stratégie de QoS, cliquez sur **Terminer** sur le **protocoles et Ports** page de l’Assistant Stratégie de QoS. Une fois terminé, la nouvelle stratégie QoS est répertoriée dans le volet de détails de l’éditeur d’objet de stratégie de groupe.  
  
Pour appliquer les paramètres de stratégie de QoS aux utilisateurs ou ordinateurs, liez l’objet de stratégie de groupe dans lequel les stratégies de qualité de service sont situés à un conteneur de Services de domaine ActiveDirectory, par exemple, un domaine, un site ou une unité d’organisation (UO).  
  
##  <a name="bkmk_editpolicy"></a>Afficher, modifier ou supprimer une stratégie de QoS

Les pages de la stratégie de QoS Assistant décrites précédemment correspondent aux pages de propriétés qui sont affichent lorsque vous affichez ou modifiez les propriétés d’une stratégie.  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>Pour afficher les propriétés d’une stratégie de QoS  
  
-   Cliquez sur le nom de la stratégie dans le volet de détails de l’éditeur d’objet de stratégie de groupe, puis cliquez sur **propriétés**.  
  
     L’éditeur d’objets de stratégie de groupe affiche la page de propriétés avec les onglets suivants:  
  
    -   Profil de stratégie  
  
    -   Nom de l’application  
  
    -   Adresses IP  
  
    -   Protocoles et Ports  
  
### <a name="to-edit-a-qos-policy"></a>Pour modifier une stratégie de QoS  
  
-   Cliquez sur le nom de la stratégie dans le volet de détails de l’éditeur d’objet de stratégie de groupe, puis cliquez sur **modifier la stratégie existante**.  
  
     L’éditeur d’objets de stratégie de groupe affiche les **modifier une stratégie de QoS existante** boîte de dialogue.  
  
### <a name="to-delete-a-qos-policy"></a>Pour supprimer une stratégie de QoS  
  
-   Cliquez sur le nom de la stratégie dans le volet de détails de l’éditeur d’objet de stratégie de groupe, puis cliquez sur **supprimer la stratégie**.  
  
### <a name="qos-policy-gpmc-reporting"></a>Rapport de stratégie de QoS de la console GPMC 

Après avoir appliqué un certain nombre de stratégies de QoS de votre organisation, il peut être utile ou nécessaire à consulter régulièrement la façon dont les stratégies sont appliquées. Un résumé des stratégies de qualité de service pour un utilisateur spécifique ou un ordinateur sont consultables à l’aide de rapports de la console GPMC.  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>Pour exécuter l’Assistant résultats de stratégie de groupe pour un rapport des stratégies de QoS  
  
-   Dans la console GPMC, cliquez sur le **résultats de stratégie de groupe** nœud et sélectionnez le menu option **Assistant résultats de stratégie de groupe.**  
  
Une fois les résultats de stratégie de groupe sont générées, cliquez sur le **paramètres** onglet. Sur le **paramètres** onglet, les stratégies de qualité de service se trouve dans les nœuds «Stratégie d’ordinateur Configuration ordinateur\Paramètres Settings\QoS» et «Configuration ordinateur\Paramètres Settings\QoS de stratégie».  
  
Sur le **paramètres**, les stratégies de qualité de service sont répertoriés par leur nom de stratégie de QoS avec leur valeur DSCP, le taux d’accélération, des conditions de la stratégie, puis gagnante objet stratégie de groupe répertoriés dans la même ligne.  
  
L’affichage des résultats de stratégie de groupe identifie de manière unique l’objet de stratégie de groupe gagnant. Lorsque plusieurs objets de stratégie de groupe ont des stratégies de QoS avec le même nom de stratégie de QoS, avec la priorité la plus élevée de la stratégie de groupe est appliqué. Il s’agit de l’objet de stratégie de groupe gagnant. Stratégies en conflit QoS (identifiés par le nom de la stratégie) qui sont attachés à une objet stratégie de groupe ne sont pas appliquées de priorité inférieure. Notez que les priorités de stratégie de groupe définissent les stratégies de QoS sont déployées dans le site, le domaine ou l’unité d’organisation, selon le cas. Après le déploiement, à un niveau d’utilisateur ou un ordinateur, le [règles de priorité de stratégie de QoS](#BKMK_precedencerules) déterminer le trafic qui est autorisé et bloqué.  
  
La valeur DSCP, le taux d’accélération et conditions de la stratégie de la stratégie de qualité de service sont également visibles dans Éditeur du objet stratégie de groupe (GPOE)  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>Paramètres avancés pour les utilisateurs itinérants et à distance  
Avec la stratégie de qualité de service, l’objectif est de gérer le trafic sur un réseau d’entreprise. Dans les scénarios mobiles, les utilisateurs peuvent être envoyer du trafic ou désactiver le réseau d’entreprise. Étant donné que les stratégies de QoS ne sont pas pertinents du réseau de l’entreprise, les stratégies de QoS sont activées uniquement sur des interfaces réseau qui sont connectés à l’entreprise pour Windows8, Windows7 ou WindowsVista.  
  
Par exemple, un utilisateur peut se connecter son ordinateur portable au réseau de son entreprise via un réseau privé virtuel (VPN) à partir d’un café. Pour VPN, l’interface réseau physique (par exemple, sans fil) n’aura pas appliquer des stratégies de qualité de service. Toutefois, l’interface VPN possède des stratégies de QoS appliquées, car il se connecte à l’entreprise. Si l’utilisateur entre ultérieurement le réseau d’une autre entreprise qui ne dispose pas d’une relation d’approbation ADDS, les stratégies de qualité de service ne seront pas activées.  
  
Notez que ces scénarios mobiles ne s’appliquent pas aux charges de travail serveur. Par exemple, un serveur avec plusieurs cartes réseau peut-être se trouvent sur le bord de réseau de l’entreprise. Le service informatique peut choisir de trafic de limitation de bande passante stratégies QoS qui egresses l’entreprise; toutefois, cette carte réseau qui envoie ce trafic sortant ne se connecte pas nécessairement sur le réseau d’entreprise. Pour cette raison, les stratégies de QoS sont toujours activées sur toutes les interfaces réseau d’un ordinateur exécutant Windows Server2012.  
  
> [!NOTE]
>  L’activation sélective s’applique uniquement aux stratégies de qualité de service et pas pour les paramètres QoS avancés traités ci-après dans ce document.  
  
### <a name="advanced-qos-settings"></a>Paramètres avancés de QoS

Paramètres avancés de QoS fournissent des contrôles supplémentaires pour les administrateurs informatiques à gérer la consommation réseau d’ordinateur et le marquage DSCP. Paramètres avancés de QoS s’appliquent uniquement au niveau de l’ordinateur, tandis que les stratégies de qualité de service peuvent être appliquées à des niveaux de l’ordinateur et l’utilisateur.

#### <a name="to-configure-advanced-qos-settings"></a>Pour configurer les paramètres avancés de QoS

1.  Cliquez sur **Configuration ordinateur**, puis cliquez sur **Windows des paramètres de stratégie de groupe**.
  
2.  Avec le bouton droit **stratégie de QoS**, puis cliquez sur **paramètres avancés de QoS**.

     L’illustration suivante montre les deux fonctions avancées onglets de paramètres de qualité de service: **trafic TCP entrant** et **remplacer le marquage DSCP**.
  
> [!NOTE]
>  Les paramètres QoS avancés sont des paramètres de stratégie de groupe au niveau de l’ordinateur.
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>Les paramètres QoS avancés: trafic TCP entrant

**Le trafic TCP entrant** contrôle la consommation de bande passante TCP du côté du destinataire, tandis que les stratégies de QoS affectent le trafic TCP et UDP sortant. 

En définissant un débit plus faible niveau sur le **trafic TCP entrant** onglet, TCP limite la taille de celui-ci est publiée la fenêtre de réception TCP. L’effet de ce paramètre sera taux de débit accru et l’utilisation de connexions TCP avec des bandes passantes plus élevées ou des latences (produit du délai de la bande passante) des liaisons. Par défaut, les ordinateurs qui exécutent Windows Server2012, Windows8, Windows Server2008R2, Windows Server2008 et WindowsVista sont définis au niveau de débit maximal.
  
La réception TCP fenêtre a changé dans Windows Server2012, Windows8, Windows Server2008R2, Windows Server2008 et WindowsVista à partir de versions précédentes de Windows. Les versions précédentes de Windows limitée de la fenêtre côté réception TCP pour un maximum de 64kilo-octets (Ko), tandis que Windows Server2012, Windows8, Windows Server2008R2, Windows Server2008 et WindowsVista dynamiquement taille de la fenêtre côté réception jusqu'à 16mégaoctets (Mo). Dans le contrôle de trafic TCP entrant, vous pouvez contrôler le niveau du débit entrant en définissant la valeur maximale que peut atteindre la-fenêtre de réception TCP. Les niveaux correspondent aux valeurs maximum suivantes. 
  
|Niveau du débit entrant|Durée maximale|  
|------------------------|-------|  
|0|64KO|
|1|256KO|
|2|1MO|
|3|16MO|

La taille de fenêtre réelle peut être une valeur égale ou inférieure à la valeur maximale, en fonction des conditions du réseau.

###### <a name="to-set-the-tcp-receive-side-window"></a>Pour définir la fenêtre côté réception TCP

1. Dans l’éditeur d’objets de stratégie de groupe, cliquez sur **stratégie ordinateur Local**, cliquez sur **paramètres Windows**, cliquez avec le bouton droit **stratégie de QoS**, puis cliquez sur **paramètres avancés de QoS**.
  
2. Dans **le débit de réception TCP**, sélectionnez **configurer débit de réception TCP**, puis sélectionnez le niveau de débit que vous souhaitez.

3.  Lier l’objet de stratégie de groupe à l’unité d’organisation.

#### <a name="advanced-qos-settings-dscp-marking-override"></a>Les paramètres QoS avancés: remplacer le marquage DSCP

Remplacer le marquage DSCP restreint la capacité des applications pour spécifier, ou «marquer»: DSCP valeurs autres que celles spécifiées dans les stratégies de qualité de service. En spécifiant que les applications sont autorisées à définir des valeurs DSCP, les applications peuvent définir des valeurs DSCP différente de zéro. 

En spécifiant **ignorer**, les applications qui utilisent QoS APIs auront leurs valeurs DSCP définies à zéro, et uniquement les stratégies de QoS peuvent définir des valeurs DSCP. 

Par défaut, les ordinateurs exécutant Windows Server2016, Windows10, Windows Server2012R2, Windows8.1, Windows Server2012, Windows8, Windows Server2008R2, Windows Server2008 et WindowsVista permettent aux applications de spécifier les valeurs DSCP; applications et les périphériques qui n’utilisent pas les APIs QoS ne sont pas remplacés.

##### <a name="wireless-multimedia-and-dscp-values"></a>Valeurs DSCP et des applications multimédias sans fil

Le [Wi-Fi Alliance](https://go.microsoft.com/fwlink/?LinkId=160769) a établi une certification multimédias sans fil \(WMM\) qui définit quatre catégories d’accès \(WMM_AC\) pour hiérarchiser le trafic réseau transmis sur un réseau sans fil Wi\-Fi. Incluent les catégories d’accès \ (dans l’ordre de priority\ plus élevé-à-la plus basse): voix, vidéo, meilleur effort et en arrière-plan; respectivement abrégé en VO VI, BE et en noir. La spécification WMM définit le DSCP valeurs correspondent avec chacune des quatre accès catégories:
  
|Valeur DSCP|Catégorie d’accès WMM|
|----------|-------------------|
|48-63|Voix (VO)|
|32-47|Vidéo (VI)|
|24-31, 0-7|Meilleur effort (BE)|
|8-23|En arrière-plan (noir)|

Vous pouvez créer des stratégies de QoS qui utilisent ces valeurs DSCP pour vérifier que les ordinateurs portables avec Wi\-Fi Certified™ pour les cartes sans fil WMM reçoivent gestion prioritaire lorsque associée Wi\-Fi certifiés pour les points d’accès WMM.
  
### <a name="BKMK_precedencerules"></a>Règles de priorité de stratégie de QoS

Comme les priorités d’objet de stratégie de groupe, les stratégies de QoS ont des règles de priorité pour résoudre les conflits lorsque plusieurs stratégies de QoS s’applique à un ensemble spécifique de trafic. TCP ou UDP du trafic sortant, qu’une seule stratégie de QoS peut être appliquée à la fois, ce qui signifie que les stratégies de QoS n’ont pas d’effet cumulatif, par exemple, où les taux de limitation de bande passante seraient additionnées.

En règle générale, la stratégie de QoS avec le plus correspondantes conditions wins. Lorsque plusieurs stratégies de QoS s’appliquent, les règles se répartissent en trois catégories: niveau de l’utilisateur et au niveau ordinateur; application et les cinq fois réseau; et entre le réseau quintuple.

Par *réseau quintuple*, nous signifie que l’adresse IP source, adresse IP de destination, port source, le port de destination et \(TCP/UDP\) de protocole.  

 **Stratégie de QoS au niveau de l’utilisateur est prioritaire sur la stratégie de QoS au niveau ordinateur**

Cette règle facilite considérablement la gestion des administrateurs réseau de qualité de service de stratégie de groupe, en particulier pour les stratégies utilisateur groupe. Par exemple, si l’administrateur réseau souhaite définir une stratégie de QoS pour un groupe d’utilisateurs, ils peuvent simplement créer et distribuer un objet de stratégie de groupe à ce groupe. Cela n’est pas vous inquiétez pas sur les ordinateurs, ces utilisateurs sont connectés à et si ces ordinateurs aura des stratégies de qualité de service en conflit définis, car, si un conflit, la stratégie au niveau de l’utilisateur est toujours prioritaire.

> [!NOTE]
>  Une stratégie de QoS au niveau de l’utilisateur s’applique uniquement au trafic généré par l’utilisateur. Autres utilisateurs d’un ordinateur spécifique et l’ordinateur lui-même, ne seront pas soumis à des stratégies de QoS qui sont définies pour cet utilisateur.

 **Spécificité de l’application et est prioritaire sur cinq fois réseau**

Lorsque plusieurs stratégies de QoS correspondent au trafic spécifique, la stratégie plus spécifique est appliquée. Parmi les stratégies qui identifient les applications, une stratégie qui inclut le chemin d’accès du fichier de l’application d’envoi est considéré comme plus spécifique à une autre stratégie qui identifie uniquement le nom de l’application (aucun chemin d’accès). Si plusieurs stratégies avec les applications s’appliquent toujours, les règles de priorité utilisent le réseau quintuple pour trouver la meilleure correspondance.

Vous pouvez également plusieurs stratégies de qualité de service peuvent s’appliquer au trafic même en spécifiant des conditions non qui se chevauchent. Entre les conditions d’applications et les cinq fois réseau, la stratégie qui spécifie l’application est considéré comme plus spécifique et est appliquée. 

Par exemple, stratégie spécifie uniquement une application nom (app.exe) et policy_B spécifie la destination IP adresse 192.168.1.0/24. Lorsque ces stratégies de qualité de service sont en conflit \ (app.exe envoie le trafic à une adresse IP dans la plage de 192.168.4.0/24\), stratégie est appliqué.

 **Spécificité plus prioritaire au sein du réseau quintuple**

Pour les conflits de stratégie dans les cinq fois réseau, la stratégie avec les conditions plus correspondantes est prioritaire. Par exemple, supposons que policy_C Spécifie l’adresse IP source «any», adresse IP de destination d’adresses 10.0.0.1, port source port de destination «quelconque», «tous» et «TCP» de protocole. 

Ensuite, supposons que policy_D Spécifie l’adresse IP source «any», adresse IP de destination adresse 10.0.0.1, port source «n’importe quel», port de destination 80 et du protocole «TCP». Policy_C et policy_D alors des connexions à destination 10.0.0.1:80. Étant donné que la stratégie de QoS s’applique la stratégie avec les conditions correspondantes plus spécifiques, policy_D est prioritaire dans cet exemple.  
  
Toutefois, les stratégies de QoS peut-être même nombre de conditions. Par exemple, plusieurs stratégies peuvent spécifient chacun une seule (mais pas les mêmes) commentaire les cinq fois réseau. Parmi les cinq fois réseau, l’ordre suivant est de plus élevé de priorité plus basse:

- Adresse IP source

- Adresse IP de destination

- Port source

- Port de destination

- Protocole (TCP ou UDP)

Au sein d’une condition spécifique, comme adresse IP, une adresse IP plus spécifique est traitée avec une priorité plus élevée; par exemple, une adresse IP 192.168.4.1 est plus spécifique que 192.168.4.0/24.

Concevez vos stratégies de QoS aussi spécifiquement que possible pour simplifier la capacité de votre organisation à comprendre les stratégies sont appliquées.

Pour la rubrique suivante dans ce guide, voir [erreurs et événements de stratégie de QoS](qos-policy-errors.md).

Pour la première rubrique de ce guide, voir [stratégie de qualité de Service (QoS)](qos-policy-top.md).
