## Installing Locally

#### First Vagrant Up
```shell
host> git clone https://github.com/cfpb/ckan-installer.git
host> git clone https://github.com/cfpb/ckanext-cfpb-extrafields
host> cd ckan-installer
host> vagrant up
```

#### Add the cfpb-extra-fields plug-in
```shell
<edit vagrant file: uncomment synced directory>
host> vagrant reload
host> vagrant ssh                      # shell in
guest>  . /ckan/default/bin/activate   # start a ckanext virtualenv
guest> cd /ckan/default/src/ckanext-cfpb-extrafields
guest> python setup.py develop         # if you don't do setup, you'll get PluginNotFoundException
guest> exit                            # leave your virtual box
host> vagrant reload                   # your plugin should be synced between virtual box and the directory in the vagrant file
```

#### Make yourself an admin
```shell
host> vagrant ssh
guest> . /ckan/default/bin/activate
guest> paster --plugin=ckan sysadmin add <username> -c /ckan/config/default/config.ini
```

#### Developing server-side code
```shell
host> vagrant ssh
guest> sudo service httpd stop         # Make sure the default http server is not running (prevents confusion)
guest> . /ckan/default/bin/activate    # start a virtual environment
guest> paster serve --reload /ckan/config/default/config.ini
... edit/test/edit ...
guest> exit
```
