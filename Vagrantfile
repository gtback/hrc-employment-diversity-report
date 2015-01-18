VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.ssh.forward_agent = true
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  #config.vm.network "forwarded_port", guest: 8888, host: 8888
  #config.vm.network "forwarded_port", guest: 9200, host: 9200
  #config.vm.network "forwarded_port", guest: 8787, host: 8787
  config.vm.network "forwarded_port", guest: 5000, host: 5000
  config.vm.provision :ansible do |ansible|
    ansible.playbook = ".ansible/playbook.yml"

  end

  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 1
  end

  config.vm.provider :aws do |aws, override|
    aws.access_key_id = ENV['AWS_ACCESS_ID']
    aws.secret_access_key = ENV['AWS_SECRET_KEY']

    # Be careful here.  DeviceName can definitely vary between AMIs.  This works for ubuntu 12.04 lts
    aws.block_device_mapping = [
        {:DeviceName => "/dev/sda1",
          :VirtualName => "ebs",
          'Ebs.VolumeSize' => 8,
          'Ebs.DeleteOnTermination' => true }
    ]
    # Also can be changed
    aws.ami = ENV['AMI']

    # Obviously this can be changed
    aws.instance_type = ENV['INSTANCE']

    # Change to your desired keypair
    aws.keypair_name = ENV['KEY_PAIR_NAME']

    # Security group required port 22 open for SSH connections.
    aws.security_groups = ENV["SECURITY"]

    # Change if aws account is not in this region...
    aws.region = ENV['REGION']
    aws.tags = { Name: "aws" }

    # The dummy box for this appears to be required if you are not launching from your own box built from an AMI.
    override.vm.box = "ubuntu_aws"
    override.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
    override.ssh.username = ENV["SSH_USER" ]
    override.ssh.forward_agent = true
    override.ssh.pty = true
    override.ssh.private_key_path = ENV['SSH_KEY_PAIR']

  end
end