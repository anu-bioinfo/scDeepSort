# DeepSort

### Reference-free Cell-type Annotation for Single-cell Transcriptomics using Deep Learning with a Weighted Graph Neural Network
Recent advance in single-cell RNA sequencing (scRNA-seq) has enabled large-scale transcriptional characterization of thousands of cells in multiple complex tissues, in which accurate cell type identification becomes the prerequisite and vital step for scRNA-seq studies. Here, we introduce DeepSort, a reference-free cell-type annotation tool for single-cell transcriptomics that uses a deep learning model with a weighted graph neural network.

## Install
### Dependencies
- Compatible with Python 3.7 and Pytorch 1.4
- Dependencies can be installed using `pip install -r requirements.txt`

1. Download pretrained model `pretrained.tar.gz` from the release page and execute `tar -xzvf pretrained.tar.gz` for it.
2. Execute `bash setup.sh` to prepare directory for test data.
3. After executing the above commands, your tree should look like this:
```
 |- pretrianed
     |- human
        |- graphs
        |- statistics
        |- models
     |- mouse
        |- graphs
        |- statistics
        |- models
 |- test
     |- human
     |- mouse
 |- models
    |- __init__.py
    |- gnn.py
 |- utils
    |- __init__.py
    |- preprocess.py
 |- run.py
 |- requirements.txt
 |- README.md
```

## Usage

### Prepare test data

1. The file name of test data should be named in this format: **species_TissueNumber_data.csv**. For example, `human_Spleen9887_data.csv` is a data file containing 9887 human spleen cells.
2. The test single-cell transcriptomics csv data file should be normalized with the defalut `LogNormalize` method with `Seurat` (R package), wherein the column represents each cell and the row represent each gene, as shown below.

      |          |Cell 1|Cell 2|Cell 3|...  |
      | :---:    |:---: | :---:| :---:|:---:|
      |__Gene 1__|    0 | 2.4  |  5.0 |...  |
      |__Gene 2__| 0.8  | 1.1  |  4.3 |...  |
      |__Gene 3__|1.8   |    0 |  0   |...  |
      |  ...     |  ... |  ... | ...  |...  |



3. All the test data should be included under the `test` directory. Furthermore, all of the human testing datasets and mouse testing datasets are required to be under `./test/human` and `./test/mouse` respectively.

### Run

To test one data file `human_Lung2064.csv`, you should execute the following command:
```shell script
python run.py --species human --tissue Lung --test_dataset 2064 --gpu 0 --save_dir result --threshold 0
```
- ``--species`` The species of cells, `human` or `mouse`.
- ``--tissue`` The tissue of cells.
- ``--test_dataset`` The dataset to be tested, in other words, as the file naming rule states, it is exactly the number of cells in the data file.
- ``--gpu`` Specify the GPU to use, `-1` for cpu.
- ``--save_dir`` directory in which the annotation result outputs.
- ``--threshold`` The threshold that constitutes the edge in the graph, default is 0.

### Output
For each test dataset, it will output a `.csv` file named as `species_Tissue_Number.csv` under the `--save_dir` directory. For example, output of test dataset `human_Spleen9887_data.csv` is `human_Spleen_9887.csv`

Each line of the output file corresponds to the predictive cell type.