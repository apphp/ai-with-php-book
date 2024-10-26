# Tensor Manipulation

####

### Tensor Manipulation

#### 1. Reshaping

```python
# Example in PyTorch
x = torch.randn(10, 20)  # 2D tensor
y = x.reshape(200)       # Flatten to 1D
z = x.view(2, 10, 10)    # Reshape to 3D
```

#### 2. Broadcasting

Rules for operating on tensors with different shapes:

1. Add leading dimensions of size 1
2. Expand dimensions of size 1 to match larger size
3. Shapes must be compatible after expansion

#### 3. Indexing and Slicing

```python
# Example in NumPy
x = np.random.randn(4, 3, 2)
slice_1 = x[1:3, :, :]    # Select specific items
slice_2 = x[:, None, :]   # Add new dimension
```

