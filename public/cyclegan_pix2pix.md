## cyclegan仓库的训练和测试方法
数据集目录结构
datasets/cyclegan_datasets/wbc_cells
├── trainA/    # A域训练集（马）
├── trainB/    # B域训练集（斑马）
├── testA/     # A域测试
└── testB/     # B域测试
a.不需要文件名一一对应**（非配对）
b.trainA /trainB 名字固定，**不能写成 A/B**（这是很多人报错根源，和 pix2pix 的 combine_A_and_B 目录完全不一样！）
c.图片后缀 jpg/png 都可以

# 训练命令
python train.py --dataroot ./datasets/cyclegan_datasets/wbc_cells --name wbc_cell_cyclegan --model cycle_gan
`--dataroot`：数据集根目录（里面包含 trainA/trainB）
`--name`：实验名字，checkpoint 存在 `checkpoints/wbc_cells/`
`--model cycle_gan` 代表跑 CycleGAN

# 测试命令
双边测试
Test a CycleGAN model (both sides):
        python test.py --dataroot ./datasets/cyclegan_datasets/wbc_hd_cells_to_test --name wbc_cell_cyclegan --model cycle_gan
模型文件夹下（checkpoints/）的会生成latest_net_G_A.pth文件和latest_net_G_B.pth文件，分别代表A到B和B到A的图像转化

单向测试
Test a CycleGAN model (one side only):
        python test.py --dataroot ./datasets/cyclegan_datasets/wbc_hd_cells_to_test --name wbc_cell_cyclegan --model test --no_dropout
这里要注意单向测试时会读取模型文件夹下（checkpoints/）的latest_net_G.pth文件，我们需要测试哪个方向的图像转化，就将对应的模型文件重命名为latest_net_G.pth

## pix2pix仓库的训练和测试方法
数据集目录结构
datasets/pix2pix_datasets/wbc_cells_train_psl/
├─ train/
│  ├─ 001.jpg   # 宽=wA+wB，左A 右B，两张等高拼接
│  ├─ 002.jpg
└─ test/
   ├─ 101.jpg
   └─ 102.jpg
a. 图片必须横向拼接：**左边输入 A，右边目标 B**
b. 必须用仓库里的 `combine_A_and_B.py` 生成
先保证原始图的路径如下
datasets/pix2pix_datasets/wbc_cells_train_psl
├─ A/
├─ train/
│  ├─ 001.jpg   # 宽=wA+wB，左A 右B，两张等高拼接
│  ├─ 002.jpg
├─ test/
│  ├─ 101.jpg
│  ├─ 102.jpg
├─ B/
├─ train/
│  ├─ 001.jpg   # 宽=wA+wB，左A 右B，两张等高拼接
│  ├─ 002.jpg
├─ test/
│  ├─ 101.jpg
│  ├─ 102.jpg
然后使用如下图片合并脚本
python datasets/combine_A_and_B.py --fold_A ./datasets/pix2pix_datasets/wbc_cells_train_psl/A --fold_B ./datasets/pix2pix_datasets/wbc_cells_train_psl/B --fold_AB./datasets/pix2pix_datasets/wbc_cells_train_psl

会自动生成
datasets/pix2pix_datasets/wbc_cells_train_psl
├─ testAB/
├─ train/
│  ├─ 001.jpg   # 宽=wA+wB，左A 右B，两张等高拼接
│  ├─ 002.jpg
├─ test/
│  ├─ 101.jpg
│  ├─ 102.jpg

训练命令
python train.py --dataroot ./datasets/pix2pix_datasets/wbc_cells_train_psl --name wbc_cell_B2A_20261112 --model pix2pix --direction BtoA 

测试命令
python test.py --dataroot ./datasets/pix2pix_datasets/neu_org_test --name wbc_cell_B2A_20261112 --model test --netG unet_256 ---norm instance
这里注意
a.显示指定--model test 进入测试模式，它会把所有输入图当作喂给加载的 G，它会自动切到 single 数据集模式
b.无需指定--direction，测试模式没有方向概念，它会把所有输入图当作喂给加载的 G，它会自动切到 single 数据集模式
c.显式指定 --netG unet_256，因为测试时默认使用resnet_9blocks，与训练时存在差异会报错
d.显式指定 ---norm batch，因为测试时默认使用 instance ，与训练时存在差异会报错
        
