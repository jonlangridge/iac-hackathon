Add the following variables:

"modulesUrl": "https://raw.githubusercontent.com/jonlangridge/iac-hackathon-dsc/master/WebsiteInstall.zip",
"dscScript": "WebsiteInstall.ps1",
"configurationFunction": "Main"


Add the following resource after the Virtual Machine resource:

{
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "[concat(variables('vmName'),'/Microsoft.Powershell.DSC')]",
      "apiVersion": "2015-05-01-preview",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
      ],
      "properties": {
        "publisher": "Microsoft.Powershell",
        "type": "DSC",
        "typeHandlerVersion": "2.20",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "configuration": {
            "url": "[variables('modulesUrl')]",
            "script": "[variables('dscScript')]",
            "function": "[variables('configurationFunction')]"
          },
           "configurationArguments": {
             "nodeName": "[variables('vmName')]"
          }
        },
        "protectedSettings": null
      }
    }