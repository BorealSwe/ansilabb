# Check hostname
 ansible multi -a "hostname"
 
# Check disk space
  ansible multi -a "df -h"

  # Check mem
 ansible multi -a "free -m"