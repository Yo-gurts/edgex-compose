## Ireland Release Docker Compose Files
此分支包含用于拉取和运行`EdgeX Ireland`版本镜像的 docker compose file。

这些compose file是从`compose-builder`文件夹中的多个文件中自动生成的。具体请看[README](https://github.com/edgexfoundry/edgex-compose/tree/ireland/compose-builder#readme_zh)

### Compose Files
本文件夹包含以下 compose files：

#### Generated Compose files

> ** 注意： **
> - *做永久性的修改时不要直接编辑这些文件。所有永久性的修改都应该直接作用于`compose-builder` 文件夹中的源文件上，再重新生成当前目录下的文件。*
> - 在`compose-builder`中使用`make build` 来重新生成所有下面的 compose files。
> - 参阅下面的说明以了解如何通过`make`命令方便地使用这些compose files。

- **docker-compose.yml**
    包含在安全配置下运行所有需要的服务，包括设备服务 Device Virtual 和 Device REST.

    **Make Commands** 
    
     - 使用 `make run <service(s)>` 和 `make down` 来启动和停止`docker-compose.yml`中的服务。
    
     - 使用 `make pull <service(s)>` 来拉取`docker-compose.yml`中的全部或者部分服务的镜像。
    
     - 使用 `make get-token` 生成一个 Kong access token，用于远程接入通过此compose file运行的服务。
    
- **docker-compose-arm64.yml**
    包含在 `ARM64` 系统中，非安全配置下运行所需的全部服务，包括设备服务 Device Virtual 和 Device REST.

    **Make Commands** 
    
     - Use `make run arm64` and `make down` to start and stop the services using this compose file.
     - Use `make pull arm64 <service(s)>` to pull all or some images for the services in this compose file.
     - Use `make get-token arm64` to generate a Kong access token for remote access of the services running from this compose file.
    
- **docker-compose-no-secty.yml**
    仅包含在非安全配置下运行所需要的服务，包括设备服务 Device Virtual 和 Device REST.

    **Make Commands**

    - Use `make run no-secty` and `make down` to start and stop the services using this compose file.
    - Use `make pull no-secty <service(s)>` to pull all or some images for the services in this compose file.
    
- **docker-compose-no-secty-arm64.yml**
    仅包含在 `ARM64` 系统中，非安全配置下运行所需要的服务，包括设备服务 Device Virtual 和 Device REST.

    **Make Commands**

    - Use `make run no-secty arm64` and `make down` to start and stop the services using this compose file.
    - Use `make pull no-secty arm64 <service(s)>` to pull all or some images for the services in this compose file.


- **docker-compose-no-secty-with-ui.yml**
  仅包含在非安全配置下运行所需要的服务 + EdgeX UI，包括设备服务 Device Virtual 和 Device REST.

  **Make Commands**

  - Use `make run no-secty ui` and `make down` to start and stop the services using this compose file.
  - Use `make pull no-secty ui <service(s)>` to pull all or some images for the services in this compose file.

- **docker-compose-no-secty-with-ui-arm64.yml**
  仅包含在 `ARM64` 系统中，非安全配置下运行所需要的服务 + EdgeX UI，包括设备服务 Device Virtual 和 Device REST.

  **Make Commands**

  - Use `make run no-secty ui arm64` and `make down` to start and stop the services using this compose file.
  - Use `make pull no-secty ui arm64 <service(s)>` to pull all or some images for the services in this compose file.

### TAF Compose files

在子文件夹`taf`中的compose file是用于自动 TAF 测试，这些文件同样是在`compose-builder`中使用`make build`命令时自动生成的。

### Additional make commands

- `make clean`

    Runs `down` commands, removes all stopped container and prunes all unused volumes and networks. Use this command when needing to do a fresh restart.
    
- `make get-token`
    For secure mode only. Runs commands via docker to generate a new API Gateway token.

- `make get-consul-acl-token`
  For secure mode only. Runs commands via docker to retrieve a Consul Access token.

### Additional compose files

- **docker-compose-portainer.yml**
    Stand-alone compose file for running Portainer which is a  Docker container management tool. Visit here https://www.portainer.io/ for more details on Portianer.
    Use `make portainer`and `make portainer-down` to start and stop Portainer.
