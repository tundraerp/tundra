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
| Amazon Sagemaker   | Amazon SageMaker is a fully-managed service that covers the entire machine learning workflow to label and prepare your data, choose an algorithm, train the model, tune and optimize it for deployment, make predictions, and take action. For more information visit https://aws.amazon.com/sagemaker/. | **Optional** Amazon Machine Learning is required to use some of Tundra's machine learning capabilities. |
| Amazon ML       | Amazon Machine Learning (Amazon ML) is a robust, cloud-based service that makes it easy for developers of all skill levels to use machine learning technology. For more information visit https://aws.amazon.com/machine-learning/ | **Optional** Amazon Machine Learning is required to use some of Tundra's machine learning capabilities. |
| Google Cloud Vision  | Google Cloud offers computer vision products that use machine learning to help you understand your images with industry-leading prediction accuracy. For more information visit https://cloud.google.com/vision/. | **Optional** Required to use Tundra's OCR tools. |

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
  - After you install the Robo3T client, open up your client and click on the connections (two monitors) icon
  - Click on "Create"
 
**Connection Tab**
  - Type: Replica Set
  - Name: Any name you would like to use to identify your cluster (e.g. TUNDRA-DEV MongoAtlas)
  - Members: (there should be 3)
    - Navigate to your MongoAtlas Cluster (on the MongoAtlas web interface), and click on your cluster name
    - There should be 3 shard names (e.g. cluster0-shard-00-00-abc123.mongodb.net:27017)
    - Add each of these to the Members section
  - Set Name **(Case-sensitive and Required)**: YOURCLUSTERNAME-shard-0 (e.g. Cluster0-shard-0)
  
**Authentication Tab**
  - Perform authentication: check this box
  - Database: admin
  - User Name:
    - Navigate to your MongoAtlas Cluster (on the MongoAtlas web interface), and click on **Database Access**
    - Find a user that has atlasAdmin OR readWriteAnyDatabase privileges
  - Password: The password associated with the username
  - Auth Mechanism: SCRAM-SHA-1
  
**SSL Tab**
  - Use SSL protocol: check this box
  - Authentication Method: Self-signed Certificate
  
After setting the above tabs up, hit Test and your instance should indicate a proper connection with two green checkmarks. If the database does not connect, make sure that you've whitelisted your own IP address in your MongoAtlas Cluster (Network Access) and try again.

Click Save, and your cluster should appear in the list of connections. You should now be able to connect to your MongoAtlas cluster.

Find your ```configuration_local``` database and follow the instructions below.

### Configuration Documents

**Your ```configurations``` Collection**:
You will need to fill in your API keys and credentials into these (9) documents and then add them to your ```configurations``` collection.

