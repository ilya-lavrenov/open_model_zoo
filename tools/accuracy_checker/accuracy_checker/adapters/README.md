# Adapters

Adapter is a function for conversion network infer output to metric specific format.
You can use 2 ways to set adapter for topology:
* Define adapter as a string.

```yml
adapter: classification
```

* Define adapter as a dictionary, using `type:` for setting adapter name. This approach gives opportunity to set additional parameters for adapter if it is required.

```yml
adapter:
  type: reid
  grn_workaround: False
```

AccuracyChecker supports following set of adapters:
* `classification` - converting output of classification model to `ClassificationPrediction` representation.
* `segmentation` - converting output of semantic segmentation model to `SeegmentationPrediction` representation.
  * `make_argmax` - allows to apply argmax operation to output values.
* `segmentation_one_class` - converting output of semantic segmentation to `SeegmentationPrediction` representation. It is suitable for situation when model's output is probability of belong each pixel to foreground class.
  * `threshold` - minimum probability threshold for valid class belonging.
* `tiny_yolo_v1` - converting output of Tiny YOLO v1 model to `DetectionPrediction` representation.
* `reid` - converting output of reidentification model to `ReIdentificationPrediction` representation.
  * `grn_workaround` - enabling processing output with adding Global Region Normalization layer.
* `yolo_v2` - converting output of YOLO v2 family models to `DetectionPrediction` representation.
  * `classes` - number of detection classes (default 20).
  * `anchors` - anchor values provided as comma-separated list or one of precomputed:
    - `yolo_v2` - `[1.3221, 1.73145, 3.19275, 4.00944, 5.05587, 8.09892, 9.47112, 4.84053, 11.2364, 10.0071]`,
    - `tiny_yolo_v2` - `[1.08, 1.19, 3.42, 4.41, 6.63, 11.38, 9.42, 5.11, 16.62, 10.52]`
  * `coords` - number of bbox coordinates (default 4).
  * `num` - num parameter from DarkNet configuration file (default 5).
* `yolo_v3` - converting output of YOLO v3 family models to `DetectionPrediction` representation.
  * `classes` - number of detection classes (default 80).
  * `anchors` - anchor values provided as comma-separited list or precomputed:
    - `yolo_v3` - `[10.0, 13.0, 16.0, 30.0, 33.0, 23.0, 30.0, 61.0, 62.0, 45.0, 59.0, 119.0, 116.0, 90.0, 156.0, 198.0, 373.0, 326.0]`
    - `tiny_yolo_v3` - `[10.0, 14.0, 23.0, 27.0, 37.0, 58.0, 81.0, 82.0, 135.0, 169.0, 344.0, 319.0]`
  * `coords` - number of bbox coordinates (default 4).
  * `num` - num parameter from DarkNet configuration file (default 3).
  * `anchor_mask` - mask for used anchors for each output layer (Optional, if not provided default way for selecting anchors will be used.)
  * `threshold` - minimal objectness score value for valid detections (default 0.001).
  * `input_width` and `input_height` - network input width and height correspondingly (default 416).
  * `outputs` - the list of output layers names (optional), if specified there should be exactly 3 output layers provided.
* `lpr` - converting output of license plate recognition model to `CharacterRecognitionPrediction` representation.
* `ssd` - converting  output of SSD model to `DetectionPrediction` representation.
* `ssd_mxnet` - converting output of SSD-based models from MXNet framework to `DetectionPrediction` representation.
* `pytorch_ssd_decoder` - converts output of SSD model from PyTorch without embedded decoder.
  * `scores_out` - name of output layer with bounding boxes scores.
  * `boxes_out` - name of output layer with bounding boxes coordinates.
  * `confidence_threshold` - lower bound for valid boxes scores (optional, default 0.05).
  * `nms_threshold` - overlap threshold for NMS (optional, default 0.5).
  * `keep_top_k ` - maximal number of boxes which should be kept (optional, default 200).
* `ssd_onnx` - converting output of SSD-based model from PyTorch with NonMaxSuppression layer.
  * `labels_out` - name of output layer with labels or regular expression for it searching.
  * `scores_out`- name of output layer with scores or regular expression for it searching.
  * `bboxes_out` - name of output layer with bboxes or regular expression for it searching.
* `tf_object_detection` - converting output of detection models from TensorFlow object detection API to `DetectionPrediction`.
  * `classes_out` - name of output layer with predicted classes.
  * `boxes_out` - name of output layer with predicted boxes coordinates in format [y0, x0, y1, x1].
  *  `scores_out` - name of output layer with detection scores.
  * `num_detections_out` - name of output layer which contains the number of valid detections.
