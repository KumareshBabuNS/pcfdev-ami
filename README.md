# PCF DEV AWS image

Image built by packer from: https://github.com/pivotal-cf/pcfdev

**Note**: The AMI image is private and only available for PCF Services AWS account

**Note**: This AMI does not include apps manager and spring cloud service... since those are proprietary for pivotal

**Note**: Check the bosh releases used by this version: [versions.json](https://github.com/datianshi/pcfdev/blob/master/versions.json)

## Why

* Quickly start a single VM Cloud Foundry environment available on the internet
* Economic
* No need to consume local laptop resources
* Apps survive after AWS EC2 stop/start

## How

1. Pick up an elastic IP and a **domain**

2. Create a wild card record to point the \*.domain -> elastic ip

3. git clone https://github.com/datianshi/pcfdev-ami.git & cd pcfdev-ami

4. Create one env.sh with necessary environment variables

    ```
    export AWS_KEY=
    export AWS_SECRET=
    export KEY_PAIR= #AWS key pair name
    export SUBNET_ID=subnet-900457ba # A public subnet (with internet gateway attached)
    export PRIAVE_KEY_PATH=XXX.pem
    export PUBLIC_IP=${elastic_ip}
    export SERVICES="redis,rabbitmq"
    export DOMAIN="pcfdev.shaozhenpcf.com"
    export SECURITY_GROUPS=sg-7437d508 #With 443 and 22 open
    export VM_TAG="pcfdev-aws-sding"
    ```
5. source env.sh

6. start VM

  ```
  vagrant plugin install vagrant-aws
  vagrant box add dummy https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box
  vagrant up
  ```


7. A cloud foundry environment with redis and rabbitmq services stood up

    ```
    cf api api.pcfdev.shaozhenpcf.com --skip-ssl-validation
    cf login (username and password are both admin)
    ```

## Stop/Start VM

  ```
  stop: vagrant halt
  start: vagrant up
  ```
