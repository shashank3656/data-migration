How to Deploy a React App to a Kubernetes Cluster

Step:-1 
    
      Creating the React Application

      $ npx create-react-app hello

      --->npx-create -react app is a command that will install all the required packages for building single-page React     applications.
     
     --->After npx create-react-app we define the name of the application, here we are using hello.
     
     --->The create react app does not handle any backend stuff or databases, it just creates a front end structure so we can use it with any suitable backend.

Step:-2

       Starting React Application

       $ npm start

       --->Above command start your react application.


Step:-3

       Optimizing React Application

       $ npm run-script build


Step:-4

       Dockerizing The Application

       1) FROM node:10.4.3
          WORKDIR /usr/src/app 
          COPY package*.json ./
          ADD package.json /usr/src/app/package.json
          RUN npm install
          RUN npm install react-scripts@1.1.0 -g
          COPY . .
          EXPOSE 3000 
          CMD ["npm ","start"];

        2) FROM nginx:1.19.0
           COPY build/ /usr/share/nginx/html

Step:-5
 
        Docker file has to be in the root folder of your project and named as Dockerfile.

        $docker build -t shashank3656/hello .

Step:-6

       Push the docker image which we have created

       $ docker push shashank3656/hello

Step:-7

       Deploying the React Application
       

       Create a deployment.yml file and paste the below code:-

       kind: Deployment
       apiVersion: apps/v1
       metadata:
         name: hello
       spec:
         replicas: 2
         selector:
           matchLabels:
             app: hello
         template:
           metadata:
             labels:
               app: hello
           spec:
              containers:
                - name: hello
                  image: shashank3656/hello
                  imagePullPolicy: Always
                  ports:
                    - containerPort: 80
              restartPolicy: Always


         Create service the file with below code:

          kind: Service
          apiVersion: v1
          metadata:   
            name: hello
          spec:
           type: NodePort
           ports:
             - port: 80
               targetPort: 80
               protocol: TCP
               nodePort: 31000
          selector:
          app: hello  
 

Step:-8 

       Now Deploying our Application to Kubernetes:

       $ kubectl apply -f deployment.yaml

Step:-9

      check the results of cluster 

      $ kubectl get pods

      $  kubectl get deployment

      $  kubectl get service

Step:-10 

       open the browser and copy the id address of the cluster and paste

       


 
    

