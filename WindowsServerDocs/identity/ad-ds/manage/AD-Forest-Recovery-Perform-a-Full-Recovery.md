---
title: Récupération de la forêt Active Directory-exécution d’une récupération complète du serveur
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 1a1182a6-4462-4a13-806e-0e642a0d5db2
ms.technology: identity-adds
ms.openlocfilehash: 1ade1f2e316387fbe84209c1bc7a986fff6f2a71
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390539"
---
# <a name="ad-forest-recovery---performing-a-full-server-recovery"></a>Récupération de la forêt Active Directory-exécution d’une récupération complète du serveur 

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Utilisez la procédure suivante pour effectuer une récupération complète du serveur pour Windows Server 2016, 2012 R2 ou 2012. 

## <a name="active-directory-full-server-recovery"></a>Active Directory la récupération complète du serveur

Une récupération complète du serveur est nécessaire si vous effectuez une restauration sur un autre matériel ou sur une autre instance du système d’exploitation. Gardez à l’esprit les points suivants :

- Le nombre de lecteurs sur le serveur cible doit être égal au nombre de la sauvegarde et doit avoir une taille identique ou supérieure.
- Le serveur cible doit être démarré à partir du DVD du système d’exploitation afin d’accéder à l’option **réparer votre ordinateur** . 
- Si le contrôleur de réseau cible est en cours d’exécution dans une machine virtuelle sur Hyper-V et que la sauvegarde est stockée sur un emplacement réseau, vous devez installer une carte réseau héritée. 
- Après avoir effectué une récupération complète du serveur, vous devez effectuer séparément une restauration faisant autorité de SYSVOL, comme décrit dans la rubrique [récupération de forêt Active Directory-exécution d’une synchronisation faisant autorité d’un SYSVOL répliqué par DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

Selon votre scénario, utilisez l’une des procédures suivantes pour effectuer une restauration complète. 
  
## <a name="perform-a-full-server-restore-with-a-local-backup-with-the-latest-image"></a>Effectuer une restauration complète du serveur avec une sauvegarde locale avec l’image la plus récente
  
1. Démarrez installation de Windows, spécifiez la langue, le format d’heure et de devise, ainsi que les options de clavier, puis cliquez sur **suivant**. 
2. Cliquez sur **Réparer votre ordinateur**.
   ![Server Restore @ no__t-1
3. Cliquez sur **Dépanner**.</br>
   ![Server Restore @ no__t-1
4. Cliquez sur récupération de l' **image système**.</br>
   ![Server Restore @ no__t-1
5. Cliquez sur **Windows Server 2016**. 
   ![Server Restore @ no__t-1
6. Si vous restaurez la sauvegarde locale la plus récente, cliquez sur **utiliser la dernière image système disponible (recommandé)** , puis cliquez sur **suivant**.
   ![Server Restore @ no__t-1
7. Vous pouvez désormais effectuer les opérations suivantes :
   -  Formater et repartitionner les disques
   -  Installer les pilotes
   -  Désélection des fonctionnalités **avancées** de redémarrage et de vérification automatiques des erreurs de disque. Celles-ci sont activées par défaut.
   ![Server Restore @ no__t-1
8. Cliquez sur **Suivant**.
9. Cliquez sur **Terminer**. Vous serez invité à confirmer que vous souhaitez continuer. Cliquez sur **Oui**. 
   ![Server Restore @ no__t-1 
10. Une fois cette opération terminée, effectuez une restauration faisant autorité de SYSVOL, comme décrit dans la rubrique [récupération de forêt Active Directory, effectuant une synchronisation faisant autorité d’un SYSVOL répliqué par DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

## <a name="perform-a-full-server-restore-with-any-image-local-or-remote"></a>Effectuer une restauration complète du serveur avec une image locale ou distante

1. Démarrez installation de Windows, spécifiez la langue, le format d’heure et de devise, ainsi que les options de clavier, puis cliquez sur **suivant**. 
2. Cliquez sur **Réparer votre ordinateur**.</br>
3. Cliquez sur **dépanner**, sur récupération de l' **image système**, puis sur **Windows Server 2016**. 
4. Si vous restaurez la sauvegarde locale la plus récente, cliquez sur **Sélectionner une image système** , puis sur **suivant**.
5. Vous pouvez maintenant sélectionner l’emplacement de la sauvegarde que vous souhaitez restaurer. Si l’image est locale, vous pouvez la sélectionner dans la liste. 
6. Si l’image se trouve sur un partage réseau, sélectionnez **avancé**. Vous pouvez également sélectionner **avancé** si vous devez installer un pilote.
   ![Server Restore @ no__t-1
7. Si vous effectuez une restauration à partir du réseau après avoir cliqué sur **avancé** , sélectionnez **Rechercher une image système sur le réseau**. Vous pouvez être invité à restaurer la connectivité réseau. Sélectionnez OK. </br>
   ![Server Restore @ no__t-1
8. Tapez le chemin d’accès UNC à l’emplacement du partage de sauvegarde (par exemple, \\ \ server1\backups), puis cliquez sur **OK**. Vous pouvez également taper l’adresse IP du serveur cible, par exemple \\ \ 192.168.1.3 \ sauvegardes. 
   ![Server Restore @ no__t-1
9. Tapez les informations d’identification nécessaires pour accéder au partage, puis cliquez sur OK. 
10. À présent, **Sélectionnez la date et l’heure de l’image système à restaurer** , puis cliquez sur **suivant**.
11. Vous pouvez désormais effectuer les opérations suivantes :
    - Formater et repartitionner les disques
    - Installer les pilotes
    - Désélection des fonctionnalités **avancées** de redémarrage et de vérification automatiques des erreurs de disque. Celles-ci sont activées par défaut.
12. Cliquez sur **Suivant**.
13. Cliquez sur **Terminer**. Vous serez invité à confirmer que vous souhaitez continuer. Cliquez sur **Oui**.  
14. Une fois cette opération terminée, effectuez une restauration faisant autorité de SYSVOL, comme décrit dans la rubrique [récupération de forêt Active Directory, effectuant une synchronisation faisant autorité d’un SYSVOL répliqué par DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

## <a name="enabling-the-network-adapter-for-a-network-backup"></a>Activation de la carte réseau pour une sauvegarde réseau

Si vous devez activer une carte réseau à partir de l’invite de commandes pour effectuer une restauration à partir d’un partage réseau, procédez comme suit.

1. Démarrez installation de Windows, spécifiez la langue, le format d’heure et de devise, ainsi que les options de clavier, puis cliquez sur **suivant**. 
2. Cliquez sur **Réparer votre ordinateur**. CLIQU
3. Cliquez sur **dépanner**, puis sur **invite de commandes**. 
4. Tapez la commande suivante et appuyez sur ENTRÉE :  

   ```  
   wpeinit  
   ```

5. Pour confirmer le nom de la carte réseau, tapez :  

   ```  
   show interfaces  
   ```  

   Tapez les commandes suivantes et appuyez sur entrée après chaque commande :  

   ```  
   netsh  
   ```  

   ```  
   interface  
   ```  
  
   ```  
   tcp  
   ```  

   ```  
   ipv4  
   ```  
  
   ```  
   set address "Name of Network Adapter" static IPv4 Address SubnetMask IPv4 Gateway Address 1  
   ```  

   Exemple :  
  
   ```  
   set address "Local Area Connection" static 192.168.1.2 255.0.0.0 192.168.1.1 1  
   ```  

   Tapez `quit` pour revenir à une invite de commandes. Tapez `ipconfig /all` pour vérifier que la carte réseau a une adresse IP et essayez d’exécuter une commande ping sur l’adresse IP du serveur qui héberge le partage de sauvegarde pour confirmer la connectivité. Lorsque vous avez terminé, fermez l’invite de commandes. 

6. Maintenant que la carte réseau fonctionne, sélectionnez les étapes ci-dessus pour terminer la restauration.

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)
