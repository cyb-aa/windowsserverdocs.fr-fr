---
ms.assetid: 01988844-df02-4952-8535-c87aefd8a38a
title: Déployer la classification automatique des fichiers (étapes de démonstration)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 77fb8cc6e13cb82e4d07808c3ae77757a4b2de79
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826780"
---
# <a name="deploy-automatic-file-classification-demonstration-steps"></a>Déployer la classification automatique des fichiers (étapes de démonstration)

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique explique comment activer les propriétés de ressource dans Active Directory, créer des règles de classification sur le serveur de fichiers, puis assigner des valeurs aux propriétés de ressource pour des fichiers sur le serveur de fichiers. Pour cet exemple, les règles de classification suivantes sont créées :  
  
-   Une règle de classification de contenu qui recherche un ensemble de fichiers pour la chaîne « Confidentiel Contoso. » Si la chaîne est détectée dans un fichier, la propriété de ressource Impact prend la valeur Haute sur le fichier.  
  
-   Une règle de classification du contenu qui recherche dans un ensemble de fichiers une expression régulière qui correspond à un numéro de sécurité sociale au moins 10 fois dans un fichier. Si le modèle est détecté, le fichier est classifié comme comportant des informations d'identification personnelle et la propriété de ressource Informations d'identification personnelle prend la valeur Haute.  
  
**Dans ce document**  
  
