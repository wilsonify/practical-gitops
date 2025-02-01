
image-build:
	docker build -t testimage/repository:version.

infra-clean:
	terraform destroy

infra-init:
	terraform init

infra-plan: init
	terraform plan

infra-apply:
	terraform apply