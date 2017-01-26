
# PyTorch binding for WarpCTC

## Installation

Install [PyTorch](https://github.com/pytorch/pytorch#installation).

The bindings assume you followed the standard warp-ctc install instructions from source (given below).
```
git clone https://github.com/baidu-research/warp-ctc.git
cd warp-ctc
mkdir build && cd build && cmake .. && make
```

Now install the bindings
```
cd pytorch_binding
python setup.py install
```

Example to use the bindings below.

```python
    import torch
    from torch.autograd import Variable
    from warpctc_pytorch import CTCLoss
    ctc_loss = CTCLoss()
    # expected shape of seqLength x batchSize x alphabet_size
    probs = torch.FloatTensor([[[0.1, 0.6, 0.1, 0.1, 0.1], [0.1, 0.1, 0.6, 0.1, 0.1]]]).transpose(0, 1).contiguous()
    labels = Variable(torch.IntTensor([1, 2]))
    label_sizes = Variable(torch.IntTensor([2]))
    probs_sizes = Variable(torch.IntTensor([2]))
    probs = Variable(probs, requires_grad=True) # tells autograd to compute gradients for probs
    cost = ctc_loss(probs, labels, probs_sizes, label_sizes)
    cost.backward()
```