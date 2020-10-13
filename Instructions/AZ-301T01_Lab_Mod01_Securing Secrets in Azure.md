---
lab:
    title: 'Azure でシークレットの保護'
    module: 'モジュール 1: Azure ソリューションのセキュリティと ID の管理'
---

# Azure ソリューションのセキュリティと ID の管理 

# ラボの回答キー: Azure でシークレットの保護

## はじめに

1. 次の認証情報を使用して Windows 10 ラボ仮想マシンにログインしていることを確認します。

    - ユーザー名: **Admin**

    - パスワード: **Pa55w.rd**

1. Windows 10 デスクトップの下部にあるタスク バーを確認します。タスク バーには、ラボで使用する一般的なアプリケーションのアイコンがあります。

    - Microsoft Edge

    - ファイル エクスプローラ

    - [Visual Studio Code](https://code.visualstudio.com/)

    - [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)

    - Ubuntu または Windows 上の Bash

    - Windows PowerShell

    > **注意**: これらのアプリケーションへのショートカットは、「**スタート メニュー**」 にあります。

## 演習 1: Key Vault リソースのデプロイ

#### タスク 1: Azure Portalを開く

1. タスク バーで、「**Microsoft Edge**」 アイコンをクリックします。

1. 開いているブラウザ ウィンドウで、**Azure Portal** (<https://portal.azure.com>) に移動します。

1. プロンプトが表示された場合は、このラボで使用する Azure サブスクリプションの所有者役割を持つユーザー アカウントを使用して認証します。

#### タスク 2: Key Vaultのデプロイ

1. Azure portal の左上隅にある **リソースの作成** をクリックします。

1. **新規** ブレードの上部にある **マーケットプレースの検索** テキストボックスに、 **Key Vault** と入力し、**Enter** キーを押します。

1. **すべて** のブレードで、検索結果から **Key Vault** をクリックします。

1. **Key Vault** ブレードで、 **作成** ボタンをクリックします。   

1. **Key Vaultの作成** ブレードで、次のタスクを実行します。

    - **名前** のテキストボックスに、グローバルに一意の値を入力します。

    - **サブスクリプション** ドロップダウンリストエントリは、既定値に設定したままにします。

    - **リソース グループ** セクションで、 **新規作成** オプションを選択して、テキストボックスに **AADesignLab0901-RG** と入力します。

    - **場所** ロップダウン リストで、このラボでリソースをデプロイするAzureリージョンを選択します。

    - **価格レベル** ブレードで **価格レベル** をクリックし、**標準** をクリックした後、**選択**をクリックします。

    - 残りの設定はすべて既定値のままにしておきます。

    - **作成** ボタンをクリックします。

1. プロビジョニングが完了するのを待ってから、次のタスクに進みます。

#### タスク 3: Azure Portalを使用してKey Vaultにシークレットを追加する

1. Azure Portalのハブメニューで、**リソースグループ** をクリックします。

1. **リソースグループ** ブレードで、 **AADesignLab0901-RG** をクリックします。

1. **AADesignLab0901-RG** ブレードで、新しく作成されたKey Vaultを表すエントリをクリックします。

1. 「Key Vault」ブレードで **シークレット** をクリックします。

1. 「Key Vaultシークレット」ブレードで、ペインの上部にある **生成/インポート** ボタンをクリックします。

1. **シークレットの作成** ブレードで、次のタスクを実行します。

    - **アップロードオプション** ドロップダウン リストで、 **手動** エントリが選択されていることを確認します。

    - **名前** テキストボックスに **thirdPartyKey** と入力します。

    - **値** テキストボックスに、値 **56d95961e597ed0f04b76e58** を入力します。

    - 残りの設定はすべて既定値のままにしておきます。

    - **作成** ボタンをクリックします。

#### タスク 4: Cloud Shellを開く

1. Portalの上部にある **Cloud Shell** アイコンをクリックして、新しいシェルインスタンスを開きます。

    > **注意**: **Cloud Shell** アイコンは、*より大きい* 文字と *下線* 文字の組み合わせで構成される記号です。

1. サブスクリプションを使用して **Cloud Shell** を初めて開く場合は、初めて使用する **Cloud Shell** を設定するウィザードが表示されます。プロンプトが表示された場合は、 **Azure Cloud Shellへようこそ** ウィンドウで、 **Bash (Linux)** をクリックします。

    > **注意**: **Cloud Shell** の環境設定オプションが表示されない場合は、このコースのラボで既存のサブスクリプションを使用している可能性が高いと考えられます。その場合は、次のタスクに直接進みます。 

1. **ストレージがマウントされていない** ウィンドウで、 **詳細設定を表示** をクリックし、次のタスクを実行します。

    - **サブスクリプション** ドロップダウンリストエントリは、既定値に設定したままにします。

    - **Cloud Shell リージョン** ドロップダウン リストで、このラボでリソースをデプロイする場所と一致する、またはその近くにある Azure リージョンを選択します

    - **リソースグループ** セクションで、**既存のものを使用** オプションを選択し、ドロップダウンリストで**AADesignLab0901-RG** を選択します。

    - **ストレージ アカウント** セクションで、 **新規作成** オプションが選択されていることを確認し、下のテキストボックスに3 - 24つの文字と数字の組み合わせで構成される一意の名前を入力します。 

    - **ファイル共有** セクションで **新規作成** オプションが選択されていることを確認し、下のテキストボックスに **cloudshell** と入力します。

    - 「**ストレージの作成**」 ボタンをクリックします。

1. **Cloud Shell** が初回セットアップ手順を完了するのを待ってから、次のタスクに進みます。

#### タスク 5: CLI を使用してKey Vaultにシークレットを追加する

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、 **Enter** キーを押して、この演習の前半でデプロイした Azure Key Vaultが含まれているリソース グループの名前を指定する変数を作成します。

```sh
    RESOURCE_GROUP='AADesignLab0901-RG'
```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、 **Enter** キーを押して、この演習の前半で作成したAzure Key Vaultの名前を取得します。

```sh
    KEY_VAULT_NAME=$(az keyvault list --resource-group $RESOURCE_GROUP --query "[0].name" --output tsv)
```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** キーを押してKey Vaultのシークレットを一覧表示します。

```sh
    az keyvault secret list --vault-name $KEY_VAULT_NAME --output json
```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、 **Enter** キーを押して、 **thirdPartyKey** シークレットの値が表示します。

```sh
    az keyvault secret show --vault-name $KEY_VAULT_NAME --name thirdPartyKey --query value --output tsv
```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** キーを押してKey Vaultに新しいシークレットを追加します。

```sh
    az keyvault secret set --vault-name $KEY_VAULT_NAME --name firstPartyKey --value 56f8a55119845511c81de488
```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** キーを押してKey Vaultのシークレットを一覧表示します。

```sh
    az keyvault secret list --vault-name $KEY_VAULT_NAME --query "[*].{Id:id,Created:attributes.created}" --out table
```

1. **Cloud Shell** ペインを閉じます。

#### タスク 6: Azure Resource Manager テンプレートを使用して、Key Vaultにシークレットを追加する

1. Azure portal の左上隅にある **リソースの作成** をクリックします。

1. **新規** ブレードの上部にある **マーケットプレースの検索** テキストボックスに、**Template Deployment** と入力し、**Enter** キーを押します。

1. **すべて** ブレードの検索結果で **Template Deployment** をクリックします。

1. **カスタムデプロイ** ブレードで、**作成** ボタンをクリックします。

1. **カスタムデプロイ** ブレードで、**エディターで独自のテンプレートを作成** リンクをクリックします。

1. **テンプレートの編集** ブレードで、**ファイルの読み込み** をクリックします。

1. **アップロードするファイルを選択** ダイアログ ボックスで、**\\allfiles\\AZ-301T01\\Module_01\\LabFiles\\Starter\\** フォルダーに移動して、**secret-template.json** ファイルを選択し、**開く** をクリックします。これにより、テンプレート エディタ ペインに次のコンテンツが読み込まれます。

```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "vaultName": {
                "type": "string"
            }
        },
        "variables": {
            "secretName": "vmPassword"
        },
        "resources": [
            {
                "apiVersion": "2016-10-01",
                "type": "Microsoft.KeyVault/vaults/secrets",
                "name": "[concat(parameters('vaultName'), '/', variables('secretName'))]",
                "properties": {
                    "contentType": "text/plain",
                    "value": "StudentPa$$w.rd"
                }
            }
        ]
    }
```

1. **保存** ボタンをクリックしてテンプレートを保持します。

1. **カスタムデプロイ** ブレードに戻って、次のタスクを実行します。

    - **サブスクリプション** ドロップダウンリストエントリは、既定値に設定したままにします。

    - **リソースグループ** セクションで、**既存のものを使用** オプションを選択し、ドロップダウンリストで **AADesignLab0901-RG** を選択します。

    - 「**Vault名**」 テキストボックスに、この演習の前半で作成したKey Vaultの名前を入力します。

    - **確認および作成** ボタンをクリックし、次に **作成** をクリックします。

1. デプロイが完了するのを待たずに、次の手順に進みます。

1. Azure portal の左上隅にある **リソースの作成** をクリックします。

1. **新規** ブレードの上部にある **マーケットプレースの検索** テキストボックスに、**Template Deployment** と入力し、**Enter** キーを押します。

1. **すべて** ブレードの検索結果で **Template Deployment** をクリックします。

1. **カスタムデプロイ** ブレードで、**作成** ボタンをクリックします。

1. **カスタムデプロイ** ブレードで、**エディターで独自のテンプレートを作成** リンクをクリックします。

1. **テンプレートの編集** ブレードで、**ファイルの読み込み** をクリックします。

1. 「**アップロードするファイルを選択**」 ダイアログ ボックスで、**\\allfiles\\AZ-301T01\\Module_01\\LabFiles\\Starter\\** フォルダーに移動して、**storage-template.json** ファイルを選択し、「**開く**」 をクリックします。これにより、テンプレート エディタ ペインに次のコンテンツが読み込まれます。

```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "vaultName": {
                "type": "string"
            }
        },
        "variables": {
            "secretName": "storageConnectionString",
            "storageName": "[concat('stor', uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2017-10-01",
                "type": "Microsoft.Storage/storageAccounts",
                "name": "[variables('storageName')]",
                "location": "[resourceGroup().location]",
                "kind": "Storage",
                "sku": {
                    "name": "Standard_LRS"
                },
                "properties": {                
                }
            },
            {
                "apiVersion": "2016-10-01",
                "type": "Microsoft.KeyVault/vaults/secrets",
                "name": "[concat(parameters('vaultName'), '/', variables('secretName'))]",
                "dependsOn": [
                    "[resourceId('Microsoft.Storage/storageAccounts', variables('storageName'))]"
                ],
                "properties": {
                    "contentType": "text/plain",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageName'), ';', 'AccountKey=', listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageName')), providers('Microsoft.Storage', 'storageAccounts').apiVersions[0]).keys[0].value, ';')]"
                }
            }
        ]
    }
```

1. **保存** ボタンをクリックしてテンプレートを保持します。

1. 「**カスタムデプロイ**」 ブレードに戻って、次のタスクを実行します。

    - **サブスクリプション** ドロップダウンリストエントリは、既定値に設定したままにします。

    - **リソースグループ** セクションで、**既存のものを使用** オプションを選択し、ドロップダウンリストで **AADesignLab0901-RG** を選択します。

    - **Vault 名** フィールドに、この演習の前半で作成したKey Vaultの名前を入力します。

    - **確認および作成** ボタンをクリックし、**作成** をクリックします。

1. デプロイが完了するのを待ってから、次のタスクに進みます。

#### タスク 7: Key Vaultシークレットを表示

1. Azure portalのハブメニューで、**リソースグループ** をクリックします。

1. **リソースグループ** ブレードで、**AADesignLab0901-RG** をクリックします。

1. **AADesignLab0901-RG** ブレードで、この演習の前半で作成したKey Vaultを表すエントリをクリックします。

1. 「Key Vault」ブレードで **シークレット** をクリックします。

1. 「Key Vaultシークレット」ブレードで、このラボ中に作成されたシークレットの一覧を確認します。

1. **vmPassword** シークレットを表すエントリをクリックします。

1. **vmPassword** ブレードで、シークレットの現在のバージョンを表すエントリをクリックします。

1. 「シークレットバージョン」ブレードで、**シークレット値を表示** ボタンをクリックします。

1. シークレットの値が、前のタスクでデプロイしたテンプレートに含まれているシークレットの値と一致していることを確認します。

> **確認**: この演習では、**Key Vault** インスタンスを作成し、いくつかの異なる方法を使用してKey Vaultにシークレットを追加しました。


## 演習 2: Key Vault シークレットを使用して Azure VM をデプロイする

#### タスク 1: キー コンテナーのリソース ID パラメーターの値を取得する

1. Portalの上部にある **Cloud Shell** アイコンをクリックして、新しいClould Shellインスタンスを開きます。

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** キーを押して、ハブ仮想ネットワークが含まれるリソースグループの名前を指定する変数を作成します。

```
    RESOURCE_GROUP='AADesignLab0901-RG'
```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キー を押して、この演習の前半で作成した Azure Key Vaultリソース ID を取得します。

```sh
    KEY_VAULT_ID=$(az keyvault list --resource-group $RESOURCE_GROUP --query "[0].id" --output tsv)
```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、Azure Key Vaultリソース ID の名前を指定し、リソース ID に含まれる特殊文字を考慮する変数を作成します。

```sh
    KEY_VAULT_ID_REGEX="$(echo $KEY_VAULT_ID | sed -e 's/\\/\\\\/g; s/\//\\\//g; s/&/\\\&/g')"
```

#### タスク 2: Azure Resource Managerのデプロイ テンプレートとパラメーター ファイルを準備する

1. **Cloud Shell** ペインで、**ファイルのアップロード/ダウンロード** アイコンをクリックし、ドロップダウンメニューで **アップロード** をクリックします。 

1. 「**開く**」 ダイアログ ボックスで、**\\allfiles\\AZ-301T01\\Module_01\\LabFiles\\Starter\\** フォルダーに移動して、**vm-template.json** ファイルを選択し、「**開く**」 をクリックします。 

1. **Cloud Shell** ペインで、**ファイルのアップロード/ダウンロード** アイコンをクリックし、ドロップダウンメニューで **アップロード** をクリックします。 

1. 「**開く**」 ダイアログ ボックスで、**\\allfiles\\AZ-301T01\\Module_01\\LabFiles\\Starter\\** フォルダーに移動して、**vm-template.parameters.json** ファイルを選択し、「**開く**」 をクリックします。 

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** キーを押して、 **vm-template.parameters.json** パラメーターファイルの **$KEY_VAULT_ID**パラメーターのプレースホルダーを **$KEY_VAULT_ID** 変数の値に置き換えます。        

```sh
    sed -i.bak1 's/"$KEY_VAULT_ID"/"'"$KEY_VAULT_ID_REGEX"'"/' ~/vm-template.parameters.json
```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter**キーを押して、パラメーターファイルでプレースホルダーが正常に置き換えられたことを確認します。

```sh
    cat ~/vm-template.parameters.json
```

#### タスク 3: Azure Resource Manager テンプレートをデプロイするためのKey Vaultを設定する

1. Azure Portalのハブメニューで、 **リソースグループ** をクリックします。

1. **リソースグループ** ブレードで、 **AADesignLab0901-RG** をクリックします。

1. 「**AADesignLab0901-RG**」 ブレードで、前の演習で作成したKey Vaultを表すエントリをクリックします。

1. 「Key Vault」 ブレードで、「**アクセス ポリシー**」 をクリックします。

1. [**アクセス ポリシー**] ブレードの [**アクセスの有効化:**] 領域で、[**Azure Resource Manager(テンプレートの展開用)**] チェックボックスを選択します。

1. ペインの上部にある **保存** ボタンをクリックします。

#### タスク 4: Key Vault シークレットを使用して、パスワード パラメーターが設定された Linux VM をデプロイします。

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter**キーを押して、指定したパラメーターファイルを使用してAzure Resource Managerテンプレートをデプロイします。

```sh
    az group deployment create --resource-group $RESOURCE_GROUP --template-file ~/vm-template.json --parameters @~/vm-template.parameters.json
```

1. デプロイが完了するのを待ってから、次のタスクに進みます。

#### タスク 5: デプロイの結果を確認する

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter**キーを押して、新しくデプロイされたAzure VMが含まれているリソースグループの名前を指定する変数を作成します。

```
    RESOURCE_GROUP='AADesignLab0901-RG'
```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** キーを押して、ローカル管理者アカウントのパスワードの値を保存するシークレットを含んでいる Azure Key Vault名前を取得します。

```sh
    KEY_VAULT_NAME=$(az keyvault list --resource-group $RESOURCE_GROUP --query "[0].name" --output tsv)
```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter**キーを押してシークレットの値を取得します。

```sh
    az keyvault secret show --vault-name $KEY_VAULT_NAME --name vmPassword --query value --output tsv
```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** キーを押して、前のタスクでデプロイした Azure VM のパブリック IP アドレスを取得します。

```sh
    PUBLIC_IP=$(az network public-ip list --resource-group $RESOURCE_GROUP --query "[0].ipAddress" --output tsv)
```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** キーを押してSSH 経由でAzure VMに接続します。

```sh
    ssh Student@$PUBLIC_IP
```

1. **Cloud Shell** コマンド プロンプトで、接続を続行するかどうかを確認するメッセージが表示された場合は、`はい`と入力し、**Enter** キーを押します。

1. **Cloud Shell** コマンド プロンプトで、パスワードの入力を求めるプロンプトが表示されたら、このタスクの前半で取得したシークレットの値を入力し、 **Enter** キーを押します。

1. 認証に成功したことを確認します。

1. **Cloud Shell** コマンドプロンプトで、`exit`と入力して、Azure VMからログアウトします。

> **確認**: この演習では、Key Vaultシークレットとして保存されているパスワードを使用してLinux VMをデプロイしました。

## 演習 3: 課題 リソースの削除

#### タスク 1: Cloud Shellを開く

1. Portalの上部にある **Cloud Shell** アイコンをクリックして、「Cloud Shell」ペインを開きます。

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter**キーを押して、このラボで作成したすべてのリソースグループを一覧表示します。

```
    az group list --query "[?starts_with(name,'AADesignLab09')]".name --output tsv
```

1. このラボで作成したリソース グループのみが出力に含まれていることを確認します。これらのグループは、次のタスクで削除されます。

#### タスク 2: リソース グループの削除

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** キーを押してこのラボで作成したリソースグループを削除します。

```sh
    az group list --query "[?starts_with(name,'AADesignLab09')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
```

1. Portalの下部にある **Cloud Shell** プロンプトを閉じます。

> **確認**: この演習では、このラボで使用したリソースを削除しました。
