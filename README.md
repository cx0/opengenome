## OpenGenome dataset

This repository contains instructions to generate a functionally-equivalent* OpenGenome dataset curated and used for training [the Evo genomic foundation model](https://arcinstitute.org/news/blog/evo). OpenGenome is a large-scale dataset of prokaryotic genomes, plasmids, and viruses based on the following three main sources: 

| Database | Description | # Sequences** | # Tokens** | Download link |
| --- | --- | --- | --- | --- |
| GTDB | Bacterial and archeal genomes | 85,205 | ~273B | [GTDB v214.1](https://data.ace.uq.edu.au/public/gtdb/data/releases/release214/214.1/) |
| IMG/VR | Curated prokaryotic viruses | ~5.6M | ~82B | [IMG/VR v4](https://img.jgi.doe.gov/cgi-bin/vr/main.cgi) |
| IMG/PR | Plasmid sequences | ~700k | ~1.7B | [IMG/PR](https://genome.jgi.doe.gov/portal/IMG_PR/IMG_PR.home.html) |

\* The Evo team has not released the exact OpenGenome dataset used for training the Evo model yet. However, the instructions in this repository will allow you to generate a functionally-equivalent dataset for the purpose of training prokaryotic genomic foundation models.

\** Total number of sequences and tokens are based on the original datasets before applying any filtering steps.

**Important note**: while the sequences from IMG/VR and IMG/PR are publicly available, you need to register an account with DoE/JGI to download any data from their website.


## Data filtering steps

### GTDB subset
- Extract FASTA files for each GTDB representative genome (GCA and GCF identifiers referring to GenBank and RefSeq accession IDs, respectively). Note that the total bases reported in the Evo paper (Table S3) include the plasmid sequences as well.

### IMG/PR subset
- Only one representative per plasmid taxonomic unit (PTU) was kept. Authors simply picked the first sequence in each PTU group (see [notebook](/misc/img_pr.ipynb) to reproduce the original Figure S1-D in the paper)

### IMG/VR subset

#### Non-redundant subset
- Keep only sequences labeled "High-confidence" (IMG_VR_2022-09-20_6.1)
- Keep only one representative per viral operational taxonomic unit (vOTU)
#### Safety filter
- Remove potential eukaryotic viruses by keeping only sequences whose assigned taxonomic classification was found within a prokaryotic host *at least twice*
- Exclude all viruses assigned to any of the 19 families or 12 orders listed in the paper
#### Taxonomic quality
- Remove viral sequences with poor taxonomic specificity

**Note**: The filtering steps are based on the original Evo paper. The authors have not released the exact filtering criteria used for the Evo model training yet. IMG/VR subset after filtering is very similar to the one used in the Evo paper (Table S3) (see [notebook](/misc/img_vr.ipynb)). The difference is largely due to how the Riboviria sequences are handled.

## Citation

Please cite the original papers below if you use OpenGenome dataset in your work.

```
@article{parks2022gtdb,
  title={GTDB: an ongoing census of bacterial and archaeal diversity through a phylogenetically consistent, rank normalized and complete genome-based taxonomy},
  author={Parks, Donovan H and Chuvochina, Maria and Rinke, Christian and Mussig, Aaron J and Chaumeil, Pierre-Alain and Hugenholtz, Philip},
  journal={Nucleic acids research},
  volume={50},
  number={D1},
  pages={D785--D794},
  year={2022},
  publisher={Oxford University Press}
}

@article{chen2022img,
  title={IMG/VR v4: an update of the largest publicly available viral sequence database},
  author={Chen, I-Min A and Chu, Ken and Palaniappan, Krishna and Ratner, Anna and Huang, Jinghua and Huntemann, Marcel and Varghese, Neha and White, James R and Seshadri, Rekha and Elgin, Sarah and others},
  journal={Nucleic acids research},
  volume={50},
  number={D1},
  pages={D570--D578},
  year={2022},
  publisher={Oxford University Press}
}

@article {nguyen2024sequence,
    author = {Eric Nguyen and Michael Poli and Matthew G Durrant and Armin W Thomas and Brian Kang and Jeremy Sullivan and Madelena Y Ng and Ashley Lewis and Aman Patel and Aaron Lou and Stefano Ermon and Stephen A Baccus and Tina Hernandez-Boussard and Christopher Ré and Patrick D Hsu and Brian L Hie},
    title = {Sequence modeling and design from molecular to genome scale with Evo},
    year = {2024},
    doi = {10.1101/2024.02.27.582234},
    publisher = {Cold Spring Harbor Laboratory},
    URL = {https://www.biorxiv.org/content/early/2024/02/27/2024.02.27.582234},
    journal = {bioRxiv}
}
```