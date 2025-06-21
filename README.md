# Computer Vision Lab 3

## Task 1

注：该任务所有的操作需要在`TensoRF`目录下进行：

```
cd TensoRF
```

### 1.1 数据和模型准备

**数据集下载**

你可以在终端执行以下代码：

```
mkdir data
cd data
wget https://drive.google.com/file/d/1ztjSvKEP2M0nTv6xebmFrTKW7QRkvs2q/view?usp=sharing
unzip nerf_sythetic.zip
cd ..
```

或者从 https://drive.google.com/file/d/1ztjSvKEP2M0nTv6xebmFrTKW7QRkvs2q/view?usp=sharing 手动下载数据集，解压后放在新建的`data`目录下。

**模型下载**

你可以使用：

```
wget https://drive.google.com/file/d/17OA8q9XNlsp0hG5-Wq4Tn97c4PS7B0Sx/view?usp=drive_link
```

或者手动从 https://drive.google.com/file/d/17OA8q9XNlsp0hG5-Wq4Tn97c4PS7B0Sx/view?usp=drive_link 下载模型文件`tensorf_lego_VM.th`

### 1.2 环境配置

你可以直接在终端执行：

```
conda create -n TensoRF python=3.8
conda activate TensoRF
pip install torch torchvision
pip install tqdm scikit-image opencv-python configargparse lpips imageio-ffmpeg kornia lpips tensorboard
```

### 1.3 训练

如果你想从头训练模型，只需要简单输入：

```
python train.py --config configs/lego.txt
```

完整训练过程在一张NVIDIA A100-SXM4-80GB上约耗时13分钟。训练完成后，你可以在`log/tensorf_lego_VM`下找到模型文件、tensorboard日志文件与可视化结果。

### 1.4 渲染

要查看我们训练好的模型在测试集上的渲染效果，可以使用：

```
python train.py --config configs/lego.txt --ckpt ./tensorf_lego_VM.th --render_only 1 --render_test 1 
```

渲染完成后，`TensoRF`目录下会出现`tensor_lego_VM/imgs_test_all`文件夹，其中包含了（1）测试集的渲染图像`xxx.png`（2）渲染出的轨迹视频`video.mp4`和`depthvideo.mp4`（3）训练过程中的平均指标`mean.txt`，里面的四行数据分别对应PSNR，SSIM，LPIPS(AlexNet)和LPIPS(VDD-16)。



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

