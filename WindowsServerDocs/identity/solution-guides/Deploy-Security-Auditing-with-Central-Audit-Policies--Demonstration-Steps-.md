---
ms.assetid: 22347a94-aeea-44b4-85fb-af2c968f432a
title: Déployer l’audit de sécurité avec les stratégies d’audit centralisées (étapes de démonstration)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ecbaa33d83d7b37f376a426571c0d2df89c7695d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407116"
---
# <a name="deploy-security-auditing-with-central-audit-policies-demonstration-steps"></a>Déployer l’audit de sécurité avec les stratégies d’audit centralisées (étapes de démonstration)

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans ce scénario, vous allez auditer l’accès aux fichiers dans le dossier Finance documents à l’aide de la stratégie financière que vous avez créée dans les [ &#40;étapes&#41;de la démonstration déployer une stratégie d’accès centralisée](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md). Si un utilisateur non autorisé tente d’accéder au dossier, l’activité est capturée dans l’Observateur d’événements.   
 Les étapes suivantes sont requises pour tester ce scénario.  
  
|Tâche|Description|  
|--------|---------------|  
|[Configurer l’accès global aux objets](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_1)|Au cours de cette étape, vous configurez la stratégie d’accès global aux objets au niveau du contrôleur de domaine.|  
|[Mettre à jour les paramètres de stratégie de groupe](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_2)|Connectez-vous au serveur de fichiers et appliquez la mise à jour de la stratégie de groupe.|  
|[Vérifier que la stratégie d’accès global aux objets a été appliquée](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_3)|Affichez les événements pertinents dans l’Observateur d’événements. Les événements doivent inclure les métadonnées relatives au pays et au type de document.|  
  
## <a name="BKMK_1"></a>Configurer la stratégie d’accès global aux objets  
Au cours de cette étape, vous configurez la stratégie d’accès global aux objets au niveau du contrôleur de domaine.  
  
#### <a name="to-configure-a-global-object-access-policy"></a>Pour configurer une stratégie d’audit d’accès global aux objets  
  
1. Connectez-vous au contrôleur de domaine DC1 en tant que CONTOSO\Administrateur avec le mot de passe <strong>pass@word1</strong>.  
  
2. Dans le Gestionnaire de serveur, pointez sur **Outils**, puis cliquez sur **Gestion de stratégie de groupe**.  
  
3. Dans l’arborescence de la console, double-cliquez sur **Domaines**, sur **contoso.com**, cliquez sur **Contoso**, puis double-cliquez sur **Serveurs de fichiers**.  
  
4. Cliquez avec le bouton droit sur **FlexibleAccessGPO**, puis sur **Modifier**.  
  
5. Double-cliquez sur **Configuration ordinateur**, sur **Stratégies**, puis sur **Paramètres Windows**.  
  
6. Double-cliquez sur **Paramètres de sécurité**, sur **Configuration avancée de la stratégie d’audit**, puis sur **Stratégies d’audit**.  
  
7. Double-cliquez sur **Accès à l’objet**, puis sur **Auditer le système de fichiers**.  
  
8. Activez successivement les cases à cocher **Configurer les événements d’audit suivants**, **Succès**, **Échec**, puis cliquez sur **OK**.  
  
9. Dans le volet de navigation, double-cliquez sur **Audit de l’accès global aux objets**, puis sur **Système de fichiers**.  
  
10. Activez la case à cocher **Définir ce paramètre de stratégie**, puis cliquez sur **Configurer**.  
  
11. Dans la zone **Paramètres de sécurité avancés pour SACL globale d’accès aux fichiers**, cliquez sur **Ajouter**, puis sur **Sélectionnez un principal**, tapez **Tout le monde**, puis cliquez sur **OK**.  
  
12. Dans la zone **Audits pour SACL globale d’accès aux fichiers**, activez **Contrôle total** dans la zone **Autorisations**.  
  
13. Dans la section **Ajouter une condition :** , cliquez sur **Ajouter une condition** , puis dans les listes déroulantes, sélectionnez   
    [**Ressource**] [**Service**] [**Tout de**] [**Valeur**] [**Finance**].  
  
14. Cliquez trois fois sur **OK** pour terminer la configuration du paramètre de la stratégie d’audit de l’accès global aux objets.  
  
15. Dans le volet de navigation, cliquez sur **Accès à l’objet**, puis dans le volet des résultats, double-cliquez sur **Auditer la manipulation de handle**. Activez successivement les cases à cocher **Configurer les événements d’audit suivants**, **Succès**, **Échec**, puis cliquez sur **OK** et fermez l’objet Stratégie de groupe d’accès souple.  
  
## <a name="BKMK_2"></a>Mettre à jour les paramètres de stratégie de groupe  
Au cours de cette étape, vous mettez à jour les paramètres de stratégie de groupe après avoir créé la stratégie d’audit.  
  
#### <a name="to-update-group-policy-settings"></a>Pour mettre à jour les paramètres Stratégie de groupe  
  
1. Connectez-vous au serveur de fichiers, FILE1 en tant que CONTOSO\Administrateur, avec le mot de passe <strong>pass@word1</strong>.  
  
2. Appuyez sur la touche Windows + R, tapez **cmd** pour ouvrir une fenêtre d’invite de commandes.  
  
   > [!NOTE]  
   > Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
3. Tapez **gpupdate /force** et appuyez sur Entrée.  
  
## <a name="BKMK_3"></a>Vérifier que la stratégie d’accès global aux objets a été appliquée  
Une fois les paramètres Stratégie de groupe appliqués, vérifiez que les paramètres de stratégie d’audit ont été appliqués correctement.  
  
#### <a name="to-verify-that-the-global-object-access-policy-has-been-applied"></a>Pour vérifier que la stratégie d’accès global aux objets a été appliquée  
  
1.  Connectez-vous à l’ordinateur client, CLIENT1 en tant que Contoso\MReid. Accédez au dossier HYPERLINK « file:///\\ @ no__t-1 @ no__t-2\ID_AD_FILE1 @ no__t-3\Finance » \\ \ FILE1\Finance documents, puis modifiez le document Word 2.  
  
2.  Connectez-vous au serveur de fichiers FICHIER1 en tant que contoso\Administrateur. Ouvrez l’Observateur d’événements, accédez à **Journaux Windows**, sélectionnez **Sécurité**, puis confirmez que vos activités ont généré les événements d’audit **4656** et **4663** (même si vous n’avez pas défini explicitement des listes SACL d’audit sur les fichiers ou les dossiers que vous avez créés, modifiés et supprimés).  
  
> [!IMPORTANT]  
> Un nouvel événement d’ouverture de session est généré sur l’ordinateur qui contient la ressource, à la place de l’utilisateur pour lequel l’accès effectif est en cours de vérification. Lors de l’analyse des journaux d’audit de sécurité relative à l’activité de connexion utilisateur, le niveau d’emprunt d’identité est inclus afin de différencier les événements d’ouverture de session qui sont générés en raison de l’accès effectif et ceux qui sont générés via la connexion interactive de l’utilisateur au réseau. Dans le premier cas, le niveau d’emprunt d’identité sera Identité. La connexion interactive de l’utilisateur au réseau génère normalement un événement d’ouverture de session avec le niveau d’emprunt d’identité = Emprunt d’identité ou Délégation.  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Scénario : audit d’accès aux fichiers](Scenario--File-Access-Auditing.md)  
  
-   [Planifier l’audit d’accès aux fichiers](Plan-for-File-Access-Auditing.md)  
  
-   [Contrôle d’accès dynamique : Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  

