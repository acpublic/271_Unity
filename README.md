## 環境構築
### Unityインストール
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
