from node:10

run git clone https://github.com/pacifica/pacifica-metadata-mgmt.git /usr/src/app
workdir /usr/src/app
run git checkout v0.0.1
run npm install
run npm run build

from nginx
run rm -rf /usr/share/nginx/html
copy default.conf /etc/nginx/conf.d/default.conf
copy --from=0 /usr/src/app/build /usr/share/nginx/html
