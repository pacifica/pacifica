from pacifica/metadata:v0.10.1
from pacifica/cli:v0.4.0
from alpine
run mkdir /jupyterdata
workdir /jupyterdata
run mkdir test_data pacifica-cli
copy *.ipynb .
copy --from=0 /usr/src/app/tests/test_files test_data/
copy --from=1 /etc/pacifica-cli/uploader.json pacifica-cli/
run chown 1000:100 -R /jupyterdata
env SOURCE_COPY_DIRECTORY /srv
cmd cp -a * $SOURCE_COPY_DIRECTORY/
