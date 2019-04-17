---
ms.assetid: 8738c03d-6ae8-49a7-8b0c-bef7eab81057
title: "Déployer une stratégie d’accès centralisée (étapes de démonstration)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 16973b32407985ffc3f66bf5ac579384a756b5d0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-a-central-access-policy-demonstration-steps"></a>Déployer une stratégie d’accès centralisée (étapes de démonstration)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Dans ce scénario, les opérations de sécurité du service finance collabore avec des centralisé sécurité des informations pour spécifier la nécessité d’une stratégie d’accès centralisée afin de pouvoir protéger les informations financières archivées stockées sur les serveurs de fichiers. Les informations financières archivées de chaque pays sont accessibles en lecture seule par les employés finance du même pays. Un groupe d’administration centrale finance permettre accéder aux informations financières de tous les pays.  
  
Déploiement d’une stratégie d’accès centralisée comprend les phases suivantes:  
  
|Phase|Description  
|---------|---------------  
|[Plan: Identifier le besoin de stratégie et la configuration requise pour le déploiement](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.2)|Identifier la nécessité d’une stratégie et la configuration requise pour le déploiement. 
|[Implémenter: Configurer les composants et la stratégie](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.3)|Configurer les composants et la stratégie.  
|[Déployer la stratégie d’accès centralisée](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.4)|Déployer la stratégie.  
|[Mettre à jour: Modifier et organiser la stratégie](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.5)|Modifications de stratégie et intermédiaires. 
  
## <a name="BKMK_1.1"></a>Configurer un environnement de test  
Avant de commencer, vous devez configurer un laboratoire pour tester ce scénario. Les étapes de configuration du laboratoire sont décrites en détail dans [annexe b: paramètres de l’environnement de Test ](Appendix-B--Setting-Up-the-Test-Environment.md).  
  
## <a name="BKMK_1.2"></a>Plan: Identifier le besoin de stratégie et la configuration requise pour le déploiement  
Cette section fournit les principales étapes de la phase de planification de votre déploiement.  
  
||Étape|Exemple|  
|-|--------|-----------|  
|1.1|L’activité commerciale détermine qu’une stratégie d’accès centralisée est nécessaire.|Pour protéger les informations financières stockées sur les serveurs de fichiers, les opérations de sécurité du service finance collabore avec des centralisé sécurité des informations pour spécifier la nécessité d’une stratégie d’accès centralisée.|  
|1.2|Exprimer la stratégie d’accès|Les documents financiers ne doivent pas être lus par les membres du service Finance. Les membres du service Finance doivent accéder uniquement aux documents de leur propre pays. Seuls les administrateurs financiers doivent avoir un accès en écriture. Une exception sera accordée aux membres du groupe FinanceException. Ce groupe disposera d’un accès en lecture.|  
|1.3|Exprimer la stratégie d’accès dans des constructions Windows Server2012|Ciblage:<br /><br />-Resource.Department contient Finance<br /><br />Règles d’accès:<br /><br />-Autoriser lecture User.Country=Resource.Country et User.department = Resource.Department<br />-Autoriser le contrôle total User.MemberOf(FinanceAdmin)<br /><br />Exception:<br /><br />Autoriser la lecture memberOf(FinanceException)|  
|1.4|Déterminer les propriétés de fichier requises pour la stratégie|Baliser les fichiers avec:<br /><br />-Service<br />-Pays|  
|1.5|Déterminer les types de revendications et les groupes nécessaires pour la stratégie|Types de revendication:<br /><br />-Pays<br />-Service<br /><br />Groupes d’utilisateurs:<br /><br />-FinanceAdmin<br />-FinanceException|  
|1.6|Déterminer les serveurs sur lesquels appliquer cette stratégie|Appliquez la stratégie sur tous les serveurs de fichiers financiers.|  
  
## <a name="BKMK_1.3"></a>Implémenter: Configurer les composants et la stratégie  
Cette section présente un exemple qui déploie une stratégie d’accès centralisée pour des documents financiers.  
  
|N°|Étape|Exemple|  
|------|--------|-----------|  
|2.1|Créer des types de revendication|Créer les types de revendications suivants:<br /><br />-Service<br />-Pays|  
|2.2|Créer des propriétés de ressource|Créer et activer les propriétés de ressource suivants:<br /><br />-Service<br />-Pays|  
|2.3|Configurer une règle d’accès central|Créer une règle Documents financiers qui comprend la stratégie déterminée dans la section précédente.|  
|2.4|Configurer une stratégie d’accès centralisées (CAP)|Créer une stratégie d’accès centralisée nommée stratégie financière et ajoutez la règle Documents financiers à cette stratégie d’accès centralisée.|  
|2.5|Stratégie d’accès centralisée cible pour les serveurs de fichiers|Publier le CAP de stratégie Finance sur les serveurs de fichiers.|  
|2.6|Activer la prise en charge des revendications, l’authentification composée et du blindage Kerberos.|Activer la prise en charge des revendications, l’authentification composée et du blindage Kerberos pour contoso.com.|  
  