* `retinanet` - converting output of RetinaNet-based model.
  * `loc_out` - name of output layer with bounding box deltas.
  * `class_out` - name of output layer with classification probabilities.
* `face_person_detection` - converting face person detection model output with 2 detection outputs to `ContainerPredition`, where value of parameters `face_out`and `person_out` are used for identification `DetectionPrediction` in container.
  * `face_out` -  face detection output layer name.
  * `person_out` - person detection output layer name.
* `person_attributes` - converting person attributes recognition model output to `MultiLabelRecognitionPrediction`.
  * `attributes_recognition_out` - output layer name with attributes scores. (optional, used if your model has more than one outputs).
* `vehicle_attributes`  - converting vehicle attributes recognition model output to `ContainerPrediction` where value of parameters `color_out`and `type_out` are used for identification `ClassificationPrediction` in container.
  * `color_out` - vehicle color attribute output layer name.
  * `type_out`- vehicle type attribute output layer name.
* `head_pose` - converting head pose estimation model output to `ContainerPrediction` where names of parameters `angle_pitch`, `angle_yaw` and `angle_roll` are used for identification `RegressionPrediction` in container.
  * `angle_pitch` - output layer name for pitch angle.
  * `angle_yaw`- output layer name for yaw angle.
  * `angle_roll` - output layer name for roll angle.
* `age_gender` - converting age gender recognition model output to `ContainerPrediction` with `ClassificationPrediction` named `gender` for gender recognition, `ClassificationPrediction` named `age_classification` and `RegressionPrediction` named `age_error` for age recognition.
  * `age_out` - output layer name for age recognition.
  * `gender_out` - output layer name for gender recognition.
* `action_detection` - converting output of model for person detection and action recognition tasks to `ContainerPrediction` with `DetectionPrdiction` for class agnostic metric calculation and `ActionDetectionPrediction` for action recognition. The representations in container have names `class_agnostic_prediction` and `action_prediction` respectively.
  * `priorbox_out` - name of layer containing prior boxes in SSD format.
  * `loc_out` - name of layer containing box coordinates in SSD format.
  * `main_conf_out` - name of layer containing detection confidences.
  * `add_conf_out_prefix` - prefix for generation name of layers containing action confidences if topology has several following layers or layer name.
  * `add_conf_out_count` - number of layers with action confidences (optional, you can not provide this argument if action confidences contained in one layer).
  * `num_action_classes` - number classes for action recognition.
  * `detection_threshold` - minimal detection confidences level for valid detections.
  * `actions_scores_threshold` - minimal actions confidences level for valid detections.
  * `action_scale` - scale for correct action score calculation.
* `super_resolution` - converting output of single image super resolution network to `SuperResolutionPrediction`.
  * `reverse_channels` - allow switching output image channels e.g. RGB to BGR (Optional. Default value is False).
* `landmarks_regression` - converting output of model for landmarks regression to `FacialLandmarksPrediction`.
* `pixel_link_text_detection` - converting output of PixelLink like model for text detection to `TextDetectionPrediction`.
  * `pixel_class_out` - name of layer containing information related to text/no-text classification for each pixel.
  * `pixel_link_out` - name of layer containing information related to linkage between pixels and their neighbors.
  * `pixel_class_confidence_threshold` - confidence threshold for valid segmentation mask (Optional, default 0.8).
  * `pixel_link_confidence_threshold` - confidence threshold for valid pixel links (Optional, default 0.8).
  * `min_area` - minimal area for valid text prediction (Optional, default 0).
  * `min_height` - minimal height for valid text prediction (Optional, default 0).
* `ctpn_text_detection` - converting output of CTPN like model for text detection to `TextDetectionPrediction`.
  * `cls_prob_out` - name of output layer with class probabilities.
  * `bbox_pred_out` - name of output layer with predicted boxes.
  * `min_size` - minimal valid detected text proposals size (Optional, default 8).
  * `min_ratio` - minimal width / height ratio for valid text line (Optional, default 0.5).
  * `line_min_score` - minimal confidence for text line (Optional, default 0.9).
  * `text_proposals_width` - minimal width for text proposal (Optional, default 16).
  * `min_num_proposals` - minimal number for text proposals (Optional, default 2).
  * `pre_nms_top_n` - saved top n proposals before NMS applying (Optional, default 12000).
  * `post_nms_top_n` - saved top n proposals after NMS applying (Optional, default 1000).
  * `nms_threshold` - overlap threshold for NMS (Optional, default 0.7).
