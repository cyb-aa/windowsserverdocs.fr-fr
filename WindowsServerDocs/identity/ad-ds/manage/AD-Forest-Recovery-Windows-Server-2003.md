---
title: Récupération de la forêt Active Directory-récupération de Windows Server 2003
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 43a2034cb707d4333abdce5f5b2b09d6c4b5a33a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390062"
---
# <a name="ad-forest-recovery---windows-server-2003-recovery"></a>Récupération de la forêt Active Directory-récupération de Windows Server 2003

>S’applique à : Windows Server 2003

Cette rubrique comprend des procédures de récupération de forêt pour les contrôleurs de domaine qui exécutent Windows Server 2003. Le processus général de récupération de forêt n’est pas différent avec les contrôleurs de Windows Server 2003, mais des procédures spécifiques peuvent différer en raison de différents outils. Par exemple, Ntdsutil. exe peut être utilisé pour sauvegarder et restaurer des contrôleurs de contrôle qui exécutent des contrôleurs de Windows Server 2003, alors que Sauvegarde Windows Server ou Wbadmin. exe est utilisé pour les contrôleurs de contrôle qui exécutent Windows Server 2008 ou version ultérieure.  
  
- [Sauvegarde des données d’État du système](#backing-up-the-system-state-data)  
- [Exécution d’une restauration ne faisant pas autorité](#performing-a-nonauthoritative-restore)  
- [Installer et configurer le service serveur DNS](#install-and-configure-the-dns-server-service)

## <a name="backing-up-the-system-state-data"></a>Sauvegarde des données d’État du système
Utilisez la procédure suivante pour sauvegarder les données d’État du système, ainsi que toutes les autres données que vous avez sélectionnées pour l’opération de sauvegarde actuelle, d’un contrôleur de réseau qui exécute Windows Server 2003. Windows Server 2003 comprend l’outil NTBackup, que vous pouvez utiliser pour sauvegarder les données d’État du système.  
  
L’appartenance au groupe **administrateurs** ou **opérateurs de sauvegarde**, ou équivalent, est la condition minimale requise pour sauvegarder des fichiers et des dossiers.   
  
Si vous sauvegardez les données d’État du système sur une bande et que le programme de sauvegarde indique qu’aucun média inutilisé n’est disponible, vous devrez peut-être utiliser le stockage amovible. Vous ajoutez ainsi votre bande au pool de supports libre afin que la sauvegarde puisse l’utiliser.  
  
Vous pouvez uniquement sauvegarder les données d’État du système sur un ordinateur local. Vous ne pouvez pas le sauvegarder sur un ordinateur distant.  
  
### <a name="to-back-up-the-system-state-data-on-a-domain-controller-that-runs-windows-server-2003"></a>Pour sauvegarder les données d’État du système sur un contrôleur de domaine qui exécute Windows Server 2003  
  
1. Cliquez sur **Démarrer**, pointez sur **tous les programmes**, sur **accessoires**, sur **Outils système**, puis cliquez sur **sauvegarde**.  
2. Dans la page **Bienvenue** , cliquez sur **mode avancé**.  
3. Sous l’onglet **sauvegarde** , activez la case à cocher de tous les lecteurs, dossiers ou fichiers que vous souhaitez sauvegarder.  
4. Activez la case à cocher **État du système** .  
5. Cliquez sur **Démarrer la sauvegarde**.  
  
## <a name="performing-a-nonauthoritative-restore"></a>Exécution d’une restauration ne faisant pas autorité  

Utilisez la procédure suivante pour effectuer une restauration ne faisant pas autorité d’un contrôleur de périphérique qui exécute Windows Server 2003. En effectuant une restauration ne faisant pas autorité sur Active Directory dans Windows Server 2003, vous effectuez automatiquement une restauration ne faisant pas autorité de SYSVOL. Aucune étape supplémentaire n’est requise.  
  
> [!NOTE]
> Si vous réinstallez également le système d’exploitation Windows Server 2003, vous pouvez ou non joindre l’ordinateur au domaine et vous pouvez attribuer n’importe quel nom à l’ordinateur lors de l’installation du système d’exploitation. N’installez pas Active Directory. Après avoir réinstallé le système d’exploitation, passez directement à l’étape 4.  
  
Sur les contrôleurs de domaine Windows Server 2003 où vous avez restauré uniquement les données d’État du système, vous devez également réinstaller toutes les applications logicielles qui étaient exécutées sur les contrôleurs de domaine avant la récupération. La restauration de AD DS sur le premier contrôleur de domaine dans le domaine restaure également le registre car ils font tous les deux partie des données d’État du système. Gardez cela à l’esprit si vous aviez des applications qui s’exécutent sur ces contrôleurs de service et si elles avaient des informations stockées dans le registre.  
  
Pour gagner du temps pour réinstaller le logiciel, déterminez si les applications qui doivent être installées sur les contrôleurs de l’ordinateur sont compatibles avec le clonage de contrôleur de contrôleur virtuel. De telles applications peuvent être installées sur le contrôleur de périphérique source avant le clonage afin de gagner du temps et de l’effort requis pour les installer sur les contrôleurs de l’ordinateur virtuel clonés.  
  
### <a name="to-perform-a-nonauthoritative-restore"></a>Pour effectuer une restauration ne faisant pas autorité
  
1. Après avoir démarré le DC, appuyez sur F8 pour redémarrer l’ordinateur en mode de restauration des services d’annuaire (DSRM).  
2. Sélectionnez le **mode de restauration des services d’annuaire (contrôleurs de domaine Windows uniquement)** .  
3. Sélectionnez le système d’exploitation que vous souhaitez démarrer en mode de restauration.  
4. Ouvrir une session en tant qu’administrateur (vous ne pouvez utiliser qu’un compte d’ordinateur local, aucune option d’ouverture de session de domaine n’est disponible).  
5. À l’invite de commandes, tapez **ntbackup**, puis appuyez sur entrée.  
6. Dans la page **Bienvenue** , cliquez sur **mode avancé**, puis sélectionnez l’onglet **restaurer et gérer le média** . (ne sélectionnez pas **Assistant Restauration**).  
7. Sélectionnez le fichier de sauvegarde approprié à partir duquel effectuer la restauration et assurez-vous que les cases à cocher **disque système** et **État du système** sont activées.  
8. Cliquez sur **Démarrer la restauration**.  
9. Une fois l’opération de restauration terminée, redémarrez l’ordinateur.  
  
Utilisez la procédure suivante pour effectuer une restauration faisant autorité (également appelée principale) de SYSVOL sur un contrôleur de périphérique qui exécute Windows Server 2003. Effectuez cette procédure uniquement sur le premier contrôleur de domaine Windows Server 2003 restauré dans le domaine.  
  
### <a name="to-perform-an-authoritative-restore-of-sysvol"></a>Pour effectuer une restauration faisant autorité de SYSVOL  
  
1. Effectuez les étapes 1 à 8 de la procédure précédente.  
2. Dans la boîte de dialogue **confirmer la restauration** , cliquez sur **avancé**.  
3. Pour effectuer une restauration faisant autorité de SYSVOL, activez la case à cocher **lors de la restauration des jeux de données répliqués, marquez les données restaurées en tant que données principales pour tous les réplicas**.  

   > [!NOTE]
   > Le marquage des données restaurées comme données primaires dans la sauvegarde revient à définir l’entrée **BurFlags** sur D4 sous la sous-clé de Registre suivante :  
   >   
   > **HKEY_LOCAL_MACHINE les jeux de réplicas \system\currentcontrolset\services\ntfrs\parameters\cumulative\\** *GUID*  

4. Une fois l’opération de restauration terminée, redémarrez l’ordinateur.  
  
## <a name="install-and-configure-the-dns-server-service"></a>Installer et configurer le service serveur DNS

Si le contrôleur de domaine que vous avez restauré à partir d’une sauvegarde exécute Windows Server 2003, vous pouvez installer le serveur DNS sans connecter le contrôleur de domaine à un réseau.  
  
### <a name="to-install-and-configure-the-dns-server-service"></a>Pour installer et configurer le service serveur DNS  
  
1. Ouvrir l’Assistant composants de Windows. Pour ouvrir l’Assistant :  

   - Cliquez sur **Démarrer**, sur **Panneau de configuration**, puis sur **Ajout/Suppression de programmes**.  
   - Cliquez sur **Ajouter/supprimer des composants Windows**.  

2. Dans **composants**, activez la case à cocher **services de mise en réseau** , puis cliquez sur **Détails**.  
3. Dans **subcomponents of Networking Services**, activez la case à cocher **Domain Name System (DNS)** , cliquez sur **OK**, puis sur **suivant**.  
4. Si vous y êtes invité, dans **copier les fichiers à partir de**, tapez le chemin d’accès complet des fichiers de distribution, puis cliquez sur **OK**.  

   Après l’installation, procédez comme suit pour configurer le serveur DNS.  

5. Cliquez sur **Démarrer**, pointez sur **tous les programmes**, sur **Outils d’administration**, puis cliquez sur **DNS**.  
6. Créez des zones DNS pour les mêmes noms de domaine DNS hébergés sur les serveurs DNS avant le dysfonctionnement critique. Pour plus d’informations, consultez Ajouter une zone de recherche directe ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).  
7. Configurez les données DNS telles qu’elles existaient avant le dysfonctionnement critique. Par exemple :  

   - Configurez les zones DNS à stocker dans AD DS. Pour plus d’informations, consultez modifier le type de zone ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).  
   - Configurez la zone DNS faisant autorité pour les enregistrements de ressource localisateur de contrôleur de domaine pour autoriser la mise à jour dynamique sécurisée. Pour plus d’informations, consultez autoriser uniquement les mises à jour dynamiques sécurisées ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).  

