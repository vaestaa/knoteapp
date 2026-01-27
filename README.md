🧱 Big Picture First (What is happening?)

You are running:

1 App container → knote-app (NodeJS app)

1 Database container → knote-mongo (MongoDB)

Both containers talk to each other inside a private Docker network

Your browser talks to the app on port 80

🪜 Step 1 — File Name & Command

This file must be called:

docker-compose.yml


You start everything with:

docker compose up -d

🪜 Step 2 — Stack Name
name: knote-stack

What this does:

This gives your project a name:

Network → knote-stack_default

Volume → knote-stack_mongo_data

Containers → grouped under this stack

Why it’s useful:

If you run multiple projects, Docker won’t mix them.

🪜 Step 3 — Services Section
services:


This means:

"Below this, I will define containers"

Each service = 1 container

🪜 Step 4 — App Container
app:


This is the service name
Docker will also use this as:

Internal DNS name → app

Container Name
container_name: knote-app


This gives a fixed name instead of auto-generated ones

So you can do:

docker logs knote-app

Image
image: learnitguide/knotejs:1.0


This means:

Pull this image from Docker Hub and run it

Environment Variable
environment:
  MONGO_URL: mongodb://mongo:27017/dev


This tells your app:

MongoDB is running at service mongo on port 27017 and database dev

Important:

mongo = service name

Docker DNS automatically resolves it

Port Mapping
ports:
  - "80:3000"


Meaning:

Side	Port
Your laptop / server	80
Inside container	3000

So:

Browser → http://localhost → App inside container

Dependency
depends_on:
  - mongo


This means:

Start Mongo container first, then App container

Note:
It doesn’t check if Mongo is "ready", only "started"

Restart Policy
restart: unless-stopped


Meaning:
If container crashes → Docker restarts it automatically

🪜 Step 5 — Mongo Container
mongo:


This is your database container

Container Name
container_name: knote-mongo


Easy to identify and manage

Image
image: mongo:6


This runs:

Official MongoDB version 6

Data Persistence
volumes:
  - mongo_data:/data/db


This means:
Mongo stores data in:

/data/db inside container

Docker volume on your system

So even if container stops → data is safe

Restart Policy
restart: unless-stopped


Keeps DB alive

🪜 Step 6 — Volume Definition
volumes:
  mongo_data:


This tells Docker:

Create a named volume for MongoDB

🧪 Step 7 — Run It

Start:

docker compose up -d


Check:

docker ps


You should see:

knote-app

knote-mongo

🌐 Step 8 — Access App

Open browser:

http://localhost

🧠 How Networking Works (Important for Kubernetes later)

Inside Docker network:

app talks to mongo

DNS name = service name

Just like in Kubernetes:

Pod → Service DNS
