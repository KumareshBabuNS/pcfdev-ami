
$script = <<SCRIPT
echo Starting PCF DEVf......
cmd="/var/pcfdev/provision $DOMAIN $(ip route get 1 | awk '{print $NF;exit}') redis,rabbitmq '' aws"
echo $cmd
exec $cmd
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "dummy"

  config.vm.provider :aws do |aws, override|
    aws.access_key_id = ENV['AWS_KEY']
    aws.secret_access_key = ENV['AWS_SECRET']
    aws.keypair_name = ENV['KEY_PAIR']
    aws.security_groups = ENV['SECURITY_GROUPS']
    aws.subnet_id = ENV['SUBNET_ID']
    aws.associate_public_ip = true
    aws.instance_type = "c4.2xlarge"
    aws.elastic_ip = ENV["PUBLIC_IP"]
    aws.block_device_mapping = [{ 'DeviceName' => '/dev/sda1', 'Ebs.VolumeSize' => 100 , 'Ebs.VolumeType' => 'io1', 'Ebs.Iops' => '1800'}]
    aws.tags = {
      'Name' => ENV['VM_TAG']
    }

    aws.ami = "ami-edb27ffb"
    override.ssh.username = "ubuntu"
    override.ssh.private_key_path = ENV['PRIAVE_KEY_PATH']
  end
  config.vm.provision "shell", inline: $script,
    env: {"DOMAIN" => ENV['DOMAIN'], "SERVICES" => ENV['SERVICES']},
    run: "always"
end
