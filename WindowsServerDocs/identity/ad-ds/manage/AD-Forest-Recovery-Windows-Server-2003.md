---
title: Récupération de forêt AD - récupération de Windows Server 2003
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: bd15df5360a50e417881d83319344dbdf48f35fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829640"
---
# <a name="ad-forest-recovery---windows-server-2003-recovery"></a>Récupération de forêt AD - récupération de Windows Server 2003

>S'applique à : Windows Server 2003

Cette rubrique inclut des procédures de récupération de forêt pour les contrôleurs de domaine (DC) qui exécutent Windows Server 2003. Le processus général de récupération de forêt n’est pas différent avec les contrôleurs de domaine Windows Server 2003, mais des procédures spécifiques peuvent varier en raison de différents outils. Par exemple, Ntdsutil.exe peut être utilisé pour sauvegarder et restaurer des contrôleurs de domaine qui exécutent des contrôleurs de domaine Windows Server 2003, tandis que la sauvegarde Windows Server ou Wbadmin.exe est utilisé pour les contrôleurs de domaine qui exécutent Windows Server 2008 ou version ultérieure.  
  
- [Sauvegarde des données d’état du système](#Backing-up-the-System-State-data)  
- [Effectuez une restauration ne faisant pas autoritée](#Performing-a-nonauthoritative restore)  
- [Installer et configurer le service serveur DNS](#Install-and-configure-the-DNS-Server-service)  

## <a name="backing-up-the-system-state-data"></a>Sauvegarde des données d’état du système
Utilisez la procédure suivante pour sauvegarder les données d’état du système, ainsi que toutes les autres données que vous avez sélectionné pour l’opération de sauvegarde actuelle, d’un contrôleur de domaine qui exécute Windows Server 2003. Windows Server 2003 inclut l’outil Ntbackup, qui vous permet de sauvegarder les données d’état du système.  
  
L’appartenance au **administrateurs** ou **opérateurs de sauvegarde**, ou équivalente, est la condition minimale requise pour sauvegarder des fichiers et dossiers.   
  
Si vous sauvegardez les données d’état du système sur une bande, et que le programme de sauvegarde indique qu’aucun média inutilisé n’est disponible, vous devrez peut-être utiliser un stockage amovible. Cela ajoute votre bande vers le pool libre afin que la sauvegarde puisse l’utiliser.  
  
Vous pouvez uniquement sauvegarder les données d’état du système sur un ordinateur local. Vous ne pouvez pas sauvegarder sur un ordinateur distant.  
  
### <a name="to-back-up-the-system-state-data-on-a-domain-controller-that-runs-windows-server-2003"></a>Pour sauvegarder les données d’état du système sur un contrôleur de domaine qui exécute Windows Server 2003  
  
1. Cliquez sur **Démarrer**, pointez sur **tous les programmes**, pointez sur **Accessoires**, pointez sur **Outils système**, puis cliquez sur **sauvegarde** .  
2. Sur le **Bienvenue** , cliquez sur **Mode avancé**.  
3. Sur le **sauvegarde** , sélectionnez la case à cocher pour n’importe quel lecteur, le dossier ou le fichier que vous souhaitez sauvegarder.  
4. Sélectionnez le **état du système** case à cocher.  
5. Cliquez sur **démarrer la sauvegarde**.  
  
## <a name="performing-a-nonauthoritative-restore"></a>Effectuez une restauration ne faisant pas autoritée  

Utilisez la procédure suivante pour effectuer une restauration ne faisant pas autoritée d’un contrôleur de domaine qui exécute Windows Server 2003. En effectuant une restauration ne faisant pas autoritée sur Active Directory dans Windows Server 2003, vous effectuer automatiquement une restauration ne faisant pas autoritée de SYSVOL. Aucune étape supplémentaire est requise.  
  
> [!NOTE]
> Si vous réinstallez également le système d’exploitation Windows Server 2003, vous pouvez ou ne pourrez pas joindre l’ordinateur au domaine et vous pouvez attribuer un nom à l’ordinateur pendant l’installation du système d’exploitation. N’installez pas Active Directory. Après la réinstallation du système d’exploitation, passez directement à l’étape 4.  
  
Sur les contrôleurs de domaine Windows Server 2003 où vous avez restauré les données d’état système uniquement, vous devez également réinstaller les applications de logiciel en cours d’exécution sur les contrôleurs de domaine avant récupération. Les services AD DS sur le premier contrôleur de domaine dans le domaine de restauration restaure également le Registre, car ils sont tous deux font partie des données d’état du système. Gardez cela à l’esprit si vous aviez toutes les applications en cours d’exécution sur ces contrôleurs de domaine et si elles avaient toutes les informations stockées dans le Registre.  
  
Pour gagner du temps requis pour réinstaller le logiciel, déterminez si les applications qui doivent être installés sur les contrôleurs de domaine sont compatibles avec le clonage de contrôleur de domaine virtuel. Ces applications peuvent être installées sur le contrôleur de domaine source avant le clonage afin d’enregistrer l’heure et l’effort requis pour les installer sur les contrôleurs de domaine virtuels clonés.  
  
### <a name="to-perform-a-nonauthoritative-restore"></a>Pour effectuer une restauration ne faisant pas autoritée
  
1. Une fois que vous démarrez le contrôleur de domaine, appuyez sur F8 pour redémarrer l’ordinateur en Mode restauration des Services d’annuaire (DSRM).  
2. Sélectionnez **Mode restauration des Services d’annuaire (contrôleurs de domaine Windows)**.  
3. Sélectionnez le système d’exploitation que vous souhaitez démarrer en mode de restauration.  
4. Ouvrez une session en tant qu’administrateur (vous pouvez uniquement utiliser un compte d’ordinateur local, aucune option d’ouverture de session de domaine est disponible).  
5. À l’invite de commandes, tapez **ntbackup**, puis appuyez sur ENTRÉE.  
6. Sur le **Bienvenue** , cliquez sur **Mode avancé**, puis sélectionnez le **restaurer et gérer le média** onglet. (Ne sélectionnez pas **Assistant Restauration**.)  
7. Sélectionnez le fichier de sauvegarde approprié pour restaurer à partir d’et assurez-vous que le **disque du système** et **état du système** cases sont cochées.  
8. Cliquez sur **démarrer la restauration**.  
9. Lorsque l’opération de restauration est terminée, redémarrez l’ordinateur.  
  
Utilisez la procédure suivante pour effectuer une restauration faisant autorité (également connu comme principale) de SYSVOL sur un contrôleur de domaine qui exécute Windows Server 2003. Effectuez cette procédure uniquement sur le premier DC Windows Server 2003 qui est restauré dans le domaine.  
  
### <a name="to-perform-an-authoritative-restore-of-sysvol"></a>Pour effectuer une restauration faisant autorité de SYSVOL  
  
1. Effectuez les étapes 1 à 8 dans la procédure précédente.  
2. Dans le **confirmer la restauration** boîte de dialogue, cliquez sur **avancé**.  
3. Pour effectuer une restauration faisant autorité de SYSVOL, activez la case à cocher **lors de la restauration de jeux de données répliqués, marquer les données restaurées en tant que données principales pour tous les réplicas**.  

   > [!NOTE]
   > Marquer les données restaurées comme les données principales dans la sauvegarde revient à affecter la **BurFlags** entrée à D4 sous la sous-clé de Registre suivante :  
   >   
   > **Jeux de réplicas HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NtFrs\Parameters\Cumulative\\**  *GUID*  

4. Lorsque l’opération de restauration est terminée, redémarrez l’ordinateur.  
  
## <a name="install-and-configure-the-dns-server-service"></a>Installer et configurer le service serveur DNS

Si le contrôleur de domaine que vous avez restauré à partir de la sauvegarde s’exécute Windows Server 2003, vous pouvez installer le serveur DNS sans connecter le contrôleur de domaine à un réseau.  
  
### <a name="to-install-and-configure-the-dns-server-service"></a>Pour installer et configurer le service serveur DNS  
  
1. Ouvrir l’Assistant Composants de Windows. Pour ouvrir l’Assistant :  

   - Cliquez sur **Démarrer**, sur **Panneau de configuration**, puis sur **Ajout/Suppression de programmes**.  
   - Cliquez sur **ajouter/supprimer des composants de Windows**.  

2. Dans **composants**, sélectionnez le **Services de mise en réseau** case à cocher, puis cliquez sur **détails**.  
3. Dans **sous-composants de Services de mise en réseau**, sélectionnez le **système DNS (Domain Name)** case à cocher, cliquez sur **OK**, puis cliquez sur **suivant**.  
4. Si vous êtes invité, dans **copier les fichiers**, tapez le chemin d’accès complet des fichiers de distribution, puis cliquez sur **OK**.  

   Après l’installation, procédez comme suit pour configurer le serveur DNS.  

5. Cliquez sur **Démarrer**, pointez sur **tous les programmes**, pointez sur **outils d’administration**, puis cliquez sur **DNS**.  
6. Créer des zones DNS pour les mêmes noms de domaine DNS qui étaient hébergés sur les serveurs DNS avant la défaillance critique. Pour plus d’informations, voir Ajouter une Zone de recherche directe ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).  
7. Configurer les données DNS telle qu’elle existait avant la défaillance critique. Exemple :  

   - Configurer des zones DNS à être stockées dans AD DS. Pour plus d’informations, consultez le Type de Zone de modification ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).  
   - Configurer la zone DNS faisant autoritée pour les enregistrements de ressource de (localisateur de contrôleur de domaine) de contrôleur de domaine permettre la mise à jour dynamique sécurisée. Pour plus d’informations, consultez Autoriser uniquement sécuriser les mises à jour dynamiques ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).  

