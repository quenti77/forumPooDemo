# Chapter 1: Init project

Création du fichier `.gitignore` pour ne pas versionner les fichiers inutiles.

On a le dossier `.idea` de l'IDE Jetbrains dedans, car c'est qui sera utilisé de mon côté.
Nous avons aussi les fichiers `.env` et `.env.*` qui contiennent les variables d'environnement.

Ensuite, on lance la commande `composer init` pour initialiser le projet avec composer.
Dans le fichier `composer.json`, on ajoute les lignes suivantes :

```json
{
  "autoload": {
    "psr-4": {
      "App\\": "app/",
      "Forum\\": "src/",
      "Tests\\": "tests/"
    }
  }
}
```

Cela permet de dire à composer que les namespaces `Forum` et `Tests`
sont dans les dossiers `src` et `tests` respectivement.

Il faut maintenant créer les premiers dossiers du projet :

- `app/`: Contient les fichiers de l'application
- `config/`: Contient les fichiers de configuration du projet
- `public/`: Contient les fichiers accessibles publiquement (index.php, assets, etc.)
- `src/`: Contient le code source de notre framework maison
- `tests/`: Contient les tests du projet

Une fois les modifications effectuées, on lance la commande `composer dump-autoload` pour que composer
mette à jour son autoloader.