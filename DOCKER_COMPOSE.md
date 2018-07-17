# Docker Compose Development Environment

Docker compose is a tool to spawn multiple docker 
containers and link them together on a single docker server
or docker swarm cluster. This is useful for developers
to bring up the environment or for demonstration purposes
to inspect the system.

## Docker Compose Details

The docker compose file defines five volumes. The volumes
are usually located on the host in
`/var/lib/docker/volumes`. These volumes define the
persistent state across reboots of the system. The location
maybe distribution dependent so check the docker provider
documentation to be certain.

The `archivedata` volume contains the files in the archive.
These files will be saved for the life of the volume. This
does happen across bring up and shutdown of the services.

The `ingestshared` volume is the intermediate storage used
by the ingest services. Data from the uploaders are staged
on the `ingestshared` volume.

The `cartshared` volume is the intermediate storage used by
the cart service. Data from the archive is staged on the
`cartshared` volume.

The `metadata` and `uniqueiddata` volumes are the 
persistent metadata databases required by the services.

Each service in docker compose has its external port
exposed so you can interact with it locally.

| Port | Service            |
| ---- | ------------------ |
| 8080 | Archive Interface  |
| 8081 | Cart Interface     |
| 8051 | UniqueID Interface |
| 8121 | Metadata Interface |
| 8181 | Policy Interface   |
| 8066 | Ingest Interface   |
| 8180 | Proxy Interface    |

## Host Requirements

Requirements for the host running the docker server are the
following.

 * RAM - 8Gb minimum - 16Gb preferred
 * CPUs - 4 minimum - 8 preferred
 * DISK - 20Gb minimum - 1Tb preferred

The disk requirement is very dependent on the data sets
that will be uploaded. Scale that as nessisary for testing
and development purposes.

## Bring Up Services

There are two commands to separate the bring up of Pacifica
with docker compose.

```
# docker-compose pull
# docker-compose up
```

Running `docker-compose up` does perform a pull prior to
bringing up the services. However, depending on I/O and
network bandwidth, services may fail to connect.

Verify the services are running.

```
# docker-compose ps
```

## Load the Test Data Set

The test data set is coupled with the metadata service and 
can be loaded through docker like the following.

```
# docker run \
  -it \
  --rm \
  --network=pacifica_default \
  -e METADATA_URL=http://metadataserver:8121 \
  --entrypoint python \
  pacifica/metadata -m test_files.loadit
```

## Verify the Services

The verification steps are coupled with the test dataset
loaded from the metadata image.

### The Archive Interface

The archive interface should respond to accessing files.

```
# docker run -it --rm --network=pacifica_default centos:7 curl -X PUT -T /etc/group http://archiveinterface:8080/103
{"message": "File added to archive", "total_bytes": 357}
# docker run -it --rm --network=pacifica_default centos:7 curl -X PUT -T /etc/group http://archiveinterface:8080/104
{"message": "File added to archive", "total_bytes": 357}
# docker run -it --rm --network=pacifica_default centos:7 curl -X HEAD -I http://archiveinterface:8080/103
HTTP/1.1 204 No Content
X-Pacifica-File-Storage-Media: disk
X-Pacifica-Messsage: File was found
X-Content-Length: 357
X-Pacifica-File: /srv/103
X-Pacifica-Ctime: 1531671385.34
Server: CherryPy/15.0.0
Last-Modified: 1531671384.35
Allow: GET, HEAD, PATCH, POST, PUT
Date: Sun, 15 Jul 2018 16:17:00 GMT
Content-Type: application/json
X-Pacifica-Bytes-Per-Level: (357L,)
```

### The Metadata Interface

The metadata service should also be available by docker.

