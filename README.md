# Travel Memory Application on AWS

### Objective:

 - Set up the backend running on Node.js.

 - Configure the front end designed with React.

 - Ensure efficient communication between the front end and back end.

 - Deploy the full application on an EC2 instance.

 - Facilitate load balancing by creating multiple instances of the application.

 - Connect a custom domain through Cloudflare.


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
    MONGO_URI='mongodb+srv://adarsh307kumar:GfzaOe274UROZDX3@travelmemory.wxwkpag.mongodb.net/travelmemory'
    PORT=3000
    sudo npm install
    
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/fbb18fd8-9f7f-48c2-a400-62be3fed0022)

    sudo node index.js  
    
The backend application at port 3000

![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/efb63c71-c3cb-4e26-a4d6-5b1a8da1c329)

Don’t forget to add the IP of backend to the Mongo DB IP access list so that backend instances can communicate to database 
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/574c7ccd-f75b-42be-a1d5-c31639aecf2d)
![image](https://github.com/AdarshIITDH/TravelMemory/assets/60352729/0ebe3fd4-0c07-4b08-95a8-bc1345e24948)

## Step3 - Setting up Reverse Proxy for the Frontend & Backend Both









`.env` file to work with the backend:

```
MONGO_URI='ENTER_YOUR_URL'
PORT=3000
```

Data format to be added: 

```json
{
    "tripName": "Incredible India",
    "startDateOfJourney": "19-03-2022",
    "endDateOfJourney": "27-03-2022",
    "nameOfHotels":"Hotel Namaste, Backpackers Club",
    "placesVisited":"Delhi, Kolkata, Chennai, Mumbai",
    "totalCost": 800000,
    "tripType": "leisure",
    "experience": "Lorem Ipsum, Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum, ",
    "image": "https://t3.ftcdn.net/jpg/03/04/85/26/360_F_304852693_nSOn9KvUgafgvZ6wM0CNaULYUa7xXBkA.jpg",
    "shortDescription":"India is a wonderful country with rich culture and good people.",
    "featured": true
}
```
