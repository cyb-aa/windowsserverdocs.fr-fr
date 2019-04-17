---
ms.assetid: 01988844-df02-4952-8535-c87aefd8a38a
title: "Déployer la Classification automatique des fichiers (étapes de démonstration)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1c5c0fa221e0d7375216426f838ba37bee852984
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-automatic-file-classification-demonstration-steps"></a>Déployer la Classification automatique des fichiers (étapes de démonstration)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique explique comment activer les propriétés de ressource dans ActiveDirectory, créer des règles de classification sur le serveur de fichiers et puis assigner des valeurs aux propriétés de ressource pour les fichiers sur le serveur de fichiers. Pour cet exemple, les règles de classification suivantes sont créés:  
  
-   Une règle de classification de contenu qui recherche un ensemble de fichiers pour la chaîne «Confidentiel Contoso.» Si la chaîne est détectée dans un fichier, la propriété de ressource Impact a la valeur haute sur le fichier.  
  
-   Une règle de classification de contenu qui recherche un ensemble de fichiers pour une expression régulière qui correspond à un numéro de sécurité sociale au moins 10fois dans un seul fichier. Si le modèle est trouvé, le fichier est classifié comme comportant des informations personnellement identifiables, et la propriété de ressource informations d’identification personnelle est définie sur haute.  
  
**Dans ce document**  
  
