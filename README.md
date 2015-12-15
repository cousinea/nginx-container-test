# nginx-container
Nginx Sample Container #2

You will need 2 execute build steps in Jenkins:

git clone https://github.com/cousinea/web-nginx-container.git
echo ${BUILD_NUMBER} > web-nginx-container/skel/build
echo 'PASSWORD=1234' > web-nginx-container/skel/password
docker build -t docker-pilot.dsc.umich.edu:31111/web-nginx-container web-nginx-container/
docker tag -f docker-pilot.dsc.umich.edu:31111/web-nginx-container docker-pilot.dsc.umich.edu:31111/web-nginx-container:${BUILD_NUMBER}
docker push docker-pilot.dsc.umich.edu:31111/web-nginx-container 

sed -i -e "s|docker-pilot.dsc.umich.edu:31111/web-nginx-container:latest|docker-pilot.dsc.umich.edu:31111/web-nginx-container:$BUILD_NUMBER|g" \
web-nginx-container/web-nginx-container.host.marathon.local.json
curl -X PUT -H "Accept: application/json" -H "Content-Type: application/json" \
http://docker-pilot.dsc.umich.edu:8080/v2/apps/web-nginx-container -d @web-nginx-container/nginx-container.host.marathon.local.json
