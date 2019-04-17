---
title: "Ajouter un onglet aux paramètres"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aac6b7f3-9020-46c3-a83f-b81542300385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9eaa1aa5a9c5e8d4c2e36f2000e0adecc83245d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-tab-to-settings"></a>Ajouter un onglet aux paramètres

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Vous pouvez ajouter un onglet aux paramètres du tableau de bord en créant et en installant un assembly de code qui est utilisé par le Gestionnaire de paramètres dans le système d’exploitation.  
  
## <a name="add-a-tab-to-settings"></a>Ajouter un onglet aux paramètres  
 Vous ajoutez un onglet aux paramètres en effectuant les tâches suivantes:  
  
-   [Ajout d’une implémentation de l’interface ISettingsData à l’assembly](Add-a-Tab-to-Settings.md#BKMK_ISettingsData).  
  
-   [Signer l’assembly avec une signature Authenticode](Add-a-Tab-to-Settings.md#BKMK_SignAssembly).  
  
-   [Installer l’assembly sur l’ordinateur de référence](Add-a-Tab-to-Settings.md#BKMK_InstallAssembly).  
  
###  <a name="BKMK_ISettingsData"></a>Ajout d’une implémentation de l’interface ISettingsData à l’assembly  
 L’interface ISettingsData est inclus dans l’espace de noms Microsoft.WindowsServerSolutions.Settings de l’assembly AdminCommon.dll qui se trouve dans \Program Files\Windows Server\Bin.  
  
##### <a name="to-add-the-isettingsdata-code-to-the-assembly"></a>Pour ajouter le code ISettingsData à l’assembly  
  
1.  Ouvrez Visual Studio2010 en tant qu’administrateur en cliquant sur le programme dans le **Démarrer** menu et en sélectionnant **exécuter en tant qu’administrateur**.  
  
2.  Cliquez sur **fichier**, cliquez sur **New**, puis cliquez sur **projet**.  
  
3.  Dans le **nouveau projet** boîte de dialogue, cliquez sur **Visual c#**, cliquez sur **bibliothèque de classes**, entrez **DashboardSettingsPage** pour le nom de la solution, puis cliquez sur **OK**.  
  
    > [!IMPORTANT]
    >  L’assembly est installé sur le serveur doit être nommé DashboardSettingsPage.dll et copiez la dll dans %ProgramFiles%\Windows Server\Bin\OEM.  
  
4.  Créez le contrôle que vous souhaitez utiliser dans l’onglet. Dans cet exemple, le contrôle de paramètres s’appelle MySettingsControl.  
  
5.  Renommez le fichier Class1.cs. Par exemple, MySettingTab.cs.  
  
6.  Ajoutez une référence au fichier AdminCommon.dll.  
  
7.  Ajoutez le code suivant à l’aide de la déclaration de:  
  
    ```c#  
    using Microsoft.WindowsServerSolutions.Settings;  
    ```  
  
8.  Modifier l’espace de noms et l’en-tête de classe pour faire correspondre l’exemple suivant:  
  
    ```  
  
    namespace DashboardSettingsPage  
    {  
        public class MySettingTab : ISettingsData  
        {  
        }  
    }  
  
    ```  
  
9. Créez une instance du contrôle que vous avez créé pour l’onglet. Par exemple:  
  
    ```c#  
    private MySettingsControl tab;  
    ```  
  
10. Ajoutez le constructeur de la classe. L’exemple de code suivant illustre le constructeur:  
  
    ```  
  
    public MySettingTab()  
    {  
       tab = new MySettingsControl();  
    }  
    ```  
  
11. Ajoutez la méthode de validation, qui envoie les modifications du paramètre. L’exemple de code suivant illustre la méthode de validation:  
  
    ```  
  
    void ISettingsData.Commit(bool dismissed)  
    {  
       // Implement the code that is required to submit your setting changes  
    }  
    ```  
  
12. Ajoutez la méthode TabControl servant à identifie le contrôle de l’onglet. L’exemple de code suivant montre la méthode TabControl:  
  
    ```  
  
    System.Windows.Forms.Control ISettingsData.TabControl  
    {  
       get { return tab; }  
    }  
    ```  
  
13. Ajoutez la méthode TabId, qui fournit un identificateur unique pour l’onglet. L’exemple de code suivant montre la méthode TabId:  
  
    ```  
  
    private Guid id = Guid.NewGuid();  
  
    Guid ISettingsData.TabId  
    {  
       get { return id; }  
    }  
    ```  
  
14. Ajoutez la méthode TabOrder, qui retourne l’ordre de l’onglet. L’exemple de code suivant montre la méthode TabOrder:  
  
    ```  
  
    int ISettingsData.TabOrder  
    {  
       get { return 0; }  
    }  
    ```  
  
    > [!NOTE]
    >  L’ordre de tabulation est défini à l’aide de chiffres commençant par 0. Les onglets de paramètres intégrés Microsoft sont affichés en premier, et puis vos onglets sont affichées en fonction de l’ordre de tabulation que vous définissez. Par exemple, si vous disposez de trois onglets de paramètres, vous devez spécifier l’ordre de tabulation en tant que 0, 1 et 2 en fonction de l’ordre que vous souhaitez que les onglets à afficher.  
  
15. Ajoutez la méthode TabTitle qui fournit le titre de l’onglet. L’exemple de code suivant illustre la méthode TabTitle:  
  
    ```  
  
    string ISettingsData.TabTitle  
    {  
      get { return "My Settings Tab"; }  
    }  
    ```  
  
    > [!NOTE]
    >  Le texte du titre peut également provenir d’un fichier de ressources en fonction des besoins de localisation.  
  
16. Enregistrez et générez la solution.  
  
###  <a name="BKMK_SignAssembly"></a>Signer l’assembly avec une signature Authenticode  
 Vous devez signer l’assembly pour pouvoir être utilisé dans le système d’exploitation. Pour plus d’informations sur la signature de l’assembly, voir [signature et vérification du Code avec Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
###  <a name="BKMK_InstallAssembly"></a>Installer l’assembly sur l’ordinateur de référence  
 Après avoir généré la solution, placez une copie du fichier DashboardSettingsPage.dll dans le dossier suivant sur l’ordinateur de référence:  
  
 **%ProgramFiles%\Windows Server\Bin\OEM**  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)