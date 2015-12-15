# nginx-container-test
Nginx Sample Container #2

You will need 2 execute build steps in Jenkins:

git clone https://github.com/cousinea/nginx-container-test.git
echo ${BUILD_NUMBER} > nginx-container-test/skel/build
echo 'PASSWORD=1234' > nginx-container-test/skel/password
docker build -t docker-pilot.dsc.umich.edu:31111/cousinea-nginx-container-test nginx-container-test/
docker tag -f docker-pilot.dsc.umich.edu:31111/cousinea-nginx-container-test docker-pilot.dsc.umich.edu:31111/cousinea-nginx-container-test:${BUILD_NUMBER}
docker push docker-pilot.dsc.umich.edu:31111/cousinea-nginx-container-test 

sed -i -e "s|docker-pilot.dsc.umich.edu:31111/cousinea-nginx-container-test:latest|docker-pilot.dsc.umich.edu:31111/cousinea-nginx-container-test:$BUILD_NUMBER|g" \
nginx-container-test/nginx-container-test.host.marathon.local.json
curl -X PUT -H "Accept: application/json" -H "Content-Type: application/json" \
http://docker-pilot.dsc.umich.edu:8080/v2/apps/cousinea-nginx-container-test -d @nginx-container-test/nginx-container-test.host.marathon.local.json
