# Using XDebug with Docker for Mac and IntelliJ or PHPStorm

1. Add the following into your Dockerfile

    ```
    RUN yes "" | pecl install xdebug                                                             && \
    	echo "zend_extension=/usr/lib/php5/20121212/xdebug.so" > /etc/php5/fpm/conf.d/xdebug.ini && \
    	echo "xdebug.remote_enable = on" >> /etc/php5/fpm/conf.d/xdebug.ini                      && \
	    echo "xdebug.remote_host = 10.254.254.254" >> /etc/php5/fpm/conf.d/xdebug.ini            && \
	    echo "xdebug.remote_port = 9000" >> /etc/php5/fpm/conf.d/xdebug.ini                      && \
    	echo 'xdebug.idekey = "INTELLIJ"' >> /etc/php5/fpm/conf.d/xdebug.ini                     && \
	    echo 'xdebug.remote_log = /var/log/xdebug_remote.log' >> /etc/php5/fpm/conf.d/xdebug.ini && \
    	touch /var/log/xdebug_remote.log && chown www-data:www-data /var/log/xdebug_remote.log
    ```
2. Setup an alias for `localhost` on your Mac
    * In terminal.app run the following command:
      `sudo ifconfig lo0 alias 10.254.254.254`

3. Configure PHPStorm / IntelliJ Preferences
    * Press &#8984;, (Command - comma) to open the Preferences pane
    * Navigate to `Languages & Frameworks` -> `PHP`
    * Make sure a PHP interpreter is set 
        ![Set Interpreter](https://coleca.github.io/imgs/phpstorm-debug/set_interpreter.png?raw=true)
    
    * If not, press the `...` button and setup an interpreter based on the PHP installation on your Mac
        ![Create Interpreter](https://coleca.github.io/imgs/phpstorm-debug/create_interpreter.png?raw=true)
   
    * Navigate to `Languages & Frameworks` -> `PHP` -> `Debug`
    * Make sure these settings are checked.  Note: that `Break at first line in PHP scripts` is optional
        ![Debug Settings](https://coleca.github.io/imgs/phpstorm-debug/debug_settings.png?raw=true)

    * Navigate to `Languages & Frameworks` -> `PHP` -> `Servers`
        ![PHP Server](https://coleca.github.io/imgs/phpstorm-debug/php_server.png?raw=true)
    * 
    
4. Configure PHPStorm / IntelliJ Project Debug Settings
    * Click the Run/Debug Configuration dropdown button to setup a new debug config, or choose "Edit Configurations..." from the "Run" menu. 
        ![Debug Menu](https://coleca.github.io/imgs/phpstorm-debug/debug_menu.png?raw=true)
    * Click the `+` button to create a new config
    * Choose "PHP Remote Debug" from the dropdown menu
        ![Add Debug Config](https://coleca.github.io/imgs/phpstorm-debug/add_debug_config.png?raw=true)
    * Setup the config according to the screenshot below:    
        ![Debug Config](https://coleca.github.io/imgs/phpstorm-debug/debug_config.png?raw=true)
    * Make sure that the Server is set to the server you created in step 3
    * Make sure that the "Ide key" is set to `INTELLIJ` (or whatever you have `xdebug.idekey` set to in your Dockerfile)

5. Make sure your Docker container is running with `docker run` or `docker-compose up -d`

6. Start the debugger by pressing the green debug toolbar button or press CTRL-D 
         ![Debug Start](https://coleca.github.io/imgs/phpstorm-debug/debug_start.png?raw=true)

7. Submit your request, or browse to a page on localhost:8080
