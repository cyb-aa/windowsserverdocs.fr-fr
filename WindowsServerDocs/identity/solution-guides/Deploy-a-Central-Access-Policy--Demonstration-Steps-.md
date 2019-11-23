---
ms.assetid: 8738c03d-6ae8-49a7-8b0c-bef7eab81057
title: Déployer une stratégie d'accès centralisée (étapes de démonstration)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 09b7edcd843dfe65d7e2391612f029cf18b633ec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357498"
---
# <a name="deploy-a-central-access-policy-demonstration-steps"></a>Déployer une stratégie d'accès centralisée (étapes de démonstration)

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans ce scénario, le service Sécurité du service Finance collabore avec le service centralisé Sécurité des informations pour spécifier la nécessité de mettre en place une stratégie d'accès centralisée afin de pouvoir protéger les informations financières archivées stockées sur des serveurs de fichiers. Les informations financières archivées de chaque pays sont accessibles en lecture seule par les employés du service Finance du même pays. Un groupe d'administration des finances centralisé peut accéder aux informations financières de tous les pays.  

Le déploiement d'une stratégie d'accès centralisée comprend les phases suivantes :  

|Phase|Description  
|---------|---------------  
|[Plan : identifier la nécessité d’une stratégie et la configuration requise pour le déploiement](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.2)|Identifier la nécessité d'une stratégie et la configuration requise pour le déploiement. 
|[Implémenter : configurer les composants et la stratégie](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.3)|Configurer les composants et la stratégie.  
|[Déployer la stratégie d’accès centralisée](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.4)|Déployez la stratégie.  
|[Tenir à jour : modifier et mettre en place la stratégie](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.5)|Modifications de stratégie et mise en lots. 

## <a name="BKMK_1.1"></a>Configurer un environnement de test  
Avant de commencer, vous devez configurer un laboratoire pour tester ce scénario. Les étapes de configuration du laboratoire sont décrites en détail dans l' [annexe B : configuration de l’environnement de test](Appendix-B--Setting-Up-the-Test-Environment.md).  

## <a name="BKMK_1.2"></a>Plan : identifier la nécessité d’une stratégie et la configuration requise pour le déploiement  
Cette section décrit les principales étapes de la phase de planification de votre déploiement.  

||Étape|Exemple|  
|-|--------|-----------|  
|1.1|L'activité commerciale détermine qu'une stratégie d'accès centralisée est nécessaire.|Pour protéger les informations financières stockées sur les serveurs de fichiers, le service Sécurité du service Finance collabore avec le service centralisé Sécurité des informations pour spécifier la nécessité de mettre en place une stratégie d'accès centralisée.|  
|1.2|Exprimer la stratégie d'accès|Les documents financiers ne doivent être lus que par les membres du service Finance. Les membres du service Finance ne doivent accéder qu'aux documents de leur pays. Seuls les Administrateurs financiers doivent avoir un accès en écriture. Une exception sera accordée aux membres du groupe FinanceException. Ce groupe disposera d'un accès en lecture.|  
|1.3|Exprimer la stratégie d’accès dans les constructions de Windows Server 2012|Cible :<br /><br />-Resource. Department contient finance<br /><br />Règles d'accès :<br /><br />-Autoriser Read User. Country = Resource. Country et User. Department = Resource. Department<br />-Autoriser Full Control User. MemberOf (FinanceAdmin)<br /><br />Exception :<br /><br />Allow read memberOf(FinanceException)|  
|1.4|Déterminer les propriétés de fichiers nécessaires pour la stratégie|Baliser les fichiers avec :<br /><br />-Service<br />-Pays|  
|1.5|Déterminer les types de revendications et les groupes nécessaires pour la stratégie|Types de revendications :<br /><br />-Pays<br />-Service<br /><br />Groupes d'utilisateurs :<br /><br />-FinanceAdmin<br />-FinanceException|  
|1.6|Identifier les serveurs sur lesquels appliquer cette stratégie|Appliquer la stratégie sur tous les serveurs de fichiers financiers.|  

## <a name="BKMK_1.3"></a>Implémenter : configurer les composants et la stratégie  
Cette section contient un exemple qui déploie une stratégie d'accès centralisée pour des documents financiers.  

