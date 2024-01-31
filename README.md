# Self-Signed SSL Certificate for Nginx on GCP ubuntu-22.04 VM
A self-signed certificate will encrypt communication between your server and any clients. However, since it is not signed by any of the trusted Certificate Authorities (CA) included with web browsers, users cannot use the certificate to validate the identity of your server automatically. A self-signed certificate may be appropriate if you do not have a domain name associated with your server and for instances where the encrypted web interface is not user-facing. If you do have a domain name, in many cases it is better to use a CA-signed certificate.

### 1. Update Package Lists
Make sure your package lists are up to date:
```bash
sudo apt update
```

### 2. Install Nginx
Install Nginx using the following command:
```bash
sudo apt install nginx
```
### 3. Start Nginx
Start the Nginx service:
```bash
sudo systemctl start nginx
```
### 4. Enable Nginx to start on boot
Start the Nginx service:
```bash
sudo systemctl enable nginx
```

### 5. Check Nginx Status
Verify that Nginx is running without any errors:
```bash
sudo systemctl status nginx
```
### 6. Allow HTTP and HTTPS traffic in the firewall
Ensure that the firewall allows HTTP (port 80) and HTTPS (port 443) traffic:
```bash
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable
```

### 7. Generate a Self-Signed SSL Certificate
Create a directory to store the SSL certificate:
```bash
sudo mkdir /etc/nginx/ssl
```
Generate a self-signed SSL certificate and key:

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/nginx.key -out /etc/nginx/ssl/nginx.crt
```
You will be prompted to provide information for the certificate. Fill in the details accordingly.

### 8. Configure Nginx for SSL
Edit the Nginx default configuration file:
```bash
sudo nano /etc/nginx/sites-available/default
```

Find the server block and update it to include the SSL configuration:
```bash
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    # SSL configuration
    listen 443 ssl default_server;
    listen [::]:443 ssl default_server;
    ssl_certificate /etc/nginx/ssl/nginx.crt;
    ssl_certificate_key /etc/nginx/ssl/nginx.key;

    # Other SSL settings...

    # ..... rest of the server block
}

```
Save the file and exit the text editor.

### 9. Test changes Nginx Configuration
Test the Nginx configuration to ensure there are no syntax errors:
```bash
sudo nginx -t
```
### 10. Restart Nginx
If the configuration test is successful, restart Nginx to apply the changes:
```bash
sudo systemctl restart nginx
```
### 11. Access the Nginx server
Open a web browser and navigate to https://[YOUR_VM_IP]. You should see the default Nginx welcome page.

Now, your Nginx server is set up with a self-signed SSL certificate on a GCP Ubuntu VM. Keep in mind that self-signed certificates are not suitable for production environments, and you should consider obtaining a valid SSL certificate from a trusted certificate authority for a production setup.
