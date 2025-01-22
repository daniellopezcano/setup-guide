# Histogram
``` python
fig, ax = mpl.pyplot.subplots(1,1, figsize=(4,4))

tmp_xx = truth_EPS
yy, bin_edges = np.histogram(tmp_xx, bins=100, range=(np.min(tmp_xx), np.max(tmp_xx)))
xx = (bin_edges[1:] + bin_edges[:-1])/2

ax.plot(xx, yy)

fig.tight_layout()
mpl.pyplot.show()
```

# colors
![[Pasted image 20220908121800.png]]

# colormaps
![[Pasted image 20220908122137.png]]
![[Pasted image 20220908122158.png]]


![[Pasted image 20220929115658.png]]
```python
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

# X AXIS -BORDER
ax.spines['bottom'].set_visible(False)
# BLUE
ax.set_xticklabels([])
# RED
ax.set_xticks([])
# RED AND BLUE TOGETHER
ax.axes.get_xaxis().set_visible(False)

# Y AXIS -BORDER
ax.spines['left'].set_visible(False)
# YELLOW
ax.set_yticklabels([])
# GREEN
ax.set_yticks([])
# YELLOW AND GREEN TOGHETHER
ax.axes.get_yaxis().set_visible(False)
```