**Essential Keys**:
  - ```YOUR_MONGODB_CONNECTION_STRING``` - this value can be found in your MongoAtlas cluster when you hit ```Connect```. This string ends in ```.net``` (e.g. mongodb+srv://YOUR_USERNAME:YOUR_PASSWORD@SHARDNAME.mongodb.net)
  - ```YOUR_MONGODB_REPLICA_SET_NAME``` - this value is the name of your MongoAtlas Cluster (case-sensitive) followed by -```shard-0```. (e.g. if the name of my cluster is Tundra, the replica set name would be ```Tundra-shard-0```)
  - ```YOUR_REDIS_HOST``` - The hostname given when you create a Redis account (for localhost, just set it to ```"127.0.0.1"```)
  - ```YOUR_REDIS_PORT``` - The port of your Redis account (usually 6379)
  - ```YOUR_REDIS_PASSWORD``` - Your Redis password (if you have one)
  - ```YOUR_PATH_TO_SSL_PRIVATE_KEY``` - The path from the root of your TundraERP application to your SSL private key
  - ```YOUR_PATH_TO_SSL_CERTIFICATE_PEM_FILE``` - The path from the root of your TundraERP application to your SSL .pem file
  - ```YOUR_JWT_TOKEN_SECRET``` - this is a random and unique alphanumeric string. you should generate and safe keep your own JWT token secret to be used in your application -- feel free to grab a random ```63 random alpha-numeric characters (a-z, A-Z, 0-9)``` from this website: https://www.grc.com/passwords.htm
  
```**JSON**```
```
/* 1 */
{
    "filepath" : "content/config/extensions/periodicjs.ext.reactapp/development.json",
    "environment" : "development",
    "config" : {
        "settings" : {
            "data_table_props" : {
                "standard" : {
                    "asset" : {
                        "flattenRowDataOptions" : {
                            "maxDepth" : 2
                        }
                    }
                }
            },
            "customIndexButton" : {
                "asset" : {
                    "onclickProps" : {
                        "pathname" : "/r-admin/standard/content/assets/newmodal",
                        "title" : "Upload new assets"
                    },
                    "onClick" : "func:this.props.createModal"
                }
            },
            "auth" : {
                "logged_out_path" : "/",
                "logged_in_homepage" : "/home",
                "enforce_mfa" : false
            },
            "default_user_image" : "/images/elements/avatar.svg",
            "data_tables" : {},
            "userprofile" : {
                "options" : {
                    "headers" : {
                        "entitytype" : "account",
                        "clientid" : "YOUR_REACTAPP_CLIENT_ID",
                        "Content-Type" : "application/json",
                        "Accept" : "application/json"
                    },
                    "method" : "POST"
                },
                "url" : "https://des-dev.tundraerp.com:8788/api/jwt/profile"
            },
            "login" : {
                "options" : {
                    "headers" : {
                        "entitytype" : "account",
                        "clientid" : "YOUR_REACTAPP_CLIENT_ID",
                        "Accept" : "application/json"
                    },
                    "method" : "POST"
                },
                "url" : "https://des-dev.tundraerp.com:8788/api/jwt/token"
            },
            "ui" : {
                "sidebar" : {
                    "use_floating_nav" : true
                },
                "footer" : {
                    "navStyle" : {
                        "display" : "block",
                        "position" : "initial",
                        "borderTopColor" : "#404041",
                        "backgroundColor" : "#404041"
                    }
                },
                "header" : {
                    "customButton" : {
                        "props" : {
                            "buttonProps" : {
                                "icon" : "fa fa-bars",
                                "color" : "isWhite"
                            },
                            "onClick" : "func:this.props.toggleUISidebar"
                        },
                        "component" : "ResponsiveButton"
                    },
                    "userNameStyle" : {
                        "color" : "#333"
                    },
                    "containerStyle" : {
                        "boxShadow" : "none"
                    },
                    "navLabelStyle" : {
                        "color" : "#333"
                    },
                    "useHeaderLogout" : false,
                    "useGlobalSearch" : false,
                    "buttonColor" : "#2b39ba",
                    "color" : "isWhite",
                    "isBold" : false
                },
                "sidebarBG" : "#ffffff",
                "fixedSidebar" : true,
                "notifications" : {
                    "timed_timeout" : 10000,
                    "error_timeout" : 10000
                },
                "initialization" : {
                    "refresh_components" : true,
                    "refresh_navigation" : true,
                    "refresh_manifests" : true,
                    "show_sidebar_overlay" : true,
                    "show_footer" : true,
                    "show_header" : false
                }
            },
            "application" : {
                "use_custom_elements" : true,
                "additional_footer" : true,
                "use_offline_cache" : false,
                "environment" : "development"
            },
            "routerHistory" : "browserHistory",
            "navigationLayout" : {
                "wrapper" : {
                    "style" : {
                        "padding-bottom" : "0",
                        "maxHeight" : "37rem",
                        "width" : "23rem"
                    }
                },
                "container" : {
                    "style" : {
                        "marginTop" : "68px"
                    }
                }
            },
            "includeCoreData" : {
                "navigation" : false,
                "manifest" : false
            },
            "include_index_route" : false,
            "title" : "Tundra DES",
            "name" : "Tundra DES",
            "adminPath" : "",
            "skip_catch_all_route" : true,
            "basename" : "https://des-dev.tundraerp.com:8788",
            "custom_css_stylesheet" : "/container/decision-engine-service-container/stylesheet/des.css",
            "hot_reload" : true,
            "server_side_react" : true
        }
    },
    "updatedat" : ISODate("2019-07-19T14:21:46.344Z"),
    "createdat" : ISODate("2019-07-19T14:21:46.344Z"),
    "container" : "decision-engine-service-container",
    "__v" : 0
}

/* 2 */
{
    "filepath" : "content/config/extensions/periodicjs.ext.oauth2server/development.json",
    "environment" : "development",
    "config" : {
        "settings" : {
            "messages" : {
                "invalid_credentials" : "Invalid credentials. Please reenter your password. If you don’t know your password you can reset it."
            }
        }
    },
    "updatedat" : ISODate("2019-01-24T01:49:41.552Z"),
    "createdat" : ISODate("2019-01-24T01:49:41.552Z"),
    "container" : "periodicjs.container.default",
    "__v" : 0
}

/* 3 */
{
    "filepath" : "content/config/process/runtime.json",
    "config" : {
        "process" : {
            "runtime" : "development"
        }
    },
    "updatedat" : ISODate("2019-08-01T13:58:40.460Z"),
    "createdat" : ISODate("2019-08-01T13:41:11.485Z"),
    "container" : "periodicjs.container.default",
    "__v" : 0
}

/* 4 */
{
    "filepath" : "content/config/container/decision-engine-service-container/development.json",
    "environment" : "development",
    "config" : {
        "settings" : {
            "name" : "TundraERP",
            "version" : "1",
            "company_name" : "TundraERP",
            "company_color_primary" : "#007AFF",
            "company_color_hover" : "#66D2FF",
            "company_logo" : "https://s3-us-west-2.amazonaws.com/tundraerp.com/cloudfiles%2F2017%2F10%2F20%2FTundraERP-Logo-RGB.png",
            "header" : {
                "header_background_color_fade_top" : "#FFFFFF",
                "header_button_text_color" : "#FFFFFF",
                "font_color" : "#414141"
            },
            "footer" : {
                "copyright" : "© 2020",
                "background_color" : "#333333",
                "font_color" : "#FFFFFF",
                "fulltext" : "© 2020 TundraERP, Inc. All Rights Reserved."
            },
            "contact" : {
                "customer_support" : {
                    "phone_number" : "SUPPORT PHONE #",
                    "email" : "SUPPORT EMAIL ADDRESS",
                    "office_hours" : "SUPPORT HOURS"
                }
            },
            "blockPageUILayout" : {
                "component" : "div",
                "children" : [ 
                    {
                        "component" : "Image",
                        "props" : {
                            "src" : "/company_logo.png",
                            "style" : {
                                "width" : "150px",
                                "margin" : "0 0 10px 0"
                            }
                        }
                    }, 
                    {
                        "component" : "Button",
                        "props" : {
                            "className" : "__is_loading",
                            "buttonStyle" : "isOutlined",
                            "color" : "isWhite",
                            "state" : "isLoading",
                            "size" : "isLarge",
                            "style" : {
                                "border" : "none"
                            },
                            "children" : "Loading..."
                        }
                    }
                ]
            },
            "simulation" : {
                "limit" : 100,
                "testcases_count_limit" : 100000,
                "upload_filesize_limit" : 2097152
            },
            "optimization" : {
                "data_source_upload_filesize_limit" : 104857600,
                "data_source_upload_min_filesize" : 10240,
                "batch_process_upload_filesize_limit" : 2097152
            },
            "token_timeout" : false,
            "inactivity_timer" : 7200,
            "socketsettings" : {
                "path" : "https://des-dev.tundraerp.com:8788"
            },
            "twilio" : {
                "accountSid" : "YOUR_TWILIO_ACCOUNTSID",
                "authToken" : "YOUR_TWILIO_AUTHTOKEN",
                "sendingNumber" : NumberLong(YOURPHONENUMBER)
            },
            "mailgun" : {
                "apiKey" : "YOUR_MAILGUN_API_KEY",
                "publicValidationKey" : "YOUR_MAILGUN_PUBLIC_VALIDATION_KEY",
                "sendingEmail" : "YOUR_MAILGUN_SENDING_EMAIL",
                "domain" : "YOUR_MAILGUN_DOMAIN"
            },
            "gridfs" : {
                "db" : "periodic_local",
                "uri" : "YOUR_MONGODB_CONNECTION_STRING"
            },
            "machinelearning" : {
                "key" : "YOUR_AMAZON_MACHINE_LEARNING_USER_KEY",
                "org_max_active_model_count" : 50,
                "org_max_concurrent_model_count" : 1,
                "secret" : "YOUR_AMAZON_MACHINE_LEARNING_USER_SECRET",
                "region" : "YOUR_AMAZON_MACHINE_LEARNING_REGION",
                "cron_interval" : 60000,
                "use_mlcrons" : true,
                "all_models" : [ 
                    "aws", 
                    "sagemaker_xgb", 
                    "sagemaker_ll", 
                    "decision_tree", 
                    "random_forest", 
                    "neural_network"
                ],
                "tundra_models" : {
                    "binary" : [ 
                        "decision_tree", 
                        "random_forest", 
                        "neural_network"
                    ],
                    "categorical" : [ 
                        "decision_tree", 
                        "random_forest", 
                        "neural_network"
                    ],
                    "regression" : [ 
                        "decision_tree"
                    ]
                },
                "providers" : [ 
                    "aws", 
                    "sagemaker_xgb", 
                    "sagemaker_ll"
                ]
            },
            "marketing" : {
                "email_cron" : true
            },
            "default_account" : {
                "organization_name" : "",
                "new_user" : true,
                "forgot_organization" : true
            },
            "sagemaker" : {
                "key" : "YOUR_SAGEMAKER_USER_KEY",
                "bucket" : "YOUR_SAGEMAKER_MODEL_BUCKET",
                "secret" : "YOUR_SAGEMAKER_USER_SECRET",
                "region" : "YOUR_SAGEMAKER_REGION",
                "role" : "YOUR_SAGEMAKER_ARN_ROLE",
                "ll" : {
                    "code_image" : "YOUR_SAGEMAKER_LINEAR_LEARNER_CODE_IMAGE",
                    "training_instance_type" : "ml.m4.2xlarge"
                },
                "xgb" : {
                    "code_image" : "YOUR_SAGEMAKER_XGB_CODE_IMAGE",
                    "training_instance_type" : "ml.m4.2xlarge"
                }
            },
            "stripe" : {
                "token" : "YOUR_STRIPE_TOKEN"
            },
            "advanced_ruleset_upload" : true,
            "premium_machinelearning" : true,
            "dataintegrations" : true,
            "googlevision" : {
                "client_email" : "YOUR_GOOGLE_CLOUD_PLATFORM_EMAIL",
                "key" : "YOUR_GOOGLE_VISION_RSA_KEY",
                "project_id" : "YOUR_GOOGLE_VISION_PROJECT_ID"
            }
        }
    },
    "updatedat" : ISODate("2019-07-12T19:54:28.461Z"),
    "createdat" : ISODate("2019-07-12T19:54:28.461Z"),
    "container" : "decision-engine-service-container",
    "__v" : 0
}

/* 5 */
{
    "filepath" : "content/config/extensions/periodicjs.ext.dblogger/development.json",
    "environment" : "development",
    "config" : {
        "settings" : {},
        "databases" : {
            "dblog" : {
                "options" : {
                    "mongoose_options" : {
                        "replset" : {
                            "rs_name" : "YOUR_MONGODB_REPLICA_SET_NAME"
                        }
                    },
                    "url" : "YOUR_MONGODB_CONNECTION_STRING/error_local"
                },
                "db" : "mongoose"
            }
        }
    },
    "updatedat" : ISODate("2019-07-25T01:49:41.552Z"),
    "createdat" : ISODate("2019-07-25T01:49:41.552Z"),
    "container" : "periodicjs.container.default"
}

/* 6 */
{
    "filepath" : "content/config/extensions/@tundraerp-tundra/reactapp/development.json",
    "environment" : "development",
    "config" : {
        "settings" : {
            "credentials" : {
                "google_places_api" : "YOUR_GOOGLE_PLACES_API_KEY"
            },
            "data_table_props" : {
                "standard" : {
                    "asset" : {
                        "flattenRowDataOptions" : {
                            "maxDepth" : 2
                        }
                    }
                }
            },
            "customIndexButton" : {
                "asset" : {
                    "onclickProps" : {
                        "pathname" : "/r-admin/standard/content/assets/newmodal",
                        "title" : "Upload new assets"
                    },
                    "onClick" : "func:this.props.createModal"
                }
            },
            "auth" : {
                "logged_out_path" : "/auth/sign-in",
                "logged_in_homepage" : "/home",
                "enforce_mfa" : false
            },
            "data_tables" : {},
            "default_user_image" : "/images/elements/avatar.svg",
            "userprofile" : {
                "options" : {
                    "headers" : {
                        "entitytype" : "account",
                        "clientid" : "YOUR_REACTAPP_CLIENT_ID",
                        "Content-Type" : "application/json",
                        "Accept" : "application/json"
                    },
                    "method" : "POST"
                },
                "url" : "https://des-dev.tundraerp.com:8788/api/jwt/profile"
            },
            "login" : {
                "options" : {
                    "headers" : {
                        "entitytype" : "user",
                        "clientid" : "YOUR_REACTAPP_CLIENT_ID",
                        "Accept" : "application/json"
                    },
                    "method" : "POST"
                },
                "url" : "https://des-dev.tundraerp.com:8788/auth/login",
                "organization_required" : true
            },
            "session" : {
                "options" : {
                    "headers" : {
                        "entitytype" : "user",
                        "clientid" : "YOUR_REACTAPP_CLIENT_ID",
                        "Accept" : "application/json"
                    },
                    "method" : "POST"
                },
                "url" : "https://des-dev.tundraerp.com:8788/auth/register_session",
                "host" : "YOUR_REDIS_HOST",
                "port" : "YOUR_REDIS_PORT",
                "auth" : "YOUR_REDIS_PASSWORD",
                "registerUser" : true
            },
            "ui" : {
                "sidebar" : {
                    "use_floating_nav" : true
                },
                "footer" : {
                    "navStyle" : {
                        "position" : "initial",
                        "display" : "block",
                        "borderTopColor" : "#404041",
                        "backgroundColor" : "#404041"
                    }
                },
                "custom_ui_layout" : {
                    "component" : "div",
                    "children" : [ 
                        {
                            "component" : "Image",
                            "props" : {
                                "src" : "/company_logo.png",
                                "style" : {
                                    "width" : "170px",
                                    "margin" : "0 0 10px 0"
                                }
                            }
                        }, 
                        {
                            "component" : "Button",
                            "props" : {
                                "className" : "__is_loading",
                                "buttonStyle" : "isOutlined",
                                "color" : "isWhite",
                                "state" : "isLoading",
                                "size" : "isLarge",
                                "style" : {
                                    "border" : "none"
                                },
                                "children" : "Loading..."
                            }
                        }
                    ]
                },
                "header" : {
                    "customDropdownNav" : true,
                    "profileImageStyle" : {
                        "backgroundPosition" : "center"
                    },
                    "productHeader" : {
                        "layout" : true,
                        "productLinks" : [ 
                            {
                                "text" : "Dashboard",
                                "location" : "/home"
                            }, 
                            {
                                "text" : "Tundra ERP",
                                "product" : true,
                                "name" : "erp_system",
                                "location" : "/tundra/applicationsdashboard",
                                "user" : "/tundra/applicationsdashboard",
                                "className" : "tundra"
                            }, 
                            {
                                "text" : "Decision Engine",
                                "product" : true,
                                "name" : "rules_engine",
                                "location" : "/decision/strategies/all",
                                "user" : "/decision/processing/individual/run",
                                "className" : "simulation"
                            }, 
                            {
                                "text" : "Machine Learning",
                                "product" : true,
                                "name" : "machine_learning",
                                "location" : "/ml/models",
                                "user" : "/ml/processing/individual",
                                "className" : "optimization"
                            }, 
                            {
                                "text" : "OCR Text Recognition",
                                "product" : true,
                                "name" : "text_recognition",
                                "location" : "/ocr/templates",
                                "user" : "/ocr/processing/individual",
                                "className" : "ocr"
                            }, 
                            {
                                "text" : "Company Settings",
                                "type" : "owner",
                                "location" : "/company-settings/company_info",
                                "className" : "settings"
                            }, 
                            {
                                "text" : "My Account",
                                "location" : "/account/profile",
                                "className" : "account"
                            }, 
                            {
                                "text" : "Documentation",
                                "reroute" : "https://docs.tundraerp.com/docs",
                                "className" : "user-guide"
                            }, 
                            {
                                "text" : "Log Out",
                                "logoutUser" : true
                            }
                        ]
                    },
                    "userNameStyle" : {
                        "color" : "inherit",
                        "fontSize" : "16px"
                    },
                    "containerStyle" : {
                        "boxShadow" : "none",
                        "position" : "relative"
                    },
                    "navLabelStyle" : {
                        "fontSize" : "16px",
                        "fontWeight" : "600"
                    },
                    "useHeaderLogout" : false,
                    "useGlobalSearch" : false,
                    "buttonColor" : "#2b39ba",
                    "color" : "isBlack",
                    "isBold" : false
                },
                "sidebarBG" : "#ffffff",
                "fixedSidebar" : true,
                "notifications" : {
                    "hide_login_notification" : true,
                    "timed_timeout" : 10000,
                    "error_timeout" : 10000
                },
                "initialization" : {
                    "refresh_components" : true,
                    "refresh_navigation" : true,
                    "refresh_manifests" : true,
                    "show_sidebar_overlay" : true,
                    "show_footer" : true,
                    "show_header" : false
                }
            },
            "application" : {
                "use_custom_elements" : false,
                "additional_header" : true,
                "additional_footer" : true,
                "use_offline_cache" : false,
                "environment" : "development"
            },
            "routerHistory" : "browserHistory",
            "navigationLayout" : {
                "wrapper" : {
                    "style" : {
                        "padding-bottom" : "0",
                        "maxHeight" : "37rem",
                        "width" : "23rem"
                    }
                }
            },
            "includeCoreData" : {
                "navigation" : false,
                "manifest" : false
            },
            "include_index_route" : false,
            "title" : "Tundra DES",
            "name" : "Tundra DES",
            "adminPath" : "",
            "validateMFAPath" : "/auth/validate_mfa",
            "skip_catch_all_route" : true,
            "basename" : "https://des-dev.tundraerp.com:8788",
            "socketsettings" : {
                "path" : "https://des-dev.tundraerp.com:8788"
            },
            "custom_css_stylesheet" : "/container/decision-engine-service-container/stylesheet/des.css",
            "hot_reload" : true,
            "server_side_react" : true,
            "fileUpload" : {
                "sizeLimit" : 10000000,
                "acceptedMIMETypes" : [ 
                    "image/png", 
                    "image/jpeg", 
                    "image/gif"
                ]
            }
        }
    },
    "updatedat" : ISODate("2019-07-19T14:21:46.344Z"),
    "createdat" : ISODate("2019-07-19T14:21:46.344Z"),
    "container" : "decision-engine-service-container",
    "__v" : 0
}

/* 7 */
{
    "filepath" : "content/config/extensions/periodicjs.ext.packagecloud/development.json",
    "environment" : "development",
    "config" : {
        "settings" : {
            "initialize" : {
                "wait_for_client" : false
            },
            "container" : {
                "name" : "des-dev.tundraerp.com"
            },
            "client" : {
                "region" : "us-east-1",
                "accessKey" : "YOUR_AWS_S3_ACCESS_KEY",
                "accessKeyId" : "YOUR_AWS_S3_ACCESS_KEY_ID",
                "provider" : "amazon"
            },
            "logger" : {
                "aws_key" : "YOUR_AWS_SNS_KEY",
                "aws_secret" : "YOUR_AWS_SNS_SECRET",
                "subscriber" : "YOUR_AWS_SNS_SUBSCRIBER_NUMBER",
                "topic_arn" : "YOUR_AWS_SNS_TOPIC_ARN",
                "region" : "YOUR_AWS_SNS_REGION"
            }
        }
    },
    "updatedat" : ISODate("2019-07-20T21:58:13.659Z"),
    "createdat" : ISODate("2019-07-20T21:58:13.659Z"),
    "container" : "periodicjs.container.default",
    "__v" : 0
}

/* 8 */
{
    "filepath" : "content/config/environment/development.json",
    "environment" : "development",
    "config" : {
        "name" : "Decision Engine Service",
        "application" : {
            "url" : "des-dev.tundraerp.com:8788",
            "hostname" : "des-dev.tundraerp.com",
            "protocol" : "https://",
            "environment" : "development",
            "server" : {
                "http" : {
                    "port" : 8786
                },
                "https" : {
                    "port" : 8788,
                    "ssl" : {
                        "private_key" : "YOUR_PATH_TO_SSL_PRIVATE_KEY",
                        "certificate" : "YOUR_PATH_TO_SSL_CERTIFICATE_PEM_FILE"
                    }
                },
                "socketio" : {
                    "type" : "redis",
                    "redis_config" : {
                        "host" : "YOUR_REDIS_HOST",
                        "port" : "YOUR_REDIS_PORT",
                        "password" : "YOUR_REDIS_PASSWORD"
                    }
                }
            }
        },
        "express" : {
            "views" : {
                "page_data" : {
                    "title" : "DES",
                    "description" : "The DES (`DES`) is an external service that acts as a web portal for internal users/administrators to manage the overall system. "
                }
            },
            "config" : {
                "csrf" : false
            },
            "body_parser" : {
                "urlencoded" : {
                    "limit" : "50mb"
                },
                "json" : {
                    "limit" : "50mb"
                }
            },
            "cookies" : {
                "cookie_parser" : "YOUR_COOKIE_PARSER_KEY"
            },
            "sessions" : {
                "enabled" : true,
                "type" : "redis",
                "store_settings" : {
                    "url" : "redis://:YOUR_REDIS_PASSWORD@YOUR_REDIS_HOST:YOUR_REDIS_PORT",
                    "ttl" : 86400
                }
            }
        },
        "periodic" : {
            "emails" : {
                "server_from_address" : "YOURNOREPLYEMAIL",
                "notification_address" : "YOURNOTIFICATIONEMAIL"
            }
        },
        "databases" : {
            "standard" : {
                "db" : "mongoose",
                "options" : {
                    "url" : "YOUR_MONGODB_CONNECTION_STRING/periodic_local",
                    "mongoose_options" : {
                        "replset" : {
                            "rs_name" : "YOUR_MONGODB_REPLICA_SET_NAME",
                            "keepAlive" : 1,
                            "connectTimeoutMS" : 30000,
                            "socketTimeoutMS" : 30000
                        },
                        "server" : {
                            "keepAlive" : 1,
                            "connectTimeoutMS" : 30000,
                            "socketTimeoutMS" : 30000
                        }
                    }
                },
                "controller" : {
                    "default" : {
                        "protocol" : {
                            "concat_documents" : true,
                            "adapter" : "http",
                            "api" : "rest"
                        },
                        "responder" : {
                            "adapter" : "json"
                        }
                    }
                },
                "router" : {
                    "ignore_models" : []
                }
            }
        },
        "core" : {
            "mailer" : {
                "transport_config" : {
                    "type" : "ses",
                    "transportoptions" : {
                        "accessKeyId" : "YOUR_SES_ACCESS_KEY",
                        "secretAccessKey" : "YOUR_SES_ACCESS_KEY_SECRET"
                    }
                }
            }
        },
        "container" : {
            "type" : "local",
            "name" : "decision-engine-service-container"
        }
    },
    "updatedat" : ISODate("2019-07-11T03:45:58.345Z"),
    "createdat" : ISODate("2019-07-11T03:45:58.345Z"),
    "__v" : 0,
    "container" : "periodicjs.container.default"
}

/* 9 */
{
    "filepath" : "content/config/extensions/periodicjs.ext.passport/development.json",
    "environment" : "development",
    "config" : {
        "settings" : {
            "api" : {
                "secret" : "YOUR_JWT_TOKEN_SECRET"
            },
            "emails" : {
                "welcome" : "content/container/decision-engine-service-container/utilities/views/email/welcome.ejs",
                "new_user" : "content/container/decision-engine-service-container/utilities/views/email/new_user.ejs",
                "forgot" : "content/container/decision-engine-service-container/utilities/views/email/forgot.ejs"
            },
            "routing" : {
                "authenication_route_prefix" : "auth",
                "login" : "login",
                "logout" : "logout",
                "forgot" : "forgot",
                "reset" : "reset",
                "activate" : "activate",
                "register" : "new",
                "signin" : "signin",
                "complete" : "complete",
                "sso" : "sso",
                "userauth" : {
                    "user_core_data" : "user",
                    "account_core_data" : "account"
                }
            },
            "email_subjects" : {
                "welcome" : false,
                "forgot" : false
            },
            "notifications" : {
                "forgot" : "Password reset instructions were sent to your email address",
                "resent_activation" : "Your activation token was resent to your email address",
                "reset" : "Password successfully changed"
            },
            "data" : {
                "user_core_data" : "standard_user",
                "account_core_data" : "standard_account"
            },
            "passport" : {
                "sessions" : true,
                "use_csrf" : false,
                "use_password" : true,
                "default_entitytype" : "user"
            },
            "registration" : {
                "require_activation" : true,
                "require_second_factor" : false,
                "require_password" : true,
                "require_properties" : [ 
                    "email", 
                    "name", 
                    "password"
                ],
                "matched_password_field" : "confirmpassword",
                "token" : {
                    "secret" : "YOUR_JWT_TOKEN_SECRET",
                    "activation_token_expires_minutes" : 525600
                },
                "signin_after_create" : true,
                "use_complexity" : true,
                "use_complexity_setting" : "medium",
                "complexity_settings" : {
                    "weak" : {
                        "uppercase" : 1,
                        "lowercase" : 1,
                        "min" : 8
                    },
                    "medium" : {
                        "uppercase" : 1,
                        "lowercase" : 1,
                        "digit" : 1,
                        "min" : 8
                    },
                    "strong" : {
                        "uppercase" : 1,
                        "lowercase" : 1,
                        "digit" : 1,
                        "special" : 1,
                        "min" : 8
                    }
                }
            },
            "forgot" : {
                "token" : {
                    "secret" : "YOUR_JWT_TOKEN_SECRET",
                    "reset_token_expires_minutes" : 1440
                }
            },
            "timeout" : {
                "use_limiter" : true
            },
            "redirect" : {
                "user" : {
                    "logged_in_homepage" : "/",
                    "logged_out_homepage" : "/",
                    "second_factor_required" : "/login-otp"
                },
                "account" : {
                    "logged_in_homepage" : "/",
                    "logged_out_homepage" : "/",
                    "second_factor_required" : "/login-otp"
                }
            },
            "errors" : {
                "invalid_credentials" : "Invalid credentials"
            },
            "reactapp" : {
                "include_manifests" : true,
                "include_core_data_entity_options" : true
            }
        }
    },
    "updatedat" : ISODate("2019-07-19T01:49:41.552Z"),
    "createdat" : ISODate("2019-07-19T01:49:41.552Z"),
    "container" : "periodicjs.container.default",
    "__v" : 0
}
```

**Your ```extensions``` Collection**:
  - Add these (10) documents to your ```extensions``` collection (no changes are required).



### System Requirements & Required Configurations

Before you can install and run TundraERP's open source platform, you must have set up certain required servers, databases and 3rd-party programs as well added the proper configuration documents to your database. For full details regarding this configuration process, please review the above two sections **System Requirements** and **Database Configuration**.



## License

[MIT](LICENSE)
