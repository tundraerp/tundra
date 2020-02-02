![TundraERP Logo](public/images/logo.png)

# TundraERP - A Modern ERP System with AI
We provide a comprehensive platform built as modular components, empowering businesses to quickly deploy business solutions across a wide range of business verticals, including Manufacturing, CRM, Project Management, HR, Marketing and many more. We also introduce AI and machine learning capabilities wherever possible via AWS Sagemaker, AWS ML and Google Cloud Vision.

## System Requirements

### Operating System Requirements
  - OS X 10.9 or later
  - Ubuntu 14.04 or later

### Coding Languages
  - Node 11
  - NPM 5.6.0 or later (Current version using 6.5.0)
  - React 15.3.2

### Web Browser
An internet browser is required to use TundraERP's web user interfaces. TundraERP's supported browsers include the latest version of Chrome (recommended), Firefox, Safari, or Microsoft Edge.    

### Required 3rd-Party Software
The following 3rd-party software is required to run TundraERP.

| Software        | Description           | Required?  |
|-----------------|-----------------------|------------|
| MongoDB         | MongoDB is a general purpose, document-based, distributed database built for modern application developers and for the cloud era. For more information visit https://www.mongodb.com/. | **Yes** |
| Redis           | Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache and message broker. For more information visit https://redis.io/. | **Yes** |
| Amazon s3       | Amazon Simple Storage Service (Amazon S3) is an object storage service that offers industry-leading scalability, data availability, security, and performance. For more information visit https://aws.amazon.com/s3/. | **Yes** Required for standard cloud deployments. Other options are available for on-premise installations. |
| Twilio          | Twilio enables programmatic phone calls and text messages. For more information visit https://www.twilio.com/. | **Optional** Required for 2-factor phone authentication. |
| Amazon SES      | Amazon SES enables programmatic email sending. For more information visit https://aws.amazon.com/ses/. | **Optional** Required for system emails. |
| AWS Sagemaker   | Amazon SageMaker is a fully-managed service that covers the entire machine learning workflow to label and prepare your data, choose an algorithm, train the model, tune and optimize it for deployment, make predictions, and take action. For more information visit https://aws.amazon.com/sagemaker/. | **Optional** Amazon Machine Learning is required to use some of Tundra's machine learning capabilities. |


## Database Configuration

### General
  - The TundraERP System uses MongoDB as its primary database.
  - Most 3rd-party API keys and credentials are stored in MongoDB documents and then referenced throughout the application.
  - For streamlined setup, we suggest creating a MongoDB Atlas account and create a cluster. You can even start off with the free version of MongoAtlas provided through their website.
  - Please refer to the MongoDB Atlas documentation for setup guidelines (https://docs.atlas.mongodb.com/).
  - Make sure to whitelist relevant IP addresses (on MongoDB Atlas in the Network section) and add a MongoDB Atlas user with readWrite access to use as a part of your connection string.
  - Once your clusters are created (if running locally, you only need one cluster), navigate to the COLLECTIONS section of your cluster and create two databases (one will be for configuration files and one will be for your application data storage). We suggested naming these databases configuration_local and periodic_local.

### Basic Configurations
  - Create two collections inside of the configuration_local database: configurations and extensions.
  - The configurations collection will hold multiple MongoDB configuration documents, which will contain the keys and credentials for various services that you can opt to connect to in your own instance of TundraERP's System.
  - The extensions collection will hold multiple MongoDB documents that reference the extensions installed as a part of the application process.
  - All of your organization's keys and credentials will be stored in the configurations collection. The extensions documents will be more boilerplate.

  > **Tip!**
  For easier insertion of the MongoDB documents discussed below, you can optionally install Robo3T: https://robomongo.org/ and connect to your clusters through the Robo3T application.

### Connecting through Robo3T (After your MongoAtlas cluster is set up) - Highly Recommended


### System Requirements & Required Configurations

Before you can install and run TundraERP's open source platform, you must have set up certain required servers, databases and 3rd-party programs as well added the proper configuration documents to your database. For full details regarding this configuration process, please visit the System Requirements and Database Configuration sections and return to this installation guide afterwards.



## License

[MIT](LICENSE)
