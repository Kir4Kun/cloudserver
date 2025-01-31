# Zenko CloudServer ❤️

![Zenko CloudServer logo](res/scality-cloudserver-logo.png)

[![Docker Pulls][badgedocker]](https://hub.docker.com/r/zenko/cloudserver)
[![Docker Pulls][badgetwitter]](https://twitter.com/zenko)

## Overview

CloudServer (formerly S3 Server) is an open-source Amazon S3-compatible
object storage server that is part of [Zenko](https://www.zenko.io),
Scality’s Open Source Multi-Cloud Data Controller.

CloudServer provides a single AWS S3 API interface to access multiple
backend data storage both on-premise or public in the cloud.

CloudServer is useful for Developers, either to run as part of a
continous integration test environment to emulate the AWS S3 service locally
or as an abstraction layer to develop object storage enabled
application on the go.

## Learn more at [www.zenko.io/cloudserver](https://www.zenko.io/cloudserver/)

## [May I offer you some lovely documentation?](http://s3-server.readthedocs.io/en/latest/)

## Docker

[Run your Zenko CloudServer with Docker](https://hub.docker.com/r/zenko/cloudserver/)

## Contributing

In order to contribute, please follow the
[Contributing Guidelines](
https://github.com/scality/Guidelines/blob/master/CONTRIBUTING.md).

## Installation

### Dependencies

Building and running the Zenko CloudServer requires node.js 10.x and yarn v1.17.x
. Up-to-date versions can be found at
[Nodesource](https://github.com/nodesource/distributions).

### Clone source code

```shell
git clone https://github.com/scality/S3.git
```

### Install js dependencies

Go to the ./S3 folder,

```shell
yarn install --frozen-lockfile
```

If you get an error regarding installation of the diskUsage module,
please install g++.

If you get an error regarding level-down bindings, try clearing your yarn cache:

```shell
yarn cache clean
```

## Run it with a file backend

```shell
yarn start
```

This starts a Zenko CloudServer on port 8000. Two additional ports 9990 and
9991 are also open locally for internal transfer of metadata and data,
respectively.

The default access key is accessKey1 with
a secret key of verySecretKey1.

By default the metadata files will be saved in the
localMetadata directory and the data files will be saved
in the localData directory within the ./S3 directory on your
machine.  These directories have been pre-created within the
repository.  If you would like to save the data or metadata in
different locations of your choice, you must specify them with absolute paths.
So, when starting the server:

```shell
mkdir -m 700 $(pwd)/myFavoriteDataPath
mkdir -m 700 $(pwd)/myFavoriteMetadataPath
export S3DATAPATH="$(pwd)/myFavoriteDataPath"
export S3METADATAPATH="$(pwd)/myFavoriteMetadataPath"
yarn start
```

## Run it with multiple data backends

```shell
export S3DATA='multiple'
yarn start
```

This starts a Zenko CloudServer on port 8000.
The default access key is accessKey1 with
a secret key of verySecretKey1.

With multiple backends, you have the ability to
choose where each object will be saved by setting
the following header with a locationConstraint on
a PUT request:

```shell
'x-amz-meta-scal-location-constraint':'myLocationConstraint'
```

If no header is sent with a PUT object request, the
location constraint of the bucket will determine
where the data is saved. If the bucket has no location
constraint, the endpoint of the PUT request will be
used to determine location.

See the Configuration section in our documentation
[here](http://s3-server.readthedocs.io/en/latest/GETTING_STARTED/#configuration)
to learn how to set location constraints.

## Run it with an in-memory backend

```shell
yarn run mem_backend
```

This starts a Zenko CloudServer on port 8000.
The default access key is accessKey1 with
a secret key of verySecretKey1.

## Run it with Vault user management

Note: Vault is proprietary and must be accessed separately.

```shell
export S3VAULT=vault
yarn start
```

This starts a Zenko CloudServer using Vault for user management.

[badgetwitter]: https://img.shields.io/twitter/follow/zenko.svg?style=social&label=Follow
[badgedocker]: https://img.shields.io/docker/pulls/scality/s3server.svg
[badgepub]: https://circleci.com/gh/scality/S3.svg?style=svg
[badgepriv]: http://ci.ironmann.io/gh/scality/S3.svg?style=svg&circle-token=1f105b7518b53853b5b7cf72302a3f75d8c598ae