* `east_text_detection` - converting output of EAST like model for text detection to `TextDetectionPrediction`.
  * `score_map_out` - the name of output layer which contains score map.
  * `geometry_map_out` - the name of output layer which contains geometry map.
  * `score_map_threshold` - threshold for score map (Optional, default 0.8).
  * `nms_threshold` - threshold for text boxes NMS (Optional, default 0.2).
  * `box_threshold` - minimal confidence threshold for text boxes (Optional, default 0.1).
* `human_pose_estimation` - converting output of model for human pose estimation to `PoseEstimationPrediction`.
  * `part_affinity_fields_out` - name of output layer with keypoints pairwise relations (part affinity fields).
  * `keypoints_heatmap_out` - name of output layer with keypoints heatmaps.
  The output layers can be omitted if model has only one output layer - concatenation of this 2.
* `beam_search_decoder` - realization CTC Beam Search decoder for symbol sequence recognition, converting model output to `CharacterRecognitionPrediction`.
  * `beam_size` -  size of the beam to use during decoding (default 10).
  * `blank_label` - index of the CTC blank label.
  * `softmaxed_probabilities` - indicator that model uses softmax for output layer (default False).
* `gaze_estimation` - converting output of gaze estimation model to `GazeVectorPrediction`.
* `hit_ratio_adapter` - converting output NCF model to `HitRatioPrediction`.
* `brain_tumor_segmentation` - converting output of brain tumor segmentation model to `BrainTumorSegmentationPrediction`.
  * `make_argmax`  - allows to apply argmax operation to output values. (default - `False`)
  * `label_order` - sets mapping from output classes to dataset classes. For example: `label_order: [3,1,2]` means that class with id 3 from model's output matches with class with id 1 from dataset,  class with id 1 from model's output matches with class with id 2 from dataset, class with id 2 from model's output matches with class with id 3 from dataset.
* `nmt` - converting output of neural machine translation model to `MachineTranslationPrediction`.
  * `vocabulary_file` - file which contains vocabulary for encoding model predicted indexes to words (e. g. vocab.bpe.32000.de). Path can be prefixed with `--models` arguments.
  * `eos_index` - index end of string symbol in vocabulary (Optional, used in cases when launcher does not support dynamic output shape for cut off empty prediction).
* `bert_question_answering` - converting output of BERT model trained to solve question answering task to `QuestionAnsweringPrediction`.
* `bert_classification` - converting output of BERT model trained for classification task to `ClassificationPrediction`.
  * `num_classes` - number of predicted classes.
  * `classification_out` - name of output layer with classification probabilities. (Optional, if not provided default first output blob will be used).
* `human_pose_estimation_3d` - converting output of model for 3D human pose estimation to `PoseEstimation3dPrediction`.
  * `features_3d_out` - name of output layer with 3D coordinates maps.
  * `keypoints_heatmap_out` - name of output layer with keypoints heatmaps.
  * `part_affinity_fields_out` - name of output layer with keypoints pairwise relations (part affinity fields).
* `ctdet` - converting output of CenterNet object detection model to `DetectionPrediction`.
  * `center_heatmap_out` - name of output layer with center points heatmaps.
  * `width_height_out` - name of the output layer with object sizes.
  * `regression_out` - name of the regression output with the offset prediction.
* `mask_rcnn` - converting raw outputs of Mask-RCNN to combination of `DetectionPrediction` and `CoCocInstanceSegmentationPrediction`.
  * `classes_out` - name of output layer with information about classes (optional, if your model has detection_output layer as output).
  * `scores_out` - name of output layer with bbox scores (optional, if your model has detection_output layer as output).
  * `boxes_out` - name of output layer with bboxes (optional, if your model has detection_output layer as output).
  * `raw_masks_out` - name of output layer with raw instances masks.
  * `num_detections_out` - name of output layer with number valid detections (used in MaskRCNN models trained with TF Object Detection API).
  * `detection_out` - SSD-like detection output layer name (optional, if your model has scores_out, boxes_out and classes_out).
* `mask_rcnn_with_text` - converting raw outputs of Mask-RCNN with additional Text Recognition head to `TextDetectionPrediction`.
  * `classes_out` - name of output layer with information about classes.
  * `scores_out` - name of output layer with bbox scores.
  * `boxes_out` - name of output layer with bboxes.
  * `raw_masks_out` - name of output layer with raw instances masks.
  * `texts_out` - name of output layer with texts.
  * `confidence_threshold` - confidence threshold that is used to filter out detected instances.
* `fcos_person` - converting output of FCOS (single class) model to `DetectionPrediction` representation.
  * `output_blob` - name of output layer with bboxes.
  * `scale` - scalar value to normalize bbox coordinates.
