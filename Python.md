### Python APP 
 - mkdir pyapp
 - cd pyapp
 - nano app.py
 - nano requirements.txt
 - nano dockerfile
###isnide this file paste the content 
from flask import Flask<br>

app = Flask(__name__)<br>

@app.route('/')<br>
def home():<br>
    return "Hello from Flask in a Docker container!"<br>

if __name__ == "__main__":<br>
    app.run(host="0.0.0.0", port=5000)<br>
##3lets create an requirement.txt file<br>
 nano requirements.txt<br>
#####inside this just put one singel line <br>
Flask<br>
###lets creat the docker file<br>
      [root@ip-172-31-115-189 pyapp]$ nano dockerfile<br>
#######inside the docker file we will put instruction<br>
FROM python:3.9-slim<br>
WORKDIR /app<br>
COPY requirements.txt .<br>
RUN pip install --no-cache-dir -r requirements.txt<br>
#copy the entire application inside your container<br>
COPY . .<br>
EXPOSE 5000<br>
ENTRYPOINT ["python","app.py"]<br>

 - docker build -t pythonapp .
 - docker images
 - docker run -d -p 82:5000 pythonapp
 - docker ps
 - curl localhost:82

#### to docker hub
 - docker images
- docker tag 877ba787cc97 mysticmac7/training
- docker push mysticmac7/training

<img width="1576" height="572" alt="image" src="https://github.com/user-attachments/assets/4f583fb3-db08-4f94-8628-5ec8b9855cda" />
