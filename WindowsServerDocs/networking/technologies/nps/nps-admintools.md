---
title: Gestion du serveur NPS avec les outils d’Administration
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur les outils que vous pouvez utiliser pour gérer le serveur NPS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cdb82c5bc1c541f19b1b2f4c8db837af4a812f81
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-management-with-administration-tools"></a>Gestion du serveur NPS avec les outils d’Administration

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur les outils que vous pouvez utiliser pour gérer vos serveurs NPS.

Après avoir installé le serveur NPS, vous pouvez administrer des serveurs NPS:

- Localement, à l’aide du composant logiciel enfichable MMC NPS \(MMC\), la console NPS statique dans Outils d’administration, les commandes Windows PowerShell ou les commandes d’environnement réseau \(Netsh\) d’un serveur NPS.
- À partir d’un serveur NPS à distance, à l’aide du composant logiciel enfichable MMC NPS, les commandes Netsh pour NPS, les commandes Windows PowerShell pour NPS ou connexion Bureau à distance.
- À partir d’une station de travail distante, à l’aide de connexion Bureau à distance en combinaison avec d’autres outils, tels que la console MMC NPS ou Windows PowerShell.

>[!NOTE]
>Dans Windows Server 2016, vous pouvez gérer le serveur NPS local à l’aide de la console NPS. Pour gérer les serveurs NPS locales et distantes, vous devez utiliser le logiciel enfichable MMC NPS.

Les sections suivantes fournissent des instructions sur la façon de gérer vos serveurs NPS locaux et distants.

## <a name="configure-the-local-nps-server-by-using-the-nps-console"></a>Configurer le serveur NPS Local à l’aide de la Console NPS

Une fois que vous avez installé le serveur NPS, vous pouvez utiliser cette procédure pour gérer le serveur NPS local à l’aide de la console MMC NPS.

**Informations d’identification d’administration** 

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

### <a name="to-configure-the-local-nps-server-by-using-the-nps-console"></a>Pour configurer le serveur NPS local à l’aide de la console NPS

1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **serveur NPS**. La console NPS s’ouvre.

2. Dans la console NPS, cliquez sur le serveur NPS \(Local\). Dans le volet détails, choisissez **Configuration Standard** ou **Advanced Configuration**, puis effectuez l’une des opérations suivantes en fonction de votre sélection:
    - Si vous choisissez **Configuration Standard**, sélectionnez un scénario dans la liste, puis suivez les instructions pour démarrer un Assistant de configuration.
    - Si vous choisissez **Advanced Configuration**, cliquez sur la flèche pour développer **options de Configuration avancée**, puis passez en revue et configurer les options disponibles basées sur la fonctionnalité de serveur NPS que vous souhaitez - serveur RADIUS, proxy RADIUS ou les deux.

## <a name="manage-multiple-nps-servers-by-using-the-nps-mmc-snap-in"></a>Gérer plusieurs serveurs NPS à l’aide de la console MMC NPS enfichable

Vous pouvez utiliser cette procédure pour gérer le serveur NPS local et plusieurs serveurs NPS à distance à l’aide du logiciel enfichable MMC NPS.

Avant d’effectuer la procédure ci-dessous, vous devez installer NPS sur l’ordinateur local et sur des ordinateurs distants.

Selon les conditions du réseau et le nombre de serveurs NPS que vous gérez à l’aide du logiciel enfichable MMC NPS, la réponse de la console MMC enfichable peut être lente. En outre, le trafic configuration de serveur NPS est envoyé sur le réseau pendant une session d’administration à distance à l’aide du serveur NPS de composants. Assurez-vous que votre réseau est physiquement sécurisé et que les utilisateurs malveillants n’ont pas accès à ce trafic.

**Informations d’identification d’administration** 

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

### <a name="to-manage-multiple-nps-servers-by-using-the-nps-snap-in"></a>Pour gérer plusieurs serveurs NPS à l’aide du serveur NPS de composants

