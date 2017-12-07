# nifi-protobuf-processor [![codecov](https://codecov.io/gh/whiver/nifi-protobuf-processor/branch/develop/graph/badge.svg)](https://codecov.io/gh/whiver/nifi-protobuf-processor) [![Build Status](https://travis-ci.org/whiver/nifi-protobuf-processor.svg?branch=develop)](https://travis-ci.org/whiver/nifi-protobuf-processor)
An Apache NiFi processor to encode and decode data using Google Protocol Buffers schemas.

## Features

As this processor is a work-in-progress, the features already implemented and tested are checked.

### Main functionalities
- [x] Graphical configuration interface in Apache NiFi
- [x] Encode a JSON payload to Protocol Buffers
- [x] Decode a Protocol Buffers payload to JSON
- [x] Support dependencies in proto files (see [below](#usage))
- [ ] Allow to encode/decode from/to XML, JSON and flowfile properties

### Protobuf schema specification
- [x] Support compiled schema at a specified user location on the disk
- [ ] Support compiled schema embedded in the flowfile
- [ ] Support embedding raw .proto files in the flowfile

### Processor packaging
- [x] Provide a ready-to-use Docker image of Apache NiFi with the NiFi Protobuf Processor

## Installation

### Using Docker
A pre-packaged version of NiFi with the processor installed is
[available on Docker Hub](https://hub.docker.com/r/whiver/nifi-protobuf/). To run it just type:

    docker run -p 8080:8080 whiver/nifi-protobuf:latest

Note that the `-p` option publishes the port 8080 used by NiFi to the host, so that you can access the UI directly *via*
`http://localhost:8080/nifi`.

### Using a stable release
Grab the latest release directly from the releases page and copy the `.nar` file in the Apache NiFi `lib` folder.

### Building the latest version
Clone this project and build the processor `nar` file using Maven:

    mvn compile
    mvn nifi-nar:nar
    
Then simply copy the generated `nar` file into the Apache NiFi `lib` folder.

#### Building the Docker image

The project also includes a Dockerfile to easily build a Docker image of the project. In fact you just need to run:

    mvn package
    
and everything should be fine ! :)

## Usage

See the installation section to learn how to integrate this processor in Apache NiFi.
This projects add 2 different new processors in NiFi:

- `ProtobufDecoder`, which **decodes** a Protobuf-encoded payload to different kind of structured formats ;
- `ProtobufEncoder`, which **encodes** a payload in a structured format using a Protobuf schema.

These processors both expect the incoming flowfiles to have the `protobuf.schemaPath` defined, which should contain the
path to the **compiled** `.desc` proto file to use to decode/encode the data.

> **Important**: The processor allows you to import **only one schema file**, so you need to package all you dependencies
> into one file. To do so, compile your main `.proto` file using the `--include_imports` option of the `protoc` compiler.


> *Note*: if you don't have a compiled `.desc` file yet, you should
> [take a look at `protoc`](https://github.com/google/protobuf/releases), the Protobuf compiler from Google. 

For now, the only structured format the processors can process is the JSON. In the future, there should be more formats
available (XML and flowfile properties are expected).

## Contributing

This project is Free as in Freedom, so feel free to contribute by posting bug report or pull requests!

## License

This project is licensed under the MIT license. The terms of this license can be found in the [LICENSE file](LICENSE).