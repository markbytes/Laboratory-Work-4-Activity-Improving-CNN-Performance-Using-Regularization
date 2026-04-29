# Laboratory-Work-4-Activity-Improving-CNN-Performance-Using-Regularization

Google Collab Link: https://colab.research.google.com/drive/1Bip9bLgwB81Yvu3vgEa5aNldf3OZj_Ji?usp=sharing

# Activity 2 Part 7 Analysis (Grad-CAM Interpretation)

Grad-CAM Results Interpretation

Based on the Grad-CAM heatmap, the model shows **weak and scattered feature learning**. The heatmap is almost entirely uniform red/orange across the whole image, indicating that the model is not focusing on any specific region but rather activating broadly across the entire input. In the overlay, while there is some partial attention toward the flower petals, the activation is still spread across the background, suggesting that the model is somewhat confused about which features are most relevant for classification. This is consistent with the model's relatively low validation accuracy of ~76.7%, meaning it has not yet fully learned to isolate and focus on the distinguishing features of each plant species. Further improvements such as more training epochs, additional images per class, or transfer learning would help the model develop sharper and more focused feature attention.

# PART 4: Compare Results (Before vs After)
<img width="901" height="369" alt="image" src="https://github.com/user-attachments/assets/b293cd58-f582-4635-9952-841343305afe" />

# GUIDE QUESTIONS (Student Explanation & Reflection)

A. Model Evaluation Analysis
1. Weakest-performing classes based on the confusion matrix:
The weakest-performing classes were Adelfa Plant (F1-score of 0.40 and precision of only 0.41), Cupid Peperomia (F1-score of 0.55), and Copper Leaf (F1-score of 0.58). These classes had the most misclassifications in the confusion matrix, likely because their visual features — such as leaf shape and color — are similar to other plant species in the dataset.
2. How Precision, Recall, and F1-score varied across classes:
There was notable variation across the 20 classes. Classes like Aster Flower (precision 0.92), Purple Heart (0.94), and Blue Pea Vine (0.88) performed consistently well across all three metrics. In contrast, Adelfa Plant and Polka Dot showed imbalanced scores — for example, Polka Dot had a high recall of 0.91 but very low precision of 0.47, meaning it was over-predicting that class and misclassifying other plants as Polka Dot.
3. What a low recall indicates:
A low recall means the model is failing to correctly identify actual instances of that class — in other words, it is producing many false negatives. For example, Garden Croton had a recall of only 0.47, meaning the model missed more than half of the actual Garden Croton images and likely classified them as other species.
4. How AUC score reflects performance compared to accuracy:
While the improved model's accuracy dropped to 68.44% compared to the baseline's 89.60%, the AUC score remained strong at 0.9062 versus the baseline's 0.9735. This shows that AUC is a more reliable indicator of true model capability because it measures how well the model separates classes across all thresholds, regardless of class imbalance. The baseline's high accuracy was largely due to overfitting, while the improved model's AUC confirms it still has strong discriminative ability on unseen data.

B. Model Improvement<br><br>
5. How data augmentation affected validation accuracy:<br>
Data augmentation initially caused training accuracy to appear lower because the model was seeing harder, more varied versions of each image. However, it helped validation accuracy become more stable and consistent throughout training. In the accuracy curves, validation accuracy was frequently higher than training accuracy, which is a healthy sign that the model was generalizing rather than memorizing.<br><br>
6. Why Batch Normalization is important in CNNs:<br>
Batch Normalization normalizes the output of each convolutional layer before passing it to the next, which stabilizes and accelerates training. In this model, it helped the loss decrease more smoothly and consistently across all 20 epochs, preventing erratic gradient updates that can slow down or destabilize learning.<br><br>
7. The role of Dropout in improving the model:<br>
Dropout randomly deactivates a percentage of neurons during each training step, forcing the network to learn more robust and distributed feature representations rather than relying on specific neurons. In this model, dropout rates of 0.4 and 0.5 were applied, which significantly reduced the overfitting seen in the baseline model where training accuracy reached 98.26% while validation was already diverging.<br><br>
8. How Early Stopping prevented overfitting:<br>
Early Stopping monitored the validation loss at each epoch and would have halted training automatically if it stopped improving for 5 consecutive epochs, restoring the best weights seen during training. In this case the model trained all 20 epochs since it kept improving, but the callback ensured that even if it had started to overfit beyond epoch 20, the best-performing version of the model would have been preserved.<br><br>

C. Performance Comparison<br><br>
9. Improvements observed after modifying the model:<br>
The most notable improvement was in model generalization. The baseline model showed clear overfitting with a training accuracy of 98.26% and validation accuracy of 89.60%, with a large and growing gap between the two. The improved model brought training accuracy down to 64.40% and validation to 68.44%, with validation accuracy actually exceeding training accuracy throughout most of training — a clear sign of better generalization. The AUC score also remained high at 0.9062.<br><br>
10. Which enhancement contributed most to improvement:<br>
Data augmentation combined with Dropout contributed the most to reducing overfitting. The aggressive augmentation — which included horizontal and vertical flipping, rotation, zoom, and contrast variation — ensured the model was exposed to diverse versions of each image, while the higher dropout rates of 0.4 and 0.5 prevented the model from memorizing specific training examples. These two together are the primary reason validation accuracy consistently stayed close to or above training accuracy.<br><br>
11. Whether the gap between training and validation accuracy decreased:<br>
Yes, the gap decreased significantly and even reversed. In the baseline model, training accuracy far exceeded validation accuracy, which is a classic sign of overfitting. In the improved model, validation accuracy was consistently higher than training accuracy throughout most of the 20 epochs, as clearly visible in the accuracy improvement curve. This confirms that the enhancements successfully addressed the overfitting problem.<br><br>

D. Explainability (Grad-CAM Integration)<br><br>
12. How Grad-CAM helped in understanding model predictions:<br>
Grad-CAM provided a visual explanation of which regions of the image the model focused on when making its prediction. Instead of treating the model as a black box, Grad-CAM generated a heatmap overlay on the original image that highlighted the areas with the highest activation, allowing us to understand whether the model was looking at the actual plant features or irrelevant background areas.<br><br>
13. Whether the improved model focused on more relevant regions:<br>
Based on the Grad-CAM overlay, the model showed partial focus on the flower petals of the test image, with some activation spread across the background as well. While not perfectly focused, this is an improvement over a randomly scattered heatmap, suggesting the model has begun learning some plant-specific features. However, the scattered activation still indicates there is room for further improvement, which aligns with the 68.44% validation accuracy.<br><br>
14. Why explainability is important in real-world AI applications:<br>
Explainability is critical in real-world AI systems because it builds trust and accountability. In applications like plant disease detection, agricultural monitoring, or medical imaging, users and decision-makers need to understand why a model made a certain prediction before acting on it. A model that is accurate but unexplainable can be dangerous — for example, if it predicts the wrong plant species for a critical reason that goes undetected. Grad-CAM and similar tools help developers identify model weaknesses, catch bias, and ensure the model is learning the right features for the right reasons.
