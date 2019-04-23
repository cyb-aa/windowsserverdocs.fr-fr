Recherchez vos certificats de guardian SGH. Vous devez un certificat de signature et un seul certificat de chiffrement pour initialiser le cluster SGH.
Pour fournir des certificats à SGH, le plus simple consiste à créer un fichier PFX protégé par mot de passe pour chaque certificat qui contient les clés publiques et privées. Si vous utilisez des clés HSM ou autres certificats non exportables, assurez-vous que le certificat est installé dans le magasin de certificats de l’ordinateur local avant de continuer.
Pour plus d’informations sur les certificats à utiliser, consultez [obtenir des certificats pour SGH](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-obtain-certs).

<!-- Appears in guarded-fabric-initialize-hgs-ad-mode-default.md and guarded-fabric-initialize-hgs-tpm-mode-default.md
-->