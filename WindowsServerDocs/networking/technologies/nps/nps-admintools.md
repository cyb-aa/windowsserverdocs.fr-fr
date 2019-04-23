---
title: Gestion des serveurs NPS avec les Outils d'administration
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur les outils que vous pouvez utiliser pour gérer le serveur NPS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fa1767e49b16f4a55f36e052d4354aaead540f26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856880"
---
# <a name="network-policy-server-management-with-administration-tools"></a>Gestion des serveurs NPS avec les Outils d'administration

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur les outils que vous pouvez utiliser pour gérer votre NPSs.

Après avoir installé le serveur NPS, vous pouvez administrer NPSs :

- Localement, à l’aide de la Console de gestion Microsoft NPS \(MMC\) enfichable, de la console NPS statique dans les outils d’administration, les commandes Windows PowerShell ou l’interpréteur de commandes réseau \(Netsh\) commandes d’un serveur NPS.
- À partir d’un serveur NPS à distance, en utilisant le composant logiciel enfichable MMC NPS, les commandes Netsh pour NPS, les commandes Windows PowerShell pour NPS ou connexion Bureau à distance.
- À partir d’une station de travail distante, à l’aide de la connexion Bureau à distance en combinaison avec d’autres outils, tels que la MMC NPS ou Windows PowerShell.

>[!NOTE]
>Dans Windows Server 2016, vous pouvez gérer le serveur NPS local à l’aide de la console NPS. Pour gérer les NPSs locaux et distants, vous devez utiliser le composant logiciel enfichable MMC NPS\-dans.

Les sections suivantes fournissent des instructions sur la façon de gérer votre NPSs locaux et distants.

## <a name="configure-the-local-nps-by-using-the-nps-console"></a>Configurer le serveur NPS Local à l’aide de la Console NPS

Une fois que vous avez installé le serveur NPS, vous pouvez utiliser cette procédure pour gérer le serveur NPS local à l’aide de la console MMC NPS.

**Informations d’identification administratives** 

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

### <a name="to-configure-the-local-nps-by-using-the-nps-console"></a>Pour configurer le serveur NPS local à l’aide de la console NPS

1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Network Policy Server**. La console NPS s’ouvre.

2. Dans la console NPS, cliquez sur NPS \(Local\). Dans le volet détails, choisissez **Configuration Standard** ou **Configuration avancée**, puis effectuez une des opérations suivantes en fonction de votre sélection :
    - Si vous choisissez **Configuration Standard**, sélectionnez un scénario dans la liste, puis suivez les instructions pour démarrer un Assistant de configuration.
    - Si vous choisissez **Advanced Configuration**, cliquez sur la flèche pour développer **les options de Configuration avancée**, puis passez en revue et de configurer les options disponibles, basées sur la fonctionnalité de serveur NPS de votre choix : Serveur RADIUS, proxy RADIUS ou les deux.

## <a name="manage-multiple-npss-by-using-the-nps-mmc-snap-in"></a>Gérer plusieurs NPSs à l’aide du composant logiciel enfichable MMC NPS\-dans

Vous pouvez utiliser cette procédure pour gérer le serveur NPS local et plusieurs NPSs à distance à l’aide du composant logiciel enfichable MMC NPS\-dans.

Avant d’effectuer la procédure ci-dessous, vous devez installer NPS sur l’ordinateur local et sur des ordinateurs distants.

En fonction des conditions du réseau et le nombre de NPSs que vous gérez à l’aide du composant logiciel enfichable MMC NPS\-dans la réponse du composant logiciel enfichable MMC\-dans peut être lente. En outre, le trafic de configuration de serveur NPS est envoyé sur le réseau pendant une session d’administration à distance à l’aide du composant logiciel enfichable NPS\-dans. Assurez-vous que votre réseau est physiquement sécurisé et que les utilisateurs malveillants n’ont pas accès à ce trafic réseau.

**Informations d’identification administratives** 

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

### <a name="to-manage-multiple-npss-by-using-the-nps-snap-in"></a>Pour gérer plusieurs NPSs à l’aide du composant logiciel enfichable NPS\-dans

