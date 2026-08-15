# 環境構築
- https://qiita.com/siruku6/items/a556b63cd3e840ab9961
## Unityインストール
- https://unity.com/ja
### Unity基礎セットアップ
- Unity Editorインストール

```
# repository をクローン
$ git clone https://github.com/Unity-Technologies/Unity-Robotics-Hub.git

# Dockerfile のあるディレクトリへ移動
$ cd Unity-Robotics-Hub/tutorials/ros_unity_integration

# Docker image 作成
$ docker build -t foxy -f ros2_docker/Dockerfile .

# Docker コンテナ起動
$ docker run -it --rm -p 10000:10000 foxy /bin/bash

# コンテナ内で、 Unity-Robotics-Hub のサーバを起動
root@c1630e8588aa:/home/dev_ws# ros2 run ros_tcp_endpoint default_server_endpoint --ros-args -p ROS_IP:=0.0.0.0
```
### Project作成
- New project
- テンプレート「Get Started With Unity」
- Create project
### ROS2通信用セットアップ
- メニュー内の「window」->「Package Management」->「Package Manager」
- 「+」ボタン ->「Install package from git URL ...」
- 以下を入力し、「install」ボタンをクリック
```
https://github.com/Unity-Technologies/ROS-TCP-Connector.git?path=/com.unity.robotics.ros-tcp-connector
```
## ROS通信の設定
- 「Robotics」->「ROS Settings」
- 「Protocol」を「ROS2」に変更
### デモによる動作確認
```
# Dockerfile のあるディレクトリへ移動
$ cd Unity-Robotics-Hub/tutorials/ros_unity_integration

# ----------- 1. Docker コンテナを起動する（起動前の場合） -----------
$ docker run -it --rm -p 10000:10000 foxy /bin/bash

# ----------- 2. Docker コンテナ内へアクセスする（起動済の場合） -----------
# 起動されているコンテナ名を確認
$ docker container ls
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                           NAMES
e960f81518b9   foxy      "/ros_entrypoint.sh …"   4 minutes ago   Up 3 minutes   0.0.0.0:10000->10000/tcp, :::10000->10000/tcp   hardcore_johnson

# 確認した Name のコンテナへアクセス
$ docker exec -it hardcore_johnson /bin/bash

# 結果
root@e960f81518b9:/home/dev_ws#
```
```
root@e960f81518b9:/home/dev_ws# colcon build
root@e960f81518b9:/home/dev_ws# source install/setup.bash
```
- source install/setup.bashを実行しても何も起きない
- 「Robotics」->「Generate ROS Messages..」
- 「ROS message path」の部分にWSLの中にあるディレクトリである、"tutorials/ros_unity_integration/ros_packages/unity_robotics_demo_msgs"ディレクトリへのフルパスを記載
- フォルダ選択例
```
\\wsl.localhost\Ubuntu/home/user_name/unity_dev/Unity-Robotics-Hub/tutorials/ros_unity_integration/ros_packages/unity_robotics_demo_msgs
```
- 「Build 2 srvs」と「Build 2 msgs」をクリック
## Publisherによるデモ動作確認
### C#ソースコードの作成
- 「Assets」->「SourceFiles」->「Scripts」を開いた状態で、「RosPublisherExample」という空のファイルを追加
- 「Create」->「Scripting」->「Empty C# Script」をクリック、空のファイルを作成。ファイル名を"RosPublisherExample"に変更
- ファイルが開き、中身を消して、Unity-Robotics-Hubのドキュメントに記載されているRosPublisherExample.csのコードを記載して保存
-> https://github.com/Unity-Technologies/Unity-Robotics-Hub/blob/main/tutorials/ros_unity_integration/publisher.md#create-unity-publisher
### 回転体オブジェクトの追加
- 「+」ボタンを押下し、「3D Object」->「Cube」
- 「+」ボタン ->「Create Empty」の順にをクリックし、オブジェクトの名前を"RosPublisher"
### C#ソースをCubeオブジェクトにアタッチ
- 「Project」タブにある「RosPublisherExample」を「Inspector」タブの中のCubeの属性欄までドラッグ&ドロップ
- 「Hierarchy」タブの中のCubeを、追加した「Ros Publisher Example」の中のCubeと書かれた入力フォーム（None と表示されている）までドラッグ&ドロップ

### ゲーム起動と通信結果の確認
- Unityのプログラムウィンドウの上部中央にある、「▶」マークのボタンをクリック
- 起動していたWSL側の標準出力
```
root@e960f81518b9:/home/dev_ws# ros2 run ros_tcp_endpoint default_server_endpoint --ros-args -p ROS_IP:=0.0.0.0
```
- Docker コンテナ内にいるターミナルでコマンド実行
```
source install/setup.bash
ros2 topic echo pos_rot
```
