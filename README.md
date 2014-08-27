dokku-app-configfiles
=====================

dokku-app-configfiles is a plugin that allows [dokku][1] app config files (such
as `VHOST`, `ENV`, `DOCKER_OPTIONS` (for the [dokku-docker-options][2] plugin),
`nginx.tpl` or `nginx.ssl.tpl` (for the [dokku-nginx-alt][3] plugin), etc. to be
taken from the app's git repository. This eliminates the need to manually create
these files on the dokku server.

In short, per-app dokku config files are checked into the app's git repository
under the directory `.dokku/`. Upon git push to the dokku server, any files in
`.dokku/*` are copied to `/home/dokku/[app name]/`.

[1]: https://github.com/progrium/dokku
[2]: https://github.com/dyson/dokku-docker-options
[3]: https://github.com/mikexstudios/dokku-nginx-alt


Why would I use this?
---------------------

Although it is prudent to always keep server configuration files separate from
app files, many cloud hosts (such as dotcloud) offer the option to supply
custom ngnix and other configuration files by checking them into your app's 
root directory. This approach is simple, and an advantage is that these
configuration files are always backed up with the app. This plugin enables
similar functionality for dokku.


Installation and Usage
----------------------

1. Install the plugin by cloning into the dokku plugins directory:
    ```sh
    git clone https://github.com/mikexstudios/dokku-app-configfiles.git /var/lib/dokku/plugins/app-configfiles
    ```

2. In your app's git repository (this is the app that you intend to push to
   dokku), create a folder called `.dokku`. For example, if my app was in a folder
   called `my-webapp/`, the folder would be created at `my-webapp/.dokku/`.
    ```sh
    cd my-webapp/ #this is the app you want to git push to dokku
    mkdir .dokku
    ```

3. Inside the `.dokku/` folder, place your dokku config files. For example, let's
   create some fixed environment variables:
    ```sh
    cd my-webapp/.dokku/ 
    echo "export HELLO='world'" > ENV
    ```
    NOTE: In practice, you may not want to check sensitive environment variables
    (such as API keys) into your git repository if it is publically accessible.

4. Add the `.dokku/` directory to your app's git repository:
    ```sh
    cd my-webapp/ #this is the app you want to git push to dokku
    git add .dokku/
    git commit -m 'Added dokku config files.'
    ```

5. Push your app to dokku and the config files in `.dokku/*` should be copied to
   your app's directory (e.g. `/home/dokku/my-webapp/`) on the dokku server.

   
License
-------

The MIT License (MIT)
