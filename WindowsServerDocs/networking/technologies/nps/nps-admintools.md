---
title: Gestion des serveurs NPS avec les Outils d'administration
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur les outils que vous pouvez utiliser pour gérer le serveur de stratégie réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 527fbf52d68f36d198068514476868bcba930a68
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396451"
---
# <a name="network-policy-server-management-with-administration-tools"></a>Gestion des serveurs NPS avec les Outils d'administration

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur les outils que vous pouvez utiliser pour gérer vos NPSs.

Après avoir installé NPS, vous pouvez administrer NPSs :

- Localement, en utilisant le composant logiciel enfichable MMC de la console de gestion Microsoft \(\) MMC, la console NPS statique dans outils d’administration, commandes Windows PowerShell ou l’interpréteur de commandes réseau \(les commandes netsh\) pour NPS.
- À partir d’un serveur NPS distant, en utilisant le composant logiciel enfichable MMC NPS, les commandes netsh pour NPS, les commandes Windows PowerShell pour NPS ou Connexion Bureau à distance.
- À partir d’une station de travail distante, en utilisant Connexion Bureau à distance conjointement avec d’autres outils, tels que le MMC NPS ou Windows PowerShell.

>[!NOTE]
>Dans Windows Server 2016, vous pouvez gérer le serveur NPS local à l’aide de la console NPS. Pour gérer à la fois les NPSs distants et locaux, vous devez utiliser le composant logiciel enfichable MMC NPS\-dans.

Les sections suivantes fournissent des instructions sur la gestion de vos NPSs locaux et distants.

## <a name="configure-the-local-nps-by-using-the-nps-console"></a>Configurer le serveur NPS local à l’aide de la console NPS

Une fois que vous avez installé NPS, vous pouvez utiliser cette procédure pour gérer le serveur NPS local à l’aide de la console MMC NPS.

**Informations d’identification d’administration** 

Pour effectuer cette procédure, vous devez être membre du groupe administrateurs.

### <a name="to-configure-the-local-nps-by-using-the-nps-console"></a>Pour configurer le serveur NPS local à l’aide de la console NPS

1. Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur **serveur NPS (Network Policy Server**). La console NPS s’ouvre.

2. Dans la console NPS, cliquez sur serveur NPS \(\)local. Dans le volet d’informations, choisissez **configuration standard** ou **Configuration avancée**, puis effectuez l’une des opérations suivantes en fonction de votre sélection :
    - Si vous choisissez **configuration standard**, sélectionnez un scénario dans la liste, puis suivez les instructions pour démarrer un Assistant Configuration.
    - Si vous choisissez **Configuration avancée**, cliquez sur la flèche pour développer les **options de configuration avancées**, puis examinez et configurez les options disponibles en fonction de la fonctionnalité NPS souhaitée : serveur RADIUS, proxy RADIUS, ou les deux.

## <a name="manage-multiple-npss-by-using-the-nps-mmc-snap-in"></a>Gérer plusieurs NPSs à l’aide du composant logiciel enfichable MMC NPS\-dans

Vous pouvez utiliser cette procédure pour gérer le serveur NPS local et plusieurs NPSs distants à l’aide du composant logiciel enfichable MMC NPS\-dans.

Avant d’effectuer la procédure ci-dessous, vous devez installer le serveur NPS sur l’ordinateur local et sur des ordinateurs distants.

En fonction des conditions du réseau et du nombre de NPSs que vous gérez à l’aide du composant logiciel enfichable MMC NPS\-dans, la réponse de la\-du composant logiciel enfichable MMC dans peut être lente. En outre, le trafic de configuration NPS est envoyé sur le réseau au cours d’une session d’administration à distance à l’aide de la\-du composant logiciel enfichable NPS dans. Assurez-vous que votre réseau est physiquement sécurisé et que les utilisateurs malveillants n’ont pas accès à ce trafic réseau.

**Informations d’identification d’administration** 

Pour effectuer cette procédure, vous devez être membre du groupe administrateurs.

### <a name="to-manage-multiple-npss-by-using-the-nps-snap-in"></a>Pour gérer plusieurs NPSs à l’aide de la\-du composant logiciel enfichable NPS dans

