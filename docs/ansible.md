# Ansible in our setup

I asked copilot about this for p1 and apparently is a good idea.

Currently, your Vagrantfile provisions the servers using shell scripts (server.sh and worker.sh). This works, but as your environment grows or becomes more complex, shell scripts can be harder to maintain and less flexible.

**How Ansible would help:**
- **Readability:** Ansible playbooks are easier to read and organize than long shell scripts.
- **Idempotency:** Ansible ensures tasks only run if needed (e.g., only installs packages if missing).
- **Extensibility:** Easily add roles for monitoring, networking, users, etc.
- **Centralized management:** Manage both VMs from one playbook, with variables for each host.

**How to use Ansible with Vagrant:**
1. **Install Ansible** on your host machine.
2. **Create an Ansible playbook** for your K3s setup.
3. **Configure Vagrant** to use Ansible as a provisioner, or run Ansible manually after `vagrant up`.
4. **Use Vagrant’s built-in inventory** to target your VMs.

**Example Vagrantfile snippet for Ansible:**
```ruby
config.vm.provision "ansible" do |ansible|
  ansible.playbook = "playbook.yml"
  ansible.inventory_path = "inventory.ini"
end
```

and
Ansible and Vagrant work together to simplify the provisioning process.

### The Ansible Directory and Inventory

You can use Ansible to provision Vagrant virtual machines. Your **Ansible directory** and playbooks should be located **on your host machine**, not inside the Vagrant VM. When you run `vagrant up`, Vagrant reads its `Vagrantfile` and, if configured, will call Ansible to provision the new VMs.

Ansible connects to the guests using SSH, and Vagrant automatically sets up the necessary connections and passes them to Ansible. You don't need to manually create an Ansible inventory file because Vagrant generates a temporary one for you. This temporary inventory file contains the SSH connection details for each of the Vagrant VMs.

### Vagrant and Ansible Integration

Vagrant has a **built-in Ansible provisioner**, so you don't need a separate plugin. You define the Ansible provisioner directly within your `Vagrantfile`.

Here’s a basic example of how the `Vagrantfile` would look:

```ruby
Vagrant.configure("2") do |config|
  config.vm.define "web" do |web|
    web.vm.box = "ubuntu/xenial64"
  end

  config.vm.define "db" do |db|
    db.vm.box = "ubuntu/xenial64"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
  end
end
```

When you run `vagrant up`, Vagrant will:

1.  Spin up the "web" and "db" VMs.
2.  Generate a temporary Ansible inventory file with the IP addresses and SSH keys needed to connect to the VMs.
3.  Execute the Ansible playbook (`playbook.yml`) on your host machine, using the generated inventory to connect and provision both VMs.

This approach keeps your provisioning logic separate from your virtual machine configuration and automates the process of connecting them.