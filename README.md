# Connectomics
## Homefolder:
On Research Computer: /n/pfister_lab2/Lab/zhjin

## Datasets and Notebooks:
### DeepFlux:

#### Notebooks:
skeleton_visualization_and_label_process.ipynb

#### Datasets:

SK-LARGE-MIX-GAUSSIAN/

SK-LARGE-MIX-GAUSSIAN-DEGREES/

SK-LARGE-MIX-GAUSSIAN-POS/

SK-LARGE-MIX-GAUSSIAN-RADIUS/

SK-LARGE-pred/

SK-LARGE-PRED.h5

SK-LARGE-test/

SK-LARGE-test-DEGREES/

SK-LARGE-test-EDGES/

SK-LARGE-test.h5

SK-LARGE-test-POS/

### Dyer:

#### Notebooks:

#### Datasets:

v1_anno_dense_bv_cell_corrected.h5

v1_img_255.h5

V2_anno_dense_bv_cell_corrected.h5

V3_bv_cleaned.h5

V3_bv_cleaned_single.h5

V3_img.h5

### NagCT: 

#### Notebooks:

nagCT_process_train_label.ipynb

nagCT_precision_recall.ipynb

#### Datasets:

Zeiss_Nag/


## Commands:


FAC RC: ssh zhjin@odyssey.rc.fas.harvard.educ

login to hp03: ssh 140.247.107.75

hp03: python /mnt/coxfs01/EM-network/script/train_superhuman.py -di /n/coxfs01/vcg_connectomics/snemi//img/train-input_df_150.h5 -dl /n/coxfs01/vcg_connectomics/snemi//label/train-labels.h5 -do /n/coxfs01/donglai/snemi/result/matin/ --volume-total 100000 --volume-save 20000 --volume-valid 1000 -lr 0.01 -it 1 -bn 1 -rl 2 -dr 1.5 -g 1 -j 1 -bs 5


conda env create -f /mnt/coxfs01/EM-network/environment.yml

To remove an environment: conda env remove --name em-net_py3 ibexHelper

conda create -n ibexHelper python=2.7


on RC: /n/pfister_lab2/Lab/zhjin


python /n/coxfs01/zhjin/EM-network/script/train_superhuman.py -di /n/coxfs01/vcg_connectomics/snemi//img/train-input_df_150.h5 -dl /n/coxfs01/vcg_connectomics/snemi//label/train-labels.h5 -do /n/coxfs01/donglai/snemi/result/matin/ --volume-total 100000 --volume-save 20000 --volume-valid 1000 -lr 0.01 -it 1 -bn 1 -rl 2 -dr 1.5 -g 1 -j 1 -bs 5


python /n/coxfs01/zhjin/EM-network/script/train_superhuman.py -di /n/coxfs01/vcg_connectomics/snemi//img/train-input_df_150.h5 -dl /n/coxfs01/vcg_connectomics/snemi//label/train-labels.h5 -do /n/coxfs01/zhjin/snemi/result/matin/ --volume-total 100000 --volume-save 20000 --volume-valid 1000 -lr 0.01 -it 1 -bn 1 -rl 2 -dr 1.5 -g 2 -j 1 -bs 10


Note: Must specify the data-shape

python /n/coxfs01/zhjin/EM-network/script/train_superhuman.py -di /n/coxfs01/vcg_connectomics/Dyer17/data/train/V1/V1_img.h5 -dl /n/coxfs01/vcg_connectomics/Dyer17/data/train/V1/V1_anno_dense_bv_cell.h5 -do /n/coxfs01/zhjin/kasthuri/result --volume-total 100000 --volume-save 20000 --volume-valid 1000 --data-shape "18,64,64" -lr 0.01 -it 1 -bn 1 -rl 2 -dr 1.5 -g 2 -j 1 -bs 10

python /n/coxfs01/zhjin/EM-network/script/test_superhuman.py -dt /n/coxfs01/vcg_connectomics/Dyer17/data/train/V3/V3_img.h5 -do /n/coxfs01/zhjin/kasthuri/test --model /n/coxfs01/zhjin/kasthuri/result/log_2019-02-13_15-05-23/volume_80000.pth --data-shape "18,64,64" --batch-size 10 -g 2 -dr 1.5 -bn 1 -rl 2 -it 1


Train V1 and V2 together:
python /n/coxfs01/zhjin/EM-network/script/train_superhuman.py -di V1_img_255.h5@/n/coxfs01/vcg_connectomics/Dyer17/data/train/V2/V2_img.h5 -dl /n/coxfs01/vcg_connectomics/Dyer17/data/train/V1/V1_anno_dense_bv_cell.h5@/n/coxfs01/vcg_connectomics/Dyer17/data/train/V2/V2_anno_dense_bv_cell.h5 -do /n/coxfs01/zhjin/kasthuri/result2 --volume-total 130000 --volume-save 20000 --volume-valid 1000 --data-shape "64,64,64" -lr 0.01 -it 1 -bn 1 -rl 2 -dr 1.5 -g 2 -j 1 -bs 10