-   [Étape 1 : Créer des définitions de propriété de ressource](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [Étape 2 : Créer une règle de classification de contenu de chaîne](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step2)  
  
-   [Étape 3 : Créer une règle de classification de contenu d’expression régulière](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [Étape 4 : Vérifier que les fichiers sont classifiés](Deploy-Automatic-File-Classification--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Step1"></a>Étape 1 : créer des définitions de propriétés de ressource  
Les propriétés de ressource Impact et Informations d'identification personnelle sont activées pour que l'Infrastructure de classification des fichiers puissent les utiliser pour baliser les fichiers analysés dans un dossier réseau partagé.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>Pour créer des définitions de propriétés de ressource  
  
1.  Sur le contrôleur de domaine, connectez-vous au serveur en tant que membre du groupe de sécurité Admins du domaine.  
  
2.  Ouvrez le Centre d'administration Active Directory. Dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Centre d'administration Active Directory**.  
  
3.  Développez **Contrôle d'accès dynamique**, puis cliquez sur **Propriétés de ressource**.  
  
4.  Cliquez avec le bouton droit sur **Impact**, puis cliquez sur **Activer**.  
  
5.  Cliquez avec le bouton droit sur **Informations d'identification personnelle**, puis cliquez sur **Activer**.  
  
![guides de solutions](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'   
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>Étape 2 : créer une règle de classification de contenu de chaîne  
Une règle de classification de contenu de chaîne analyse un fichier à la recherche d'une chaîne spécifique. Si la chaîne est détectée, la valeur d'une propriété de ressource peut être configurée. Dans cet exemple, nous analyse chaque fichier sur un dossier réseau partagé et recherchez la chaîne « Confidentiel Contoso. » Si la chaîne est détectée, le fichier associé est classifié comme ayant un fort impact commercial.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-create-a-string-content-classification-rule"></a>Pour créer une règle de classification de contenu de chaîne  
  
1.  Connectez-vous au serveur de fichiers en tant que membre du groupe de sécurité Administrateurs.  
  
2.  À l'invite de commandes Windows PowerShell, tapez **Update-FsrmClassificationPropertyDefinition**, puis appuyez sur Entrée. Cette commande permet de synchroniser les définitions de propriétés créées sur le contrôleur de domaine avec le serveur de fichiers.  
  
3.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Gestionnaire de ressources du serveur de fichiers**.  
  
4.  Développez **Gestion de la classification**, cliquez avec le bouton droit sur **Règles de classification**, puis cliquez sur **Configurer la planification de la classification**.  
  
5.  Cochez la case **Activer la planification fixe**, la case **Autoriser la classification continue de nouveaux fichiers**, choisissez un jour de la semaine pour l'exécution de la classification, puis cliquez sur **OK**.  
  
6.  Cliquez avec le bouton droit sur **Règles de classification**, puis cliquez sur **Créer une règle de classification**.  
  
7.  Sous l'onglet **Général**, dans la zone **Nom de la règle**, tapez un nom de règle tel que **Contoso Confidential**.  
  
8.  Sous l'onglet **Étendue** , cliquez sur **Ajouter**et choisissez les dossiers qui doivent être inclus dans cette règle, tels que D:\Finance Documents.  
  
    > [!NOTE]  
    > Vous pouvez aussi choisir un espace de noms dynamique pour l'étendue. Pour plus d’informations sur les espaces de noms dynamiques pour les règles de classification, consultez [quelles sont les nouveautés dans File Server Resource Manager dans Windows Server 2012 \[redirigé\]](assetId:///d53c603e-6217-4b98-8508-e8e492d16083).  
  
9. Sous l'onglet **Classification**, configurez ce qui suit :  
  
    -   Dans la zone **Choisissez une méthode pour attribuer une propriété aux fichiers**, vérifiez que **Classifieur de contenus** est sélectionné.  
  
    -   Dans la zone **Choisissez une propriété à attribuer aux fichiers**, cliquez sur **Impact**.  
  
    -   Dans la zone **Spécifiez une valeur**, cliquez sur **Haute**.  
  
10. Sous l'en-tête **Paramètres** , cliquez sur **Configurer**.  
  
11. Dans la colonne **Type d'expression**, sélectionnez **Chaîne**.  
  
12. Dans la colonne **Expression** , tapez **Contoso Confidential**, puis cliquez sur **OK**.  
  
13. Sous l'onglet **Type d'évaluation**, cochez la case **Réévaluer les valeurs de propriété existantes**, cliquez sur  **Remplacer la valeur existante**, puis sur **OK**.  
  
![guides de solutions](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;$AutomaticClassificationScheduledTask  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name 'Contoso Confidential' -Property "Impact_MS" -PropertyValue "3000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step3"></a>Étape 3 : créer une règle de classification de contenu d'expression régulière  
Une règle de classification de contenu d'expression régulière analyse un fichier à la recherche d'un modèle qui correspond à l'expression régulière. Si une chaîne correspondant à l'expression régulière est détectée, la valeur d'une propriété de ressource peut être configurée. Dans cet exemple, nous allons analyser chaque fichier d'un dossier réseau partagé à la recherche d'une chaîne qui correspond au modèle d'un numéro de sécurité sociale (XXX-XX-XXXX). Si ce modèle est détecté, le fichier associé est classifié comme contenant des informations d'identification personnelle.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-regular-expression-content-classification-rule"></a>Pour créer une règle de classification de contenu d'expression régulière  
  
1.  Connectez-vous au serveur de fichiers en tant que membre du groupe de sécurité Administrateurs.  
  
2.  À l'invite de commandes Windows PowerShell, tapez **Update-FsrmClassificationPropertyDefinition**, puis appuyez sur Entrée. Cette commande permet de synchroniser les définitions de propriétés qui sont créées sur le contrôleur de domaine avec le serveur de fichiers.  
  
3.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Gestionnaire de ressources du serveur de fichiers**.  
  
4.  Cliquez avec le bouton droit sur **Règles de classification**, puis cliquez sur **Créer une règle de classification**.  
  
5.  Sous l'onglet **Général**, dans la zone **Nom de la règle**, tapez un nom pour la règle de classification, par exemple Règle PII.  
  
6.  Sous l'onglet **Étendue**, cliquez sur **Ajouter** et choisissez les dossiers qui doivent être inclus dans cette règle, tels que D:\Finance Documents.  
  
7.  Sous l'onglet **Classification**, configurez ce qui suit :  
  
    -   Dans la zone **Choisissez une méthode pour attribuer une propriété aux fichiers**, vérifiez que **Classifieur de contenus** est sélectionné.  
  
    -   Dans la zone **Choisissez une propriété à attribuer aux fichiers**, cliquez sur **Informations d'identification personnelle**.  
  
    -   Dans la zone **Spécifiez une valeur**, cliquez sur **Haute**.  
  
8.  Sous l'en-tête **Paramètres** , cliquez sur **Configurer**.  
  
9. Dans la colonne **Type d'expression** , sélectionnez **Expression régulière**.  
  
10. Dans le **Expression** colonne, tapez **^ ( ? 000) ([0-7] \d{2}| 7([0-7]\d|7[012])) ([-] ?) (?! 00) \d\d\3 ( ? ! 0000) \d{4}$**  
  
11. Dans la colonne **Occurrences minimales**, tapez **10**, puis cliquez sur **OK**.  
  
12. Sous l'onglet **Type d'évaluation**, cochez la case **Réévaluer les valeurs de propriété existantes**, cliquez sur  **Remplacer la valeur existante**, puis sur **OK**.  
  
![guides de solutions](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
New-FSRMClassificationRule -Name "PII Rule" -Property "PII_MS" -PropertyValue "5000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=10;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step4"></a>Étape 4 : vérifier que les fichiers sont classifiés correctement  
Vous pouvez vérifier que les fichiers sont classifiés correctement en affichant les propriétés d'un fichier créé dans le dossier spécifié dans les règles de classification.  
  
#### <a name="to-verify-that-the-files-are-classified-correctly"></a>Pour vérifier que les fichiers sont classifiés correctement  
  
1.  Sur le serveur de fichiers, exécutez les règles de classification à l'aide du Gestionnaire de ressources du serveur de fichiers.  
  
    1.  Cliquez sur **Gestion de la classification**, cliquez avec le bouton droit sur **Règles de classification**, puis cliquez sur **Exécuter la classification avec toutes les règles maintenant**.  
  
    2.  Cliquez sur l'option **Attendre la fin de la classification**, puis cliquez sur **OK**.  
  
    3.  Fermez le Rapport de classification automatique.  
  
    4.  Pour ce faire, vous pouvez exécuter la commande suivante dans Windows PowerShell : **Start-FSRMClassification » « RunDuration 0 - confirmer : $false**  
  
2.  Accédez au dossier qui a été spécifié dans les règles de classification, tel que D:\Documents de finance.  
  
3.  Cliquez avec le bouton droit sur un fichier dans ce dossier, puis cliquez sur **Propriétés**.  
  
4.  Cliquez sur l'onglet **Classification** et vérifiez que le fichier est classifié correctement.  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Scénario : Obtenir un aperçu de vos données à l’aide de Classification](Scenario--Get-Insight-into-Your-Data-by-Using-Classification.md)  
  
-   [Planifier la Classification automatique des fichiers](https://docs.microsoft.com/previous-versions/orphan-topics/ws.11/jj574209(v%3dws.11))  

  
-   [Contrôle d’accès dynamique : Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  

