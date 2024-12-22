Here’s a list of commonly used Ansible commands along with example playbooks and scripts you can try out for learning and experimentation.

---

### **Common Ansible Commands**

1. **Ping all hosts in the inventory file**
   ```bash
   ansible all -i inventory.yml -m ping
   ansible all -m ping
   ```

2. **Execute a shell command on all hosts**
   ```bash
   ansible all -i inventory.yml -m shell -a "uptime"
   ansible all -m shell -a "uptime"
   ```

3. **Run a playbook**
   ```bash
   ansible-playbook -i inventory.yml playbook.yml
   ```

4. **Check syntax of a playbook**
   ```bash
   ansible-playbook playbook.yml --syntax-check
   ```

5. **Dry run a playbook**
   ```bash
   ansible-playbook -i inventory.yml playbook.yml --check
   ```

6. **List tasks in a playbook**
   ```bash
   ansible-playbook -i inventory.yml playbook.yml --list-tasks
   ```

7. **Gather facts about a host**
   ```bash
   ansible all -i inventory.yml -m setup
   ansible all -m setup
   ```

8. **Run a playbook on a specific host**
   ```bash
   ansible-playbook -i inventory.yml playbook.yml --limit "host_name"
   ```

---

### **Example Ansible Scripts**

#### **1. Check System Uptime**

Run a one-liner to check uptime across all hosts.

```bash
ansible all -i inventory.yml -m command -a "uptime"
```

---

#### **2. Update All Packages**

Playbook to update all packages on Amazon Linux.

```yaml
---
- name: Update all packages
  hosts: amazon_linux
  become: true
  tasks:
    - name: Update all packages to the latest version
      yum:
        name: "*"
        state: latest
```

Run it:
```bash
ansible-playbook -i inventory.yml update_packages.yml
```

---

#### **3. Install and Start Apache**

```yaml
---
- name: Install and configure Apache
  hosts: amazon_linux
  become: true
  tasks:
    - name: Install Apache
      yum:
        name: httpd
        state: present

    - name: Start and enable Apache
      service:
        name: httpd
        state: started
        enabled: true
```

Run it:
```bash
ansible-playbook -i inventory.yml install_apache.yml
```

---

#### **4. Create a New User**

```yaml
---
- name: Create a new user
  hosts: amazon_linux
  become: true
  tasks:
    - name: Add a user with sudo privileges
      user:
        name: ansible_user
        shell: /bin/bash
        groups: wheel
        state: present

    - name: Set up an SSH key for the new user
      authorized_key:
        user: ansible_user
        key: "ssh-rsa AAAA..."
        state: present
```

Run it:
```bash
ansible-playbook -i inventory.yml create_user.yml
```

---

#### **5. Configure a Basic Firewall**

```yaml
---
- name: Configure a firewall
  hosts: amazon_linux
  become: true
  tasks:
    - name: Install firewalld
      yum:
        name: firewalld
        state: present

    - name: Start and enable firewalld
      service:
        name: firewalld
        state: started
        enabled: true

    - name: Allow SSH and HTTP traffic
      firewalld:
        service: "{{ item }}"
        permanent: true
        state: enabled
      with_items:
        - ssh
        - http

    - name: Reload firewall rules
      command: firewall-cmd --reload
```

Run it:
```bash
ansible-playbook -i inventory.yml configure_firewall.yml
```

---

### **Example Inventory File**

```yaml
amazon_linux:
  hosts:
    ec2-3-12-45-67.compute-1.amazonaws.com:
      ansible_user: ec2-user
      ansible_ssh_private_key_file: /path/to/private-key.pem
```

---

### **Practice Commands**

1. **Test Connection**:
   ```bash
   ansible all -i inventory.yml -m ping
   ```

2. **Install Packages**:
   ```bash
   ansible all -i inventory.yml -m yum -a "name=git state=present"
   ```

3. **Copy a File to Remote Hosts**:
   ```bash
   ansible all -i inventory.yml -m copy -a "src=/path/to/local/file dest=/path/to/remote/file"
   ```

4. **Restart a Service**:
   ```bash
   ansible all -i inventory.yml -m service -a "name=nginx state=restarted"
   ```

---

These commands and playbooks are great starting points for working with Amazon Linux. Let me know if you’d like more advanced examples or guidance!
