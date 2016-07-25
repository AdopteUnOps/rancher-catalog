# rancher-catalog

This repository acts as a rancher catalog.

## Create a new catalog entry

You need to create a catalog structure like this :
```
templates/
└── test
    ├── 0
    │   ├── docker-compose.yml
    │   └── rancher-compose.yml
    ├── catalogIcon-test.svg
    ├── config.yml
    └── README.md
```

The first level under the templates folder is the name of your catalog entry it should contains a config.yml with a name and description.
Inside this directory you should have a subfolder (usually number starting at 0) with your docker-compose.yml and rancher-compose.yml.

Instead of creating this structure manually you could use a yeoman generator (see https://github.com/slashgear/generator-rancher-catalog)

See official documentation to get more details http://docs.rancher.com/rancher/latest/en/catalog/

## Create a new version of a catalog entry

You just need to create a new version in templates/YOURSTACK/VERSION.

