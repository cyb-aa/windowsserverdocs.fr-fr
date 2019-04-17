---
title: "Récupération de la forêt ActiveDirectory - prendre un rôle de maître d’opérations"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.technology: identity-adfs
ms.openlocfilehash: 7ca6e3746586feeb3573b1ad6ba02831d1f5addd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>Récupération de la forêt ActiveDirectory - prendre un rôle de maître d’opérations  

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

 Utilisez la procédure suivante pour prendre un rôle de maître d’opérations (également appelé un rôle d’opérations à maître unique (FSMO)). Vous pouvez utiliser Ntdsutil.exe, un outil de ligne de commande qui est installé automatiquement sur tous les contrôleurs de domaine.  
  
## <a name="to-seize-an-operations-master-role"></a>Pour prendre un rôle de maître d’opérations  
  
1.  À l’invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE:  
  
    ```  
    ntdsutil  
    ```  
  
2.  À la **ntdsutil:** invite, tapez la commande suivante et appuyez sur ENTRÉE:  
  
    ```  
    roles  
    ```  
  
3.  À la **maintenance FSMO:** invite, tapez la commande suivante et appuyez sur ENTRÉE:  
  
    ```  
    connections  
    ```  
  
4.  À la **connexions au serveur:** invite, tapez la commande suivante et appuyez sur ENTRÉE:  
  
    ```  
    Connect to server ServerFQDN  
    ```  
  
     Où *Fqdn_serveur* est le nom de domaine complet (FQDN) de ce contrôleur de domaine, par exemple: **se connecter au serveur nycdc01.example.com**.  
  
     Si *Fqdn_serveur* ne pas réussir, utilisez le nom NetBIOS du contrôleur de domaine.  
  
5.  À la **connexions au serveur:** invite, tapez la commande suivante et appuyez sur ENTRÉE:  
  
    ```  
    quit  
    ```  
  
6.  Selon le rôle que vous souhaitez prendre, à le **maintenance FSMO:** invite, tapez la commande appropriée comme décrit dans le tableau suivant, puis appuyez sur ENTRÉE.  
  
    |Rôle|Informations d’identification|Commande|  
    |----------|-----------------|-------------|  
    |Maître d’attribution de noms de domaine|Administrateurs de l’entreprise|**Prendre le rôle maître d’attribution de noms**|  
    |Contrôleur de schéma|Administrateurs du schéma|**Prendre le rôle maître de schéma**|  
    |Maître d’infrastructure **Remarque:** après prendre le rôle de maître d’infrastructure, vous pouvez recevoir une erreur plus tard si vous avez besoin exécuter Adprep /Rodcprep. Pour plus d’informations, voir l’article [949257](https://support.microsoft.com/kb/949257).|Admins du domaine|**Prendre le rôle maître d’infrastructure**|  
    |Maître d’émulateur PDC|Admins du domaine|**Prendre le contrôleur de domaine principal**|  
    |Maître RID|Admins du domaine|**Prendre le rôle maître rid**|  
  
     Après avoir confirmé la demande, ActiveDirectory ou domaine ActiveDirectory tente de transférer le rôle. Lorsque le transfert échoue, des informations d’erreur s’affiche, et ActiveDirectory ou domaine ActiveDirectory effectue la saisie. Une fois la saisie terminée, une liste des rôles et le nom de répertoire accès protocole LDAP (Lightweight) du serveur qui détient actuellement chaque rôle s’affiche. Vous pouvez également exécuter **Netdom Query FSMO** à une invite de commandes avec élévation de privilèges pour vérifier les détenteurs du rôle en cours.  
  
    > [!NOTE]
    >  Si cet ordinateur n’a pas été un maître d’opérations RID avant la défaillance et que vous essayez de prendre le rôle de maître RID, l’ordinateur essaie de se synchroniser avec un partenaire de réplication avant d’accepter ce rôle. Toutefois, étant donné que cette étape est exécutée lorsque l’ordinateur est isolé, il échoue lors de la synchronisation avec un partenaire. Par conséquent, une boîte de dialogue s’affiche vous demandant si vous souhaitez poursuivre l’opération en dépit de cet ordinateur n’est ne pas en mesure de se synchroniser avec un partenaire. Cliquez sur **Oui**.  
  
## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de forêt ActiveDirectory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)
