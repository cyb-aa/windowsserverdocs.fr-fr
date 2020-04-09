---
title: Récupération de la forêt Active Directory-déclenchement des pools RID
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: c37bc129-a5e0-4219-9ba7-b4cf3a9fc9a4
ms.technology: identity-adds
ms.openlocfilehash: 308dce9be53194eb7db91944964ae5de03345ab6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823852"
---
# <a name="ad-forest-recovery---raising-the-value-of-available-rid-pools"></a>Récupération de la forêt Active Directory-augmentation de la valeur des pools RID disponibles 

>S’applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Utilisez la procédure suivante pour augmenter la valeur des pools d’ID relatifs (RID) que le maître d’opérations RID allouera après la restauration de ce contrôleur de bus. En augmentant la valeur des pools RID disponibles, vous pouvez vous assurer qu’aucun contrôleur de domaine n’alloue un RID pour un principal de sécurité qui a été créé après la sauvegarde qui a été utilisée pour restaurer le domaine. 

## <a name="about-active-directory-rid-pools-and-ridavailablepool"></a>À propos de Active Directory les pools RID et rIDAvailablePool

Chaque domaine possède un objet **CN = RID Manager $, CN = System, DC**=<*domain_name*>. Cet objet a un attribut nommé **rIDAvailablePool**. Cette valeur d’attribut gère l’espace RID global pour un domaine entier. La valeur est un grand entier avec des parties supérieure et inférieure. La partie supérieure définit le nombre de principaux de sécurité qui peuvent être alloués pour chaque domaine (0x3FFFFFFF ou juste plus de 1 milliard). La partie inférieure est le nombre d’identificateurs RID qui ont été alloués dans le domaine. 
  
> [!NOTE]
> Dans Windows Server 2016 et 2012, le nombre de principaux de sécurité qui peuvent être alloués augmente jusqu’à 2 milliards. Pour plus d’informations, consultez Gestion de l' [émission RID](https://technet.microsoft.com/library/jj574229.aspx). 
  
- Exemple de valeur : 4611686014132422708  
- Partie basse : 2100 (début du pool RID suivant à allouer)  
- Partie supérieure : 1073741823 (nombre total d’identificateurs RID pouvant être créés dans un domaine)  
  
Lorsque vous augmentez la valeur de l’entier long, vous augmentez la valeur de la partie inférieure. Par exemple, si vous ajoutez 100 000 à la valeur échantillon de 4611686014132422708 pour une somme de 4611686014132522708, la nouvelle partie basse est 102100. Cela indique que le prochain pool RID qui sera alloué par le maître RID commencera par 102100 au lieu de 2100. 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-adsiedit-and-the-calculator"></a>Pour élever la valeur des pools RID disponibles à l’aide d’Adsiedit et de la calculatrice

1. Ouvrez Gestionnaire de serveur, cliquez sur **Outils** , puis sur **Éditeur ADSI**.
2. Cliquez avec le bouton droit sur, sélectionnez **se connecter à** et connectez pour faire le contexte d’appellation par défaut, puis cliquez sur **OK**.
   ![l’Éditeur ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi1.png) 
3. Accédez au chemin d’accès du nom unique suivant : **CN = gestionnaire RID $, CN = System, DC =<domain name>** .
   ![l’Éditeur ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi2.png) 
3. Cliquez avec le bouton droit et sélectionnez les propriétés CN = RID Manager $. 
4. Sélectionnez l’attribut **rIDAvailablePool**, cliquez sur **modifier**, puis copiez la valeur de l’entier long dans le presse-papiers.
   ![l’Éditeur ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi3.png)  
5. Démarrez la calculatrice et, dans le menu **affichage** , sélectionnez **mode scientifique**. 
6. Ajoutez 100 000 à la valeur actuelle.
   ![l’Éditeur ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi4.png) 
7. À l’aide de Ctrl-c ou de la commande **copier** du menu **Edition** , copiez la valeur dans le presse-papiers. 
8. Dans la boîte de dialogue modifier de ADSIEdit, collez cette nouvelle valeur. 
   ![l’Éditeur ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi5.png) 
9. Cliquez sur **OK** dans la boîte de dialogue, puis **appliquez** dans la feuille de propriétés pour mettre à jour l’attribut **rIDAvailablePool** . 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-ldp"></a>Pour augmenter la valeur des pools RID disponibles à l’aide de LDP  
  
1. À l'invite de commandes, tapez la commande suivante, puis appuyez sur ENTRÉE :  
   **LDP**  
2. Cliquez sur **connexion**, **sur connexion**, tapez le nom du gestionnaire RID, puis cliquez sur **OK**. 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp1.png)
3. Cliquez sur **connexion**, sur **lier**, sélectionnez **lier avec les informations d’identification** , tapez vos informations d’identification d’administration, puis cliquez sur **OK**. 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp2.png)
4. Cliquez sur **affichage**, sur **arborescence** , puis tapez le chemin d’accès de nom unique suivant : CN = gestionnaire RID $, CN = System, DC =*nom de domaine*  
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp3.png)
5. Cliquez sur **Parcourir**, puis sur **modifier**. 
6. Ajoutez 100 000 à la valeur **rIDAvailablePool** actuelle, puis tapez la somme dans **valeurs**. 
7. Dans **DN**, tapez `cn=RID Manager$,cn=System,dc=` *< nom de domaine\>* . 
8. Dans **attribut de modification d’entrée**, tapez `rIDAvailablePool`. 
9. Sélectionnez **remplacer** comme opération, puis cliquez sur **entrée**.
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp4.png) 
10. Cliquez sur **exécuter** pour exécuter l’opération. Cliquez sur **Fermer**.
11. Pour valider la modification, cliquez sur **affichage**, sur **arborescence**, puis tapez le chemin d’accès de nom unique suivant : CN = gestionnaire RID $, CN = System, DC =*nom de domaine*.   Vérifiez l’attribut **rIDAvailablePool** . 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp5.png)

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)
