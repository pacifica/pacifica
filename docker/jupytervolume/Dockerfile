FROM pacifica/metadata:v0.12.4
FROM pacifica/cli:v0.5.0
FROM alpine
RUN mkdir /jupyterdata
WORKDIR /jupyterdata
RUN mkdir test_data pacifica-cli
COPY *.ipynb ./
COPY --from=0 /usr/src/app/tests/test_files test_data/
COPY --from=1 /etc/pacifica-cli/uploader.json pacifica-cli/
RUN chown 1000:100 -R /jupyterdata
ENV SOURCE_COPY_DIRECTORY /srv
CMD cp -a * $SOURCE_COPY_DIRECTORY/; chown 1000:100 -R $SOURCE_COPY_DIRECTORY