Dans la procédure suivante, vous créez deux types de revendication: Country et Department.  
  
#### <a name="to-create-claim-types"></a>Pour créer des types de revendication  
  
1.  Ouvrez Server DC1 dans le Gestionnaire Hyper-V et connectez-vous sur en tant que Contoso\Administrateur avec le mot de passe **pass@word1**.  
  
2.  Ouvrir le centre d’administration ActiveDirectory.  
  
3.  Cliquez sur le **icône arborescence**, développez **contrôle d’accès dynamique**, puis sélectionnez **Types de revendications **.  
  
    Avec le bouton droit **Types de revendications**, cliquez sur **New**, puis cliquez sur **Type de revendication **.  
  
    > [!TIP]  
    > Vous pouvez également ouvrir une **créer un Type de revendication:** fenêtre à partir de la **tâches** volet. Sur le **tâches** volet, cliquez sur **New**, puis cliquez sur **Type de revendication **.  
  
4.  Dans le **attribut Source** liste, faites défiler la liste des attributs, puis cliquez sur **service**. Cela doit remplir les **nom d’affichage** champ avec **service**. Cliquez sur **OK**.  
  
5.  Dans **tâches** volet, cliquez sur **New**, puis cliquez sur **Type de revendication**.  
  
6.  Dans le **attribut Source** liste, faites défiler la liste des attributs, puis cliquez sur le **c** attribut (Country-Name). Dans le **nom d’affichage**, tapez **pays**.  
  
7.  Dans le **valeurs suggérées** section, sélectionnez **les valeurs suivantes sont suggérées:**, puis cliquez sur **ajouter**.  
  
8.  Dans le **valeur** et **nom d’affichage** champs, tapez **américain**, puis cliquez sur **OK**.  
  
9. Répétez l’étape ci-dessus. Dans le **ajouter une valeur suggérée** boîte de dialogue, tapez **JP** dans les **valeur** et **nom d’affichage** champs, puis cliquez sur **OK**.  
  
![guides de solutions](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
 
    New-ADClaimType country -SourceAttribute c -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("US","US","")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("JP","JP","")))  
    New-ADClaimType department -SourceAttribute department  
      
 
  