python /n/coxfs01/zhjin/EM-network/script/test_superhuman.py -dt /n/coxfs01/vcg_connectomics/Dyer17/data/train/V2/V2_img.h5 -do /n/coxfs01/zhjin/kasthuri/test3 --model /n/coxfs01/zhjin/kasthuri/result2/log_2019-02-22_12-52-50/volume_80000.pth --data-shape "64,64,64" --batch-size 10 -g 2 -dr 1.5 -bn 1 -rl 2 -it 1


Train V1 and V2 together on train_superhuman_kasthuri:
python /n/coxfs01/zhjin/EM-network/script/train_superhuman_kasthuri.py -di V1_img_255.h5@/n/coxfs01/vcg_connectomics/Dyer17/data/train/V2/V2_img.h5 -dl V1_anno_dense_bv_cell_corrected.h5@V2_anno_dense_bv_cell_corrected.h5 -do /n/coxfs01/zhjin/kasthuri/result --volume-total 11000 --volume-save 2000 --volume-valid 1000 --data-shape "64,64,64" -lr 0.005 -it 1 -bn 1 -rl 2 -dr 1.5 -g 2 -j 1 -bs 10


python /n/coxfs01/zhjin/EM-network/script/test_superhuman.py -dt /n/coxfs01/vcg_connectomics/Dyer17/data/train/V2/V2_img.h5 -do /n/coxfs01/zhjin/kasthuri/test_V2 --model /n/coxfs01/zhjin/kasthuri/result/model_alpha5/volume_11000.pth --data-shape "64,64,64" --batch-size 10 -g 2 -dr 1.5 -bn 1 -rl 2 -it 1


Train V1 only on train_superhuman_kasthuri:
python /n/coxfs01/zhjin/EM-network/script/train_superhuman_kasthuri.py -di V1_img_255.h5 -dl V1_anno_dense_bv_cell_corrected.h5 -do /n/coxfs01/zhjin/kasthuri/result_V1 --volume-total 11000 --volume-save 2000 --volume-valid 1000 --data-shape "64,64,64" -lr 0.01 -it 1 -bn 1 -rl 2 -dr 1.5 -g 2 -j 1 -bs 10

Train V2 only on train_superhuman_kasthuri:
python /n/coxfs01/zhjin/EM-network/script/train_superhuman_kasthuri.py -di /n/coxfs01/vcg_connectomics/Dyer17/data/train/V2/V2_img.h5 -dl V2_anno_dense_bv_cell_corrected.h5 -do /n/coxfs01/zhjin/kasthuri/result_V2 --volume-total 11000 --volume-save 2000 --volume-valid 1000 --data-shape "64,64,64" -lr 0.01 -it 1 -bn 1 -rl 2 -dr 1.5 -g 2 -j 1 -bs 10



Train on NagCT

python /n/coxfs01/zhjin/EM-network/script/train_superhuman_nagCT.py -di nagCT_V0_Train.h5@nagCT_V1_Train.h5 -dl nagCT_V0_Label_corrected.h5@nagCT_V1_Label_corrected.h5 -do /n/coxfs01/zhjin/nagCT/result --volume-total 11000 --volume-save 2000 --volume-valid 1000 --data-shape "24,64,64" -lr 0.005 -it 1 -bn 1 -rl 2 -dr 1.5 -g 2 -j 1 -bs 32


python /n/coxfs01/zhjin/EM-network/script/test_superhuman_nagCT.py -dt nagCT_V1_Train.h5 -do /n/coxfs01/zhjin/nagCT/test --model /n/coxfs01/zhjin/nagCT/result/nagCT_alpha100/volume_11008.pth --data-shape "24,64,64" --batch-size 32 -g 2 -dr 1.5 -bn 1 -rl 2 -it 1


Train on Zeiss_Nag

python /n/coxfs01/zhjin/EM-network/script/train_superhuman_kasthuri.py -di /n/coxfs01/vcg_connectomics/Zeiss_Nag/vol1/vol1_im-yz.h5 -dl /n/coxfs01/vcg_connectomics/Zeiss_Nag/vol1/vol1_seg_v1.h5 -do /n/coxfs01/zhjin/Zeiss_Nag/result --volume-total 11000 --volume-save 2000 --volume-valid 1000 --data-shape "64,64,64" -lr 0.005 -it 1 -bn 1 -rl 2 -dr 1.5 -g 4 -j 1 -bs 32


