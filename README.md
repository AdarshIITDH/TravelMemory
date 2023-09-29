# Travel Memory Application on AWS

### Objective:

•	Set up the backend running on Node.js.

•	Configure the front end designed with React.

•	Ensure efficient communication between the front end and back end.

•	Deploy the full application on an EC2 instance.

•	Facilitate load balancing by creating multiple instances of the application.

•	Connect a custom domain through Cloudflare.


## Step1

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

Public IP 43.204.219.34 

Private IP 172.31.7.92 

Availability zone: ap-south-1 









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