-   [Étape1: Créer des définitions de propriété de ressource](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [Étape2: Créer une règle de classification de contenu de chaîne](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step2)  
  
-   [Étape3: Créer une règle de classification de contenu d’expression régulière](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [Étape4: Vérifier que les fichiers sont classifiés](Deploy-Automatic-File-Classification--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> Cette rubrique inclut des applets de commande exemple Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, voir [applets de commande à l’aide de](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Step1"></a>Étape1: Créer des définitions de propriété de ressource  
Les propriétés de ressource Impact et informations d’identification personnelle sont activées pour que l’Infrastructure de Classification des fichiers peut utiliser ces propriétés de ressource pour baliser les fichiers analysés dans un dossier réseau partagé.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>Pour créer des définitions de propriétés de ressource  
  
1.  Sur le contrôleur de domaine, connectez-vous au serveur en tant que membre du groupe de sécurité Admins du domaine.  
  
2.  Ouvrir le centre d’administration ActiveDirectory. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **centre d’administration Active Directory**.  
  
3.  Développez **contrôle d’accès dynamique**, puis cliquez sur **propriétés de ressource**.  
  
4.  Avec le bouton droit **Impact**, puis cliquez sur **activer **.  
  
5.  Avec le bouton droit **informations d’identification personnelle**, puis cliquez sur **activer **.  
  
![guides de solutions](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'   
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>Étape2: Créer une règle de classification de contenu de chaîne  
Une règle de classification de contenu de chaîne analyse un fichier d’une chaîne spécifique. Si la chaîne est détectée, la valeur d’une propriété de ressource peut être configurée. Dans cet exemple, nous analyser chaque fichier dans un dossier réseau partagé et recherchez la chaîne «Confidentiel Contoso.» Si la chaîne est trouvée, le fichier associé est classifié comme ayant un fort impact commercial.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-create-a-string-content-classification-rule"></a>Pour créer une règle de classification de contenu de chaîne  
  
1.  Ouvrez une session sur le serveur de fichiers en tant que membre du groupe de sécurité Administrateurs.  
  
2.  À partir de l’invite de commandes Windows PowerShell, tapez **Update-FsrmClassificationPropertyDefinition** et appuyez sur ENTRÉE. Cette commande permet de synchroniser les définitions de propriétés créées sur le contrôleur de domaine pour le serveur de fichiers.  
  
3.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **File Server Resource Manager**.  
  
4.  Développez **gestion de la Classification**, avec le bouton droit **des règles de Classification**, puis cliquez sur **configurer la planification de Classification**.  
  
5.  Sélectionnez le **activer la planification fixe** case à cocher, sélectionnez le **autoriser la classification continue de nouveaux fichiers** case à cocher, choisissez un jour de la semaine pour exécuter la classification, puis cliquez sur **OK**.  
  
6.  Avec le bouton droit **des règles de Classification**, puis cliquez sur **créer une règle de Classification**.  
  
7.  Sur le **général** onglet le **nom de la règle**, tapez un nom de la règle comme **confidentiel Contoso**.  
  
8.  Sur le **étendue**, cliquez sur **ajouter**et choisir les dossiers qui doivent être inclus dans cette règle, tels que D:\Finance Documents.  
  
    > [!NOTE]  
    > Vous pouvez également choisir un espace de noms dynamique pour l’étendue. Pour plus d’informations sur les espaces de noms dynamique pour les règles de classification, voir [Nouveautés dans le Gestionnaire de ressources du serveur de fichiers dans Windows Server2012 \[redirected\]](assetId:///d53c603e-6217-4b98-8508-e8e492d16083).  
  
9. Sur le **Classification** onglet, configurez les éléments suivants:  
  
    -   Dans le **choisir une méthode pour attribuer une propriété aux fichiers** zone, vérifiez que **classificateur de contenu** est sélectionné.  
  
    -   Dans le **choisissez une propriété à attribuer aux fichiers**, cliquez sur **Impact**.  
  
    -   Dans le **spécifier une valeur**, cliquez sur **haute**.  
  
10. Sous le **paramètres** en-tête, cliquez sur **configurer**.  
  
11. Dans le **Type d’Expression** colonne, sélectionnez **chaîne**.  
  
12. Dans le **Expression** colonne, tapez **confidentiel Contoso**, puis cliquez sur **OK**.  
  
13. Sur le **Type d’évaluation** onglet, sélectionnez le **réévaluer les valeurs de propriété existantes** case à cocher, cliquez sur **remplacer la valeur existante**, puis cliquez sur **OK**.  
  
![guides de solutions](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;$AutomaticClassificationScheduledTask  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name 'Contoso Confidential' -Property "Impact_MS" -PropertyValue "3000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step3"></a>Étape3: Créer une règle de classification de contenu d’expression régulière  
Une règle de classification d’expression régulière analyse un fichier d’un modèle qui correspond à l’expression régulière. Si une chaîne qui correspond à l’expression régulière est détectée, la valeur d’une propriété de ressource peut être configurée. Dans cet exemple, nous analyser chaque fichier dans un dossier réseau partagé et recherche d’une chaîne qui correspond au modèle d’un numéro de sécurité sociale (XXX-XX-XXXX). Si le modèle est trouvé, le fichier associé est classifié comme comportant des informations personnellement identifiables.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-regular-expression-content-classification-rule"></a>Pour créer une règle de classification de contenu d’expression régulière  
  
1.  Connectez-vous au serveur de fichiers en tant que membre du groupe de sécurité Administrateurs.  
  
2.  À partir de l’invite de commandes Windows PowerShell, tapez **Update-FsrmClassificationPropertyDefinition**, puis appuyez sur ENTRÉE. Cette commande permet de synchroniser les définitions de propriétés qui sont créées sur le contrôleur de domaine pour le serveur de fichiers.  
  
3.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **File Server Resource Manager**.  
  
4.  Avec le bouton droit **des règles de Classification**, puis cliquez sur **créer une règle de Classification**.  
  
5.  Sur le **général** onglet le **nom de la règle**, tapez un nom pour la règle de classification, par exemple règle PII.  
  
6.  Sur le **étendue**, cliquez sur **ajouter**, puis choisissez les dossiers qui doivent être inclus dans cette règle, tels que D:\Finance Documents.  
  
7.  Sur le **Classification** onglet, configurez les éléments suivants:  
  
    -   Dans le **choisir une méthode pour attribuer une propriété aux fichiers** zone, vérifiez que **classificateur de contenu** est sélectionné.  
  
    -   Dans le **choisissez une propriété à attribuer aux fichiers**, cliquez sur **informations d’identification personnelle**.  
  
    -   Dans le **spécifier une valeur**, cliquez sur **haute**.  
  
8.  Sous le **paramètres** en-tête, cliquez sur **configurer**.  
  
9. Dans le **Type d’Expression** colonne, sélectionnez **une expression régulière**.  
  
10. Dans le **Expression** colonne, tapez **^ (? 000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]?) (?! 00) \d\d\3 (?! 0000) \d {4} $**  
  
11. Dans le **Occurrences minimales** colonne, tapez **10**, puis cliquez sur **OK**.  
  
12. Sur le **Type d’évaluation** onglet, sélectionnez le **réévaluer les valeurs de propriété existantes** case à cocher, cliquez sur **remplacer la valeur existante**, puis cliquez sur **OK**.  
  
![guides de solutions](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
New-FSRMClassificationRule -Name "PII Rule" -Property "PII_MS" -PropertyValue "5000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=10;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step4"></a>Étape4: Vérifier que les fichiers sont classifiés correctement  
Vous pouvez vérifier que les fichiers sont classifiés correctement en affichant les propriétés d’un fichier qui a été créé dans le dossier spécifié dans les règles de classification.  
  
#### <a name="to-verify-that-the-files-are-classified-correctly"></a>Pour vérifier que les fichiers sont classifiés correctement  
  
1.  Sur le serveur de fichiers, exécutez les règles de classification à l’aide du Gestionnaire de ressources du serveur de fichiers.  
  
    1.  Cliquez sur **gestion de la Classification**, avec le bouton droit **des règles de Classification**, puis cliquez sur **exécuter la Classification avec toutes les règles maintenant**.  
  
    2.  Cliquez sur le **attendre de classification Terminer** option, puis cliquez sur **OK**.  
  
    3.  Fermez le rapport de Classification automatique.  
  
    4.  Vous pouvez pour cela à l’aide de Windows PowerShell avec la commande suivante: **Start-FSRMClassification» «RunDuration 0 - confirmer: $false**  
  
2.  Accédez au dossier qui a été spécifié dans les règles de classification, tels que D:\Finance Documents.  
  
3.  Cliquez sur un fichier dans ce dossier, puis cliquez sur **propriétés**.  
  
4.  Cliquez sur le **Classification** onglet et vérifiez que le fichier est classifié correctement.  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Scénario: Obtenir l’aide à mieux appréhender vos données à l’aide de Classification](Scenario--Get-Insight-into-Your-Data-by-Using-Classification.md)  
  
-   [Planifier la classification automatique des fichiers](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955)  
  
-   [Contrôle d’accès dynamique: Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  

