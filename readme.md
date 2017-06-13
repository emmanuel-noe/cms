
- [API simple pour le developeur](#ap-simple-pour-le-developeur)
- [Création de plugin simplifié](#création-de-plugin-simplifié)
- [Moteur de template](#moteur-de-template)
- [Gestion des images](#gestion-des-images)

# API simple pour le developeur

S'inspirer d'API de type Laravel / Eloquent ORM pour éviter de devoir écrire du code MySQL ou du code PHP inutile. Rendre la récupération de contenu plus simple et beaucoup plus "fun".

#### Exemple de syntax à s'inspirer
##### Corcel pour WP

```php
<?php
// Récuperer tous les posts WP
$posts = Post::published()->get();
$posts = Post::status('publish')->get();

// Récuperer un post spécifique
$post = Post::find(31);
echo $post->post_title;

// Filtrer les posts par meta/custom field
$posts = Post::published()
              ->hasMeta('field')
              ->get();
$posts = Post::hasMeta('acf')->get();
```
Source : [Librairie Corcel pour WP](https://github.com/corcel/corcel)

##### Eloquent ORM
```php
<?php
$flights = Flight::where('active', 1)
                  ->orderBy('name', 'desc')
                  ->take(10)
                  ->get();

```
Avoir des updates ultra simples. Le code qui suit récupère la donnée, et la met à jour directement en base de donnée, pas d'autre code nécessaire.

```php
<?php
$flight = Flight::find(1);
$flight->name = 'New Flight Name';
$flight->save();
```


Source : [Eloquent ORM]( https://laravel.com/docs/5.4/eloquent)

##### Laravel Medialibrary
Le code suivant upload et lie automatiquement un média à une news.

```php
<?php
$newsItem = News::find(1);
$newsItem->addMedia($cheminVersFichier)->toMediaCollection('images');
```

Source : [Laravel Medialibrary](https://docs.spatie.be/laravel-medialibrary/v5/introduction)

Tous ces exemples sont là pour montrer que le developpement pourrait être beaucoup plus rapide en PHP si l'API est directement pensée pour le developpeur qui va ensuite devoir la manipulé et au temps nécessaire à cette manipulation. On le voit beaucoup de **choses existent** et **rien ne sert de réinventer des solutions à chaques fois**, qui ne sont pas performantes, pas utilisables par un dev ensuite et pas robustes.



# Création de plugin simplifié
S'inspirer de Themosis, de _flat CMS_ comme KirbyCMS ou Statamic ou même Podio.


##### Themosis
Cet exemple crée automatiquement un PostType de slug _news_ avec un titre et un éditeur avec en dessous un bloc pour l'auteur contenant un nom, un téléphone et une adresse. On déclare également les types attendus pour validation.

```php
<?php
PostType::make('news')->set([
    'supports' => ['title', 'editor']
]);
$metabox = Metabox::make('Auteur', 'news')->set([
    Field::text('nom'),
    Field::text('telephone'),
    Field::textarea('adresse')
]);
$metabox->validate([
    'nom'         => ['textfield', 'min:3', 'alpha'],
    'telephone'   => ['num', 'max:25'],
    'adresse'     => ['textarea']
]);
```


Source : [Framework Themosis](http://framework.themosis.com/docs/)

##### KirbyCMS

```yaml
title: Page
pages: true
files: true
fields:
  title:
    label: Title
    type:  text
  text:
    label: Text
    type:  textarea
```
![Kirby Panel](https://getkirby.com/content/1-docs/5-panel/1-blueprints/form.png)


L'exemple précédent part d'un simple _YAML_ de config et génère l'interface en backoffice automatiquement. Kirby possède aussi la possibilité de créer des nouveaux types de _field_. Exemple d'un plugin pour Kirby qui crée un nouveau type _geolocalisation_

```yaml
fields:
  location:
    label: Location
    type: geolocation
```

Automatiquement en back-office un nouveau champ est disponible :

![geolocation](https://raw.githubusercontent.com/lekkerduidelijk/kirby-geolocation-field/master/geolocation-field.gif)

Source : [KirbyCMS](https://getkirby.com/docs)

Prévoir aussi la possibilité de pouvoir ajouter par exemple un champs select rempli avec la liste d'un autre plugin. (Ex. _Slider_ qui a une propriété _link_ qui est un select de la lsite des pages)

Tout ces exemples nous montre que la déclaration dans un plugin de champs obligatoires, standards ou spécifiques, devrait se faire en peu de temps, avec peu de connaissances. Dans l'exemple de Kirby, un simple fichier _yaml_ suffit, juste une liste avec des tabulations, syntax parfaitement maîtrisable par un stagiaire ou même par un chef de projet (liste qui est souvent déjà réalisé par celui-ci dans un .doc)

Statamic qui a la même syntax en _yaml_ a aussi une interface de type Podio qui permet depuis le back-office de déclarer ces champs.

Source : [Statamic](https://statamic.com/features)


##### Piklist


![piklist](https://ps.w.org/piklist/assets/screenshot-1.png?rev=1120463)

Dans le même principe, il existe Piklist qui permet de créer en code des PostType, Metabox, des page Settings etc.. il a même + de types de champs possible que Themosis.

On peut y réfléchir en dépendance, pour par exemple une classe Plugin propre pour créer facilement des plugins ou générer depuis des yaml de config.

Source : [Piklist](https://wordpress.org/plugins/piklist/)


# Moteur de template
Prévoir _Twig_, moteur de template de Symfony, robuste et qui a fait ses preuves. Encourage la séparation entre code et template.

```twig
<ul id="navigation">
  {% for item in navigation %}
    <li>
        <a href="{{ item.href }}">{{ item.title }}</a>
    </li>
  {% endfor %}
</ul>
```

Prévoir la possibilité de pouvoir surcharger un template présent dans un plugin par celui du template. (Comme pour WooCommerce ou comme les composants sur Joomla)

Idée : Prévoir qu'automatiquement lors de la création d'un plugin/type des données (Cf chapitre précédent) un template _twig_ reçoit toutes les données déclarées dans la config (_yaml_ ou autre).

Source : [Twig](https://twig.sensiolabs.org/doc/2.x/)

# Gestion des images
Prévoir une gestion des images plus propre et simple qu'aujourd'hui avec par exemple l'utilisation de l'API Intervention Image. Quid de l'integration avec la lib de WordPress? et sa gestion de thumbnail horrible.

```php
<?php
// open an image file
$img = Image::make('public/foo.jpg');
// now you are able to resize the instance
$img->resize(320, 240);
// finally we save the image as a new file
$img->save('public/bar.jpg');
```

[Intervention Image](http://image.intervention.io/)
