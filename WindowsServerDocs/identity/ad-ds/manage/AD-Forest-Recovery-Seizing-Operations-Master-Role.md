---
title: 'Récupération de la forêt Active Directory : prise d’un rôle de maître d’opérations'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.technology: identity-adds
ms.openlocfilehash: 672dc119845acbe9cf38f82c793bd377d31db3b2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390283"
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>Récupération de la forêt Active Directory : prise d’un rôle de maître d’opérations  

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Utilisez la procédure suivante pour prendre un rôle de maître d’opérations (également connu sous le nom de rôle d’opérations à maître unique flottant (FSMO)). Vous pouvez utiliser Ntdsutil. exe, un outil de ligne de commande qui est installé automatiquement sur tous les contrôleurs de contrôle.  
  
## <a name="to-seize-an-operations-master-role"></a>Pour prendre un rôle de maître d’opérations  
  
1. À l'invite de commandes, tapez la commande suivante et appuyez sur Entrée :  

   ```  
   ntdsutil  
   ```  

2. À l’invite **ntdsutil :** , tapez la commande suivante, puis appuyez sur entrée :  

   ```  
   roles  
   ```  

3. À l’invite **maintenance FSMO :** , tapez la commande suivante, puis appuyez sur entrée :  

   ```  
   connections  
   ```  

4. À l’invite **connexions serveur :** , tapez la commande suivante, puis appuyez sur entrée :  

   ```  
   Connect to server ServerFQDN  
   ```  

   Où *ServerFQDN* est le nom de domaine complet (FQDN) de ce contrôleur de domaine, par exemple : **se connecter au serveur nycdc01.example.com**.  

   Si *ServerFQDN* ne fonctionne pas, utilisez le nom NetBIOS du contrôleur de périphérique.  

5. À l’invite **connexions serveur :** , tapez la commande suivante, puis appuyez sur entrée :  

   ```  
   quit  
   ```  

6. En fonction du rôle que vous souhaitez prendre, à l’invite **maintenance FSMO :** , tapez la commande appropriée, comme décrit dans le tableau suivant, puis appuyez sur entrée.  
  
|Rôle|Informations d’identification|Command|  
|----------|-----------------|-------------|  
|Maître d’attribution de noms de domaine|Administrateurs de l’entreprise|**Prendre le maître d’attribution de noms**|  
|Contrôleur de schéma|Administrateurs du schéma|**Prendre le contrôleur de schéma**|  
|Remarque du maître d’infrastructure **:**  Une fois que vous avez assumé le rôle de maître d’infrastructure, vous pouvez recevoir un message d’erreur ultérieurement si vous devez exécuter adprep/rodcprep. Pour plus d’informations, consultez l’article [949257](https://support.microsoft.com/kb/949257)de la base de connaissances.|Administrateurs du domaine|**Prendre le maître d’infrastructure**|  
|Maître d’émulateur de contrôleur de domaine principal|Administrateurs du domaine|**Prendre le PDC**|  
|Maître RID|Administrateurs du domaine|**Prendre le maître RID**|  

Après avoir confirmé la demande, Active Directory ou AD DS tente de transférer le rôle. En cas d’échec du transfert, des informations d’erreur s’affichent, et Active Directory ou AD DS se poursuivent. Une fois la cessation terminée, une liste des rôles et le nom LDAP (Lightweight Directory Access Protocol) du serveur qui contient actuellement chaque rôle s’affiche. Vous pouvez également exécuter **NETDOM QUERY FSMO** à une invite de commandes avec élévation de privilèges pour vérifier les détenteurs de rôle actuels.  
  
> [!NOTE]
> Si cet ordinateur n’était pas un maître RID avant l’échec et que vous tentez de prendre le rôle de maître RID, l’ordinateur tente de se synchroniser avec un partenaire de réplication avant d’accepter ce rôle. Toutefois, étant donné que cette étape est effectuée lorsque l’ordinateur est isolé, elle ne parvient pas à se synchroniser avec un partenaire. Par conséquent, une boîte de dialogue s’affiche pour vous demander si vous souhaitez poursuivre l’opération en dépit de l’impossibilité de synchroniser l’ordinateur avec un partenaire. Cliquez sur **Oui**.  
  
## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)