8. Vérifiez que la zone DNS parent contient les enregistrements de ressource de délégation (NS) et les enregistrements de ressource d’hôte (A) de la zone enfant hébergée sur ce serveur DNS. Pour plus d’informations, consultez créer une délégation de zone ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).  
9. Après avoir configuré DNS, à l’invite de commandes, tapez la commande suivante, puis appuyez sur entrée :  

   **net stop Netlogon**

10. Tapez la commande suivante et appuyez sur ENTRÉE :  

    **NET START Netlogon**

    > [!NOTE]
    > Net Logon enregistre les enregistrements de ressource du localisateur de contrôleur de domaine dans DNS pour ce contrôleur de domaine. Si vous installez le service serveur DNS sur un serveur dans le domaine enfant, ce contrôleur de domaine ne pourra pas inscrire ses enregistrements immédiatement. Cela est dû au fait qu’il est actuellement isolé dans le cadre du processus de récupération et que son serveur DNS principal est le serveur DNS racine de la forêt. Configurez cet ordinateur avec la même adresse IP que celle qu’il avait avant l’incident pour éviter les échecs de recherche de service DC.

## <a name="next-steps"></a>Étapes suivantes

- [Récupération de la forêt Active Directory : conditions préalables](AD-Forest-Recovery-Prerequisties.md)  
- [Récupération de la forêt Active Directory-élaboration d’un plan de récupération de forêt personnalisé](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Récupération de la forêt Active Directory-identifier le problème](AD-Forest-Recovery-Identify-the-Problem.md)
- [Récupération de la forêt Active Directory-déterminer comment récupérer](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Récupération de la forêt Active Directory-effectuer la récupération initiale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)  
- [Récupération de la forêt Active Directory-Forum aux questions](AD-Forest-Recovery-FAQ.md)  
- [Récupération de la forêt Active Directory-récupération d’un domaine unique au sein d’une forêt à plusieurs domaines](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Récupération de forêt AD-récupération de forêt avec les contrôleurs de domaine Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
