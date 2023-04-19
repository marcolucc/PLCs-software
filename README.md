
# Docker Projects for PLCs MicroLogix-1100, S7-300, and S7-1200

This repository contains three Docker projects for programmable logic controllers (PLCs) - MicroLogix-1100, S7-300, and S7-1200. All of them are built on top of OpenPLC and use httpd to serve the static HTML page on port 80. Additionally, they are configured to talk to each other using Modbus.

## Requirements

To run the Docker projects in this repository, you will need to have Docker installed on your system. You can download Docker from the official website: [https://www.docker.com/get-started](https://www.docker.com/get-started)

## MicroLogix-1100

The MicroLogix-1100 Docker project can be found in the `micrologix-1100` directory. To run it, navigate to the directory and run the following command:


`docker build -t micrologix-1100 .
docker run -p 80:80 -p 502:502 -p 6668:6668 -d micrologix-1100` 

This will build the Docker image and start the container, making it available on port 80. You can access the static HTML page by navigating to `http://localhost` in your web browser.

## S7-300

The S7-300 Docker project can be found in the `s7-300` directory. To run it, navigate to the directory and run the following command:


`docker build -t s7-300 .
docker run -p 80:80 -p 502:502 -p 6668:6668  -d s7-300` 

This will build the Docker image and start the container, making it available on port 80. You can access the static HTML page by navigating to `http://localhost` in your web browser.

## S7-1200

The S7-1200 Docker project can be found in the `s7-1200` directory. To run it, navigate to the directory and run the following command:


`docker build -t s7-1200 .
docker run -p 80:80 -p 502:502 -p 6668:6668  -d s7-1200` 

This will build the Docker image and start the container, making it available on port 80. You can access the static HTML page by navigating to `http://localhost` in your web browser.

## Modbus Configuration

All three Docker projects are configured to talk to each other using Modbus. The configuration files can be found in the respective project directories.

## Contributing

If you would like to contribute to this repository, please fork the repository and submit a pull request. We welcome contributions of all kinds, including bug fixes, new features, and documentation updates.

## License

This repository is licensed under the MIT License. See the LICENSE file for more information.