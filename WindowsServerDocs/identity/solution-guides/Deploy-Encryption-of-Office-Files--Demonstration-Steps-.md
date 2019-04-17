---
ms.assetid: 2c76e81a-c2eb-439f-a89f-7d3d70790244
title: "Déployer le chiffrement des fichiers Office (étapes de démonstration)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 529000c60a80ee33fc2aa7d09370d8ac1e06311c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="deploy-encryption-of-office-files-demonstration-steps"></a>Déployer le chiffrement des fichiers Office (étapes de démonstration)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Service Finance de Contoso a un nombre de serveurs de fichiers qui stockent leurs documents. Ces documents peuvent être documentation générale ou ils peuvent avoir un fort impact commercial (HBI). Par exemple, tout document qui contient des informations confidentielles est considéré comme, par Contoso ont un fort impact commercial. Contoso souhaite s’assurer que toute sa documentation dispose d’une quantité minimale de protection et que sa documentation HBI est limitée aux personnes appropriées. Pour ce faire, Contoso envisage d’utiliser l’Infrastructure de Classification de fichiers (ICF) et les services ADRMS qui est disponible dans Windows Server2012. À l’aide de l’ICF, Contoso est classer tous les documents sur ses serveurs de fichiers, en fonction du contenu et ensuite utiliser les services ADRMS pour appliquer la stratégie des droits appropriés.  
  
Dans ce scénario, vous allez effectuer les étapes suivantes:  
  
|Tâche|Description|  
|--------|---------------|  
|[Activer les propriétés de ressource](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_1.1)|Activer la **Impact** et **informations d’identification personnelle** propriétés de ressource.|  
|[Créer des règles de classification](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_2)|Créer les règles de classification suivantes: **règle de Classification HBI** et **règle de Classification IIP **.|  
|[Utiliser des tâches de gestion de fichiers pour protéger automatiquement les documents avec les services ADRMS](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_3)|Créer une tâche de gestion de fichiers qui utilise automatiquement les services ADRMS pour protéger les documents avec des informations d’identification personnelle (IIP). Seuls les membres du groupe FinanceAdmin auront accès aux documents qui contiennent le niveau IIP élevé.|  
|[Afficher les résultats](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_4)|Examiner la classification des documents et observer les changements à mesure que vous modifiez le contenu dans le document. Vérifier également comment les documents sont protégés par les services ADRMS.|  
|[Vérifier la protection ADRMS](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_5)|Vérifiez que le document est protégé avec les services ADRMS.|  
|||  
  
## <a name="BKMK_1.1"></a>Étape1: Activer les propriétés de ressource  
  
#### <a name="to-enable-resource-properties"></a>Pour activer les propriétés de ressource  
  
1.  Dans le Gestionnaire Hyper-V, connectez-vous au serveur ID_AD_DC1. Connectez-vous au serveur avec le compte contoso\administrateur avec le mot de passe **pass@word1**.  
  
2.  Ouvrir le centre d’administration ActiveDirectory, puis cliquez sur **arborescence **.  
  
3.  Développez **contrôle d’accès dynamique**, puis sélectionnez **propriétés de ressource **.  
  
4.  Faites défiler jusqu'à la **Impact** propriété dans le **nom d’affichage** colonne. Avec le bouton droit **Impact**, puis cliquez sur **activer **.  
  
5.  Faites défiler jusqu'à la **informations d’identification personnelle** propriété dans le **nom d’affichage** colonne. Avec le bouton droit **informations d’identification personnelle**, puis cliquez sur **activer **.  
  
6.  Pour publier les propriétés de ressource dans le **liste de ressources globales**, dans le volet gauche, cliquez sur **propriété répertorie les ressources**, puis double-cliquez sur **liste de propriété de ressource globale **.  
  
7.  Cliquez sur **ajouter**, puis faites défiler vers le bas et cliquez sur **Impact** pour l’ajouter à la liste. Faites de même pour **informations d’identification personnelle **. Cliquez sur **OK** deux fois pour terminer.  
  
