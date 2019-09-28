---
ms.assetid: 2c76e81a-c2eb-439f-a89f-7d3d70790244
title: Déployer le chiffrement des fichiers Office (étapes de démonstration)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 05da1b7df2e3242c9b68bd7858c824f91e81a563
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407100"
---
# <a name="deploy-encryption-of-office-files-demonstration-steps"></a>Déployer le chiffrement des fichiers Office (étapes de démonstration)

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le service finance de Contoso a un certain nombre de serveurs de fichiers qui stockent leurs documents. Il peut s'agir de documentation générale ou de documents à fort impact commercial (HBI, High-Business Impact). Par exemple, tout document qui contient des informations confidentielles est considéré par Contoso comme ayant un fort impact commercial. Contoso souhaite s'assurer que toute sa documentation dispose d'un niveau de protection minimal et que l'accès à sa documentation HBI est limité aux personnel autorisé. Pour ce faire, contoso explore à l’aide des Infrastructure de classification des fichiers (FCI) et AD RMS disponibles dans Windows Server 2012. L'Infrastructure de classification des fichiers permettra à Contoso de classifier tous les documents sur ses serveurs de fichiers en fonction du contenu, puis d'utiliser les services AD RMS pour appliquer la stratégie de droits appropriée.  
  
Dans ce scénario, vous allez effectuer les étapes suivantes :  
  
|Tâche|Description|  
|--------|---------------|  
|[Activer les propriétés de ressource](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_1.1)|Activer les propriétés de ressources **Impact** et **Informations d'identification personnelle** .|  
|[Créer des règles de classification](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_2)|Créer les règles de classification suivantes : **Règle de classification HBI** et **Règle de classification IIP**.|  
|[Utilisez des tâches de gestion de fichiers pour protéger automatiquement les documents avec AD RMS](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_3)|Créer une tâche de gestion de fichiers qui utilise automatiquement les services AD RMS pour protéger les documents contenant des informations d'identification personnelle (IIP). Seuls les membres du groupe FinanceAdmin auront accès aux documents qui contiennent des informations d'identification personnelle.|  
|[Afficher les résultats](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_4)|Examiner la classification des documents et observer les changements à mesure que vous modifiez le contenu des documents. Vérifier également comment les documents sont protégés par les services AD RMS.|  
|[Vérifier AD RMS protection](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_5)|Vérifier que les documents sont protégés par les services AD RMS.|  
|||  
  
## <a name="BKMK_1.1"></a>Étape 1 : Activer les propriétés de ressource  
  
#### <a name="to-enable-resource-properties"></a>Pour activer les propriétés de ressource  
  
1. Dans le Gestionnaire Hyper-V, connectez-vous au serveur ID_AD_DC1. Connectez-vous au serveur à l’aide de CONTOSO\Administrateur avec le mot de passe <strong>pass@word1</strong>.  
  
2. Ouvrez le Centre d'administration Active Directory et cliquez sur **Arborescence**.  
  
3. Développez **Contrôle d'accès dynamique**et sélectionnez **Propriétés de ressource**.  
  
4. Faites défiler la liste jusqu'à atteindre la propriété **Impact** dans la colonne **Nom complet**. Cliquez avec le bouton droit sur **Impact**, puis cliquez sur **Activer**.  
  
5. Faites défiler la liste jusqu'à atteindre la propriété **Informations d'identification personnelle** dans la colonne **Nom complet**. Cliquez avec le bouton droit sur **Informations d'identification personnelle**, puis cliquez sur **Activer**.  
  
6. Pour publier les propriétés de ressource dans la **Liste de ressources globales**, dans le volet gauche, cliquez sur **Liste de propriétés de ressources**, puis double-cliquez sur **Liste de propriétés de ressource globales**.  
  
7. Cliquez sur **Ajouter**, puis accédez à **Impact** et cliquez dessus pour l'ajouter à la liste. Faites de même pour **Informations d'identification personnelle**. Cliquez deux fois sur **OK** pour terminer.  
  
