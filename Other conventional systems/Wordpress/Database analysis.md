---
tags:
  - EN
  - Finished
  - Linux
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

Database analysis answers the question of "What happened on wordpress ?".
And database analysis can give the following answers :
- Did an admin user get created, 
- Are there malicious plugin and where are they, 
- Is there SEO spam injected, 
- Are there malicious internal wordpress crontab created ?

And many more. But there are two problem to solve first : 
- How do we mount the database(s) from disk ?
- What to query ?

# Mounting the database

First identify the name of the DB associated with the wordpress website you want by checking `wp-config.php`
You"ll find `define('DB_NAME', 'wordpress_db')`.

And it's important to check the other vhost on the server because DB may be linked together between webhost if wrongly configured => Further spreading infection between vhosts.

On internet you'll find name of databases you wanna analyse : 
- wp_options (the most importants)
- wp_users
- wp_usermeta

But on disk you'll find them with <DB_NAME>  before them instead of wp. So wp_option will be for exemple Frvlok_options.

## Recover/read InnoDB

Get the `<db_name>.ibd` and `<db_name>.frm` files matching the database you want to read

Get undrop-for-innodb tool : 

```
$ git clone https://github.com/twindb/undrop-for-innodb.git
$ cd undrop-for-innodb/
$ sudo apt-get update
$ sudo apt-get install gcc flex bison
$ make
```

Recover a table schema from `<db_name>.frm`

```
$ curl -s http://get.dbsake.net > dbsake
$ chmod u+x dbsake
$ ./dbsake frmdump <db_name>.frm > <db_name>.sql
```

Display the database schema and use this MySQL query to create the table in a new database.

```sql
$ cat <db_name>.sql
--
-- Table structure for table ``
CREATE TABLE `<db_name>` (...)
```

```
$ sudo mysql -u root
MariaDB []> CREATE DATABASE <whateveryouwant, can be test>
MariaDB []> USE test
MariaDB [test]> CREATE TABLE `<db_name>` (...)
```

Fetch records from `<db_name>.ibd`. First step would be to parse the ibd file and sort InnoDB pages in it.

```sql
$ ./stream_parser -f users.ibd
```

The `stream_parser` creates `pages-<db_name>.ibd/FIL_PAGE_INDEX/0000000000000367.page`. The number will be different always pick the smallest one. This is a file with InnoDB pages of the `PRIMARY` index of the `<db_name>` table. Basically, this is where the table records are.

Second step is to fetch records from `0000000000000367.page`.

-  <b>Absolutely create the folder dumps/default</b> as this is hardcoded in the code and will break paths in SQL Query if not

