



<p align="center"> MEMENTO</p>

<p align="center"><img src="https://www.a5sys.com/wp-content/uploads/2021/10/symfony_logo_vertical.png" width="250px"></p>



# Prérequis: Symfony, Composer et PHP d'installé 





## Créer un nouveau projet : 

##### Nouveau projet : ```composer create-project symfony/skeleton:"6.3.*" my_project_directory ``` 



##### Projet déjà existant : ```composer install```



##### Lancer Symfony en localhost : ```symfony server:start``` ou ```php -S localhost:8000 -t public```

## Controller, Views & Entity

##### Créer un Controller et la View correspondante :  ``symfony console make:controller name``



##### Créer une entité : ``` php bin/console make:entity``` 


## Formulaires 

##### Créer un formulaire :  ```php bin/console make:form```

##### Par défaut, sur la partie twig, lors de la création des formulaires, les inputs et les labels seront groupés :  

##### - soit sous la forme complète (tout le formulaire en un) : exemple : ```{{ form_widget(form) }}``` 

##### - soit sous forme de "row" (input + label groupé) exemple :  ```{{ form_row(registrationForm.email) }}```

##### Si vous souhaitez appliquer une class sur l'input et/ou modifier le label, vous pouvez décomposer chaque élément en deux partie comme ceci : 

 ```{{ form_label(registrationForm.email, 'Adresse Email') }}``` ==> choix du label 

  ```{{ form_widget(registrationForm.email,{'attr': {'class': 'form-control'}}) }}``` ==> choix de la classe

## Base de donnée : 

##### Connexion à la base de donnée :  dans le fichier .env , ajouter cette ligne à la place de celle initiale :
```DATABASE_URL="mysql://user:mdp@127.0.0.1:3306/nomdelabd?serverVersion=mariadb-10.4.21"``` 
##### (version de mariadb en 10.4.21 chez nous)


##### Créer la Base de donnée :   ```php bin/console doctrine:database:create   ```


##### Migrer les entité sur la base de donnée : ** ```php bin/console make:migration``` puis ```php bin/console doctrine:migrations:migrate```


## Créer ses propres requettes DQL :

Dans votre Contrôler, vous pouvez appeller des fonctions dans lesquelles vous avez créer des requêtes DQL personnalisés. 

*exemple avec la fonction* ```findPostValidated()```

```php
public function index(PostRepository $postRepository): Response
{
    
    return $this->render('index/index.html.twig', [
        'controller_name' => 'IndexController',
        'postshow' => $postRepository->findPostValidated(),
    ]);
}
```

Coté Repository la fonction se construit de cette manière : 

```php
public function findPostValidated(): array
    {
        $entityManager = $this->getEntityManager();

        $query = $entityManager->createQuery(
            "SELECT p
            FROM App\Entity\Post p 
            WHERE p.publishedAt <= CURRENT_DATE()
            ORDER BY p.publishedAt ASC"
        );

        // returns an array of Product objects
        return $query->getResult();
    }
```




##  Qu'est ce qu'un CRUD ? 

<span style='color:red'>L'acronyme informatique anglais CRUD désigne les quatre opérations de base pour la persistance des données, en particulier le stockage d'informations en base de données. Soit : create : créer read : lire update : mettre à jour delete : supprimer  </span>

#### Créer un CRUD: 

Sous Symfony la commande ```php bin/console make:crud``` permet de générer un Controller lié à une entité avec toute les fonctions gérant le CRUD. 	De même,  les différentes View propres au CRUD sont créées.



## Installer Bootstrap et scss :

Installer WebpackEncore: ```composer require symfony/webpack-encore-bundle```

Mettre à jout npm : ```sudo npm install -g npm```

installer des dépendances Js et CSS avec `npm`:  ```npm install bootstrap jquery popper.js sass-loader@^9.0.1 node-sass@4.14.1 --save-dev```

relancer npm : ```npm install```

	

### Bootstrap :

- Depuis le dossier `assets`, ajouter un dossier `scss` puis le fichier `app.scss` file on it.
  Ajouter la librarie `boostrap`: 

  
 
 
 ```javascript
  // Depuis assets/scss/app.scss 
  @import "~bootstrap/scss/bootstrap";
  ```

- Toujours depuis le dossier `assets`, ouvrir le fichier `app.js`

  - Importer le css dans le fichier js:
  - Importer aussi `Jquery` et `Popper.js`

 
 
 
 
 ```javascript
  import './scss/app.scss';
  
  import 'bootstrap';
  import $ from 'jquery';
  import 'popper.js';
  ```



### SCSS :

- Dans le fichier `webpack.config.js`, décommenter la ligne 59 (environ) : ```.enableSassLoader()```

- Pour compiler le javascript et le scss, utiliser la commande suivante : ```npm run dev``` ou ```npm run watch``` (plus cool car besoin de la faire qu'une fois)


## Autres : 

##### Récupérer un attribut user de "session" : 

##### exemple : ``{{app.user.id }}``


