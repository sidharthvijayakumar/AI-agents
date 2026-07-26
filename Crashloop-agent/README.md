# AI-agents
Basic setup needed:

Kind cluster
---
Create the kind cluster with this config
```bash
kind create cluster --config=kind-config.yaml
```
Use this config file to create the cluster
```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
networking:
  apiServerAddress: "127.0.0.1"
  apiServerPort: 50200
kubeadmConfigPatches:
  - |
    kind: ClusterConfiguration
    apiServer:
      certSANs:
        - localhost
        - 127.0.0.1
        - host.docker.internal
        - kind-control-plane
```
---
2. Create the config for mcp container
```bash
kind get kubeconfig --name kind > ~/.kube/config_mcp
```
Update the server with server: https://kind-control-plane:6443

---
3. Create config for running kubectl
```bash
kind get kubeconfig --name kind > ~/.kube/config
```
---
4. Now start the mcp server from vscode (cmd+shift+p-> MCP: List servers)
Select kubernetes mcp and start the server
---
5. Now config if you are able to run kubectl commands from mcp kubernetes container
```bash
docker exec -it $(docker ps -q --filter ancestor=mcp/kubernetes) bash
```
---
You will then need to open codex chat(cmd+shift+p -> select copilot chat):

Give the below prompt:
```text
Use crashloop-agent.md file and then analyse what are the pods which are crashing and give me suggestions to fix. Create a new file called summary.md which says the fix. Use mcp to connect to cluster
```