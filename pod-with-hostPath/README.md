

```
Create an index.html file on your Node
Open a shell to the single Node in your cluster. How you open a shell depends on how you set up your cluster. For example, if you are using Minikube, you can open a shell to your Node by entering minikube ssh.

In your shell on that Node, create a /mnt/data directory:

# This assumes that your Node uses "sudo" to run commands
# as the superuser
sudo mkdir /mnt/data
In the /mnt/data directory, create an index.html file:

# This again assumes that your Node uses "sudo" to run commands
# as the superuser
sudo sh -c "echo 'Hello from Kubernetes storage' > /mnt/data/index.html"
Note: If your Node uses a tool for superuser access other than sudo, you can usually make this work if you replace sudo with the name of the other tool.
Test that the index.html file exists:

cat /mnt/data/index.html
The output should be:

Hello from Kubernetes storage
You can now close the shell to your Node.
```

#### only work with single node cluster