python /n/coxfs01/zhjin/EM-network/script/test_superhuman.py -dt /n/coxfs01/vcg_connectomics/Zeiss_Nag/vol1/vol1_im-yz.h5 -do /n/coxfs01/zhjin/Zeiss_Nag/test --model /n/coxfs01/zhjin/Zeiss_Nag/result/ZeissNag_softmax_background_alpha50/volume_11008.pth --data-shape "64,64,64" --batch-size 32 -g 4 -dr 1.5 -bn 1 -rl 2 -it 1


python /n/coxfs01/zhjin/EM-network/script/train_superhuman_nagCT.py -di /n/coxfs01/vcg_connectomics/Zeiss_Nag/vol1/vol1_im-yz.h5 -dl /n/coxfs01/vcg_connectomics/Zeiss_Nag/vol1/vol1_seg_v1.h5 -do /n/coxfs01/zhjin/Zeiss_Nag/result --volume-total 11000 --volume-save 2000 --volume-valid 1000 --data-shape "32,128,128" -lr 0.005 -it 1 -bn 1 -rl 2 -dr 1.5 -g 8 -j 1 -bs 16

python /n/coxfs01/zhjin/EM-network/script/test_superhuman_nagCT.py -dt /n/coxfs01/vcg_connectomics/Zeiss_Nag/vol1/vol1_im-yz.h5 -do /n/coxfs01/zhjin/Zeiss_Nag/test --model /n/coxfs01/zhjin/Zeiss_Nag/result/alpha1_128/volume_8000.pth --data-shape "32,128,128" --batch-size 16 -g 8 -dr 1.5 -bn 1 -rl 2 -it 1


Train all together

python /n/coxfs01/zhjin/EM-network/script/train_superhuman_nagCT.py -di /n/coxfs01/vcg_connectomics/Zeiss_Nag/vol1/vol1_im-yz.h5@V1_img_255.h5@/n/coxfs01/vcg_connectomics/Dyer17/data/train/V2/V2_img.h5 -dl /n/coxfs01/vcg_connectomics/Zeiss_Nag/vol1/vol1_seg_v1.h5@V1_anno_dense_bv_cell_corrected.h5@V2_anno_dense_bv_cell_corrected.h5 -do /n/coxfs01/zhjin/all/result --volume-total 20000 --volume-save 2000 --volume-valid 1000 --data-shape "32,128,128" -lr 0.005 -it 1 -bn 1 -rl 2 -dr 1.5 -g 8 -j 1 -bs 10

python /n/coxfs01/zhjin/EM-network/script/test_superhuman_nagCT.py -dt /n/coxfs01/vcg_connectomics/Zeiss_Nag/vol1/vol1_im-yz.h5 -do /n/coxfs01/zhjin/all/test --model /n/coxfs01/zhjin/all/result/alpha1_128/volume_20000.pth --data-shape "32,128,128" --batch-size 10 -g 8 -dr 1.5 -bn 1 -rl 2 -it 1




Autoencoder Script

python /n/coxfs01/zhjin/EM-network/script/train_unet_autoencoder.py -di V1_img_255.h5@/n/coxfs01/vcg_connectomics/Dyer17/data/train/V2/V2_img.h5 -dl V1_img_255.h5@/n/coxfs01/vcg_connectomics/Dyer17/data/train/V2/V2_img.h5 -do /n/coxfs01/zhjin/autoencoder/result --volume-total 12000 --volume-save 2000 --volume-valid 1000 --data-shape "64,64,64" -lr 0.01 -it 1 -bn 1 -rl 2 -dr 1.5 -g 2 -j 1 -bs 10

python /n/coxfs01/zhjin/EM-network/script/train_deep_autoencoder.py -di V1_img_255.h5@/n/coxfs01/vcg_connectomics/Dyer17/data/train/V2/V2_img.h5 -dl V1_img_255.h5@/n/coxfs01/vcg_connectomics/Dyer17/data/train/V2/V2_img.h5 -do /n/coxfs01/zhjin/autoencoder/result2 --volume-total 12000 --volume-save 2000 --volume-valid 1000 --data-shape "128,128,128" -lr 0.01 -it 1 -bn 1 -rl 2 -dr 1.5 -g 2 -j 1 -bs 10
 

srun --pty -p cox -t 7-00:00 --mem 64g -n 16 --gres=gpu:2 /bin/bash

srun --pty -p cox -t 7-00:00 --mem 40000 -n 4 --gres=gpu:4 /bin/bash


Tensorboard:

ssh -L 10012:localhost:10012 zhjin@140.247.107.75
source /home/donglai/miniconda2/bin/activate tensorB
tensorboard --logdir=Zeiss_Nag/result --port=10012


python /n/coxfs01/zhjin/EM-network/setup.py install

/n/coxfs01/zhjin/kasthuri/result/log_2019-02-13_15-05-23

conda env remove -n ENV_NAME


scp zhjin@140.247.107.75:/home/zhjin/SK-LARGE-MIX-GAUSSIAN-label.h5

