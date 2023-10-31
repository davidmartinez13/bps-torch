# bps_torch
A Pytorch implementation of the [bps](https://github.com/sergeyprokudin/bps) representation using chamfer distance on GPU. This implementation is very fast and was used for the [GrabNet](https://github.com/otaheri/GrabNet) model.

**Basis Point Set (BPS)** is a simple and efficient method for encoding 3D point clouds into fixed-length representations. For the original implementation please visit [this implementation](https://github.com/amzn/basis-point-sets) by [Sergey Prokudin](https://github.com/sergeyprokudin).


### Requirements

- Python >= 3.7
- PyTorch >= 1.1.0 
- Numpy >= 1.16.2
- [chamfer_distance](https://github.com/otaheri/chamfer_distance)

### Installation

If PyTorch is not installed run the following line:
```
 pip install torch==1.5.1+cpu torchvision==0.6.1+cpu -f https://download.pytorch.org/whl/torch_stable.html
```
To install the chamfer_distance package run:

```
pip install git+https://github.com/otaheri/chamfer_distance
```
Finally install the package using the command below:
```
pip install git+https://github.com/otaheri/bps_torch
```

In case of pytorch3d issue. Follow installation here: https://github.com/facebookresearch/pytorch3d/blob/main/INSTALL.md

### Demos

Below is an example of how to use the bps_torch code.

```python
import torch
import time
from bps_torch.bps import bps_torch

device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')

# initiate the bps module
bps = bps_torch(bps_type='random_uniform',
                n_bps_points=1024,
                radius=1.,
                n_dims=3,
                custom_basis=None)

pointcloud = torch.rand([1000000,3]).to(device)

s = time.time()

bps_enc = bps.encode(pointcloud,
                     feature_type=['dists','deltas'],
                     x_features=None,
                     custom_basis=None)

print(time.time() - s)

deltas = bps_enc['deltas']
bps_dec = bps.decode(deltas)

```

## Citation

If you use this code in your research, please consider citing:
```
@inproceedings{prokudin2019efficient,
  title={Efficient Learning on Point Clouds With Basis Point Sets},
  author={Prokudin, Sergey and Lassner, Christoph and Romero, Javier},
  booktitle={Proceedings of the IEEE International Conference on Computer Vision},
  pages={4332--4341},
  year={2019}
}
```

## License

This library is licensed under the MIT-0 License of the original implementation. See the [LICENSE](https://github.com/sergeyprokudin/bps/blob/master/LICENSE) file.

## Contact
The code of this repository was implemented by [Omid Taheri](https://ps.is.tue.mpg.de/person/otaheri).

For questions, please contact [omid.taheri@tue.mpg.de](mailto:omid.taheri@tue.mpg.de).
