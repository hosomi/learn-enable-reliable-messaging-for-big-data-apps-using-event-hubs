# learn-enable-reliable-messaging-for-big-data-apps-using-event-hubs

https://docs.microsoft.com/ja-jp/learn/modules/enable-reliable-messaging-for-big-data-apps-using-event-hubs/

## ��

Learn �ł� Java �ł����AC# �ō\�z���܂��B  
�i��x�A Learn �ʂ�ɉ��K���Ă��� C# �Ŏ��{����Ɨ������[�܂�܂��B�j  

* Azrue Portal (Concierge Subscription)
* VisualStudio 2019

Microsoft Learn �ŗ��p�ł��� Concierge Subscription �𗘗p���܂��B  
�쐬�������\�[�X�̓T���h�{�b�N�X��L���ɂ��Ă���� 4 ���ԗ��p�ł��܂��B  
�� 4 ���Ԍo�ߌ�A�쐬�������\�[�X�͍폜����܂��i�ė��p�s�ł��j�B  

## 1. Azure CLI �p�̊���l��ݒ�

���[�J���ɃC���X�g�[������ Azure CLI�A�R���\�[���� PowerShell �ōs���Ă��܂��B


``az configure --defaults group=<defaults group> location=<location>`` :

* ``<defaults group>`` : ���\�[�X�O���[�v�̊���l�B
* ``<location>`` : ���[�W�����̊���l

```powershell
> az configure --defaults group=learn-416afc63-dc23-4ae1-a5ad-826c4d7ea99c location=japanwest
```

���\�[�X�O���[�v�̓T���h�{�b�N�X�̊���l�𗘗p���܂��B  
���[�W�����͐����{���w�肵�Ă��܂��B  

## 2. Azure �Ƀ��O�C��

``az login`` : 

�u���E�U���N������̂Ńt�H�[���F�؂���B  

```powershell
> az login
You have logged in. Now let us find all the subscriptions to which you have access...
The following tenants don't contain accessible subscriptions. Use 'az login --allow-no-subscriptions' to have tenant level access.
********-****-****-****-************
********-****-****-****-************
[
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "********-****-****-****-************",
    "id": "********-****-****-****-************",
    "isDefault": true,
    "managedByTenants": [],
    "name": "Concierge Subscription",
    "state": "Enabled",
    "tenantId": "********-****-****-****-************",
    "user": {
      "name": "***********@*****.***",
  }
]
```

## 3. Azure �ɕK�v�ȃ��\�[�X���쐬

### 3.1 Azure CLI ���g�p���ăC�x���g �n�u���쐬

#### 3.1.1 Event Hubs ���O��Ԃ��쐬����

``az eventhubs namespace create --name <name>`` : 

* ``--name`` : ��ӂ̖��O���w��

```powershell
> az eventhubs namespace create --name ehubns-hosomi
{
  "clusterArmId": null,
  "createdAt": "2020-08-06T02:04:06.533000+00:00",
  "encryption": null,
  "id": "/subscriptions/********-****-****-****-************/resourceGroups/learn-416afc63-dc23-4ae1-a5ad-826c4d7ea99c/providers/Microsoft.EventHub/namespaces/ehubns-hosomi",
  "identity": null,
  "isAutoInflateEnabled": false,
  "kafkaEnabled": true,
  "location": "Japan West",
  "maximumThroughputUnits": 0,
  "metricId": "********-****-****-****-************:ehubns-hosomi",
  "name": "ehubns-hosomi",
  "provisioningState": "Succeeded",
  "resourceGroup": "learn-416afc63-dc23-4ae1-a5ad-826c4d7ea99c",
  "serviceBusEndpoint": "https://ehubns-hosomi.servicebus.windows.net:443/",
  "sku": {
    "capacity": 1,
    "name": "Standard",
    "tier": "Standard"
  },
  "tags": {},
  "type": "Microsoft.EventHub/Namespaces",
  "updatedAt": "2020-08-06T02:04:59.620000+00:00",
  "zoneRedundant": false
}
```

#### 3.1.2 �C�x���g �n�u�̐ڑ���������擾

``az eventhubs namespace authorization-rule keys list --name <name> --namespace-name <ehubns-name>`` : 

* ``<name>`` : RootManageSharedAccessKey
* `<ehubns-name>` : 3.1 �ō쐬�����T�[�r�X�o�X�̃��\�[�X���B

```powerhsell
> az eventhubs namespace authorization-rule keys list `
>>     --name RootManageSharedAccessKey `
>>     --namespace-name ehubns-hosomi
{
  "aliasPrimaryConnectionString": null,
  "aliasSecondaryConnectionString": null,
  "keyName": "RootManageSharedAccessKey",
  "primaryConnectionString": "Endpoint=sb://ehubns-hosomi.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=*******************************************",
  "primaryKey": "*******************************************",
  "secondaryConnectionString": "Endpoint=sb://ehubns-hosomi.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=*******************************************",
  "secondaryKey": "*******************************************"
}
```

�\�[�X�R�[�h�ŗ��p���܂��B  


#### 3.1.3 �C�x���g �n�u���쐬����

``az eventhubs eventhub create --name <name> --namespace-name <namespace-name>`` : 

* ``<name>`` : �쐬����C�x���g�n�u�̈�ӂ̖��O�B
* ``<namespace-name>`` : 3.1 �ō쐬�����T�[�r�X�o�X�̃��\�[�X���B

```powershell
> az eventhubs eventhub create --namespace-name ehubns-hosomi --name hubname-hosomi
{
  "captureDescription": null,
  "createdAt": "2020-08-06T02:24:22.980000+00:00",
  "id": "/subscriptions/********-****-****-****-************/resourceGroups/learn-416afc63-dc23-4ae1-a5ad-826c4d7ea99c/providers/Microsoft.EventHub/namespaces/ehubns-hosomi/eventhubs/hubname-hosomi",
  "location": "Japan West",
  "messageRetentionInDays": 7,
  "name": "hubname-hosomi",
  "partitionCount": 4,
  "partitionIds": [
    "0",
    "1",
    "2",
    "3"
  ],
  "resourceGroup": "learn-416afc63-dc23-4ae1-a5ad-826c4d7ea99c",
  "status": "Active",
  "type": "Microsoft.EventHub/Namespaces/EventHubs",
  "updatedAt": "2020-08-06T02:24:24.163000+00:00"
}
```

### 3.2 �X�g���[�W�A�J�E���g�̍쐬

#### 3.2.1 �ėp�� Standard �X�g���[�W �A�J�E���g���쐬


``az storage account create --name <name> --sku Standard_RAGRS --encryption-service blob`` : 

* ``<name>`` : �쐬����X�g���[�W�A�J�E���g�̖��O�i�p���������̂ݎg���܂��j�B

```powershell
> az storage account create --name storagenamehosomi --sku Standard_RAGRS --encryption-service blob
{
  "accessTier": "Hot",
  "allowBlobPublicAccess": null,
  "azureFilesIdentityBasedAuthentication": null,
  "blobRestoreStatus": null,
  "creationTime": "2020-08-06T02:32:17.075736+00:00",
  "customDomain": null,
  "enableHttpsTrafficOnly": true,
  "encryption": {
    "keySource": "Microsoft.Storage",
    "keyVaultProperties": null,
    "requireInfrastructureEncryption": null,
    "services": {
      "blob": {
        "enabled": true,
        "keyType": "Account",
        "lastEnabledTime": "2020-08-06T02:32:17.138227+00:00"
      },
      "file": {
        "enabled": true,
        "keyType": "Account",
        "lastEnabledTime": "2020-08-06T02:32:17.138227+00:00"
      },
      "queue": null,
      "table": null
    }
  },
  "failoverInProgress": null,
  "geoReplicationStats": null,
  "id": "/subscriptions/********-****-****-****-************/resourceGroups/learn-416afc63-dc23-4ae1-a5ad-826c4d7ea99c/providers/Microsoft.Storage/storageAccounts/storagenamehosomi",
  "identity": null,
  "isHnsEnabled": null,
  "kind": "StorageV2",
  "largeFileSharesState": null,
  "lastGeoFailoverTime": null,
  "location": "japanwest",
  "minimumTlsVersion": null,
  "name": "storagenamehosomi",
  "networkRuleSet": {
    "bypass": "AzureServices",
    "defaultAction": "Allow",
    "ipRules": [],
    "virtualNetworkRules": []
  },
  "primaryEndpoints": {
    "blob": "https://storagenamehosomi.blob.core.windows.net/",
    "dfs": "https://storagenamehosomi.dfs.core.windows.net/",
    "file": "https://storagenamehosomi.file.core.windows.net/",
    "internetEndpoints": null,
    "microsoftEndpoints": null,
    "queue": "https://storagenamehosomi.queue.core.windows.net/",
    "table": "https://storagenamehosomi.table.core.windows.net/",
    "web": "https://storagenamehosomi.z31.web.core.windows.net/"
  },
  "primaryLocation": "japanwest",
  "privateEndpointConnections": [],
  "provisioningState": "Succeeded",
  "resourceGroup": "learn-416afc63-dc23-4ae1-a5ad-826c4d7ea99c",
  "routingPreference": null,
  "secondaryEndpoints": {
    "blob": "https://storagenamehosomi-secondary.blob.core.windows.net/",
    "dfs": "https://storagenamehosomi-secondary.dfs.core.windows.net/",
    "file": null,
    "internetEndpoints": null,
    "microsoftEndpoints": null,
    "queue": "https://storagenamehosomi-secondary.queue.core.windows.net/",
    "table": "https://storagenamehosomi-secondary.table.core.windows.net/",
    "web": "https://storagenamehosomi-secondary.z31.web.core.windows.net/"
  },
  "secondaryLocation": "japaneast",
  "sku": {
    "name": "Standard_RAGRS",
    "tier": "Standard"
  },
  "statusOfPrimary": "available",
  "statusOfSecondary": "available",
  "tags": {},
  "type": "Microsoft.Storage/storageAccounts"
}
```

#### 3.2.2 �A�N�Z�X �L�[�����ׂĈꗗ�\��

``az storage account keys list --account-name <account-name>`` : 

* ``<account-name>`` : 4.1 �ō쐬�����X�g���[�W�A�J�E���g�̖��O

```powershell
> az storage account keys list --account-name storagenamehosomi
[
  {
    "keyName": "key1",
    "permissions": "Full",
    "value": "****************************************************************************************"
  },
  {
    "keyName": "key2",
    "permissions": "Full",
    "value": "****************************************************************************************"
  }
]
```

#### 3.2.3 �쐬�����X�g���[�W�A�J�E���g�̐ڑ���������擾

``az storage account show-connection-string -n <n>`` : 

* ``<n>`` : 4.1 �ō쐬�����X�g���[�W�A�J�E���g�̖��O

```powershell
> az storage account show-connection-string -n storagenamehosomi
{
  "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=storagenamehosomi;AccountKey=***********************************************************************"
}
```

�����ŗ��p���܂��B


#### 3.2.4 �X�g���[�W �A�J�E���g���ɃR���e�i�[ messages ���쐬


az storage container create -n messages --connection-string "<connectionString>"

* ``<connectionString>`` : 4.3 �Ŏ擾�����ڑ�������i�ڑ�������̓_�u���N�E�H�[�g�ň͂��Ă��������j�B

```powershell
> az storage container create -n messages --connection-string "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=storagenamehosomi;AccountKey=***********************************************************************"
{
  "created": true
}
```


## 4. Azure Portal �C�x���g�n�u�̊m�F����

### 4.1 �쐬�����C�x���g�n�u�̃v���p�e�B

![�쐬�����C�x���g�n�u�̃v���p�e�B](learn-enable-reliable-messaging-for-big-data-apps-using-event-hubs-01.png)

### 4.2 ���M�A��M���̃��g���b�N�m�F

���M�� ``Sender`` �T�u�v���W�F�N�g�����s���Ă��������B  
�ڑ������񓙂� ``Sender/Program.cs`` �̒萔 ``EventHubConnectionString``, ``EventHubName`` �������g�̊��ɍ��킹�ĕύX���Ă��������B  

``dotnet run --project Sender`` :
�i�\�����[�V���������Ŏ��s�ł��܂��B�j  

```powershell
> dotnet run --project Sender
```

�@  
��M�� ``Receiver`` �T�u�v���W�F�N�g�����s���Ă��������B  
�ڑ������񓙂� ``Receiver/Program.cs`` �̒萔 ``EventHubConnectionString``, ``EventHubName``, ``StorageContainerName``, ``StorageAccountName``, ``StorageAccountKey`` �������g�̊��ɍ��킹�ĕύX���Ă��������B  

``dotnet run --project Receiver`` :
�i�\�����[�V���������Ŏ��s�ł��܂��B�j  

```powershell
> dotnet run --project Receiver
```


![���M��M���̃��g���b�N�m�F](learn-enable-reliable-messaging-for-big-data-apps-using-event-hubs-02.png)

### 4.3 �������̊m�F

![�������̊m�F](learn-enable-reliable-messaging-for-big-data-apps-using-event-hubs-03.png)


