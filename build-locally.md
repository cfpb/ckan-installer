## Installing Locally

#### First Vagrant Up
```shell
git clone https://github.com/cfpb/ckan-installer.git
git clone https://github.com/cfpb/ckanext-cfpb-extrafields
cd ckan-installer
vagrant up
```

#### Add the cfpb-extra-fields plug-in
```shell
<edit vagrant file: uncomment synced directory>
vagrant reload
vagrant ssh                     # shell in
 . /ckan/default/bin/activate   # start a ckanext virtualenv
cd /ckan/default/src/ckanext-cfpb-extrafields
python setup.py develop         # if you don't do setup, you'll get PluginNotFoundException
exit                            # leave your virtual box
vagrant reload                  # your plugin should be synced between virtual box and the directory in the vagrant file
```

#### Make yourself an admin
```shell
vagrant ssh
. /ckan/default/bin/activate
paster --plugin=ckan sysadmin add <username> -c /ckan/config/default/config.ini
```