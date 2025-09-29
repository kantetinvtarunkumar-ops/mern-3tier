3-Tier Application Deployment Architecture on AWS

Backend (branch-devops )
------------------------
**Clone the Repository**

```bash
git clone https://github.com/vickydevo/mern-3tier.git
cd mern-3tier
```

**Navigate to Backend Directory**

```bash
cd backend
```

**Install npm and Dependencies**

```bash
sudo apt update -y && sudo apt install npm -y
npm i
```

**Set up your MongoDB Database**

- Open MongoDB Compass and connect to `mongodb://localhost:27017`.

**Install MongoDB Community Edition**

Follow these steps to install MongoDB Community Edition using the apt package manager:

1. Import the public key:

    ```bash
    sudo apt-get install gnupg curl
    curl -fsSL https://www.mongodb.org/static/pgp/server-8.0.asc | \
        sudo gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg \
        --dearmor
    ```

2. Create the list file for Ubuntu 24.04 (Noble):

    ```bash
    echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu noble/mongodb-org/8.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list
    ```

3. Reload the package database:

    ```bash
    sudo apt-get update
    ```

4. Install, Start and Enable MongoDB Community Server:

    ```bash
    sudo apt-get install -y mongodb-org
    sudo systemctl start mongod
    sudo systemctl enable mongod
    ```
    **Uninstall MongoDB Community Edition**

    1. **Stop the MongoDB Service**

        ```bash
        sudo systemctl stop mongod
        ```

    2. **Remove MongoDB Packages**

        ```bash
        sudo apt-get purge mongodb-org*
        sudo rm -r /var/log/mongodb
        sudo rm -r /var/lib/mongodb
        ```

**Add Sample Data**

- Check data is added in mongo:

  ```bash
  mongosh
  show dbs
  show collections
  ```

- Import sample data:

  > To populate the database with sample posts, copy the content from `backend/data/sample_posts.json` and insert it as a document in the `wanderlust.posts` collection using MongoDB Compass or `mongoimport`.

  ```bash
  mongoimport --db wanderlust --collection posts --file ./data/sample_posts.json --jsonArray
  ```

**Configure Environment Variables**

- Update the private IP address in `.env.sample` for MongoDB and Redis URLs.
- Copy the sample environment file:

  ```bash
  cp .env.sample .env
  ```

**Start the Backend Server**

```bash
npm start
# Or run in background:
nohup npm start > backend-output.log 2>&1 < /dev/null &
```

- Access the backend at `http://<backend-public-ip>:<port>` or `http://<backend-public-ip>:<port>/api/post`.
 > You should see the following on your terminal output on successful setup.
   >
   > ```bash
   > [BACKEND] Server is running on port 5000
   > [BACKEND] Database connected: mongodb://<backend-pravite-ip>/wanderlust
   >

- Provide these instructions in your `README.md` file.
Frontend (branch-devops )
------------------------
### Setting up the Frontend

1. **Open a frontend server**

   ```bash 
   git clone https://github.com/vickydevo/mern-3tier.git
   cd mern-3tier/frontend
   ```

2. **Install Dependencies**

   ```bash
   sudo apt update -y && sudo apt install npm -y
   npm i
   ```
> **Update the Backend API URL in Environment File**
>
> - Edit `.env.sample` and set the VITE_BACKEND_URL to your backend's public IP and port (e.g., `http://<backend-public-ip>:<port>`).
> - Copy the sample environment file:
>
>   ```bash
>   cp .env.sample .env.local
>   ```
>
> **Start the Frontend Server**
>
> - Run the development server in the foreground:
>
>   ```bash
>   npm run dev
>   ```
>
> - Or run it in the background:
>
>   ```bash
>   nohup npm run dev > frontend-output.log 2>&1 < /dev/null &
>   ```
>
> **Access the Frontend**
>
> - Open your browser and navigate to `http://<frontend-public-ip>:<port>`.
3. **Configure Environment Variables**

   ```bash
   cp .env.sample .env.local
   ```

4. **Launch the Development Server**

   ```bash
   npm run dev
   ```

## Creating a New Post

To create a new post in the application, send a POST request to the backend API endpoint `/api/post` with the required data. You can use `curl`, Postman, or any HTTP client.

**Example using `curl`:**

```bash
curl -X POST http://<backend-public-ip>:<port>/api/post \
    -H "Content-Type: application/json" \
    -d '{
        "title": "My First Post",
        "content": "This is the content of my first post.",
        "author": "Your Name"
    }'
```

Replace `<backend-public-ip>` and `<port>` with your backend server's public IP and port.

**Expected Response:**

A successful request will return the created post object in JSON format.

**Note:**  
Ensure your backend server is running and accessible, and that MongoDB is properly configured.
