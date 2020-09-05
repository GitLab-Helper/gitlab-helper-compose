# Gitlab Helper Deployment

This project is responsible for the deployment of the Gitlab Helper application.

Due to the current development status, the only available docker tag for images is `development`.  

The containers are available on the Docker Hub platform:  
Frontend: https://hub.docker.com/repository/docker/adrixop95/gitlab-helper-frontend  
Backend: https://hub.docker.com/repository/docker/adrixop95/gitlab-helper-backend  

The current demo of the application is available at:  
https://gitlabhelper.adrox.xyz/

## Docker-compose
To run the application using docker-compose, go to the `docker-compose` folder and then execute the following commands:  
```
$ URL="localhost" docker-compose -f docker-compose.dev.yml pull
$ URL="localhost" docker-compose -f docker-compose.dev.yml up -d
```

Parameters:
```
- URL - defines the address at which the application is to be launched without specifying the http protocol
```

Traefik automatically manages the SSL certificate generating the let's encrypt certificate. The certificate is generated for `example@example.com`, please change if necessary.  

Docker-compose stack has additional services configured, `amir20/dozzle` available under the prefix `/logs` to facilitate viewing docker logs and a `containrrr/watchtower` to automatically update available images.  

Without changing the configuration and parameters, the application will be available at: `https://localhost`.

## Kubernetes
Running the application using kubernetes requires having installed helm and installing a traefic with it.  
Installation of the traefik can be performed using the following commands:
```
$ helm repo add traefik https://containous.github.io/traefik-helm-chart
$ helm repo update
$ helm install traefik traefik/traefik
```

Then go to the `kubernetes` folder and execute the following commands:
```
$ helm upgrade traefik traefik/traefik --values 000-values.yaml
$ kubectl apply -f 001-deployment-development.yaml
```

Traefik automatically manages the SSL certificate generating the let's encrypt certificate. The certificate is generated for `example@example.com`, please change if necessary.  


In order for the application to be launched on an address other than `localhost`, localhost addresses should be changed in the `001-deployment-development.yaml` file to the target address of your choice.

Without changing the configuration and parameters, the application will be available at: `https://localhost`.