|Non|Étape|Exemple|  
|------|--------|-----------|  
|2.1|Créer des types de revendications|Créez les types de revendications suivants :<br /><br />-Service<br />-Pays|  
|2.2|Créer des propriétés de ressource|Créez et activez les propriétés de ressource suivantes :<br /><br />-Service<br />-Pays|  
|2.3|Configurer une règle d'accès central|Créez une règle Documents financiers qui comprend la stratégie déterminée dans la section précédente.|  
|2.4|Configurer une stratégie d'accès centralisée|Créez une stratégie d'accès centralisée nommée Stratégie financière et ajoutez la règle Documents financiers à cette stratégie d'accès centralisée.|  
|2.5|Cibler la stratégie d'accès centralisée sur les serveurs de fichiers|Publiez la stratégie d'accès centralisée Stratégie financière sur les serveurs de fichiers.|  
|2.6|Activer la prise en charge par le contrôleur de domaine Kerberos des revendications, de l'authentification composée et du blindage Kerberos.|Activez la prise en charge par le contrôleur de domaine Kerberos des revendications, de l'authentification composée et du blindage Kerberos pour contoso.com.|  

Dans la procédure suivante, vous créez deux types de revendications : Country et Department.  

#### <a name="to-create-claim-types"></a>Pour créer des types de revendications  

1. Ouvrez Server DC1 dans le Gestionnaire Hyper-V et connectez-vous en tant que CONTOSO\Administrateur, avec le mot de passe <strong>pass@word1</strong>.  

2. Ouvrez le Centre d'administration Active Directory.  

3. Cliquez sur l'icône **Arborescence**, développez **Contrôle d'accès dynamique**, puis sélectionnez **Types de revendications**.  

   Cliquez avec le bouton droit sur **Types de revendications**, cliquez sur **Nouveau**, puis sur **Type de revendication**.  

   > [!TIP]  
   > Vous pouvez aussi ouvrir une fenêtre **Créer un type de revendication** dans le volet **Tâches**. Dans le volet **Tâches**, cliquez sur **Nouveau**, puis sur **Type de revendication**.  

4. Dans la liste **Attribut source**, faites défiler la liste d'attributs et cliquez sur **department**. **department** devrait apparaître dans le champ **Nom complet**. Cliquez sur **OK**.  

5. Dans le volet **Tâches**, cliquez sur **Nouveau**, puis sur **Type de revendication**.  

6. Dans la liste **Attribut source**, faites défiler la liste d'attributs et cliquez sur l'attribut **c** (Country-Name). Dans le champ **Nom complet**, tapez **country**.  

7. Dans la section **Valeurs suggérées**, sélectionnez **Les valeurs suivantes sont suggérées**, puis cliquez sur **Ajouter**.  

8. Dans les champs **Valeur** et **Nom complet**, tapez **US**, puis cliquez sur **OK**.  

9. Répétez l'étape ci-dessus. Dans la boîte de dialogue **Ajouter une valeur suggérée**, tapez **JP** dans les champs **Valeur** et **Nom complet**, puis cliquez sur **OK**.  

guides de solution ![](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  


    New-ADClaimType country -SourceAttribute c -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("US","US","")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("JP","JP","")))  
    New-ADClaimType department -SourceAttribute department  



> [!TIP]  
> Vous pouvez utiliser la Visionneuse de l'historique de Windows PowerShell dans le Centre d'administration Active Directory pour rechercher les applets de commande Windows PowerShell pour chaque procédure que vous effectuez dans le Centre d'administration Active Directory. Pour plus d’informations, voir [Visionneuse de l’historique de Windows PowerShell](https://technet.microsoft.com/library/hh831702)  

L'étape suivante consiste à créer des propriétés de ressource. Lors de la procédure suivante, vous allez créer une propriété de ressource qui est ajoutée automatiquement à la liste Propriétés de ressource globales sur le contrôleur de domaine, pour être disponible sur le serveur de fichiers.  

#### <a name="to-create-and-enable-pre-created-resource-properties"></a>Pour créer et activer des propriétés de ressource créées au préalable  

1.  Dans le volet gauche du Centre d'administration Active Directory, cliquez sur **Arborescence**. Développez **Contrôle d'accès dynamique**, puis sélectionnez **Propriétés de ressource**.  

2.  Cliquez avec le bouton droit sur **Propriétés de ressource**, cliquez sur **Nouveau**, puis sur **Propriété de ressource de référence**.  

    > [!TIP]  
    > Vous pouvez aussi choisir une propriété de ressource dans le volet **Tâches**. Cliquez sur **Nouveau**, puis sur **Propriété de ressource de référence**.  

3.  Dans la liste **Sélectionner un type de revendication pour partager ses valeurs suggérées**, cliquez sur **country**.  

4.  Dans le champ **Nom complet**, tapez **country**, puis cliquez sur **OK**.  

5.  Double-cliquez sur la liste **Propriétés de ressource** et faites-la défiler jusqu'à la propriété de ressource **Department**. Cliquez dessus avec le bouton droit, puis cliquez sur **Activer**. Cette opération permet d'activer la propriété de ressource intégrée **Department**.  

