# ROCm-Megatron-LM-Testing
This repository is intended to quickly setup a test environment for ROCm-Megatron-LM in LUMI. This repository do _not_ contain any code or scirpts, just instructions how to do the setup.
- Note that `USE_FUSIONS` is might not be working correctly &rarr; for reason X Megatron tries to compile fused kernels even they are explicitly disabled
# Setup
1. Clone the official [ROCm/Megatron-LM](https://github.com/ROCm/Megatron-LM)
```bash
git clone https://github.com/ROCm/Megatron-LM
cd Megatron-LM
git checkout fe353fd
```
2. Download scripts, data and tokenizer from Allas
```bash
wget https://a3s.fi/te-error-repro-462000353/te-error-repro.tar.gz
tar -xvf te-error-repro.tar.gz
cd workdir
```
3. Add your project number and container into `train.sh`
```bash
    # Line 11: insert your project number
    #SBATCH --account=<your project number>

    #Line 260: insert your singularity container
    CONTAINER="<path to your container>"
```
4. Optional If you use container that is not provided by LUMI, update `launcher.sh`
 ```bash
    # Line 11: Remove/Replace the environment activation
    $WITH_CONDA
```
5. Test training with small model, without parallelisation and without fused kernels
```bash
cd workdir
sbatch train-sbatch.sh MODEL_SIZE=350M TEST_PARALLELISM=0 USE_FUSIONS=0
```
6. Test training with small model, without parallelisation using fused kernels
```bash
cd workdir
sbatch train-sbatch.sh MODEL_SIZE=350M TEST_PARALLELISM=0 USE_FUSIONS=1
```
7. Test training with larger model and parallelisation enabled
```bash
cd workdir
sbatch train-sbatch.sh MODEL_SIZE=7B TEST_PARALLELISM=1
```
8. Test training with larger model, parallelisation enabled and using fused kernels
```bash
cd workdir
sbatch train-sbatch.sh MODEL_SIZE=7B TEST_PARALLELISM=1 USE_FUSIONS=1
```
9. After successfully being able to run the steps below, try with scaling and communications $\geq16$ nodes