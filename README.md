# primal-php
Primal it's a PHP framework for template and SQL query manage.
**Install**
Install using composer
> composer.json

    {
      "require": {
        "luismoralesp/primal": "1.0"
      }
    }

**Get Started**
The first stpep for use Primal is star a new proyect, we will use the bellow command:

    $ php vendors/Kernel/commands/start.php myapp
Then we will have a new folder structure:

 - myapp
   - models
   - views
     - Myapp.php
 - index.php

`myapp` folder is our proyect folder  inside it we will found the `models` folder where we will put our models class files and `views` folder where we will put our view class files, by default a file class was createt with our project the name but with first word  uppercase this important for this framework.

Inside the file we will found the bellow code:

```php
    <?php 

  class Myapp extends JsonView {
    /**
     * This is an autogenerated code for started project
     **/
    public function index(){
      return '{"message": "Congratulation this is your first service using primal"}';
    }
  }
```
and finally a file in the root, named `index.php`, here we will found the our project configuration routing, by default we will found this code:
```php
 <?php
  /**
   * Import kernel framework and routing 
   */
  require_once 'vendors/primal/kernel/Kernel.php';
  require_once 'vendors/primal/kernel/Router.php';
  /**
   * Set a default routing
   */
  $router = Router::getInstance('myapp.myapp.index');
  /**
   * Configure routing
   */
  $router->setUrls([
      'hi' => 'myapp.myapp.index'
  ]);
  /**
   * Show routed service
   */
  echo $router->rout(realpath(dirname(__FILE__)));
```
With the parameter in de `Router::getInstance(default_rouing)`  will set the path to the serive method in the view file, using the bello format:

    [app_name].[view_file].[method_name]


So, if we invoke `/` or `/hi/` in the web browser
We will recive:
```json
{"message": "Congratulation this is your first service using primal-php"}
```

**Creating a model**
A model reprecent directly a table in the data base, the model is an interface for manipulate each database table.

For define an *model* we will need to create a class with the same name that our table and extends it from base class **Model**.
```php
Kernel::package('app');
class MyModel extends Model{
```
Each model need belong to an app, for this we must to use:

`Kernel::package(package_name);`

So in our class file  we have to define our fields and contrains.
```php
  private $id;
  private $first_name;
  private $last_name;
  private $tel;
  
  public function __construct(){
    $this->id = Constrain::pk($this);
    $this->first_name = Input::create_text('first_name');
    $this->last_name = Input::create_text('last_name');
    $this->tel = Input::create_integer('tel');
  }
  /* So your getters and setters */
```
- The `Constrain::pk` method is used  `primary key` fields for this model, see about more contrains in the CONSTRAINS.md tuto.
- The `Input::create_text` and `Input::create_integer` methods specify a varchar and int columns respectly in the table reprecented by this model, read about more inputs in the INPUTS.md tuto.

For use that model class in our view file we will have to import it, using the bellow line:

    Kernel::import('app.MyModel ');
  
Then we can use the model in our view, for example calling a filter method, it we will return all rows in our table for this model:

    MyModel::persistence()->filter();

  
Then we will to have a view file like this:
```php
    <?php 
  Kernel::import('app.MyModel ');
  class Myapp extends JsonView {
    /**
     * This is an autogenerated code for started project
     **/
    public function index(){
      return MyModel::persistence()->filter();
    }
  }
```