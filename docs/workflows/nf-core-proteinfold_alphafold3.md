# Guide: Running nf-core/proteinfold with AlphaFold3 on Alma

This guide explains how to run the [`nf-core/proteinfold`](https://github.com/nf-core/proteinfold/) Nextflow pipeline on Alma using Singularity and AlphaFold3 mode.

The setup below assumes:

- You are running on ICR HPC, called ALMA.
- You have already downloaded or have have access to the AlphaFold3 model parameters.
To download, please refer to [Downloading AlphaFold Parameters](../recipes/alphafold3.md#downloading-alphafold-parameters)
- The AlphaFold3 databases are available on Alma HPC under `/data/reference-data/alphafold_db`.
- You want to use the latest ICR AlphaFold3 container image:
  `docker://icrsc/proteinfold_alphafold3_b1f7802:2.0.0`.

---

## 1. Create and activate a Conda environment

Create a Conda environment containing Nextflow version 25.10.3:

```bash
conda create --name nf-env -c conda-forge -c bioconda nextflow=25.10.3
conda activate nf-env
```

Check that Nextflow is available and the version matches the requested one:

```bash
nextflow -version
```

---

## 2. Configure Nextflow storage locations

By default, Nextflow stores pipeline files in your home directory. On Alma, home storage is limited, so it is recommended to redirect the Nextflow assets directory and the Singularity cache directory to scratch space.

Open your `~/.bashrc` file using `vi`, `vim` or `nano`:

```bash
vi ~/.bashrc
```

Add the following lines. Replace the placeholder path with your own personal scratch path:

```bash
export NXF_ASSETS=/data/scratch/personal/path/<your_username>/.nextflow/assets
export NXF_SINGULARITY_CACHEDIR=/data/scratch/shared/SINGULARITY-DOWNLOAD/nextflow/.singularity
```

Reload your shell configuration:

```bash
source ~/.bashrc
```

Check that the variables are set correctly:

```bash
echo $NXF_ASSETS
echo $NXF_SINGULARITY_CACHEDIR
```

---

## 3. AlphaFold3 container image

For the latest AlphaFold3 setup on Alma GPU nodes, use the following Docker image through Singularity:

```text
docker://icrsc/proteinfold_alphafold3_b1f7802:2.0.0
```

Docker Hub page:

```text
https://hub.docker.com/layers/icrsc/proteinfold_alphafold3_b1f7802/2.0.0
```

This image is specified in the custom Nextflow configuration file below.

---

## 4. Prepare the AlphaFold3 parameters file

Create a file called:

```text
alphafold3_params.json
```

Example content:

```json
{
  "alphafold3_small_bfd_path": "/data/reference-data/alphafold_db/bfd-first_non_consensus_sequences.fasta",
  "alphafold3_params_path": "/data/scratch/DCO/DIGOPS/SCIENCOM/ashcherbakova/af3_model/af3.bin",
  "alphafold3_mgnify_path": "/data/reference-data/alphafold_db/mgy_clusters_2022_05.fa",
  "alphafold3_pdb_mmcif_path": "/data/reference-data/alphafold_db/mmcif_files",
  "alphafold3_uniref90_path": "/data/reference-data/alphafold_db/uniref90_2022_05.fa",
  "alphafold3_pdb_seqres_path": "/data/reference-data/alphafold_db/pdb_seqres_2022_09_28.fasta",
  "alphafold3_uniprot_path": "/data/reference-data/alphafold_db/uniprot_all_2021_04.fa",
  "alphafold3_rnacentral_path": "/data/reference-data/alphafold_db/rnacentral_active_seq_id_90_cov_80_linclust.fasta",
  "alphafold3_nt_rna_path": "/data/reference-data/alphafold_db/nt_rna_2023_02_23_clust_seq_id_90_cov_80_rep_seq.fasta",
  "alphafold3_rfam_path": "/data/reference-data/alphafold_db/rfam_14_9_clust_seq_id_90_cov_80_rep_seq.fasta"
}
```

Update the `alphafold3_params_path` value if your AlphaFold3 model parameter file is stored somewhere else.

---

## 5. Prepare the custom Nextflow configuration file

Create a file called:

```text
custom_alphafold3.config
```

Example content:

```nextflow
singularity {
    enabled = true
    autoMounts = true
    runOptions = '--nv --bind /mnt:/mnt --bind /data:/data'
}

process {
    withName: /.*RUN_ALPHAFOLD3.*/ {
        queue = 'gpuhm'
        container = 'docker://icrsc/proteinfold_alphafold3_b1f7802:2.0.0'
        clusterOptions = '--gres=gpu:1'

        ext.args = '--flash_attention_implementation=xla --db_dir=/data/reference-data/alphafold_db --ntrna_database_path=/data/reference-data/alphafold_db/nt_rna_2023_02_23_clust_seq_id_90_cov_80_rep_seq.fasta --rfam_database_path=/data/reference-data/alphafold_db/rfam_14_9_clust_seq_id_90_cov_80_rep_seq.fasta --rna_central_database_path=/data/reference-data/alphafold_db/rnacentral_active_seq_id_90_cov_80_linclust.fasta'
    }
}
```

This configuration does three important things:

1. Enables Singularity.
2. Adds GPU support using `--nv`.
3. Overrides the AlphaFold3 process to use the custom container image.

---

## 6. Prepare the input samplesheet

Create a samplesheet called:

```text
samples.csv
```

Example:

```csv
id,fasta
my_protein,data/sequences/my_protein.fasta
```

Make sure the FASTA file exists at the path specified in the `fasta` column.

Example FASTA location:

```text
data/sequences/my_protein.fasta
```

Example my_protein.fasta :

```text
>my_protein
MKTAYIAKQRQISFVKSHFSRQDILDLWQ
```

---

## 7. Create the SLURM submission script

Create a file called:

```text
run_proteinfold_af3.sh
```

Example content:

```bash
#!/bin/bash
#SBATCH --job-name=nf-proteinfold
#SBATCH --output=proteinfold.out
#SBATCH --error=proteinfold.err
#SBATCH --partition=master-worker
#SBATCH --ntasks=1
#SBATCH --time=1:00:00
#SBATCH --mem-per-cpu=4000

source ~/.bashrc

env_path="/<path/to/your/nextflow/env/nf-env>"
conda activate "${env_path}"

model_params="/path/to/model/params/af3_model/af3.bin"
database="/data/reference-data/alphafold_db"

nextflow run /data/scratch/shared/RSE/nextflow-pipelines/proteinfold \
  -profile singularity,icr_alma \
  --input samples.csv \
  --outdir results_af3_latest \
  --mode alphafold3 \
  --use_gpu \
  --db "$database" \
  --alphafold3_db "$database" \
  --alphafold3_params_path "$model_params" \
  -params-file alphafold3_params.json \
  -c custom_alphafold3.config \
  -resume
```

Update these values before running:

```bash
env_path="<path/to/your/nextflow/env/nf-env>"
model_params="/path/to/model/params/af3_model/af3.bin"
```

---

## 8. Submit the job

Submit the SLURM job:

```bash
sbatch run_proteinfold_af3.sh
```

Check the job status:

```bash
squeue -u $USER
```

Monitor the output and error logs:

```bash
tail -f proteinfold.out
tail -f proteinfold.err
```

---

## 9. Expected output

The output directory is defined in the run command:

```bash
--outdir results_af3_latest
```

After a successful run, results should be available in:

```text
results_af3_latest/
```

---

## 10. Troubleshooting

### Container is pulled from the wrong registry

If Nextflow tries to pull:

```text
docker://nf-core/proteinfold_alphafold3_standard:2.0.0
```

instead of the custom image, check that your `custom_alphafold3.config` file contains:

```nextflow
container = 'docker://icrsc/proteinfold_alphafold3_b1f7802:2.0.0'
```

Also confirm that the config file is passed to Nextflow:

```bash
-c custom_alphafold3.config
```

### Singularity cache or assets are stored in home directory

Check:

```bash
echo $NXF_ASSETS
echo $NXF_SINGULARITY_CACHEDIR
```

If either variable points to your home directory or is empty, update your `~/.bashrc` and reload it:

```bash
source ~/.bashrc
```

### GPU is not detected

Confirm that the custom config includes:

```nextflow
runOptions = '--nv --bind /mnt:/mnt --bind /data:/data'
```

Also confirm that the process requests a GPU:

```nextflow
clusterOptions = '--gres=gpu:1'
```

### Resume a failed or interrupted run

The run command already includes:

```bash
-resume
```

This tells Nextflow to reuse completed tasks where possible.

---

## 11. Minimal checklist before running

Before submitting the job, confirm that these files exist:

```bash
ls -lh samples.csv
ls -lh alphafold3_params.json
ls -lh custom_alphafold3.config
ls -lh run_proteinfold_af3.sh
ls -lh data/sequences/my_protein.fasta
```

Confirm that the database directory exists:

```bash
ls -lh /data/reference-data/alphafold_db
```

Confirm that the AlphaFold3 model parameter file exists:

```bash
ls -lh /path/to/model/params/af3_model/af3.bin
```

Then submit:

```bash
sbatch run_proteinfold_af3.sh
```
