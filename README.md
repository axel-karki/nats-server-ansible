## NATS Messaging System Setup

### What is NATS?

**NATS** is a high-performance messaging system for cloud-native applications, edge computing, and IoT. It enables microservices to communicate using publish/subscribe (pub/sub), request/reply, and streaming patterns. It is lightweight, easy to deploy, and designed for low-latency, scalable messaging.

---

### Pub/Sub in NATS

* **Publisher (Pub)**: Sends messages to a **subject**.
* **Subscriber (Sub)**: Listens to messages from a specific **subject**.
* Subscribers receive messages in real time from the publishers as long as they are subscribed to the subject.

---

### What is JetStream?

**JetStream** extends NATS with persistence, message replay, at-least-once delivery, and stream-based storage. It is suitable for scenarios where message durability and replay are needed, such as auditing, caching, or system state recovery.

---

## System Architecture

* **3 Virtual Machines (VMs)**

  * One VM runs the **NATS server**
  * One VM runs the **Publisher**
  * One VM runs the **Subscriber**
* The **Publisher VM** sends messages to the **Subscriber VM** via the **NATS server**.

<img width="288" alt="Screenshot 2025-05-26 at 6 19 57 AM" src="https://github.com/user-attachments/assets/07445b1b-c1be-469b-bd0c-3da20e94db46" />

---

## Playbook Execution Order

### 1. Install NATS Server

**Playbook**: `playbooks/main.yml`

* Installs the NATS server
* Configures JetStream
* Uses the following Jinja2 templates:

  * `templates/nats-server.conf.j2`: JetStream and server configuration
  * `templates/nats-server.service.j2`: systemd service file

---

### 2. Create Subscriber

**Playbook**: `playbooks/create_sub.yml`

* Installs NATS CLI client to subscribe to a subject

---

### 3. Create Publisher

**Playbook**: `playbooks/create_pub.yml`

* Installs NATS CLI client to publish messages

---

## Configuration Security

Edit the password in:

```bash
roles/nats/vars/main.yml
```

Encrypt the file using Ansible Vault for secure credential storage:

```bash
ansible-vault encrypt roles/nats/vars/main.yml
```

To edit later:

```bash
ansible-vault edit roles/nats/vars/main.yml
```

To run a playbook with the vault:

```bash
ansible-playbook playbooks/main.yml --ask-vault-pass
```

Or use a vault password file:

```bash
ansible-playbook playbooks/main.yml --vault-password-file ~/.vault_pass.txt
```

### UI
```
ssh -i <ssh_file_path> -L 8080:<nats_vm_server_private_ip>:8222 root@<server_public_ip>
```
