---
title: "Ajouter des entrées pour configurer des compléments, état rapide et des liens d’aide"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0a8f10d-fd85-4c8d-b9bb-176cb1db1f46
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6d3303f2c6d84932ad9d5dee8a547cd478447732
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="add-entries-to-setup-add-ins-quick-status-and-help-links"></a>Ajouter des entrées pour configurer des compléments, état rapide et des liens d’aide

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Vous pouvez ajouter des tâches à le **le programme d’installation**, **compléments**, **état rapide** listes, des tâches et vous pouvez ajouter des liens à la section liens de la Communauté dans la page d’accueil du tableau de bord. Tâches et des liens sont ajoutés à ces listes et de la section en plaçant un fichier XML appelé OEMHomePageContent.home fichier ou un fichier de ressources incorporé appelé OEMHomePageContent.dll dans %ProgramFiles%\Windows Server\Bin\Addins\Home. Le fichier de ressources incorporé peut être utilisé pour localiser le texte dans les tâches et les liens que vous ajoutez. Le fichier .home contient les définitions XML des tâches et des liens.  
  
## <a name="adding-tasks-to-the-setup-add-ins-quick-status-task-lists-and-adding-links-to-help-task"></a>Ajout de tâches à la configuration, compléments, listes de tâches état rapide et ajout de liens à la tâche aide  
 Vous pouvez ajouter des tâches à le **le programme d’installation**, **compléments**, **état rapide** listes et des liens vers des tâches le **aide** tâches en définissant les tâches et les liens à l’aide de XML, éventuellement la création du fichier de ressources incorporé et l’installation du fichier sur le serveur. Si le fichier XML est installé sur le serveur sans fichier de ressources, il doit être nommé OEMHomePageContent.home. Si un assembly est utilisé pour installer un fichier XML et un fichier de ressources, il doit être nommé OEMHomePageContent.dll et il doit être signé avec Authenticode.  
  
### <a name="define-the-tasks-and-links"></a>Définir les tâches et les liens  
 Vous pouvez utiliser un éditeur de texte tel que le bloc-notes pour créer le fichier .home, ou si vous créez également un fichier de ressources incorporé, vous pouvez utiliser Visual Studio2010 ou version ultérieure pour définir les fichiers. La procédure suivante montre comment utiliser Visual Studio2010 ou version ultérieure pour créer les fichiers.  
  
##### <a name="to-define-the-tasks-and-links"></a>Pour définir les tâches et les liens  
  
1.  Ouvrez Visual Studio2010 ou version ultérieure en tant qu’administrateur en cliquant sur le programme dans le menu Démarrer et en sélectionnant **exécuter en tant qu’administrateur**.  
  
2.  Cliquez sur **fichier**, cliquez sur **New**, puis cliquez sur **projet**.  
  
3.  Dans le **modèles** volet, cliquez sur **bibliothèque de classes**, type **OEMHomePageContent** dans les **nom** zone, puis cliquez sur **OK**.  
  
4.  Supprimez le fichier Class1.cs.  
  
5.  Cliquez sur le nouveau projet, cliquez sur **ajouter**, puis cliquez sur **nouvel élément**.  
  
6.  Dans le **modèles** volet, cliquez sur **fichier XML**, type **OEMHomePageContent.home** dans les **nom** zone, puis cliquez sur **ajouter**.  
  
    > [!NOTE]
    >  Si le fichier XML est installé sans fichier de ressources, il doit être nommé OEMHomePageContent.home. Si elle est incluse dans un assembly, il peut recevoir n’importe quel nom dans la mesure où il a une extension .home.  
  
