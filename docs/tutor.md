
# DROP Tutorials

## Instalation

### Vagrant 

#### Build VM on host

1. Install Vagrant
2. Install VirtualBox

# 
    curl -O https://raw.githubusercontent.com/droopp/vagrant/master/Vagrantfile .
    vagrant up
    vagrant status

### On Premis

1. Connect to host and run 
#
    curl https://dropfaas.com/RPMS/drop-bootstrap.sh | sudo sh



### AWS

#### TODO


## Configure DROP cluster namespace

.1. Connect to any node (ssh) or install drop-cli local 

    drop-cli <ANY NODE HOST>

.2. Run drop-cli and login (admin/admin123)

#
    drop-core:127.0.0.1:nologin> login admin
.3. View all avaliable nodes freenode_list
#
    drop-core:127.0.0.1:admin> freenode_list


.4. Create namespace with name (example main)
#
    drop-core:127.0.0.1:admin> cluster_create main --node-ids * --vip 192.168.50.10

    --node-ids - number uid (ex 1,2,3 ) or * - all freenodes
    --vip - floating/ virtual ip address 

.5. View cluster namespace info
# 
    drop-core:127.0.0.1:admin> cluster_info main

and all avaliable clusters namespaces 

    drop-core:127.0.0.1:admin> cluster_list 
 
.6. Install avaliable plugin

    //Virew all plugins in repo 
  	drop-core:127.0.0.1:admin> plugin_repo

    drop-core:127.0.0.1:admin> plugin_install main --id drop-plgn-cmd-exec
    drop-core:127.0.0.1:admin> plugin_install main --id drop-plgn-rrd // Optional
	drop-core:127.0.0.1:admin> plugin_install main --id drop-plgn-webbone //Optional

	
.7. View all installed plugin and check version nodes

    drop-core:127.0.0.1:admin> plugin_list main
#

    drop-core:127.0.0.1:admin> cluster_version main

.8. Configure services 

    service_conf <namespace> --id <systemd.service> 
    --code 
        0 - not start service
        1 - start on node with vip
        N - start on N nodes

Haproxy run on node with vip 

    drop-core:127.0.0.1:admin> service_conf main --id haproxy --code 1


Web admin ui run on node with vip

    drop-core:127.0.0.1:admin> service_conf main --id webbone --code 1


.9. Open you browser on vip adress http://192.168.50.10:8084 (admin/admin123)


## Create function (any lang / use stdin/stdout/stderr)

### Python 

    #
    # Python func example
    #


    import sys
    import time

    # API


    def read():
        msg = sys.stdin.readline()
        return msg.strip()


    def log(m):
        sys.stderr.write("{}: {}\n".format(time.time(), m))
        sys.stderr.flush()


    def send(m):
        sys.stdout.write("{}\n".format(m))
        sys.stdout.flush()


    # Process - actor
    #  read - recieve message from world
    #  send - send message to world
    #  log  -  logging anything


    def main(t):
        while 1:
          msg = read()
          if not msg:
              break
    
          log("start working..")
          log("get message: " + msg)
    
          resp = "{}".format(msg)
    
          send(resp)
    
          log("message send: {}".format(resp))
    
    
    if __name__ == "__main__":
      main(sys.argv[1])
 

## Create flow

### Func declaration

    # func declare example
    name: pyex
    func:
        - name: pyex
        image: pyex:0.1.0
        cmd: /usr/bin/python pyex.py 1



### Publish function on DROP

.1. Create image build and publish to registry
# 
    FROM ubuntu:16.04

    RUN apt-get update && apt-get install -y python
    RUN useradd drop

    COPY ./pyex.py /home/drop/

.2. Run drop-cli and install function

    drop-core:127.0.0.1:admin> fun_install main --id pyex --ver 0.1.0 --space droopp

    --id image name
    --ver image version
    --space repository name 


.3. Run drop-cli and install flow
#
    drop-core:127.0.0.1:admin> flow_install main --id https://raw.githubusercontent.com/droopp/drop-fun-examples/master/python/pyex.yaml

    --id local or remote YAML config location
