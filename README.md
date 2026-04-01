# Streamlit app base template

> **ローカル検証ポート:** `task dev` で起動すると **http://localhost:8080** でアプリにアクセスできます。

In this repository you will find an empty Streamlit application base template to kickstart custom application development. The Datarobot client is already set up for you to use. It uses the application owner's API key by default.

## クイックスタート

```shell
# 1. 初期セットアップ
task start        # .env作成 + 依存インストール
# .env を編集して DATAROBOT_API_TOKEN 等を設定

# 2. ローカル検証
task dev          # Streamlit を localhost:8080 で起動

# 3. DataRobot へデプロイ（新規作成）
dr task run deploy   # または task deploy

# 4. 既存アプリの置換（.env に DATAROBOT_APPLICATION_ID を設定済み）
dr task run deploy   # 既存アプリのソースを更新
```

## Setup

You can run the app using a custom application or by running the Streamlit app directly. Custom applications can be created either via the NextGen Registry's **Applications** page or by using [DRApps](https://github.com/datarobot/dr-apps/blob/main/README.md).

Be sure to define the required variables for the app to communicate with DataRobot. If you run the app locally or in another environment than a custom application, you'll need to set the env variables. When this app is run via a custom application, the variables are set automatically.

```shell
#start-app.sh
export token="$DATAROBOT_API_TOKEN"  # Your API key, accessed from DataRobot's Developer Tools page
export endpoint="$DATAROBOT_ENDPOINT"  # Example: https://app.datarobot.com/api/v2/
```

## Add and use runtime parameters

To add runtime parameters, create a `metadata.yaml` file in your application source folder. Here is an example of a `DEPLOYMENT_ID` that creates 
an environment variable called `MLOPS_RUNTIME_PARAM_DEPLOYMENT_ID`:

```yaml
runtimeParameterDefinitions:
- fieldName: DEPLOYMENT_ID
  type: string
```

Once this file is part of your application source in DataRobot, it displays the new runtime parameter(s) as part of the
app configuration.

To use the parameters, DataRobot recommends you add them via `start-app.sh`. Add the following conditional export before `streamlit run` starts:

```shell
if [ -n "$MLOPS_RUNTIME_PARAM_DEPLOYMENT_ID" ]; then
  export deployment_id="$MLOPS_RUNTIME_PARAM_DEPLOYMENT_ID"
fi
```

You can now use `os.getenv("deployment_id")` within your application code. DataRobot suggests you add every new environment variable to the `initiate_session_state` function, that way they can be used across the app with ease.

## Streamlit configuration file

This base template comes with a `config.toml` file in the `src/.streamlit` directory. You can adjust your app preferences in this file ([read more](https://docs.streamlit.io/develop/concepts/configuration/theming)). There are some preset defaults show in the code below.

```toml
[browser]
gatherUsageStats = false            # Disables component usage tracking by Streamlit

[theme]
base="dark" 
primaryColor="#297ab4"              # Accent color of user interaction elements (button, checkbox, etc.)
backgroundColor="#0e1117"           # Background for the main content area
secondaryBackgroundColor="#22272b"  # Background for sidebar and various interactive widgets

[client]
toolbarMode = "minimal"             # Hides the Streamlit actions from the toolbar (clear cache, rerun, custom themes)
```
