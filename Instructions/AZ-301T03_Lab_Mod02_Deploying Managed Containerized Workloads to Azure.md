---
lab:
    title: 'Azure へのコンテナー化されたマネジド ワークロードのデプロイ'
    module: 'モジュール 2: Azure でのマネージド サーバー アプリケーションの作成'
---

# Azure でのマネージド サーバー アプリケーションの作成

# ラボの回答キー: Azure へのコンテナー化されたマネジド ワークロードのデプロイ

## はじめに

1. 次の資格情報を使用して Windows 10 ラボ仮想マシンにログインしていることを確認します:

    - ユーザ名: **Admin**

    - パスワード: **Pa55w.rd**

2. Windows 10 デスクトップの下部にあるタスクバーを確認します。 タスクバーには、ラボで使用する一般的なアプリケーションのアイコンが含まれています:

    - Microsoft Edge

    - File Explorer

    - [Visual Studio Code](https://code.visualstudio.com/)

    - [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)

    - Bash on Ubuntu on Windows

    - Windows PowerShell

    > **注記**: これらのアプリケーションへのショートカットは、**スタート メニュー**で見つけることができます。

## エクササイズ 1: Azure Kubernetes Service (AKS) クラスター の作成:

#### タスク 1: Azure Portal を開きます。

1. タスクバーで、**Microsoft Edge** アイコンをクリックします。

2. 開いているブラウザ ウィンドウで、**Azure Portal** (<https://portal.azure.com>) に移動します。

3. プロンプトが表示された場合は、このラボで使用する Azureサブスクリプションの所有者役割を持つユーザ アカウントで認証します。

#### タスク 2: Cloud Shell を開

1. ポータルの上部にある **Cloud Shell** アイコンをクリックして、新しいシェル インスタンスを開きます。

    > **注記**: **Cloud Shell** アイコンは、*より大きい* 文字と *アンダースコア* 文字の組み合わせで構成されるシンボルです。

2. サブスクリプションを使用して **Cloud Shell** を初めて開く場合は、初めて使用する **Cloud Shell** を設定するウィザードが表示されます。 プロンプトが表示された場合は、**Azure Cloud Shell** ペインで 、**Bash (Linux)** をクリックします。

    > **注記**: **Cloud Shell** の構成オプションが表示されない場合は、このコースのラボで既存のサブスクリプションを使用している可能性が高いと考えられます。 その場合は、次のタスクに直接進みます。

3. **マウントされたストレージがありません** ペインで、**詳細設定を表示** をクリックし、次のタスクを実行します:

    - **Subscription** ドロップダウン リスト エントリ セットをデフォルト値に設定したままにします。

    - **Cloud Shell リージョン** ドロップダウン リストで、Azure リージョンの一致、またはこのラボでリソースをデプロイする場所の近くを選択します。

    - **リソースグループ** セクションで、**新規作成** オプションが選択されていることを確認し、テキスト ボックスに **AADesignLab0401-RG** と入力します。

    - **ストレージ アカウント** セクションで、**新規作成** オプションが選択されていることを確認し、下のテキスト ボックスに、3 - 24 文字と数字の組み合わせで構成される一意の名前を入力します。

    - **ファイル共有** セクションで、**新規作成** オプションが選択されていることを確認し、下のテキスト ボックスに **cloudshell** と入力します。

    - **ストレージの作成** ボタンをクリックします。

4. **Cloud Shell** が初回セットアップ操作が完了するのを待ってから、次のタスクに進みます。

#### タスク 3: Cloud Shell を使用して AKS クラスターを作成します。

1. ポータルの下部にある **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、このタスクで使用するリソース グループの名前を指定する変数を作成します:

    ```sh
    RESOURCE_GROUP='AADesignLab0402-RG'
    ```

2. 「**Cloud Shell**」 コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、デプロイに使用する Azure リージョンを指定する変数を作成します (プレースホルダー `<Azure リージョン>` を、このラボでリソースをデプロイする Azure リージョンの名前に置き換えます)。`az account list-locations` は、サブスクリプションで利用可能なすべての場所を一覧表示します。):

    ```sh
    LOCATION='<Azure region>'
    ```

3. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して新リソース グループを作成します:

    ```sh
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```

4. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して新AKS クラスターを作成します:

    ```sh
    az aks create --resource-group $RESOURCE_GROUP --name aad0402-akscluster --node-count 1 --node-vm-size Standard_DS1_v2 --generate-ssh-keys
    ```

    > **注記**: `--node-vm-size` パラメーターで表される VM サイズの可用性に関するエラー メッセージが表示された場合は、メッセージを確認し、他のお勧めの VM サイズを試してください。

    > **注記**: または、**Cloud Shell** の **PowerShell** では、次のコマンドを実行し、**Restriction** 列の値を確認すると、特定のリージョンでサブスクリプションで使用可能な VM サイズを識別できます (必ず `リージョン`プレースホルダをターゲット領域の名前と置き換えてください):

    ```pwsh
    Get-AzComputeResourceSku | where {$_.Locations -icontains "region"} | Where-Object {($_.ResourceType -ilike "virtualMachines")}
    ```

    > **注記**: **Restriction** 列には、サブスクリプションで使用できない VM サイズの値 **NotAvailableForSubscription** が含まれます。

    > **注記**: 2019 年 2 月 21 日現在、VM サイズ **Standard_DS2_V2** が **westeurope** 1 で利用可能でした。


5. 次のタスクに進む前に、デプロイメントが完了するのを待ちます。

    > **注記**: この作業には最大 10 分かかる場合があります。


#### タスク 4: AKS クラスターに接続します。

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、AKS クラスターにアクセスするための資格情報を取得します：

    ```sh
    az aks get-credentials --resource-group $RESOURCE_GROUP --name aad0402-akscluster 
    ```

2. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して AKS クラスターへの接続性を確認します:

    ```
    kubectl get nodes
    ```

3. **Cloud Shell** コマンド プロンプトで、出力を確認し、ノードが **Ready** ステータスを報告していることを確認します。 正しいステータスが表示されるまで、コマンドを再実行します。
  
> **結果**: このエキササイズを完了したら、新しい AKS クラスターを正常にデプロイできています。


## エクササイズ 2: AKS クラスターとそのコンテナ化されたワークロードの管理。

#### タスク 1: コンテナ化されたアプリケーションを AKS クラスターにデプロイします。 

1. Microsoft Edge ウィンドウで、Azure Portal の **Cloud Shell** プロンプトで次のコマンドを入力し、**Enter** キーを押して、Docker Hub から **nginx** イメージをデプロイします:

    ```
    kubectl create deployment nginx-deployment --image=nginx
    ```

    > **注記**: デプロイメントの名前を入力するときは、必ず小文字を使用してください。 また、「このコマンドは非推奨であり、将来のバージョンでは削除されますが、クラスターは正常に作成されました」という通知も受け取ります。

2. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、Kubernetes ポッドが作成されたことを確認します:

    ```
    kubectl get pods
    ```

3. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押してデプロイメントの状態を識別します:

    ```
    kubectl get deployment
    ```

4. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押してポッドをインターネットから使用できるようにします:

    ```
    kubectl expose deployment nginx-deployment --port=80 --type=LoadBalancer
    ```

    > **注記**: デプロイメントの名前を入力するときは、必ず小文字を使用してください。

5. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、パブリック IP アドレスがプロビジョンされているかどうかを識別します:

    ```
    kubectl get service --watch
    ```

6. **aad0402-akscluster** エントリの **EXTERNAL-IP** 列の値が **\<pending\>** からパブリック IP アドレスに変更されるまで待ってから、**Ctrl-C** キーの組み合わせを押します。**aad0402-akscluster** の **EXTERNAL-IP** 列のパブリック IP アドレスにご注意ください。 

7. Microsoft Edge を起動し、前項の操作で取得した IP アドレスを参照します。 Microsoft Edge に **Welcome to nginx!** メッセージが表示されていることを確認します。


#### タスク 2: コンテナ化されたアプリケーションと AKS クラスタ ノードのスケーリング。 

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押してデプロイメントをスケールします:

    ```
    kubectl scale --replicas=2 deployment/nginx-deployment
    ```

2. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押してデプロイメントをスケールします:

    ```
    kubectl get pods
    ```

    > **注記**: コマンドの出力を確認し、ポッドの数が 2 個に増加したことを確認します。

3. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押してクラスターノードの数をスケール アウトします:

    ```sh
    az aks scale --resource-group $RESOURCE_GROUP --name aad0402-akscluster --node-count 2
    ```

4. 追加ノードのプロビジョニングが完了するまで待ちます。

    > **注記**: この作業には最大 10 分かかる場合があります。 失敗した場合は、`az aks scale` コマンドを再実行します。

5. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、クラスターのスケーリングの結果を確認します:

    ```
    kubectl get nodes
    ```

    > **注記**: コマンドの出力を確認し、ノード数が 2 個に増加したことを確認します。

6. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押してデプロイメントをスケールします:

    ```
    kubectl scale --replicas=10 deployment/nginx-deployment
    ```

7. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、デプロイメントをスケーリングした結果を確認します:

    ```
    kubectl get pods
    ```

    > **注記**: コマンドの出力を確認し、ポッドの数が 10 個に増加したことを確認します。

8. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、クラスターノード間のポッド分布を確認します:

    ```
    kubectl get pod -o=custom-columns=NODE:.spec.nodeName,POD:.metadata.name
    ```

    > **注記**: コマンドの出力を確認し、ポッドが両方のノードに分散されていることを確認します。

9. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押してデプロイメントを削除します:

    ```
    kubectl delete deployment nginx-deployment
    ```

## エクササイズ 3: AKS クラスタ内のポッドのオートスケール 

#### タスク 1: **yaml** ファイルを使用して Kubernetes ポッドをデプロイします。

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、サンプルのコンテナ化されたアプリケーションをダウンロードします:

    ```
    git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
    ```

2. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押してダウンロードしたアプリの場所に移動します:

    ```sh
    cd azure-voting-app-redis
    ```

3. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、アプリケーション **yaml** ファイルの内容を一覧表示します:

    ```sh
    cat azure-vote-all-in-one-redis.yaml
    ```

4. コマンドの出力を確認し、ポッドの定義に次の形式の要求と制限が含まれていることを確認します:

    ```yaml
    resources:
      requests:
         cpu: 250m
      limits:
         cpu: 500m
    ```

5. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して**yaml** ファイルに基づいてアプリケーションをデプロイします:

    ```
    kubectl apply -f azure-vote-all-in-one-redis.yaml
    ```

6. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して Kubernetes ポッドが作成されたことを確認します:

    ```
    kubectl get pods
    ```

7. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、コンテナ化されたアプリケーションのパブリック IP アドレスがプロビジョンされているかどうかを識別します:

    ```
    kubectl get service azure-vote-front --watch
    ```

8. **azure-vote-front** エントリの **EXTERNAL-IP** 列の値が **\<pending\>** からパブリック IP アドレスに変更されるまで待ってから、**Ctrl-C** キーの組み合わせを押します。**azure-vote-fron** の **EXTERNAL-IP** 列のパブリック IP アドレスにご注意ください。  

9. Microsoft Edge を起動し、前項の操作で取得した IP アドレスを参照します。 Microsoft Edge の Web ページに **Azure Voting App** メッセージが表示されていることを確認します。

#### タスク 2: Kubernetes ポッドのオートスケール。

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、サンプルのコンテナ化されたアプリケーションをダウンロードします:

    ```
    cd ..
    git clone https://github.com/kubernetes-incubator/metrics-server.git
    ```

2. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して **Metrics Server** をインストールします:

    ```
    kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.3.6/components.yaml
    ```

3. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して **azure-vote-front** デプロイのオートスケールを設定します:

    ```
    kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
    ```

4. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、オートスケールの状態を表示します:

    ```
    kubectl get hpa
    ```

5. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押してポッドを表示します:

    ```
    kubectl get pods
    ```

    > **注記**: レプリカの数が 3 個 に増していることを確認します。 そうでない場合は、分間待ってから、前の 2 つの操作を再実行します。

6. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押してデプロイメントを削除します:

    ```
    kubectl delete deployment azure-vote-front 
    ```

7. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押してデプロイメントを削除します:

    ```
    kubectl delete deployment azure-vote-back 
    ```

8. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、前の操作で実行したコマンドが正常に完了したことを確認します:

    ```
    kubectl get pods
    ```

9. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して AKS クラスターを削除します:

    ```
    az aks delete --resource-group AADesignLab0402-RG --name aad0402-akscluster --yes --no-wait
    ```

10. **Cloud Shell** ペインを閉じます。

> **復習**: このエキササイズでは、AKS クラスターでポッドのオートスケールを実装しました:


## エクササイズ 4: AKS を使用して DevOps を実装します:

#### タスク 1: AKSで DevOps をデプロイ

    > **注記**: このソリューションは、https://docs.microsoft.com/ja-jp/azure/architecture/example-scenario/apps/devops-with-aksで説明されているコンテナ ソリューションを使用した DevOps に基づいています。

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、Jenkins インスタンスと Grafana コンソールを実行している Linux VM にアクセスするときに認証に使用される SSH キーペアを生成します;

    ```
    ssh-keygen -t rsa -b 2048
    ```

    - キーを保存するファイルの入力を求めるプロンプトが表示された場合は、**Enter** キーを押して、既定値 **(~/.ssh/id_rsa)** をそのまま使用します。

    - パスフレーズの入力を求めるメッセージが表示された場合は、**Enter** キーを 2 回押します。

2. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、新しく生成されたキーペアの公開キーを指定する変数を作成します:

    ```sh
    PUBLIC_KEY=$(cat ~/.ssh/id_rsa.pub)
    ```

3. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、新しく生成されたキーペアの公開キーを指定し、公開キーに含まれる可能性のある特殊文字を考慮する変数を作成します:

    ```sh
    PUBLIC_KEY_REGEX="$(echo $PUBLIC_KEY | sed -e 's/\\/\\\\/g; s/\//\\\//g; s/&/\\\&/g')"
    ```

    > **注記**: または、ファイルを開いて、公開キー文字列をファイルに直接入力することもできます。 

4. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、デプロイメントに使用するリソース グループの名前を指定する変数を作成します:

    ```sh
    RESOURCE_GROUP='AADesignLab0403-RG'
    ```

5. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、デプロイに使用する Azure リージョンを指定する変数を作成します:

    ```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab0402-RG'].location" --output tsv)
    ```

6. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して新リソース グループを作成します:

    ```sh
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```

7. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、サンプル ソリューション内のサービスとリソースの認証用の Azure Active Directory サービス プリンシパルを作成します:

    ```sh
    SERVICE_PRINCIPAL=$(az ad sp create-for-rbac --name AADesignLab0403-SP --output json)
    ```

8. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、新しく作成されたサービス プリンシパルの **appId** 属性を取得します:

    ```sh
    APP_ID=$(echo $SERVICE_PRINCIPAL | jq -r .appId)
    ```

9. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、新しく作成されたサービス プリンシパルの **password** 属性を取得します:

    ```sh
    PASSWORD=$(echo $SERVICE_PRINCIPAL | jq -r .password)
    ```

10. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、サンプル ソリューションのデプロイメントに使用するパラメーター ファイルを作成し、vi インターフェイスで開きます:

    ```sh
    vi ~/parameters.json
    ```

11. **Cloud Shell** コマンド プロンプトで、viエディタインターフェイスでサンプルパラメータファイルの内容を追加します  (**\\allfiles\\AZ-301T03\\Module_02\\Labfiles\\Starter\\parameters.json**):

    ```json
    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "spClientId": {
          "value": "$APP_ID"
        },
        "spClientSecret": {		
          "value": "$PASSWORD"
        },
        "linuxAdminUsername": {
          "value": "Student"
        },
        "linuxAdminPassword": {		
          "value": "Pa55w.rd1234"
        },
        "linuxSSHPublicKey": {		
          "value": "$PUBLIC_KEY_REGEX"
        }
      }
    }
    ```

12. **Cloud Shell** コマンド プロンプトで、vi エディタ インターフェイスで変更内容を保存してファイルを閉じるには、**Esc** キーを押し、**:** を押し、**wq!** と入力し、**Enter** キーを押します。

13. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して**appId** 属性のプレースホルダをパラメーター ファイルの**$APP_ID** 変数の値に置き換えます:

    ```sh
    sed -i.bak1 's/"$APP_ID"/"'"$APP_ID"'"/' ~/parameters.json
    ```

14. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して **password** 属性のプレースホルダをパラメーター ファイルの **$PASSWORD** 変数の値に置き換えます:

    ```sh
    sed -i.bak2 's/"$PASSWORD"/"'"$PASSWORD"'"/' ~/parameters.json
    ```

15. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して**sshPublicKey** パラメーターのプレースホルダをパラメーター ファイルの **$PUBLIC_KEY_REGEX** 変数の値に置き換えます:

    ```sh
    sed -i.bak3 's/"$PUBLIC_KEY_REGEX"/"'"$PUBLIC_KEY_REGEX"'"/' ~/parameters.json
    ```

16. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、プレースホルダがパラメーター ファイル内で正常に置き換えられたことを確認します:

    ```sh
    cat ~/parameters.json
    ```

17. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、GitHub リポジトリに存在する Azure Resource Managerテンプレートを使用してサンプル ソリューションをデプロイします:

    ```sh
    az group deployment create --resource-group $RESOURCE_GROUP --template-uri https://raw.githubusercontent.com/MicrosoftLearning/AZ-301-MicrosoftAzureArchitectDesign/master/allfiles/AZ-301T03/Module_02/LabFiles/Starter/azuredeploy.json --parameters ~/parameters.json
    ```

18. 次のタスクに進む前に、デプロイメントが完了するのを待ちます。

    > **注記**: デプロイメントには最大 15 分かかる場合があります。


#### タスク 2: AKS アーキテクチャ 1 を使用した DevOps を確認します。

1. Azure Portal のハブ メニューで、**リソース グループ** をクリックします。

2. **リソース グループ** ブレードで、**AADesignLab0403-RG** リソース グループを表すエントリーをクリックします。

3. **AADesignLab0403-RG** リソースグループブレードで、リソースのリストを確認し、以下で利用可能な情報と比較します: https://docs.microsoft.com/ja-JP/azure/architecture/example-scenario/apps/devops-with-aks

> **復習**: この演習では、AKSアーキテクチャを使用して DevOps をデプロイしました。


## エクササイズ 5: ラボ リソースの削除

#### タスク 1: Cloud Shell を開

1. ポータルの上部にある **Cloud Shell** アイコンをクリックして、Cloud Shell ペインを開きます。

2. ポータルの下部にある **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、このラボで作成したすべてのリソース グループを一覧表示します:

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab04')]".name --output tsv
    ```

3. 出力に、この実習ラボで作成したリソース グループのみが含まれていることを確認します。 これらのグループは、次のタスクで削除されます。

#### タスク 2: リソース グループ 1 を削除します。

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、押して、**Enter** このラボで作成したリソース グループを削除します: 

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab04')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
    ```

2. ポータルの下部にある **Cloud Shell** プロンプトを閉じます。


> **校閲**: このエキササイズでは、このラボで使用したリソースを削除しました。
