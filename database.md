```
-
  name: Deploy a web application
  hosts: localhost
  become: yes
  tasks:
    - name: Install dependencies
      apt:
        - name: {{ item }} 
          state: installed
      with_items:
        - python
        - python-setuptools
        - python-dev
        - build-essential
        - python-pip
        - python-mysqldb
        
    - name: Install Mysql database
      apt: 
        - name: {{ item }}
          state: installed
      with_items:
        - mysql-server
        - mysql-client
        
    - name: "Start Mysql service"
      service: 
        - name: Mysql
          state: present
          enable: yes
          
    - name: "Create Application database"
      mysql_db: 
        - name: nency_db
          state: present

    - name: "Create Application DB user"
      mysql_user:
        - name: nency_admin
          password: 12345
          priv: '*.*:ALL'
          state: present
          
    - name: "Install python flask dependencies"
      pip: 
       - name: {{ item }}
         state: installed
      with_items: 
        - flask
        - flask-mysql
        
    - name: "Copy web-server code"
      copy: 
        src: /app.py 
        dest: /opt/app.py
    
    - name: "run we-server"
      shell: 
```