1. Pour ouvrir la console MMC, exécutez Windows PowerShell en tant qu’administrateur. Dans Windows PowerShell, tapez **MMC**, puis appuyez sur entrée. La console Microsoft Management Console s’ouvre.
2. Dans la console MMC, dans le menu **fichier** , cliquez sur **Ajouter/supprimer un composant logiciel enfichable\-dans**. La boîte de dialogue **Ajouter ou supprimer un composant logiciel enfichable\-** s’ouvre.
3. Dans **Ajouter ou supprimer un composant logiciel enfichable\-ins**, dans les **\-enfichables disponibles**, faites défiler la liste, cliquez sur **serveur NPS (Network Policy Server**), puis cliquez sur **Ajouter**. La boîte de dialogue **Sélectionner un ordinateur** s'affiche.
4. Dans **Sélectionner un ordinateur**, vérifiez que **l’ordinateur local \(l’ordinateur sur lequel cette console s’exécute\)** est sélectionné, puis cliquez sur **OK**. Le\-d’alignement de pour le serveur NPS local est ajouté à la liste dans les **\-d’accrochage sélectionnés**.
5. Dans **Ajouter ou supprimer un composant logiciel enfichable\-ins**, dans composants **logiciels enfichables\-disponibles**, assurez-vous que le **serveur NPS** est toujours sélectionné, puis cliquez sur **Ajouter**. La boîte de dialogue **Sélectionner un ordinateur** s’ouvre à nouveau.
6. Dans **Sélectionner un ordinateur**, cliquez sur **un autre ordinateur**, puis tapez l’adresse IP ou le nom de domaine complet \(\) de noms de domaine complet du serveur NPS distant que vous souhaitez gérer à l’aide du composant logiciel enfichable NPS\-dans. Si vous le souhaitez, vous pouvez cliquer sur **Parcourir pour consulter** le répertoire de l’ordinateur que vous souhaitez ajouter. Cliquez sur **OK**.
7. Répétez les étapes 5 et 6 pour ajouter d’autres NPSs à la\-du composant logiciel enfichable NPS dans. Une fois que vous avez ajouté tous les NPSs que vous souhaitez gérer, cliquez sur **OK**.
8. Pour enregistrer le composant logiciel enfichable NPS en vue d’une utilisation ultérieure, cliquez sur **fichier**, puis sur **Enregistrer**. Dans la boîte de dialogue **Enregistrer sous** , accédez à l’emplacement du disque dur où vous souhaitez enregistrer le fichier, tapez un nom pour le fichier de\) de votre console de gestion Microsoft \(. msc, puis cliquez sur **Enregistrer**. 

## <a name="manage-an-nps-by-using-remote-desktop-connection"></a>Gérer un serveur NPS à l’aide de Connexion Bureau à distance

Vous pouvez utiliser cette procédure pour gérer un serveur NPS distant à l’aide de Connexion Bureau à distance.

À l’aide de Connexion Bureau à distance, vous pouvez gérer à distance votre NPSs exécutant Windows Server 2016. Vous pouvez également gérer NPSs à distance à partir d’un ordinateur exécutant Windows 10 ou des systèmes d’exploitation clients Windows antérieurs.

Vous pouvez utiliser Bureau à distance connexion pour gérer plusieurs NPSs à l’aide de l’une des deux méthodes.

1. Créez une connexion Bureau à distance à chacun de vos NPSs individuellement.
2. Utilisez Bureau à distance pour vous connecter à un serveur NPS, puis utilisez la console MMC NPS sur ce serveur pour gérer d’autres serveurs distants. Pour plus d’informations, consultez la section précédente **gérer plusieurs NPSs à l’aide du composant logiciel enfichable MMC NPS\-dans**.

**Informations d’identification d’administration** 

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur le serveur NPS.

### <a name="to-manage-an-nps-by-using-remote-desktop-connection"></a>Pour gérer un serveur NPS à l’aide de Connexion Bureau à distance

1. Sur chaque serveur NPS que vous souhaitez gérer à distance, dans Gestionnaire de serveur, sélectionnez **serveur local**. Dans le volet d’informations Gestionnaire de serveur, affichez le paramètre **Bureau à distance** , puis effectuez l’une des opérations suivantes. 
    1. Si la valeur du paramètre **Bureau à distance** est **activée**, vous n’avez pas besoin d’effectuer certaines des étapes de cette procédure. Passez à l’étape 4 pour commencer à configurer des Bureau à distance des autorisations utilisateur.
    2. Si le paramètre **Bureau à distance** est **désactivé**, cliquez sur le mot **désactivé**. La boîte de dialogue **Propriétés système** s’ouvre sous l’onglet **à distance** .
2. Dans **Bureau à distance**, cliquez sur **autoriser les connexions à distance à cet ordinateur**. La boîte de dialogue **Connexion Bureau à distance** s’ouvre. Effectuez l’une des opérations suivantes :
    1. Pour personnaliser les connexions réseau autorisées, cliquez sur **pare-feu Windows avec fonctions avancées de sécurité**, puis configurez les paramètres que vous souhaitez autoriser. 
    2. Pour activer Connexion Bureau à distance pour toutes les connexions réseau sur l’ordinateur, cliquez sur **OK**.
3. Dans **Propriétés système**, dans **Bureau à distance**, indiquez si vous souhaitez activer **autoriser les connexions uniquement à partir d’ordinateurs exécutant Bureau à distance avec authentification au niveau du réseau**, puis effectuez votre sélection.
4. Cliquez sur **Sélectionner des utilisateurs**. La boîte de dialogue **Bureau à distance utilisateurs** s’ouvre.
5. Dans **Bureau à distance utilisateurs**, pour accorder à un utilisateur l’autorisation de se connecter à distance au serveur NPS, cliquez sur **Ajouter**, puis tapez le nom d’utilisateur du compte de l’utilisateur. Cliquez sur **OK**.
6. Répétez l’étape 5 pour chaque utilisateur auquel vous souhaitez accorder une autorisation d’accès à distance au serveur NPS. Lorsque vous avez terminé d’ajouter des utilisateurs, cliquez sur **OK** pour fermer la boîte de dialogue **Bureau à distance des utilisateurs** , puis de nouveau sur **OK** pour fermer la boîte de dialogue **Propriétés système** .
7. Pour vous connecter à un serveur NPS distant que vous avez configuré à l’aide des étapes précédentes, cliquez sur **Démarrer**, faites défiler la liste alphabétique, cliquez sur **accessoires Windows**, puis cliquez sur **Connexion Bureau à distance**. La boîte de dialogue **Connexion Bureau à distance** s’ouvre.
8. Dans la boîte de dialogue **Connexion Bureau à distance** , dans la zone **ordinateur**, tapez le nom ou l’adresse IP du serveur NPS. Si vous préférez, cliquez sur **options**, configurer des options de connexion supplémentaires, puis cliquez sur **Enregistrer** pour enregistrer la connexion en vue d’une utilisation répétée.
9. Cliquez sur **se connecter**et, lorsque vous y êtes invité, fournissez les informations d’identification du compte d’utilisateur pour un compte qui dispose des autorisations pour se connecter à et configurer le serveur NPS.

## <a name="use-netsh-nps-commands-to-manage-an-nps"></a>Utiliser les commandes netsh NPS pour gérer un serveur NPS

Vous pouvez utiliser les commandes du contexte netsh NPS pour afficher et définir la configuration de la base de données d’authentification, d’autorisation, de gestion des comptes et d’audit utilisée à la fois par NPS et par le service d’accès à distance. Utilisez les commandes du contexte netsh NPS pour effectuer les opérations suivantes :

- Configurez ou reconfigurez un serveur NPS, y compris tous les aspects de NPS, qui sont également disponibles pour la configuration à l’aide de la console NPS dans l’interface Windows.
- Exportez la configuration d’un serveur NPS (le serveur source), y compris les clés de Registre et le magasin de configuration NPS, sous la forme d’un script netsh.
- Importez la configuration vers un autre serveur NPS en utilisant un script netsh et le fichier de configuration exporté à partir du serveur NPS source.

Vous pouvez exécuter ces commandes à partir de l’invite de commandes Windows Server 2016 ou de Windows PowerShell. Vous pouvez également exécuter des commandes netsh nps dans des scripts et des fichiers de commandes.

**Informations d’identification d’administration** 

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur l'ordinateur local.

### <a name="to-enter-the-netsh-nps-context-on-an-nps"></a>Pour entrer dans le contexte netsh NPS sur un serveur NPS

1. Ouvrez l’invite de commandes ou Windows PowerShell.
2. Tapez **netsh**, puis appuyez sur entrée.
3. Tapez **NPS**, puis appuyez sur entrée.
4. Pour afficher la liste des commandes disponibles, tapez un point d’interrogation \(?\) et appuyez sur entrée.


Pour plus d’informations sur les commandes netsh NPS, consultez [commandes netsh pour le serveur NPS (Network Policy Server) dans Windows Server 2008](https://technet.microsoft.com/library/cc754428(v=ws.10).aspx)ou téléchargez la totalité de la [référence technique netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0) à partir de la Galerie technet. Ce téléchargement est la référence technique complète de Network Shell pour Windows Server 2008 et Windows Server 2008 R2. Le format est Windows Help \(*. chm\) dans un fichier zip. Ces commandes étant toujours présentes dans Windows Server 2016 et Windows 10, vous pouvez utiliser Netsh dans ces environnements, bien que l’utilisation de Windows PowerShell soit recommandée.

## <a name="use-windows-powershell-to-manage-npss"></a>Utiliser Windows PowerShell pour gérer NPSs

Vous pouvez utiliser les commandes Windows PowerShell pour gérer NPSs. Pour plus d’informations, consultez les rubriques de référence sur les commandes Windows PowerShell suivantes.

- [Applets de commande du serveur NPS (Network Policy Server) dans Windows PowerShell](https://technet.microsoft.com/library/jj872739(v=wps.630).aspx). Vous pouvez utiliser ces commandes netsh dans les systèmes d’exploitation Windows Server 2012 R2 ou version ultérieure.
- [Module NPS](https://technet.microsoft.com/itpro/powershell/windows/nps/index). Vous pouvez utiliser ces commandes netsh dans Windows Server 2016.

Pour plus d’informations sur l’administration NPS, consultez [gérer le serveur NPS (Network Policy Server)](nps-manage-top.md).
