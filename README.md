I have done services automation with Ansible 
# Here are the Ansible Playbooks that I have used 
- NTP SERVER (CHRONY)
---
- hosts: all
  gather_facts: no
  tasks:
  - name: Install chrony service
    shell: yum install chrony -y
    register: chrony_output
  - name: Print output of Install chrony service
    debug:
      msg: "{{ chrony_output }}"
  - name: enable the chrony service
    shell: systemctl enable chronyd
  - name: start the chrony service
    service:
     name: chronyd
     state: started
  - name: check status of chrony
    shell: systemctl status chronyd
    register: chrony_status
  - name: Print status of chrony
    debug:
     msg: "{{ chrony_status }}"
  - name: Creating a /etc/chrony.conf
    copy:
        dest: /etc/chrony.conf
        content:  |
            ntp_conf: "/etc/chrony.conf"  # Default path for chrony.conf on RHEL
            ntp_servers:
               - "0.rhel.pool.ntp.org iburst"
               - "1.rhel.pool.ntp.org iburst"
               - "2.rhel.pool.ntp.org iburst"
               - "3.rhel.pool.ntp.org iburst"
            allow_network: "10.206.15.0/24"
            allow_network: "10.206.25.0/24"
            allow_network :"10.200.0.0/13" 
