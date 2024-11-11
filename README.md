# helm-charts



## Step-01: Introduction
- Host Helm Repository on GitHub




## Step-02: Create GitHub Repository
- **Repository Name:** helm-charts
- **Description:** Helm Charts Repository on GitHub
- **Repository Type:** Public
- **Initialize this repository with:** CHECK Add a README file
- Click on **Create repository**




## Step-03: Create gh-pages branch
- **New branch name:** gh-pages
- **Source:** main
- Click on **Create new branch**




## Step-04: Enable GitHub Pages for gh-pages branch (if not enabled by default)
- Go to repository: helm-charts -> Settings -> Code and Automation -> Pages
- Review **Branch**
- Also access the GitHub pages site
- https://nileshzarkar.github.io/helm-charts/




## Step-05: Clone the GitHub Repository local desktop
```t
# Clone the GitHub Repository
git clone https://github.com/nileshzarkar/helm-charts.git
```




## Step-06: Review and Copy GitRepo Files
```t
# Change Directory
cd D:\Experiments\helm-experiments\helm-experiments\helm-charts

# Copy content from gitrepo-content to helm-charts-repo
1. .github folder: contains GitHub Actions release.yaml
Copy the folder 
https://github.com/nileshzarkar/helm-experiments/tree/main/helm-masterclass/helm-masterclass/44-Helm-Repo-on-GitHub/gitrepo-content/.github/
to 
D:\Experiments\helm-experiments\helm-experiments\helm-charts

niles@Nilesh MINGW64 /d/Experiments/helm-experiments/helm-experiments/helm-charts/.github/workflows (main)
-rw-r--r-- 1 niles 197121 710 Nov 10 11:34 release.yml


2. charts folder: Contains "html-page-1.0.0" helm chart

Copy the htmlpage chart content from  
D:\Experiments\helm-experiments\helm-experiments\html-page-1.0.0\htmlpage
to 
D:\Experiments\helm-experiments\helm-experiments\helm-charts\charts\htmlpagechart

niles@Nilesh MINGW64 /d/Experiments/helm-experiments/helm-experiments/helm-charts (main)
drwxr-xr-x 1 niles 197121     0 Nov 10 14:34 ../
drwxr-xr-x 1 niles 197121     0 Nov 10 14:34 .git/
drwxr-xr-x 1 niles 197121     0 Nov 10 14:36 .github/
drwxr-xr-x 1 niles 197121     0 Nov 10 14:36 charts/
drwxr-xr-x 1 niles 197121     0 Nov 10 14:36 ./
-rw-r--r-- 1 niles 197121  7513 Nov 10 14:40 README.md

niles@Nilesh MINGW64 /d/Experiments/helm-experiments/helm-experiments/helm-charts/charts/htmlpagechart (main)
-rw-r--r-- 1 niles 197121 2378 Nov 10 13:12 values.yaml
-rw-r--r-- 1 niles 197121  124 Nov 10 13:12 Chart.yaml
drwxr-xr-x 1 niles 197121    0 Nov 10 14:36 templates/


Chart Releaser is tool designed to helm github repos self-host their own chart repos by adding helm chart artifacts to github releases named for the chart version and then creating an index.yaml file for those releases that can be hosted on github pages (or else where)

```




## Step-07: Create Chart Release 0.1.0
### Step-07-01: Verify Chart.yaml
- Ensure we have the `appVersion: "1.0.0"` and `version: 1.0.0`
```yaml

Chart.yaml
apiVersion: v2
appVersion: "1.0.0"
description: Signed Charts
name: htmlpage
type: application
version: 1.0.0

values.yaml
...
image:
  repository: nileshzarkar/htmlpage
  pullPolicy: Always
  tag: "1.0.0"

serviceAccount:
  create: false

service:
  type: NodePort
  port: 8080
  targetPort: 8080
  nodePort: 30082

config:
  pageColor: "red"
...

service.yaml
...
spec:
  type: {{ .Values.service.type }}
  ports:
    - port: {{ .Values.service.port }}
      targetPort: {{ .Values.service.targetPort }}
      nodePort: {{ .Values.service.nodePort }}
      protocol: TCP
      name: http
...

deployment.yaml
...
          env:
            - name: page.color
              value: {{ .Values.config.pageColor }}
...

```