> [!TIP]  
> Vous pouvez utiliser la visionneuse de l’historique de Windows PowerShell dans le centre d’administration ActiveDirectory pour rechercher les applets de commande Windows PowerShell pour chaque procédure que vous effectuez dans le centre d’administration ActiveDirectory. Pour plus d’informations, voir [visionneuse de l’historique de Windows PowerShell](https://technet.microsoft.com/library/hh831702)  
  
L’étape suivante consiste à créer des propriétés de ressource. Dans la procédure suivante vous créez une propriété de ressource qui est automatiquement ajoutée à la liste des propriétés de ressource globales sur le contrôleur de domaine, afin qu’il soit disponible pour le serveur de fichiers.  
  
#### <a name="to-create-and-enable-pre-created-resource-properties"></a>Pour créer et activer les propriétés de ressource créées au préalable  
  
1.  Dans le volet gauche du centre d’administration Active Directory, cliquez sur **arborescence**. Développez **contrôle d’accès dynamique**, puis sélectionnez **propriétés de ressource**.  
  
2.  Avec le bouton droit **propriétés de ressource**, cliquez sur **New**, puis cliquez sur **propriété de ressource de référence**.  
  
    > [!TIP]  
    > Vous pouvez également choisir une propriété de ressource à partir de la **tâches** volet. Cliquez sur **New** puis cliquez sur **propriété de ressource de référence**.  
  
3.  Dans **liste de valeurs de sélectionner un type de revendication pour partager qu’il est suggéré**, cliquez sur **pays**.  
  
4.  Dans le **nom d’affichage**, tapez **pays**, puis cliquez sur **OK**.  
  
5.  Double-cliquez sur le **propriétés de ressource** liste, faites défiler jusqu'à la **service** propriété de ressource. Avec le bouton droit, puis cliquez sur **activer**. Cela permettra intégré **service** propriété de ressource.  
  
6.  Dans le **propriétés de ressource** liste du volet de navigation du centre d’administration ActiveDirectory, vous avez maintenant deux propriétés de ressource activées:  
  
    -   Pays  
  
    -   Service  
  
![guides de solutions](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
New-ADResourceProperty Country -IsSecured $true -ResourcePropertyValueType MS-DS-MultivaluedChoice -SharesValuesWith country  
Set-ADResourceProperty Department_MS -Enabled $true  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Country  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Department_MS  
  
```  
  
L’étape suivante consiste à créer des règles d’accès central qui définissent qui peuvent accéder aux ressources. Dans ce scénario, les règles professionnelles sont:  
  
-   Documents financiers peuvent être lus que par les membres du service Finance.  
  
-   Les membres du service Finance peuvent accéder qu’aux documents de leur propre pays.  
  
-   Seuls les administrateurs financiers peut avoir un accès en écriture.  
  
-   Une exception sera accordée aux membres du groupe FinanceException. Ce groupe disposera d’un accès en lecture.  
  
-   L’administrateur et le propriétaire du document ont toujours accès complet.  
  
Ou, pour exprimer les règles avec des constructions Windows Server2012:  
  
Ciblage: Resource.Department contient Finance  
  
Règles d’accès:  
  
-   Autoriser la lecture User.Country=Resource.Country et User.department = Resource.Department  
  
-   Autoriser le contrôle total User.MemberOf(FinanceAdmin)  
  
-   Autoriser la lecture User.MemberOf(FinanceException)  
  
#### <a name="to-create-a-central-access-rule"></a>Pour créer une règle d’accès centralisée  
  
1.  Dans le volet gauche du centre d’administration ActiveDirectory, cliquez sur **arborescence**, sélectionnez **contrôle d’accès dynamique**, puis cliquez sur **règles d’accès Central**.  
  
2.  Avec le bouton droit **règles d’accès Central**, cliquez sur **New**, puis cliquez sur **règle d’accès Central**.  
  
3.  Dans le **nom**, tapez **règle Documents financiers**.  
  
4.  Dans le **ressources cibles**, cliquez sur **modifier**et dans le **règle d’accès Central** boîte de dialogue, cliquez sur **ajouter une condition**. Ajoutez la condition suivante:   
    [**Ressource**] [**Service**] [**Est égal à**] [**Value**] [**Finance**], puis cliquez sur **OK**.  
  
5.  Dans le **autorisations** section, sélectionnez **utiliser les autorisations suivantes en tant qu’autorisations actuelles**, cliquez sur **modifier**et dans le **paramètres de sécurité avancés pour autorisations** boîte de dialogue cliquez sur **ajouter**.  
  
    > [!NOTE]  
    > **Utilisez les autorisations suivantes en tant qu’autorisations proposées** option vous permet de créer la stratégie intermédiaire. Pour plus d’informations sur la façon de procéder reportez-vous à la section mise à jour: modification et l’étape de la section de stratégie dans cette rubrique.  
  
6.  Dans le **entrée d’autorisation pour les autorisations** boîte de dialogue, cliquez sur **sélectionnez un principal**, type **utilisateurs authentifiés**, puis cliquez sur **OK**.  
  
7.  Dans le **entrée d’autorisation pour les autorisations** boîte de dialogue, cliquez sur **ajouter une condition**et ajoutez les conditions suivantes:   
    [**User**] [**pays**] [**Any of**] [**Ressource**] [**pays**]   
     Cliquez sur **ajouter une condition**.   
     [**And**]   
    Cliquez sur [**utilisateur**] [**service**] [**des**] [**ressource**] [**service**]. Définir le **autorisations** à **lecture**.  
  
8.  Cliquez sur **OK**, puis cliquez sur **ajouter**. Cliquez sur **sélectionnez un principal**, type **FinanceAdmin**, puis cliquez sur **OK**.  
  
9. Sélectionnez le **modification, lecture et exécution, lecture, écriture** autorisations, puis cliquez sur **OK**.  
  
10. Cliquez sur **ajouter**, cliquez sur **sélectionnez un principal**, type **FinanceException**, puis cliquez sur **OK**. Sélectionnez les autorisations à être **lecture** et **lecture et exécution**.  
  
11. Cliquez sur **OK** trois fois pour terminer et revenir au centre d’administration Active Directory.  
  
    ![guides de solutions](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
    L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
 
    $countryClaimType = Get-ADClaimType pays  
    $departmentClaimType = Get-ADClaimType service  
    $countryResourceProperty = Get-ADResourceProperty pays  
    $departmentResourceProperty = Get-ADResourceProperty service  
    $currentAcl = O:SYG:SYD:AR(A;;» IMMOBILISATION;; O W) (A; iMMOBILISATION;; BA) (A; 0 x1200a9;; S-1-5-21-1787166779-1215870801-2157059049-1113) (A; 0 x1301bf;; S-1-5-21-1787166779-1215870801-2157059049-1112) (A; iMMOBILISATION;; SY) (XA; 0 x1200a9;; AU;((@USER." + $countryClaimType.Name + «Any_of @RESOURCE.» + $countryResourceProperty.Name +») et et (@USER.» + $departmentClaimType.Name + «Any_of @RESOURCE.» + $departmentResourceProperty.Name +»)))»  
    $resourceCondition =» (@RESOURCE.» + $departmentResourceProperty.Name +» contient {`"Finance`»})»  
    New-ADCentralAccessRule «Finance Documents règle» - CurrentAcl $currentAcl - ResourceCondition $resourceCondition  

  
> [!IMPORTANT]  
> Dans l’exemple d’applet de commande ci-dessus, les identificateurs de sécurité (SID) du groupe FinanceAdmin et les utilisateurs est déterminée au moment de la création et diffèreront dans votre exemple. Par exemple, la SID valeur fournie (S-1-5-21-1787166779-1215870801-2157059049-1113) pour FinanceAdmins doit être remplacé par le SID du groupe FinanceAdmin que vous devez créer dans votre déploiement. Vous pouvez utiliser Windows PowerShell pour rechercher la valeur du SID de ce groupe, assigner cette valeur à une variable, puis utilisez la variable ici. Pour plus d’informations, voir [Conseil Windows PowerShell: utilisation de SID](https://go.microsoft.com/fwlink/?LinkId=253545).  
  
Vous devez maintenant avoir une règle d’accès central qui permet aux utilisateurs d’accéder aux documents à partir du même pays et le même service. La règle autorise le groupe FinanceAdmin à modifier les documents, et il autorise le groupe FinanceException à lire les documents. Cette règle cible uniquement les documents classifiés comme financiers.  
  
#### <a name="to-add-a-central-access-rule-to-a-central-access-policy"></a>Pour ajouter une règle d’accès central à une stratégie d’accès centralisée  
  
1.  Dans le volet gauche du centre d’administration ActiveDirectory, cliquez sur **contrôle d’accès dynamique**, puis cliquez sur **stratégies d’accès centralisées**.  
  
2.  Dans le **tâches** volet, cliquez sur **New**, puis cliquez sur **stratégie d’accès centralisée**.  
  
3.  Dans **créer de stratégie d’accès centralisée:**, type **stratégie financière** dans les **nom** zone.  
  
4.  Dans **règles d’accès central membres**, cliquez sur **ajouter**.  
  
5.  Double-cliquez sur le **règle Documents financiers** pour l’ajouter à la **ajouter les règles d’accès central suivantes** liste, puis cliquez sur **OK **.  
  
6.  Cliquez sur **OK** pour terminer. Vous devez maintenant avoir une stratégie d’accès centralisée nommée stratégie Finance.  
  
    ![guides de solutions](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
    L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
    ```  
    New-ADCentralAccessPolicy "Finance Policy" Add-ADCentralAccessPolicyMember   
    -Identity "Finance Policy"   
    -Member "Finance Documents Rule"  
  
    ```  
  
#### <a name="to-apply-the-central-access-policy-across-file-servers-by-using-group-policy"></a>Pour appliquer la stratégie d’accès centralisée sur les serveurs de fichiers à l’aide de stratégie de groupe  
  
1.  Sur le **Démarrer** de l’écran, dans le **recherche**, tapez **gestion des stratégies de groupe **. Double-cliquez sur **gestion des stratégies de groupe **.  
  
    > [!TIP]  
    > Si le **outils afficher d’administration** paramètre est désactivé, le **outils d’administration** dossier et son contenu s’affiche pas dans le **paramètres** résultats.  
  
    > [!TIP]  
    > Dans votre environnement de production, vous devez créer une unité d’organisation de fichier serveur (OU) et ajouter vos serveurs de fichiers à cette unité d’organisation, à laquelle vous souhaitez appliquer cette stratégie. Vous pouvez ensuite créer une stratégie de groupe et ajouter cette unité d’organisation à cette stratégie.  
  
2.  Dans cette étape, vous modifiez l’objet de stratégie de groupe vous avez créé dans [créer le contrôleur de domaine](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_Build) section dans l’environnement de Test pour inclure la stratégie d’accès centralisée que vous avez créé. Dans l’éditeur de gestion de stratégie de groupe, accédez à et sélectionnez l’unité d’organisation dans le domaine (contoso.com dans cet exemple): **gestion des stratégies de groupe**, **forêt: contoso.com**, **domaines**, **contoso.com**, **Contoso**, **FileServerOU **.  
  
3.  Avec le bouton droit **FlexibleAccessGPO**, puis cliquez sur **modifier **.  
  
4.  Dans la fenêtre Éditeur de gestion de stratégie de groupe, accédez à **Configuration ordinateur**, développez **stratégies**, développez **paramètres Windows**, puis cliquez sur **paramètres de sécurité **.  
  
5.  Développez **système de fichiers**, avec le bouton droit **stratégie d’accès centralisée**, puis cliquez sur **stratégies d’accès gérer centralisées**.  
  
6.  Dans le **Configuration des stratégies d’accès Central** boîte de dialogue zone, ajoutez **stratégie financière**, puis cliquez sur **OK **.  
  
7.  Faites défiler jusqu'à **Configuration avancée de stratégie d’Audit**et le développer.  
  
8.  Développez **stratégies d’Audit**, puis sélectionnez **l’accès aux objets **.  
  
9. Double-cliquez sur **auditer la stratégie d’accès centralisée intermédiaire **. Sélectionnez les trois cases, puis **OK **. Cette étape permet au système de recevoir les événements d’audit liés aux stratégies de mise en attente d’accès Central.  
  
10. Double-cliquez sur **auditer les propriétés de système de fichiers **. Sélectionnez les trois cases, puis cliquez sur **OK **.  
  
11. Fermez l’éditeur de gestion de stratégie de groupe. Vous avez maintenant inclus la stratégie d’accès centralisée à la stratégie de groupe.  
  
Pour les contrôleurs de domaine d’un domaine fournir des revendications ou des données d’autorisation de périphériques, les contrôleurs de domaine doivent être configurés pour prendre en charge le contrôle d’accès dynamique.  
  
#### <a name="to-enable-support-for-claims-and-compound-authentication-for-contosocom"></a>Pour activer la prise en charge des revendications et l’authentification composée pour contoso.com  
  
1.  Ouvrez Gestion des stratégies de groupe, cliquez sur **contoso.com**, puis cliquez sur **contrôleurs de domaine **.  
  
2.  Avec le bouton droit **stratégie des contrôleurs de domaine par défaut**, puis cliquez sur **modifier **.  
  
3.  Dans la fenêtre Éditeur de gestion de stratégie de groupe, double-cliquez sur **Configuration ordinateur**, double-cliquez sur **stratégies**, double-cliquez sur **modèles d’administration**, double-cliquez sur **système**, puis double-cliquez sur **KDC**.  
  
4.  Double-cliquez sur **prise en charge des revendications, l’authentification composée et le blindage Kerberos **. Dans le **prise en charge des revendications, l’authentification composée et le blindage Kerberos** boîte de dialogue, cliquez sur **activé** et sélectionnez **pris en charge** à partir de la **Options** liste déroulante. (Vous devez activer ce paramètre pour utiliser des revendications d’utilisateur dans les stratégies d’accès centralisées.)  
  
5.  Fermer **gestion des stratégies de groupe**.  
  
6.  Ouvrez une invite de commandes et tapez `gpupdate /force`.  
  
## <a name="BKMK_1.4"></a>Déployer la stratégie d’accès centralisée  
  
||Étape|Exemple|  
|-|--------|-----------|  
|3.1|Affectez l’embout dans les dossiers partagés appropriés sur le serveur de fichiers.|Assigner la stratégie d’accès centralisée au dossier partagé approprié sur le serveur de fichiers.|  
|3.2|Vérifiez que l’accès est configuré correctement.|Vérifiez l’accès pour les utilisateurs à partir de différents pays et services.|  
  
Dans cette étape, vous allez attribuer la stratégie d’accès centralisée à un serveur de fichiers. Vous ouvrez une session sur un serveur de fichiers qui reçoit la stratégie d’accès centralisée que vous avez créé les étapes précédentes et assigner la stratégie à un dossier partagé.  
  
#### <a name="to-assign-a-central-access-policy-to-a-file-server"></a>Pour affecter une stratégie d’accès centralisée à un serveur de fichiers  
  
1.  Dans le Gestionnaire Hyper-V, connectez-vous au serveur FILE1. Ouvrez une session sur le serveur à l’aide de Contoso\Administrateur avec le mot de passe:**pass@word1**.  
  
2.  Ouvrez une invite de commandes avec élévation de privilèges et le type: **gpupdate /force**. Cela garantit que vos modifications de stratégie de groupe prennent effet sur votre serveur.  
  
3.  Vous devez aussi actualiser les propriétés de ressource globales à partir d’Active Directory. Ouvrez une fenêtre de Windows PowerShell avec élévation de privilèges et tapez `Update-FSRMClassificationpropertyDefinition`. Cliquez sur entrée, puis fermez Windows PowerShell.  
  
    > [!TIP]  
    > Vous pouvez également actualiser les propriétés de ressource globales en ouvrant une session sur le serveur de fichiers. Pour actualiser les propriétés de ressource globales à partir du serveur de fichiers, procédez comme suit  
    >   
    > 1.  Ouvrez une session sur un serveur fichiers FILE1 en tant que Contoso\Administrateur, avec le mot de passe **pass@word1**.  
    > 2.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Pour ouvrir le Gestionnaire de ressources du serveur de fichiers, cliquez sur **Démarrer**, type **FSRM**, puis cliquez sur **File Server Resource Manager**.  
    > 3.  Dans le Gestionnaire de ressources de serveur de fichiers, cliquez sur **gestion de la Classification fichier**, avec le bouton droit **propriétés de Classification** puis cliquez sur **Actualiser **.  
  
4.  Ouvrez l’Explorateur Windows et dans le volet gauche, cliquez sur le lecteur D. Cliquez sur le **Documents financiers** dossier, puis cliquez sur **propriétés **.  
  
5.  Cliquez sur le **Classification**, cliquez sur **pays**, puis sélectionnez **États-Unis** dans les **valeur** champ.  
  
6.  Cliquez sur **service**, puis sélectionnez **Finance** dans les **valeur** champ, puis cliquez sur **appliquer **.  
  
    > [!NOTE]  
    > N’oubliez pas que la stratégie d’accès centralisée a été configurée pour cibler les fichiers du service Finance. Les étapes précédentes marquent tous les documents dans le dossier avec les attributs Country et Department.  
  
7.  Cliquez sur le **sécurité** onglet, puis cliquez sur **avancé**. Cliquez sur le **stratégie centralisée** onglet.  
  
8.  Cliquez sur **modification**, sélectionnez **stratégie financière** dans le menu déroulant, puis cliquez sur **appliquer **. Vous pouvez voir les **règle Documents financiers** répertoriés dans la stratégie. Développez l’élément pour afficher toutes les autorisations que vous avez définies lors de la création de la règle dans ActiveDirectory.  
  
9. Cliquez sur **OK** pour revenir à l’Explorateur Windows.  
  
Dans l’étape suivante, vous vérifiez que l’accès est configuré correctement.  Comptes d’utilisateur doivent disposer de l’ensemble d’attribut Department approprié (définir, utilisez le centre d’administration ActiveDirectory). Le moyen le plus simple pour afficher les résultats de la nouvelle stratégie efficaces consiste à utiliser le **accès effectif** onglet dans l’Explorateur Windows. Le **accès effectif** onglet affiche les droits d’accès pour un compte d’utilisateur donné.  
  
#### <a name="to-examine-the-access-for-various-users"></a>Pour examiner l’accès pour différents utilisateurs  
  
1.  Dans le Gestionnaire Hyper-V, connectez-vous au serveur FILE1. Connectez-vous au serveur avec le compte contoso\administrateur. Accédez à D:\ dans l’Explorateur Windows. Cliquez sur le **Documents financiers** dossier, puis cliquez sur **propriétés **.  
  
2.  Cliquez sur le **sécurité**, cliquez sur **avancé**, puis cliquez sur le **accès effectif** onglet.  
  
3.  Pour examiner les autorisations pour un utilisateur, cliquez sur **sélectionner un utilisateur**, tapez le nom d’utilisateur, puis cliquez sur **afficher l’accès effectif** pour afficher les droits d’accès effectif. Par exemple:  
  
    -   Myriam Delesalle (MDelesalle) se trouve dans le service Finance et doit avoir un accès en lecture au dossier.  
  
    -   Miles Reid (MReid) est un membre du groupe FinanceAdmin et doit avoir accès en modification au dossier.  
  
    -   Esther Valle (EValle) n’est pas au service Finance; toutefois, elle est membre du groupe FinanceException et doit avoir un accès en lecture.  
  
    -   Maira Wenzel (MWenzel) n’est pas au service Finance et n’est pas membre d’un du groupe FinanceAdmin ou FinanceException. Elle ne doit pas avoir d’accès au dossier.  
  
    Notez que la dernière colonne nommée **accès limité** dans la fenêtre de l’accès effectif. Cette colonne indique les portes des autorisations de la personne. Dans ce cas, les autorisations de partage et NTFS permettent à tous les utilisateurs. Toutefois, la stratégie d’accès centralisée limite l’accès selon les règles que vous avez configurée précédemment.  
  
## <a name="BKMK_1.5"></a>Mettre à jour: Modifier et organiser la stratégie  
  
||||  
|-|-|-|  
|Nombre|Étape|Exemple|  
|4.1|Configurer des revendications de périphérique pour les Clients|Définissez le paramètre de stratégie de groupe pour activer les revendications de périphérique|  
|4.2|Activer une revendication pour les périphériques.|Activer le type de revendication de pays pour les périphériques.|  
|4.3|Ajouter une stratégie intermédiaire à la règle d’accès central existante que vous souhaitez modifier.|Modifier la règle Documents financiers pour ajouter une stratégie intermédiaire.|  
|4.4|Afficher les résultats de la stratégie intermédiaire.|Vérifiez les autorisations d’Ester Velle.|  
  
#### <a name="to-set-up-group-policy-setting-to-enable-claims-for-devices"></a>Pour configurer la stratégie de groupe paramètre pour activer les revendications pour les périphériques  
  
1.  Ouvrez une session sur DC1, ouvrez Gestion des stratégies de groupe, cliquez sur ce bouton **contoso.com**, cliquez sur **stratégie de domaine par défaut**, avec le bouton droit et sélectionnez **modifier **.  
  
2.  Dans la fenêtre Éditeur de gestion de stratégie de groupe, accédez à **Configuration ordinateur**, **stratégies**, **modèles d’administration**, **système**, **Kerberos **.  
  
3.  Sélectionnez **prise en charge du client Kerberos des revendications, l’authentification composée et le blindage Kerberos** et cliquez sur **activer **.  
  
#### <a name="to-enable-a-claim-for-devices"></a>Pour activer une revendication pour les périphériques  
  
1.  Ouvrez Server DC1 dans le Gestionnaire Hyper-V et connectez-vous sur en tant que Contoso\Administrateur avec le mot de passe **pass@word1**.  
  
2.  À partir de la **outils** menu, ouvrez Centre d’administration ActiveDirectory.  
  
3.  Cliquez sur **arborescence**, développez **contrôle d’accès dynamique**, double-cliquez sur **Types de revendication**, double-cliquez sur le **pays** de revendication.  
  
4.  Dans **revendications de ce type peuvent être émises pour les classes suivantes**, sélectionnez le **ordinateur** case à cocher. Cliquez sur **OK**.   
    Les deux **utilisateur** et **ordinateur** cases à cocher doit maintenant être sélectionnés. La revendication country permet désormais avec les périphériques et utilisateurs.  
  
L’étape suivante consiste à créer une règle de stratégie intermédiaire. Stratégies intermédiaires peuvent servir à contrôler les effets d’une nouvelle entrée de stratégie avant d’activer. Dans l’étape suivante, vous créez une entrée de stratégie intermédiaire et contrôler son effet sur votre dossier partagé.  
  
#### <a name="to-create-a-staging-policy-rule-and-add-it-to-the-central-access-policy"></a>Pour créer une règle de stratégie intermédiaire et l’ajouter à la stratégie d’accès centralisée  
  
1.  Ouvrez Server DC1 dans le Gestionnaire Hyper-V et connectez-vous sur en tant que Contoso\Administrateur avec le mot de passe **pass@word1**.  
  
2.  Ouvrir le centre d’administration ActiveDirectory.  
  
3.  Cliquez sur **arborescence**, développez **contrôle d’accès dynamique**, puis sélectionnez **règles d’accès Central **.  
  
4.  Avec le bouton droit **règle Documents financiers**, puis cliquez sur **propriétés **.  
  
5.  Dans le **autorisations proposées** section, sélectionnez le **autorisation activer intermédiaire configuration** case à cocher, cliquez sur **modifier**, puis cliquez sur **ajouter **. Dans le **entrée d’autorisation pour autorisations proposées** fenêtre, cliquez sur le **sélectionnez un Principal** lier, tapez **utilisateurs authentifiés**, puis cliquez sur **OK **.  
  
6.  Cliquez sur le **ajouter une condition** lier et ajoutez la condition suivante:   
     [**User**] [**pays**] [**Any of**] [**Ressource**] [**Pays**].  
  
7.  Cliquez sur **ajouter une condition** à nouveau et ajoutez la condition suivante:  
    [**And**]   
     [**Périphérique**] [**pays**] [**Any of**] [**Ressource**] [**Pays**]  
  
8.  Cliquez sur **ajouter une condition** à nouveau et ajoutez la condition suivante.  
    [Et]   
     [**User**] [**Group**] [**Membre de n’importe lequel**] [**Valeur**] \ (**FinanceException**)  
  
9. Pour définir le groupe FinanceException, groupe, cliquez sur **ajouter des éléments** et dans le **sélectionner un utilisateur, ordinateur, compte de Service ou groupe** fenêtre, tapez **FinanceException **.  
  
10. Cliquez sur **autorisations**, sélectionnez **contrôle total**, puis cliquez sur **OK **.  
  
11. Dans les paramètres de sécurité avancé pour des informations sur les autorisations proposées, sélectionnez **FinanceException** et cliquez sur **supprimer **.  
  
12. Cliquez sur **OK** deux fois pour terminer.  
  
![guides de solutions](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Set-ADCentralAccessRule  
-Identity: "CN=FinanceDocumentsRule,CN=CentralAccessRules,CN=ClaimsConfiguration,CN=Configuration,DC=Contoso.com"  
-ProposedAcl: "O:SYG:SYD:AR(A;;FA;;;BA)(A;;FA;;;SY)(A;;0x1301bf;;;S-1-21=1426421603-1057776020-1604)"  
-Server: "WIN-2R92NN8VKFP.Contoso.com"  
  
```  
  
> [!NOTE]  
> Dans l’exemple d’applet de commande ci-dessus, la valeur Server correspond au serveur dans l’environnement de laboratoire de test. Vous pouvez utiliser la visionneuse de l’historique de Windows PowerShell pour rechercher les applets de commande Windows PowerShell pour chaque procédure que vous effectuez dans le centre d’administration ActiveDirectory. Pour plus d’informations, voir [visionneuse de l’historique de Windows PowerShell](https://technet.microsoft.com/library/hh831702)  
  
Dans cet ensemble d’autorisations proposées, les membres du groupe FinanceException aura un accès complet aux fichiers à partir de leur propre pays quand ils y accèderont par le biais d’un appareil à partir du même pays que le document. Entrées d’audit sont disponibles dans le journal de sécurité lorsqu’un utilisateur du service Finance tente d’accéder aux fichiers des serveurs de fichiers. Toutefois, les paramètres de sécurité ne sont pas appliquées jusqu'à ce que la stratégie est promue de transit.  
  
Dans la procédure suivante, vous vérifiez les résultats de la stratégie intermédiaire. Vous accédez au dossier partagé avec un nom d’utilisateur qui dispose des autorisations basées sur la règle actuelle. Esther Valle (EValle) est un membre du groupe FinanceException et elle actuellement dispose des droits en lecture. Selon notre stratégie intermédiaire, EValle ne doit disposer des droits.  
  
#### <a name="to-verify-the-results-of-the-staging-policy"></a>Pour vérifier les résultats de la stratégie intermédiaire  
  
1.  Se connecter au serveur de fichiers FILE1 dans le Gestionnaire Hyper-V et connectez-vous sur en tant que Contoso\Administrateur avec le mot de passe **pass@word1**.  
  
2.  Ouvrez une fenêtre d’invite de commandes et tapez **gpupdate /force **. Cela garantit que les modifications de la stratégie de groupe prennent effet sur votre serveur.  
  
3.  Dans le Gestionnaire Hyper-V, connectez-vous au serveur CLIENT1. Se déconnecter de l’utilisateur actuellement connecté. Redémarrez l’ordinateur virtuel, CLIENT1. Puis ouvrez une session sur l’ordinateur à l’aide de contoso\EValle pass@word1.  
  
4.  Double-cliquez sur le raccourci du bureau vers des Documents \\\FILE1\Finance. EValle doit encore avoir accès aux fichiers. Revenez à FILE1.  
  
5.  Ouvrez **l’Observateur d’événements** à partir du raccourci sur le bureau. Développez **journaux Windows**, puis sélectionnez **sécurité **. Ouvrez les entrées avec **Event ID4818**sous le **stratégie d’accès centralisée intermédiaire** catégorie de tâche. Vous verrez que EValle a été autorisé à accéder; toutefois, en fonction de la stratégie intermédiaire, l’utilisateur aurait été refusé l’accès.  
  
## <a name="next-steps"></a>Étapes suivantes  
Si vous disposez d’un système de gestion de serveur central tels que SystemCenter Operations Manager, vous pouvez également configurer la surveillance des événements. Cela permet aux administrateurs de contrôler les effets des stratégies d’accès centralisées avant de les appliquer.  
  

