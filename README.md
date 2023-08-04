# CICD-Pipeline-Ansible-Docker-Github-Webhooks-on-AWS
![Screenshot_9](https://github.com/mhaamahdhi/CICD-Pipeline-Ansible-Docker-Github-Webhooks-on-AWS/assets/12277830/a2f478c5-09be-4c54-9d5a-ab01d0b52359)




ansible playbook



jenkins@jenkins:~/playbooks$ vi deployment.yaml
- name: Build and Delploy docker container
  hosts: dockerservers
  gather_facts: false
  remote_user: root
  tasks:
    - name: copy the files to remote server
      become: true
      copy:
        src: /var/lib/jenkins/workspace/ansible-jenkins-pipeline
        dest: ~/projects

    - name: Building a docker image
      docker_image:
        name: job-portal:latest
        build:
          path: ~/projects
        state: present

    - name: Creating a container
      docker_container:
        name: job-portal-container
        image: job-portal:latest
        ports:
          - "80:80"
        state: started