1. Pour ouvrir la console MMC, exécutez Windows PowerShell en tant qu’administrateur. Dans Windows PowerShell, tapez **mmc**, puis appuyez sur ENTRÉE. La console Microsoft Management Console s’ouvre.
2. Dans la console MMC, sur le **fichier** menu, cliquez sur **ajouter/supprimer un composant logiciel\-dans**. Le **ajouter ou supprimer\-ins** boîte de dialogue s’ouvre.
3. Dans **ajouter ou supprimer\-ins**, dans **composant logiciel enfichable disponible\-ins**, faites défiler la liste, cliquez sur **Network Policy Server**, puis cliquez sur  **Ajouter**. La boîte de dialogue **Sélectionner un ordinateur** s'affiche.
4. Dans **sélectionner un ordinateur**, vérifiez que **ordinateur Local \(l’ordinateur sur lequel cette console s’exécute\)**  est sélectionnée, puis cliquez sur **OK**. Le composant logiciel enfichable\-dans pour le serveur NPS local est ajoutée à la liste dans **sélectionné le composant logiciel enfichable\-ins**.
5. Dans **ajouter ou supprimer\-ins**, dans **composant logiciel enfichable disponible\-ins**, vérifiez que **Network Policy Server** est encore sélectionné, puis cliquez sur  **Ajouter**. Le **sélectionner un ordinateur** boîte de dialogue s’ouvre à nouveau.
6. Dans **sélectionner un ordinateur**, cliquez sur **un autre ordinateur**, puis tapez l’adresse IP ou le nom de domaine complet \(FQDN\) de serveur NPS à distance que vous souhaitez gérer à l’aide du serveur NPS Aligner\-dans. Si vous le souhaitez, vous pouvez cliquer sur **Parcourir** consulter le répertoire de l’ordinateur que vous souhaitez ajouter. Cliquez sur **OK**.
7. Répétez les étapes 5 et 6 pour ajouter plus de NPSs pour le composant logiciel enfichable NPS\-dans. Lorsque vous avez ajouté tous les NPSs que vous souhaitez gérer, cliquez sur **OK**.
8. Pour enregistrer le composant logiciel enfichable NPS pour une utilisation ultérieure, cliquez sur **fichier**, puis cliquez sur **enregistrer**. Dans le **enregistrer en tant que** boîte de dialogue, accédez à l’emplacement de disque dur où vous souhaitez enregistrer le fichier, tapez un nom pour votre Console de gestion Microsoft \(.msc\) de fichiers, puis cliquez sur **enregistrer**. 

## <a name="manage-an-nps-by-using-remote-desktop-connection"></a>Gérer un serveur NPS à l’aide de connexion Bureau à distance

Vous pouvez utiliser cette procédure pour gérer un serveur NPS à distance à l’aide de connexion Bureau à distance.

À l’aide de connexion Bureau à distance, vous pouvez gérer à distance votre NPSs exécutant Windows Server 2016. Vous pouvez aussi à distance gérer NPSs à partir d’un ordinateur exécutant Windows 10 ou les systèmes d’exploitation client Windows.

Vous pouvez utiliser Connexion Bureau à distance pour gérer plusieurs NPSs en utilisant l’une des deux méthodes.

1. Créer une connexion Bureau à distance pour chacun de vos NPSs individuellement.
2. Utilisez le Bureau à distance pour se connecter à un serveur NPS, puis utilisez la console MMC NPS sur ce serveur pour gérer d’autres serveurs distants. Pour plus d’informations, consultez la section précédente **gérer plusieurs NPSs à l’aide du composant logiciel enfichable MMC NPS\-dans**.

**Informations d’identification administratives** 

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur le serveur NPS.

### <a name="to-manage-an-nps-by-using-remote-desktop-connection"></a>Pour gérer un serveur NPS à l’aide de connexion Bureau à distance

1. Sur chaque serveur NPS que vous souhaitez gérer à distance, dans le Gestionnaire de serveur, sélectionnez **serveur Local**. Dans le volet de détails du Gestionnaire de serveur, consultez le **Bureau à distance** paramètre et effectuez l’une des opérations suivantes. 
    1. Si la valeur de la **Bureau à distance** paramètre est **activé**, vous n’avez pas besoin d’effectuer certaines des étapes dans cette procédure. Passez à étape 4 pour démarrer la configuration des autorisations de l’utilisateur du Bureau à distance.
    2. Si le **Bureau à distance** paramètre est **désactivé**, cliquez sur le mot **désactivé**. Le **propriétés système** boîte de dialogue s’ouvre sur la **distant** onglet.
2. Dans **Bureau à distance**, cliquez sur **autoriser les connexions à distance à cet ordinateur**. Le **connexion Bureau à distance** boîte de dialogue s’ouvre. Effectuez l’une des opérations suivantes :
    1. Pour personnaliser les connexions réseau qui sont autorisées, cliquez sur **pare-feu Windows avec fonctions avancées de sécurité**, puis configurez les paramètres que vous souhaitez autoriser. 
    2. Pour activer la connexion Bureau à distance pour toutes les connexions sur l’ordinateur réseau, cliquez sur **OK**.
