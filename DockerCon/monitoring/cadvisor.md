# Google cAdvisor (Container Asvisor)

<img src="https://raw.githubusercontent.com/56kcloud/Training/master/img/cadvisor-logo.png" alt="cAdvisor Logo" width="150" height="99"> 

[cAdvisor](https://hub.docker.com/r/google/cadvisor/) is an amazing tool. It is an all-in-one tool for grabbing real-time metrics from all the containers running on a single host and exposing these metrics. The cAdvisor collects, aggregates, processes, and exports usaage and performance information from running containers.

In this section we will deploy a cAdvisor container and walk through the UI, configurations, and take a look at the metrics which it exposes.

> **Tasks**:
>
>
> * [Task 1: Deploy cAdvisor](#Task_1)
> * [Task 2: Tour the cAdvisor UI and configurations](#Task_2)
> * [Task 3: cAdvisor Exposed Metrics](#Task_3)

## <a name="Task_1"></a>Task 1: Deploy cAdvisor

Let's get started with deploying cAdvisor. First, we will review the cAdvisor GitHub Repos. The repo contains a wealth of documentation from using the UI to configurations. Take some time and dive into the different aspects of cAdvisor.

> We recommend using [Play-with-Docker](https://labs.play-with-docker.com/) for this exercise to alleviate any permissions issues or Windows issues with having to run sudo.


1. Clone the Voting App Repo

    ```
    $ git clone https://github.com/dockersamples/example-voting-app.git
    ```

1. Enable Docker Swarm the Voting App with docker-compose.

    ```
    $ cd example-voting-app

    $ docker swarm init --advertise-addr $(hostname -i)

    $ docker stack deploy -c docker-stack.yml vote
    
    ```

3. Navigate to [cAdvisor GitHub Repo](https://github.com/google/cadvisor)


4. Deploy cAdvisor

    ```
    sudo docker run \
    --volume=/:/rootfs:ro \
    --volume=/var/run:/var/run:rw \
    --volume=/sys:/sys:ro \
    --volume=/var/lib/docker/:/var/lib/docker:ro \
    --publish=8081:8080 \
    --detach=true \
    --name=cadvisor \
    google/cadvisor:latest
    ```
    
    > Notice how we need to run cAdvisor with Sudo permissions as cAdvisor needs access to the rootfs & docker daemon
    > Also, I changed the default port of 8080 to 8081 so we don't conflict with the vote stack

### <a name="Task_2"></a>Task 2: Tour the cAdvisor UI and configurations

1. Once the cAdvisor is running open the `PWD` link 8081 which is located at the top of the screen

Have a look at the cAdvisor UI. What we see here is a host performance view. Scroll through the various screens to view Reservations, CPU, Memory, Network, and Storage.

2. Scroll to the top of the screen and click `Docker Containers`

> Here we see all the `Vote Stack` containers. Choose one of the containers and view the specific resource consumption of just that particular container.


### <a name="Task_3"></a>Task 3: cAdvisor Exposed Metrics

Now, we will have a look to see how cAdvisor exposes the metrics it is gathering from running containers. As default, cAdvisor exposes metrics in the [Prometheus format](https://prometheus.io/docs/instrumenting/writing_exporters/)

## Next Steps, Docker Networking
For the next step in the tutorial, head over to [Prometheus](./monitoring-stack.md)
