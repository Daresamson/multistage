

# 🚀 Dockerized Python Flask App  

This guide walks you through creating a **Python Flask application**, containerizing it with **Docker**, and optimizing the image size using **Alpine Linux**.  

---

## 🔹 Project Overview  
We’ll:  
✅ Create a **Flask application**  
✅ Use **Docker** to containerize the app  
✅ Optimize the **Docker image size** using **Alpine Linux**  

---

## ✅ Step 1: Set Up Your Project Directory  
On your **AWS EC2 instance**, create a project folder:  

```sh
mkdir my-python-app && cd my-python-app
```

---

## ✅ Step 2: Create the Python Application (`app.py`)  
Inside the project folder, create a new file called `app.py`:  

```sh
nano app.py
```

Paste the following **Flask** code:  

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello, Dockerized Python!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

**Save and exit** (`CTRL + X`, then `Y`, then `ENTER`).  

---

![Screenshot (64)](https://github.com/user-attachments/assets/9b4f36ad-ef1e-4d64-81c8-7dbe6b63b7a4)


## ✅ Step 3: Create `requirements.txt`  
This file lists dependencies needed for the project.  

```sh
nano requirements.txt
```

Add the following line:  

```txt
flask
```

**Save and exit** (`CTRL + X`, then `Y`, then `ENTER`).  

---

![Screenshot (65)](https://github.com/user-attachments/assets/ff92fef2-b74d-48df-b3d1-44acb344809c)

![Screenshot (66)](https://github.com/user-attachments/assets/0e48cb12-6aad-4144-a7e2-2e71be50b03c)

## ✅ Step 4: Create a **Multistage Dockerfile**  
Now, let's create an optimized **multistage Dockerfile**.  

```sh
nano Dockerfile
```

Paste the following content:  

```dockerfile
# Stage 1: Build Stage (Use a lightweight Python base image)
FROM python:3.9-alpine AS builder

# Set the working directory
WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Stage 2: Production Stage
FROM python:3.9-alpine

# Set the working directory
WORKDIR /app

# Copy only necessary files from the builder stage
COPY --from=builder /usr/local/lib/python3.9/site-packages /usr/local/lib/python3.9/site-packages
COPY app.py .

# Expose the port the app runs on
EXPOSE 5000

# Default command to run the app
CMD ["python", "app.py"]
```

**Save and exit** (`CTRL + X`, then `Y`, then `ENTER`).  

---
![Screenshot (67)](https://github.com/user-attachments/assets/a9c6c66b-134e-40eb-a241-23c46668cbd6)
![Screenshot (65)](https://github.com/user-attachments/assets/079d2d3a-47b6-4208-a088-52ab33f12a57)


## ✅ Step 5: Create a `.dockerignore` File  
To avoid copying unnecessary files into the Docker image:  

```sh
nano .dockerignore
```

Add the following:  

```txt
__pycache__/
*.pyc
*.pyo
*.pyd
venv/
```

**Save and exit** (`CTRL + X`, then `Y`, then `ENTER`).  

---
![Screenshot (69)](https://github.com/user-attachments/assets/d7d11ed8-b76f-4ace-9fa7-3d77330d810f)


## ✅ Step 6: Build the Docker Image  
Now, build your optimized **Docker image**:  

```sh
sudo docker build -t my-python-app .
```

This will:  
✅ **Use Alpine Linux** for a **smaller** image size  
✅ **Install dependencies** in a separate **builder stage**  
✅ **Copy only the necessary files** to minimize image size  

---
![Screenshot (70)](https://github.com/user-attachments/assets/69261a1b-ef7f-4909-a45d-22cea92faadf)


## ✅ Step 7: Run the Container  
Now, start a container from the built image:  

```sh
sudo docker run -d -p 5000:5000 my-python-app
```

- `-d` → Run in **detached mode**  
- `-p 5000:5000` → Map **port 5000** in the container to **port 5000** on EC2  

---
![Screenshot (67)](https://github.com/user-attachments/assets/399104cd-70f9-4be4-9473-a8f49ecfc5ff)

## ✅ Step 8: Verify the Running Container  
Check if the container is running:  

```sh
sudo docker ps
```

You should see something like this:  

```sh
CONTAINER ID   IMAGE           COMMAND          STATUS          PORTS                    NAMES
abcd1234       my-python-app   "python app.py"  Up 1 min        0.0.0.0:5000->5000/tcp   my-python-container
```

---

![Screenshot (66)](https://github.com/user-attachments/assets/4677a8d7-f258-4f89-9f34-31746cc26e83)


![Screenshot (69)](https://github.com/user-attachments/assets/a5a3437a-6c3e-4248-95a4-0ebcf1e2d084)

![Screenshot (70)](https://github.com/user-attachments/assets/46f20d58-0788-42a7-81b3-9c0c8d886874)



## ✅ Step 9: Test the Application  
Check if the app is running by curling from the EC2 instance:  

```sh
curl http://localhost:5000
```

You should see:  

```sh
Hello, Dockerized Python!
```

Or, open in your browser:  

```sh
http://your-ec2-public-ip:5000
```

---

## ✅ Step 10: Push the Image to Docker Hub (Optional)  
If you want to **store and share** your image, push it to **Docker Hub**:  

### 1️⃣ Login to Docker Hub:  
```sh
sudo docker login
```

### 2️⃣ Tag the Image:  
```sh
sudo docker tag my-python-app your-dockerhub-username/my-python-app:latest
```

### 3️⃣ Push the Image:  
```sh
sudo docker push your-dockerhub-username/my-python-app:latest
```

![Screenshot (64)](https://github.com/user-attachments/assets/af1456f8-37bb-4d34-ab59-eed508ca811d)

![Screenshot (65)](https://github.com/user-attachments/assets/b0736d1c-4523-445a-8d3e-1dd66c35abbe)
![Screenshot (65)](https://github.com/user-attachments/assets/34af13c4-98f8-478e-ab18-02ce1b4963c2)

![Screenshot (70)](https://github.com/user-attachments/assets/79cc8a7e-7df7-4666-8193-9e3b7ed04b8e)

![Screenshot (69)](https://github.com/user-attachments/assets/cdeffd44-76b9-45c6-aaf4-ce25de43a252)



