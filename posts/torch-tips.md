---
aliases:
- /ml/2022/02/25/torch-tips
categories:
- ML
date: '2022-02-25'
description: PyTorch zen tips
layout: post
author: Zeel B Patel
title: PyTorch Tips
toc: true

---

Several tips for building `torch` models from scratch from my experience. Some of the tips are like zen, they are not immediately intuitive but useful for efficient code.

* All the initializations or new tensor creation should only happen in `__init__` method. During the `forward()` call, ideally no new tensors should be created from scratch such as `torch.zeros()`, `torch.ones()` etc.
**Reason:** Violating this can sometimes brake your forward pass and end-to-end backprop may become buggy.

* `.cuda()` and `.cpu()` are discouraged, use `.to(device)` instead.
**Reason:** `.to(device)` is more dynamic and scalable.

* Do not save models with `torch.save(model)`, that may become incompaitable with different torch versions and may take more memory. Save `torch.save(model.state_dict())` instead.

* Need to set parameter names dynamically? Use this example, `zero=0;self.register_parameter(f"name_{zero}")`. They can be accessed with `model.name_0`.

* Have something in model which is necessary for forward pass but does not require backprop? define those variables with `self.register_buffer`.

* Let `.to(device)` to be set outside the model defition.
**Reason:** It is less confusing to the users this way and it is less messy with internal tools to set device such as:
    * `module.to(deivce)` sends all parameters and buffers of model/submodules to the device.

* `module.float()` or `module.double()` will convert all model/submodule parameters and buffers into `float32` and `float64` respectively.

* Let `.train()` and `.eval()` to be set outside the model defition or set by user. 
**Reason:** It can be confusing to user if these things are used inside the model against torch conventions.

* `torch.no_grad()` should not be used within the model.
**Reason:** Sometimes user may want to backprop through that chunk of code. 

* Link the multiple modules togather.
**Reason:** Ideally, it is useful if model is built like a assembled product (say a car). You should be able to replace the parts as per your requirement. Several benefits on these lines are:
    * setting `module.train()` or `module.eval()` puts all submodules in train mode or eval mode respectively.
    * All submodules parameters can be accesses directly from the parent module with `module.parameters()`.

* Creating a list of parameters in model `__init__` definition? consider `torch.nn.ModuleList(params)` else individual parameters in the list will not be recognized as parameters.