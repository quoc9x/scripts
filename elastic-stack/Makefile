ELASTICSEARCH_IMAGE := hub.saobang.vn/nextpay/elasticsearch:v7.16.2@sha256:92795204f6235d52903a1eda5b39ee3037d0fc2d2f7e3421365b12c82193b104

cert:
	docker run --name elastic-helm-charts-certs -i -w /app \
		$(ELASTICSEARCH_IMAGE) \
		/bin/sh -c " \
			elasticsearch-certutil ca --out /app/elastic-stack-ca.p12 --pass '' && \
			elasticsearch-certutil cert --name elasticsearch-cluster --dns elasticsearch-master,elasticsearch,localhost --ip 127.0.0.1 --ca /app/elastic-stack-ca.p12 --pass '' --ca-pass '' --out /app/elastic-certificates.p12" && \
	docker cp elastic-helm-charts-certs:/app/elastic-certificates.p12 ./ && \
	docker rm -f elastic-helm-charts-certs && \
	openssl pkcs12 -nodes -passin pass:'' -in elastic-certificates.p12 -out elastic-certificate.pem && \
	openssl x509 -outform der -in elastic-certificate.pem -out elastic-certificate.crt

# Các file sinh ra cần quyền để container có thể sử dụng (775)
# --dns: các tên để client gọi tới có thể xác thực