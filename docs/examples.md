



# Examples of DROP functions

### 1. Create function 

### Python func example


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



### 2. Build docker image and push to registry

### Dockerfile


    FROM ubuntu:16.04
    RUN apt-get update && apt-get install -y python
    RUN useradd drop
    COPY ./pyex.py /home/drop/



### 3. Declare function

### pyex.yaml

    name: pyex
    func:
        - name: pyex
        image: pyex:0.1.0
        cmd: /usr/bin/python pyex.py 1


