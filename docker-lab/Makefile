SHELL=/bin/bash

dockerfile = Dockerfile.centos
image_name = centos7
container_prefix = lab

containers = ${container_prefix}-root ${container_prefix}-intermediate ${container_prefix}-issuing

ssh_pub_key = ${HOME}/.ssh/id_rsa.pub

all: build setup run

build:
	docker build -t ${image_name}:systemd -f ${dockerfile} .

setup:
	docker run --privileged -d --name temp ${image_name}:systemd
	docker exec -it temp /bin/bash -c "yum update -y"; \
	docker exec -it temp /bin/bash -c "yum install -y openssh-server"; \
	docker exec -it temp /bin/bash -c "systemctl start sshd"; \
	docker exec -it temp /bin/bash -c "mkdir /root/.ssh"; \
	docker exec -it temp /bin/bash -c "echo \"$$(cat ${ssh_pub_key})\" > /root/.ssh/authorized_keys"; \
	docker commit temp ${image_name}:lab
	docker rm -f temp

run: $(containers)
$(containers):
	docker run -d --privileged \
		-l ${container_prefix}-pki \
		-p 2222-2224:22 \
		-p 8200-8202:22 \
		--name $@ \
		${image_name}:lab

ps:
	docker ps -af "label=${container_prefix}-pki"

clean:
	docker rm -f $$(docker ps -aqf "label=${container_prefix}-pki")
