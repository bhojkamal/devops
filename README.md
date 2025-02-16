# Laravel Development Environment with Docker

This repository provides a boilerplate Docker setup for local development of PHP and Laravel applications. It includes containers for PHP, Composer, Node.js, and a web server (Nginx), allowing you to quickly spin up a consistent development environment.  For database and other services, you can leverage setups like the one provided here: [https://github.com/puncoz-official/dock](https://github.com/puncoz-official/dock)

This setup uses Docker Compose v2 (compose.yml).

## Prerequisites

*   Docker and Docker Compose installed on your system. See the official Docker installation guide for instructions: [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)

## Getting Started

1.  **Clone the Repository (or use your existing Laravel project):**

    ```bash
    git clone <repository_url>
    cd <your_project_directory>
    ```

    If you're using this with an existing Laravel project, copy the contents of this repository into your project's root directory.  Convert the `devops` folder to `.devops` and remove the `.git` directory.

2.  **Build the Docker Images:**

    ```bash
    docker compose build
    ```

    This command builds the Docker images defined in the `compose.yml` file. This might take some time the first time, as it downloads and installs the necessary dependencies.

3.  **Start the Containers:**

    ```bash
    docker compose up -d
    ```

    This command starts the Docker containers in detached mode (running in the background).

4.  **Access the Application:**

    Once the containers are running, you can access your Laravel application in your web browser at the address defined by your Traefik configuration (see the `compose.yml` file and the `traefik.http.routers.${DOCKER_NAME}.rule` label).  This will likely be something like `http://<your_domain>` (e.g., `http://example.com`), `http://<admin_domain>` (e.g., `http://admin.example.com`), or `http://<api_domain>` (e.g., `http://api.example.com`). If you're using a local setup, you'll likely need to configure your `/etc/hosts` file (or equivalent) to resolve these domains to your Docker host's IP address (often 127.0.0.1 or the IP of your Docker network, depending on your setup).

## Key Features and Components

*   **PHP:** Uses the latest stable PHP version (configurable in the Dockerfile).
*   **Laravel:** Ready for Laravel development.
*   **Composer:** Pre-installed for managing PHP dependencies.
*   **Node.js & npm/yarn:** Included for front-end development (configurable in the Dockerfile).
*   **Web Server (Nginx):** Serves the Laravel application.
*   **Database:**  Uses external database services (e.g., MySQL, PostgreSQL, MongoDB - as needed) configured separately (see the linked repository for examples).
*   **Traefik:**  Handles routing and reverse proxying for the application.  This allows you to use custom domains and potentially HTTPS.
*   **.env File:** Use a `.env` file at the root of your project to configure environment variables. This file should *not* be committed to version control.

## Usage

*   **Running Artisan Commands:**

    ```bash
    docker compose exec app php artisan <command>
    ```

    For example:

    ```bash
    docker compose exec app php artisan migrate
    docker compose exec app php artisan tinker
    ```

*   **Running Composer Commands:**

    ```bash
    docker compose exec app composer <command>
    ```

    For example:

    ```bash
    docker compose exec app composer install
    docker compose exec app composer require <package>
    ```

*   **Running npm/yarn Commands (if applicable):**

    ```bash
    docker compose exec app npm <command>  # or yarn <command>
    ```

*   **Stopping the Containers:**

    ```bash
    docker compose down
    ```

*   **Viewing Logs:**

    ```bash
    docker compose logs -f app  # Follow the logs of the 'app' container
    docker compose logs -f server # Follow the logs of the 'server' container
    ```

## `compose.yml` Configuration Details

Your `compose.yml` file uses external networks (`traefik_web`, `postgres`, `redis`, `mail`) for integration with other services.  You'll need to ensure these networks exist if you haven't already created them.  The example provided assumes you're using Traefik for routing and potentially other services like PostgreSQL, Redis, and a mail server.

copy compose.yml and dock to your root folder. You are ready to go

The `DOCKER_NAME`, `DOCKER_PUID`, `DOCKER_PGID`, `DOCKER_USER`, `BASE_DOMAIN`, `ADMIN_DOMAIN`, and `API_DOMAIN` environment variables are used for configuration.  You'll likely want to define these in a `.env` file or export them before running `docker compose`.

The `volumes` section mounts your project directory into the containers for development.  The `:cached` option is used for performance.

**Important:**  The Traefik labels in the `server` service are crucial for routing.  Make sure these are configured correctly for your domain setup.

## Contributing

Contributions are welcome!

## License

[MIT]