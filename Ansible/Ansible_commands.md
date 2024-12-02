```
ssh 52.200.231.186 

ansible server:
ssh-keygen > copy the key id_rsa.pub

asible client:
ssh-keygen > cd /root/.ssh/ > paste the copied key to authorized_keys file
```
now test ssh 52.200.231.186 from ansible server machine

## Ansible ad-hoc commands
these are used for simpler tasks

#commands:
```
ansible -i inventory 52.200.231.186 -m "shell" -a "touch ansible.txt"
ansible -i inventory all -m "shell" -a "touch ansible.txt"
```
-m stands for module
-a used for passing the shell command


## Ansible playbooks
used for complex tasks

##inventory
list of target servers

## executing on group of servers
in the inventoryfile
```
[webservers]
192.78.89.100
192.78.89.101

[dbserveres]
192.78.89.102
192.78.89.103
```
now!!
 ```
 ansible -i inventory webservers -m"shell" -a "ls"
```

### **Playbook Keywords:** https://docs.ansible.com/ansible/latest/reference_appendices/playbooks_keywords.html#playbook-keywords

### **Ansible Documentation :** https://docs.ansible.com/

Here is a comprehensive list of Ansible playbook keywords, categorized by their usage. These are essential for understanding and writing effective playbooks.

---

### **Play-Level Keywords**
These are used to define the scope and execution parameters of a play.

| **Keyword**       | **Description**                                             |
|--------------------|-------------------------------------------------------------|
| `hosts`           | Specifies the target group or hosts for the play.           |
| `remote_user`     | User account for connecting to the hosts.                   |
| `become`          | Elevates privileges (e.g., `sudo`).                         |
| `become_user`     | User account to switch to with `become`.                    |
| `vars`            | Defines variables for the play.                             |
| `vars_files`      | Includes external YAML files containing variables.          |
| `gather_facts`    | Enables/disables automatic fact gathering.                  |
| `tasks`           | List of tasks to execute.                                   |
| `handlers`        | Special tasks triggered by `notify`.                        |
| `roles`           | Includes roles for modular tasks.                           |
| `environment`     | Sets environment variables for tasks.                       |
| `tags`            | Used to run specific parts of a playbook.                   |
| `strategy`        | Sets the execution strategy (`linear`, `free`).             |
| `max_fail_percentage` | Stops the play if a percentage of hosts fail.          |
| `serial`          | Limits the number of hosts executed in parallel.            |

---

### **Task-Level Keywords**
These are used within a `tasks` block to define actions.

| **Keyword**       | **Description**                                             |
|--------------------|-------------------------------------------------------------|
| `name`            | Describes the task (optional but recommended).              |
| `action`          | Specifies the module and its parameters (rarely used).      |
| `args`            | Specifies arguments for the module.                         |
| `when`            | Conditional statement for executing the task.               |
| `loop`            | Runs the task iteratively over a list.                      |
| `with_items`      | Alternative to `loop` for iterating over items.             |
| `register`        | Saves the result of a task to a variable.                   |
| `notify`          | Triggers a handler after the task changes something.        |
| `delegate_to`     | Executes the task on a different host.                      |
| `ignore_errors`   | Continues execution even if the task fails.                 |
| `until`           | Retries the task until a condition is met.                  |
| `retries`         | Sets the number of retries for `until`.                     |
| `delay`           | Sets the delay between retries.                             |
| `block`           | Groups tasks together.                                      |
| `rescue`          | Executes tasks if a failure occurs within a block.          |
| `always`          | Executes tasks always, regardless of success or failure.    |

---

### **Variable and Data Keywords**
| **Keyword**       | **Description**                                             |
|--------------------|-------------------------------------------------------------|
| `vars`            | Inline variable definitions.                                |
| `vars_files`      | Includes external variable files.                           |
| `vars_prompt`     | Prompts the user for input at runtime.                      |
| `set_fact`        | Creates variables dynamically during playbook execution.    |

---

### **Include and Import Keywords**
| **Keyword**       | **Description**                                             |
|--------------------|-------------------------------------------------------------|
| `include_tasks`   | Includes a set of tasks from another file dynamically.       |
| `import_tasks`    | Statically imports tasks from another file.                  |
| `include_vars`    | Includes variables from another file.                        |
| `import_playbook` | Statically imports another playbook.                         |

---

### **Handlers Keywords**
| **Keyword**       | **Description**                                             |
|--------------------|-------------------------------------------------------------|
| `notify`          | Calls a handler task after changes.                         |
| `listen`          | Groups handlers under a common name.                        |

---

### **Roles Keywords**
| **Keyword**       | **Description**                                             |
|--------------------|-------------------------------------------------------------|
| `roles`           | Includes roles for modular task management.                 |
| `role`            | Defines a single role to include.                           |
| `include_role`    | Dynamically includes a role.                                |
| `import_role`     | Statically imports a role.                                  |

---

### **Miscellaneous Keywords**
| **Keyword**       | **Description**                                             |
|--------------------|-------------------------------------------------------------|
| `debug`           | Outputs messages or variables for debugging.                |
| `assert`          | Ensures conditions are met, failing the play otherwise.     |
| `pause`           | Pauses execution for a set time or user input.              |
| `meta`            | Performs playbook meta-actions (e.g., dependency management).|
| `fail`            | Intentionally fails a playbook execution.                   |

---