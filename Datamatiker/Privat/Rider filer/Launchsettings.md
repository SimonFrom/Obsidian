{  
  "$schema": "http://json.schemastore.org/launchsettings.json",  
    "iisSettings": {  
      "windowsAuthentication": false,  
      "anonymousAuthentication": true,  
      "iisExpress": {  
        "applicationUrl": "http://localhost:14832",  
        "sslPort": 44302  
      }  
    },    "profiles": {  
      "http": {  
        "commandName": "Project",  
        "dotnetRunMessages": true,  
        "launchBrowser": true,  
        "inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}",  
        "applicationUrl": "http://localhost:5028",  
        "environmentVariables": {  
          "ASPNETCORE_ENVIRONMENT": "Development",  
          "ASPNETCORE_Kestrel__Certificates__Default__Path": "/home/sf/.localdevca/localhost.crt",  
          "ASPNETCORE_Kestrel__Certificates__Default__KeyPath": "/home/sf/.localdevca/localhost.key"  
        }  
      },      "https": {  
        "commandName": "Project",  
        "dotnetRunMessages": true,  
        "launchBrowser": true,  
        "inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}",  
        "applicationUrl": "https://localhost:7118;http://localhost:5028",  
        "environmentVariables": {  
          "ASPNETCORE_ENVIRONMENT": "Development",  
          "ASPNETCORE_Kestrel__Certificates__Default__Path": "/home/sf/.localdevca/localhost.crt",  
          "ASPNETCORE_Kestrel__Certificates__Default__KeyPath": "/home/sf/.localdevca/localhost.key"  
        }  
      },      "IIS Express": {  
        "commandName": "IISExpress",  
        "launchBrowser": true,  
        "inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}",  
        "environmentVariables": {  
          "ASPNETCORE_ENVIRONMENT": "Development"  
        }  
      }    
    }  
}



