---
ms.assetid: 22347a94-aeea-44b4-85fb-af2c968f432a
title: "Déployer la sécurité de l’audit avec les stratégies d’Audit centralisées (étapes de démonstration)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac2b1643ed151e94c3815abca9a57eb3706c845a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="deploy-security-auditing-with-central-audit-policies-demonstration-steps"></a>Déployer la sécurité de l’audit avec les stratégies d’Audit centralisées (étapes de démonstration)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Dans ce scénario, vous allez contrôler l’accès aux fichiers dans le dossier Documents financiers à l’aide de la stratégie financière que vous avez créé dans [déployer une stratégie d’accès centralisée et #40; étapes de démonstration et #41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md). Si un utilisateur qui n’est pas autorisé à accéder au dossier tente d’y accéder, l’activité est capturée dans l’Observateur d’événements.   
 Les étapes suivantes sont requises pour tester ce scénario.  
  
|Tâche|Description|  
|--------|---------------|  
|[Configurer l’accès Global aux objets](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_1)|Dans cette étape, vous configurez la stratégie d’accès globale d’objet sur le contrôleur de domaine.|  
|[Paramètres de stratégie de groupe de mise à jour](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_2)|Connectez-vous au serveur de fichiers et appliquer la mise à jour de stratégie de groupe.|  
|[Vérifiez que la stratégie d’accès globale d’objet a été appliquée.](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_3)|Afficher les événements pertinents dans l’Observateur d’événements. Les événements doivent inclure les métadonnées pour le type de document et pays.|  
  
## <a name="BKMK_1"></a>Configurer la stratégie d’accès globale d’objet  
Dans cette étape, vous configurez la stratégie d’accès globale d’objet dans le contrôleur de domaine.  
  
#### <a name="to-configure-a-global-object-access-policy"></a>Pour configurer une stratégie d’accès aux objets globaux  
  
1.  Connectez-vous au contrôleur de domaine DC1 en tant que Contoso\Administrateur avec le mot de passe **pass@word1**.  
  
2.  Dans le Gestionnaire de serveur, pointez sur **outils**, puis cliquez sur **gestion des stratégies de groupe **.  
  
3.  Dans l’arborescence de la console, double-cliquez sur **domaines**, double-cliquez sur **contoso.com**, cliquez sur **Contoso**, puis double-cliquez sur **serveurs de fichiers **.  
  
4.  Avec le bouton droit **FlexibleAccessGPO**, puis cliquez sur **modifier **.  
  
5.  Double-cliquez sur **Configuration ordinateur**, double-cliquez sur **stratégies**, puis double-cliquez sur **paramètres Windows **.  
  
6.  Double-cliquez sur **paramètres de sécurité**, double-cliquez sur **Configuration avancée de stratégie d’Audit**, puis double-cliquez sur **stratégies d’Audit **.  
  
7.  Double-cliquez sur **l’accès aux objets**, puis double-cliquez sur **auditer le système de fichiers **.  
  
8.  Sélectionnez le **configurer les événements suivants** case à cocher, sélectionnez le **réussite** et **échec** cases à cocher, puis cliquez sur **OK **.  
  
9. Dans le volet de navigation, double-cliquez sur **audit Global aux objets accès**, puis double-cliquez sur **système de fichiers **.  
  
10. Sélectionnez le **définir ce paramètre de stratégie** case à cocher, puis cliquez sur **configurer **.  
  
11. Dans le **paramètres de sécurité avancés pour SACL de fichier Global**, cliquez sur **ajouter**, puis cliquez sur **sélectionnez un principal**, type **tout le monde**, puis cliquez sur **OK **.  
  
12. Dans le **entrée d’audit pour SACL de fichier Global** zone, sélectionnez **contrôle total** dans les **autorisations** zone.  
  
13. Dans le **ajouter une condition:**, cliquez sur **ajouter une condition** et dans la liste déroulante répertorie sélectionnez   
    [**Ressource**] [**Service**] [**Any of**] [**Value**] [**Finance**].  
  
14. Cliquez sur **OK** trois fois pour terminer la configuration de l’accès global aux objets d’audit de paramètre de stratégie.  
  
15. Dans le volet de navigation, cliquez sur **l’accès aux objets**et dans le volet des résultats, double-cliquez sur **d’Audit de Manipulation de Handle **. Cliquez sur **configurer les événements d’audit suivants**, **réussite**, et **échec**, cliquez sur **OK**, puis fermez l’objet stratégie de groupe d’accès flexible.  
  
## <a name="BKMK_2"></a>Mettre à jour les paramètres de stratégie de groupe  
Dans cette étape, vous mettez à jour les paramètres de stratégie de groupe après avoir créé la stratégie d’audit.  
  
#### <a name="to-update-group-policy-settings"></a>Pour mettre à jour les paramètres de stratégie de groupe  
  
1.  Connectez-vous au serveur de fichiers, FILE1 en tant que Contoso\Administrateur avec le mot de passe **pass@word1**.  
  
2.  Appuyez sur la touche Windows + R, puis tapez **cmd** pour ouvrir une fenêtre d’invite de commandes.  
  
    > [!NOTE]  
    > Si le **contrôle de compte d’utilisateur** boîte de dialogue s’affiche, vérifiez que l’action affichée est ce que vous souhaitez, puis cliquez sur **Oui **.  
  
3.  Type **gpupdate /force** et appuyez sur ENTRÉE.  
  
## <a name="BKMK_3"></a>Vérifiez que la stratégie d’accès globale d’objet a été appliquée.  
Une fois les paramètres de stratégie de groupe ont été appliqués, vous pouvez vérifier que les paramètres de stratégie d’audit ont été appliquées correctement.  
  
#### <a name="to-verify-that-the-global-object-access-policy-has-been-applied"></a>Pour vérifier que la stratégie d’accès globale d’objet a été appliquée.  
  
1.  Connectez-vous à l’ordinateur client, CLIENT1 en tant que Contoso\MReid. Accédez au dossier lien hypertexte «file:///\\\ID_AD_FILE1\\\Finance» \\\ FILE1\Finance Documents et modifiez Document Word 2.  
  
2.  Connectez-vous au serveur de fichiers, FILE1 en tant que Contoso\Administrateur. Ouvrez l’Observateur d’événements, accédez au **journaux Windows**, sélectionnez **sécurité**et confirmez que vos activités ont généré des événements d’audit **4656** et **4663** (même si vous n’avez pas défini des listes SACL d’audit explicite sur les fichiers ou dossiers que vous avez créé, modifiés et supprimés).  
  
> [!IMPORTANT]  
> Un événement d’ouverture de session est généré sur l’ordinateur où la ressource se trouve, pour le compte de l’utilisateur pour lequel accès effectif est en cours de la version vérifié. Lors de l’analyse des journaux d’audit de sécurité de connexion activité utilisateur, pour différencier les événements d’ouverture de session qui sont générés en raison de l’accès effectif et ceux qui sont générés en raison d’un utilisateur réseau interactive s’y connecter, les informations au niveau d’emprunt d’identité sont incluses. Lorsque l’événement d’ouverture de session est généré en raison de l’accès effectif, le niveau d’emprunt d’identité sera identité. En règle générale, une connexion interactive de l’utilisateur réseau génère un événement d’ouverture de session avec le niveau d’emprunt d’identité = emprunt d’identité ou délégation.  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Scénario: Audit d’accès du fichier](Scenario--File-Access-Auditing.md)  
  
-   [Planifier le fichier de l’audit d’accès](Plan-for-File-Access-Auditing.md)  
  
-   [Contrôle d’accès dynamique: Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  

