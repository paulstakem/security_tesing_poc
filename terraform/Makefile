
scan/tfsec:
	tfsec ./

scan/checkov:
	checkov --directory ./

tfsec-docker:
	docker run --rm -it -v "$(pwd):/src" aquasec/tfsec ./
