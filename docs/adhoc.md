
# ping
 ansible all -m ping

# Check hostname
 ansible multi -a "hostname"
 
# Check disk space
  ansible multi -a "df -h"

  # Check mem
 ansible multi -a "free -m"

 # Check inventory
ansible linuxservers --list-hosts

# Check hostname
 ansible multi -a "hostname"

# Commands
ansible linuxservers -m command -a "uptime"
ansible linuxservers -m command -a "df -uptime"


# Create a file with "become"
ansible ubuntu -m file -a "path=/var/new_dir state=directory" -b