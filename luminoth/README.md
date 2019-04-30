
# Changes made on General Model Creation:

- verified with WBCProject data

## examples/sample_config.yml

1. only one class - objectness
    edit:   examples/sample_config.yml
            num_classes: 20         ->  num_classes: 1

2. dataset default directory - tf_dataset
    edit:   examples/sample_config.yml
            dir: datasets/voc/tf    ->  dir: tf_dataset


## luminoth/models/fasterrcnn/base_config.yml

1. Changed variables
    - save_checkpoint_secs: 600 -> 100
    - dir: datasets/voc/tf -> tf_dataset
    - image_preprocessing: removed everything here
    - model: network: num_classes: 20 -> 1
    - model: rpn: proposals: pre_nms_top_n: 12000 -> 24000
    - model: rpn: proposals: post_nms_top_n: 2000 -> 4000
    - model: rcnn: proposals: class_max_detections: 100 -> 800
    - model: rcnn: proposals: class_nms_threshold: 0.5 -> 0.3
    - model: rcnn: proposals: total_max_detections: 300 -> 800



## luminoth/predict.py

1. split the -d destination folder into 2 sub-folders for JSON & PNG files
2. remove output path creation line
3. introduced some new flags:
    --do-labelling
    --only-one-class




## luminoth/vis.py

1. add flag for unique color or multi-color:
    seen_labels[label] = (0, 255, 0)
2. disabling vis labels facility








## Frequently Changeable Variables

- jobs
- save_checkpoint_secs: 600 -> 100
- num_epochs
- dataset:
    - dir: datasets/voc/tf -> tf_dataset
    - image_preprocessing: removed everything here
- model:
    - type: fasterrcnn
    - network:
        - num_classes: 20 -> 1
        - with_rcnn: True
    - base_network:
        - architecture: resnet_v1_101
        - endpoint:
        - fine_tune_from: block2
        - anchors:
            - base_size: 256
    - rpn
        - proposals:
            - pre_nms_top_n: 12000 -> 24000
            - post_nms_top_n: 2000 -> 4000
    - rcnn
        - proposals:
            - class_max_detections: 100 -> 800
            - class_nms_threshold: 0.5 -> 0.3
            - total_max_detections: 300 -> 800
            - min_prob_threshold: 0.5
        - target:
            - foreground_fraction: 0.25
            - minibatch_size: 256
            - foreground_threshold: 0.5
            - background_threshold_high: 0.5
            - background_threshold_low: 0.0




Note:

1. Always have image data in .jpg format. If you have .png format, then change
    .png -> .jpg in the file /luminoth/tools/dataset/readers/object_detection/pascalvoc.py