7.  Ajoutez le code XML suivant dans le fichier OEMHomePageContent.home:  
  
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
  
     Où:  
  
    |Attribut|Description|  
    |---------------|-----------------|  
    |Nom (tâche)|Le nom affiché pour la tâche dans la liste. Si vous créez un fichier de ressources incorporé, la valeur de cet attribut est la ressource de chaîne.|  
    |Description (tâche)|La description de la tâche. Si vous créez un fichier de ressources incorporé, la valeur de cet attribut est la ressource de chaîne.|  
    |ID (tâche)|L’identificateur de la tâche. Cet identificateur doit être un GUID. Vous créez un nouveau GUID pour une **exe** tâche, mais pour une **global** tâche, vous utilisez le GUID que vous avez créé lors de la définition de la tâche pour le volet des tâches du sous-onglet. Pour plus d’informations sur la création d’un GUID, voir [créer un Guid (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098).|  
    |image|Ce champ sera ignoré.|  
    |Nom (Action)|Affiche le nom de la tâche.|  
    |Type (Action)|Décrit le type de tâche. La tâche peut être l’une des opérations suivantes:- **global** tâche, **exe**, ou une tâche d’url. Un **global** tâche est la même tâche globale que vous avez créé lors de la définition des tâches pour le volet des tâches du sous-onglet. Pour plus d’informations sur la création d’une tâche globale qui peut être utilisée dans les deux volet Tâches du sous-onglet et les listes tâches de mise en route ou tâches courantes de la page d’accueil, voir œCreating les classes de prise en charge? dans Comment faire: créer un sous-onglet? de la [WindowsServerSolutionsSDK](https://go.microsoft.com/fwlink/?LinkID=248648). Un **exe** tâche peut être utilisée pour exécuter des applications à partir de tâches de mise en route ou répertorie les tâches courantes.|  
    |exelocation|Le chemin d’accès à l’application qui est associée à la tâche. Cet attribut est uniquement utilisé pour **exe** tâches.|  
    |replaceid|L’identificateur de la tâche qui est remplacée par cette tâche.|  
    |assembly|L’AssemblyName de l’assembly qui fournit la classe pour implémenter une demande d’état rapide. L’assembly doit se trouver dans le programme files\ windows server\bin\\.|  
    |classe|Le nom de la classe implémente la demande d’état rapide. La classe doit implémenter **ITaskStatusQuery** interface.|  
    |Titre (lien)|Le texte qui est affiché pour le lien. Si vous créez un fichier de ressources incorporé, la valeur de cet attribut est la ressource de chaîne.|  
    |Description (lien)|La description de la destination du lien. Si vous créez un fichier de ressources incorporé, la valeur de cet attribut est la ressource de chaîne.|  
    |ShellExecPath|Le chemin d’accès à l’application ou l’URL.<br /><br /> **Remarque:** variables d’environnement sont pris en charge dans l’attribut ShellExecPath.|  
  
     L’exemple de code suivant montre comment définir un lien vers une application:  
  
    ```  
    <Links>  
       <Link Title="Calc" Description="Launches Calc" ShellExecPath="%windir%\system32\calc.exe" />  
    </Links>  
    ```  
  
     L’exemple de code suivant montre comment définir un lien vers une page Web:  
  
    ```  
    <Links>  
       <Link Title="Browser" Description="Open browser" ShellExecPath="http://www.adventureworks.com/" />  
    </Links>  
    ```  
  
8.  Modifier les valeurs d’attribut pour représenter votre tâche ou un lien.  
  
9. Dans **Explorateur de solutions**, avec le bouton droit **OEMHomePageContent.home**, puis cliquez sur **propriétés**.  Dans le **propriétés** volet, sous **Action de génération**, sélectionnez **ressource incorporée**.  
  
10. Enregistrez le fichier OEMHomePageContent.home.  
  
 Pour savoir comment mettre en œuvre une requête d’état rapide, consultez les documents et exemples fournis dans le [SDK WindowsServerSolutions](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
#### <a name="change-the-status-of-a-setupadd-ins-task"></a>Modifier le statut d’une tâche d’installation des compléments  
 Les tâches qui sont répertoriés dans le programme d’installation et compléments peuvent être basculées entre les États de fin (configuré pour Add-ins) et non complété (pas configuré pour Add-ins).  
  
 Lorsque vous définissez l’application qui est associée à votre nouvelle tâche, vous pouvez utiliser la méthode SetTaskStatus de l’espace de noms Microsoft.WindowsServerSolutions.Administration.ObjectModel.TaskStatusHelper (fourni, mais non documenté dans le SDK WindowsServerSolutions) pour modifier l’état de la tâche. Par exemple, vous pouvez modifier la coche du gris au vert en appelant la méthode SetTaskStatus avec la valeur d’énumération TaskStatus.Complete (SetTaskStatus (id, TaskStatus.Complete), où **id** est l’identificateur de la tâche). Les valeurs d’énumération qui peuvent être utilisées sont TaskStatus.Complete, TaskStatus.Incomplete ou TaskStatus.Hidden.  
  
##### <a name="replace-tasks"></a>Remplacer des tâches  
 Vous pouvez remplacer les tâches qui sont prédéfinies dans les tâches de mise en route ou les listes tâches courantes en ajoutant le GUID de la tâche à l’attribut replaceid de la définition de tâche. Le tableau suivant répertorie les tâches et les identificateurs correspondants qui peuvent être remplacés dans le tableau de bord:  
  
|Nom de la tâche|Identificateur|  
|---------------|----------------|  
|Obtenir les mises à jour d’autres produits Microsoft|8412D35A-13EE-4112-AE0B-F7DBC83EA83D|  
|Configurer la sauvegarde du serveur|F68B3F3F-19DE-499D-9ACB-4BB41B8FF420|  
|Configurer l’accès en tout lieu|8991302D-676A-4A7C-B244-D1E08AE0EFEA|  
|Programme d’installation de notification d’alerte par courrier électronique|DE6F2B36-F19C-4FAF-998B-9772300E3530|  
|Ajouter un compte d’utilisateur|6D5B5D5F-2EC7-4B1F-9580-4DB084B278B1|  
|Ajouter des dossiers serveur|03F1F438-D94E-439B-A9F7-0C817C37D625|  
|Accès en tout lieu - état rapide|6093B462-1F04-4212-8804-9BC823070FAD|  
|Sauvegarde du serveur - état rapide|156947D8-21DC-45FE-A9A8-2F80AE304191|  
|Dossiers du serveur - état rapide|C657E8AB-AC1F-4AA1-8E80-5AF6BB27C314|  
|Comptes d’utilisateur actifs - état rapide|68BCB125-CE8F-467F-B65B-56AD45A614B5|  
|Par courrier électronique de notification d’alerte - état rapide|75AB06E7-A679-4D62-A5EC-65362FE4F9DB|  
|Ordinateurs - état rapide|7966A974-D52D-4F5D-B37F-05C1B73CEEF3|  
  
##### <a name="optional-create-the-resource-file"></a>(Facultatif) Créer le fichier de ressources  
 Si vous souhaitez localiser le texte dans les tâches que vous ajoutez à des tâches de mise en route, les tâches courantes et les liens de la Communauté, vous devez créer un assembly qui contient le fichier .home. home.resx fichier qui définit les chaînes de texte.  
  
###### <a name="to-create-the-resource-file"></a>Pour créer le fichier de ressources  
  
1.  Cliquez sur le projet que vous avez créé pour vos tâches, cliquez sur **ajouter**, puis cliquez sur **nouvel élément**.  
  
2.  Dans le **modèles** volet, cliquez sur **fichier de ressources**, type **OEMHomePageContent.home.resx** dans les **nom** zone, puis cliquez sur **ajouter**.  
  
    > [!NOTE]
    >  Le fichier de ressources peut figurer n’importe quel nom dans la mesure où il a. home.resx extension.  
  
3.  Pour chaque tâche ou le lien que vous ajoutez, vous devez ajouter les chaînes et les valeurs dans le fichier OEMHomePageContent.home.resx qui correspondent aux éléments tâche et lien définis dans le fichier OEMHomePageContent.home. L’exemple de code suivant montre un exemple de fichier Tasks.xml structuré pour le fichier de ressources:  
  
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
    >  Les identificateurs des attributs qui sont utilisés pour la localisation ne peut pas contenir d’espaces.  
  
4.  Ajoutez les noms de ressources MyTask, MyTaskDescription, MyActionName et IconForAction au fichier .resx avec les valeurs appropriées.  
  
5.  Enregistrez le fichier OEMHomePageContent.home.resx, puis générez la solution.  
  
#####  <a name="BKMK_SignAssembly"></a>Signer l’assembly avec une signature Authenticode  
 Vous devez signer l’assembly pour pouvoir être utilisé dans le système d’exploitation. Pour plus d’informations sur la signature de l’assembly, voir [signature et vérification du Code avec Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
##### <a name="install-the-task-files"></a>Installer les fichiers de tâches  
 Après avoir créé le .home. home.resx, fichiers, vous devez les installer sur le serveur.  
  
###### <a name="to-install-the-task-files"></a>Pour installer les fichiers de tâches  
  
1.  Assurez-vous que votre solution se génère sans erreur.  
  
2.  Si vous n’avez pas créé un fichier de ressources incorporé, copiez le fichier OEMHomePageContent.home **%ProgramFiles%\Windows Server\Bin\Addins\Home** sur le serveur. Si vous avez créé un fichier de ressources incorporé, copiez le fichier OEMHomePageContent.dll **%ProgramFiles%\Windows Server\Bin\Addins\Home** sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)