8. Assurez-vous que la zone DNS parente contienne des enregistrements de ressources délégation (nom de serveur de (noms NS) et de type glue ressource hôte (A) enregistrements) pour la zone enfant qui est hébergé sur ce serveur DNS. Pour plus d’informations, voir créer une délégation de Zone ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).  
9. Après avoir configuré le DNS, à l’invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE :  

   **net stop netlogon**

10. Tapez la commande suivante et appuyez sur ENTRÉE :  

   **Net start netlogon**

   > [!NOTE]
   > Connexion réseau enregistre les enregistrements de ressources du localisateur de contrôleur de domaine DNS pour ce contrôleur de domaine. Si vous installez le service serveur DNS sur un serveur dans le domaine enfant, il se peut que ce contrôleur de domaine ne sera pas capable d’inscrire ses enregistrements immédiatement. Il s’agit, car il est actuellement isolé dans le cadre du processus de récupération et son serveur DNS principal est le serveur DNS de racine de forêt. Configurer cet ordinateur avec la même adresse IP qu’elle avait avant le sinistre afin d’éviter les échecs de recherche de service de contrôleur de domaine.

## <a name="next-steps"></a>Étapes suivantes

- [Récupération de forêt AD - conditions préalables](AD-Forest-Recovery-Prerequisties.md)  
- [Récupération de forêt AD - concevoir un plan de récupération personnalisée-forêt](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Récupération de forêt AD - identifier le problème](AD-Forest-Recovery-Identify-the-Problem.md)
- [Récupération de forêt AD - déterminer comment récupérer](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Récupération de forêt AD - effectuer une récupération initiale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)  
- [Récupération de forêt AD - Forum aux Questions](AD-Forest-Recovery-FAQ.md)  
- [Récupération de forêt AD - récupération d’un domaine unique au sein d’une forêt Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Récupération de forêt AD - récupération de forêt avec les contrôleurs de domaine Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
