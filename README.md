# web-nginx
Nginx Container

You will need 2 execute build steps in Jenkins:

git clone https://github.com/cousinea/web-nginx.git
echo ${BUILD_NUMBER} > web-nginx/skel/build
echo 'PASSWORD=1234' > web-nginx/skel/password
docker build -t docker-pilot.dsc.umich.edu:31111/web-nginx web-nginx/
docker tag -f docker-pilot.dsc.umich.edu:31111/web-nginx docker-pilot.dsc.umich.edu:31111/web-nginx:${BUILD_NUMBER}
docker push docker-pilot.dsc.umich.edu:31111/web-nginx 

sed -i -e "s|docker-pilot.dsc.umich.edu:31111/web-nginx:latest|docker-pilot.dsc.umich.edu:31111/web-nginx:$BUILD_NUMBER|g" \
web-nginx/web-nginx.host.marathon.local.json
curl -X PUT -H "Accept: application/json" -H "Content-Type: application/json" \
http://docker-pilot.dsc.umich.edu:8080/v2/apps/web-nginx -d @web-nginx/web-nginx.host.marathon.local.json
