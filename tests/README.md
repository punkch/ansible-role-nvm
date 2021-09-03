# Ansible Role tests

## Docker - requirements
I want to create secure base docker images even for testing. To ensure I do, I use (Trivy)[https://github.com/aquasecurity/trivy] which will scan the image for issues.

`trivy image ubuntu-bionic:ansible`


## Docker - Build
`docker build --no-cache --rm --file=tests/Dockerfile.ubuntu-bionic --tag ansible-role-nvm:1.0 ./tests`


## Docker - Run

`docker run --tty --detach --name nvm ansible-role-nvm:1.0`
`docker exec -it nvm /bin/bash`


## Docker - Clean up
`docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)`


## Using Docker as a VM to test Ansible provisioning
Run the docker container
`docker run --tty --detach --hostname <HOSTNAME> --name <CONTAINER_NAME> <IMAGE>`

Edit the inventory file accordingly,

```
[<HOSNAME>]
<CONTAINER_NAME> ansible_connection=docker ansible_python_interpreter=/usr/bin/python3
```

Run the playbook as usual `ansible-playbook playbook.yml -i inventory --become --become-user "ansible-test" -vvv`
