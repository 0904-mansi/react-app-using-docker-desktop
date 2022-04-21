# Creating my First React app using Docker Desktop
React is a JavaScript library for building user interfaces. It makes it painless to create interactive UIs. You can design simple views for each state in your application, and React efficiently update and render just the right components when your data changes.

React is a declarative, efficient, and flexible JavaScript library for building user interfaces. It lets you compose complex UIs from small and isolated pieces of code called “components”. React can also render on the server using Node and power mobile apps using React Native. React allows you to interface with other libraries and frameworks

# Get Started Without Docker Run the below command:
```
npx create-react-app whalified
```
# Starting the React app
```
cd whalified
yarn start 
```
or 
```
npm start
```
It will open up the first react app over the browser at port 3000.
# With Docker

## Step 1. Create a Dockerfile
```
FROM node:15.4 as build 
WORKDIR /react-app
COPY package*.json .
RUN yarn install
COPY . .
RUN yarn run build
FROM nginx:1.19
COPY ./nginx/nginx.conf /etc/nginx/nginx.conf
COPY --from=build /react-app/build /usr/share/nginx/html
```
## Step 2. Create a dockerignore
Now, let's configure the .dockerignorefile . Copy and paste the below snippet.
```
**/node_modules
```

## Step 3. Create nginx.conf file
Create a folder nginx/ and then add the below content in nginx.conf file:
```
worker_processes  1;

events {
  worker_connections  1024;
}

http {
  server {
    listen 80;
    server_name  localhost;

    root   /usr/share/nginx/html;
    index  index.html index.htm;
    include /etc/nginx/mime.types;

    gzip on;
    gzip_min_length 1000;
    gzip_proxied expired no-cache no-store private auth;
    gzip_types text/plain text/css application/json application/javascript application/x-javascript text/xml application/xml application/xml+rss text/javascript;

    location / {
      try_files $uri $uri/ /index.html;
    }
  }
}
```
## Step 5. Build the Docker Image
```
docker build -t mansimishra/firstreact-app .
```
## Step 6. Run the app
```
docker run -d -p 80:80 mansimishra/firstreact-app
```
## step 7. Access the app
![image](https://user-images.githubusercontent.com/81081105/164446047-2418fd10-3303-42a9-b12f-130d10de8fa9.png)
