# spark-vagrant
This Vagrantfile creates a VM containing OpenJDK, Scala and Apache Spark for experimentation and PoCs.

You need to execute the following commands in a terminal to get started.
Please ensure you change into your favorite directory before executing these
commands.

`$ cd ~/my_favorite_directory`

`$ git clone git@github.com:nabheet/spark-vagrant.git`

`$ cd spark-vagrant`

`$ vagrant up`

`$ vagrant ssh`

Now you are in the `/vagrant` directory inside the VM. Please keep in mind that
the `/vagrant` directory inside the VM is mapped to the `spark-vagrant`
directory on your computer/host. So all files on your host directory will be
accessible in this directory. This is an easy way to access/transfer files
between the host and the VM.

You can now start the `spark-shell` or execute `scalac` or `sbt` and you can
follow any spark tutorial.

If you need to rebuild your VM to get the new installs or settings or need a
new clean VM for any reason:

`$ vagrant destroy -f && vagrant up`
.
