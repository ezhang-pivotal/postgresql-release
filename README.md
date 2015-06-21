安装RVM和Ruby1.9
curl -L get.rvm.io | bash -s stable
rvm list known，选择1.9.3的最新版
rvm install ruby-1.9.3-p551

安装bosh-gen for vsphere
$ git clone git://github.com/BrianMMcClain/bosh-gen.git -b vsphere_manifest
$ cd bosh-gen
$ bundle install
$ rake build
$ gem install pkg/bosh-gen-0.6.1.gem
创建发行版项目
bosh-gen new postgresql-release
cd postgresql-release
$ bosh-gen new postgresql-release
$ cd postgresql-release

创建包
$ bosh-gen package postgresql
$ bosh-gen source redis http://redis.googlecode.com/files/redis-2.4.13.tar.gz

创建作业
$ bosh-gen job postgresql -d postgresql
$ bosh-gen template postgresql config/postgresql.conf
手动修改job文件，暂时为单实例，见jobs/postgresql/templates/postgresql_ctl

生成vsphere的部署文件
$ bosh create release
$ bosh upload release
$ bosh-gen manifest postgresql-release . f303fefa-1701-4352-b1b4-ceb316e950ac -c vsphere -a 172.20.69.161 -r 172.20.69.0/24 -g 172.20.69.254 --dns 172.20.69.12 -w 1 -n "VM Network"
$ bosh deployment postgresqldev001.yml
$ bosh deploy
