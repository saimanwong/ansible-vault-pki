SHELL=/bin/bash

dockerfile = Dockerfile.centos
image_name = centos7:systemd
container_prefix = lab

containers = ${container_prefix}-root ${container_prefix}-intermediate ${container_prefix}-issuing

ssh_pub_key = ${HOME}/.ssh/id_rsa.pub

all: build run setup

build:
	docker build -t ${image_name} -f ${dockerfile} .

run: $(containers)
$(containers):
	docker run -d --privileged \
		-l ${container_prefix}-pki \
		-p 2222-2224:22 \
		--name $@ \
		${image_name}

setup:
	for i in $$(docker ps -qf "label=${container_prefix}-pki"); do \
		docker exec -it $${i} /bin/bash -c "yum install -y openssh-server"; \
		docker exec -it $${i} /bin/bash -c "systemctl start sshd"; \
		docker exec -it $${i} /bin/bash -c "mkdir /root/.ssh"; \
		docker exec -it $${i} /bin/bash -c "echo \"$$(cat ${ssh_pub_key})\" > /root/.ssh/authorized_keys"; \
	done


ps:
	docker ps -af "label=${container_prefix}-pki"

clean:
	docker rm -f $$(docker ps -aqf "label=${container_prefix}-pki")
