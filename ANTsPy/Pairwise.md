# ANTsPy-VII: Pair-wise tab, registering one image to another

Previous: [ANTsPy-VI: Analysis tab, template mask and Jacobian analysis](Analysis.md) | Next: [ANTsPy-VIII: Troubleshooting and advanced topics](Troubleshooting.md)

---

## Using an Existing Transform as the Initial Transform

The **Pair-wise** tab can start a registration from a transform that is already in the scene, instead of computing one from landmarks. This is useful when the two specimens are far apart to begin with, and when another tool has already produced an alignment: a previous registration, ALPACA, or FastModelAlign.

1. Make sure the transform is loaded in the scene (**Data** module)
2. Open the **Pair-wise** tab and set **Fixed Image** and **Moving Image**
3. Check ☑ **Initial Transform**
4. Select ◉ **Use existing transform:** and pick the transform node
5. Set **Transform Type** as usual and click **Run Registration**

**Pick the last node of a chain.** Tools that align in several steps leave a chain of transforms in the scene - FastModelAlign, for example, leaves `<name>_scaling` → `<name>_rigid` → `<name>_deformable` - and only the last node carries the complete alignment. The module follows the whole parent chain of the node you select, so selecting the leaf is correct; selecting a node in the middle initializes the registration with only part of the alignment. Check the **Data** module if you are not sure which node is the leaf.

**Linear and non-linear transforms are handled differently.** A linear transform (rigid, similarity, affine) is passed to ANTs as-is. A non-linear one - a thin plate spline, a grid transform, or any chain that contains one - is first flattened into a displacement field sampled on the **fixed image** grid, because ANTs cannot read the transform types Slicer writes for those.

**Displacement field downsampling** controls the resolution of that field. The field holds one vector per voxel of the fixed image, so at micro-CT resolution it becomes large: a 0.1 mm scan of a mouse skull produces a field of several hundred MB, which is slow to write and to read back.

- **1.0** (default) - the field matches the fixed image resolution. Most accurate, largest field.
- **2.0 to 4.0** - recommended when the initial transform is a smooth, landmark-driven warp. A factor of 2 makes the field 8 times smaller, a factor of 4 makes it 64 times smaller. In testing, a landmark warp resampled at a factor of 4 reproduced the original transform to about 0.002 mm, far below one voxel.
- Leave it at 1.0 when the initial transform carries fine local detail, such as the output of a previous deformable registration.

The control is only active for **Use existing transform**, since a landmark-based initial transform does not need it. The **Label Image Reg** tab has the same control, where the field is sampled on the fixed label image instead.

**The inverse transform is not available with a non-linear initial transform.** ANTs can only produce an inverse when every part of the registration can be inverted, and a displacement field cannot be inverted analytically. If you select an **Inverse Transform** output in this case, the registration still completes and the forward transform and the resampled volume are correct, but the inverse output node is left empty and a message explains why. Use a linear initial transform if you need the inverse.

---

Previous: [ANTsPy-VI: Analysis tab, template mask and Jacobian analysis](Analysis.md) | Next: [ANTsPy-VIII: Troubleshooting and advanced topics](Troubleshooting.md)
