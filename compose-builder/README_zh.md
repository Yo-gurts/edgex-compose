## Edgex Docker Compose Builder for Ireland Release

这个文件夹包含`Compose Builder`，它是由**source** compose 和*环境变量文件*以及用于构建Ireland版本的docker compose 文件的**makefile**组成。

> **Note to Developers**: 
> For `Hanoi` patch release, once you have edited and tested your changes to the source compose files you **MUST** regenerate the committed `Ireland` compose files using the `make build` command.

### 生成定制的 Compose files

如果任何一个标准的`Ireland` compose files都不满足你的需要，你可以通过使用`make gen <options>`来生成和运行一个定制的 compose file。[Gen](#gen) 和 [Run](#run) 的详细说明在下面。`Run` 只是在生成定制的compose file后运行它。

### Multiple Compose files approach

使用这些source compose files的方法是`使用多个compose files进行扩展`，说明见此：https://docs.docker.com/compose/extends/#multiple-compose-files


`使用多个compose files进行扩展`方法与环境文件一起删除了早期EdgeX compose file中发现的所有重复项。但由于必须列出运行特定配置所需的多个compose file，因此这种方法使运行变得更加复杂。为了减轻这种复杂性，我们提供了生成好的单个文件，见上一级文件夹中的compose file。这里提供了一个`Makefile`和多个命令，可以在测试你的改变时轻松地运行多种可能的配置。有关这些命令的详细信息，请参阅下面的 Makefile 部分。

> *注意： `make run`、`make pull` 和 `make gen` 命令都生成一个单独的 `docker-compose.yml` 文件，其中包含来自多个compose file的内容，环境文件中的所有变量都针对所使用的指定选项进行了解析。 请参阅下面的选项列表。*

### Compose Files

此文件夹包含以下 compose files：

- **docker-compose-base.yml**
    基本非安全模式compose file，包含在非安全配置中运行的所有服务。
- **add-security.yml**
    安全 **扩展** compose file，添加额外的安全服务和服务配置，以便所有服务都在安全配置中运行。
- **add-secure-redis-messagebus.yml**
    安全 Redis MessageBus **扩展** compose file，为安全模式下将 Redis 用作 MessageBus 时添加额外的安全配置，以便 Kuiper 可以连接到安全的 MesssageBus。
- **add-device-bacnet.yml**
    设备服务 **扩展** compose file，添加了 **Device Bacnet** 服务。
- **add-device-camera.yml**
    设备服务 **扩展** compose file，添加了 **Device Camera** 服务。
- **add-device-grove.yml**
    设备服务 **扩展** compose file，添加了 **Device Grove** 服务。
- **add-device-modbus.yml**
    设备服务 **扩展** compose file，添加了 **Device Modbus** 服务。
- **add-device-mqtt.yml**
    设备服务 **扩展** compose file，添加了 **Device MQTT** 服务。
- **add-device-rest.yml**
    设备服务 **扩展** compose file，添加了 **Device MQTT** 服务。
- **add-device-snmp.yml**
    设备服务 **扩展** compose file，添加了 **Device SNMP** 服务。
- **add-device-virtual.yml**
    设备服务 **扩展** compose file，添加了 **Device Virtual** 服务。
- **add-device-virtual.yml**
    设备服务 **扩展** compose file，添加了 **Device Virtual** 服务。
- **add-device-coap.yml**
    设备服务 **扩展** compose file，添加了 **Device COAP** 服务。
- **add-device-gpio.yml**
    设备服务 **扩展** compose file，添加了 **Device GPIO** 服务。
- **add-device-rfid-llrp.yml**
    设备服务 **扩展** compose file，添加了 **Device RFID LLRP** 服务。
- **add-asc-http-export.yml**
    可配置的应用服务 **扩展** compose file，添加了 **App Service Http Export** 服务
- **add-service-secure-template.yml**
    A template for a single service **extending** compose file from its base service for security mode,
    and the service is enabled with secret store by default.
    单个服务的模板 **扩展** compose file,从其安全模式的基本服务，该服务默认启用秘密存储。
- **add-asc-mqtt-export.yml**
    可配置的应用服务 **扩展** compose file，添加了 **App Service MQTT Export** 服务
- **add-app-rfid-llrp-inventory.yml**
    可配置的应用服务 **扩展** compose file，添加了 **App RFID LLRP Inventory** 服务
- **add-modbus-simulator.yml**
    ModBus 模拟器 **扩展** compose file，添加了 MQTT ModBus 模拟器服务。必须配合 **add-device-modbus.yml** 使用。
- **add-mqtt-broker.yml**
    MQTT Broker **extending** compose file. Adds the Eclipse Mosquitto MQTT Broker.
- **add-mqtt-messagebus.yml**
    MQTT MesssageBus **extending** compose file. Adds additional configuration of services so that the `MQTT` implementation of the Edgex Message Bus is used. **Must be used in conjunction with add-mqtt-broker.yml**
- **add-taf-app-services.yml**
    TAF App Services **extending** compose file. Adds additional App Service for the TAF testing compose files.
- **add-taf-app-services-secure.yml**
    TAF App Services **extending** `add-taf-app-services` compose file, and services are enabled with secret store by default.
- **add-taf-device-services-mods.yml**
    TAF Device Services **extending** compose file. Modifies setting of Device Virtual and Device Modbus for the TAF testing compose files. **Must be used in conjunction with add-device-modbus.yml and add-device-virtual.yml**
- **add-ui.yml**
    UI **扩展** compose file，添加了 **EdgeX UI** 服务。 

### Environment Files

该文件夹包含以下环境文件：

- **.env**
    此文件包含 compose file 中引用的 `version`、`repositories` 和 镜像 `version` 变量。Docker compose 隐式使用 `.env` 文件（如果存在），因此您不会在 compose 文件中看到它被引用。 它在 Makefile 中被引用，以便它也可以使用这些设置。
- **common.env**
    此文件包含被所有 Edgex 服务使用的常见环境变量。
- **common-security.env**
    此文件包含许多 Edgex 服务使用的与安全相关的常见环境变量。
- **common-sec-stage-gate.env**
    此文件包含许多 Edgex 服务使用的常见安全引导程序阶段门相关的环境变量。
- **asc-common.env**
    此文件包含所有应用程序服务使用的公共环境变量。
- **asc-http-export.env**
    此文件包含所有使用`http-export`配置文件的可配置应用服务实例所使用的公共环境变量。
- **asc-mqtt-export.env**
    此文件包含所有使用`mqtt-export`配置文件的可配置应用服务实例所使用的公共环境变量。

### Makefile

这个文件夹包含一个`Makefile`，它为各种 EdgeX 配置提供用于构建、运行、停止、生成、清理等的命令。它在 **开发** compose files 或生成用于测试服务更改的 DEV 版本期间使用。有关生成 compose file 的可用选项的更多详细信息，请参阅下面的 [Gen](#gen) 命令。

```
用法： `make <target>` 其中 target 是:
```
#### Portainer

```
portainer       独立于 EdgeX 服务运行 Portainer
portainer-down	停止独立于 EdgeX 服务的 Portainer
```
#### Build

```
build
生成所有标准 Edgex compose file 变体和 TAF 测试compose file。 生成的 compose files 存储在顶级文件夹中。标准 compose files 的每个变体都包括 Device REST 和 Device Virtual。Compose 文件被适当地命名为用于生成它们的选项。TAF 撰写文件存储在`taf`子文件夹中

标准的 compose 变体有：
   full secure (docker-compose.yml)
   full secure for arm64 (docker-compose-arm64.yml)
   non-secure (docker-compose-no-secty.yml)
   non-secure for arm64 (docker-compose-no-secty-arm64.yml)
   non-secure with UI (docker-compose-no-secty-with-ui.yml)
   non-secure with UI for arm64 (docker-compose-no-secty-with-ui-arm64.yml)

 TAF compose variations are:
   full secure general testing (docker-compose-taf.yml)
   full secure general testing for arm64 (docker-compose-taf-arm64.yml)
   non-secure general testing (docker-compose-taf-no-secty.yml)
   nonsecure general testing for arm64 (docker-compose-taf-no-secty-arm64.yml)
   full secure perf testing (docker-compose-taf-perf.yml)
   full secure perf testing for arm64 (docker-compose-taf-perf-arm64.yml)
   non-secure perf testing (docker-compose-taf-perf-no-secty.yml)
   nonsecure perf testing for arm64 (docker-compose-taf-perf-no-secty-arm64.yml)
```
#### Run

```
run [options] [services]
运行指定的 EdgeX 服务:

Options:
    no-secty:    Runs in Non-Secure Mode, otherwise runs in Secure Mode
    arm64:       Runs using ARM64 images    
    dev:         Runs using local dev built images from edgex-go repo's    
                 'make docker' which creates docker images tagged with '0.0.0-dev'
    app-dev:     Runs using local dev built images from app-service-configurable repo's
                 'make docker' which creates docker images tagged with '0.0.0-dev'
    ds-modbus:   Runs with device-modbus included
    ds-bacnet:   Runs with device-bacnet included
    ds-camera:   Runs with device-camera included
    ds-grove:    Runs with device-grove included (valid only with arm64 option)
    ds-mqtt:     Runs with device-mqtt included
    ds-rest:     Runs with device-rest included
    ds-snmp:     Runs with device-snmp included
    ds-virtual:  Runs with device-virtual included
    ds-coap:     Runs with device-coap included
    ds-gpio:     Runs with device-gpio included
    ds-llrp:     Runs with device-rfid-llrp included
    modbus-sim:  Runs with ModBus simulator included
    asc-http:    Runs with App Service HTTP Export included
    asc-mqtt:    Runs with App Service MQTT Export included
    as-llrp:     Runs with App RFID LLRP Inventory included
    mqtt-broker: Runs with a MQTT Broker service included 
    mqtt-bus:    Runs with services configure for MQTT Message Bus 
    zmq-bus:     Runs with services configure for ZMQ Message Bus     
    ui:          Runs with the UI service included

Services:
    <names...>: Runs only services listed (and their dependent services) where 'name' matches a service name in one of the compose files used
```
#### Down

```    
down
停止所有 EdgeX 服务，无论哪种配置启动的它们
```

#### Pull

```				
pull [options] [services]
拉取指定的 EdgeX 服务镜像:

Options:
    no-secty:    Pulls images for Non-Secure Mode, otherwise pull images for Secure Mode
    arm64:       Pulls ARM64 version of images    
    ds-bacnet:   Pull includes device-bacnet 
    ds-camera:   Pull includes device-camera
    ds-grove:    Pull includes device-grove (valid only with arm64 option)
    ds-modbus:   Pull includes device-modbus 
    ds-mqtt:     Pull includes device-mqtt
    ds-rest:     Pull includes device-rest
    ds-snmp:     Pull includes device-snmp
    ds-virtual:  Pull includes device-virtual
    ds-coap:     Pull includes device-coap
    ds-gpio:     Pull includes device-gpio
    ds-llrp:     Pull includes device-rfid-llrp
    modbus-sim:  Pull includes ModBus simulator
    asc-http:    Pull includes App Service HTTP Export
    asc-mqtt:    Pull includes App Service MQTT Export
    as-llrp:     Pull includes App RFID LLRP Inventory
    mqtt-broker: Pull includes MQTT Broker service
    mqtt-bus:    Pull includes additional services for MQTT Message Bus
    zmq-bus:     Pull includes additional services for ZMQ Message Bus     
    ui:          Pulls includes the EdgeX UI service.

Services:
    <names...>: Pulls only images for the service(s) listed
```
#### Gen

```	
gen [options]
生成指定的临时单个compose file（`docker-compose.yml`）：

Options:
    no-secty:   Generates non-secure compose, otherwise generates secure compose file
    arm64:      Generates compose file using ARM64 images    
    dev:        Generates compose file using local dev built images from edgex-go repo's 
                'make docker' which creates docker images tagged with '0.0.0-dev'
    app-dev:    Generates compose file using local dev built images from app-service-configurable repo's
                'make docker' which creates docker images tagged with '0.0.0-dev'
    ds-modbus:  Generates compose file with device-modbus included
    ds-bacnet:  Generates compose file with device-bacnet included
    ds-camera:  Generates compose file with device-camera included
    ds-grove:   Generates compose file with device-grove included (valid only with arm64 option)
    ds-mqtt:     Generates compose file with device-mqtt included
    ds-rest:     Generates compose file with device-rest included
    ds-snmp:     Generates compose file with device-snmp included
    ds-virtual:  Generates compose file with device-virtual included
    ds-coap:     Generates compose file with device-coap included
    ds-gpio:     Generates compose file with device-gpio included
    ds-llrp:     Generates compose file with device-rfid-llrp included
    modbus-sim:  Generates compose file with ModBus simulator included
    asc-http:    Generates compose file with App Service HTTP Export included
    asc-mqtt:    Generates compose file with App Service MQTT Export included
    as-llrp:     Generates compose file with App RFID LLRP Inventory included
    mqtt-broker: Generates compose file with a MQTT Broker service included 
    mqtt-bus:    Generates compose file with services configured for MQTT Message Bus 
                 The MQTT Broker service is also included. 
    zmq-bus:     Generates compose file with services configured for ZMQ Message Bus     
    ui:          Generates compose file with UI sevice included             
```
#### Clean

```
clean
运行 'down'，然后删除所有停止的容器，未使用的卷和网络
```

#### Get-token

```
get-token [options] 
生成指定的 Kong 访问令牌：

Options:
    arm64:  Generates a Kong access token using ARM64 image
    dev:    Generates a Kong access token using local dev built docker image
            'make docker', which creates docker images tagged with '0.0.0-dev'    
```
#### Upload-tls-cert

```
upload-tls-cert [options] <environment_variables>
将自带 (BYO) TLS 证书上传到 Kong 代理服务器，如下所示：

Options:
    arm64:  Upload TLS certificate to the Kong server using ARM64 image
    dev:    Upload TLS certificate to the Kong server using local dev built docker image
            'make docker', which creates docker images tagged with '0.0.0-dev'
Environment Variables: 
    CERT_INPUT_FILE=<full_path_to_cert_file>: the full file name path to your own certificate file, this is required
    KEY_INPUT_FILE=<full_path_to_key_file>: the full file name path to your own key file, this is required
    EXTRA_SNIS="comma_separated_server_name_list_if_any": an extra server name indicator list in addition to localhost and kong, this is optional and can be omitted
```

#### Get-consul-acl-token

```
get-consul-acl-token [options]
检索由以下指定的 Consul ACL 令牌：

Options:
    arm64:  Retrieves the Consul ACL token using ARM64 image
    dev:    Retrieves the Consul ACL token using local dev built docker image
            'make docker', which creates docker images tagged with '0.0.0-dev'
```

#### Compose

```
compose [options] 
生成由选项指定的 EdgeX compose file 并将它们存储在配置的发布文件夹中。 Compose 文件被适当地命名为发布和用于生成它们的选项。

Options:
    no-secty:   Generates non-secure compose file, otherwise generates secure compose file
    arm64:      Generates compose file using ARM64 images
    dev:        Generates compose file using local dev built images from edgex-go repo's 
                'make docker' which creates docker images tagged with '0.0.0-dev'    
    app-dev:    Generates compose file using local dev built images from app-service-configurable repo's
                'make docker' which creates docker images tagged with '0.0.0-dev'
    ds-bacnet:  Generates compose file with device-bacnet included
    ds-camera:  Generates compose file with device-camera included
    ds-grove:   Generates compose file with device-grove included (valid only with arm64 option)
    ds-modbus:   Generates compose file with device-modbus included
    ds-mqtt:     Generates compose file with device-mqtt included
    ds-rest:     Generates compose file with device-rest included
    ds-snmp:     Generates compose file with device-snmp included
    ds-virtual:  Generates compose file with device-virtual included
    ds-coap:     Generates compose file with device-coap included
    ds-gpio:     Generates compose file with device-gpio included
    ds-llrp:     Generates compose file with device-rfid-llrp included
    modbus-sim:  Generates compose file with ModBus simulator included
    asc-http:    Generates compose file with App Service HTTP Export included
    asc-mqtt:    Generates compose file with App Service MQTT Export included
    as-llrp:     Generates compose file with App RFID LLRP Inventory included
    mqtt-broker: Generates compose file with a MQTT Broker service included 
    mqtt-bus:    Generates compose file with services configure for MQTT Message Bus 
                 The MQTT Broker service is also included.
    zmq-bus:     Generates compose file with services configured for ZMQ Message Bus     
    ui:          Generates compose file with UI sevice included             
```

#### Taf-compose

```
taf-compose [options] 
生成选项指定的 TAF 常规测试 compose files，并将它们存储在配置的 TAF 文件夹中。 Compose 文件根据用于生成它们的选项进行了适当的命名。

Options:
    taf-secty:	  Generates general TAF testing compose file with security services
    taf-no-secty: Generates general TAF testing compose file without security services
    arm64:        Generates TAF compose file using ARM64 images
```

#### Taf-perf-compose

```
taf-perf-compose [options] 
生成选项指定的 TAF 性能测试撰写文件，并将它们存储在配置的 TAF 文件夹中。 Compose 文件根据用于生成它们的选项进行了适当的命名。

Options:
    taf-secty:	  Generates performance TAF testing compose file with security services
    taf-no-secty: Generates performance TAF testing compose file without security services
    arm64:        Generates TAF compose file using ARM64 images
```
