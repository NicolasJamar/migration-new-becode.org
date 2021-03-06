## For migrating the DB on your local PC


#### 1. Remove redirects.conf
To prevent some issue, you have to remove the file `redirects.conf`.

After lauching vagrant, go to the server : 

```
vagrant ssh
```

then, connect with sudo : 

```
sudo -i
```

Go to this folder :

```
cd /etc/nginx/includes.d/all
```

Check the files inside : `ll`

If there is a file `redirects.conf`, remove it : `rm redirects.conf`

Restart nginx :

```
service nginx restart
```

#### 2. becode.test

Go to `http://becode.test` on your web browser. 

If it's the first time you connect to becode.test, you must install WordPress. Fill in the form with fakes informations. 


#### 3. Retrieve the DB online on your local PC

connect to http://becode.test/wp-admin > *Check on 1Password for login and password* 

Activate all plugins **WP Migrate DB Pro**. 

> :point_right: *NB : You may need the licence to activate. Check on 1Password, to copy/paste it.*


Then, go to `Tools > Migrate DB Pro > Migrate`

Select `Pull`. You'll need to link with the connexion info of the DB online.

So, connect to http://becode.org/wp-admin and go to `Tools > Migrate DB Pro > Settings` and copy the **Connection info**.

Go back to http://becode.test/wp-admin `Tools > Migrate DB Pro > Migrate` and paste the connection info of the DB in the `Pull` field. 

:warning: Check the **Advanced options**. You mustn't import the plugins settings, so check the box `Do not migrate the active plugins settings`

It should be like that :

![migrating not import plugin settings](migratedb3.png)

The options `Find Replace` should look like that :

![migrating DB 1](migratedb1.png)


> :point_right: Don't select `Media files` option the first time you want to migrate the DB. 

Save your migration profile. 

Click on the button `Pull & Save`. It will take few minutes and it will done. 


#### Possible issue 

If you have this message :

![migrating DB 2](migratedb2.png)

That means you need to change the version of **WP Migrate DB Pro** on your project on local. 

Go to `./site` and open `composer.json`. 

Check for the line `"deliciousbrains-plugin/wp-migrate-db-pro": ...` and change with the correct version. 

After, in your terminal run `composer update`

## Deploy on the staging

> To launch the staging the first time `ssh -i staging-becode-website.pub web@staging.becode.org`

Go to the staging branch and run : 

```
git checkout staging
git pull
composer update
```

Go to the trellis folder and run : 

```
./bin/deploy.sh staging becode.org
```

If there is a problem, check the file `trellis/groups_vars/staging/users.yml`. It should look like that :

You have to change the keys with `.pem` file : `"{{ lookup('file', './ssh_keys/staging-becode-website.pem') }}"`

## For working with staging

1. Go on the 'staging' branch
1. `git pull`
1. Got to trellis folder : `cd trellis`
1. `vagrant up`
1. View changes on local with becode.test
1. commit & push
1. Deploy on staging

To comeback to a previous version use a [rollback](https://roots.io/docs/trellis/master/deployments/#rollbacks). 

### Possible issue after pulling the stage

In order to work on local with the staging, check the dev env configuration. 
A pull or a merge can change this file. 
Go to : `site > config > environments > development.php`

This file have to be configured like that :

![development file env](config_env_newb.png)