3. Dans **propriétés système**, dans **Bureau à distance**, décidez s’il faut activer **autoriser les connexions uniquement sur les ordinateurs exécutant Bureau à distance avec authentification NLA**et effectuez votre sélection.
4. Cliquez sur **Sélectionner des utilisateurs**. Le **Remote Desktop Users** boîte de dialogue s’ouvre.
5. Dans **Remote Desktop Users**, pour accorder une autorisation à un utilisateur pour se connecter à distance au serveur NPS, cliquez sur **ajouter**, puis tapez le nom d’utilisateur pour le compte d’utilisateur. Cliquez sur **OK**.
6. Répétez l’étape 5 pour chaque utilisateur pour lequel vous souhaitez accorder l’autorisation d’accès à distance au serveur NPS. Lorsque vous avez terminé l’ajout d’utilisateurs, cliquez sur **OK** pour fermer la **Remote Desktop Users** boîte de dialogue et **OK** à nouveau pour fermer la **propriétés système**boîte de dialogue.
7. Pour vous connecter à un serveur NPS à distance que vous avez configuré à l’aide des étapes précédentes, cliquez sur **Démarrer**, faites défiler la liste alphabétique, puis sur **accessoires de Windows**, puis cliquez sur **à distance Connexion Bureau**. Le **connexion Bureau à distance** boîte de dialogue s’ouvre.
8. Dans le **connexion Bureau à distance** boîte de dialogue **ordinateur**, tapez l’adresse IP ou le nom du serveur NPS. Si vous préférez, cliquez sur **Options**, configurer les options de connexion supplémentaires, puis cliquez sur **enregistrer** pour enregistrer la connexion pour une utilisation répétée.
9. Cliquez sur **Connect**et lorsque vous y êtes invité fournissent des informations d’identification du compte utilisateur d’un compte qui dispose des autorisations pour ouvrir une session et de configurer le serveur NPS.

## <a name="use-netsh-nps-commands-to-manage-an-nps"></a>Utilisez les commandes Netsh NPS pour gérer un serveur NPS

Vous pouvez utiliser des commandes dans le contexte Netsh NPS pour afficher et définir la configuration de l’authentification, autorisation, gestion des comptes et l’audit de base de données utilisée par NPS et le service d’accès à distance. Utilisez les commandes dans le contexte Netsh NPS pour :

- Configurer ou reconfigurer un serveur NPS, y compris tous les aspects de NPS qui sont également disponibles pour la configuration à l’aide de la console NPS dans l’interface Windows.
- Exporter la configuration d’un serveur NPS (le serveur source), y compris les clés de Registre et la configuration NPS stocker, sous forme de script Netsh.
- Importer la configuration dans un autre serveur NPS à l’aide d’un script de Netsh et le fichier de configuration exporté à partir de la source de serveur NPS.

Vous pouvez exécuter ces commandes à partir de l’invite de commandes Windows Server 2016 ou à partir de Windows PowerShell. Vous pouvez également exécuter les commandes netsh nps dans des scripts et fichiers de commandes.

**Informations d’identification administratives** 

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur l'ordinateur local.

### <a name="to-enter-the-netsh-nps-context-on-an-nps"></a>Pour entrer dans le contexte Netsh NPS sur un serveur NPS

1. Ouvrez une invite de commandes ou Windows PowerShell.
2. Type **netsh**, puis appuyez sur ENTRÉE.
3. Type **nps**, puis appuyez sur ENTRÉE.
4. Pour afficher une liste des commandes disponibles, tapez un point d’interrogation \(?\) et appuyez sur ENTRÉE.


Pour plus d’informations sur les commandes Netsh NPS, consultez [commandes Netsh pour NPS dans Windows Server 2008](https://technet.microsoft.com/library/cc754428(v=ws.10).aspx), ou télécharger l’intégralité de [référence technique de Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0) à partir de la Galerie TechNet. Ce téléchargement est la complètes réseau Shell technique référence pour Windows Server 2008 et Windows Server 2008 R2. Le format est à l’aide de Windows \(*.chm\) dans un fichier zip. Ces commandes sont toujours présentes dans Windows Server 2016 et Windows 10, vous pouvez donc utiliser netsh dans ces environnements, bien que l’utilisation de Windows PowerShell est recommandée.

## <a name="use-windows-powershell-to-manage-npss"></a>Utiliser Windows PowerShell pour gérer NPSs

Vous pouvez utiliser les commandes Windows PowerShell pour gérer les NPSs. Pour plus d’informations, consultez les rubriques de référence de commande Windows PowerShell suivantes.

- [Applets de commande stratégie Server (NPS) dans Windows PowerShell réseau](https://technet.microsoft.com/library/jj872739(v=wps.630).aspx). Vous pouvez utiliser ces commandes netsh dans Windows Server 2012 R2 ou des systèmes d’exploitation ultérieurs.
- [Module NPS](https://technet.microsoft.com/itpro/powershell/windows/nps/index). Vous pouvez utiliser ces commandes netsh dans Windows Server 2016.

Pour plus d’informations sur l’administration de serveur NPS, consultez [gérer serveur NPS (Network Policy)](nps-manage-top.md).
