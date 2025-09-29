### 3-Tier Application Deployment Architecture on AWS

## Setting up the Backend Server
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
    **if want to Uninstall MongoDB Community Edition**
    1. **Stop and Remove the MongoDB Service**

        ```bash
        sudo systemctl stop mongod
        sudo apt-get purge mongodb-org*
        sudo rm -r /var/log/mongodb
        sudo rm -r /var/lib/mongodb
        ```

**Add Sample Data**

  > To populate the database with sample posts, copy the content from `backend/data/sample_posts.json` and insert it as a document in the `wanderlust.posts` collection using MongoDB Compass or `mongoimport`.

  ```bash
  mongoimport --db wanderlust --collection posts --file ./data/sample_posts.json --jsonArray
  ```
- Check data is added in mongo:

  ```bash
  mongosh
  show dbs
  show collections
  ```
**Configure Environment Variables**
**Configure MongoDB Network Interfaces (Optional)**

  - To allow MongoDB to accept connections on a private IP, edit the `mongod.conf` file (usually at `/etc/mongod.conf`):
          ```yaml
          net:
            port: 27017
            bindIp: 127.0.0.1,<your-private-ip>
          ```
        - Replace `<your-private-ip>` with your server's private IP address (e.g., `172.31.18.243`).
        - Restart MongoDB for changes to take effect:
          ```bash
          sudo systemctl restart mongod
          ```

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

- Provide these instructions in your `README.md` file.

## Setting up the Frontend Server

1. **Open a frontend server**

   ```bash 
   git clone https://github.com/vickydevo/mern-3tier.git
   cd mern-3tier/frontend
   ```

2. **Install Dependencies**

   ```bash
   sudo apt update -y && sudo apt install npm -y
   npm i
3. **Configure Environment Variables**

    **Update the Backend API URL in Environment File**
    
    - Edit `.env.sample` and set `VITE_BACKEND_URL` to your backend's public IP and port (e.g., `http://<backend-public-ip>:<port>`).


    - Copy the sample environment file:
    
      ```bash
       cp .env.sample .env.local
      ```

4. **Launch the Development Server**

    - Run the development server in the foreground:
      ```bash
      npm run dev
      ```
    - Or run it in the background:
      ```bash
      nohup npm run dev > frontend-output.log 2>&1 < /dev/null &
      ```

5. **Access the Frontend**

    - Open your browser and navigate to `http://<frontend-public-ip>:<port>`.

    ## Creating a Post from the Website

    1. **Open the Frontend Application**

        - In your browser, go to `http://<frontend-public-ip>:<port>`.

    2. **Navigate to the "Create Post" Page**

        - Click on the "Create Post" or similar button/link in the navigation menu.
        <img width="1857" height="933" alt="Image" src="https://github.com/user-attachments/assets/6fc05c13-63a2-4608-b091-f71384a79f96" />

    3. **Fill Out the Post Form**

        - Enter the required details such as title, content, and any other fields.
        <img width="1536" height="893" alt="Image" src="https://github.com/user-attachments/assets/58bdeec7-2d37-4953-9c11-f9b28f9a5cd9" />

        - Click the "Submit" 
        - If successful, you should see your new post listed on the homepage or posts page.

    5. **Verify in the Backend**

        - You can check the backend database to confirm the post was created:
          ```bash
          mongosh
          use wanderlust
          db.posts.find().pretty()
          ```