```sql
$ mkdir -p dumps/default

- Same, point the output folder to dumps/default/<db_name>
$ ./c_parser -f pages-<db_name>.ibd/FIL_PAGE_INDEX/0000000000000367.page -t <db_name>.sql 2> load-<db_name>.sql > dumps/default/<db_name>

The command saves records in a file `dumps/default/<db_name>`.  The files must be in this exact directory or you will get an error later.


The SQL query used in  `load-<db_name>.sql`

```bash
$ mysql my_database < load-<db_name>.sql
```

To explore the database start mysql and then make SQL queries

``````
$ sudo mysql -u
``````

## What to query ?

As a reminder, the 3 most important tables are the following : 
- wp_options (the most importants)
- wp_users
- wp_usermeta

### Plugins and persistance

<b>Active plugins</b>

```
SELECT option_value FROM wp_options WHERE option_name = 'active_plugins';
```
![[Pasted image 20260123160844.png]]
This can then be fed to the script in [[Script]].

<b>Internal Wordpress crontabs</b>

```
SELECT option_value FROM wp_options WHERE option_name = 'cron';
```
![[Pasted image 20260123161130.png]]

This does give the name of Wordpress crontabs and their schedule.
However, there a multiple **important things** to notice about wordpress crons : 
- Those are not real cron
- A task is done when someone visit the site, and if the last timestamp when cron task was used is compare to the request timestamp to know if it must do action
- When it is time to make the cron action, wordpress does : do_action( '<action_name>' );
- To identify action, you must grep the files and identify where "add_action('<action_name>')" is made to identify what the action does.

#### TLDR

- Database : <b>WHEN/Name of actions</b>  
- `add_action()` : <b>WHO</b>
- Details in  `add_action()` : <b>WHAT/HOW</b>

<b><u>External callbacks</u></b>

```
SELECT option_name, option_value FROM FmVdo_options WHERE option_value like '%http%';
```

![[Pasted image 20260123164628.png|400]]

<b><u>Redirections</b></u>

```
SELECT option_value FROM wp_options
WHERE option_name IN ('siteurl','home');
```

<b><u>Available roles</b></u>

```
SELECT option_value FROM wp_options WHERE option_name ='wp_user_roles';
```

<b><u>Identify users</b></u>

```
SELECT ID, user_login, user_email FROM wp_users;
```

<b><u>Admin users</b></u>

```
SELECT user_id, meta_value FROM wp_usermeta WHERE meta_key LIKE '%capabilities%';
```

<b><u>Get more information on a user</b></u>

```
SELECT umeta_id, user_id, meta_key, meta_value WHERE user_id='<id>';
```

<b><u>Full wp_option check (long to do)</u></b>

`SELECT option_name,option_value FROM FmVdo_options ;`

### Themes and persistance

The classic ways of infection through a plugin are the following :
- Admin uploads a cracked/modified/unofficial theme
- Vulnerability in theme (write file)
- Theme Editor usage by compromised admin account

Themes path is the following :
- wp-content/themes/

For exemple a theme called `<myowntheme>` would be located in : 
- wp-content/themes/myowntheme

#### Database analysis

<b>Loaded theme</b> 

```
SELECT option_value
FROM wp_options
WHERE option_name = 'stylesheet';
```

Active theme, corresponds to its folder for CSS + templates loaded

<b>Parent template (used for child theme)</b>

```
SELECT option_value
FROM wp_options
WHERE option_name = 'template';
```

Folder from the theme which the child inherits. Child theme load the parent code, and complete it with his own code.
#### Files to audit

| File          | Full path                                  | Wordpress usage                                 |
| ------------- | ------------------------------------------ | ----------------------------------------------- |
| functions.php | `/wp-content/themes/mytheme/functions.php` | Loaded at each request, before templates        |
| header.php    | `/wp-content/themes/mytheme/header.php`    | Included in almost all pages                    |
| footer.php    | `/wp-content/themes/mytheme/footer.php`    | Included in almost all pages                    |
| index.php     | `/wp-content/themes/mytheme/index.php`     | Used if no template correspond within the theme |
| single.php    | `/wp-content/themes/mytheme/single.php`    | Used for static pages                           |
| page.php      | `/wp-content/themes/mytheme/page.php`      | Used for static pages                           |
| archive.php   | `/wp-content/themes/mytheme/archive.php`   | Conditional file                                |
| search.php    | `/wp-content/themes/mytheme/search.php`    | Conditional file                                |
| 404.php       | `/wp-content/themes/mytheme/404.php`       | Conditional file                                |

#### Apache file : .htaccess

.htaccess if an Apache configuration file, it's loaded before PHP (TODO : VERIFY), it can be used for :
- URL rewriting 
- Redirects Access control P
- HP configuration 
- HTTP headers 
- Restriction files
- SEO spam
- Backdoor

.htaccess file(s) are located in : 
- Website root
- Subfolders like : 
	- /wp-admin/.htaccess
	- /wp-includes/.htaccess
	- /wp-content/.htaccess
	- /wp-content/uploads/.htaccess

Secondary .htaccess influence their directory and subdirectory.

In Wordpress, there is a plugin to modify .htaccess files called [(Htaccess Editor)](https://fr.wordpress.org/plugins/wp-htaccess-editor/), check for this plugin in case of compromised .htaccess, it can indicate an admin account compromise.


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>


### Resources

The sources used for the creation of this article are the following :
- [GitHub - twindb/undrop-for-innodb: TwinDB data recovery toolkit for MySQL/InnoDB](https://github.com/twindb/undrop-for-innodb)
- https://stackoverflow.com/questions/75090681/restore-table-from-frm-and-ibd-file
- .htaccess shells : https://github.com/wireghoul/htshells
- .htaccess administration shenanigans : https://www.active24.de/en/how-to-website-creation/wordpress/how-to-secure-wordpress-without-plugins-just-using-the-htaccess-file
- .htaccess wordpress shenanigans https://wpmarmite.com/htaccess-wordpress/
- Wordpress .htaccess modification plugin : https://fr.wordpress.org/plugins/wp-htaccess-editor/

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2026-01-21 18:21 | Page creation |