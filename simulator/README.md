# Modbus Simulator
Modbus Simulator is used to create mock Modbus device for testing purpose. 

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
    docker run --rm -d --network host --name modbus-simulator  \
        -e SIMULATOR_NUMBER=1000 -e STARTING_PORT=10000 randgen-modbus-sim
    ```
    To handle a lot of ports, docker recommends user using the host network, see https://docs.docker.com/network/host/.
    
    In scalability test mode, the simulator will count the Modbus command amount as the reading amount and provide APIs for edgex-taf to measure the event amount: 
    * Query reading amount: http://localhost:1503/reading/count
    * Reset reading amount: http://localhost:1503/reading/count/reset