
Tutorial per crear plugins en WordPress: 
https://www.youtube.com/watch?v=Z7QfH-s-15s&t=288s


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
        function __construct() {
                add_action( 'init', array( $this, 'custom_post_type' ) );
        }
        function activate() {
                $this->custom_post_type();//crida un metode dins d'un altre metode
                flush_rewrite_rules();
                //encara que es desactivi el plugin el new titol que hagis creat es guardara
        }
        function deactivation() {
                flush_rewrite_rules();
        }
        function uninstall() {

        }
        function custom_post_type() {
                register_post_type( 'llibte', ['public' => true, 'label' => 'PATATAAAAAAAA'] );
        }

}

if ( class_exists( 'MariaPlugin' ) ) {
        $mariaplugin = new MariaPlugin();
}
//fem els arrays per indicar a quin metode volem accedir i que faci el hook, $mariaplugin es la variable en la que es guarda la classe i pertant té els metodes
register_activation_hook( __FILE__, array( $mariaplugin, 'activate' ) );
register_deactivation_hook( __FILE__, array( $mariaplugin, 'deactivation' ) );

?>
´´´
