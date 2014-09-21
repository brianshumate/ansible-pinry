## Ansible Role for Pinry

                        _
                    ,.-" "-.,
                   /   ===   \
                  /  =======  \
               __|  (o)   (0)  |__
              / _|    .---.    |_ \
             | /.----/ O O \----.\ |
              \/     |     |     \/
              |                   |
              |                   |
              |                   |
              _\   -.,_____,.-   /_
          ,.-"  "-.,_________,.-"  "-.,
         /          |       |          \
        |           l.     .l           |
        |            |     |            |
        l.           |     |           .l
         |           l.   .l           | \,
         l.           |   |           .l   \,
          |           |   |           |      \,
          l.          |   |          .l        |
           |          |   |          |         |
           |          |---|          |         |
           |          |   |          |         |
           /"-.,__,.-"\   /"-.,__,.-"\"-.,_,.-"\
          |            \ /            |         |
          |             |             |         |
           \__|__|__|__/ \__|__|__|__/ \_|__|__/ Sandra


This repository contains an [Ansible](https://github.com/ansible/ansible)
playbook collection, a set of configuration files, tasks, and other example
bits for bootstrapping a Linux host to serve
[Pinry](https://github.com/pinry/pinry.git), a Django based web application.

With some configuration changes, the project can be used as the basis of
custom playbooks for Pinry deployment and maintenance.

The project provides both a development environment for Mac OS X with
VirtualBox and Vagrant, and a production environment on Linux with Amazon
EC2.

## Development on Mac OS X

To operate a development instance of Pinry on Mac OS X with VirtualBox and
Vagrant, follow these steps:

1. Install VirtualBox
2. Install Vagrant
3. Install Ansible
4. Paste a SSH public key into `roles/common/files/pinry_id_rsa.pub`
5. Open a terminal and run `vagrant up`

When the playbooks finish, you can log into the virtual machine and start
the app:

```
vagrant ssh
sudo su - pinry
cd pinry/pinry
workon pinry
python manage.py syncdb
python manage.py migrate
python manage.py runserver 0.0.0.0:8000
```

## Production on Linux

Edit hosts in the `production` inventory, make sure to change
`app_settings` variable in `group_vars/all` and then run:

```
ansible-playbook -i production site.yml`
```

### Launching The Application

After running through the full playbook, you can log into the machine and do
the following steps to start the application in *development* mode:

```bash
cd pinry
workon pinry
python manage.py syncdb
python manage.py migrate
python manage.py runserver 0.0.0.0:8000
```

Ansible's `django_manage` module is useful and offers great convenience
features, like kickstarting an app using *syncdb* and *migrate* commands
if more automation in the deployment is necessary.

Provided all of the above goes well, you should be able to access the
development version of our application here:

http://example.com:8000/

Replace *example.com* in the above URL with the domain name for the host
that you are using.

### Deployment

This section is TBD and will cover:

* Inventory host selection for production
* Configuration and use of uwsgi
* Configuration and use of nginx
* Production DB configuration
* Miscellaneous production configuration and permissions

After adjusting hosts in the inventory, you can bootstrap a Pinry
installation with an `ansible-playbook` command like this one:

```
ansible-playbook -i production site.yml
```

If you only need to execute certain tagged tasks, such as to install all
packaged software, you can use the `--tags` option like this:

```
ansible-playbook -i production site.yml --tags app
```

Similarly, you could execute only user related tasks:

```
ansible-playbook -i production site.yml --tags user
```

See the Ansible Advanced Playbooks [tags documentation section](http://www.ansibleworks.com/docs/playbooks2.html#tags) for more details on how tags work within playbooks.

## Resources

1. [Ansible GitHub](https://github.com/ansible/ansible)
2. [AnsibleWorks Documentation](http://www.ansibleworks.com/docs/)
3. [Best Practices](http://www.ansibleworks.com/docs/bestpractices.html)
4. [Ansible Modules](http://www.ansibleworks.com/docs/modules.html)

