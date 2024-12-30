# Ansible Playbook: DDOS Mitigation Script

## Description
This Ansible playbook, `ddos.yml`, is used to deploy and execute an image downloading script across multiple servers. The script automates downloading images from a URL list while tracking bandwidth usage and the number of requests made.

## Requirements
1. **Ansible** installed on your control machine.
2. SSH access to the remote servers specified in the `hosts.ini` file.
3. Python 3 configured on the remote servers.
4. A file named `final_image_urls_combined.txt` containing the image URLs to download.

## Provided Files
- **`ddos.yml`**: The Ansible playbook.
- **`final_image_urls_combined.txt`**: File with the image URLs.
- **`hosts.ini`**: Inventory file with the remote servers' configuration.

## Steps to Execute
1. **Configure the inventory file**:
   Ensure the `hosts.ini` file contains the correct entries for the remote servers. This file should follow the proper format with server groups and necessary variables. Example:

   ```ini
   [all_servers]
   server1 ansible_host=192.168.1.100
   server2 ansible_host=192.168.1.101

   [all_servers:vars]
   ansible_user=root
   ansible_python_interpreter=/usr/bin/python3
   ansible_interpreter_discovery=False
   ```

2. **Place the files in the project directory**:
   Ensure the following files are present:
   - `ddos.yml`
   - `final_image_urls_combined.txt`
   - `hosts.ini`

3. **Run the playbook**:
   Use the following command to execute the playbook. It will prompt for the SSH password to connect to the remote servers:

   ```bash
   ansible-playbook ddos.yml -i hosts.ini --ask-pass
   ```

## Playbook Features
- **Install required packages**: Installs `curl` and `sudo` on remote servers.
- **Configure sudo**: Adjusts sudo configuration to not require a TTY.
- **File copying**: Uploads the `final_image_urls_combined.txt` file and a download script to the servers.
- **Create a systemd service**: Sets up and enables a systemd service to run the download script continuously for 5 hours.
- **Download monitoring**: Verifies that initial downloads are successfully completed on remote servers.

## Important Notes
- Ensure remote servers have internet access to download images.
- The download script uses a temporary directory that is automatically cleaned up after execution.

## Relevant Files
- **`ddos.yml`**: [View full content here](ddos.yml)
- **`final_image_urls_combined.txt`**: Contains a list of URLs like these:

  ```
  https://www.globalrefuge.org/wp-content/uploads/2023/08/careers-1.jpg
  https://www.globalrefuge.org/wp-content/uploads/2023/09/What_We_Do_Advocacy_priorities.jpg
  ```

- **`hosts.ini`**: Server configuration. [View example format here](hosts.ini)

## Troubleshooting
1. **Connection error**:
   Ensure the IPs in `hosts.ini` are correct and you have SSH permissions for the servers.

2. **Permission issues**:
   Verify that the user specified in `hosts.ini` has sudo privileges.

3. **Services not started**:
   Use the command `systemctl status image_downloader.service` on remote servers to check the service status.

## Contact
For more information, questions, or contributions, contact [your contact information here].
