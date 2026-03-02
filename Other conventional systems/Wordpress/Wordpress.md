%% Begin Waypoint %%
- [[Database analysis]]
- [[Plesk analysis]]
- [[Script]]

%% End Waypoint %%


Wordpress is usually installed on Linux systems, so the forensic page [[Linux]] applies to this kind of system.

The important files are usually located in www related path : 

- /var/www/vhosts/

Each subsequent path concerns a different vhost, understand a different website hosted on the same server.

The following path contains plugin :
- /var/www/vhosts/<vhost_name>/httpdocs/wp-content/plugins/<plugin_name>/

Those plugin should always be known by https://wordpress.org/plugins/<plugin_name>, otherwise this could indicate either a custom developed plugin or a malicious one.

When doing malicious plugin analysis, the following Wordpress actions / filter can be found for example : 
- add_action( 'template_redirect', 'custom_mobile_homepage_logic', 99999 );
- add_filter( 'all_plugins', function( $plugins ) { ... }

These actions are used to load scripts before the page loading, or hide information to other plugins.

The full list of actions and filters can be found here : https://adambrown.info/p/wp_hooks/hook : 



