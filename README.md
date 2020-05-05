# DeepSort

### Reference-free Cell-type Annotation for Single-cell Transcriptomics using Deep Learning with a Weighted Graph Neural Network
Recent advance in single-cell RNA sequencing (scRNA-seq) has enabled large-scale transcriptional characterization of thousands of cells in multiple complex tissues, in which accurate cell type identification becomes the prerequisite and vital step for scRNA-seq studies. Here, we introduce DeepSort, a reference-free cell-type annotation tool for single-cell transcriptomics that uses a deep learning model with a weighted graph neural network.

## Install
### Dependencies
- Compatible with Python 3.7 and Pytorch 1.4
- Dependencies can be installed using `pip install -r requirements.txt`

1. Download pretrained model `pretrained.tar.gz` from the release page and execute `tar -xzvf pretrained.tar.gz` for it.
2. After executing the above commands, your tree should look like this:
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

1. The file name of test data should be named in this format: **species_TissueNumber_data.csv**. For example, `human_Pancreas11_data.csv` is a data file containing 11 human pancreas cells.
2. The test single-cell transcriptomics csv data file should be normalized with the defalut `LogNormalize` method with `Seurat` (R package), wherein the column represents each cell and the row represent each gene, as shown below.

      |          |Cell 1|Cell 2|Cell 3|...  |
      | :---:    |:---: | :---:| :---:|:---:|
      |__Gene 1__|    0 | 2.4  |  5.0 |...  |
      |__Gene 2__| 0.8  | 1.1  |  4.3 |...  |
      |__Gene 3__|1.8   |    0 |  0   |...  |
      |  ...     |  ... |  ... | ...  |...  |



3. All the test data should be included under the `test` directory. Furthermore, all of the human testing datasets and mouse testing datasets are required to be under `./test/human` and `./test/mouse` respectively.

### Run

To test one data file `human_Pancreas11.csv`, you should execute the following command:
```shell script
python run.py --species human --tissue Pancreas --test_dataset 11 --gpu -1 --threshold 0
```
- ``--species`` The species of cells, `human` or `mouse`.
- ``--tissue`` The tissue of cells.
- ``--test_dataset`` The dataset to be tested, in other words, as the file naming rule states, it is exactly the number of cells in the data file.
- ``--gpu`` Specify the GPU to use, `-1` for cpu.
- ``--threshold`` The threshold that constitutes the edge in the graph, default is 0.

### Output
For each test dataset, it will output a `.csv` file named as `species_Tissue_Number.csv` under the `result` directory. For example, output of test dataset `human_Pancreas11_data.csv` is `human_Pancreas_11.csv`

Each line of the output file corresponds to the predictive cell type.

## Examples
```
python run.py --species human --tissue Pancreas --test_dataset 11 --gpu -1 --threshold 0
```

```
python run.py --species mouse --tissue Intestine --test_dataset 28 --gpu -1 --threshold 0
```