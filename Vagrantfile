# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "bento/centos-7.2"

  config.ssh.insert_key = false

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  vm_name = "spark"
  config.vm.hostname = vm_name

  config.vm.provider :virtualbox do |vb|
    vb.name = vm_name
    # Customize the amount of memory on the VM:
    vb.memory = "2048"
  end

  config.vm.provider :parallels do |p|
    p.name = vm_name
    # Customize the amount of memory on the VM:
    p.memory = "2048"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    scala_version="2.11.8"
    spark_version="2.0.1"
    hadoop_version="2.7"
    sbt_launch_version="0.13.12"
    wget "http://downloads.lightbend.com/scala/${scala_version}/scala-${scala_version}.tgz" "http://d3kbcqa49mib13.cloudfront.net/spark-${spark_version}-bin-hadoop${hadoop_version}.tgz" "https://repo.typesafe.com/typesafe/ivy-releases/org.scala-sbt/sbt-launch/${sbt_launch_version}/sbt-launch.jar"
    yum install -y java
    tar xvf scala-${scala_version}.tgz && rm scala-${scala_version}.tgz
    tar xvf spark-${spark_version}-bin-hadoop${hadoop_version}.tgz && rm spark-${spark_version}-bin-hadoop${hadoop_version}.tgz
    mkdir -pv /usr/local/scala /usr/local/spark /usr/local/sbt-launcher
    mv -v scala-${scala_version} /usr/local/scala
    mv -v spark-${spark_version}-bin-hadoop${hadoop_version} /usr/local/spark
    mv -v sbt-launch.jar /usr/local/sbt-launcher
    cat > /usr/bin/sbt << EOF
#!/bin/bash
SBT_OPTS="-Xms512M -Xmx1536M -Xss1M -XX:+CMSClassUnloadingEnabled -XX:MaxPermSize=256M"
java $SBT_OPTS -jar /usr/local/sbt-launcher/sbt-launch.jar "\\$@"
EOF
    chmod -v 755 /usr/bin/sbt
    echo "Downloading sbt dependencies - this may take a while ... be patient ..."
    sudo -H -u vagrant bash -c 'sbt'
    echo "Setting environment variables"
    echo "export PATH=$PATH:/usr/local/scala/scala-${scala_version}/bin:/usr/local/spark/spark-${spark_version}-bin-hadoop${hadoop_version}/bin" >> /home/vagrant/.bashrc
    echo "cd /vagrant"  >> /home/vagrant/.bashrc
  SHELL
end
