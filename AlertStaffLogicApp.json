{
    "definition": {
        "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
        "actions": {
            "Compare_last_run_time,_if_more_than_an_hour": {
                "actions": {
                    "Create_blob": {
                        "inputs": {
                            "body": "@utcNow()",
                            "headers": {
                                "Content-Type": "text/plain"
                            },
                            "host": {
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "path": "/datasets/default/files",
                            "queries": {
                                "folderPath": "/environment-container\n",
                                "name": "variables.txt",
                                "queryParametersSingleEncoded": true
                            }
                        },
                        "runAfter": {},
                        "runtimeConfiguration": {
                            "contentTransfer": {
                                "transferMode": "Chunked"
                            }
                        },
                        "type": "ApiConnection"
                    },
                    "Send_an_email_(V2)": {
                        "inputs": {
                            "body": {
                                "Body": "<p>An issue has been detected with the environment of the Server Room. Current temperature and humidity are shown below -<br>\n<br>\n@{base64ToString(triggerBody()?['ContentData'])}</p>",
                                "Subject": "Server Room Environment Alert",
                                "To": "shaundavidwilkinson@gmail.com"
                            },
                            "host": {
                                "connection": {
                                    "name": "@parameters('$connections')['office365']['connectionId']"
                                }
                            },
                            "method": "post",
                            "path": "/v2/Mail"
                        },
                        "runAfter": {
                            "Create_blob": [
                                "Succeeded"
                            ]
                        },
                        "type": "ApiConnection"
                    }
                },
                "expression": {
                    "and": [
                        {
                            "greater": [
                                "@ticks(utcNow())",
                                "@ticks(addMinutes(body('Get_blob_content_-_Last_Run'), variables('IntervalBetweenEmailsMinutes')))"
                            ]
                        }
                    ]
                },
                "runAfter": {
                    "Initialize_variable": [
                        "Succeeded"
                    ]
                },
                "type": "If"
            },
            "Get_blob_content_-_Last_Run": {
                "inputs": {
                    "host": {
                        "connection": {
                            "name": "@parameters('$connections')['azureblob']['connectionId']"
                        }
                    },
                    "method": "get",
                    "path": "/datasets/default/files/@{encodeURIComponent(encodeURIComponent('JTJmZW52aXJvbm1lbnQtY29udGFpbmVyJTJmdmFyaWFibGVzLnR4dA=='))}/content",
                    "queries": {
                        "inferContentType": true
                    }
                },
                "metadata": {
                    "JTJmZW52aXJvbm1lbnQtY29udGFpbmVyJTJmdmFyaWFibGVzLnR4dA==": "/environment-container/variables.txt"
                },
                "runAfter": {},
                "type": "ApiConnection"
            },
            "Initialize_variable": {
                "inputs": {
                    "variables": [
                        {
                            "name": "IntervalBetweenEmailsMinutes",
                            "type": "integer",
                            "value": 60
                        }
                    ]
                },
                "runAfter": {
                    "Get_blob_content_-_Last_Run": [
                        "Succeeded"
                    ]
                },
                "type": "InitializeVariable"
            }
        },
        "contentVersion": "1.0.0.0",
        "outputs": {},
        "parameters": {
            "$connections": {
                "defaultValue": {},
                "type": "Object"
            }
        },
        "triggers": {
            "When_a_message_is_received_in_a_queue_(auto-complete)": {
                "inputs": {
                    "host": {
                        "connection": {
                            "name": "@parameters('$connections')['servicebus']['connectionId']"
                        }
                    },
                    "method": "get",
                    "path": "/@{encodeURIComponent(encodeURIComponent('alert_queue'))}/messages/head",
                    "queries": {
                        "queueType": "Main"
                    }
                },
                "recurrence": {
                    "frequency": "Minute",
                    "interval": 5
                },
                "type": "ApiConnection"
            }
        }
    },
    "parameters": {
        "$connections": {
            "value": {
                "azureblob": {
                    "connectionId": "/subscriptions/0274e946-107d-40bd-bf9a-36679b961420/resourceGroups/Honours/providers/Microsoft.Web/connections/azureblob",
                    "connectionName": "azureblob",
                    "id": "/subscriptions/0274e946-107d-40bd-bf9a-36679b961420/providers/Microsoft.Web/locations/ukwest/managedApis/azureblob"
                },
                "office365": {
                    "connectionId": "/subscriptions/0274e946-107d-40bd-bf9a-36679b961420/resourceGroups/Honours/providers/Microsoft.Web/connections/office365",
                    "connectionName": "office365",
                    "id": "/subscriptions/0274e946-107d-40bd-bf9a-36679b961420/providers/Microsoft.Web/locations/ukwest/managedApis/office365"
                },
                "servicebus": {
                    "connectionId": "/subscriptions/0274e946-107d-40bd-bf9a-36679b961420/resourceGroups/Honours/providers/Microsoft.Web/connections/servicebus",
                    "connectionName": "servicebus",
                    "id": "/subscriptions/0274e946-107d-40bd-bf9a-36679b961420/providers/Microsoft.Web/locations/ukwest/managedApis/servicebus"
                }
            }
        }
    }
}
