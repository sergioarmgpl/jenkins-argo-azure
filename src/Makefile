push_az:
	docker login ${AZ_REGISTRY} -u ${AZ_USER} -p ${AZ_PASSWORD}
	docker build . -t ${AZ_REGISTRY}/${IMG_NAME}:${APP_VERSION}-${BUILD_NUMBER}
	docker push ${AZ_REGISTRY}/${IMG_NAME}:${APP_VERSION}-${BUILD_NUMBER}
push_base:
	cat ${GCR_KEY} | docker login -u _json_key --password-stdin ${DOCKER_REGISTRY}
	docker build . -t ${DOCKER_REPO}/${IMG_NAME}:${APP_VERSION}-${BUILD_NUMBER}
	docker push ${DOCKER_REPO}/${IMG_NAME}:${APP_VERSION}-${BUILD_NUMBER}
clean:
	docker rmi ${AZ_REGISTRY}/${IMG_NAME}:${APP_VERSION}-${BUILD_NUMBER} || echo "everything clean"
deploy:
	curl -sSL -k -o argocd https://${ARGOCD_SERVER}/download/argocd-linux-amd64
	chmod 755 argocd
	./argocd app set ${ARGO_PROJECT} -p dockerImage="${AZ_REGISTRY}/${IMG_NAME}:${APP_VERSION}-${BUILD_NUMBER}" \
	-p namespace="${NAMESPACE}" -p message="hello world USAC :) :)" --auth-token ${ARGO_TOKEN}
	./argocd app sync ${ARGO_PROJECT} --auth-token ${ARGO_TOKEN}
	rm argocd
	echo "deployed done"
deploy_argo:
	curl -sSL -k -o argocd https://${ARGOCD_SERVER}/download/argocd-linux-amd64
	chmod 755 argocd
	./argocd app --insecure --grpc-web set ${ARGO_PROJECT} -p dockerImage="${AZ_REGISTRY}/${IMG_NAME}:${APP_VERSION}-${BUILD_NUMBER}" \
	-p namespace="${NAMESPACE}" -p domain="${DOMAIN}" -p message="hello world CodeCamp 2021" --auth-token ${ARGO_TOKEN}
	./argocd app --insecure --grpc-web sync ${ARGO_PROJECT} --auth-token ${ARGO_TOKEN}
	rm argocd
	echo "deployed done"