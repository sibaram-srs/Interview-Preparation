Below are concise, interview-ready answers for an **Azure DevOps Engineer (2–5 years experience)**.

---

# 1. Explain Azure Role-Based Access Control (RBAC)

**Answer:**
Azure RBAC (Role-Based Access Control) is Azure's authorization system used to control who can access Azure resources and what actions they can perform.

RBAC has three main components:

* **Security Principal** – User, Group, Service Principal, or Managed Identity.
* **Role Definition** – Collection of permissions (Owner, Contributor, Reader, etc.).
* **Scope** – Management Group, Subscription, Resource Group, or Resource.

**Example:**

* Reader → View resources only.
* Contributor → Create and manage resources but cannot assign roles.
* Owner → Full access including permission management.

---

# 2. Tell basic steps to give Contributor and Reader role

**Answer:**

1. Login to Azure Portal.
2. Go to the Resource Group/Subscription.
3. Select **Access Control (IAM)**.
4. Click **Add → Add Role Assignment**.
5. Select the role:

   * Contributor
   * Reader
6. Select User/Group/Service Principal.
7. Click **Review + Assign**.

---

# 3. How can I give user permission to files in Linux?

**Answer:**

Using **chmod**, **chown**, and **ACL**.

Examples:

Change ownership:

```bash
sudo chown user:user filename
```

Give permissions:

```bash
chmod 755 filename
```

Using ACL:

```bash
setfacl -m u:john:rwx file.txt
```

Check permissions:

```bash
ls -l
getfacl file.txt
```

---

# 4. What is Docker? Tell about Dockerfile.

**Answer:**

Docker is a containerization platform used to package an application with all its dependencies so it runs consistently across different environments.

**Benefits**

* Lightweight
* Fast deployment
* Portability
* Isolation
* Version control

### Dockerfile

Dockerfile is a text file containing instructions to build a Docker image.

Common instructions:

```dockerfile
FROM ubuntu:22.04

WORKDIR /app

COPY . .

RUN apt update

EXPOSE 8080

CMD ["python3","app.py"]
```

Common keywords:

* FROM
* RUN
* COPY
* ADD
* WORKDIR
* ENV
* EXPOSE
* CMD
* ENTRYPOINT

---

# 5. How can I give Owner permission to a user?

**Answer:**

1. Azure Portal
2. Subscription/Resource Group
3. Access Control (IAM)
4. Add Role Assignment
5. Select **Owner**
6. Select user
7. Assign

**Owner** can:

* Create resources
* Delete resources
* Assign RBAC permissions

---

# 6. Tell steps to build a VM

**Answer:**

1. Login Azure Portal
2. Create Resource Group
3. Create Virtual Network
4. Create Subnet
5. Create Network Security Group (NSG)
6. Click Create → Virtual Machine
7. Select Image (Ubuntu/Windows)
8. Choose VM Size
9. Configure Authentication
10. Attach Disk
11. Configure Networking
12. Review + Create
13. Connect using SSH/RDP

---

# 7. Which type of VM have you built?

**Sample Answer:**

"I have created Ubuntu Linux virtual machines for hosting Jenkins, Docker, and application deployments. I have also worked with Windows Server VMs for IIS applications. Most of my work involved Standard B-series and D-series VMs depending on project requirements."

---

# 8. Tell about Kubernetes and Kubernetes Cluster

**Answer:**

Kubernetes is an open-source container orchestration platform used to deploy, manage, scale, and monitor containerized applications.

### Kubernetes Cluster

A cluster consists of:

**Control Plane**

* API Server
* Scheduler
* Controller Manager
* etcd

**Worker Nodes**

* kubelet
* kube-proxy
* Container Runtime
* Pods

Benefits:

* Auto Scaling
* Self-Healing
* Load Balancing
* Rolling Updates
* High Availability

---

# 9. Explain Azure Pipelines and process

**Answer:**

Azure Pipelines is a CI/CD service in Azure DevOps used to automate build, testing, and deployment.

Pipeline Flow:

```
Developer
      ↓
Git Repository
      ↓
Trigger Pipeline
      ↓
Build
      ↓
Unit Testing
      ↓
Publish Artifact
      ↓
Release Pipeline
      ↓
Deploy to Dev
      ↓
QA
      ↓
UAT
      ↓
Production
```

Pipeline is usually defined using YAML.

---

# 10. Explain Terraform Lock

**Answer:**

Terraform Lock prevents multiple users from modifying the same infrastructure simultaneously.

When `terraform apply` starts:

* Terraform creates a lock.
* Other users cannot perform changes until the first operation finishes.
* Lock prevents state file corruption.

In Azure, the lock is managed when using an Azure Storage backend for the Terraform state.

---

# 11. Explain Terraform commands

**Answer:**

Initialize:

```bash
terraform init
```

Format:

```bash
terraform fmt
```

Validate:

```bash
terraform validate
```

Plan:

```bash
terraform plan
```

Apply:

```bash
terraform apply
```

Destroy:

```bash
terraform destroy
```

Show:

```bash
terraform show
```

List resources:

```bash
terraform state list
```

Refresh:

```bash
terraform refresh
```

Output:

```bash
terraform output
```

---

# 12. Share 10 Linux commands

```bash
pwd
```

Print current directory.

```bash
ls
```

List files.

```bash
cd
```

Change directory.

```bash
mkdir
```

Create directory.

```bash
rm
```

Remove files.

```bash
cp
```

Copy files.

```bash
mv
```

Move/Rename files.

```bash
cat
```

Display file content.

```bash
grep
```

Search text.

```bash
find
```

Search files.

Bonus:

```bash
top
df -h
free -m
ps -ef
systemctl status
journalctl
chmod
chown
```

---

# 13. Do we use terraform apply in pipelines?

**Answer:**

Yes.

In Azure DevOps pipelines we typically execute:

```
terraform init
terraform validate
terraform plan
```

After approval:

```
terraform apply -auto-approve
```

Usually, the `apply` stage is protected with approvals for production environments.

---

# 14. Why need to change company?

**Answer:**

"I'm grateful for my current organization because I gained valuable hands-on experience in Azure DevOps, CI/CD, Docker, Kubernetes, Terraform, and Azure services. Now I'm looking for a role where I can work on larger-scale cloud environments, learn advanced DevOps practices, and take on more challenging responsibilities while continuing to grow professionally."

---

# 15. If error occurs in Linux without any hardware or service issue, what will you check?

**Answer:**

I would check:

1. System logs

```bash
journalctl
```

2. Authentication logs

```bash
/var/log/auth.log
```

3. Syslog

```bash
/var/log/syslog
```

4. Disk space

```bash
df -h
```

5. Memory

```bash
free -m
```

6. CPU

```bash
top
```

7. Running processes

```bash
ps -ef
```

8. Network connectivity

```bash
ping
```

9. File permissions

```bash
ls -l
```

10. Recent changes made to the server.

---

# 16. Suppose error occurs in Docker, what will you check?

**Answer:**

My troubleshooting approach:

1. Check running containers.

```bash
docker ps -a
```

2. Check logs.

```bash
docker logs <container_id>
```

3. Inspect the container.

```bash
docker inspect <container_id>
```

4. Verify image.

```bash
docker images
```

5. Check Docker service.

```bash
systemctl status docker
```

6. Check port mapping.

```bash
docker port
```

7. Verify volume mounts.

8. Check network connectivity.

```bash
docker network ls
```

9. Review resource usage (CPU/Memory/Disk).

10. Restart the container if necessary.

---

# 17. Before DevOps what was the gap?

**Answer:**

"Before moving into DevOps, I focused on learning Linux, networking, cloud fundamentals, Git, scripting, Docker, Kubernetes, Terraform, and Azure. During that period, I completed training, practiced in lab environments, built hands-on projects, and prepared myself for a DevOps role."

*(If you had a different personal reason for the gap, tailor this answer accordingly.)*

---

# 18. How were things running before DevOps?

**Answer:**

Before DevOps:

* Development and Operations worked separately.
* Software releases were manual.
* Deployments were slow and error-prone.
* Infrastructure was manually provisioned.
* Communication between teams was limited.
* Rollbacks were difficult.
* Frequent production issues occurred.

DevOps improved this through:

* Automation
* CI/CD
* Infrastructure as Code
* Monitoring
* Collaboration
* Faster and more reliable releases

---

# 19. Why need to change company? (Alternative Answer)

**Answer:**

"I'm looking for a role that offers more opportunities to work on cloud-native technologies, large-scale Azure environments, Kubernetes, Terraform, and end-to-end CI/CD implementations. I value the experience I've gained in my current company, and I'm now seeking greater technical challenges, continuous learning, and long-term career growth."

**Interview Tip:** For a 2–5 year Azure DevOps role, answer confidently with practical examples from your experience. Interviewers often appreciate responses that explain not only *what* you did, but also *why* you chose a particular approach or tool.
