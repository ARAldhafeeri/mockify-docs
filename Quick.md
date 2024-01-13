# Quick Start Guide

The goal of this guide is to have mockify.io running on your machine as fast as possible so you can test it yourself.

## Prerequisites

### Step 1: Clone GitHub Repository

Clone the mockify.io GitHub repository using the following command:

```bash
git clone https://github.com/ARAldhafeeri/mockify.git
```

### Step 2: Install Docker

Install Docker on your machine. Docker is a containerization platform that allows you to package your application and all its dependencies into a single container.

[Docker Installation Guide](https://docs.docker.com/get-docker/)

---

# Configurations

### Step 3: Create .env File

In both the client and server folders, copy `example.env` and update the environment variables.

#### Step 3.1: Client environment variables

```bash
cp client/example.env client/.env
nano client/.env
```

Uncomment and update the environment variables in the `.env` file with your values. The client env should look something like this:

```env
REACT_APP_API_URL=http://localhost:5000
```

#### Step 3.2: Server environment variables

```bash
cp server/example.env server/.env
nano server/.env
```

Update the environment variables in the `.env` file with your values. The server env should look something like this:

```env
SECRET_KEY=secret
DATABASE_URL=mongodb://mongo:27017/mockify
SUPER_ADMIN_USERNAME=admin
SUPER_ADMIN_PSWD=admin
SUPER_ADMIN_EMAIL=admin@admin.com
JWT_EXPIRES_IN=1d
MOCKIFY_DOMAIN=sss
MOCKIFY_API=sss
DOMAIN=https://mockify.ioJWT_SECRET=secret
JWT_EXPIRE=30d
JWT_COOKIE_EXPIRE=
ADMIN_USERNAME=reg_admin
ADMIN_PSWD=reg_admin
```

---

# Build and Run

### Step 4: Build Docker Images

Build the mockify.io Docker images using the following command:

```bash
docker-compose build --env-file=.env build
```

### Step 5: Run Docker Containers

Run the mockify.io Docker containers using the following command:

```bash
docker-compose up --env-file=.env -d
```

The `-d` flag runs the containers in the background. Now mockify is running as follows:

- Admin Portal: [http://admin.localhost](http://admin.localhost)
- API: [http://api.localhost/v1](http://api.localhost/v1)