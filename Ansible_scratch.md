ssh 52.200.231.186 

ansible server:
ssh-keygen > copy the key id_rsa.pub

asible client:
ssh-keygen > cd /root/.ssh/ > paste the copied key to authorized_keys file

now test ssh 52.200.231.186 from ansible server machine

## Ansible ad-hoc commands
these are used for simpler tasks

#commands:
ansible -i inventory 52.200.231.186 -m "shell" -a "touch ansible.txt"
ansible -i inventory all -m "shell" -a "touch ansible.txt"

-m stands for module
-a used for passing the shell command


## Ansible playbooks
used for complex tasks

##inventory
list of target servers

## executing on group of servers
in the inventoryfile
[webservers]
192.78.89.100
192.78.89.101

[dbserveres]
192.78.89.102
192.78.89.103

now!!>> ansible -i inventory webservers -m"shell" -a "ls"

Learning Ansible playbooks involves understanding YAML syntax, modules, tasks, variables, loops, conditionals, and advanced features like roles and plugins. Here’s a guide from scratch to advanced:

---

### **1. Introduction to Ansible Playbooks**
Ansible playbooks are YAML files that define a series of tasks to be executed on managed nodes. Each playbook consists of one or more **plays**, which map hosts to tasks.

#### **Basic Syntax**
- YAML format: Indentation-sensitive, uses spaces (no tabs).
- Starts with `---` (YAML document start).
- Each play starts with `- name`.

---

### **2. Basic Structure**
```yaml
---
- name: Basic Playbook Example
  hosts: all  # Define the target hosts (inventory group or specific hosts)
  tasks:  # Define tasks within the play
    - name: Ensure Apache is installed
      yum:
        name: httpd
        state: present
```

---

### **3. Core Attributes**
#### **Play-Level Attributes**
1. **`hosts`**: Target group/host for the play (e.g., `all`, `webservers`).
2. **`become`**: Elevate privilege (e.g., `become: yes` for sudo).
3. **`vars`**: Define variables (inline or from files).
4. **`tasks`**: List of tasks for the play.
5. **`gather_facts`**: Collect host system facts (default: `yes`).

---

#### **Task-Level Attributes**
1. **`name`**: Description of the task (optional but recommended).
2. **`module_name`**: The Ansible module to use (e.g., `yum`, `copy`).
3. **`args`**: Parameters for the module.

Example:
```yaml
- name: Create a directory
  file:
    path: /tmp/example
    state: directory
    mode: '0755'
```

---

### **4. Variables**
- Defined inline, in separate YAML files, or via inventory files.
- Example:
```yaml
vars:
  app_name: MyApp
  app_port: 8080
```

Use in tasks:
```yaml
- name: Print application name
  debug:
    msg: "Application is {{ app_name }}"
```

---

### **5. Loops**
Run tasks multiple times with varying input.
```yaml
- name: Install multiple packages
  yum:
    name: "{{ item }}"
    state: present
  loop:
    - git
    - vim
    - curl
```

---

### **6. Conditionals**
Execute tasks based on conditions.
```yaml
- name: Install Apache if on RHEL
  yum:
    name: httpd
    state: present
  when: ansible_os_family == "RedHat"
```

---

### **7. Handlers**
Used to trigger tasks after changes.
```yaml
tasks:
  - name: Start Apache
    service:
      name: httpd
      state: restarted
    notify: Restart Apache

handlers:
  - name: Restart Apache
    service:
      name: httpd
      state: restarted
```

---

### **8. Advanced Features**
#### **Roles**
Organize playbooks into reusable components.
- Folder structure:
  ```
  roles/
    webserver/
      tasks/
      templates/
      vars/
  ```

#### **Include/Import**
Include other playbooks or tasks.
```yaml
- import_tasks: common.yml
```

#### **Error Handling**
- **`ignore_errors`**: Ignore failure.
- **`rescue`** and **`always`**: Handle failure tasks.
```yaml
tasks:
  - name: Attempt risky command
    command: /bin/false
    ignore_errors: yes
```

#### **Vault**
Encrypt sensitive data.
```bash
ansible-vault encrypt secrets.yml
```

---

### **9. Debugging and Testing**
Use `debug` module for output and `check` mode for dry runs.
```yaml
- name: Debug variable
  debug:
    var: app_name
```

---

### **10. Best Practices**
1. Use meaningful names.
2. Avoid hardcoding; use variables.
3. Group related tasks into roles.
4. Keep playbooks modular.

