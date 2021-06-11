# Containerizing an Angular app featuring Nginx

- To start ensure that you clone the repo to get a basic working Angular App

- Install the dependencies of the application by running

  ```bash
  npm install
  ```

  at the project root.

- In the root of the project, create a `Dockerfile`.

- In the `Dockerfile`, add the following:

  ```bash
    # Stage 1

    FROM node:14.17.0-alpine as build-step

    RUN mkdir -p /app

    WORKDIR /app

    COPY package.json /app

    RUN npm install

    COPY . /app

    RUN npm run  build --prod


    # Stage 2
    From nginx:1.20.1

    COPY --from=build-step /app/dist/ng-docker-example /usr/share/nginx/html

    EXPOSE 4200:80
  ```

  From above:

  - First, we are getting a `node.js` image from DockerHub version 14.17.0.

  - Then, we are making a directory called app.

  - Transitioning to the app folder.

  - Copying the `package.json` file from our project folder to the app folder.

  - Inside the app folder, we are installing the dependencies by running `npm install`.

  - Copying the other contents of the project folder to the app folder.

  - Building the project in the app folder.

  - Getting an `nginx` image from Dockerhub version 1.20.1.

  - Copying all the build contents to the configured `nginx` html folder.

  - Exposing `4200` as our container port and `80` as our host port.

- At the root of the project, also create a `docker-compose.yml` file.

- Add the following contents to the file:

  ```bash
  version: "3.7"
  services:
    angular-service:
       container_name: ng-docker-example
       build: .
       ports:
           - "4200:80"
  ```

  From above:

  - The `version` is the version we will use for the compose file.

  - In the `services`, we define our application's services.

  - For this case, we have `angular-service` but feel free to rename it to any name.

  - In the service, we specify the following:

        - The container name: This is the preferred name of our container.

        - The build: This is the folder location of the `Dockerfile`.

        - The ports: This is the exposed combination of `container port` and `host port`

- From your terminal, build your image by running:

  ```bash
  docker-compose build <name_of_your_service>
  ```

  Remember to insert the name of the service in the `docker-compose.yml` file. For example: `angular-service`.

- After all the steps are finalized, then run the following command:

  ```bash
  docker-compose up
  ```

  The above command will start all the services in the `docker-compose.yml` file.

- Access the application from your browser by keying: `http://localhost:4200`.

- In your terminal, if you can see your image by running:

  ```bash
  docker images
  ```

- With the image all set, you can deploy it to Dockerhub or share it with teammates.

Happy Hacking!!
