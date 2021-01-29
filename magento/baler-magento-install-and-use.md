# Baler Setup


![logo](https://docsify.js.org/_media/icon.svg ':size=50x100')


**Steps To Install**

 - *Step 1*
 
Baler main NPM tool
Clone the Baler repository https://github.com/magento/baler/

 

Git clone git@github.com:magento/baler.git

– Or –

Download https://github.com/magento/baler/archive/master.zip

Follow instructions from https://github.com/magento/baler/blob/master/docs/ALPHA.md

Make sure your node version is >= 10.12.9
Run npm run build from the main Baler directory you just cloned/downloaded
Run npm link so that it’s installed globally.

Install the Magento Module
Baler needs a Magento module to be installed. https://github.com/magento/m2-baler

Either download/clone the repo and put it in app/code/ or, better, install via composer.

 

Using composer:

composer config repositories.magento-baler vcs git@github.com:magento/m2-baler.git

composer require magento/module-baler:dev-master@dev

composer install

 

bin/magento module:enable Magento_Baler

bin/magento setup:upgrade


 

Set Magento configuration
Disable all core JavaScript options, and enable Baler Bundling. Again, remove –lock-config if you want it set in database instead of app/etc/config.php


bin/magento config:set dev/js/enable_baler_js_bundling 1 --lock-config

bin/magento config:set dev/js/enable_js_bundling 0 --lock-config

bin/magento config:set dev/js/merge_files 0 --lock-config

bin/magento config:set dev/js/minify_files 0 --lock-config


 

Compile your themes
I usually run setup:upgrade to remove all compiled assets, and then compile DI and static content:


bin/magento setup:upgrade

bin/magento setup:di:compile

bin/magento setup:static-content:deploy en_US


By the way, did you know there is an excellent feature of Symfony Console, on which the Magento console is based, that allows you to abbreviate those commands?

This one liner performs all three commands above:

bin/magento set:up && bin/magento set:di:co && bin/magento set:sta:dep en_US

 

Run Baler
From the root of your project, run:

baler build

If you get stuck here, please see the troubleshooting section below.


Fix is to run:

npm run build  
To test once this is installed:

~/baler/bin$ ./baler --help
Usage  
  $ baler <command> [options]

  Commands
    build --theme Vendor/name
    graph --theme Vendor/name

  Examples
    Optimize all eligible themes
    $ baler build

    Optimize multiple themes
    $ baler build --theme Magento/foo --theme Magento/bar

    Generate Dependency Graph
    $ baler graph --theme Magento/luma
Next jump to the Magento 2 docroot and run it:

:~/public_html$ /srv/baler/bin/baler 
✔ Collected theme/module data 155ms
✔ (Magento/luma) Created core bundle file 1.2s
✔ (Magento/luma) Minified core bundle and RequireJS 17.6s

Optimization is done, but stats have not been implemented in the CLI yet  
Clear Magento and Redis (if installed) cache:

~/public_html$ n98-magerun2 cache:clean
config cache cleaned  
layout cache cleaned  
block_html cache cleaned  
collections cache cleaned  
reflection cache cleaned  
db_ddl cache cleaned  
compiled_config cache cleaned  
eav cache cleaned  
customer_notification cache cleaned  
config_integration cache cleaned  
config_integration_api cache cleaned  
google_product cache cleaned  
full_page cache cleaned  
config_webservice cache cleaned  
translate cache cleaned  
vertex cache cleaned  
~/public_html$ n98-magerun2 cache:flush
redis-cli flushconfig cache flushed  
layout cache flushed  
block_html cache flushed  
collections cache flushed  
reflection cache flushed  
db_ddl cache flushed  
compiled_config cache flushed  
eav cache flushed  
customer_notification cache flushed  
config_integration cache flushed  
config_integration_api cache flushed  
google_product cache flushed  
full_page cache flushed  
config_webservice cache flushed  
translate cache flushed  
vertex cache flushed  
~/public_html$ redis-cli flushall
OK  
(this one is optional if Redis used)
Next, we need to install Magento 2 module which is going to handle bundlings:

composer config repositories.baler vcs https://github.com/adifucan/m2-baler  
Then run:

composer require magento/module-baler:dev-master  
Activate module:

n98-magerun2 module:enable Magento_Baler  
Run deployment process (setup:upgrade ; setup:di:compile and setup:static-content:deploy)

n98-magerun2 setup:upgrade  
n98-magerun2 setup:di:compile  
n98-magerun2 setup:static-content:deploy  
Clear Magento/Redis cache like we did it upper and then push the following:

php bin/magento config:set dev/js/enable_baler_js_bundling 1  
Run the baler once again:

$ /srv/baler/bin/baler build 
✔ Collected theme/module data 156ms
✔ (Magento/luma) Created core bundle file 1.2s
✔ (Magento/luma) Minified core bundle and RequireJS 17.8s

Optimization is done, but stats have not been implemented in the CLI yet  
Then we need to disable Magento 2 built in JavaScript Minify/Merge/Bundling:

php bin/magento config:set dev/js/minify_files 0  
php bin/magento config:set dev/js/enable_js_bundling 0  
php bin/magento config:set dev/js/merge_files 0  
 