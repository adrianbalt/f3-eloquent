# Eloquent Wrapper for Fat Free Framework 

This is a library package to use Laravel's [Eloquent ORM](http://laravel.com/docs/5.0/eloquent) with Fat Free Framework .


## Package Installation

To install this package, edit your `composer.json` file:

```js
{
    "require": {
        "adrianbalt/f3-eloquent": "dev-master"
    }
}
```

Now run:

`$ composer install`

Database Configuration:

```php

$options = array( \PDO::ATTR_ERRMODE => \PDO::ERRMODE_EXCEPTION );
$db=new DB\SQL("mysql:host=".DB_HOST.";port=3306;dbname=".DB_NAME, DB_USER, DB_PASS, $options);
$f3->set('DB', $db); // IMPORTANT: use 'DB' as key

```

# Usage Example

## Basic Usage

```php

$db = \WeDevs\ORM\Eloquent\Database::instance();

var_dump( $db->table('users')->find(1) );
var_dump( $db->select('SELECT * FROM users WHERE id = ?', [1]) );
var_dump( $db->table('users')->where('user_login', 'john')->first() );

// DB facade
use \WeDevs\ORM\Eloquent\Facades\DB;

var_dump( DB::table('users')->find(1) );
var_dump( DB::select('SELECT * FROM users WHERE id = ?', [1]) );
var_dump( DB::table('users')->where('user_login', 'john')->first() );
```

## Creating Models For Custom Tables
You can use custom tables to create models:

```php
	namespace whatever;


	class CustomTableModel extends \WeDevs\ORM\Eloquent\Model {

		/**
		 * Name for table without prefix
		 *
		 * @var string
		 */
		protected $table = 'table_name';


		/**
		 * Columns that can be edited - IE not primary key and timestamps if being uses
		 */
		protected $fillable = [
			'city',
			'state',
			'country'
		];

		/**
		 * Disable created_at and update_at columns, unless you have those.
		 */
		public $timestamps = false;

		/** Everything below this is best done in an abstract class that custom tables extend */

		/**
		 * Set primary key as ID, because WordPress
		 *
		 * @var string
		 */
		protected $primaryKey = 'ID';

		/**
		 * Make ID guarded -- without this ID doesn't save.
		 *
		 * @var string
		 */
		protected $guarded = [ 'ID' ];

		/**
		 * Overide parent method to make sure prefixing is correct.
		 *
		 * @return string
		 */
		public function getTable()
		{
			//In this example, it's set, but this is better in an abstract class
			if( isset( $this->table ) ){
				$prefix =  $this->getConnection()->db->prefix;
				return $prefix . $this->table;

			}

			return parent::getTable();
		}

	}
```

### Retrieving All Rows From A Table

```php
$users = $db->table('users')->get();

foreach ($users as $user) {
    var_dump($user->display_name);
}
```


### Other Examples

 - [Queries](http://laravel.com/docs/5.0/queries)
 - [Eloquent ORM](http://laravel.com/docs/5.0/eloquent)

## Writing a Model

```php
use \WeDevs\ORM\Eloquent\Model as Model;

class Employee extends Model {

	/**
	 * Name for table 
	 *
	 * @var string
	 */
	protected $table = 'employees';


	/**
	 * Columns that can be edited - IE not primary key and timestamps if being uses
	 */
	// protected $fillable = [
	// 	'city',
	// 	'state',
	// 	'country'
	// ];

	/** Everything below this is best done in an abstract class that custom tables extend */

	/**
	 * Set primary key as ID, because WordPress
	 *
	 * @var string
	 */
	protected $primaryKey = 'idEmployee';

	/**
	 * Make ID guarded -- without this ID doesn't save.
	 *
	 * @var string
	 */
	protected $guarded = [ 'idEmployee' ];

}

var_dump( Employee::all()->toArray() ); // gets all employees
var_dump( Employee::find(1) ); // find employee with ID 1
```


## Minimum Requirement
 - PHP 5.3.0

## Original Idea from
[Tareq Hasan](http://tareq.wedevs.com)