@no__t-guides 0solution-](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com"  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com" 
```  
  
## <a name="BKMK_2"></a>Étape 2 : Créer des règles de classification  
Cette étape explique comment créer la règle de classification **Fort impact** . Cette règle recherche le contenu des documents et, si la chaîne « contoso Confidential » est trouvée, elle classifie ce document comme ayant un impact élevé sur l’activité. Cette classification remplace toute classification de faible impact commercial assignée précédemment.  
  
Vous allez aussi créer une règle **Niveau IIP élevé** . Cette règle analyse aussi le contenu des documents et, si un numéro de sécurité social est détecté, elle classifie ce document comme ayant un niveau IIP élevé.  
  
#### <a name="to-create-the-high-impact-classification-rule"></a>Pour créer la règle de classification de fort impact  
  
1. Dans le Gestionnaire Hyper-V, connectez-vous au serveur ID_AD_FILE1. Connectez-vous au serveur à l’aide de CONTOSO\Administrateur avec le mot de passe <strong>pass@word1</strong>.  
  
2. Vous devez actualiser les propriétés de ressource globales à partir d'Active Directory. Ouvrez Windows PowerShell, tapez `Update-FSRMClassificationPropertyDefinition`, puis appuyez sur ENTRÉE. Fermez Windows PowerShell.  
  
3. Ouvrez le Gestionnaire de ressources du serveur de fichiers. Pour ouvrir le Gestionnaire de ressources du serveur de fichiers, cliquez sur **Démarrer**, tapez **gestionnaire de ressources du serveur de fichiers**, puis cliquez sur **Gestionnaire de ressources du serveur de fichiers**.  
  
4. Dans le volet gauche du Gestionnaire de ressources du serveur de fichiers, développez **Gestion de la classification** et sélectionnez **Règle de classification**.  
  
5. Dans le volet **Actions**, cliquez sur **Configurer la planification de la classification**. Sous l'onglet **Classification automatique**, sélectionnez **Activer la planification fixe**, sélectionnez un **Jour de la semaine**, puis cochez la case **Autoriser la classification continue de nouveaux fichiers**. Cliquez sur **OK**.  
  
6. Dans le volet **Actions** , cliquez sur **Créer une règle de classification**. La boîte de dialogue **Créer une règle de classification** s’affiche.  
  
7. Dans la zone **Nom de la règle**, tapez **Fort impact commercial**.  
  
8. Dans la zone **Description** , tapez **détermine si le document a un impact élevé sur l’entreprise en fonction de la présence de la chaîne « contoso Confidential »** .  
  
9. Sous l'onglet **Étendue** , cliquez sur **Set Folder Management Properties**, sélectionnez **Folder Usage**, cliquez sur **Ajouter**, puis sur **Parcourir**, accédez au chemin d'accès D:\Finance Documents, cliquez sur **OK**, puis choisissez une valeur de propriété nommée **Fichiers de groupe** et cliquez sur **Fermer**. Une fois les propriétés de gestion définies, sous l'onglet **Étendue de la règle** , sélectionnez **Fichiers de groupe**.  
  
10. Cliquez sur l'onglet **Classification**.  Sous Choisissez une méthode pour attribuer une propriété aux fichiers, sélectionnez **Classifieur de contenus** dans la liste déroulante.  
  
11. Sous **Choisissez une propriété à attribuer aux fichiers**, sélectionnez **Impact** dans la liste déroulante.  
  
12. Sous **Spécifiez une valeur**, sélectionnez **Élevé** dans la liste déroulante.  
  
13. Cliquez sur **Configurer** sous **Paramètres**.  Dans la boîte de dialogue **Paramètres de classification**, dans la liste **Type d'expression**, sélectionnez **Chaîne**. Dans la zone **Expression**, tapez : Confidentiel Contoso, puis cliquez sur **OK**.  
  
14. Cliquez sur l'onglet **Type d'évaluation**.  Cliquez sur Réévaluer les valeurs de propriété existantes, cliquez sur **Remplacer**la valeur existante, puis cliquez sur **OK** pour terminer.  
  
@no__t-guides 0solution-](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Update-FSRMClassificationPropertyDefinition  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name "High Business Impact" -Property "Impact_MS" -Description "Determines if the document has a high business impact based on the presence of the string 'Contoso Confidential'" -PropertyValue "3000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
#### <a name="to-create-the-high-pii-classification-rule"></a>Pour créer la règle de classification de niveau IIP élevé  
  
1. Dans le Gestionnaire Hyper-V, connectez-vous au serveur ID_AD_FILE1. Connectez-vous au serveur à l’aide de CONTOSO\Administrateur avec le mot de passe <strong>pass@word1</strong>.  
  
2. Sur le Bureau, ouvrez le dossier **Expressions régulières**, puis ouvrez le document texte nommé **RegEx-SSN**. Mettez en surbrillance et copiez la chaîne d’expression régulière suivante : **^ ( ?! 000) ([0-7] \d @ no__t-1 | 7 ([0-7] \d | 7 [012])) ([-] ?) (?! 00) \d\d\3 ( ?! 0000) \d @ no__t-2 $** . Nous utiliserons cette chaîne plus loin lors de cette étape. Conservez-la dans votre Presse-papiers.  
  
3. Ouvrez le Gestionnaire de ressources du serveur de fichiers. Pour ouvrir le Gestionnaire de ressources du serveur de fichiers, cliquez sur **Démarrer**, tapez **gestionnaire de ressources du serveur de fichiers**, puis cliquez sur **Gestionnaire de ressources du serveur de fichiers**.  
  
4. Dans le volet gauche du Gestionnaire de ressources du serveur de fichiers, développez **Gestion de la classification** et sélectionnez **Règle de classification**.  
  
5. Dans le volet **Actions**, cliquez sur **Configurer la planification de la classification**. Sous l'onglet **Classification automatique**, sélectionnez **Activer la planification fixe**, sélectionnez un **Jour de la semaine**, puis cochez la case **Autoriser la classification continue de nouveaux fichiers**. Cliquez sur OK.  
  
6. Dans la zone **Nom de la règle**, tapez **Niveau IIP élevé**. Dans la zone **Description** , tapez **Détermine si le document a un niveau IIP élevé en fonction de la présence d'un numéro de sécurité sociale**.  
  
7. Cliquez sur l'onglet **Étendue** et cochez la case **Fichiers de groupe**.  
  
8. Cliquez sur l'onglet **Classification**.  Sous Choisissez une méthode pour attribuer une propriété aux fichiers, sélectionnez **Classifieur de contenus** dans la liste déroulante.  
  
9. Sous **Choisissez une propriété à attribuer aux fichiers**, sélectionnez **Informations d'identification personnelle** dans la liste déroulante.  
  
10. Sous **Spécifiez une valeur**, sélectionnez **Élevé** dans la liste déroulante.  
  
11. Cliquez sur **Configurer** sous **Paramètres**.   
    Dans le volet **Paramètres de classification**, dans la liste **Type d'expression** , sélectionnez **Expression régulière**. Dans la zone **expression** , collez le texte de votre presse-papiers : **^ ( ?! 000) ([0-7] \d @ no__t-2 | 7 ([0-7] \d | 7 [012])) ([-] ?) (?! 00) \d\d\3 ( ?! 0000) \d @ no__t-3 $** , puis cliquez sur **OK**.  
  
    > [!NOTE]  
    > Cette expression autorise les numéros de sécurité sociale non valides. Cela nous permet d'utiliser des numéros de sécurité sociale fictifs dans la démonstration.  
  
12. Cliquez sur l'onglet **Type d'évaluation**.  Sélectionnez Réévaluer les valeurs de propriété existantes, **Remplacer**la valeur existante, puis cliquez sur **OK** pour terminer.  
  
@no__t-guides 0solution-](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
New-FSRMClassificationRule -Name "High PII" -Description "Determines if the document has a high PII based on the presence of a Social Security Number." -Property "PII_MS" -PropertyValue "5000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=1;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
Vous devez maintenant avoir deux règles de classification :  
  
-   Fort impact commercial  
  
-   Niveau IIP élevé  
  
## <a name="BKMK_3"></a>Étape 3 : Utilisez des tâches de gestion de fichiers pour protéger automatiquement les documents avec AD RMS  
Maintenant que vous avez créé des règles pour classifier automatiquement les documents en fonction du contenu, l’étape suivante consiste à créer une tâche de gestion de fichiers qui utilise AD RMS pour protéger automatiquement certains documents en fonction de leur classification. Lors de cette étape, vous allez créer une tâche de gestion de fichiers qui protège automatiquement tout document ayant un niveau IIP élevé. Seuls les membres du groupe FinanceAdmin auront accès aux documents qui contiennent des informations d'identification personnelle.  
  
#### <a name="to-protect-documents-with-ad-rms"></a>Pour protéger les documents avec les services AD RMS  
  
1. Dans le Gestionnaire Hyper-V, connectez-vous au serveur ID_AD_FILE1. Connectez-vous au serveur à l’aide de CONTOSO\Administrateur avec le mot de passe <strong>pass@word1</strong>.  
  
2. Ouvrez le Gestionnaire de ressources du serveur de fichiers. Pour ouvrir le Gestionnaire de ressources du serveur de fichiers, cliquez sur **Démarrer**, tapez **gestionnaire de ressources du serveur de fichiers**, puis cliquez sur **Gestionnaire de ressources du serveur de fichiers**.  
  
3. Dans le volet gauche, sélectionnez **Tâches de gestion de fichiers**. Dans le volet **Actions**, sélectionnez **Créer une tâche de gestion de fichiers**.  
  
4. Dans le champ **Nom de la tâche**, tapez **Niveau IIP élevé**. Dans le champ **Description**, tapez **Protection RMS automatique des documents ayant un niveau IIP élevé**.  
  
5. Cliquez sur l'onglet **Étendue** et cochez la case **Fichiers de groupe**.  
  
6. Cliquez sur l'onglet **Action**. Sous Type, sélectionnez **Chiffrement RMS**. Cliquez sur **Parcourir** pour sélectionner un modèle, puis sélectionnez le modèle **Contoso Finance Admin uniquement** .  
  
7. Cliquez sur l'onglet **Condition**, puis sur **Ajouter**. Sous **Propriété**, sélectionnez **Informations d'identification personnelle**. Sous **Opérateur**, sélectionnez **Égal**. Sous **Valeur**, sélectionnez **Élevé**. Cliquez sur **OK**.  
  
8. Cliquez sur l'onglet **Planification**. Dans la section Planification , cliquez sur **Toutes les semaines**, puis sélectionnez **Dimanche**. L'exécution hebdomadaire de la tâche permet d'intercepter tout document qui pourrait avoir été ignoré pour cause de panne de service ou autre événement perturbateur.  
  
9. Dans la section **Opération continue**, sélectionnez **Exécuter la tâche en continu sur les nouveaux fichiers**, puis cliquez sur **OK**. Vous devez maintenant avoir une tâche de gestion de fichiers nommée Niveau IIP élevé.  
  
@no__t-guides 0solution-](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
$fmjRmsEncryption = New-FSRMFmjAction -Type 'Rms' -RmsTemplate 'Contoso Finance Admin Only'  
$fmjCondition1 = New-FSRMFmjCondition -Property 'PII_MS' -Condition 'Equal' -Value '5000'  
$date = get-date  
$schedule = New-FsrmScheduledTask -Time $date -Weekly @('Sunday')    
$fmj1=New-FSRMFileManagementJob -Name "High PII" -Description "Automatic RMS protection for high PII documents" -Namespace @('D:\Finance Documents') -Action $fmjRmsEncryption -Schedule $schedule -Continuous -Condition @($fmjCondition1)  
```  
  
## <a name="BKMK_4"></a>Étape 4 : Afficher les résultats  
Il est temps de jeter un coup d’œil à vos nouvelles règles de classification automatique et de AD RMS de protection en action. Lors de cette étape, vous allez examiner la classification des documents et observer les changements à mesure que vous modifiez le contenu des documents.  
  
#### <a name="to-view-the-results"></a>Pour afficher les résultats  
  
1. Dans le Gestionnaire Hyper-V, connectez-vous au serveur ID_AD_FILE1. Connectez-vous au serveur à l’aide de CONTOSO\Administrateur avec le mot de passe <strong>pass@word1</strong>.  
  
2. Dans l'Explorateur Windows, accédez à D:\Finance Documents.  
  
3. Cliquez avec le bouton droit sur le document Finance Memo et cliquez sur **Propriétés**. Cliquez sur l'onglet **Classification** et notez que la propriété Impact n'a pas de valeur actuellement. Cliquez sur **Annuler**.  
  
4. Cliquez avec le bouton droit sur le document **Request for Approval to Hire**, puis sélectionnez **Propriétés**.  
  
5. Cliquez sur l'onglet **Classification** et notez que la propriété **Informations d'identification personnelle** n'a pas de valeur actuellement. Cliquez sur **Annuler**.  
  
6. Basculez vers CLIENT1. Déconnectez tout utilisateur connecté, puis connectez-vous en tant que Contoso\MReid avec le mot de passe <strong>pass@word1</strong>.  
  
7. Sur le Bureau, ouvrez le dossier partagé **Finance Documents** .  
  
8. Ouvrez le document **Finance Memo** . Près du bas du document figure le mot **Confidential**. Remplacez-le par : **Confidentiel Contoso**. Enregistrez le document et fermez-le.  
  
9. Ouvrez le document **Request for Approval to Hire**. Dans la section **Social Security#:** , tapez : 777-77-7777. Enregistrez le document et fermez-le.  
  
    > [!NOTE]  
    > Vous devrez peut-être patienter 30 secondes avant que la classification ait lieu.  
  
10. Revenez à ID_AD_FILE1. Dans l'Explorateur Windows, accédez à D:\Finance Documents.  
  
11. Cliquez avec le bouton droit sur le document Finance Memo, puis cliquez sur **Propriétés**. Cliquez sur l'onglet **Classification**. Notez que la propriété Impact a maintenant la valeur **Élevé**. Cliquez sur **Annuler**.  
  
12. Cliquez avec le bouton droit sur le document Request for Approval to Hire et sélectionnez **Propriétés**.  
  
13. . Cliquez sur l'onglet **Classification**. Notez que la propriété Informations d'identification personnelle a maintenant la valeur **Élevé**. Cliquez sur **Annuler**.  
  
## <a name="BKMK_5"></a>Étape 5 : Vérifier la protection avec AD RMS  
  
#### <a name="to-verify-that-the-document-is-protected"></a>Pour vérifier que le document est protégé  
  
1.  Revenez à ID_AD_CLIENT1.  
  
2.  Ouvrez le document **Request for Approval to Hire**.  
  
3.  Cliquez sur **OK** pour permettre au document de se connecter à votre serveur AD RMS.  
  
4.  Vous pouvez maintenant constater que le document a été protégé par les services AD RMS car il contient un numéro de sécurité sociale.  
  

