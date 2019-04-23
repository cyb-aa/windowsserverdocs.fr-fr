---
title: Récupération de forêt AD - prendre un rôle de maître d’opérations
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.technology: identity-adds
ms.openlocfilehash: 1994d49652ee9eb10f6afc73cf5b4630b4718e77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858460"
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>Récupération de forêt AD - prendre un rôle de maître d’opérations  

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

La procédure suivante permet de prendre un rôle de maître d’opérations (également appelé un rôle d’opérations maître unique flottant (FSMO)). Vous pouvez utiliser Ntdsutil.exe, un outil de ligne de commande qui est installé automatiquement sur tous les contrôleurs de domaine.  
  
## <a name="to-seize-an-operations-master-role"></a>Pour prendre un rôle de maître d’opérations  
  
1. À l'invite de commandes, tapez la commande suivante et appuyez sur Entrée :  

   ```  
   ntdsutil  
   ```  

2. À la **ntdsutil :** invite, tapez la commande suivante, puis appuyez sur ENTRÉE :  

   ```  
   roles  
   ```  

3. À la **FSMO maintenance :** invite, tapez la commande suivante, puis appuyez sur ENTRÉE :  

   ```  
   connections  
   ```  

4. À la **connexions au serveur :** invite, tapez la commande suivante, puis appuyez sur ENTRÉE :  

   ```  
   Connect to server ServerFQDN  
   ```  

   Où *ServerFQDN* est le nom de domaine complet (FQDN) de ce contrôleur de domaine, par exemple : **se connecter au serveur nycdc01.example.com**.  

   Si *Fqdn_serveur* ne pas réussir, utilisez le nom NetBIOS du contrôleur de domaine.  

5. À la **connexions au serveur :** invite, tapez la commande suivante, puis appuyez sur ENTRÉE :  

   ```  
   quit  
   ```  

6. En fonction du rôle que vous souhaitez prendre, à la **FSMO maintenance :** invite, tapez la commande appropriée, comme décrit dans le tableau suivant, puis appuyez sur ENTRÉE.  
  
|Rôle|Informations d’identification|Command|  
|----------|-----------------|-------------|  
|Maître d’attribution de noms de domaine|Administrateurs de l’entreprise|**Prendre le rôle maître d’attribution de noms**|  
|Contrôleur de schéma|Administrateurs du schéma|**Prendre le rôle maître de schéma**|  
|Maître d’infrastructure **Remarque :**  Une fois que vous prenez le rôle de maître d’infrastructure, vous pouvez recevoir une erreur plus tard si vous avez besoin d’exécuter Adprep /Rodcprep. Pour plus d’informations, consultez l’article [949257](https://support.microsoft.com/kb/949257).|Administrateurs du domaine|**Prendre le rôle maître d’infrastructure**|  
|Maître d’émulateur PDC|Administrateurs du domaine|**Prendre le contrôleur de domaine principal**|  
|Maître RID|Administrateurs du domaine|**Prendre le rôle maître rid**|  

Après avoir confirmé la demande, Active Directory ou AD DS tente de transférer le rôle. Lorsque le transfert échoue, des informations d’erreur s’affiche, et Active Directory ou AD DS effectue la saisie. Une fois la saisie terminée, une liste des rôles et le nom Lightweight Directory Access Protocol (LDAP) du serveur qui détient actuellement chaque rôle s’affiche. Vous pouvez également exécuter **Netdom Query FSMO** à une invite de commandes avec élévation de privilèges pour vérifier les détenteurs du rôle en cours.  
  
> [!NOTE]
> Si cet ordinateur n’a pas un maître RID avant la défaillance et que vous tentez de prendre le rôle de maître RID, l’ordinateur essaie de se synchroniser avec un partenaire de réplication avant d’accepter ce rôle. Toutefois, étant donné que cette étape est effectuée lorsque l’ordinateur est isolé, il ne réussira pas lors de la synchronisation avec un partenaire. Par conséquent, une boîte de dialogue s’affiche vous demandant si vous souhaitez poursuivre l’opération en dépit de cet ordinateur n’est pas en mesure de se synchroniser avec un partenaire. Cliquez sur **Oui**.  
  
## <a name="next-steps"></a>Étapes suivantes

- [Guide de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)
