# Chapter 1: Init project

## Structure and first version of composer.json

Création du fichier `.gitignore` pour ne pas versionner les fichiers inutiles.

On a le dossier `.idea` de l'IDE Jetbrains dedans, car c'est qui sera utilisé de mon côté.
Nous avons aussi les fichiers `.env` et `.env.*` qui contiennent les variables d'environnement.

Ensuite, on lance la commande `composer init` pour initialiser le projet avec composer.
Dans le fichier `composer.json`, on ajoute les lignes suivantes :

```json
{
  "require": {
    "php": "^8.2"
  },
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

## Init tools for development

On va utiliser les outils suivants pour le développement :
- [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer)
- [PHPStan](https://phpstan.org/user-guide/getting-started)
- [Pest](https://pestphp.com/docs/installation)


### Cache folder

On va créer un dossier `cache` dans le dossier `config` pour y mettre les fichiers de cache. Dedans on y ajoute
le fichier `.gitignore` pour ne pas versionner les fichiers de cache.

```gitignore
*
!.gitignore
```

### PHP_CodeSniffer

On va utiliser PHP_CodeSniffer pour vérifier que le code est bien écrit selon les standards du PSR.

On va utiliser des packages en plus de phpcs pour analyser le code.

### PHPStan

On va utiliser PHPStan pour la partie analyse statique du code. Cela permet de vérifier que le code
ne comportement pas d'erreurs de typage, et autres erreurs qui peuvent être détectées sans avoir à run le code.

Pour lancer l'analyse statique, on lance la commande `vendor/bin/phpstan analyse -c phpstan.neon`.

Si ce n'est pas déjà fait, on peut ajouter le fichier index.php dans le dossier `public`.

### Pest (Le framework de test)

On va utiliser Pest comme framework de test, car il est plus simple à utiliser que PHPUnit. Une fois la commande
`composer require pestphp/pest --dev` lancée, et la commande `./vendor/bin/pest --init` lancée, on a la création
de dossier et fichiers par défauts qui sont :

```markdown
tests/
├── Feature/
│   └── ExampleTest.php
├── Unit/
│   └── ExampleTest.php
├── Pest.php
└── TestCase.php
```

On peut retirer les fichiers d'exemples `ExampleTest.php` des dossiers `Feature` et `Unit`. On peut aussi retirer
le contenu du fichier `Pest.php` et de ne mettre que la ligne `<?php` dedans.

## Scripts

Pour simplifier l'utilisation des commandes des différents outils, nous allons créer des scripts dans le fichier
composer.json.

```json
{
    "scripts": {
        "lint": [
            "vendor/bin/phpcs",
            "vendor/bin/phpstan analyse -c phpstan.neon --xdebug --memory-limit=1G"
        ],
        "fix": [
            "vendor/bin/phpcbf"
        ],
        "test": "vendor/bin/pest",
        "test:cov": "vendor/bin/pest --coverage --min=100"
    }
}
```

À partir de maintenant, nous pouvons lancer les commandes suivantes :

```bash
composer lint # Lint le code
composer fix # Fix le code
composer test # Lance les tests
composer test:cov # Lance les tests avec le coverage
```
