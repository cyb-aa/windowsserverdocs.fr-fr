# <a name="metadata-and-version-identifiers"></a>Identificateurs de version et de métadonnées

Voici ce que vous devez savoir sur les marques commerciales, le contrôle de version et les métadonnées pour les articles de référentiel de windowsserverdocs-pr. Les auteurs de l’article sont chargés de s’assurer que leurs articles satisfont à ces normes en matière de.

## <a name="trademarks"></a>Marques commerciales
N’utilisez pas les symboles de marque après les références de produits mentionnées dans les articles dans la bibliothèque technique. Ils ne sont pas nécessaires car technet.microsoft.com, docs.microsoft.com et autres canaux de publication de Microsoft officielle comprennent un [marque](https://www.microsoft.com/trademarks) lien de pied de page à la liste des marques déposées de Microsoft. Ce lien répond à l’exigence juridique. Pour plus d’informations, consultez les conseils de [What IT](https://microsoft.sharepoint.com/sites/LCAWeb/Home/Copyrights-Trademarks-and-Patents/Trademarks/Trademark-List-and-Usage), sous « sites Web » et le Guide de Style d’écriture Microsoft [droits d’auteur et marques](https://worldready.cloudapp.net/Styleguide/Read?id=2700&topicid=26696) page, sous « Pages Web sur Microsoft.com » 

## <a name="versioning"></a>Contrôle de version
Plusieurs types de contrôle de version s’appliquent pour les articles de ce référentiel : 

-  Le contrôle de version qui indique la version du produit auquel s’applique un article est effectuée de deux façons :
    - Manuellement sur une seule ligne dans l’article, il est visible sous forme de texte sur un article publié.
    - Par les métadonnées, décrit ci-dessous.
-  Le contrôle de version contenu source est gérée via l’historique de Github sur le fichier. 

## <a name="metadata-structure-and-format"></a>Format et la structure de métadonnées

- Placez les métadonnées en haut du fichier, au-dessus du titre H1.
- Séparez le bloc du reste du contenu du fichier à l’aide de seulement trois des tirets dans les première et dernière lignes du bloc. Ne placez pas de tout autre texte sur ces lignes.
- Placez chaque paire nom/valeur de métadonnées sur une ligne distincte.
- Utilisez un des modèles de syntaxe suivantes, selon que les métadonnées requiert des valeurs prédéfinies ou accepte des valeurs personnalisées. 

        ---
        name1: predefined-value
        name2: "my custom value"
        ---

## <a name="metadata-names-and-values"></a>Valeurs et noms de métadonnées

Certaines métadonnées sont obligatoire pour tous les fichiers publiés en tant qu’articles dans la bibliothèque technique TechNet, tandis que d’autres métadonnées sont recommandée mais pas obligatoire. Dans certains cas, seules certaines valeurs sont autorisées pour un élément de métadonnées spécifique. 

|Nom|Value|
|---|---|
|title|Valeur personnalisée qui correspond au titre H1. Détermine ce qui s’affiche dans l’onglet du navigateur.|
|description|Affiche dans les résultats de la recherche et affecte des moteurs de recherche. Inclure les mots clés appropriés en conservant longueur inférieure à 160 caractères.|
|Auteur|Alias de Github de l’auteur principal de l’article|
|ms.author|Alias MSFT d’auteur ou du contenu développeur l’article responsable de la zone de technologie traitée dans l’article.|
|ms.date|Date de dernière mise à jour de contenu. Ne mettez à jour pour les modifications apportées aux métadonnées uniquement. Utilisez le format mm/jj/aaaa.|
|ms.prod|Identifie la version de Windows Server pour création de rapports, BI selon prédéfini [valeur](https://microsoft.sharepoint.com/teams/STBCSI/Insights/_layouts/15/WopiFrame.aspx?sourcedoc=%7b7A321BF1-0611-4184-84DA-A0E964C435FA%7d&file=WEDCS_MasterList_CSIValues.xlsx&action=default&IsList=1&ListId=%7b46B17C8A-CD7E-47ED-A1B6-F2B654B55E2B%7d&ListItemId=969).|
|ms.technology|Identifie le domaine technologique pour création de rapports, BI selon prédéfini [valeur](https://microsoft.sharepoint.com/teams/STBCSI/Insights/_layouts/15/WopiFrame.aspx?sourcedoc=%7b7A321BF1-0611-4184-84DA-A0E964C435FA%7d&file=WEDCS_MasterList_CSIValues.xlsx&action=default&IsList=1&ListId=%7b46B17C8A-CD7E-47ED-A1B6-F2B654B55E2B%7d&ListItemId=969)|
|ms.asset|GUID qui identifie l’article sur tous les paramètres régionaux pour la création de rapports BI. Articles migré à partir de la création antérieures systèmes déjà le faire. Pour les articles créés dans Github, utilisez un outil tel que [ https://guidgenerator.com/ ](https://guidgenerator.com/).| 

## <a name="troubleshooting"></a>Résolution des problèmes

Les métadonnées qui s’affiche en haut d’un article publié se produit lorsque le fichier source comporte des erreurs de mise en forme. Certaines erreurs courantes sont :

- Manquant dans les triples des traits d’union aux première et dernière lignes de bloc, ou un nombre incorrect de traits d’union.
- Métadonnées ne respectent pas la syntaxe requise : \<nom\>:\<unique espace\>\<valeur >

Vérifiez votre fichier pour celles-ci et d’autres erreurs évidentes, envoyez une demande de tirage pour publier le fichier mis à jour. Si vous êtes bloqué, l’alias de relecteurs de demande de tirage de messagerie : wssc-pra@microsoft.com

## <a name="see-also"></a>Voir aussi
Les métadonnées utilisées dans ce dépôt sont basée sur les métadonnées utilisées dans les Services de contenu & International \(CSI\). Plus d’informations, y compris les métadonnées facultatives, est disponible à l’adresse [ http://aka.ms/skyeye/meta ](http://aka.ms/skyeye/meta).
Pour les ressources de business intelligence, consultez l’équipe CSI BI [wiki](https://microsoft.sharepoint.com/teams/STBCSI/Insights/Selfserve%20BI%20wiki/Self-serve%20BI%20wiki.aspx).