### Step-07-02: Check-in Code to Remote GitHub Repo
```t
# Check-in Code
D:\Experiments\helm-experiments\helm-experiments\helm-charts>git add .
D:\Experiments\helm-experiments\helm-experiments\helm-charts>git commit -m "1.0.0"
D:\Experiments\helm-experiments\helm-experiments\helm-charts>git push
```



### Step-07-03: Verify Actions in GitHub Repo
- Go to helm-charts-repo -> Actions
- Review workflow runs
  - 1.0.0
  - pages build and deployment
  - https://github.com/nileshzarkar/helm-charts/actions




### Step-07-04: Switch to gh-pages branch and verify index.yaml
- Switch to `gh-pages` and review `index.yaml`
- https://github.com/nileshzarkar/helm-charts/tree/gh-pages




### Step-07-05: Verify Releases and Tags
- Go to **Releases** and verify 
- https://github.com/nileshzarkar/helm-charts/releases
- Go to **Tags** and verify
- https://github.com/nileshzarkar/helm-charts/tags




## Step-08: Create Chart Release 0.2.0
### Step-08-01: Update Chart.yaml version
```t
# Update Chart.yaml
version: 0.2.0
appVersion: "0.2.0"
```
### Step-08-02: Check-in Code to Remote GitHub Repo
```t
# Check-in Code
git add .
git commit -am "0.2.0 commit"
git push
```
### Step-08-03: Verify Actions in GitHub Repo
- Go to helm-charts-repo -> Actions
- Review workflow runs
  - 0.1.0 commit
  - pages build and deployment
### Step-08-04: Switch to gh-pages branch and verify index.yaml
- Switch to `gh-pages` and review `index.yaml`
- https://github.com/stacksimplify/helm-charts/blob/gh-pages/index.yaml
### Step-08-05: Verify Releases and Tags
- Go to **Releases** and verify 
- Go to **Tags** and verify




## Step-09: Add GitHub Helm Repo in local desktop and Search Repo
```t
# Helm Repo URL
https://nileshzarkar.github.io/helm-charts/

# List Helm Repo
helm repo list

# Add Helm Repo
D:\Experiments\helm-experiments\helm-experiments\helm-charts>helm repo add helm-repo https://nileshzarkar.github.io/helm-charts/
"htmlrepo" has been added to your repositories
# List Helm Repo
helm repo list
# Helm Search Repo
PS D:\Experiments\helm-experiments\helm-experiments\helm-charts> helm search repo helm-repo/htmlpage
NAME                    CHART VERSION   APP VERSION     DESCRIPTION
helm-repo/htmlpage      1.0.0           1.0.0           A Helm chart for Kubernetes

D:\Experiments\helm-experiments\helm-experiments\helm-charts>helm repo update helm-repo

# Helm Search Repo with --versions
PS D:\Experiments\helm-experiments\helm-experiments\helm-charts> helm search repo helm-repo/htmlpage --versions
NAME                    CHART VERSION   APP VERSION     DESCRIPTION
helm-repo/htmlpage      4.0.0           4.0.0           A Helm chart for Kubernetes
helm-repo/htmlpage      3.0.0           3.0.0           A Helm chart for Kubernetes
helm-repo/htmlpage      2.0.0           2.0.0           A Helm chart for Kubernetes
helm-repo/htmlpage      1.0.0           1.0.0           A Helm chart for Kubernetes
```




## Step-10: Deploy and Verify from GitHub Helm Repo
```t
# Helm Install

D:\Experiments\helm-experiments\helm-experiments\helm-charts>helm repo update helm-repo

D:\Experiments\helm-experiments\helm-experiments\helm-charts>helm install htmlpage helm-repo/htmlpage --version 1.0.0 --atomic
or
D:\Experiments\helm-experiments\helm-experiments\helm-charts>helm install htmlpage helm-repo/htmlpage --atomic
NAME: htmlpage
LAST DEPLOYED: Sun Nov 10 12:14:47 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services htmlpage)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT

# Helm Status
helm status htmlpage --show-resources


helm install htmlpage helm-repo/htmlpage --version 1.0.0 --atomic
helm install htmlpage helm-repo/htmlpage --version 2.0.0 --atomic
helm install htmlpage helm-repo/htmlpage --version 3.0.0 --atomic
helm install htmlpage helm-repo/htmlpage --version 4.0.0 --atomic

# Access Application
http://localhost:30082/page

# Helm Uninstall
helm uninstall htmlpage
```
