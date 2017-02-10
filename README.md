# CKAN

**Description**:  A simple vagrant box with provisioning scripts to stand up an out-of-the-box version of CKAN.  

Other things to include:

  - **Technology stack**: 
   - Python 2.7
   - Apache
   - Postgres
   - Solr
   - CKAN
  - **Status**:  1.0  Should be stable but we are still making refinements and looking for feedback

## Dependencies
- **Vagrant / Virtualbox** - creates the VM that is built by Ansible.  
Installation instructions can be found here: http://docs.vagrantup.com/v2/installation/
- **Ansible** - provisions the VM.  
    Installation: `pip install ansible` from your python virtual environment

## Installation

1. `vagrant up`
2. Once provisioning completes, open your browser to [localhost:8080](http://localhost:8080) and you should see CKAN

### Starting / Stopping the VM
To start the vagrant box: `vagrant up`  
To stop the vagrant box: `vagrant halt`  

## Installing on Aurora
If you would like to install ckan onto a locally running instance of [Aurora](https://github.com/cfpb/aurora), follow the steps below.
Since each Aurora app consists of a single ephemeral foreground process, this technique relies on installing CKAN the traditional way first and using its instance of PostgreSQL/Solr with an Apache process running on Mesos via Marathon.

1. `cd` into the `provisioners/` folder in this repo and run `vagrant up`
2. Create SSH tunnels to the PostgreSQL/Solr instances: `vagrant ssh -- -L 8983:localhost:8983 -L 5432:localhost:5432`
3. Visit [10.0.1.31:8080](http://10.0.1.31:8080) to access the marathon UI and make a new app
4. Edit the app's config, click the "json" toggle in the upper right, and paste the json from `marathon_app.json` (located in the same directory as this readme).
5. Upon saving the config, you'll be prompted to (re)deploy the application. Do so and give it time to install (~10 minutes?)
6. Open the Mesos UI at [10.0.1.31:5050](http://10.0.1.31:5050). Locate your job under the "Active Tasks" section and click its "Sandbox" link.
7. Click the "stdout" link and confirm that the bottom says `PLAY RECAP` and that you see `failed=0`. If there is no play recap yet, you probably need to wait for ansible to finish running
8. Go back to the [Marathon UI](http://10.0.1.31:8080) and locate your task. Click the link undereath the ID (something like '10.0.1.34:31029').
9. This should open the Data Catalog in a new tab. If you receive an error page, go back to the sandbox and check `stdout`, `stderr` and `httpd/apache.error.log` for error messages.

## Getting Started
For information about how to use CKAN, please see the [official documentation](http://docs.ckan.org/en/latest/user-guide.html)

**Customizing Look and Feel**  
See the section on [Customizing Look and Feel](http://docs.ckan.org/en/latest/sysadmin-guide.html#customizing-look-and-feel) in the official documentation

## Useful commands
The ckan docs contain several commands that are useful for administering the ckan instance. Some of them are listed here for convenience:

Make a user an admin:

    source /ckan/default/bin/activate && cd /ckan/default/src/ckan && paster sysadmin add <username> -c /ckan/config/default/config.ini

Re-index solr  (required after upgrade)

    source /ckan/default/bin/activate && cd /ckan/default/src/ckan && paster search-index rebuild -r --config=/ckan/config/default/config.ini

## Upgrading CKAN
If you want to upgrade the version of ckan, follow the instructions in the [CKAN Docs](http://docs.ckan.org/en/latest/maintaining/upgrading/upgrade-source.html).

Note that you must upgrade one minor version at a time (you can't go straight from 2.3 to 2.5, but instead must do `2.3 -> 2.4 -> 2.5`).

Furthermore, there is a bug in version 2.5.0, so the actual list of tags should be: `ckan-2.3` -> `ckan-2.4.0` -> `ckan-2.5.3` -> `ckan-2.6.0`.

## Developers

Check out the [Guide to Extending](http://docs.ckan.org/en/latest/extensions/tutorial.html) in the offical documentation

If you are building with the extra fields, check out the [local developement guide](build-locally.md)

### Customizing this VM  
If you make any changes to the `provisioners/group_vars/local` or any of the default variables, you will need to re-provision the VM.  Simply run `vagrant provision` and the changes will be applied.  

## How to test the software

At the moment there are no tests for this codebase.  

## Known issues

None

## Getting help

If you have questions, concerns, bug reports, etc, please file an issue in this repository's Issue Tracker.

## Getting involved

Please see our [CONTRIBUTING](CONTRIBUTING.md) file for more information about contributing

## Troubleshooting  

### Solr 
If you can't seem to access solr in Vagrant (checking http://localhost:8983/solr), try stopping the iptables service:
`sudo service iptables stop`

----

## Open source licensing info
1. [TERMS](TERMS.md)
2. [LICENSE](LICENSE)
3. [CFPB Source Code Policy](https://github.com/cfpb/source-code-policy/)


----

