---
title: "Récupération de la forêt ActiveDirectory - exécution d’une récupération complète du serveur"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 1a1182a6-4462-4a13-806e-0e642a0d5db2
ms.technology: identity-adfs
ms.openlocfilehash: 5de71cc005e9ec4c1425f9fa805b3c043ab4ea36
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---performing-a-full-server-recovery"></a>Récupération de la forêt ActiveDirectory - exécution d’une récupération complète du serveur 

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2
 
Utilisez la procédure suivante pour effectuer une récupération complète du serveur pour Windows Server2016, 2012R2 ou 2012. 

## <a name="active-directory-full-server-recovery"></a>Récupération complète du serveur d’ActiveDirectory
Une récupération complète du serveur est nécessaire si vous restaurez vers un autre matériel ou sur une instance différente du système d’exploitation. Gardez à l’esprit les éléments suivants:

- Le nombre des lecteurs sur le serveur cible doit être égal au nombre dans la sauvegarde et ils doivent être la même taille ou supérieur.
- Le serveur cible doit être démarré à partir du DVD du système d’exploitation afin d’accéder à la **réparer votre ordinateur** option. 
- Si la cible de que contrôleur de domaine est en cours d’exécution dans une machine virtuelle sur Hyper-V et la sauvegarde est stockée sur un emplacement réseau, vous devez installer une carte réseau héritée.  
- Une fois que vous effectuez une récupération complète du serveur, vous devez effectuer séparément une restauration faisant autorité de SYSVOL, comme décrit dans [récupération de la forêt ActiveDirectory - effectuer une synchronisation faisant autoritée de SYSVOL répliqué DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).


Selon votre scénario, utilisez une des procédures suivantes pour effectuer une restauration complète.  
  
### <a name="perform-a-full-server-restore-with-a-local-backup-with-the-latest-image"></a>Effectuer une restauration complète du serveur avec une sauvegarde locale avec la dernière image
  
1.  Démarrer le programme d’installation de Windows, spécifiez la langue, l’heure et format monétaire et options de clavier, cliquez sur **suivant**.  
2.  Cliquez sur **réparer votre ordinateur**.</br>
![Restauration du serveur](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore1.png)
3.  Cliquez sur **dépanner**.</br>
![Restauration du serveur](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore2.png)
4.  Cliquez sur **récupération de l’Image système**.</br>
![Restauration du serveur](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore3.png)
5.  Cliquez sur **Windows Server2016**.  
![Restauration du serveur](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore4.png)
6.  Si vous restaurez la dernière sauvegarde locale, cliquez sur **utiliser la dernière image système (recommandée)** et cliquez sur **suivant**.
![Restauration du serveur](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore5.png)
7.  Vous aurez désormais une option permettant de:
    -  Formater et repartitionner les disques
    -  Installer des pilotes
    -  Désélectionnant le **avancé** fonctionnalités de redémarrer automatiquement et la vérification des erreurs de disque.  Ces dernières sont activées par défaut.
![Restauration du serveur](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore6.png)
8. Cliquez sur **suivant**.
9. Cliquez sur **Terminer**.  Vous serez invité demandant si vous êtes sûr de que vouloir continuer.  Cliquez sur **Oui**.  
![Restauration du serveur](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore11.png) 
10. Une fois cette étape termine effectuer une restauration faisant autorité de SYSVOL, comme décrit dans [récupération de la forêt ActiveDirectory - effectuer une synchronisation faisant autoritée de SYSVOL répliqué DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).
 

### <a name="perform-a-full-server-restore-with-any-image-local-or-remote"></a>Effectuer une restauration complète du serveur avec une image locale ou distante
1.  Démarrer le programme d’installation de Windows, spécifiez la langue, l’heure et format monétaire et options de clavier, cliquez sur **suivant**.  
2.  Cliquez sur **réparer votre ordinateur**.</br>
3.  Cliquez sur **dépanner**, cliquez sur **récupération de l’Image système**, puis cliquez sur **Windows Server2016**.  
4.  Si vous restaurez la dernière sauvegarde locale, cliquez sur **sélectionner une image système** et cliquez sur **suivant**.

5.  Vous pouvez maintenant sélectionner l’emplacement de la sauvegarde que vous voulez restaurer.  Si l’image est local vous pouvez le sélectionner dans la liste.  
6.  Si l’image se trouve sur un partage réseau, sélectionnez **avancé**.  Vous pouvez également sélectionner **avancé** si vous avez besoin installer un pilote.
![Restauration du serveur](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore7.png)
7.  Si vous restaurez à partir du réseau après avoir cliqué sur **avancé** sélectionnez **recherche d’une image du système sur le réseau**.  Vous pouvez être invité à restaurer la connectivité réseau.  Cliquez sur Ok. </br>
![Restauration du serveur](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore8.png)
8. Tapez le chemin d’accès UNC vers l’emplacement du partage de la sauvegarde (par exemple, \\\server1\backups), cliquez sur **OK**.  Vous pouvez également taper l’adresse IP du serveur cible, par exemple, \\\192.168.1.3\backups.  
![Restauration du serveur](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore9.png)
10. Tapez les informations d’identification nécessaires pour accéder au partage, puis cliquez sur OK.  
11. Désormais **sélectionner la date et l’heure de l’image du système pour restaurer** et cliquez sur **suivant**.
12. Vous aurez désormais une option permettant de:
    1.   Formater et repartitionner les disques
    2.   Installer des pilotes
    3.   Désélectionnant le **avancé** fonctionnalités de redémarrer automatiquement et la vérification des erreurs de disque.  Ces dernières sont activées par défaut.
13. Cliquez sur **suivant**.
14. Cliquez sur **Terminer**.  Vous serez invité demandant si vous êtes sûr de que vouloir continuer.  Cliquez sur **Oui**.   
15. Une fois cette étape termine effectuer une restauration faisant autorité de SYSVOL, comme décrit dans [récupération de la forêt ActiveDirectory - effectuer une synchronisation faisant autoritée de SYSVOL répliqué DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).


### <a name="enabling-the-network-adapter-for-a-network-backup"></a>L’activation de la carte réseau pour une sauvegarde réseau
Si vous avez besoin activer une carte réseau à partir de l’invite de commandes restaurer à partir d’un partage réseau, procédez comme suit.

1.  Démarrer le programme d’installation de Windows, spécifiez la langue, l’heure et format monétaire et options de clavier, cliquez sur **suivant**.  
2.  Cliquez sur **réparer votre ordinateur**. Je
3.  Cliquez sur **dépanner**, cliquez sur **invite de commandes**.  
4.  Tapez la commande suivante et appuyez sur ENTRÉE:  
  
    ```  
    wpeinit  
    ```   
5.  Pour vérifier que le nom de la carte réseau, tapez:  
  
    ```  
    show interfaces  
    ```  
  
     Tapez les commandes suivantes et appuyez sur ENTRÉE après chaque commande:  
  
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
  
     Par exemple:  
  
    ```  
    set address "Local Area Connection" static 192.168.1.2 255.0.0.0 192.168.1.1 1  
    ```  
  
     Type `quit`pour revenir à une invite de commandes. Type `ipconfig /all`pour vérifier le réseau carte dispose d’une adresse IP et un test ping avec l’adresse IP du serveur qui héberge le partage de la sauvegarde pour confirmer la connectivité. Fermez l’invite de commandes lorsque vous avez terminé.  
  
6.  Maintenant que la carte réseau fonctionne, sélectionnez les étapes ci-dessus pour effectuer la restauration.

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de forêt ActiveDirectory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)
