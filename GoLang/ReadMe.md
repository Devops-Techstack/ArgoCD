 How to Dockerize a Golang Application?

 Golang is a modern programming language known for its speed.
 
 Take-Away:
 -> How to create a simple Golang application
 -> How to Dockerize the application
 -> How to run and test the Dockerized application.
 -> How to Deploy the Golang Application on kubernetes Cluster .
 
 Step 1: Clone the Repo
 git clone  URL
 
 Step 2: Create a Golang Application
 go mod init go-app
 (go.mod file will be created with the required go version)
 go mod init is the command that officially turns a simple directory into a modern Go project and track dependencies for your project .
 main.go is the main file where we are using a custom HTML page for the output.
 
 Step 3: Run the Application Locally
 To compile and run the application, use the following command.
 
 go run main.go
 
 Once the app starts running, we can access the web page from our local machine.
 
 For that, open any browser and paste the URL http://localhost:8080/
 
 Step 4: Create a Dockerfile for the Go Application
 We need to create a multi-stage container image to reduce the weight of the application.
 
 Step 5: Build, tag and push the docker Image to your docker registry
  docker build -t go-web-app .
 
 Step 6: Run Containerized Go Application
 docker run -d --name go-app -p 8080:8080  go-web-app
 docker ps
 For that, open any browser and paste the URL http://localhost:8080/
 
 Step 7 : Deploy on Kubernetes 
 kubectl apply -f deployment.yaml
 Access the Application over browser
 kubectl port-forward go-app-deployment-* 8080:8080
