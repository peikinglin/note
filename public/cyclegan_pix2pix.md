# cyclegan仓库的训练和测试方法
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
这里要注意单向测试时会读取模型文件夹下（checkpoints/）的latest_net_G.pth文件，我们需要测试哪个方向的图像转化，就将对应的模型文件重命名为latest_net_G.pth
Test a CycleGAN model (one side only):
        python test.py --dataroot ./datasets/cyclegan_datasets/wbc_hd_cells_to_test --name wbc_cell_cyclegan --model test --no_dropout

        
