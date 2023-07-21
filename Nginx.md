# Overview

# Operation
## Ubuntu
1. Install
    To install Nginx, utilize the `apt-get` package manager. Once the installation is complete, Nginx will automatically start running.
    ```shell
    sudo apt update
    sudo apt install nginx
    ```
    Once Nginx is installed, you can create a `index.html` file under the `/var/www/html/`. To verify the installation, you can access the page by navigating to `http://localhost` in your web browser or use `curl http://localhost` command to check the content from the command line.

2. Check its status:
    To check its status, use
    ```shell
    sudo systemctl status nginx
    ```

3. Uninstall
    It is advisable to refrain from using the command `apt uninstall nginx` directly, as it may leave behind certain configuration files even after the main Nginx package has been removed.
    1. Login as root:
        ```shell
        sudo su -
        ```
    2. Then uninstall `nginx`:
        ```shell
        apt-get purge nginx nginx-common `nginx-full`
        ```
        - `purge` means that not only the package will be removed, but also all configuration files and data associated with that package will be deleted from the system. This ensures a complete and clean removal of the package, leaving no traces of its existence on your system.
        - The `nginx-common` package contains shared files and directories that are used by all Nginx flavors (variants). It includes configuration files, default server configurations, and other common files that are shared among different Nginx packages. When you uninstall `nginx`, it's a good practice to remove `nginx-common` as well to clean up any shared files or configuration remnants.
        - The `nginx-full` package is one of the Nginx flavors available. In Debian-based systems, the Nginx package is divided into different flavors to provide users with various installation options. The `nginx-full` package includes more features and modules compared to the nginx package. It may include additional modules for extended functionality. If you installed `nginx-full` previously, it includes everything from the nginx package as well. Uninstalling `nginx-full` will remove this flavor of Nginx, along with its additional features and modules.


4. Collision:
    Potential collisions may occur when both Apache and Nginx attempt to use the same port `80`. If you encounter the error message "A high-performance web server and a reverse proxy server," it is likely that Apache has already occupied port `80`.
    To check the port by port number, use
    ```shell
    sudo lsof -i:80
    ```
    To stop the apache server, use
    ```shell
    sudo service apache2 stop
    ```

5. Firewall:
    Sometimes we need to enable Nginx through `ufw` as well.
    1. Enable `ufw`:
        ```shell
        sudo ufw enable
        ```
    2. List the application configurations that `ufw` knows.
        ```shell
        sudo ufw app list
        ```
        You will get an output
        ```txt
        Output
        Available applications:
        Nginx Full
        Nginx HTTP
        Nginx HTTPS
        OpenSSH
        ```
        - Nginx Full: This profile opens both port 80 (normal, unencrypted web traffic) and port 443 (TLS/SSL encrypted traffic)
        - Nginx HTTP: This profile opens only port 80 (normal, unencrypted web traffic)
        - Nginx HTTPS: This profile opens only port 443 (TLS/SSL encrypted traffic)
    3. Enable the traffic.
        ```shell
        sudo ufw allow 'Nginx HTTP'
        ```
    4. Check the `ufw`'s status
        ```shell
        sudo ufw status
        ```

# Deployment
## Ubuntu
After installing Nginx, the file `/var/www/html/index.html` is considered the default entry file. To deploy your React project or any other frontend server, it is recommended to move the `build/` folder to the same directory `/var/www/html/`. Next, navigate to the configuration file located at `/etc/nginx/sites-available/default` and update the `root` directive to point to your `build/` folder. The modified `root` directive should look like this:
```txt
server {
    // ... other configurations
    root /var/www/html/build;
}
```