```
# docker run \
  -it \
  --rm \
  --network=pacifica_default \
  centos:7 \
  curl \
  -X POST \
  -H 'Content-Type: application/json' \
  http://metadataserver:8121/files?_id=103 \
  -d'{\"size\":357,\"hashtype\":\"sha1\",\"hashsum\":\"8692d27afeda98a594d30e8b44bf402f87fb332f\"}'
null
# docker run \
  -it \
  --rm \
  --network=pacifica_default \
  centos:7 \
  curl \
  -X POST \
  -H 'Content-Type: application/json' \
  http://metadataserver:8121/files?_id=104 \
  -d'{\"size\":357,\"hashtype\":\"sha1\",\"hashsum\":\"8692d27afeda98a594d30e8b44bf402f87fb332f\"}'
null
```

# The UniqueID Interface

```
# docker run \
  -it \
  --rm \
  --network=pacifica_default \
  centos:7 \
  curl \
  'http://uniqueid:8051/getid?mode=test_index&range=1'
{"startIndex": 1, "endIndex": 1}
```

# The Policy Interface

```
# docker run \
  -it \
  --rm \
  --network=pacifica_default \
  centos:7 \
  curl \
  http://policyserver:8181/status/users/search/dmlb2001/simple | python -m json.tool
[
    {
        "display_name": "David\u00e9 Brown\u00e9 Jr",
        "email_address": "david.brown@pnnl.gov",
        "emsl_employee": true,
        "first_name": "David\u00e9",
        "last_name": "Brown\u00e9 Jr",
        "person_id": 10
    }
]
```

# The Proxy Interface

```
# docker run \
  -it \
  --rm \
  --network=pacifica_default \
  centos:7 \
  curl \
  http://proxyserver:8180/files/sha1/8692d27afeda98a594d30e8b44bf402f87fb332f
root:x:0:
bin:x:1:
daemon:x:2:
sys:x:3:
adm:x:4:
tty:x:5:
disk:x:6:
lp:x:7:
mem:x:8:
kmem:x:9:
wheel:x:10:
cdrom:x:11:
mail:x:12:
man:x:15:
dialout:x:18:
floppy:x:19:
games:x:20:
tape:x:33:
video:x:39:
ftp:x:50:
lock:x:54:
audio:x:63:
nobody:x:99:
users:x:100:
utmp:x:22:
utempter:x:35:
input:x:999:
systemd-journal:x:190:
systemd-network:x:192:
dbus:x:81:
```

# The Cart Interface

```
# docker run \
  -it \
  --rm \
  --network=pacifica_default \
  centos:7 \
  curl \
  -X POST \
  -H 'Content-Type: application/json' \
  http://cartfrontend:8081/example_cart \
  -d'{\"fileids\":[{\"id\":\"103\",\"path\":\"a/b/c/foo.txt\",\"hashtype\":\"sha1\",\"hashsum\":\"8692d27afeda98a594d30e8b44bf402f87fb332f\"}]}'
{"message": "Cart Processing has begun"}
# docker run \
  -it \
  --rm \
  --network=pacifica_default \
  centos:7 \
  curl -I \
  http://cartfrontend:8081/example_cart
HTTP/1.1 204 No Content
Content-Type: application/json
Server: CherryPy/16.0.3
Date: Sun, 15 Jul 2018 18:25:06 GMT
Allow: DELETE, GET, HEAD, POST
X-Pacifica-Status: ready
X-Pacifica-Message:
```

# The Ingest Interface

```
# docker run \
  -it \
  --rm \
  --network=pacifica_default \
  -e UPLOAD_URL=http://ingestfrontend:8066/upload \
  -e STATE_URL=http://ingestfrontend:8066/get_state \
  -e POLICY_URL=http://policyserver:8181/uploader \
  -e POLICY_ADDR=policyserver \
  pacifica/cliuploader \
  upload \
  --logon 10 \
  entrypoint.sh
...
...
...
Authentication Type (): Setting logon to 10.
Setting instrument to 54.
Setting proposal to 1234a.
Setting user-of-record to 10.
Setting directory-proposal to 1234a.
Done 10240.
Waiting job to complete (5).
Done.
{
    "created": "2018-07-16 04:27:29",
    "exception": "",
    "job_id": 1,
    "state": "OK",
    "task": "ingest metadata",
    "task_percent": "100.00000",
    "updated": "2018-07-16 04:27:31"
}
```
