[![CI](https://github.com/nogibjj/mlops-template/actions/workflows/cicd.yml/badge.svg?branch=GPU)](https://github.com/nogibjj/mlops-template/actions/workflows/cicd.yml)
[![Codespaces Prebuilds](https://github.com/nogibjj/mlops-template/actions/workflows/codespaces/create_codespaces_prebuilds/badge.svg?branch=GPU)](https://github.com/nogibjj/mlops-template/actions/workflows/codespaces/create_codespaces_prebuilds)

## Rust TensorFlow

Checkout project and cd into `rust` repo:  https://github.com/tensorflow/rust

* build with multicore: `cargo build -j 6 --features="eager" --example mobilenetv3`

* to test out GPU project, cd into `kick-tires-rust-tf` and do a `make build` then `cargo run` and also run `nvidia-smi -l 1`


## Verify GPU

The following examples test out the GPU (including Docker GPU)

* run pytorch training test: `python utils/quickstart_pytorch.py`
* run pytorch CUDA test: `python utils/verify_cuda_pytorch.py`
* run tensorflow training test: `python utils/quickstart_tf2.py`
* run nvidia monitoring test: `nvidia-smi -l 1` it should show a GPU
* run whisper transcribe test `./utils/transcribe-whisper.sh` and verify GPU is working with `nvidia-smi -l 1`
* run `lspci | grep -i nvidia` you should see something like:  `0001:00:00.0 3D controller: NVIDIA Corporation GV100GL [Tesla V100 PCIe 16GB] (rev a1)`


Additionally, this workspace is setup to fine-tune Hugging Face

![fine-tune](https://user-images.githubusercontent.com/58792/195709866-121f994e-3531-493b-99af-c3266c4e28ea.jpg)


`python hugging-face/hf_fine_tune_hello_world.py` 

#### Verify containerized GPU works for Tensorflow

*Because of potential versioning conflicts between PyTorch and Tensorflow it is recommended to run Tensorflow via GPU Container and PyTorch via default environment.* 

See [TensorFlow GPU documentation](https://www.tensorflow.org/install/docker)
* Run `docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu \
   python -c "import tensorflow as tf; print(tf.reduce_sum(tf.random.normal([1000, 1000])))"`

* Also interactively explore:  `docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu`, then when inside run:
`apt-get update && apt-get install pciutils` then `lspci | grep -i nvidia`

* To mount the code into your container:  `docker run --gpus all -it --rm -v $(pwd):/tmp tensorflow/tensorflow:latest-gpu /bin/bash`.  Then do `apt-get install -y git && cd /tmp`.  Then all you need to do is run `make install`.  Now you can verify you can train deep learning models by doing `python utils/quickstart_tf2.py`

### Setup Docker Toolkit NVidia

* [reference docs](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#install-guide)

![mlops-tensorflow-gpu](https://user-images.githubusercontent.com/58792/206875904-114b4cf0-879d-497b-8690-267dac8b222d.jpg)



### Used in Following Projects

Used as the base and customized in the following Duke MLOps and Applied Data Engineering Coursera Labs:

* [MLOPs-C2-Lab1-CICD](https://github.com/nogibjj/Coursera-MLOPs-Foundations-Lab-1-CICD)
* [MLOps-C2-Lab2-PokerSimulator](https://github.com/nogibjj/Coursera-MLOPs-Foundations-Lab-2-poker-simulator)
* [MLOps-C2-Final-HuggingFace](https://github.com/nogibjj/Coursera-MLOps-C2-Final-HuggingFace)
* [Coursera-MLOps-C2-lab3-probability-simulations](Coursera-MLOps-C2-lab3-probability-simulations)
* [Coursera-MLOps-C2-lab4-greedy-optimization](https://github.com/nogibjj/Coursera-MLOps-C2-lab4-greedy-optimization)
### References

* [Docker "one-liners" for Tensorflow recommenders](https://www.tensorflow.org/resources/recommendation-systems)
* [Watch GitHub Universe Talk:  Teaching MLOps at scale with Github](https://watch.githubuniverse.com/on-demand/ec17cbb3-0a89-4764-90a5-9debb58515f8)
* [Building Cloud Computing Solutions at Scale Specialization](https://www.coursera.org/specializations/building-cloud-computing-solutions-at-scale)
* [Python, Bash and SQL Essentials for Data Engineering Specialization](https://www.coursera.org/learn/web-app-command-line-tools-for-data-engineering-duke)
* [Implementing MLOps in the Enterprise](https://learning.oreilly.com/library/view/implementing-mlops-in/9781098136574/)
* [Practical MLOps: Operationalizing Machine Learning Models](https://www.amazon.com/Practical-MLOps-Operationalizing-Machine-Learning/dp/1098103017)
* [Coursera-Dockerfile](https://gist.github.com/noahgift/82a34d56f0a8f347865baaa685d5e98d)
