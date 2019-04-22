Si vous n’avez pas fourni un fichier PFX pour le chiffrement ou la signature des certificats sur le premier serveur SGH, seule la clé publique sera répliquée vers ce serveur.
Vous devez installer la clé privée en important un fichier PFX contenant la clé privée dans le magasin de certificats local ou, dans le cas de clés HSM, configuration du fournisseur de stockage de clé et l’associer à vos certificats par le fabricant de votre HSM obtenir des instructions.

<!-- Appears in guarded-fabric-initialize-hgs-ad-mode-default.md and guarded-fabric-initialize-hgs-tpm-mode-default.md
-->