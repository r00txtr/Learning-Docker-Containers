Setting up a Docker Swarm cluster and deploying services involves several steps. Let's break down the process:

### 1. Set up a Docker Swarm cluster with multiple nodes:
To set up a Docker Swarm cluster, you need multiple Docker hosts running Docker Engine in swarm mode. You can use Docker Machine to create virtual machines for this purpose. Here's a basic overview of the steps:

1. Initialize a swarm on one of the nodes:
   ```bash
   docker swarm init --advertise-addr <manager-node-ip>
   ```

2. Join other nodes to the swarm:
   ```bash
   docker swarm join --token <token> <manager-node-ip>:2377
   ```

Replace `<manager-node-ip>` with the IP address of your manager node. You can get the token from the output of the `docker swarm init` command.

### 2. Deploy a service to the Swarm cluster and scale it across nodes:
Once you have a Docker Swarm cluster set up, you can deploy services to it. Here's an example of deploying a service:

```bash
docker service create --name myservice --replicas 3 -p 8080:80 myimage
```

This command creates a service named `myservice` with 3 replicas, exposing port 8080 on each node and routing traffic to port 80 of the service container.

### 3. Experiment with rolling updates and service rollback capabilities:
Docker Swarm provides rolling updates and service rollback capabilities out of the box. Here's how you can perform rolling updates:

1. Update the service:
   ```bash
   docker service update --image newimage myservice
   ```

2. Monitor the rolling update:
   ```bash
   docker service ps myservice
   ```

You'll see the status of each replica as it gets updated.

If something goes wrong during the update and you need to rollback, you can do so using the `--rollback` flag:

```bash
docker service update --rollback myservice
```

This will revert the service to the previous version.

That's it! You've set up a Docker Swarm cluster with multiple nodes, deployed a service to it, and experimented with rolling updates and service rollback capabilities. You can further explore Docker Swarm's features such as load balancing, automatic service scaling, and more.
