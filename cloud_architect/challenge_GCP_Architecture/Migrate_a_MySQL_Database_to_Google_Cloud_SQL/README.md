# Migrate a MySQL Database to Google Cloud SQL

[url](https://google.qwiklabs.com/focuses/1740?parent=catalog)

## Topics tested

Create a Google Cloud SQL Instance and create a Database
Import a MySQL database into Google Cloud SQL
Reconfigure an application to use Google Cloud SQL instead of a local MySQL database

## Challenge scenario

Your WordPress blog is running on a server that is no longer suitable. As the first part of a complete migration exercise, you are migrating the locally hosted database used by the blog to Google Cloud SQL.

The existing WordPress installation is installed in the `/var/www/html/wordpress` directory in the Compute Instance called blog that is already running in the lab. You can access the blog by opening a web browser and pointing to the external IP address of the blog instance.

The existing database for the blog is provided by MySQL running on the same server. The existing MySQL database is called wordpress and the user called **blogadmin** with password **Password1***, which provides full access to that database.

## Your challenge

You need to create a new Google Cloud SQL instance to host the migrated database. Once you have created the new database and configured it, you can then create a database dump of the existing database and import it into Cloud SQL. When the data has been migrated, you will then reconfigure the blog software to use the migrated database.

For this lab, the WordPress site configuration file is located here: `/var/www/html/wordpress/wp-config.php`.

To sum it all up, your challenge is to migrate the database to Google Cloud SQL and then reconfigure the application so that it no longer relies on the local MySQL database. Good luck!

> Note: Your lab activity tracking score will initially report a score of 20 points because your blog is running. If you reconfigure the blog application to use Google Cloud SQL database successfully, those points will remain in your grand total.
> 
> If the database has been incorrectly migrated, the blog is running test will fail, reducing your score by 20 points.

## Tips and Tricks

- **Google Cloud SQL - How-To Guides**: The Google Cloud SQL documentation includes a set of [How-to guides](https://cloud.google.com/sql/docs/mysql/how-to) that provide guidance on how to create instances and databases, and how to connect applications to those databases.

- **WordPress Installation and Migration**: The [WordPress Codex](https://codex.wordpress.org/Installing_WordPress) provides information on how to install, configure, and migrate WordPress sites. You will find the instructions on how to create and prepare databases for use with WordPress [here](https://codex.wordpress.org/Installing_WordPress#Detailed_Instructions).