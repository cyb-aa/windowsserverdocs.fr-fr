---
title: Récupération de forêt AD - pools RID de déclenchement
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c37bc129-a5e0-4219-9ba7-b4cf3a9fc9a4
ms.technology: identity-adds
ms.openlocfilehash: c8f91226e10ea6681933d5a5dc00b92f5ab2179c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862980"
---
# <a name="ad-forest-recovery---raising-the-value-of-available-rid-pools"></a>Récupération de forêt AD - augmentation de la valeur de pools RID disponibles 

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Utilisez la procédure suivante pour augmenter la valeur de l’ID relatif (RID) des pools que le maître d’opérations RID alloue une fois que ce contrôleur de domaine est restauré. En augmentant la valeur des pools RID disponibles, vous pouvez vous assurer qu’aucun contrôleur de domaine n’alloue un RID pour un principal de sécurité qui a été créé après la sauvegarde a été utilisée pour restaurer le domaine. 

## <a name="about-active-directory-rid-pools-and-ridavailablepool"></a>À propos des Pools RID Active Directory et rIDAvailablePool

Chaque domaine possède un objet **CN = RID Manager$, CN = System, DC**=<*nom_domaine*>. Cet objet possède un attribut nommé **rIDAvailablePool**. Cette valeur d’attribut conserve l’espace RID global pour un domaine entier. La valeur est un entier long avec les parties supérieures et inférieures. La partie supérieure définit le nombre d’entités de sécurité qui peuvent être alloués pour chaque domaine (0x3FFFFFFF ou simplement plus de 1 milliard). La partie inférieure est le nombre d’identificateurs RID qui ont été allouées dans le domaine. 
  
> [!NOTE]
> Dans Windows Server 2016 et 2012, le nombre d’entités de sécurité qui peuvent être alloués est augmenté à peine plus de 2 milliards. Pour plus d’informations, consultez [l’émission RID de la gestion des](https://technet.microsoft.com/library/jj574229.aspx). 
  
- Exemple de valeur : 4611686014132422708  
- Partie basse : 2100 (à partir du pool RID suivant doit être allouée)  
- Partie supérieure : 1073741823 (nombre total d’identificateurs RID qui peuvent être créés dans un domaine)  
  
Lorsque vous augmentez la valeur de l’entier élevé, vous augmentez la valeur de la partie basse. Par exemple, si vous ajoutez 100 000 à la valeur de l’exemple de 4611686014132422708 pour une somme de 4611686014132522708, la nouvelle partie basse est 102100. Cela indique que le pool RID suivant qui sera alloué par le maître RID commence par 102100 au lieu de 2100. 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-adsiedit-and-the-calculator"></a>Pour augmenter la valeur de pools RID disponibles à l’aide d’adsiedit et la calculatrice

1. Ouvrez le Gestionnaire de serveur, cliquez sur **outils** et cliquez sur **ADSI Edit**.
2. Avec le bouton droit, sélectionnez **se connecter à** et vous connecter ne le contexte de nommage par défaut, cliquez sur **OK**.
   ![Éditeur ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi1.png) 
3. Recherchez le chemin d’accès nom_unique suivantes : **CN = RID Manager$, CN = System, DC =<domain name>**.
   ![Éditeur ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi2.png) 
3. Avec le bouton droit et sélectionnez les propriétés de CN = RID Manager$. 
4. Sélectionnez l’attribut **rIDAvailablePool**, cliquez sur **modifier**, puis copiez la valeur d’entier long dans le Presse-papiers.
   ![Éditeur ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi3.png)  
5. Démarrer la calculatrice et à partir de la **vue** menu, sélectionnez **Mode scientifique**. 
6. Ajouter 100 000 à la valeur actuelle.
   ![Éditeur ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi4.png) 
7. À l’aide de ctrl-c, ou le **copie** commande à partir de la **modifier** menu, copiez la valeur dans le Presse-papiers. 
8. Dans la boîte de dialogue de modification d’adsiedit, collez cette nouvelle valeur. 
   ![Éditeur ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi5.png) 
9. Cliquez sur **OK** dans la boîte de dialogue, et **appliquer** dans la feuille de propriétés pour mettre à jour le **rIDAvailablePool** attribut. 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-ldp"></a>Pour augmenter la valeur de pools RID disponibles à l’aide de LDP  
  
1. À l'invite de commandes, tapez la commande suivante et appuyez sur Entrée :  
   **ldp**  
2. Cliquez sur **connexion**, cliquez sur **Connect**, tapez le nom du Gestionnaire RID, puis cliquez sur **OK**. 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp1.png)
3. Cliquez sur **connexion**, cliquez sur **lier**, sélectionnez **lier avec les informations d’identification** et tapez vos informations d’identification d’administration, puis cliquez sur **OK**. 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp2.png)
4. Cliquez sur **vue**, cliquez sur **arborescence** puis tapez le chemin de nom unique suivant :  CN = RID Manager$, CN = System, DC =*nom de domaine*  
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp3.png)
5. Cliquez sur **Parcourir**, puis cliquez sur **modifier**. 
6. Ajouter 100 000 à actuel **rIDAvailablePool** valeur et tapez ensuite la somme en **valeurs**. 
7. Dans **Dn**, type `cn=RID Manager$,cn=System,dc=` *< nom de domaine\>*. 
8. Dans **modifier l’entrée attribut**, type `rIDAvailablePool`. 
9. Sélectionnez **remplacer** comme l’opération, puis cliquez sur **entrée**.
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp4.png) 
10. Cliquez sur **exécuter** pour exécuter l’opération. Cliquez sur **Fermer**.
11. Pour valider la modification, cliquez sur **vue**, cliquez sur **arborescence**, puis tapez le chemin de nom unique suivant :   CN = RID Manager$, CN = System, DC =*nom de domaine*.   Vérifier le **rIDAvailablePool** attribut. 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp5.png)

## <a name="next-steps"></a>Étapes suivantes

- [Guide de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)
