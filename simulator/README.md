# Modbus Simulator
The Modbus Simulator is used to create mock Modbus device for testing purposes. 

In this version of the simulator, a random generator is included, which sets pseudorandom values in the first 10 registers of the Holding Register table in random time intervals, in the range of 0-10 seconds.

## Build docker image
The docker image built with git branch https://github.com/lefmylonas/device-modbus-go/tree/edgex-modbus-simulator
```
git clone -b edgex-modbus-simulator https://github.com/lefmylonas/device-modbus-go.git
cd device-modbus-go/
sudo docker build -t randgen-modbus-sim .
```

## Usage

1. Create a default simulator with port 1502
    ```
    docker run --rm -p 1502:1502 randgen-modbus-sim
    ```

2. create simulators for scalability test 
    ```
    docker run --rm -d --net=host --name modbus-simulator  \
        -e SIMULATOR_NUMBER=1000 -e STARTING_PORT=10000 randgen-modbus-sim
    ```
    To handle a lot of ports, docker recommends user using the host network, see https://docs.docker.com/network/host/.
    
    In scalability test mode, the simulator will count the Modbus command amount as the reading amount and provide APIs for edgex-taf to measure the event amount: 
    * Query reading amount: http://localhost:1503/reading/count
    * Reset reading amount: http://localhost:1503/reading/count/reset

    When running a scalibility test, in case of trouble during connection with the mock Modbus slave devices, it may be necassary to create a rule in your firewall settings. For example, if we want to connect to a slave device in port 10000, we could run this command:
    ```
    sudo ufw allow 10000
    ```
    in order to allow the connection.

    You can always inspect the firewall rules with this command:
    ```
    sudo ufw status
    ```