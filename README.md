Ready to use React-native Application with Hasura
=================================================

Introduction
------------

This is a fully working react-native app with a Hasura backend. You can clone it and modify as per your requirements. It has basic BAAS features implemented.

- When you clone this quickstart project, there are two tables (article and author) in your database populated with some data.
```:bash
Note: This is just to get you familiar with the system. You can delete these tables whenever you like.
```
- There is a login screen in this app where the authentication is managed by the Hasura Auth APIs.
- Then we make data API calls to get the list of articles and their authors.
- The functions that make these calls are in the `react-native/src/hasuraApi.js` file. Modify it as you like and the changes will reflect in the app.

In order to get this app running, you must have the following:
1. [hasura CLI tool](https://docs.hasura.io/0.15/manual/install-hasura-cli.html) (hasura).

2. Expo client (XDE). Download from https://expo.io/tools

(For more such apps, check out https://hasura.io/hub)

Getting the project
-----------
- Login with Hasura
```
$ hasura login
```
- Clone this project and enter the directory
```
$ hasura quickstart hello-react-native && cd hello-react-native
```
- From project directory, run
```
$ cd react-native && npm install
```

 Pushing your project to the cluster
-------------------
- To get cluster information, run `hasura cluster status`. Info will be of the following form.
```
INFO Reading cluster status...
INFO Status:
Cluster Name:       athlete80
Cluster Alias:      hasura
Kube Context:       athlete80
Platform Version:   v0.15.3
Cluster State:      Synced
```
- Set the cluster name in your project by modifying `react-native -> src -> hasuraApi.js`
```:javascript
const clusterName = athlete80;
```
- Run the following commands to finally push your project to your Hasura cluster.
```
$ git add .
$ git commit -m "Commit message"
$ git push hasura master
```
**The app is now ready to use!!**

Opening the app
---------
- Open Expo XDE, do a login/signup and click on `Open existing project...`. Browse to the hello-react-native directory and open the app folder.
- Once the project loads, click on Share.
- Scan the QR code using the Expo app from your phone (Install from Playstore/Appstore)
- Fully working app will open on your phone


```
Note: You can open the app with any of your desired react-native simulators. We prefer Expo because of its simple onboarding for beginners.
```
Migrating an existing project
-----------------------------
- Replace react-native directory with your pre-existing react-native project directory.
- run `npm install` from this new directory
- App is ready

About Data APIs
---------
- Hasura provides instant data APIs to make powerful data queries. For example, to select "id" and "title" all rows from the article table, make this query to https://data.cluster-name.hasura-app.io/v1/query/
```:json
{
    "type":"select",
    "args":{
        "table":"article",
        "columns":[
            "title",
            "id"
        ],
        "where":{
            "author_id":4
        }
    }
}
```
- App uses the above query and renders the list of articles as shown below.

![List of articles](https://github.com/hasura/hello-react-native/raw/master/readme-assets/list.png)

- You can also exploit relationships. In the pre-populated schema, the author table has a relationship to the article table. The app uses the following query to render the article page.
```:json
{
    "type":"select",
    "args":{
        "table":"article",
        "columns":[
            "title",
            "content"
            "id",
            {
                "name": "author",
                "columns":[
                    "name",
                    "id"
                ]
            }
        ],
        "where":{
            "author_id":4
        }
    }
}
```
![List of articles](https://github.com/hasura/hello-react-native/raw/master/readme-assets/article.png)

- The API-console has awesome features that show you the amazing things you can do with the data APIs. Run this command from your project directory to open the API-console.
```
$ hasura api-console
```

About Auth APIs
---------
- Auth is the gateway of any enterprise level application. Hasura gives you a flexibility to implement almost every popular login mechanism (mobile, email, facebook, google etc) in your app.
- In this application, we are using just the normal username password login. You can implement whichever login you need. The auth screen looks like this.

![List of articles](https://github.com/hasura/hello-react-native/raw/master/readme-assets/auth.png)

- You can try out all the auth APIs in the API console. Check out.
```
$ hasura api-console
```

Filestore
------
- Securely add, remove, manage, update files such as pictures, videos, documents using Hasura filestore.
- Read more about our filestore [here](https://docs.hasura.io/0.15/manual/files/index.html).
- Try uploading a file to your database from the api-console.

Custom microservices
---------
- Sometimes you might need to add new microservices/APIs as per your requirements. In such cases, you can deploy your microservices with Hasura using git push or docker.
- This quickstart comes with one such custom microservice written in nodejs using the express framework. Check it out in action at https://api.cluster-name.hasura-app.io . Currently, it just returns a "Hello-React" at that endpoint.
- This microservice is in the microservices folder of the project directory. You can add your custom microservice there.
- In case you want to use another language/framework for your custom microservice. Take a look at our docs to see how you can add a new custom microservice.
