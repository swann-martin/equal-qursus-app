# Qursus
[![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0)
[![Maintainer](https://img.shields.io/badge/maintainer-yesbabylon-blue)](https://github.com/yesbabylon)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](https://github.com/cedricfrancoys/equal/pulls)
<!-- ![eQual - Create great Apps, your way!](https://github.com/equalframework/equal/blob/master/public/assets/img/equal_logo.png?raw=true) -->

<a href="https://github.com/equalframework/" style="font-size:2rem;display:flex; align-items:center; wrap; flex-wrap: wrap; flex-shrink:0; text-decoration:none; color:inherit">Made with eQual framework <img itemprop="image" style="margin-left:10px; border-radius:10px" src="https://avatars.githubusercontent.com/u/111111764?s=200&amp;v=4" width="46px" height="46px" alt="@equalframework"/>
</a>

![qursus](./assets/images/qursus.png)

## Why Qursus :

We needed a way to present people about how to use eQual so we created this app. Ultimately, you will be able to learn how to use Qursus, how to use eQual framework by following our module Qursus.

## How do we propose to solve the problem by using Qursus

Qursus uses eQual framework in back-end to connect to modules of lessons. There are two views. A user can view a lesson or if the person has the rights there is an edition mode. Every module can be created and edited in the browser.

## How does Qursus work ?

**UML Diagram of the application**

![uml](./assets/images/qursus-uml.drawio.png)

**Schema of the application**

![qursus-page-schemma](./assets/images/qursus-page-schema.png)


## Install Qursus

Prerequisite : To install Qursus you need to have eQual installed. Go to the eQual documentation at

Then navigate to your eQual in your docker or server instance.
Go to the branch dev-2.0 and init the package qursus.


```bash
cd /var/www/html/
git clone https://github.com/yesbabylon/symbiose.git packages
cd /var/www/html/packages
git checkout dev-2.0
git pull
cd /var/www/html/
./equal.run --do=init_package --package=qursus
```


Now in your qursus package you should see :

```
/packages
    /qursus
        /actions
             import.php --> Create Modules, Chapters, Leaves, Pages, Groups and Widgets based on ATModule.json file.
             next.php --> Handle action from user when performing a click to see next page of a given module.
             survey.php --> Send an invite to satisfaction survey.
        /apps
            /qursus --> front-end in TypeScript and jQuery web.app that will be unpacked in /public/qursus on init
                export.sh -->
                web.app --> zip of the front-end build
                manifest.json
        /classes
            Bundle.class.php
            BundleAttachment.class.php
            Chapter.class.php
            Group.class.php
            Lang.class.php
            Leaf.class.php
            Module.class.php
            Pack.class.php
            Page.class.php
            Quiz.class.php
            Section.class.php
            UserAccess.class.php
            UserStatus.class.php
            Widget.class.php
        /data
            /module
                render.php  --> Returns a fully loaded JSON formatted single module
            /pack
                access.php --> Checks if current user has a license for a given program.
                certificate.php --> Returns a html page or a signed pdf certificate.
                complete.php --> Checks if a pack has been fully completed by current user.
                grant.php --> Checks if current user has a license for a given program.
            bundle.php   --> Sends either a single attachment or a zip archive containing all attachments.
            module.php  --> Returns a fully loaded JSON formatted single module.
            modules.php  --> "Returns a list of all modules for a given pack, enriched with current user status.
        /init
            /data
        /views
            Bundle.list.default.json
            Bundle.form.default.json
            ...
            Widget.form.create.default.json
            Widget.form.default.json
            Widget.list.default.json
        config.inc.php --> specify the DEFAULT_PACKAGE constant used for routing
        manifest.json --> usual eQual package manifest
```



## Pack

A pack

### Pack class
```php
 [
            'name' => [
                'type'              => 'string',
                'description'       => 'Unique slug of the program.'
            ],

            'title' => [
                'type'              => 'string',
                'multilang'         => true
            ],

            'subtitle' => [
                'type'              => 'string',
                'multilang'         => true
            ],

            'description' => [
                'type'              => 'string',
                'usage'             => 'text/plain',
                'multilang'         => true
            ],

            'modules' => [
                'type'              => 'alias',
                'alias'             => 'modules_ids'
            ],

            'modules_ids' => [
                'type'              => 'one2many',
                'foreign_object'    => 'qursus\Module',
                'foreign_field'     => 'pack_id',
                'ondetach'          => 'delete'
            ],

            'quizzes_ids' => [
                'type'              => 'one2many',
                'foreign_object'    => 'qursus\Quiz',
                'foreign_field'     => 'pack_id',
                'ondetach'          => 'delete'
            ],

            'bundles_ids' => [
                'type'              => 'one2many',
                'foreign_object'    => 'qursus\Bundle',
                'foreign_field'     => 'pack_id',
                'ondetach'          => 'delete'
            ],

            'langs_ids' => [
                'type'              => 'many2many',
                'foreign_object'    => 'qursus\Lang',
                'foreign_field'     => 'packs_ids',
                'rel_table'         => 'qursus_rel_lang_pack',
                'rel_foreign_key'   => 'lang_id',
                'rel_local_key'     => 'pack_id',
                'description'       => "List of languages in which the program is available"
            ]

        ];
}
```

```typescript
class PackClass {
    public id: number;
    public name: string;               // (ex. PDT, AT)
    public subtitle: string;           // (ex. Duration: 15 minutes)
    public title: string;              // (ex. Program Development Training, Awareness Training)
    public description: string;

```





















**View of a page components**
![qursus-page](./assets/images/qursus-page.png)



























Check the typescript syntax (lint):

`yarn run tsc`

Use babel to transpile .ts file into .js :

`npm run build`

Generate an app.bundle.js that can be embedded to any .html file:

`npm run webpack`