1. Pour ouvrir la console MMC, exécutez Windows PowerShell en tant qu’administrateur. Dans Windows PowerShell, tapez **mmc**, puis appuyez sur ENTRÉE. MicrosoftManagement Console s’ouvre.
2. Dans la console MMC, sur le **fichier** menu, cliquez sur **Ajout/Suppression de composants**. Le **ajouter ou supprimer des composants logiciels enfichables** boîte de dialogue s’ouvre.
3. Dans **ajouter ou supprimer des composants logiciels enfichables**, dans **disponibles composants logiciels enfichables**, faites défiler la liste, cliquez sur **serveur NPS**, puis cliquez sur **ajouter**. Le **sélectionner un ordinateur** boîte de dialogue s’ouvre.
4. Dans **sélectionner un ordinateur**, vérifiez que **ordinateur Local \ (l’ordinateur sur lequel cette console est running\)** est sélectionné, puis cliquez sur **OK**. Le composant logiciel enfichable pour le serveur NPS local est ajouté à la liste de **sélectionné des composants logiciels enfichables**.
5. Dans **ajouter ou supprimer des composants logiciels enfichables**, dans **disponibles composants logiciels enfichables**, assurez-vous que **serveur NPS** est toujours activée, puis cliquez sur **ajouter**. Le **sélectionner un ordinateur** boîte de dialogue s’ouvre à nouveau.
6. Dans **sélectionner un ordinateur**, cliquez sur **un autre ordinateur**, puis tapez l’adresse IP ou le nom de domaine complet nom \(FQDN\) du serveur NPS à distance que vous souhaitez gérer à l’aide du serveur NPS de composants. Si vous le souhaitez, vous pouvez cliquer sur **Parcourir** consulter le répertoire de l’ordinateur que vous souhaitez ajouter. Cliquez sur **OK**.
7. Répétez les étapes 5 et 6 pour ajouter des serveurs NPS pour le serveur NPS de composants. Lorsque vous avez ajouté tous les serveurs NPS que vous souhaitez gérer, cliquez sur **OK**.
8. Pour enregistrer le composant logiciel enfichable NPS pour une utilisation ultérieure, cliquez sur **fichier**, puis cliquez sur **enregistrer**. Dans le **Enregistrer sous** boîte de dialogue, accédez à l’emplacement du disque dur sur lequel vous souhaitez enregistrer le fichier, tapez un nom pour votre fichier \(.msc\) de Microsoft Management Console, puis cliquez sur **enregistrer**. 

## <a name="manage-an-nps-server-by-using-remote-desktop-connection"></a>Gérer un serveur NPS à l’aide de connexion Bureau à distance

Vous pouvez utiliser cette procédure pour gérer un serveur NPS à distance à l’aide de connexion Bureau à distance.

À l’aide de connexion Bureau à distance, vous pouvez gérer à distance vos serveurs NPS exécutant Windows Server 2016. Vous pouvez gérer à distance de serveurs NPS à partir d’un ordinateur exécutant Windows 10 ou des systèmes d’exploitation client Windows.

Vous pouvez utiliser Connexion Bureau à distance pour gérer plusieurs serveurs NPS à l’aide d’une des deux méthodes.

1. Créer une connexion Bureau à distance pour chacun de vos serveurs NPS individuellement.
2. Utiliser le Bureau à distance pour se connecter à un serveur NPS, puis utiliser la console MMC NPS sur ce serveur pour gérer les autres serveurs à distance. Pour plus d’informations, voir la section précédente **gérer plusieurs serveurs NPS à l’aide du logiciel enfichable MMC NPS**.

**Informations d’identification d’administration** 

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur le serveur NPS.

### <a name="to-manage-an-nps-server-by-using-remote-desktop-connection"></a>Pour gérer un serveur NPS à l’aide de connexion Bureau à distance

1. Sur chaque serveur NPS que vous souhaitez gérer à distance, dans le Gestionnaire de serveur, sélectionnez **serveur Local**. Dans le volet de détails du Gestionnaire de serveur, consultez le **Bureau à distance** paramètre et effectuez l’une des opérations suivantes. 
    1. Si la valeur de la **Bureau à distance** paramètre est **activé**, vous n’avez pas besoin d’effectuer certaines étapes de cette procédure. Passez à l’étape 4 pour commencer à configurer les autorisations utilisateur du Bureau à distance.
    2. Si le **Bureau à distance** paramètre est **désactivé**, cliquez sur le mot **désactivé**. Le **propriétés système** boîte de dialogue s’ouvre sur la **distant** onglet.
2. Dans **Bureau à distance**, cliquez sur **autoriser les connexions à distance à cet ordinateur**. Le **connexion Bureau à distance** boîte de dialogue s’ouvre. Effectuez l’une des opérations suivantes.
    1. Pour personnaliser les connexions réseau qui sont autorisées, cliquez sur **pare-feu Windows avec fonctions avancées de sécurité**, puis configurez les paramètres que vous voulez autoriser. 
    2. Pour activer la connexion Bureau à distance pour toutes les connexions sur l’ordinateur réseau, cliquez sur **OK**.
