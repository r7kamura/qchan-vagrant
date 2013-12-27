# Qchan Vagrant
Vagrantfile for the Qchan cluster.

## Usage
```sh
git clone git@github.com:r7kamura/qchan-vagrant.git
cd qchan-vagrant
bundle install
bundle exec vagrant up
bundle exec vagrant ssh -c "cd qchan-vagrant && foreman start"
```
