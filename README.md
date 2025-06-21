# Computer Vision Lab 3

## Task 1



#############################################################################################

**TODO**

#############################################################################################





## Task 2

### 2.1 数据集下载

从 https://drive.google.com/file/d/1PoW996lQMy4-XDj46SaqC9UO7BUnjqoT/view?usp=sharing  链接中下载包含 Task 2 所用数据的文件夹 `data`，并将其放在 `gaussian-splatting` 目录下。

最后使用如下命令进入工作目录 `gaussian-splatting` ：

```powershell
cd gaussian-splatting
```



### 2.2 训练模型

若想对 `lego` 数据集进行训练，可执行如下指令：

- 训练：

  ```powershell
  python train.py `
  --source_path data/lego `
  --model_path output/lego `
  --eval
  ```

- 渲染：

  ```powershell
  python render.py `
  --model_path output/lego `
  --source_path data/lego `
  --skip_train
  ```

- 拼接：

  ```powershell
  ffmpeg -y -framerate 30 `
  -i output/lego/test/ours_40000/renders/%05d.png `
  -c:v libx264 -pix_fmt yuv420p output/lego/lego_orbit.mp4
  ```

- 评估：

  ```powershell
  python metrics.py -m output/lego
  ```

若想对 `long` 数据集进行训练，可执行如下指令：

- 训练：

  ```powershell
  python train.py `
  --source_path data/long `
  --model_path output/long `
  --eval
  ```

- 渲染：

  ```powershell
  python render.py `
  --model_path output/long `
  --source_path data/long `
  --skip_train
  ```

- 拼接：

  ```powershell
  ffmpeg -y -framerate 1 `
    -i output/long/test/ours_40000/renders/%05d.png `
    -vf "scale=trunc(iw/2)*2:trunc(ih/2)*2" `
    -c:v libx264 -pix_fmt yuv420p `
    output/long/long_orbit.mp4
  ```

- 评估：

  ```powershell
  python metrics.py -m output/long
  ```



### 2.3 使用预训练模型

若不想进行训练，可从 https://drive.google.com/file/d/1TkvA6i0MRTHc-d5ggiFGdLbdaKP7NLRG/view?usp=sharing  链接中下载包含 Task 2 预训练模型权重的文件夹 `output`，并将其放在工作目录 `gaussian-splatting` 下，最后执行之前的渲染、拼接与评估指令即可。