![guides de solutions](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com"  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com" 
```  
  
## <a name="BKMK_2"></a>Étape2: Créer des règles de classification  
Cette étape explique comment créer le **fort Impact** règle de classification. Cette règle recherche le contenu des documents et si la chaîne «Confidentiel Contoso» est détectée, elle classifie ce document comme ayant fort impact commercial. Cette classification remplace toute classification de faible impact commercial déjà attribuée.  
  
Vous allez également créer un **niveau IIP élevé** règle. Cette règle recherche le contenu des documents, et si un numéro de sécurité sociale est trouvé, elle classifie ce document comme ayant le niveau IIP élevé.  
  
#### <a name="to-create-the-high-impact-classification-rule"></a>Pour créer la règle de classification de fort impact  
  
1.  Dans le Gestionnaire Hyper-V, connectez-vous au serveur ID_AD_FILE1. Connectez-vous au serveur avec le compte contoso\administrateur avec le mot de passe **pass@word1**.  
  
2.  Vous devez actualiser les propriétés de ressource globales à partir d’ActiveDirectory. Ouvrez Windows PowerShell et tapez: `Update-FSRMClassificationPropertyDefinition`, puis appuyez sur ENTRÉE. Fermez Windows PowerShell.  
  
3.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Pour ouvrir le Gestionnaire de ressources du serveur de fichiers, cliquez sur **Démarrer**, type **FSRM**, puis cliquez sur **File Server Resource Manager**.  
  
4.  Dans le volet gauche du Gestionnaire de ressources du serveur de fichiers, développez **gestion de la Classification**, puis sélectionnez **des règles de Classification **.  
  
5.  Dans le **Actions** volet, cliquez sur **configurer la planification de Classification **. Sur le **Classification automatique** onglet, sélectionnez **activer la planification fixe**, sélectionnez un **jour de la semaine**, puis sélectionnez le **autoriser la classification continue de nouveaux fichiers** case à cocher. Cliquez sur **OK**.  
  
6.  Dans le **Actions** volet, cliquez sur **créer une règle de Classification **. Cela ouvre le **créer une règle de Classification** boîte de dialogue.  
  
7.  Dans le **nom de la règle**, tapez **fort Impact commercial **.  
  
8.  Dans le **Description**, tapez **détermine si le document a un fort impact commercial en fonction de la présence de la chaîne «Confidentiel Contoso»**  
  
9. Sur le **étendue**, cliquez sur **définir les propriétés de gestion des dossiers**, sélectionnez **utilisation du dossier**, cliquez sur **ajouter**, puis cliquez sur **Parcourir**, accédez à D:\Finance Documents en tant que le chemin d’accès, cliquez sur **OK**, puis choisissez une valeur de propriété nommée **fichiers de groupe** et cliquez sur **fermer **. Une fois que les propriétés de gestion sont définies sous le **étendue de la règle** onglet sélectionnez **fichiers de groupe **.  
  
10. Cliquez sur le **Classification** onglet.  Sous **choisir une méthode pour attribuer une propriété aux fichiers**, sélectionnez **classificateur de contenu** dans la liste déroulante.  
  
11. Sous **choisissez une propriété à attribuer aux fichiers**, sélectionnez **Impact** dans la liste déroulante.  
  
12. Sous **spécifier une valeur**, sélectionnez **haute** dans la liste déroulante.  
  
13. Cliquez sur **configurer** sous **paramètres **.  Dans le **paramètres de Classification** la boîte de dialogue dans le **Type d’Expression** liste, sélectionnez **chaîne **. Dans le **Expression**, tapez: **confidentiel Contoso**, puis cliquez sur **OK **.  
  
14. Cliquez sur le **Type d’évaluation** onglet.  Cliquez sur **réévaluer les valeurs de propriété existantes**, cliquez sur **remplacer**existant de la valeur, puis cliquez sur **OK** pour terminer.  
  
![guides de solutions](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Update-FSRMClassificationPropertyDefinition  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name "High Business Impact" -Property "Impact_MS" -Description "Determines if the document has a high business impact based on the presence of the string 'Contoso Confidential'" -PropertyValue "3000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
#### <a name="to-create-the-high-pii-classification-rule"></a>Pour créer la règle de classification de niveau IIP élevé  
  
1.  Dans le Gestionnaire Hyper-V, connectez-vous au serveur ID_AD_FILE1. Connectez-vous au serveur avec le compte contoso\administrateur avec le mot de passe **pass@word1**.  
  
2.  Sur le bureau, ouvrez le dossier nommé **des Expressions régulières**, puis ouvrez le document texte nommé **RegEx-SSN **. Sélectionnez et copiez la chaîne d’expression régulière suivante: **^ (? 000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]?) (?! 00) \d\d\3 (?! 0000) \d {4} $**. Cette chaîne sera utilisée ultérieurement dans cette étape conservez-la dans votre Presse-papiers.  
  
3.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Pour ouvrir le Gestionnaire de ressources du serveur de fichiers, cliquez sur **Démarrer**, type **FSRM**, puis cliquez sur **File Server Resource Manager**.  
  
4.  Dans le volet gauche du Gestionnaire de ressources du serveur de fichiers, développez **gestion de la Classification**, puis sélectionnez **des règles de Classification **.  
  
5.  Dans le **Actions** volet, cliquez sur **configurer la planification de Classification **. Sur le **Classification automatique** onglet, sélectionnez **activer la planification fixe**, sélectionnez un **jour de la semaine**, puis sélectionnez le **autoriser la classification continue de nouveaux fichiers** case à cocher. Cliquez sur OK.  
  
6.  Dans le **nom de la règle**, tapez **niveau IIP élevé **. Dans le **Description**, tapez **détermine si le document a un haut niveau IIP basée sur la présence d’un numéro de sécurité sociale.**  
  
7.  Cliquez sur le **étendue** onglet, sélectionnez le **fichiers de groupe** case à cocher.  
  
8.  Cliquez sur le **Classification** onglet.  Sous **choisir une méthode pour attribuer une propriété aux fichiers**, sélectionnez **classificateur de contenu** dans la liste déroulante.  
  
9. Sous **choisissez une propriété à attribuer aux fichiers**, sélectionnez **informations d’identification personnelle** dans la liste déroulante.  
  
10. Sous **spécifier une valeur**, sélectionnez **haute** dans la liste déroulante.  
  
11. Cliquez sur **configurer** sous **paramètres **.   
    Dans le **paramètres de Classification**fenêtre, dans le **Type d’Expression** liste, sélectionnez **une Expression régulière **. Dans le **Expression** zone, collez le texte à partir de votre Presse-papiers: **^ (? 000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]?) (?! 00) \d\d\3 (?! 0000) \d {4} $**, puis cliquez sur **OK **.  
  
    > [!NOTE]  
    > Cette expression autorise les numéros de sécurité sociale non valides. Cela nous permet d’utiliser des numéros de sécurité sociale fictifs dans la démonstration.  
  
12. Cliquez sur le **Type d’évaluation** onglet.  Sélectionnez **réévaluer les valeurs de propriété existantes**, **remplacer**existant de la valeur, puis cliquez sur **OK** pour terminer.  
  
![guides de solutions](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
New-FSRMClassificationRule -Name "High PII" -Description "Determines if the document has a high PII based on the presence of a Social Security Number." -Property "PII_MS" -PropertyValue "5000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=1;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
Vous devez maintenant avoir deux règles de classification:  
  
-   Fort Impact commercial  
  
-   Niveau IIP élevé  
  
## <a name="BKMK_3"></a>Étape3: Utiliser des tâches de gestion de fichiers pour protéger automatiquement les documents avec les services ADRMS  
Maintenant que vous avez créé des règles pour classifier automatiquement les documents en fonction du contenu, l’étape suivante consiste à créer une tâche de gestion de fichiers qui utilise les services ADRMS pour protéger automatiquement certains documents en fonction de leur classification. Dans cette étape, vous allez créer une tâche de gestion de fichiers qui protège automatiquement tout document ayant un niveau IIP élevé. Seuls les membres du groupe FinanceAdmin auront accès aux documents qui contiennent le niveau IIP élevé.  
  
#### <a name="to-protect-documents-with-ad-rms"></a>Pour protéger les documents avec les services ADRMS  
  
1.  Dans le Gestionnaire Hyper-V, connectez-vous au serveur ID_AD_FILE1. Connectez-vous au serveur avec le compte contoso\administrateur avec le mot de passe **pass@word1**.  
  
2.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Pour ouvrir le Gestionnaire de ressources du serveur de fichiers, cliquez sur **Démarrer**, type **FSRM**, puis cliquez sur **File Server Resource Manager**.  
  
3.  Dans le volet gauche, sélectionnez **tâches de gestion de fichiers **. Dans le **Actions** volet, sélectionnez **créer une tâche de gestion de fichiers **.  
  
4.  Dans le **nom de la tâche:**, tapez **niveau IIP élevé **. Dans le **Description**, tapez **protection RMS automatique des documents de niveau IIP élevés **.  
  
5.  Cliquez sur le **étendue** onglet, sélectionnez le **fichiers de groupe** case à cocher.  
  
6.  Cliquez sur le **Action** onglet. Sous **Type**, sélectionnez **chiffrement RMS **. Cliquez sur **Parcourir** pour sélectionner un modèle, puis sélectionnez le **Contoso Finance Admin uniquement** modèle.  
  
7.  Cliquez sur le **Condition** onglet, puis cliquez sur **ajouter **. Sous **propriété**, sélectionnez **informations d’identification personnelle **. Sous **opérateur**, sélectionnez **égale **. Sous **valeur**, sélectionnez **haute **. Cliquez sur **OK**.  
  
8.  Cliquez sur le **calendrier** onglet. Dans le **calendrier**, cliquez sur **hebdomadaire**, puis sélectionnez **dimanche **. Exécution de la tâche une fois une semaine garantit permet d’intercepter tout document qui ont peut-être été manqués en raison d’une interruption de service ou autre événement perturbateur.  
  
9. Dans le **fonctionnement continu** section, sélectionnez **exécuter la tâche en continu sur les nouveaux fichiers**, puis cliquez sur **OK **. Vous devez maintenant avoir une tâche de gestion de fichiers nommée niveau IIP élevé.  
  
![guides de solutions](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
$fmjRmsEncryption = New-FSRMFmjAction -Type 'Rms' -RmsTemplate 'Contoso Finance Admin Only'  
$fmjCondition1 = New-FSRMFmjCondition -Property 'PII_MS' -Condition 'Equal' -Value '5000'  
$date = get-date  
$schedule = New-FsrmScheduledTask -Time $date -Weekly @('Sunday')    
$fmj1=New-FSRMFileManagementJob -Name "High PII" -Description "Automatic RMS protection for high PII documents" -Namespace @('D:\Finance Documents') -Action $fmjRmsEncryption -Schedule $schedule -Continuous -Condition @($fmjCondition1)  
```  
  
## <a name="BKMK_4"></a>Étape4: Afficher les résultats  
Il est maintenant un aperçu de votre nouvelle classification automatique et les règles de protection ADRMS en action. Dans cette étape vous examiner la classification des documents et observer les changements à mesure que vous modifiez le contenu dans le document.  
  
#### <a name="to-view-the-results"></a>Pour afficher les résultats  
  
1.  Dans le Gestionnaire Hyper-V, connectez-vous au serveur ID_AD_FILE1. Connectez-vous au serveur avec le compte contoso\administrateur avec le mot de passe **pass@word1**.  
  
2.  Dans l’Explorateur Windows, accédez à D:\Finance Documents.  
  
3.  Cliquez sur le document Finance Memo et cliquez sur **propriétés**. Cliquez sur le **Classification** onglet et notez que la propriété Impact a actuellement aucune valeur. Cliquez sur **Annuler **.  
  
4.  Avec le bouton droit le **Request for Approval to Hire document**, puis sélectionnez **propriétés **.  
  
5.  Cliquez sur le **Classification** onglet et notez que **la propriété d’informations d’identification personnelle** n’a aucune valeur actuellement. Cliquez sur **Annuler **.  
  
6.  Basculez vers CLIENT1. Déconnectez tout utilisateur qui est connecté dans, puis connectez-vous en tant que Contoso\MReid avec le mot de passe **pass@word1**.  
  
7.  À partir du bureau, ouvrez le **Documents financiers** dossier partagé.  
  
8.  Ouvrez le **Finance Memo** document. Au bas du document, vous verrez le mot **confidentiel **. Remplacez-le: **confidentiel Contoso **. Enregistrer le document et fermez-le.  
  
9. Ouvrez le **Request for Approval to Hire** document. Dans le **Social Security #:**, tapez: 777-77-7777. Enregistrer le document et fermez-le.  
  
    > [!NOTE]  
    > Vous devrez peut-être patienter 30secondes pour la classification ait lieu.  
  
10. Revenez à ID_AD_FILE1. Dans l’Explorateur Windows, accédez à D:\Finance Documents.  
  
11. Cliquez sur le document Finance Memo, puis cliquez sur **propriétés **. Cliquez sur le **Classification** onglet. Notez que le **Impact** propriété a maintenant la valeur **haute **. Cliquez sur **Annuler **.  
  
12. Avec le bouton droit de la demande d’approbation à Hire document et cliquez sur **propriétés **.  
  
13. . Cliquez sur le **Classification** onglet. Notez que le **informations d’identification personnelle** propriété a maintenant la valeur **haute **. Cliquez sur **Annuler **.  
  
## <a name="BKMK_5"></a>Étape5: Vérifier la protection avec les services ADRMS  
  
#### <a name="to-verify-that-the-document-is-protected"></a>Pour vérifier que le document est protégé.  
  
1.  Revenez à ID_AD_CLIENT1.  
  
2.  Ouvrez le **demande d’approbation d’embauche** document.  
  
3.  Cliquez sur **OK** pour permettre au document pour vous connecter à votre serveur ADRMS.  
  
4.  Vous pouvez maintenant voir que le document a été protégé par les services ADRMS car il contient un numéro de sécurité sociale.  
  

