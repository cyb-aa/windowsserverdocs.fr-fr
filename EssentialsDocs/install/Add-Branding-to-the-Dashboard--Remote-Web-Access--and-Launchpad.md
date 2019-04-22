---
title: Ajout d’un logo au Tableau de bord, au site Web Accès à distance et à la zone de lancement
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 04/10/2014
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 166262f8-b2a5-4b1c-a4a7-a141e1c54f10
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 02088b169e44cdcf87385425e1949232ffa408a6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823500"
---
# <a name="add-branding-to-the-dashboard-remote-web-access-and-launchpad"></a>Ajout d’un logo au Tableau de bord, au site Web Accès à distance et à la zone de lancement

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_Branding"></a> Ajout d’un logo au tableau de bord, accès Web à distance et Launchpad  
 Vous pouvez inclure de nombreux éléments de personnalisation propres à votre marque en ajoutant des entrées dans le Registre. Toutes les entrées de marques commerciales dans le Registre du système d'exploitation sont localisée sous HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OEM.  
  
 Les éléments de personnalisation doivent respecter les spécifications suivantes :  
  
-   Le logo Windows Server Essentials doit avoir une largeur minimale de **170 pixels**, conserver une proportion correcte, avec **96 PPP**.  
  
#### <a name="to-add-branding-by-changing-the-registry"></a>Pour ajouter un élément de personnalisation en modifiant le Registre  
  
1.  Sur le serveur, déplacez votre souris sur le coin supérieur droit de l’écran, puis cliquez sur **Rechercher**.  
  
2.  Dans la zone de recherche, tapez **regedit**, puis cliquez sur l’application **Regedit** .  
  
3.  Dans le volet de navigation, développez successivement les entrées **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft** et **Windows Server**. Si la clé **OEM** n’existe pas, procédez de la façon suivante pour la créer :  
  
    1.  Faites un clic droit sur **Windows Server**, cliquez sur **Nouveau**, puis cliquez sur **Clé**.  
  
    2.  Tapez **OEM** comme nom de la clé.  
  
4.  (Facultatif) Si vous définissez une nouvelle entrée pour un logo, vous pouvez créer plusieurs clés afin de distinguer les versions du logo prévues pour chaque langue. Si vous disposez, par exemple, d'une version anglaise et française du logo, il suffit de créer une clé en-us et une clé fr-fr. Comme tous les fichiers des logos sont stockés dans le même dossier, les différentes instances du fichier d'image spécifiques à chaque langue doivent porter un nom unique. Dans notre exemple, il conviendrait de créer un fichier appelé DashboardLogo_en.png et un fichier appelé DashboardLogo_fr.png.  
  
5.  Cliquez avec le bouton droit sur **OEM** ou sur la clé de langue appropriée, cliquez sur **Nouveau**, puis sur **Valeur de chaîne**.  
  

6.  Tapez le nom de la chaîne et appuyez sur ENTRÉE. Référez-vous au tableau [Chaînes et valeurs de Registre](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings) pour connaître les noms de chaînes et les valeurs appropriées.  

6.  Tapez le nom de la chaîne et appuyez sur ENTRÉE. Référez-vous au tableau [Chaînes et valeurs de Registre](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings) pour connaître les noms de chaînes et les valeurs appropriées.  

  
7.  Cliquez avec le bouton droit sur la nouvelle chaîne, puis cliquez sur **Modifier**.  
  
8.  Entrez la valeur du tableau associée au nom de la chaîne, puis cliquez sur **OK**.  
  
9. Si vous définissez une entrée pour l'image d'un logo ou des liens ajoutés, copiez le fichier dans %programFiles%\Windows Server\Bin\OEM. Si le répertoire OEM n'existe pas, créez-le.  
  
10. Si des modifications ont un impact sur l’accès web à distance, le client qui prendra possession du serveur sera tenu d’activer ce service sur le serveur. Demandez au client d’appliquer ces instructions :  
  
    1.  Dans le Tableau de bord, cliquez sur **Paramètres**, puis cliquez sur l'onglet **Accès en tout lieu**.  
  
    2.  Si l'Accès en tout lieu est activé, cliquez sur **Configurer**, puis désactivez la case à cocher Accès en tout lieu à la page **Choisir les fonctions d'Accès en tout lieu à activer** de l'Assistant Configurer l'Accès en tout lieu.  
  
    3.  Cliquez sur **configurer**.  
  
###  <a name="BKMK_RegStrings"></a> Le tableau suivant répertorie l’emplacement où les modifications apportées au Registre affectent la marque, le nom de chaîne et la valeur de données.  
  
### <a name="registry-strings-and-values"></a>Chaînes et valeurs de Registre  
  
