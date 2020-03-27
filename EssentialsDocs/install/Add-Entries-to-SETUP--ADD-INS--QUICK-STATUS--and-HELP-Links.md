---
title: Ajout d’entrées pour les listes CONFIGURATION, COMPLÉMENTS, ÉTAT RAPIDE et des liens vers l'AIDE
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0a8f10d-fd85-4c8d-b9bb-176cb1db1f46
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 074e4e638a1fe96bedf2c8340ec71848a8fa4ac4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310269"
---
# <a name="add-entries-to-setup-add-ins-quick-status-and-help-links"></a>Ajout d’entrées pour les listes CONFIGURATION, COMPLÉMENTS, ÉTAT RAPIDE et des liens vers l'AIDE

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous pouvez ajouter des tâches aux listes **CONFIGURATION**, **COMPLÉMENTS**, **ÉTAT RAPIDE**, et vous pouvez ajouter des liens a la section Liens vers d'autres communautés dans la page d'accueil du Tableau de bord. Pour ce faire, il convient de placer un fichier XML appelé OEMHomePageContent.home ou un fichier de ressources incorporé appelé OEMHomePageContent.dll dans %ProgramFiles%\Windows Server\Bin\Addins\Home. Le fichier de ressources incorporé peut être utilisé pour localiser le texte dans les tâches et les liens que vous ajoutez. Le fichier .home contient les définitions XML des tâches et des liens.  
  
## <a name="adding-tasks-to-the-setup-add-ins-quick-status-task-lists-and-adding-links-to-help-task"></a>Ajout de tâches aux listes CONFIGURATION, COMPLÉMENTS, ÉTAT RAPIDE et ajout de liens à la tâche AIDE  
 Vous pouvez ajouter des tâches aux listes **CONFIGURATION**, **COMPLÉMENTS**, **ÉTAT RAPIDE** et des liens à la tâche **AIDE** en définissant les tâches et les liens à l'aide d'XML, en créant en option un fichier de ressource incorporé, et en installant le fichier sur le serveur. Si le fichier XML est installé sur le serveur sans fichier de ressources, il doit avoir pour nom OEMHomePageContent.home. Si une assembly est utilisée pour installer un fichier XML et un fichier de ressources, il doit avoir pour nom OEMHomePageContent.dll et disposer d’une signature Authenticode.  
  
### <a name="define-the-tasks-and-links"></a>Définition des tâches et des liens  
 Vous pouvez créer le fichier .home à l’aide d’un éditeur de texte (tel que le Bloc-notes) ou utiliser Visual Studio 2010 (ou une version ultérieure) si vous comptez également définir un fichier de ressources incorporé. Procédez comme suit pour créer les fichiers avec Visual Studio 2010 (ou une version ultérieure).  
  
##### <a name="to-define-the-tasks-and-links"></a>Pour définir les tâches et les liens  
  
1. Ouvrez Visual Studio 2010 (ou une version ultérieure) en tant qu’administrateur en cliquant avec le bouton droit sur le programme dans le menu Démarrer, puis en sélectionnant **Exécuter en tant qu’administrateur**.  
  
2. Cliquez sur **Fichier**, sur **Nouveau**, puis sur **Projet**.  
  
3. Dans le volet **Modèles**, cliquez sur **Bibliothèque de classes**, entrez **OEMHomePageContent** dans le champ **Nom**, puis cliquez sur **OK**.  
  
4. Supprimez le fichier Class1.cs.  
  
5. Cliquez avec le bouton droit sur le nouveau projet, cliquez sur **Ajouter**, puis sur **Nouvel élément**.  
  
6. Dans le volet **Modèles**, cliquez sur **Fichier XML**, entrez **OEMHomePageContent.home** dans le champ **Nom**, puis cliquez sur **Ajouter**.  
  
   > [!NOTE]
   >  Si le fichier XML est installé sans fichier de ressources, il doit être avoir pour nom OEMHomePageContent.home. S’il est exclu dans une assembly, le nom du fichier peut être quelconque tant que son extension est .home.  
  
