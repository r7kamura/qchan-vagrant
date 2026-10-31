# Qchan Vagrant

> [!NOTE]
> This project is archived because the external service it integrates with is no longer available, so I took that as an opportunity to archive it. If you would like to take over maintenance, please contact me somewhere.

Vagrantfile for the Qchan cluster.

## Usage
```sh
git clone git@github.com:r7kamura/qchan-vagrant.git
cd qchan-vagrant
bundle install
bundle exec vagrant up
bundle exec vagrant ssh -c "cd qchan-vagrant && foreman start"
```
