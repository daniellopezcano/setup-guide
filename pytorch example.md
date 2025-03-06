```python
import torch
import torch.nn as nn
import torch.optim as optim

# ======= Data Preparation =======

num_classes = 2
batch_size = 8
input_dim = 2 # Number of input features

# Generate random data (8 samples, 2 features each)
xx = torch.randn(batch_size, input_dim)  

# Generate random class labels (0 to num_classes-1)
yy = torch.randint(0, num_classes, (batch_size,))

# Compute class weights (inverse frequency)
class_counts = torch.bincount(yy, minlength=num_classes).float()
class_weights = 1.0 / class_counts
class_weights = class_weights / class_weights.sum() # Normalize

# ======= Model Definition =======
model = nn.Sequential(
nn.Linear(input_dim, 16),
nn.ReLU(),
nn.Linear(16, num_classes) # Multi-class output (no softmax needed)
)
print(model)

# ======= Loss Function (with Class Weights) =======
criterion = nn.CrossEntropyLoss(weight=class_weights)

# ======= Optimizer =======
optimizer = torch.optim.AdamW([*model.parameters()], lr=0.01, betas=(0.9, 0.999), eps=1e-8, amsgrad=False, weight_decay=0.00000000001)

# ======= Training Loop =======
num_epochs = 100
	for epoch in range(num_epochs):
	logits = model(xx) # Forward pass
	loss = criterion(logits, yy) # Compute loss
	if epoch == 0:
		print(logits.shape)
		print(logits)
		print(yy.shape)
		print(yy)
		print(loss)

	optimizer.zero_grad() # Reset gradients
	loss.backward() # Backpropagation
	torch.nn.utils.clip_grad_norm_([*model.parameters()], max_norm=100)
	optimizer.step() # Update weights
	
	if (epoch + 1) % 10 == 0:
		print(f"Epoch {epoch+1}/{num_epochs}, Loss: {loss.item():.4f}")

# ======= Prediction Example =======
with torch.no_grad():
	prediction = model(xx) # Get logits
	predicted_class = torch.argmax(prediction, dim=1) # Get class index
	print(prediction)
	print(predicted_class)
	print(yy)
```