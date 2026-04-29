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

# PART 4: Compare Results (Before vs After)
<img width="1440" height="600" alt="image" src="https://github.com/user-attachments/assets/0ea0577f-a61b-41e8-a266-07d96c950323" />

# GUIDE QUESTIONS (Student Explanation & Reflection)
A. Model Evaluation Analysis
1. Weakest-performing classes based on the confusion matrix:
Adelfa was the weakest in both models — baseline F1 of 0.61, improved F1 of only 0.49. Katakataka (0.69), Red_Powder_Puff (0.71), and Celosia (0.72) were also consistently weak. The confusion matrix shows Adelfa being misclassified across many different classes, suggesting high visual ambiguity.
2. How Precision, Recall, and F1 varied across classes:
Blue_Pea_Vine had perfect precision (1.00) in both models, while Purple_Heart achieved the highest F1 (0.94) in the improved model. In contrast, Adelfa had the lowest scores across all three metrics. This variation reflects how visually distinct each plant class is — classes with unique colors or shapes scored higher.
3. What low recall indicates:
Low recall means the model is missing actual instances of that class — it fails to identify them even when they are present. For example, Adelfa's recall of 0.50 in the improved model means it correctly found only half of all actual Adelfa samples, misclassifying the rest as other plants.
4. How AUC reflects performance compared to accuracy:
AUC measures how well the model distinguishes between classes across all classification thresholds, not just at one fixed threshold. The baseline AUC of 0.9576 means it was very good at ranking correct classes higher than incorrect ones, even though its accuracy was 83%. AUC is more reliable than accuracy alone because it is not affected by class imbalance.

B. Model Improvement<br>
  5. How data augmentation affected validation accuracy:<br>
  - Data augmentation made the task harder for the model during training by randomly flipping, rotating, zooming, and adjusting images each epoch. This slowed convergence — the improved model only reached 69% training accuracy after 40 epochs versus the baseline's 89% after 15. However, augmentation reduces overfitting by preventing the model from memorizing fixed image patterns, which is why the gap between training and validation accuracy was smaller in the improved model.<br><br>
  6. Why Batch Normalization is important in CNNs:<br>
- Batch Normalization normalizes the output of each layer before passing it to the next. This stabilizes training by preventing activations from becoming too large or too small, allows higher learning rates, and speeds up convergence. It also acts as a mild regularizer, reducing the need for aggressive dropout.<br><br>
7. The role of Dropout:<br>
- Dropout randomly deactivates a percentage of neurons during each training step, forcing the network to learn redundant representations rather than relying on specific neurons. This directly prevents overfitting. The improved model used 0.4 dropout after the conv layers and 0.5 after the dense layer, which helped keep val accuracy closer to training accuracy.<br><br>
8. How Early Stopping prevented overfitting:<br>
- Early stopping monitored val loss every epoch and stopped training when it did not improve for 5 consecutive epochs, then restored the weights from the best epoch. This automatically prevented the model from continuing to train past its peak, which is exactly what happened in the baseline when overfitting began at epoch 13.<br><br>

C. Performance Comparison<br>
9. What improvements were observed:<br>
  - The improved model showed a smaller gap between training and validation accuracy (69.3% train vs 77.2% val) compared to the baseline (89.6% train vs 83.3% val). This indicates less overfitting. However, the overall validation accuracy decreased, which means the model needs more training time or a stronger architecture like transfer learning to surpass the baseline.<br><br>
10. Which enhancement contributed most:<br>
- Early stopping contributed most to generalization control, as it directly prevented the model from overfitting past its best checkpoint. BatchNormalization was the second most impactful, stabilizing training across 40 epochs which would otherwise have been very unstable.<br><br>
11. Did the gap between training and validation accuracy decrease:<br>
- Yes. The baseline had a 6.28% gap (89.58% train vs 83.30% val), while the improved model had a negative gap — val accuracy (77.16%) actually exceeded training accuracy (69.34%). This is a strong sign of reduced overfitting and shows that augmentation and regularization worked, even though the absolute accuracy was lower due to the model needing more epochs to fully converge.<br><br>

D. Explainability (Grad-CAM)<br>
12. How Grad-CAM helped understand model predictions:<br>
- Grad-CAM revealed which regions of the input image the model focused on when making its classification decision. By visualizing the gradient-weighted activations of the last convolutional layer, it showed whether the model was attending to the plant itself or to irrelevant background areas.<br><br>
13. Did the improved model focus on more relevant regions:<br>
- Based on the baseline Grad-CAM results, the heatmap was scattered and uniform across the entire image, indicating weak feature learning. Running Grad-CAM on the improved model after retraining would be needed to confirm improvement. However, since the improved model uses BatchNormalization and deeper filters (32→64→128), its feature maps are more stable and likely to produce more focused activations — this can be verified by re-running Activity 2 on the improved model.<br><br>
14. Why explainability is important in real-world AI applications:<br>
- In real-world applications like medical diagnosis, agriculture, or security, a model's decision must be trustworthy and transparent. If a model classifies a plant disease incorrectly and no one can explain why, it cannot be corrected. Grad-CAM and other XAI tools allow developers and users to verify that the model is learning the right features, catch bias or shortcuts the model may have learned, and build trust with end users who need to act on the model's predictions.