6.  Dans la liste **Propriétés de ressource** dans le volet de navigation du Centre d'administration Active Directory, vous aurez maintenant deux propriétés de ressource activées :  

    -   Country  

    -   Service  

guides de solution ![](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  

```  
New-ADResourceProperty Country -IsSecured $true -ResourcePropertyValueType MS-DS-MultivaluedChoice -SharesValuesWith country  
Set-ADResourceProperty Department_MS -Enabled $true  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Country  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Department_MS  

```  

L'étape suivante consiste à créer des règles d'accès central qui définissent qui peut accéder aux ressources. Dans ce scénario, les règles professionnelles sont les suivantes :  

-   Les documents financiers ne doivent être lus que par les membres du département Finance.  

-   Les membres du département Finance ne doivent accéder qu'aux documents de leur propre pays.  

-   Seuls les Administrateurs financiers doivent avoir un accès en écriture.  

-   Une exception sera accordée aux membres du groupe FinanceException. Ce groupe disposera d'un accès en lecture.  

-   L'administrateur et le propriétaire du document disposeront toujours d'un accès total.  

Ou pour exprimer les règles avec les constructions de Windows Server 2012 :  

Ciblage : Resource. Department contient finance  

Règles d'accès :  

-   Allow Read User.Country=Resource.Country AND User.department = Resource.Department  

-   Allow Full control User.MemberOf(FinanceAdmin)  

-   Allow Read User.MemberOf(FinanceException)  

#### <a name="to-create-a-central-access-rule"></a>Pour créer une règle d'accès central  

1. Dans le volet gauche du Centre d'administration Active Directory, cliquez sur **Arborescence**, sélectionnez **Contrôle d'accès dynamique**, puis cliquez sur **Règles d'accès central**.  

2. Cliquez avec le bouton droit sur **Règles d'accès central**, cliquez sur **Nouveau**, puis sur **Règle d'accès central**.  

3. Dans le champ **Nom**, tapez **Règle pour les documents financiers**.  

4. Dans la section **Ressources cibles**, cliquez sur **Modifier** puis, dans la boîte de dialogue **Règle d'accès central**, cliquez sur **Ajouter une condition**. Ajoutez la condition suivante :   
   [**Ressource**] [**Department**] [**Égal**] [**Valeur**] [**Finance**], puis cliquez sur **OK**.  

5. Dans la section **Autorisations**, sélectionnez **Utiliser les autorisations suivantes en tant qu'autorisations actuelles**, cliquez sur **Modifier** puis, dans la boîte de dialogue **Paramètres de sécurité avancés pour Autorisations**, cliquez sur **Ajouter**.  

   > [!NOTE]  
   > L'option **Utiliser les autorisations suivantes en tant qu'autorisations proposées** vous permet de créer la stratégie avec des phases intermédiaires. Pour plus d’informations sur la façon de procéder, consultez la section gérer : modifier et mettre en place la stratégie dans cette rubrique.  

6. Dans la boîte de dialogue **Autorisations pour Autorisations**, cliquez sur **Sélectionnez un principal**, tapez **Utilisateurs authentifiés**, puis cliquez sur **OK**.  

7. Dans la boîte de dialogue **Autorisations pour Autorisations**, cliquez sur **Ajouter une condition** et ajoutez les conditions suivantes :   
   [**Utilisateur**] [**Country**] [**Tout de**] [**Ressource**] [**Country**]   
    Cliquez sur **Ajouter une condition**.   
    [**Et**]   
   Cliquez sur [**utilisateur**] [**Department**] [**n’importe lequel de**] [**ressource**] [**Department**]. Affectez la valeur **Lecture** à **Autorisations**.  

8. Cliquez sur **OK**, puis sur **Ajouter**. Cliquez sur **Sélectionnez un principal**, tapez **AdminFinance**, puis cliquez sur **OK**.  

9. Sélectionnez les autorisations **Modification, Lecture et exécution, Lecture, Écriture**, puis cliquez sur **OK**.  

10. Cliquez sur **Ajouter**, sur **Sélectionnez un principal**, tapez **FinanceException**, puis cliquez sur **OK**. Sélectionnez les autorisations **Lecture** et **Lecture et exécution**.  

11. Cliquez sur **OK** à trois reprises pour terminer et revenir au Centre d'administration Active Directory.  

    guides de solution ![](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  

    L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  


~~~
$countryClaimType = Get-ADClaimType country  
$departmentClaimType = Get-ADClaimType department  
$countryResourceProperty = Get-ADResourceProperty Country  
$departmentResourceProperty = Get-ADResourceProperty Department  
$currentAcl = "O:SYG:SYD:AR(A;;FA;;;OW)(A;;FA;;;BA)(A;;0x1200a9;;;S-1-5-21-1787166779-1215870801-2157059049-1113)(A;;0x1301bf;;;S-1-5-21-1787166779-1215870801-2157059049-1112)(A;;FA;;;SY)(XA;;0x1200a9;;;AU;((@USER." + $countryClaimType.Name + " Any_of @RESOURCE." + $countryResourceProperty.Name + ") && (@USER." + $departmentClaimType.Name + " Any_of @RESOURCE." + $departmentResourceProperty.Name + ")))"  
$resourceCondition = "(@RESOURCE." + $departmentResourceProperty.Name + " Contains {`"Finance`"})"  
New-ADCentralAccessRule "Finance Documents Rule" -CurrentAcl $currentAcl -ResourceCondition $resourceCondition  
~~~


> [!IMPORTANT]  
> Dans l'exemple d'applet de commande ci-dessus, les identificateurs de sécurité (SID) du groupe FinanceAdmin et des utilisateurs sont déterminés au moment de la création et diffèreront dans votre exemple. Par exemple, la valeur de SID fournie (S-1-5-21-1787166779-1215870801-2157059049-1113) pour FinanceAdmins doit être remplacée par le SID du groupe FinanceAdmin que vous devrez créer dans votre déploiement. Vous pouvez utiliser Windows PowerShell pour rechercher la valeur SID de ce groupe, assigner cette valeur à une variable, puis utiliser la variable ici. Pour plus d’informations, consultez [Windows PowerShell Tip : Working with sid](https://go.microsoft.com/fwlink/?LinkId=253545).  

Vous devriez maintenant avoir une règle d'accès central qui permet aux utilisateurs d'accéder à des documents du même pays et du même service. Cette règle autorise le groupe FinanceAdmin à modifier les documents et le groupe FinanceException à lire les documents. Elle cible uniquement les documents classifiés comme financiers.  

#### <a name="to-add-a-central-access-rule-to-a-central-access-policy"></a>Pour ajouter une règle d'accès central à une stratégie d'accès centralisée  

1. Dans le volet gauche du Centre d'administration Active Directory, cliquez sur **Contrôle d'accès dynamique**, puis sur **Stratégies d'accès centralisées**.  

2. Dans le volet **Tâches**, cliquez sur **Nouveau**, puis sur **Stratégie d'accès centralisée**.  

3. Dans **Créer une stratégie d'accès centralisée**, tapez **Stratégie Finance** dans la zone **Nom**.  

4. Dans **Règles d'accès central membres**, cliquez sur **Ajouter**.  

5. Double-cliquez sur la **Règle pour les documents financiers** pour l'ajouter à la liste **Ajoutez les règles d'accès central suivantes**, puis cliquez sur **OK**.  

6. Cliquez sur **OK** pour terminer. Vous devriez maintenant avoir une stratégie d'accès centralisée nommée Stratégie Finance.  

   guides de solution ![](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  

   L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  

   ```  
   New-ADCentralAccessPolicy "Finance Policy" Add-ADCentralAccessPolicyMember   
   -Identity "Finance Policy"   
   -Member "Finance Documents Rule"  

   ```  

#### <a name="to-apply-the-central-access-policy-across-file-servers-by-using-group-policy"></a>Pour appliquer la stratégie d'accès centralisée sur les serveurs de fichiers à l'aide de la stratégie de groupe  

1.  Dans l'écran d'**accueil**, dans la zone **Rechercher**, tapez **Gestion des stratégies de groupe**. Double-cliquez sur **Gestion des stratégies de groupe**.  

    > [!TIP]  
    > Si le paramètre **Afficher les outils d’administration** est désactivé, le dossier **Outils d’administration** et son contenu ne figurent pas dans les résultats **Paramètres**.  

    > [!TIP]  
    > Dans votre environnement de production, vous devez créer une unité d'organisation Serveur de fichiers et y ajouter tous les serveurs de fichiers auxquels vous souhaitez appliquer cette stratégie. Vous pouvez ensuite créer une stratégie de groupe et ajouter cette unité d'organisation à cette stratégie.  

2.  Lors de cette étape, vous allez modifier l'objet de stratégie de groupe que vous avez créé dans la section [Créer le contrôleur de domaine](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_Build) dans l'environnement de test pour inclure la stratégie d'accès centralisée que vous avez créée. Dans le Éditeur de gestion des stratégies de groupe, accédez à l’unité d’organisation dans le domaine (contoso.com dans cet exemple) et sélectionnez-la, puis sélectionnez **gestion des stratégie de groupe**, **forêt : contoso.com**, **domaines**, **contoso.com**, **contoso**, **FileServerOU**.  

3.  Cliquez avec le bouton droit sur **FlexibleAccessGPO**, puis sur **Modifier**.  

4.  Dans la fenêtre de l'Éditeur de gestion des stratégies de groupe, accédez à **Configuration ordinateur**, développez **Stratégies**, **Paramètres Windows**, puis cliquez sur **Paramètres de sécurité**.  

5.  Développez **Système de fichiers**, cliquez avec le bouton droit sur **Stratégie d'accès centralisée**, puis cliquez sur **Gérer les stratégies d'accès centralisées**.  

6.  Dans la boîte de dialogue **Configuration des stratégies d'accès centralisées**, ajoutez **Stratégie Finance**, puis cliquez sur **OK**.  

7.  Accédez à **Configuration avancée de la stratégie d'audit** et développez cette option.  

8.  Développez **Stratégies d'audit** et sélectionnez **Accès à l'objet**.  

9. Double-cliquez sur **Auditer la stratégie d'accès centralisée intermédiaire**. Cochez les trois cases, puis cliquez sur **OK**. Cette étape permet au système de recevoir les événements d'audit liés aux stratégies d'accès centralisées intermédiaires.  

10. Double-cliquez sur **Auditer les propriétés du système de fichiers**. Cochez les trois cases, puis cliquez sur **OK**.  

11. Fermez l’éditeur de gestion des stratégies de groupe. Vous avez maintenant inclus la stratégie d'accès centralisée à la stratégie de groupe.  

Pour que les contrôleurs de domaine d’un domaine fournissent des revendications ou des données d’autorisation de périphérique, les contrôleurs de domaine doivent être configurés pour prendre en charge le contrôle d’accès dynamique.  

#### <a name="to-enable-support-for-claims-and-compound-authentication-for-contosocom"></a>Pour activer la prise en charge des revendications et de l'authentification composée pour contoso.com  

1.  Ouvrez Gestion des stratégies de groupe, cliquez sur **contoso.com**, puis sur **Contrôleurs de domaine**.  

2.  Cliquez avec le bouton droit sur **Stratégie des contrôleurs de domaine par défaut**, puis cliquez sur **Modifier**.  

3.  Dans la fenêtre Éditeur de gestion des stratégies de groupe, double-cliquez sur **Configuration ordinateur**, **Stratégies**, **Modèles d'administration**, **Système**, puis **Contrôleur de domaine Kerberos**.  

4.  Double-cliquez sur **Prise en charge par le contrôleur de domaine Kerberos des revendications, de l’authentification composée et du blindage Kerberos**. Dans la boîte de dialogue **Prise en charge par le contrôleur de domaine Kerberos des revendications, de l’authentification composée et du blindage Kerberos**, cliquez sur **Activé** et sélectionnez **Pris en charge** dans la liste déroulante **Options**. (Vous devez activer ce paramètre pour utiliser les revendications utilisateur dans les stratégies d'accès centralisées.)  

5.  Fermez **Gestion des stratégies de groupe**.  

6.  Ouvrez une invite de commande et tapez `gpupdate /force`.  

## <a name="BKMK_1.4"></a>Déployer la stratégie d’accès centralisée  

||Étape|Exemple|  
|-|--------|-----------|  
|3.1|Assigner la stratégie d'accès centralisée aux dossiers partagés appropriés sur le serveur de fichiers.|Assignez la stratégie d'accès centralisée au dossier partagé approprié sur le serveur de fichiers.|  
|3.2|Vérifier que l'accès est configuré correctement.|Vérifiez l'accès pour les utilisateurs de différents pays et services.|  

Lors de cette étape, vous allez assigner la stratégie d'accès centralisée à un serveur de fichiers. Vous allez vous connecter à un serveur de fichiers qui reçoit la stratégie d'accès centralisée que vous avez créée lors des étapes précédentes et assigner la stratégie à un dossier partagé.  

#### <a name="to-assign-a-central-access-policy-to-a-file-server"></a>Pour assigner une stratégie d'accès centralisée à un serveur de fichiers  

1. Dans le Gestionnaire Hyper-V, connectez-vous au serveur FILE1. Connectez-vous au serveur à l’aide de CONTOSO\Administrateur avec le mot de passe : <strong>pass@word1</strong>.  

2. Ouvrez une invite de commandes avec élévation de privilèges et tapez **gpupdate /force**. Cette commande permet de s'assurer que les modifications apportées à la stratégie de groupe prennent effet sur votre serveur.  

3. Vous devez aussi actualiser les propriétés de ressource globales à partir d'Active Directory. Ouvrez une fenêtre Windows PowerShell avec élévation de privilèges et tapez `Update-FSRMClassificationpropertyDefinition`. Appuyez sur Entrée et fermez Windows PowerShell.  

   > [!TIP]
   > Vous pouvez aussi actualiser les propriétés de ressource globales en vous connectant au serveur de fichiers. Pour actualiser les propriétés de ressource globales à partir du serveur de fichiers, procédez comme suit :  
   > 
   > 1. Connectez-vous au serveur de fichiers fichier1 en tant que CONTOSO\Administrateur, en utilisant le mot de passe <strong>pass@word1</strong>.  
   > 2. Ouvrez le Gestionnaire de ressources du serveur de fichiers. Pour ouvrir le Gestionnaire de ressources du serveur de fichiers, cliquez sur **Démarrer**, tapez **gestionnaire de ressources du serveur de fichiers**, puis cliquez sur **Gestionnaire de ressources du serveur de fichiers**.  
   > 3. Dans le Gestionnaire de ressources du serveur de fichiers, cliquez sur **Gestion de la classification de fichiers**, cliquez avec le bouton droit sur **Propriétés de classification**, puis cliquez sur **Actualiser**.  

4. Ouvrez l'Explorateur Windows et, dans le volet gauche, cliquez sur le lecteur D. Cliquez avec le bouton droit sur le dossier **Finance Documents**, puis cliquez sur **Propriétés**.  

5. Cliquez sur l'onglet **Classification**, sur **Country**, puis sélectionnez **US** dans le champ **Valeur**.  

6. Cliquez sur **Department**, puis sélectionnez **Finance** dans le champ **Valeur** et cliquez sur **Appliquer**.  

   > [!NOTE]  
   > Souvenez-vous que la stratégie d'accès centralisée a été configurée pour cibler les fichiers du service Finance. Les étapes précédentes marquent tous les documents du dossier avec les attributs Country et Department.  

7. Cliquez sur l'onglet **Sécurité**, puis sur **Avancé**. Cliquez sur l'onglet **Stratégie centralisée**.  

8. Cliquez sur **Modifier**, sélectionnez **Stratégie Finance** dans le menu déroulant, puis cliquez sur **Appliquer**. La **Règle pour les documents financiers** est mentionnée dans la stratégie. Développez l'élément pour afficher toutes les autorisations que vous avez définies lors de la création de la règle dans Active Directory.  

9. Cliquez sur **OK** pour revenir à Windows Explorer.  

Lors de l'étape suivante, vous allez vous assurer que l'accès est configuré correctement.  L'attribut Department approprié doit être défini pour les comptes d'utilisateur (pour le définir, utilisez le Centre d'administration Active Directory). Le plus simple pour afficher les résultats effectifs de la nouvelle stratégie consiste à utiliser l'onglet **Accès effectif** dans l'Explorateur Windows. L'onglet **Accès effectif** montre les droits d'accès pour un compte d'utilisateur donné.  

#### <a name="to-examine-the-access-for-various-users"></a>Pour examiner l'accès pour différents utilisateurs  

1.  Dans le Gestionnaire Hyper-V, connectez-vous au serveur FILE1. Ouvrez une session sur le serveur avec le compte contoso\administrateur. Accédez à D:\ dans l'Explorateur Windows. Cliquez avec le bouton droit sur le dossier **Finance Documents**, puis cliquez sur **Propriétés**.  

2.  Cliquez sur l'onglet **Sécurité**, sur **Avancé**, puis sur l'onglet **Accès effectif**.  

3.  Pour examiner les autorisations d’un utilisateur, cliquez sur **Sélectionner un utilisateur**, tapez le nom de l’utilisateur, puis cliquez sur **afficher l’accès effectif** pour afficher les droits d’accès effectifs. Par exemple :  

    -   Myriam Delesalle (MDelesalle) travaille au service Finance et doit avoir un accès en lecture au dossier.  

    -   Miles Reid (MReid) est membre du groupe FinanceAdmin et doit avoir un accès en modification au dossier.  

    -   Esther Valle (EValle) ne travaille pas au service Finance. Toutefois, elle est membre du groupe FinanceException et doit avoir un accès en lecture.  

    -   Maira Wenzel (MWenzel) ne travaille pas au service Finance et n'est pas membre du groupe FinanceAdmin ou FinanceException. Elle ne doit pas avoir accès au dossier.  

    Notez la dernière colonne nommée **Accès limité par** dans la fenêtre d'accès effectif. Cette colonne indique les portes qui affectent les autorisations de la personne. Dans ce cas, les autorisations Partager et NTFS accordent un contrôle total à tous les utilisateurs. Toutefois, la stratégie d'accès centralisée limite l'accès en fonction des règles que vous avez configurées précédemment.  

## <a name="BKMK_1.5"></a>Tenir à jour : modifier et mettre en place la stratégie  

||||  
|-|-|-|  
|Numéro|Étape|Exemple|  
|4.1|Configurer les revendications de périphérique pour les clients.|Définissez le paramètre de stratégie de groupe pour activer les revendications de périphérique.|  
|4.2|Activer une revendication pour les périphériques.|Activez le type de revendication « country » pour les périphériques.|  
|4.3|Ajouter une stratégie intermédiaire à la règle d'accès central existante que vous souhaitez modifier.|Modifiez la règle Documents financiers pour ajouter une stratégie intermédiaire.|  
|4.4|Afficher les résultats de la stratégie intermédiaire.|Vérifiez les autorisations d’ester velle.|  

#### <a name="to-set-up-group-policy-setting-to-enable-claims-for-devices"></a>Pour configurer le paramètre de stratégie de groupe pour activer les revendications pour les périphériques  

1.  Connectez-vous à DC1, ouvrez Gestion des stratégies de groupe, cliquez sur **contoso.com**, sur **Stratégie de domaine par défaut**, cliquez avec le bouton droit et sélectionnez **Modifier**.  

2.  Dans la fenêtre Éditeur de gestion des stratégies de groupe, accédez à **Configuration ordinateur**, **Stratégies**, **Modèles d'administration**, **Système** et **Kerberos**.  

3.  Sélectionnez **Prise en charge par le client Kerberos des revendications, de l'authentification composée et du blindage Kerberos** et cliquez sur **Activer**.  

#### <a name="to-enable-a-claim-for-devices"></a>Pour activer une revendication pour les périphériques  

1. Ouvrez Server DC1 dans le Gestionnaire Hyper-V et connectez-vous en tant que CONTOSO\Administrateur, avec le mot de passe <strong>pass@word1</strong>.  

2. Dans le menu **Outils**, ouvrez le Centre d'administration Active Directory.  

3. Cliquez sur **Arborescence**, développez **Contrôle d'accès dynamique**, double-cliquez sur **Types de revendications**, puis double-cliquez sur la revendication **country**.  

4. Dans **Les revendications de ce type peuvent être émises pour les classes suivantes**, cochez la case **Ordinateur**. Cliquez sur **OK**.   
   Les cases **Utilisateur** et **Ordinateur** doivent maintenant être toutes deux cochées. La revendication country peut maintenant être utilisée avec les périphériques et les utilisateurs.  

L'étape suivante consiste à créer une règle de stratégie intermédiaire. Les stratégies intermédiaires peuvent servir à contrôler les effets d'une nouvelle entrée de stratégie avant son activation. Lors de l'étape suivante, vous allez créer une entrée de stratégie intermédiaire et contrôler son effet sur votre dossier partagé.  

#### <a name="to-create-a-staging-policy-rule-and-add-it-to-the-central-access-policy"></a>Pour créer une règle de stratégie intermédiaire et l'ajouter à la stratégie d'accès centralisée  

1. Ouvrez Server DC1 dans le Gestionnaire Hyper-V et connectez-vous en tant que CONTOSO\Administrateur, avec le mot de passe <strong>pass@word1</strong>.  

2. Ouvrez le Centre d'administration Active Directory.  

3. Cliquez sur **Arborescence**, développez **Contrôle d'accès dynamique** et sélectionnez **Règles d'accès central**.  

4. Cliquez avec le bouton droit sur le dossier **Règle pour les documents financiers**, puis cliquez sur **Propriétés**.  

5. Dans la section **Autorisations proposées**, cochez la case **Activer la configuration intermédiaire des autorisations**, cliquez sur **Modifier**, puis sur **Ajouter**. Dans la fenêtre **Autorisations pour Autorisations proposées**, cliquez sur le lien **Sélectionnez un principal**, tapez **Utilisateurs authentifiés**, puis cliquez sur **OK**.  

6. Cliquez sur le lien **Ajouter une condition** et ajoutez la condition suivante :   
    [**Utilisateur**] [**country**] [**N'importe lequel de**] [**Ressource**] [**Country**].  

7. Cliquez de nouveau sur **Ajouter une condition** et ajoutez la condition suivante :  
   [**Et**]   
    [**Périphérique**] [**pays**] [**N’importe lequel de**] [**Ressource**] [**Pays**]  

8. Cliquez de nouveau sur **Ajouter une condition** et ajoutez la condition suivante.  
   Les   
    [**Utilisateur**] [**Groupe**] [**Membre de n’importe lequel**] [**Valeur**]\(**FinanceException**)  

9. Pour définir le groupe FinanceException, cliquez sur **Ajouter des éléments** et, dans la fenêtre **Sélectionner Utilisateur, Ordinateur, Compte de service ou Groupe**, tapez **FinanceException**.  

10. Cliquez sur **Autorisations**, sélectionnez **Contrôle total**, puis cliquez sur **OK**.  

11. Dans la fenêtre Paramètres de sécurité avancés pour Autorisations proposées, sélectionnez **FinanceException** et cliquez sur **Supprimer**.  

12. Cliquez sur **OK** à deux reprises pour terminer.  

guides de solution ![](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  

```  
Set-ADCentralAccessRule  
-Identity: "CN=FinanceDocumentsRule,CN=CentralAccessRules,CN=ClaimsConfiguration,CN=Configuration,DC=Contoso.com"  
-ProposedAcl: "O:SYG:SYD:AR(A;;FA;;;BA)(A;;FA;;;SY)(A;;0x1301bf;;;S-1-21=1426421603-1057776020-1604)"  
-Server: "WIN-2R92NN8VKFP.Contoso.com"  

```  

> [!NOTE]  
> Dans l'exemple d'applet de commande ci-dessus, la valeur Server correspond au serveur dans l'environnement de laboratoire. Vous pouvez utiliser la Visionneuse de l'historique de Windows PowerShell pour rechercher les applets de commande Windows PowerShell pour chaque procédure que vous effectuez dans le Centre d'administration Active Directory. Pour plus d’informations, voir [Visionneuse de l’historique de Windows PowerShell](https://technet.microsoft.com/library/hh831702)  

Dans ce jeu d'autorisations proposé, les membres du groupe FinanceException disposeront d'un accès total aux fichiers dans leur propre pays quand ils y accèderont par l'intermédiaire d'un périphérique du même pays que le document. Des entrées d'audit sont disponibles dans le journal de sécurité Serveurs de fichiers quand un membre du service Finance tente d'accéder à des fichiers. Toutefois, les paramètres de sécurité ne sont pas appliqués tant que la stratégie intermédiaire n'a pas été promue.  

Lors de la procédure suivante, vous allez vérifier les résultats de la stratégie intermédiaire. Vous allez accéder au dossier partagé avec un nom d'utilisateur ayant des autorisations basées sur la règle active. Esther Valle (EValle) est membre du groupe FinanceException et elle dispose actuellement de droits de Lecture. Selon notre stratégie intermédiaire, EValle ne doit disposer d'aucun droit.  

#### <a name="to-verify-the-results-of-the-staging-policy"></a>Pour vérifier les résultats de la stratégie intermédiaire  

1. Connectez-vous au serveur de fichiers FILE1 dans le Gestionnaire Hyper-V et ouvrez une session en tant que CONTOSO\Administrateur, avec le mot de passe <strong>pass@word1</strong>.  

2. Ouvrez une fenêtre Invite de commandes et tapez **gpupdate /force**. Cette commande permet de s'assurer que les modifications apportées à la stratégie de groupe prennent effet sur votre serveur.  

3. Dans le Gestionnaire Hyper-V, connectez-vous au serveur CLIENT1. Déconnectez l'utilisateur actuellement connecté. Redémarrez l'ordinateur virtuel CLIENT1. Connectez-vous ensuite à l’ordinateur à l’aide de contoso\EValle pass@word1.  

4. Double-cliquez sur le raccourci du Bureau pour \\documents \FILE1\Finance. EValle doit encore avoir accès aux fichiers. Revenez à FILE1.  

5. Ouvrez l'**Observateur d'événements** à partir du raccourci sur le Bureau. Développez **Journaux Windows**, puis sélectionnez **Sécurité**. Ouvrez les entrées avec l' **ID d’événement 4818**dans la catégorie tâche de la tâche de transit de la **stratégie d’accès centralisée** . Vous constaterez que l'accès a été accordé à EValle. Toutefois, conformément à la stratégie intermédiaire, l'accès aurait été refusé à cet utilisateur.  

## <a name="next-steps"></a>Étapes suivantes  
Si vous disposez d'un système de gestion de serveur centralisé tel que System Center Operations Manager, vous pouvez aussi configurer la surveillance des événements. Cela permet aux administrateurs de contrôler les effets des stratégies d'accès centralisées avant de les appliquer.  


