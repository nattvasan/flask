## Application node runs on Flask Python
1. Lauch a instance with Ubuntu OS
2. In the launched instance, execute the following commands :
    ```
     apt-get update -y
     apt-get upgrade -y
    ```
3. I have used python flask application on the two applicaton nodes. To install flask application, following commands need to be executed :
     ```
      apt-get install python-setuptools
      easy_install pip
      pip install flask
     ```
4. By default, python flask application runs on port 5000. In order to modify the listening port to 8484 [as per guidelines], I have modified the hello.py with the following code :

    ```
     from flask import Flask
     import os
     app = Flask(__name__)
     @app.route("/")
     def hello():
         return "Hi there, I'm served from ip-172-31-64-60!"
     app.debug = True
     if __name__ == '__main__':
         port = int(os.environ.get('PORT', 8484))

         if port == 8484:
            app.debug = True

         app.run(host='0.0.0.0', port=8484)
    ```
    
  *I have verified that security group is modified in such a way that traffic is allowed to access application in web-browser* 
5. Run the following command to start the flask applicaiton.
   ```
    python hello.py
    
   Sample output :
   * Running on http://0.0.0.0:8484/ (Press CTRL+C to quit)
   * Restarting with stat
   * Debugger is active!
   * Debugger PIN: 302-961-758
   ```
6. Screenshots of the running flask application uploaded with the names `app_1.png, app_2.png`. Please check.

7. Repeated the same by creating instance Store-Backed Linux AMI for the `application node 2` 



## Wed node runs on nginx 

1. Launched Ubuntu instance with prerequisites commands of update, upgrade
2. Installed nginx using the following commands :
   ```
   apt-get install nginx
   ```
3. Configured nginx service for balancing the requests in round robin fashion between two application nodes.

   ```
   Edited the file /etc/nginx/sites-available/default with the following contents :
   
   upstream nginxtest {
   server <Public_IPaddress>:8484;
   server <Public_IPaddress>:8484;
   }

   server {
   listen 80;
   server_name <server_IPaddress>;

   location / {
   proxy_pass http://nginxtest/;
   }
   }
   
   ```
 4. Nginx Service restarted and the results are uploaded as screenshots `nginx_1.png, nginx_2.png`
