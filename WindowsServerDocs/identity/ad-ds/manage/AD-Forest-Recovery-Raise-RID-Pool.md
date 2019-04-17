---
title: "Récupération de la forêt ActiveDirectory - pools RID de déclenchement"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c37bc129-a5e0-4219-9ba7-b4cf3a9fc9a4
ms.technology: identity-adfs
ms.openlocfilehash: e6b5dc8b9c0b701fe2cd1b0c88f7edc22802c393
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---raising-the-value-of-available-rid-pools"></a>Récupération de la forêt ActiveDirectory - déclencher la valeur de pools RID disponibles 

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2
 
 Utilisez la procédure suivante pour augmenter la valeur de l’ID relatif (RID) des pools que le maître d’opérations RID alloue une fois que ce contrôleur de domaine est restauré. En augmentant la valeur des pools RID disponibles, vous pouvez vous assurer qu’aucun contrôleur de domaine n’alloue un RID pour une entité de sécurité qui a été créée après la sauvegarde qui a été utilisée pour restaurer le domaine.  
 
## <a name="about-active-directory-rid-pools-and-ridavailablepool"></a>À propos des Pools RID ActiveDirectory et rIDAvailablePool
 Chaque domaine possède un objet **CN = RID Manager$, CN = System, DC**=<*nom_domaine*>. Cet objet possède un attribut nommé **rIDAvailablePool**. Cette valeur d’attribut conserve l’espace RID global pour un domaine entier. La valeur est un entier avec une partie supérieure et inférieure. La partie supérieure définit le nombre de principaux de sécurité pouvant être allouée pour chaque domaine (0x3FFFFFFF ou simplement plus de 1 milliard). La partie inférieure est le nombre d’identificateurs RID qui ont été allouées dans le domaine.  
  
> [!NOTE]
>  Dans Windows Server2016 et 2012, le nombre de principaux de sécurité pouvant être allouée est augmenté pour simplement plus de 2milliards. Pour plus d’informations, voir [gestion RID issuance](https://technet.microsoft.com/library/jj574229.aspx).  
  
-   Exemple de valeur: 4611686014132422708  
  
-   Partie basse: 2100 (début du pool RID suivant pour être allouée)  
  
-   La partie supérieure: 1073741823 (nombre total d’identificateurs RID qui peuvent être créés dans un domaine)  
  
 Lorsque vous augmentez la valeur de l’entier, vous augmentez la valeur de la partie basse. Par exemple, si vous ajoutez 100000 à la valeur de l’exemple de 4611686014132422708 pour une somme de 4611686014132522708, la nouvelle partie faible est 102100. Cela indique que le pool RID suivant qui est affecté par le maître RID commence avec 102100 au lieu de 2100.  
  
### <a name="to-raise-the-value-of-available-rid-pools-using-adsiedit-and-the-calculator--"></a>Pour augmenter la valeur de pools RID disponibles à l’aide d’adsiedit et la calculatrice»  
1.  Ouvrez le Gestionnaire de serveur, cliquez sur **outils** et cliquez sur **ADSI Edit**.    
2.  Avec le bouton droit, sélectionnez **se connecter à** et connecter le contexte de nommage par défaut et puis cliquez sur **OK**.
![Éditeur ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi1.png) 
3. Recherchez le chemin d’accès du nom unique suivantes: **CN = RID Manager$, CN = System, DC =<domain name>**.
![Éditeur ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi2.png) 
3.  Avec le bouton droit et sélectionnez les propriétés de CN = RID Manager$.  
4.  Sélectionnez l’attribut **rIDAvailablePool**, cliquez sur **modifier**, puis copiez la valeur entier dans le Presse-papiers.
![Éditeur ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi3.png)  
5.  Démarrer la calculatrice et à partir de la **affichage** menu, sélectionnez **en Mode scientifique**.  6.  Ajoutez 100000 à la valeur actuelle.  
![Éditeur ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi4.png) 
7.  À l’aide de ctrl-c, ou le **copie** commande à partir de la **modifier** menu, copiez la valeur dans le Presse-papiers.  
8.  Dans la boîte de dialogue Modification d’adsiedit, collez cette nouvelle valeur. 
![Éditeur ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi5.png) 
9. Cliquez sur **OK** dans la boîte de dialogue et **appliquer** dans la feuille de propriétés pour mettre à jour le **rIDAvailablePool** attribut.  
  
### <a name="to-raise-the-value-of-available-rid-pools-using-ldp"></a>Pour augmenter la valeur de pools RID disponibles à l’aide de LDP  
  
1.  À l’invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE:  
  
     **LDP**  
  
2.  Cliquez sur **connexion**, cliquez sur **connexion**, tapez le nom du Gestionnaire RID, puis cliquez sur **OK **.  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp1.png)
3.  Cliquez sur **connexion**, cliquez sur **liaison**, sélectionnez **liaison avec informations d’identification** et vos informations d’identification d’administration, puis tapez **OK **.  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp2.png)
4.  Cliquez sur **affichage**, cliquez sur **arborescence** puis tapez le chemin d’accès du nom unique suivant: CN = RID Manager$, CN = System, DC =*nom de domaine*  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp3.png)
5.  Cliquez sur **Parcourir**, puis cliquez sur **modifier **.  
6.  Ajouter de 100000 à actuel **rIDAvailablePool** valeur, puis tapez la somme dans **valeurs **.  
7.  Dans **Dn**, type `cn=RID Manager$,cn=System,dc=`*< domaine nom\ >*.  
8.  Dans **modifier l’entrée attribut**, type `rIDAvailablePool`.  
9. Sélectionnez **remplacer** comme l’opération, puis cliquez sur **entrée **. </br>
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp4.png) 
10. Cliquez sur **exécuter** pour exécuter l’opération.  Cliquez sur **fermer **.
11. Pour valider les modifications, cliquez sur **affichage**, cliquez sur **arborescence**, puis tapez le chemin d’accès du nom unique suivant: CN = RID Manager$, CN = System, DC =*nom de domaine *.    Vérifiez le **rIDAvailablePool** attribut.  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp5.png)

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de forêt ActiveDirectory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)
 
