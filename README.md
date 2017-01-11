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

1. `cd` into this repo and run `vagrant up`
2. Create SSH tunnels to the PostgreSQL/Solr instances: `vagrant ssh -- -L 8983:localhost:8983 -L 5432:localhost:5432`
3. From the aurora repo, open an ssh tunnel to marathon: `vagrant ssh mesos_master_1 -- -L 8000:localhost:8080`
4. Within the ssh tunnel, find your gateway IP by running `netstat -nr`
5. Visit [localhost:8000](http://localhost:8000) to access the marathon UI and make a new app
6. Edit the app's config, click the "json" toggle in the upper right, and paste the json from `marathon_app.json` (located in the same directory as this readme). Update any instances of `10.0.2.2` with the gateway IP you noted in step 4.
7. Upon saving the config, you'll be prompted to (re)deploy the application. Do so and give it time to install (~10 minutes?)
8. Download the stdout/stderr files in marathon and briefly scan them for errors
9. Within those stdout/stderr, note the long directory that everything is being put into (`MESOS_SANDBOX`). Try `vagrant ssh mesos_agent_1 -- -L 8888:localhost:8888` and within that machine `ls <path/to/sandbox>`. If the file does not exist, kill the ssh and retry with `mesos_agent_2` and `mesos_agent_3` until you've found the agent that is running the application.
10. Try visiting [localhost:8888](http://localhost:8888) and ensure that you can see the Data Catalog page.
11. If you encounter an apache error page, go to your ssh session with the mesos agent and look for details in the logs at `/path/to/sandbox/httpd`

## Getting Started
For information about how to use CKAN, please see the [official documentation](http://docs.ckan.org/en/latest/user-guide.html)

**Customizing Look and Feel**  
See the section on [Customizing Look and Feel](http://docs.ckan.org/en/latest/sysadmin-guide.html#customizing-look-and-feel) in the official documentation

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

