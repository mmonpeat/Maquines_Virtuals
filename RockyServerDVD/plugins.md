
Tutorial per crear plugins en WordPress: 
https://www.youtube.com/playlist?list=PLriKzYyLb28kR_CPMz8uierDWC2y3znI2

Per instal·lar phpMyAdmin: 
https://www.golinuxcloud.com/install-phpmyadmin-rocky-linux-9/


maria.php
*********

```
<?php

/**
 * @package MariaPlugin
 *
 */

/*
Plugin Name: Maria Plugin
Description: holaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
Version: 1.0.0
Author: Maria
License: GPLv2 later
Text Domain: maria-plugin
*/

defined( 'ABSPATH' ) or die( 'Hey, bitch' );

class MariaPlugin
{
        public $public;

        function __construct() {
                // Aquesta línia estableix la propietat "plugin" amb el nom base del plugin (és una funció de WordPress) que correspon a aquest arxiu.
                $this->plugin = plugin_basename( __FILE__ );
        }
        function register() {
                /* Afegeix una acció per a l'admin del WordPress per cridar la funció enqueue i add_admin_pages.
                El primer argument especifica el "hook" (ganxo) al qual vols associar la funció que s'ha de cridar quan es produeix aquest ganxo.
                ex:  utilitza el ganxo 'admin_enqueue_scripts', que s'activa quan s'està carregant la part d'administració (admin) de WordPress. Quan aquest ganxo es                         dispara, la funció $this->enqueue s'executarà.
                */
                add_action( 'admin_enqueue_scripts', array($this, 'enqueue') );
                add_action( 'admin_menu', array( $this, 'add_admin_pages' ) );
                // echo $this->plugin;
                plugin_basename( __FILE__ );// Crida la funció "plugin_basename" per obtenir el nom base del plugin, però no fa res amb aquesta informació.
                add_filter( "plugin_action_link_$this->plugin",array( $this, 'settings_link') );//afegeix un filtre per a les accions del plugin per cridar la funció                         "settings_link" d'aquesta classe.
        }

        public function settings_link( $links ) {
                $settings_link = '<a href="admin.php?page=maria_plugin">Settings</a>';//defineix un enllaç d'ajustos per al plugin.
                array_push( $links, $settings_links );//afegeix l'enllaç als enllaços existents
                return $links;
        }
        public function add_admin_pages() {
                add_menu_page( 'Maria Plugin', 'Maria', 'manage_options', 'maria_plugin', array( $this, 'admin_index' ), 'dashicons-stordashicons-store', 110 );//Afegeix una pàgina d'administració al menú de WordPress.
    }
        }
        public function admin_index() {
                require_once plugin_dir_path( __FILE__ ) . 'templates/admin.php';//inclou el fitxer d'administració.
        }
        function activate() {
                //flush_rewrite_rules();
                require plugin_dir_path( __FILE__ ) . 'inc/plugin-activate.php';//es carregarà un fitxer específic quan es produeixi l'activació del plugin.
                PluginActivate::activate(); //Crida a la funció "activate" de la classe "PluginActivate."
        }
        protected function create_post_type() {
                add_action( 'init', array( $this, 'custom_post_type' ) );//Afegeix una acció "init" per cridar la funció "custom_post_type" d'aquesta classe.
        }

        /*function custom_post_type() {
                register_post_type( 'book', ['public' => true, 'label' => 'PATATAAAAAAAA'] );
        }*/
        function enqueue() {
                wp_enqueue_style( 'mypluginstyle', plugins_url( '/aix/mystyle.css', __FILE__ ) );
                wp_enqueue_script( 'mypluginscript', plugins_url( '/aix/myscript.js', __FILE__ ) );
        }
}

$mariaplugin = new MariaPlugin();//Instància de la classe MariaPlugin.
$mariaplugin->register();// Crida a la funció "register" de la instància per configurar el plugin.

//fem els arrays per indicar a quin metode volem accedir i que faci el hook, $mariaplugin es la variable en la que es guarda la classe i pertant té els metodes
register_activation_hook( __FILE__, array( $mariaplugin, 'activate' ) );

require_once plugin_dir_path( __FILE__ ) . 'inc/plugin-deactivate.php';
register_deactivation_hook( __FILE__, array( 'PluginDeactivate', 'deactivate' ) );


?>
´´´
### function register():
add_filter( "plugin_action_link_$this->plugin", array( $this, 'settings_link' ) );
Aquest filtre està dissenyat per afegir o modificar els enllaços d'acció d'un plugin.
- "plugin_action_link_$this->plugin" és el nom del ganxo de filtre al qual s'està afegint la funció. Aquesta és una cadena que sembla ser dinàmica i està basada en una propietat $this->plugin de la classe actual. El valor d'aquesta cadena serà específic del plugin o del context en el qual s'estigui utilitzant aquest codi.

- "array( $this, 'settings_link' )" especifica la funció de filtratge que es cridarà quan s'activi aquest ganxo. $this fa referència a la instància actual de la classe, i 'settings_link' és el nom del mètode dins de la classe que s'executarà com a part del filtre. Aquest mètode pot modificar o afegir enllaços a les accions del plugin, com ara enllaços a pàgines de configuració o altres funcionalitats relacionades amb el plugin.
