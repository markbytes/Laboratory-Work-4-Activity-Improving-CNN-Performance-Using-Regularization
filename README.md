# Laboratory-Work-4-Activity-Improving-CNN-Performance-Using-Regularization

Google Collab Link: https://colab.research.google.com/drive/1EQCW6wQ19eJqolGaT1uAKKhc_mJanKuD?usp=sharing

# Activity 2 Part 7 Analysis (Grad-CAM Interpretation)

The Grad-CAM heatmap shows a nearly uniform green activation across the entire image with no concentrated hotspot on the plant. The overlay confirms this — the activation is scattered throughout the background (shown by the widespread magenta coloring) rather than focused on the plant's distinctive features like its leaves, shape, or structure.
This corresponds to the "Scattered heatmap = Weak feature learning" case. The model is not clearly identifying what makes the plant unique — it is responding to broad, distributed patterns across the whole image rather than locking onto the object of interest.
Possible reasons:

The CNN filters (especially with only 16→32→64 channels) may not have learned strong enough discriminative features
No BatchNormalization in the baseline model means feature maps can be noisy
The background of the image is visually complex, and the model may be picking up on background cues instead of the plant itself

What this means going forward (Activity 3):
Adding BatchNormalization, deeper filters (32→64→128), and stronger data augmentation should help the model focus on the actual plant features — which you can verify by running Grad-CAM again after retraining and checking if the heatmap becomes more concentrated on the plant itself.
