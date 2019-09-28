---
title: Ajouter les balises de métadonnées nécessaires à votre article Windows Server
description: Liste des informations que vous devez ajouter en tant que balises de métadonnées au sommet de vos articles relatifs à Windows Server. Les balises requises sont sujettes à modification, en fonction de vos besoins en matière de création de rapports et d’équipe.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 9bedc7ec13aedab9cfaa655f537d5e431d20cccb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363959"
---
# <a name="add-the-required-metadata-tags-to-your-windows-server-related-article"></a>Ajouter les balises de métadonnées nécessaires à votre article Windows Server

En haut de chaque article, il existe des métadonnées spécifiques qui doivent être incluses pour le suivi et le SEO. Les balises requises sont sujettes à modification, selon les besoins de création de rapports. Toutefois, vous devez être averti si vous devez ajouter/supprimer des champs.

Elle doit ressembler à ce qui suit, y compris les trois traits d’Union (---) en haut et en bas :

```markdown

---
title: The title of the article should go here. This is used in SEO and search results.

description: A description for the article should go here. This is used in search results, to provide users with information about whether the article has the information they're looking for.

ms.prod: Use this specific text, windows-server-threshold

ms.reviewer: The Microsoft alias for the primary PM for the feature/functionality

author: Your GitHub alias

ms.author: Your Microsoft alias

manager: Your manager's Microsoft alias

ms.topic: Type of article, including article, landing-page, get-started-article, or reference

ms.date: Date of change (MM/DD/YYYY)

---

```

## <a name="example"></a>Exemple

```markdown

---
title: What is Windows Admin Center?
description: Learn about the Windows Admin Center, a locally-deployed, browser-based management tool set that lets you manage your Windows Servers with no Azure or cloud dependency.
ms.prod: windows-server
ms.reviewer: alainch
author: danielle-github
ms.author: danielle
manager: alainch
ms.topic: article
ms.date: 07/06/2019
---

```