|Emplacement de l'élément de personnalisation|Description|Nom de la chaîne|Valeur|  
|--------------------------|-----------------|-----------------|----------------|  
|Logo Tableau de bord|Ajoute l’image du logo au tableau de bord. Choisissez un logo au format .png et de 350 pixels de large par 38 pixels de haut maximum.<br /><br /> **Important :** Pour donner une touche personnelle au Tableau de bord en y associant votre logo, vous devez modifier la mosaïque de la conception graphique fournie sur le DVD du Kit de préinstallation OEM (OPK) et ajouter le logo de votre société à l'image en respectant les conditions prévues pour les espaces blancs. Pour plus d'informations, consultez la mosaïque proposée en guise d'exemple.|DashboardLogo|Nom du fichier image contenant le logo|  
|DashboardClientLogo|Ajoute l'image du logo à l'écran de connexion du client du tableau de bord.|DashboardClientLogo|Nom du fichier image contenant le logo|  
|Image d’arrière-plan du site Web|Changez l’image affichée en arrière-plan sur la page de connexion au site Accès web à distance. Les résolutions standard sont représentées de la façon suivante :<br /><br /> -résolution de pixels 768 1024 x image recouvre la page d’ouverture de session<br /><br /> -800 x 600 pixels sera centré dans la page et apparaissent avec une bordure noire<br /><br /> -résolution de pixels 720 1280 x sera centré et les pixels qui dépassent 1024 x 768 apparaîtra pas|LogonBackground|Nom du fichier image contenant l’arrière-plan|  
|Titre du site Web|Remplace le titre du site accès Web à distance de Windows Server Essentials à un titre que vous choisissez.|WebsiteName|Nouveau titre du site Accès web à distance|  
|Logo du site Web|Changez le logo par défaut sur le site Accès web à distance. La taille attendue du logo est de 32 pixels par 32 pixels. Si les dimensions de votre logo sont inférieures ou supérieures, la taille du logo est agrandie ou réduite en conséquence.|WebsiteLogo|Nom du fichier image contenant le logo|  
|Logo du site Web ajouté|Votre logo de partenaire figure juste en dessous du logo Microsoft affiché sur le site Accès web à distance. La taille attendue du logo est de 200 pixels (hauteur) par 50 pixels (largeur). Si votre logo est plus grand, il sera automatiquement réduit en conséquence afin de conserver les proportions d’origine. Si votre logo est plus petit, il sera centré dans la zone correspondant à 200 x 50 pixels, mais ni la taille, ni les proportions ne seront modifiées.|OEMLogo|Nom du fichier image contenant le logo|  

| Liens sur la page d’accueil du site Web et de la page d’ouverture de session | Ajouter des liens à la page de connexion et de la page d’accueil du site accès Web à distance. Le fichier .xml contenant les informations propres aux liens doit se trouver dans %programFiles%\Windows Server\Bin\OEM. L'exemple suivant illustre le format du fichier .xml :<br /><br /> < OemLinks\><br /> <LogonLinks\><br /> < nom de lien\=LogonLinkName ><br /> <Text\>LogonLinkDescription</Text\><br /> < Url\>LogonLinkURL < / Url\><br /> <Icon\>LinkIcon</Icon\><br /> < / link\><br /> </LogonLinks\><br /> < HomepageLinks\><br /> < nom de lien\=HomepageLinkName ><br /> <Text\>HomepageLinkDescription</Text\><br /> < Url\>HomepageLinkURL < / Url\><br /> < / link\><br /> < / HomepageLinks\><br /> < / OemLinks\>| LinksXML | Reportez-vous à la [éléments LinksXML](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links) table pour obtenir la liste des éléments et des descriptions. |  

| Liens sur la page d’accueil du site Web et de la page d’ouverture de session | Ajouter des liens à la page de connexion et de la page d’accueil du site accès Web à distance. Le fichier .xml contenant les informations propres aux liens doit se trouver dans %programFiles%\Windows Server\Bin\OEM. L'exemple suivant illustre le format du fichier .xml :<br /><br /> < OemLinks\><br /> <LogonLinks\><br /> < nom de lien\=LogonLinkName ><br /> <Text\>LogonLinkDescription</Text\><br /> < Url\>LogonLinkURL < / Url\><br /> <Icon\>LinkIcon</Icon\><br /> < / link\><br /> </LogonLinks\><br /> < HomepageLinks\><br /> < nom de lien\=HomepageLinkName ><br /> <Text\>HomepageLinkDescription</Text\><br /> < Url\>HomepageLinkURL < / Url\><br /> < / link\><br /> < / HomepageLinks\><br /> < / OemLinks\>| LinksXML | Reportez-vous à la [éléments LinksXML](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links) table pour obtenir la liste des éléments et des descriptions. |  

| Logo du Launchpad | Ajoute l’image du logo au Launchpad. Le logo de Launchpad doit être au format .png et de doit pas dépasser 64 pixels. | LaunchpadLogo | Nom du fichier d’image de logo |  
  
###  <a name="BKMK_Links"></a> Le tableau suivant répertorie et décrit les éléments de nom de chaîne LinksXML.  
  
### <a name="linksxml-elements"></a>Éléments LinksXML  
  
|Élément LinksXML|Description|  
|----------------------|-----------------|  
|**LogonLinks**|  
|LogonLinkName|Nom du lien d'ouverture de session.|  
|LogonLinkDescription|Texte affiché sous forme de lien permettant d'accéder à la page de connexion.|  
|LogonLinkURL|Adresse URL permettant de résoudre le lien d'ouverture de session.|  
|LinkIcon|Nom du fichier d'icône utilisé pour le lien d'ouverture de session. Ce fichier doit figurer dans le même dossier que le fichier .xml. L'image utilisée comme icône du lien doit mesurer 16 x 16 pixels et être au format .png. Si vous omettez de spécifier l'élément LinkIcon, l'image représentant le lien d'ouverture de session par défaut est utilisée.|  
|**HomepageLinks**|  
|HomepageLinkName|Nom du lien de la page d'accueil.|  
|HomepageLinkDescription|Texte affiché sous forme de lien permettant d’accéder à la page d’accueil.|  
|HomepageLinkURL|Adresse URL permettant de résoudre le lien de la page d’accueil.|  
|HomepageLinkIcon|Nom du fichier d'icône utilisé pour le lien de la page d'accueil. Ce fichier doit figurer dans le même dossier que le fichier .xml. L'image utilisée comme icône du lien doit mesurer 16 x 16 pixels et être au format .png. Si vous omettez de spécifier l'élément HomepageLinkIcon, l'image représentant le lien de la page d'accueil par défaut est utilisée.|  
  
## <a name="see-also"></a>Voir aussi  

 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)

 [Création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](../install/Testing-the-Customer-Experience.md)

