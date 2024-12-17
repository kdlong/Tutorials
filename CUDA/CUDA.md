Trying to run [this tutorial](https://developer.nvidia.com/blog/even-easier-introduction-cuda/)

To run on submit

Take an up to date CUDA singularity image from NVIDIA

In principal I think this should work:
```bash
APPTAINER_CACHEDIR=/scratch/submit/cms/kdlong singularity run docker://nvidia/cuda:12.6.3-cudnn-devel-ubi9
```

But memory usage seems to prefer that I build it as singularity first and then use the container directly:

```bash
APPTAINER_CACHEDIR=/scratch/submit/cms/kdlong singularity build cuda12.6.3-cudnn-devel-ubi9.sif docker://nvidia/cuda:12.6.3-cudnn-devel-ubi9
```

Some reminders: 
* APPTAINER_CACHEDIR is needed to avoid the temp dir filling up when pulling the container.
* The container is a relatively new one that's compatible with RHEL9, from [here](https://hub.docker.com/r/nvidia/cuda/tags)

Then check out a GPU with slurm

```bash
salloc --partition=submit-gpu-a30
```

and start the singularity container with nvidia drivers

```bash
singularity run --nv cuda12.6.3-cudnn-devel-ubi9.sif
```

Note that nvprof doesn't exist anymore. To use the current nvidia tool, nsys, you need to install it in a new container built from the nvidia one.
You can build the file from the [Dockerfile](Dockerfile) in this repo. It is built on top of nvidia/cuda:12.6.3-cudnn-devel-ubuntu20.04. Ubuntu is
used so that apt-get is usable. The install commands for nsys came from [here](https://developer.nvidia.com/blog/using-nsight-compute-in-containers/)

Can build the container with Docker and then convert to a .sif with Singularity:

```bash
docker buildx build -t kdlong/cuda_tutorial:v1 .
docker push kdlong/cuda_tutorial:v1
```
```bash
APPTAINER_CACHEDIR=/scratch/submit/cms/kdlong singularity build cuda_tutorial.sif docker://kdlong/cuda_tutorial:v1
```

Then run on submit-gpu-a30 in the same way as above
