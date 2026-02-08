{
  "name": "Bapak N8N Indonesia - Auto Clipper Video with Vizard AI",
  "nodes": [
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "db577529-a0b0-4cdd-9c2d-27adcf2ee302",
              "name": "vizard_api_key",
              "value": "API_KEY_HERE",
              "type": "string"
            },
            {
              "id": "bc7aa714-a297-47df-91c8-6ab246521809",
              "name": "video_type",
              "value": "2",
              "type": "string"
            },
            {
              "id": "677c5107-634d-4c9a-921c-c82c99938d7f",
              "name": "video_length",
              "value": "[0]",
              "type": "string"
            },
            {
              "id": "aa6cb6f8-6db8-42de-ba31-e0e11630f29f",
              "name": "video_ratio",
              "value": "1",
              "type": "string"
            },
            {
              "id": "1d7cda63-cd0d-4c45-8df2-97e8e108c39a",
              "name": "video_url",
              "value": "={{ $json['URL Youtube (Bisa di custom mau URL apapun)'] }}",
              "type": "string"
            },
            {
              "id": "c027906c-eafa-4596-8979-dfc045707abd",
              "name": "video_lang",
              "value": "=auto",
              "type": "string"
            },
            {
              "id": "ccc22ee8-7d4e-4204-9fad-91820185b63d",
              "name": "viral_score",
              "value": "={{ $json['Skor Viral (0-100)'] }}",
              "type": "number"
            }
          ]
        },
        "options": {

        }
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [624, -608],
      "id": "d638b82b-7af5-4b50-80a8-15b48cabebec",
      "name": "Configuration"
    },
    {
      "parameters": {
        "url": "=https://elb-api.vizard.ai/hvizard-server-front/open-api/v1/project/query/{{ $json.projectId }}",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "VIZARDAI_API_KEY",
              "value": "={{ $('Configuration').item.json.vizard_api_key }}"
            }
          ]
        },
        "options": {

        }
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [848, -448],
      "id": "170b9a0c-43db-4b86-b489-e44bea3b4e0d",
      "name": "Get Status"
    },
    {
      "parameters": {

      },
      "type": "n8n-nodes-base.wait",
      "typeVersion": 1.1,
      "position": [624, -448],
      "id": "5baefb33-46c0-4026-90a8-ed928c848814",
      "name": "Wait",
      "webhookId": "935bdb16-109c-434c-a084-0e6e4614369b"
    },
    {
      "parameters": {
        "rules": {
          "values": [
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "leftValue": "={{ $json.code }}",
                    "rightValue": 2000,
                    "operator": {
                      "type": "number",
                      "operation": "equals"
                    },
                    "id": "2ecbce10-5138-4833-9c77-0206f38652af"
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "Success"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "id": "c1bad68b-0775-4f12-861d-3d465ee57852",
                    "leftValue": "={{ $json.code }}",
                    "rightValue": 1000,
                    "operator": {
                      "type": "number",
                      "operation": "equals"
                    }
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "Processing"
            }
          ]
        },
        "options": {
          "fallbackOutput": "extra",
          "renameFallbackOutput": "Failed"
        }
      },
      "type": "n8n-nodes-base.switch",
      "typeVersion": 3.3,
      "position": [848, -304],
      "id": "82aa1ddd-4a0d-47a7-95fd-3f845897c58d",
      "name": "Check Status"
    },
    {
      "parameters": {
        "fieldToSplitOut": "videos",
        "options": {

        }
      },
      "type": "n8n-nodes-base.splitOut",
      "typeVersion": 1,
      "position": [384, -128],
      "id": "23bbfff9-fe24-44fe-9a22-93282b4e76f5",
      "name": "Split Clips"
    },
    {
      "parameters": {
        "formTitle": "AI Clipper Video",
        "formDescription": "Auto Clipping, Auto Edit, Auto Post\nBukan zaman nya lagi buat manual-manual an",
        "formFields": {
          "values": [
            {
              "fieldLabel": "URL Youtube (Bisa di custom mau URL apapun)",
              "requiredField": true
            },
            {
              "fieldLabel": "Skor Viral (0-100)",
              "fieldType": "number",
              "requiredField": true
            }
          ]
        },
        "options": {
          "appendAttribution": false,
          "buttonLabel": "Generate Short Video",
          "customCss": ":root {\n\t--font-family: 'Inter', 'Open Sans', system-ui, sans-serif;\n\t--font-weight-normal: 400;\n\t--font-weight-bold: 600;\n\n\t--font-size-body: 12px;\n\t--font-size-label: 14px;\n\t--font-size-test-notice: 12px;\n\t--font-size-input: 14px;\n\t--font-size-header: 20px;\n\t--font-size-paragraph: 14px;\n\t--font-size-link: 12px;\n\t--font-size-error: 12px;\n\n\t--font-size-html-h1: 28px;\n\t--font-size-html-h2: 20px;\n\t--font-size-html-h3: 16px;\n\t--font-size-html-h4: 14px;\n\t--font-size-html-h5: 12px;\n\t--font-size-html-h6: 10px;\n\t--font-size-subheader: 14px;\n\n\t/* Colors — Pastel Red / White / Black */\n\t--color-background: #0c0c10;\n\n\t--color-test-notice-text: #e57373;\n\t--color-test-notice-bg: rgba(229, 115, 115, 0.12);\n\t--color-test-notice-border: rgba(229, 115, 115, 0.35);\n\n\t--color-card-bg: #15151a;\n\t--color-card-border: rgba(255, 255, 255, 0.08);\n\t--color-card-shadow: rgba(0, 0, 0, 0.6);\n\n\t--color-link: #ffffff;\n\t--color-header: #ffffff;\n\t--color-header-subtext: #b6b6c2;\n\t--color-label: #d4d4dc;\n\n\t--color-input-border: rgba(255, 255, 255, 0.14);\n    --color-input-text: #000000;\n\t--color-focus-border: #e57373;\n\n\t--color-submit-btn-bg: #e57373;\n\t--color-submit-btn-text: #ffffff;\n\n\t--color-error: #ef5350;\n\t--color-required: #e57373;\n\n\t--color-clear-button-bg: rgba(255, 255, 255, 0.12);\n\n\t--color-html-text: #e2e2ea;\n\t--color-html-link: #e57373;\n\n\t/* Border Radii */\n\t--border-radius-card: 12px;\n\t--border-radius-input: 10px;\n\t--border-radius-clear-btn: 50%;\n\t--card-border-radius: 12px;\n\n\t/* Spacing */\n\t--padding-container-top: 28px;\n\t--padding-card: 28px;\n\t--padding-test-notice-vertical: 12px;\n\t--padding-test-notice-horizontal: 24px;\n\t--margin-bottom-card: 18px;\n\t--padding-form-input: 14px;\n\t--card-padding: 28px;\n\t--card-margin-bottom: 18px;\n\n\t/* Dimensions */\n\t--container-width: 448px;\n\t--submit-btn-height: 50px;\n\t--checkbox-size: 18px;\n\n\t/* Others */\n\t--box-shadow-card: \n\t\t0px 10px 28px rgba(0, 0, 0, 0.65),\n\t\t0 0 0 1px rgba(255, 255, 255, 0.04);\n\n\t--opacity-placeholder: 0.45;\n}\n"
        }
      },
      "type": "n8n-nodes-base.formTrigger",
      "typeVersion": 2.3,
      "position": [400, -608],
      "id": "0c5ed6e0-4f02-4014-bedd-f95dd1f86b23",
      "name": "On form submission",
      "webhookId": "049c9f82-e3f8-4778-a1bd-f215eeabe4f4"
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "1bYilqYnDEdkkIwFUMf2HEIItSBSFH1CQ4F7KJBC0zG4",
          "mode": "list",
          "cachedResultName": "Auto Clipping Video with Vizard AI",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1bYilqYnDEdkkIwFUMf2HEIItSBSFH1CQ4F7KJBC0zG4/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": 961786042,
          "mode": "list",
          "cachedResultName": "Clip",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1bYilqYnDEdkkIwFUMf2HEIItSBSFH1CQ4F7KJBC0zG4/edit#gid=961786042"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "Status": "Pending",
            "Viral Score": "={{ $json.viralScore }}",
            "Clip URL": "={{ $json.videoUrl }}",
            "Editor URL": "={{ $json.clipEditorUrl }}",
            "Clip Title": "={{ $json.title }}",
            "Viral Reason": "={{ $json.viralReason }}",
            "Clip ID": "={{ $json.videoId }}",
            "Clip Duration (ms)": "={{ $json.videoMsDuration }}",
            "Project ID": "={{ $('Get Status').item.json.projectId }}"
          },
          "matchingColumns": [
            "Source URL"
          ],
          "schema": [
            {
              "id": "Status",
              "displayName": "Status",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Project ID",
              "displayName": "Project ID",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Clip ID",
              "displayName": "Clip ID",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Clip URL",
              "displayName": "Clip URL",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Clip Duration (ms)",
              "displayName": "Clip Duration (ms)",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Clip Title",
              "displayName": "Clip Title",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Viral Score",
              "displayName": "Viral Score",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Viral Reason",
              "displayName": "Viral Reason",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Editor URL",
              "displayName": "Editor URL",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {

        }
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [384, 64],
      "id": "dddfe43f-70ed-494b-bf35-484f048e780c",
      "name": "Append Clips",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "fszf6Tb1uFJbQKzh",
          "name": "Johan Google Sheets"
        }
      }
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "loose",
            "version": 2
          },
          "conditions": [
            {
              "id": "6c00620e-694f-4414-acd8-c46ef6235b24",
              "leftValue": "={{ $json.viralScore }}",
              "rightValue": "={{ $('Configuration').item.json.viral_score }}",
              "operator": {
                "type": "number",
                "operation": "gte"
              }
            }
          ],
          "combinator": "and"
        },
        "looseTypeValidation": true,
        "options": {

        }
      },
      "type": "n8n-nodes-base.filter",
      "typeVersion": 2.2,
      "position": [624, 64],
      "id": "770afb8b-2878-4167-b7bf-a520ce382c38",
      "name": "Filter by Viral Score"
    },
    {
      "parameters": {
        "maxItems": "={{ $('Configuration').item.json.publish_limit }}"
      },
      "type": "n8n-nodes-base.limit",
      "typeVersion": 1,
      "position": [848, 64],
      "id": "6489c145-d80e-4bd2-ae2b-01bd8a052b42",
      "name": "Limit Clips"
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "1bYilqYnDEdkkIwFUMf2HEIItSBSFH1CQ4F7KJBC0zG4",
          "mode": "list",
          "cachedResultName": "Auto Clipping Video with Vizard AI",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1bYilqYnDEdkkIwFUMf2HEIItSBSFH1CQ4F7KJBC0zG4/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Source",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1bYilqYnDEdkkIwFUMf2HEIItSBSFH1CQ4F7KJBC0zG4/edit#gid=0"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "Status": "Success",
            "Project ID": "={{ $json.projectId }}",
            "Source URL": "={{ $('Configuration').item.json.video_url }}",
            "Project Name": "={{ $('Configuration').item.json.project_name }}",
            "Language": "={{ $('Configuration').item.json.video_lang }}",
            "Keywords": "={{ $('Configuration').item.json.keywords }}"
          },
          "matchingColumns": [
            "Source URL"
          ],
          "schema": [
            {
              "id": "Status",
              "displayName": "Status",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Source URL",
              "displayName": "Source URL",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Project Name",
              "displayName": "Project Name",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Language",
              "displayName": "Language",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Keywords",
              "displayName": "Keywords",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Project ID",
              "displayName": "Project ID",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Error Message",
              "displayName": "Error Message",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {

        }
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [624, -128],
      "id": "29c61fa4-4d38-4065-bd32-6fada81b1012",
      "name": "Append Source (Success)",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "fszf6Tb1uFJbQKzh",
          "name": "Johan Google Sheets"
        }
      }
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "1bYilqYnDEdkkIwFUMf2HEIItSBSFH1CQ4F7KJBC0zG4",
          "mode": "list",
          "cachedResultName": "Auto Clipping Video with Vizard AI",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1bYilqYnDEdkkIwFUMf2HEIItSBSFH1CQ4F7KJBC0zG4/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Source",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1bYilqYnDEdkkIwFUMf2HEIItSBSFH1CQ4F7KJBC0zG4/edit#gid=0"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "Status": "Failed",
            "Project ID": "={{ $json.projectId }}",
            "Source URL": "={{ $('Configuration').item.json.video_url }}",
            "Project Name": "={{ $('Configuration').item.json.project_name }}",
            "Language": "={{ $('Configuration').item.json.video_lang }}",
            "Keywords": "={{ $('Configuration').item.json.keywords }}",
            "Error Message": "={{ $json.errMsg }}"
          },
          "matchingColumns": [
            "Source URL"
          ],
          "schema": [
            {
              "id": "Status",
              "displayName": "Status",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Source URL",
              "displayName": "Source URL",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Project Name",
              "displayName": "Project Name",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Language",
              "displayName": "
