# Phase-2: Object Detection Using Detection Transformers 

Training [DeTr](https://github.com/facebookresearch/detr) on a custom dataset to predict Materials used in Construction Materials.


### Step-1) How was the labeling handled?
We were having a labeled dataset for panoptic segmentation but only things like grader, cement, hydra crane were labeled in the images not stuff classes. So, we used pretrained DeTr on coco dataset for getting annotations for all the classes. 
In this procedure the aim was to get the predicted mask and the bounding box by the pretrained weights for getting the stuff classes. 

NOTE: Everything with is not a a construction material like cars, truck, person were treated like stuff for our problem statement.


### Step-2) Predefined coco classes

We used 1000 images from COCO validation 2017 dataset. All the things classes like person, car truck were labelled miscellaneous stuff. The coco dataset was mixed our dataset didnt have all the stuff classes and we didnt want to miss out on any.

### Step-3) What about the two separated outputs of DeTr Prediction & Ground Truth?
Finally we had two things: the first is ground truth which was labeled by us and prediction of DeTr weights. we mapped both the outputs together for getting labels for all the classes 
Another issue of overlapping coordinates for the classes that were labeled and the same classes predicted by the DeTr, in that case the overlap was removed in predictions and original mask was considered.

### Step-4) Final step
Finally we had the labeled data so, splitted it in train and test set for training in ration of 80:20, Cloned the Github Repository of DeTr and trained the model on custom classes

The Things classes: *'aac_blocks', ' adhesives', ' ahus', ' aluminium_frames_for_false_ceiling', ' chiller', ' concrete_mixer_machine', ' concrete_pump_(50%)', ' control_panel', ' cu_piping', ' distribution_transformer', ' dump_truck___tipper_truck', ' emulsion_paint', ' enamel_paint', ' fine_aggregate', ' fire_buckets', ' fire_extinguishers', ' glass_wool', ' grader', ' hoist', ' hollow_concrete_blocks', ' hot_mix_plant', ' hydra_crane', ' interlocked_switched_socket', ' junction_box', ' lime', ' marble', ' metal_primer', ' pipe_fittings', ' rcc_hume_pipes', ' refrigerant_gas', ' river_sand', ' rmc_batching_plant', ' rmu_units', ' sanitary_fixtures', ' skid_steer_loader_(bobcat)', ' smoke_detectors', ' split_units', ' structural_steel_-_channel', ' switch_boards_and_switches', ' texture_paint', ' threaded_rod', ' transit_mixer', ' vcb_panel', ' vitrified_tiles', ' vrf_units', ' water_tank', ' wheel_loader', ' wood_primer'*

The Stuff Classes: *'banner', 'blanket', 'bridge', 'cardboard', 'counter', 'curtain', 'door-stuff', 'floor-wood', 'flower', 'fruit', 'gravel', 'house', 'light', 'mirror-stuff', 'net', 'pillow', 'platform', 'playingfield', 'railroad', 'river', 'road', 'roof', 'sand', 'sea', 'shelf', 'snow', 'stairs', 'tent', 'towel', 'wall-brick', 'wall-stone', 'wall-tile', 'wall-wood', 'water-other', 'window-blind', 'window-other', 'tree-merged', 'fence-merged', 'ceiling-merged', 'sky-other-merged', 'cabinet-merged', 'table-merged', 'floor-other-merged', 'pavement-merged', 'mountain-merged', 'grass-merged', 'dirt-merged', 'paper-merged', 'food-other-merged', 'building-other-merged', 'rock-merged', 'wall-other-merged', 'rug-merged', 'miscellaneous stuff*

Data Prepration
---------------------------

1. Update annotations of labelled classes using `python autolabellingstuff.py`
2. Change the classes of COCO Valid Set using `python3 transform_coco.py`
3. Create Train Test Split `python train_test_split.py` this script will create train.json and test.json

Steps For training
--------------

```
git clone https://github.com/facebookresearch/detr.git
pip install git+https://github.com/cocodataset/panopticapi.git
pip install -U 'git+https://github.com/cocodataset/cocoapi.git#subdirectory=PythonAPI'
```

Download Pre-Trained Weights
```
cd detr/
mkdir weights
cd weights
wget https://dl.fbaipublicfiles.com/detr/detr-r50-e632da11.pth
cd ..
```
**Start Training :**

For the initial 80 epochs model was trained on lower input size of scales `scales = [480, 512, 544, 576, 608, 640]` to speed up the training and later it was traiined on higher resolution `scales = [480, 512, 544, 576, 608, 640, 672, 704, 736, 768, 800]` Training total of ... epochs
```
python main.py --dataset_file coco --coco_path data/ --output_dir output --resume weights/detr-r50-e632da11.pth --epochs 100 --batch_size 2

When resuming Training 
python main2.py --dataset_file coco --coco_path data/ --output_dir output --resume output/checkpoint.pth --epochs 100 --batch_size 2
```

Logs
------------


Output
--------