3. Dans **propriétés système**, dans **Bureau à distance**, décidez si vous souhaitez activer **autoriser les connexions uniquement à partir d’ordinateurs exécutant Bureau à distance avec authentification NLA**et effectuez votre sélection.
4. Cliquez sur **sélectionnez utilisateurs**. Le **Remote Desktop Users** boîte de dialogue s’ouvre.
5. Dans **Remote Desktop Users**, pour accorder l’autorisation à un utilisateur pour se connecter à distance sur le serveur NPS, cliquez sur **ajouter**, puis tapez le nom d’utilisateur pour le compte d’utilisateur. Cliquez sur **OK**.
6. Répétez l’étape 5 pour chaque utilisateur pour lequel vous souhaitez accorder l’autorisation d’accès à distance sur le serveur NPS. Lorsque vous avez terminé d’ajouter des utilisateurs, cliquez sur **OK** pour fermer la **Remote Desktop Users** boîte de dialogue et **OK** à nouveau pour fermer la **propriétés système** boîte de dialogue.
7. Pour vous connecter à un serveur NPS à distance que vous avez configurée à l’aide de la procédure précédente, cliquez sur **Démarrer**, faites défiler la liste alphabétique, puis cliquez sur **Accessoires Windows**, puis cliquez sur **connexion Bureau à distance**. Le **connexion Bureau à distance** boîte de dialogue s’ouvre.
8. Dans le **connexion Bureau à distance** la boîte de dialogue dans **ordinateur**, tapez le nom du serveur NPS ou l’adresse IP. Si vous préférez, cliquez sur **Options**, configurer les options de connexion supplémentaires, puis cliquez sur **enregistrer** pour enregistrer la connexion pour une utilisation répétée.
9. Cliquez sur **connexion**et lorsque vous y êtes invité fournissent des informations d’identification du compte utilisateur d’un compte qui dispose des autorisations pour ouvrir une session et de configurer le serveur NPS.

## <a name="use-netsh-nps-commands-to-manage-an-nps-server"></a>Utilisez les commandes Netsh NPS pour gérer un serveur NPS

Vous pouvez utiliser des commandes dans le contexte Netsh NPS pour afficher et définir la configuration de l’authentification, d’autorisation, gestion des comptes et l’audit de base de données utilisée par NPS et le service d’accès à distance. Utilisez les commandes dans le contexte Netsh NPS pour:

- Configurer ou reconfigurer un serveur NPS, y compris tous les aspects de NPS sont également disponibles pour la configuration à l’aide de la console NPS dans l’interface Windows.
- Exporter la configuration d’un serveur NPS (le serveur source), y compris les clés de Registre et le magasin de configuration NPS, sous forme de script Netsh.
- Importer la configuration vers un autre serveur NPS à l’aide d’un script Netsh et le fichier de configuration exportées à partir du serveur NPS source.

Vous pouvez exécuter ces commandes à partir de l’invite de commandes Windows Server 2016 ou à partir de Windows PowerShell. Vous pouvez également exécuter les commandes netsh nps dans les scripts et fichiers de commandes.

**Informations d’identification d’administration** 

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur l’ordinateur local.

### <a name="to-enter-the-netsh-nps-context-on-an-nps-server"></a>Pour entrer dans le contexte Netsh NPS sur un serveur NPS

1. Ouvrez une invite de commandes ou Windows PowerShell.
2. Type **netsh**, puis appuyez sur ENTRÉE.
3. Type **nps**, puis appuyez sur ENTRÉE.
4. Pour afficher la liste des commandes disponibles, tapez un point d’interrogation \(?\) et appuyez sur ENTRÉE.


Pour plus d’informations sur les commandes Netsh NPS, voir [commandes Netsh pour NPS dans Windows Server 2008](https://technet.microsoft.com/library/cc754428(v=ws.10).aspx), ou télécharger l’intégralité de la [référence technique de Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0) à partir de la Galerie TechNet. Ce téléchargement est le complètes réseau Shell technique référence pour Windows Server 2008 et Windows Server 2008 R2. Le format est l’aide de Windows de \(*.chm\) dans un fichier zip. Ces commandes sont toujours présents dans Windows Server 2016 et Windows 10, vous pouvez donc utiliser netsh dans ces environnements, bien que l’utilisation de Windows PowerShell est recommandée.

## <a name="use-windows-powershell-to-manage-nps-servers"></a>Utiliser Windows PowerShell pour gérer les serveurs NPS

Vous pouvez utiliser les commandes Windows PowerShell pour gérer les serveurs NPS. Pour plus d’informations, consultez les rubriques de référence de commande Windows PowerShell suivantes.

- [Applets de commande stratégie serveur (NPS) dans Windows PowerShell réseau](https://technet.microsoft.com/library/jj872739(v=wps.630).aspx). Vous pouvez utiliser ces commandes netsh dans Windows Server 2012 R2 ou des systèmes d’exploitation ultérieurs.
- [Module NPS](https://technet.microsoft.com/itpro/powershell/windows/nps/index). Vous pouvez utiliser ces commandes netsh dans Windows Server 2016.

Pour plus d’informations sur l’administration de serveur NPS, voir [gérer serveur NPS (Network Policy)](nps-manage-top.md).
