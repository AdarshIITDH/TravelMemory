# Travel Memory Application on AWS

### Objective:

 - Set up the backend running on Node.js.

 - Configure the front end designed with React.

 - Ensure efficient communication between the front end and back end.

 - Deploy the full application on an EC2 instance.

 - Facilitate load balancing by creating multiple instances of the application.

 - Connect a custom domain through Cloudflare.

![1](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/64e9327d-3705-4344-b8b2-b2f9e8f6ebb2)

## Step1 - Setting up the EC2 instances for Frontend & Backend

Create the 2 EC2 instances (instance type - t2.micro), One is for the frontend and the other is for the backend.

Do the configuration of each instances at the time of creation using below script

```
#!/bin/bash 
sudo apt update
sudo bash
curl -s https://deb.nodesource.com/setup_18.x | sudo bash
sudo apt install nodejs -y
sudo cd /home/ubuntu/
sudo git clone https://github.com/UnpredictablePrashant/TravelMemory
```
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/240c6b65-7e1d-4167-855a-c6f76497b829)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/45693a8d-c068-4de2-a01e-5302cac1c0e2)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/cc689901-e425-41a4-a6ef-d1a7c1bcee4b)

The Backend instances is up and running with:
 - Public IP 43.204.219.34 
 - Private IP 172.31.7.92 
 - Availability zone: ap-south-1

![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/f25f0283-7310-461a-8600-36c2b5389e20)

The Frontend instances is up and running with 
 - Public IP 3.109.55.137 
 - Private IP 172.31.12.91 
 - Availability zone: ap-south-1 

![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/59fe7260-a2ec-4c84-a9cf-4a1b615daaaf)
### The configuration we have done in both instances is successful as we can see the repo gets cloned.


## Step2 - Setting up the Frontend & Backend individually

### Frontend Configuration
    cd /home/ubuntu/TravelMemory/frontend
    sudo npm install
    nano src/url.js
    export const baseUrl = "http://[backend IP]"
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/50acdc40-edb7-47df-b68b-f2316e8022f4)

    sudo npm start   
The frontend application is running at port 3000

![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/8c3c0f31-0a09-4e58-93b3-a563663a7071)

### Backend Configuration
    cd /home/ubuntu/TravelMemory/backend/
    nano .env
    MONGO_URI='mongodb+srv://adarsh307kumar:<password>@travelmemory.wxwkpag.mongodb.net/travelmemory'
    PORT=3000
    sudo npm install
    
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/fbb18fd8-9f7f-48c2-a400-62be3fed0022)

    sudo node index.js  
    
The backend application at port 3000

![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/efb63c71-c3cb-4e26-a4d6-5b1a8da1c329)

Don’t forget to add the IP of the backend to the Mongo DB IP access list so that backend instances can communicate to the database 
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/574c7ccd-f75b-42be-a1d5-c31639aecf2d)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/0ebe3fd4-0c07-4b08-95a8-bc1345e24948)

## Step3 - Setting up Reverse Proxy for the Frontend & Backend Both

 - ### Backend Reverse proxy configuration

We are doing the reverse proxy using Nginx
 
    sudo apt install nginx
    sudo systemctl status nginx 
    
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/6709c23e-6f36-4255-bd09-5b45e4cc7ddd)

We can check the nginx is running at port 80
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/d50a1528-0782-41d7-baf4-f201269392c3)

Now for the reverse proxy 
```
sudo unlink /etc/nginx/sites-enabled/default
cd /etc/nginx/sites-available/
sudo nano custom_server.conf
```

```
server { 
listen 80;
location / {
proxy_pass http://my_server;
}}
```
```
ln -s /etc/nginx/sites-available/custom_server.conf /etc/nginx/sites-enabled/custom_server.conf
sudo service nginx configtest
sudo service nginx restart
```
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/630c312b-4cc8-41b6-a670-ca7ee6e2a9b4)

