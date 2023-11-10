#### Communication among containers
- use the default bridge network
  - each container is assigned an IP
  - container can talk to each other using the IP address
  - `docker inspect <container_name> | grep IPAddress`
- user-defined bridge
  - To let Docker containers communicate with each other by name, you can create a user-defined bridge network. 
  - In a user-defined bridge network, you can be more explicit about who joins the network, and you get an added bonus.
  - In a user-defined bridge network, you control which containers are in the network, and they can address each other by hostname
- [host network is only supported in Linux](https://docs.docker.com/network/network-tutorial-host/)
  - The goal of this tutorial is to start a nginx container which binds directly to port 80 on the Docker host. From a networking point of view, this is the same level of isolation as if the nginx process were running directly on the Docker host and not in a container. However, in all other ways, such as storage, process namespace, and user namespace, the nginx process is isolated from the host.