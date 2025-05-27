
# Estate Wise – Bangalore House Price Predictor

**Estate Wise** is a web-based machine learning application that predicts house prices in Bangalore using features like location, area, and number of bedrooms.

---

## Technologies Used

- **Python** – Core programming language  
- **Pandas & NumPy** – For handling and cleaning data  
- **Matplotlib** – For visualizing trends and distributions  
- **Scikit-learn** – To train and evaluate machine learning models  
- **Jupyter Notebook, VS Code, PyCharm** – Development environments used  
- **Flask** – Backend API framework  
- **HTML, CSS, JavaScript** – Frontend user interface  
- **AWS EC2** – Deployment platform for production  

---

## Deployment Instructions (AWS EC2)

### 1. Launch EC2 Instance
- Create a new instance from the AWS Management Console  
- In **Security Groups**, add a rule to allow incoming HTTP traffic (port 80)  

### 2. Connect to Your Server
Use this command to SSH into your EC2 instance:
```bash
ssh -i "C:\Users\YourName\.ssh\EstateWise.pem" ubuntu@your-ec2-public-ip
```

### 3. Install and Configure Nginx
Update and install nginx:
```bash
sudo apt-get update
sudo apt-get install nginx
```

Check nginx status:
```bash
sudo service nginx status
```

Manage nginx:
```bash
sudo service nginx start     # Start server
sudo service nginx stop      # Stop server
sudo service nginx restart   # Restart server
```

Visit your EC2 public IP in a browser—you should see the "Welcome to nginx" page.

---

## Upload Project Files

Use **WinSCP** to transfer your project files to the server:  
- Download: [https://winscp.net/eng/download.php](https://winscp.net/eng/download.php)  
- Upload your code into: `/home/ubuntu/EstateWise`  

---

## Configure Nginx to Serve Your App

Create a new config file at `/etc/nginx/sites-available/estatewise.conf` with the following content:
```nginx
server {
    listen 80;
    server_name estatewise;
    root /home/ubuntu/EstateWise/client;
    index app.html;

    location /api/ {
        rewrite ^/api(.*) $1 break;
        proxy_pass http://127.0.0.1:5000;
    }
}
```

Create a symbolic link to enable the config:
```bash
sudo ln -s /etc/nginx/sites-available/estatewise.conf /etc/nginx/sites-enabled/
```

Remove the default site:
```bash
sudo unlink /etc/nginx/sites-enabled/default
```

Restart nginx:
```bash
sudo service nginx restart
```

---

## Run the Flask Server

Install Python packages and launch the backend:
```bash
sudo apt-get install python3-pip
sudo pip3 install -r /home/ubuntu/EstateWise/server/requirements.txt
python3 /home/ubuntu/EstateWise/server/server.py
```

After starting, Flask will run on port **5000**.

---

## Final Step

Visit your EC2 public IP in the browser (e.g., `http://ec2-your-ip.compute.amazonaws.com`) to view the live **Estate Wise** site in production.
