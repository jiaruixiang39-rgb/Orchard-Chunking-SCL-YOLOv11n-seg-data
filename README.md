# Orchard chunking and SCL-YOLOv11n-seg: sample chunks and experimental data

This repository accompanies the study on joint instance segmentation of trunks,
branches, and young apples in high-density dwarf apple orchards using overlapping
image chunking and SCL-YOLOv11n-seg.

## Contents

- `images/chunks/`: ten public 640 x 640 pixel chunk samples, comprising five
  training chunks and five validation chunks. These are copied unchanged from
  the chunked dataset, with one chunk from each of ten source photographs.
- `metadata/image_pairs.csv`: chunk-to-source identifiers, source image sizes,
  crop coordinates, row/column indices, split, and window type. The
  `source_original_filename` field identifies a source photograph; it does not
  refer to an original image file provided in this public release.
- `data/table2_chunking_summary.csv` and `table2_chunking_per_run.csv`: the
  whole-image and chunked-image comparisons across random seeds 0, 1, and 2.
- `data/table3_baseline_vs_SCL_summary.csv`: the baseline and improved-model
  comparison, reported as means and sample standard deviations over three runs.
- `data/table4_ablation_summary.csv`, `table4_ablation_mask_per_run.csv`, and
  `table4_complexity.csv`: the four progressive module configurations.
- `data/table5_model_comparison_seed0.csv`: five-model comparison for seed 0.
- `data/training_histories/`: fifteen numeric training logs for the Table 2 and
  Table 4 experiments. The manifest records each run and its selected checkpoint
  epoch.
- `metadata/`: manifests and file checksums. The image manifest lists only the
  ten publicly included chunks.

## Image samples and annotation access

The complete source collection comprises 200 RGB photographs split at the
original image level into 160 training and 40 validation images. Original
photographs are not included in this public release. The ten public chunks are
illustrative samples, not the complete benchmark dataset. They come from five
training source images and five validation source images selected at evenly
spaced positions in each filename-sorted source list. Exactly one chunk from each
source image is included. All ten chunk files retain their source filenames and
original file contents. Six regular chunks, two right-edge chunks, one bottom-edge
chunk, and one bottom-right corner chunk illustrate the chunking layout.

Crop coordinates are recorded in pixels in the source image: x increases
rightward and y increases downward. The top-left image pixel is (0, 0).
The x0/y0 bounds are inclusive and the x1/y1 bounds are exclusive. Row and column
indices are zero-based. These are crop positions, not object annotations.
For a 3072 x 4096 source image, the final boundary-aligned starts are
x = 2432 and y = 3456; these retain the prescribed 640 x 640 chunk size.

Images were collected at the Xinyuan apple demonstration orchard in Weihai,
Shandong, China. The study concerns trunks, branches, and young apples.
The image chunking settings were 640 x 640 pixels with 20% regular overlap,
corresponding to a 512-pixel regular stride, with boundary completion.

Manually annotated instance segmentation labels are not included in this public
repository. They can be obtained from the corresponding author upon reasonable
request. No Labelme JSON, YOLO label TXT, ground-truth masks, model weights,
or annotation-containing archive is included.

## Reading the numerical data

- Accuracy metrics are expressed in percent. Differences in Table 2 are
  percentage points. `SD_ddof=1` denotes sample standard deviation.
- Tables 2-4 use three runs with seeds 0, 1, and 2. Table 5 uses seed 0 only.
  Consequently, SCL-YOLOv11n-seg has mean Mask mAP50/mAP50-95 values of
  74.10%/38.32% in Tables 3-4, and seed-0 values of 74.07%/38.20% in Table 5.
- Table 2 evaluates original-image input and chunked-image input under their
  respective evaluation pipelines. Its evaluation units differ between inputs.
- Table 4 configurations are `baseline` (YOLOv11n-seg), `A` (SPDConv), `AB`
  (SPDConv and CAFMFusion), and `ABC` (SPDConv, CAFMFusion, and LSDECD).
- P, R, and AP run-level values were retained from stored training metrics.
  Dice and IoU for Tables 3-4 are the reported, rounded terminal summaries;
  individual-seed Dice and IoU values have not been inferred from those summaries.
- Training-history metrics use the original fractional units from the logs;
  summary accuracy CSVs use percent. `selected_epoch` is the recorded training
  epoch associated with the selected checkpoint. Full training curves can also
  contain values from later epochs, which are not the selected checkpoint result.
- In Table 5, AP for Mask R-CNN and YOLACT comes from their native COCO evaluation,
  whereas AP for YOLO models comes from their native Ultralytics training records.
  The supplementary Dice/IoU values use the same selected checkpoints.
- Computational cost retains the reported measurement bases. Classic-model
  GFLOPs use supplementary full-pipeline evaluation; YOLO GFLOPs use the reported
  THOP calculation. These are identified explicitly in the CSV. Input size is
  640 x 640 pixels. Rounded values in the manuscript may have fewer decimal places.

## Contact

Corresponding author: Zhenchao Wu  
Email: wuzhenchao@ahau.edu.cn