Now we can access the backend at port 80 because of the reverse proxy
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/7ae36411-e9e5-4f97-ba73-6042f46b07a5)

 - ### Frontend Reverse proxy configuration

Follow the same steps as we did in Backend reverse configuration
After doing that frontend will be running at port 80
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/df69c313-681a-4c57-862d-f1ee70fcd253)


## Step4 - Scaling the Appliacation

 - Create multiple instances of both the frontend and backend servers.
 - Add these instances to a load balancer to ensure efficient distribution of incoming traffic.

Launching the extra instances to distribute the load using taking snapshots of frontend & backend both.

![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/78b550fd-6dee-462b-b010-e7a68c398b7d)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/dcfa2df4-ea7d-43c1-9806-73fc1272e787)

You can find the snapshots in AMIs (Amazon Machine Images)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/2730bd95-bbd8-4d6d-9b76-282c6c71b790)

Then will be launching the instance using the AMIs we have saved earlier.
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/1ab4029f-c1c6-4fa4-9a1d-8ddeb839baa8)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/5beb5758-187d-4945-824d-0b00a08d571c)

Frontend

![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/409cf6a0-90e8-41a1-b311-5e3747785f81)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/5888d14b-c5ef-44b4-bc63-8426ad812e2e)

We are able to see the same data and configuration.
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/cb3a548f-3809-4a6f-864b-1e7c454784c8)

For Load balancing we need to create Target Groups to collect the instances of frontend ( i.e                          adarsh_mern_frontend & adarsh_mern_frontend2) and backend (adarsh_mern_backend & adarsh_mern_backend2)

- EC2 > Target groups > Create target group
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/91ac2a51-be19-439c-ba6b-33e4b8973a9d)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/bbe9475c-0826-4a14-a9a5-23b03dc37599)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/49230a6b-2e30-444a-abed-8b3b74356c0a)

For creating the load balancer based on target groups
 - EC2 > Load balancers > Create Application Load Balancer

![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/b46a6f81-2eea-4b60-a4a9-fcf8e9a7a6b6)
Make sure the Load balancer is mapped to multi AZs
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/4b081ca1-de2a-4992-a0ea-c42d90c64450)

Adding the Target group we have created for the frontend instances.
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/2f3f0160-6be8-4e55-97bb-c88389309d47)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/9c92521b-c1f6-490d-9e4f-b759fcb74857)

Same for the Backend Loadbalancer.
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/2ef82c3f-8dcb-45eb-8db5-49be5bb0893b)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/297caace-c2cc-4d8a-b001-af4e3602c352)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/43366f1b-cfdf-4092-9408-37ee900cbe00)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/0c03946c-2dd6-42fc-acd7-9d1ec17eef65)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/3845df45-8d1f-4106-a1f7-d83f0902072d)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/93482cd9-acf9-48e0-a4cb-5c2546b238f5)

Both the Load Balancers are active.
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/0a815a8f-5a1a-453e-9c97-bfb93ea2568c)


## Step5 - Scaling the Appliacation

Domain Setup with Cloudflare:
 - Connect your custom domain to the application using Cloudflare.
 - Create a CNAME record pointing to the load balancer endpoint.

Used the domain [adarshkumars.co.in] to point towards the Frontend Load Balancer by adding a CNAME entry in Cloudflare and used the subdomain [back.adarshkumars.co.in].
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/49837820-d3c3-4403-a76a-9891d4db757d)

Now making the backend accessible to the subdomain we have created for both the backend and putting in url.js file of frontend instances.
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/411a3390-37de-42df-a82a-2fe1a5b8086b)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/65da169d-bf71-41e0-b532-6a9b0735f3bf)

Lets run the application using domain name 
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/a36ef4e6-0bb8-43d8-9113-7e23fdfb5d52)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/0eda79de-a8aa-4026-b23d-7452122102a9)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/29285716-afb1-4ca0-80a8-3c986918342a)

The data get stored in the MongoDB database.
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/c642f0c6-7ece-4e79-9e55-23b42e7c1ecb)




