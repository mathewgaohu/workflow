## Motivation
- Jupyter Notebook is good for representing results.
- Some work needs high-speed computation / larger memory, which is only available on HPC clusters. 
- Want to run Jupyter Notebook on HPC clusters. 


## Preparation
1. Install Jupyter on the cluster.


## Use sbatch to submit a Jupyter Notebook job
This is a template of the sbatch file.
```bash
#!/bin/bash
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --mem=4G
#SBATCH --time=00:05:00
#SBATCH --job-name=jupyter-notebook

# get tunneling info
XDG_RUNTIME_DIR=""
node=$(hostname -s)
user=$(whoami)
cluster="tigercpu"
port=8889

# print tunneling instructions jupyter-log
echo -e "
Command to create ssh tunnel:
ssh -N -f -L ${port}:${node}:${port} ${user}@${cluster}.princeton.edu

Use a Browser on your local machine to go to:
localhost:${port}  (prefix w/ https:// if using password)
"

# load modules or conda environments here
module load anaconda3/2023.3

# Run Jupyter
jupyter-notebook --no-browser --port=${port} --ip=${node}
```

Submit this file and follow the output. 


Reference: 
- https://researchcomputing.princeton.edu/support/knowledge-base/jupyter