7. Ajoutez le code XML suivant dans le fichier OEMHomePageContent.home :  
  
   ```  
  
   <Tasks version=?2.0? xmlns=?https://schemas.microsoft.com/WindowsServerSolutions/2010/01/Dashboard>  
      <SetupMyServerTasks>  
         <Task name="MyTask"  
            description="MyTaskDescription"  
            id="GUID">  
                 <Action   
                 name=?MyAction1Name?   
                 image=?IconForAction1?  
                 type=?TaskType?  
                 exelocation=?ActionExeLocation? />  
                 <Action   
                 name=?MyAction2Name?   
                 image=?IconForAction2?  
                 type=?TaskType?  
                 exelocation=?ActionExeLocation? />  
                  ¦  
          </Task>  
                  ¦  
       </SetupMyServerTasks>  
   <MailServiceTasks>  
        <!-- Same schema as in œSetupMyServerTasks? but the tasks are shown in œConnect to Email Service? category. -->  
   </MailServiceTasks>  
   <LineOfBusinessTasks>  
        <!-- Same schema as in œSetupMyServerTasks? but the tasks are shown in œAdd-ins? category. -->  
  
   <GetQuickStatusTasks>  
         <Task name="MyQuickStatusTask1"  
            description="MyQuickStatusTask1Desc   "  
            id="GUID"  
            assembly="AssemblyName of quick status query implementation"  
            class="ClassName of quick status query implementation"           
            replaceid="GUID"/>  
              <!--  Same schema as Actions in œSetupMyServerTasks? -->   
            </Task>  
   </GetQuickStatusTasks>  
      <Links>  
         <Link  
            ID=?GUID?  
            Title="Displayed text of the link"  
            Description="A very short description"  
            ShellExecPath="Path to the application or URL"/>  
      </Links>  
   </Tasks>  
   ```  
  
    Où :  
  
   |Attribut|Description|  
   |---------------|-----------------|  
   |Name (Task)|Nom d'affichage de la tâche dans la liste. Si vous créez un fichier de ressources incorporé, la valeur de cet attribut correspond à la ressource de chaîne.|  
   |description (Task)|Description de la tâche. Si vous créez un fichier de ressources incorporé, la valeur de cet attribut correspond à la ressource de chaîne.|  
   |id (Task)|Identificateur de la tâche. Cet identificateur doit être un GUID. Vous créez un nouveau GUID pour une tâche **exe** , mais pour une tâche **globale** , vous utilisez le GUID que vous avez créé lors de la définition de la tâche pour le volet des tâches du sous-onglet. Pour plus d’informations sur la création d’un GUID, consultez [Create GUID (Guidgen. exe)](https://go.microsoft.com/fwlink/?LinkId=116098).|  
   |image|Ce champ sera ignoré.|  
   |Name (Action)|Affiche le nom de la tâche.|  
   |Type (Action)|Décrit le type de tâche. La tâche peut être l’une des tâches suivantes : tâche **globale**, **exe**ou URL. Une tâche **globale** est la même tâche globale que vous avez créée lors de la définition des tâches du volet de tâches dans le sous-onglet. Pour plus d’informations sur la création d’une tâche globale qui peut être utilisée à la fois dans le volet tâches du sous-onglet et dans les listes tâches Prise en main ou tâches courantes de la page d’hébergement, consultez œCreating the support classes ? dans Comment à : créer un sous-onglet ? du [Kit de développement logiciel (SDK) des solutions Windows Server](https://go.microsoft.com/fwlink/?LinkID=248648). Une tâche **exe** permet d'exécuter des applications à partir des listes Tâches de mise en route ou Tâches courantes.|  
   |exelocation|Chemin d'accès à l'application associée à la tâche. Cet attribut s'applique uniquement aux tâches **exe**.|  
   |replaceid|Identificateur de la tâche remplacée par cette tâche.|  
   |assembly|L’AssemblyName de l’assembly qui fournit la classe afin de mettre en œuvre une requête de l’état rapide. L’assembly doit se trouver dans Program Files \ Windows Server\Bin\\.|  
   |classe|Le nom de la classe met en œuvre la requête de l’état rapide. La classe doit mettre en œuvre l’interface **ITaskStatusQuery**.|  
   |Title (link)|Texte affiché en guise de lien. Si vous créez un fichier de ressources incorporé, la valeur de cet attribut correspond à la ressource de chaîne.|  
   |Description (link)|Description de la destination du lien. Si vous créez un fichier de ressources incorporé, la valeur de cet attribut correspond à la ressource de chaîne.|  
   |ShellExecPath|Chemin d'accès à l'application ou adresse URL.<br /><br /> **Remarque :** Les variables d’environnement sont prises en charge dans l’attribut ShellExecPath.|  
  
    L'exemple de code suivant montre comment définir un lien vers une application :  
  
   ```  
   <Links>  
      <Link Title="Calc" Description="Launches Calc" ShellExecPath="%windir%\system32\calc.exe" />  
   </Links>  
   ```  
  
    L'exemple de code suivant montre comment définir un lien vers une page Web :  
  
   ```  
   <Links>  
      <Link Title="Browser" Description="Open browser" ShellExecPath="http://www.adventureworks.com/" />  
   </Links>  
   ```  
  
8. Changez les valeurs d'attribut pour représenter votre tâche ou votre lien.  
  
9. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur **OEMHomePageContent.home**, puis cliquez sur **Propriétés**.  Dans le volet **Propriétés**, sous **Action de génération**, sélectionnez **Ressource incorporée**.  
  
10. Enregistrez le fichier OEMHomePageContent.home.  
  
    Pour savoir comment mettre en œuvre une requête de l’état rapide, consultez les documents et exemples fournis dans le [SDK Windows Server Solutions](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
#### <a name="change-the-status-of-a-setupadd-ins-task"></a>Modification de l'état d'une tâche CONFIGURATION/COMPLÉMENTS  
 L'état des tâches référencées dans les listes CONFIGURATION et COMPLÉMENTS peuvent être basculées entre Complété (configuré pour les compléments) et Non complété (pas configuré pour les compléments).  
  
 Lorsque vous définissez l'application associée à votre nouvelle tâche, vous pouvez utiliser la méthode SetTaskStatus de l'espace de noms Microsoft.WindowsServerSolutions.Administration.ObjectModel.TaskStatusHelper (fourni, mais non documenté dans le Kit de développement logiciel (SDK) des solutions Windows Server) pour modifier l'état de la tâche. Par exemple, vous pouvez faire passer la couleur de la coche du gris au vert en appelant la méthode SetTaskStatus avec la valeur d'énumération TaskStatus.Complete (SetTaskStatus(id, TaskStatus.Complete), où **id** correspond à l'identifiant de la tâche). Les valeurs d'énumération qui peuvent être utilisées sont TaskStatus.Complete, TaskStatus.Incomplete ou TaskStatus.Hidden.  
  
##### <a name="replace-tasks"></a>Remplacer des tâches  
 Rien ne vous empêche de remplacer les tâches prédéfinies dans les listes Tâches de mise en route ou Tâches courantes en ajoutant le GUID de votre nouvelle tâche à l'attribut replaceid de la définition de la tâche. Le tableau suivant indique les tâches et les identificateurs correspondants susceptibles d'être remplacés dans le Tableau de bord :  
  
|Nom de la tâche|Identificateur|  
|---------------|----------------|  
|Obtenir des mises à jour pour d'autres produits Microsoft|8412D35A-13EE-4112-AE0B-F7DBC83EA83D|  
|Configurer la sauvegarde du serveur|F68B3F3F-19DE-499D-9ACB-4BB41B8FF420|  
|Configurer l'accès à distance|8991302D-676A-4A7C-B244-D1E08AE0EFEA|  
|Configurer la notification des alertes par courrier électronique|DE6F2B36-F19C-4FAF-998B-9772300E3530|  
|Ajouter un compte d'utilisateur|6D5B5D5F-2EC7-4B1F-9580-4DB084B278B1|  
|Ajouter des dossiers de serveur|03F1F438-D94E-439B-A9F7-0C817C37D625|  
|Accès en tout lieu - État rapide|6093B462-1F04-4212-8804-9BC823070FAD|  
|Sauvegarde du serveur - État rapide|156947D8-21DC-45FE-A9A8-2F80AE304191|  
|Dossiers de serveur - État rapide|C657E8AB-AC1F-4AA1-8E80-5AF6BB27C314|  
|Comptes d’utilisateur actifs - État rapide|68BCB125-CE8F-467F-B65B-56AD45A614B5|  
|Notifications des alertes par courrier électronique - État rapide|75AB06E7-A679-4D62-A5EC-65362FE4F9DB|  
|Ordinateurs - État rapide|7966A974-D52D-4F5D-B37F-05C1B73CEEF3|  
  
##### <a name="optional-create-the-resource-file"></a>(Facultatif) Création du fichier de ressources  
 Si vous avez l'intention de localiser le texte dans les tâches/liens ajoutés aux listes Tâches de mise en route et Tâches courantes ou à la section Liens vers d'autres communautés, vous devez créer un assembly contenant le fichier .home et le fichier .home.resx de définition des chaînes de texte.  
  
###### <a name="to-create-the-resource-file"></a>Pour créer le fichier de ressources  
  
1.  Cliquez avec le bouton droit sur le projet créé pour vos tâches, cliquez sur **Ajouter**, puis sur **Nouvel élément**.  
  
2.  Dans le volet **Modèles**, cliquez sur **Fichier de ressources**, entrez **OEMHomePageContent.home.resx** dans le champ **Nom**, puis cliquez sur **Ajouter**.  
  
    > [!NOTE]
    >  Vous êtes libre de l'appeler comme bon vous semble, à condition de lui donner l'extension .home.resx.  
  
3.  Pour chacune des tâches ou des liens que vous ajoutez, n'oubliez pas d'insérer dans le fichier OEMHomePageContent.home.resx les chaînes et les valeurs correspondant aux éléments Tâche et Lien définis dans le fichier OEMHomePageContent.home. Le code suivant représente un exemple de fichier Tasks.xml structuré pour le fichier de ressources :  
  
    ```  
  
    <Tasks version=?2.0? xmlns=?https://schemas.microsoft.com/WindowsServerSolutions/2010/01/Dashboard>  
       <SetupMyServerTasks>  
          <Task name="MyTask"  
             description="MyDescription"  
             id="GUID">  
             <Action  
             name="MyActionname"  
             image="IconForAction"  
             type="TaskType"  
             exelocation="ActionExeLocation" />  
          </Task>  
       </SetupMyServerTasks>  
    </Tasks>  
  
    ```  
  
    > [!NOTE]
    >  Les identificateurs des attributs utilisés pour la localisation ne peuvent pas contenir d'espaces.  
  
4.  Ajoutez les noms de ressources MyTask, MyTaskDescription, MyActionName et IconForAction au fichier .resx et affectez-leur les valeurs appropriées.  
  
5.  Enregistrez le fichier OEMHomePageContent.home.resx, puis générez la solution.  
  
#####  <a name="sign-the-assembly-with-an-authenticode-signature"></a><a name="BKMK_SignAssembly"></a>Signer l’assembly avec une signature Authenticode  
 Vous devez signer l'assembly avec une signature Authenticode pour pouvoir l'utiliser dans le système d'exploitation. Pour plus d’informations sur la signature de l’assembly, consultez [Signature et vérification d’un code avec Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
##### <a name="install-the-task-files"></a>Installation des fichiers de tâches  
 Après avoir créé les fichiers .home et .home.resx, vous devez les installer sur le serveur.  
  
###### <a name="to-install-the-task-files"></a>Pour installer les fichiers de tâches  
  
1.  Assurez-vous que la solution se génère sans erreur.  
  
2.  Si vous n'avez pas prévu de fichier de ressources incorporé, copiez le fichier OEMHomePageContent.home dans **%ProgramFiles%\Windows Server\Bin\Addins\Home** sur le serveur. Si vous avez créé un fichier de ressources incorporé, copiez le fichier OEMHomePageContent.dll dans **%ProgramFiles%\Windows Server\Bin\Addins\Home** sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)