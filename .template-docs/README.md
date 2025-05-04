# How to devcontainer for CCIT

First of all, `clone` this project or `fork` it

> [!NOTE]
> While you can technically use this guide for every
> service, it is recommended only for those you have the
> `source code` availale

## Prerequisites

- ***MAKE A COPY OF THE SERVICE***
- Have `docker` installed
- [Be sure that `docker` can be run by `non-root` user](https://docs.docker.com/engine/install/linux-postinstall/)
- Install this [Dev Container Extension](https://marketplace.visualstudio.com/items/?itemName=ms-vscode-remote.remote-containers)
- Copy this repo

> [!CAUTION]
> Once you fix here, you need to copy your source code
> back to the original service and run there
> `docker compose up --build`

## File Structure

```text
#  File structure
workspace-root/
 |-- .devcontainer/
 |      \- devcontainer.json
 |-- .template-docs/
 |      \- README.md
 |-- .vscode/
 |      \- launch.json
...
 |   #  Other files
...
 |-- docker-compose.yml
 \-- DOCKERFILE
```

## [`devcontainer.json`](./../.devcontainer/devcontainer.json)

- Put the `language` extension for the right `language`
    - Go to `extension`
    - Search for the `language`
    - Right-Click and choose `Copy Extension ID`
    - Paste it on the `extension` part of [`devcontainer.json`](./../.devcontainer/devcontainer.json)

    ```json
     "customizations": {
        "vscode": {
            "extensions": [
                //  Put it here
                "your-extension-id-here",
                "fabiospampinato.vscode-highlight",
                "fabiospampinato.vscode-todo-plus"
            ]
        }
    },
    ```

## Further steps

- Comment out the CMD instruction in [`DOCKERFILE`](./../DOCKERFILE):

    ```DOCKERFILE
    #  Comment out the line below
    CMD ["python", "app.py"]
    ```

- Add a command on the
[`docker-compose.yml`](./../docker-compose.yml)
to `sleep infinity`:

    ```yaml
    services:
        #  I'm assuming this is your service.
        #   Take note of:
        #       - service-name (e.g. web)
        #       - exposed port (e.g. 5001)
        web:
            build: .
            ports:
                - "5001:5000"
            volumes:
                - .:/app
            depends_on:
                - mongo
            environment:
                - FLASK_ENV=development
            networks:
                - viber_network
            #  Add this line below
            command: sleep infinity
    ```

- Remove any drivers from the `network` section in
    [`docker-compose.yml`](./../docker-compose.yml):

    ```yaml
    networks:
        viber_network:
            #  Comment out line below
            driver: bridge
    ```

- Open your firewall (e.g. `ufw`) for incoming connections
    on the port specified in
    [`docker-compose.yml`](./../docker-compose.yml)
