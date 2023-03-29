# Scheme 2

This scheme is generally preferred over scheme 1 because it can handle larger datasets that cannot be fully loaded into memory, and also has other advantages in terms of accurately detecting and labeling spikes.

In phase 1, the first scheme is used as a training step, performing unsupervised clustering on a subset of the dataset. Then in phase 2, a set of classifiers are trained based on the labels of the training step. The classifiers are then used to label the remaining data.

## Usage

```python
import spikeinterface as si
import spikeinterface.preprocessing as spre
import mountainsort5 as ms5

recording = ... # load your recording using SpikeInterface

# Make sure the recording is preprocessed appropriately
# lazy preprocessing
recording_filtered = spre.bandpass_filter(recording, freq_min=300, freq_max=6000)
recording_preprocessed: si.BaseRecording = spre.whiten(recording_filtered)

# use scheme 2
sorting = ms5.sorting_scheme2(
    recording=recording_preprocessed,
    sorting_parameters=ms5.Scheme2SortingParameters(
        detect_sign=-1,
        phase1_detect_channel_radius=200,
        detect_channel_radius=50,
        # other parameters...
    )
)

# Now you have a sorting object that you can save to disk or use for further analysis
```

## Parameters

**phase1_detect_channel_radius**

The `detect_channel_radius` parameter for phase 1 (see [scheme 1](./scheme1.md)).

**detect_channel_radius**

This is just like the `phase1_detect_channel_radius` parameter, but it is used for phase 2.

**phase1_detect_threshold**

The `detect_threshold` parameter for phase 1 (see [scheme 1](./scheme1.md)).

**detect_threshold**

This is just like the `phase1_detect_threshold` parameter, but it is used for phase 2.

**phase1_detect_time_radius_msec**

The `detect_time_radius_msec` parameter for phase 1 (see [scheme 1](./scheme1.md)).

**detect_time_radius_msec**

This is just like the `phase1_detect_time_radius_msec` parameter, but it is used for phase 2.

**phase1_npca_per_branch**

The `npca_per_branch` parameter for phase 1 (see [scheme 1](./scheme1.md)).

**phase1_pairwise_merge_step**

The `pairwise_merge_step` parameter for phase 1 (see [scheme 1](./scheme1.md)).

**detect_sign**

Same as in scheme 1 (see [scheme 1](./scheme1.md)).

**snippet_T1** and **snippet_T2**

Same as in scheme 1 (see [scheme 1](./scheme1.md)).

**snippet_mask_radius**

Same as in scheme 1 (see [scheme 1](./scheme1.md)).

**max_num_snippets_per_training_batch**

The maximum number of snippets to use for training the classifier in each batch.

**classifier_npca**

The number of principal components to use for the classifier.

**training_duration_sec**

The duration of the training data (in seconds).

**training_recording_sampling_mode**

How to sample the training data. If 'initial', then the first training_duration_sec of the recording will be used. If 'uniform', then the training data will be sampled uniformly in 10-second chunks from the recording.
