# Deploy your first capsule

Now that you have a configured Rig server up and running, and you have installed the CLI, you are ready to deploy your first capsule. This will be a short introduction to capsules, and how to deploy a containerized application to Rig using the CLI.

## What is a capsule?
In Rig the concept of a capsule encapsulates (😉) a collection of resources on Rig, that is used to manage and run an application.
Capsules contain builds, which hold information on how to deploy the application: which container image to use, git repository, etc.
Capsules then contain rollouts, which are deployments of a build to a specific environment and network network configuration. Rollouts are immutable and are used to manage the lifecycle of the application. This means that when the deployed build is updated, the number of replicas is changed or when a different environment or network configuration is used, a new rollout is automatically created.

## Deploy Nginx in a Rig Capsule
This guide will take you through the process of deploying an Nginx Server to Rig using the CLI. 
If you did not create a project in the [CLI setup guide](/getting-started/cli-setup), you can create one now by running:
    
```bash
rig project create --name my-first-project --use
```

Using the CLI, we can create a capsule by running the following command:

### Create a capsule
```bash
rig capsule create --name nginx-capsule
```

This will create an empty capsule called `nginx-capsule` with no additional resources. We can verify that the capsule was created by running:

```bash
rig capsule list
```

This will list all capsules in the current project, where you should see the capsule you just created.

### Create a build
Next, we need to create a build with the Nginx image for the capsule. This is done using the following command:

```bash
rig capsule create-build nginx-capsule --image nginx:latest
```

From the command we should see an output similar to: `Image available as build <build-id>`

We can verify that the build was actually create by running:

```bash
rig capsule list-builds nginx-capsule
```

This will list all builds for the capsule, where you should see the build you just created.

### Deploy the build
Now that we have a build, we can deploy it in the nginx-capsule. This is done using the command:

```bash
rig capsule deploy nginx-capsule --build-id <build-id>
```

Where `<build-id>` is the id of the build you just created. This will create a rollout for the build, and deploy it with the default configuration. We can verify that the rollout was create by running:

```bash
rig capsule list-rollouts nginx-capsule
```

This will list all rollouts for the capsule, where you should see one for the deployment you just initiated. 

if you run Rig in Kubernetes, you can run
```bash
kubectl get pods -n <project-id>
```
to see the Nginx pod running. 

Alternatively if you run Rig in Docker, you can run 
```bash
docker ps
```
to see the Nginx container running. 

### Scale the capsule
Now that we have a running deployment, we can scale the number of replicas. This is done using the command:

```bash
rig capsule scale nginx-capsule --replicas 3
```

This will create a new rollout with the updated number of replicas. In order to verify this, you can run the previous commands to see the new changes reflected in the rollout and the pods/containers.

### Configure the network
In order to expose the Nginx server to the public internet, we need to configure the network. This is done by creating a `network.yaml` file with the for example following content:

```yaml
interfaces:
  - name: http
    port: 80
    public:
      enabled: true
      method:
        load_balancer:
          port: 8081
```

and then running the following command:

```bash
rig capsule configure-network nginx-capsule network.yaml
```

This will create a new rollout with the updated network configuration. Now open your favorite browser and navigate to [http://localhost:8081](http://localhost:8081) to see the Nginx server running. Well done, you have created and deployed and exposed your first capsule to Rig! 🎉

## Shortcut
The above guide executed every step of the process in each command explicitly. However, the CLI provides a shortcut for created, deploying and configuring the capsule in one command. This above could be achieved by running the following command:

```bash
rig capsule create --name nginx-capsule --interactive
```

Which will take you through the process of creating a capsule, adding an image, configuring the network, setting the number of replicas and then deploying the capsule.