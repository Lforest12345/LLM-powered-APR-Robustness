# Exploring and Lifting the Robustness of LLM-powered Automated Program Repair with Metamorphic Testing

## Datasets:
Two datasets are employed as our test subjects:
* Defects4J
* QuixBugs

## Code:
This repository includes the following code:

* Code for using AST to implement nine kinds of MRs: Located in the `` directory.
* Code for LLM-based code patch generation: Located in the `` directory, comprising code for LLaMA3-8B, LLaMA3-70B, Mistral Large, and CodeGemma-7B models. For GPT-3.5 and Gemini.
* Construction of test cases: Located in the `` directory.
* Code for training a CodeT5-based code editing model aiming at improving code readability: Located in the `` directory, which involves CodeT5-base⋆ and CodeT5-large⋆.

## Quick Start

### Implemention of Nine MRs Based on AST.
Navigate to the `` directory and execute the following code to implemnt MRs:

#### Step 1: 

- Configuration Variables

  Before running the code, please modify the following variables according to your requirements:

  * `input_file`: Specify the file path of the input data, for example, `input.jsonl`.
  * `lan_output_file`: Specify the output file path for language data, for example, `output.jsonl`.
  * `other_output_file`: Specify the output file path for other data, for example, `other_data.jsonl`.
  * `lan`: Specify the programming language being processed, for example, `Java`.

- Running the Code

  Ensure that you have correctly set the variables mentioned above.

  Run the corresponding Python script to perform the task, for example:
  ```python
  $ python filter.py
  ```
#### Step 2: 

- Configuration Variables

  Before running the code, please modify the following variables according to your requirements:
  
  * `batch_size`: Specify the batch size, for example, `128`.
  * `epochs`: Specify the number of training epochs, for example, `10`.
  * `dropout`: Specify the dropout rate, for example, `0.4`.
  * `rnn_hidden`: Specify the size of the RNN hidden layer, for example, `768`.
  * `rnn_layer`: Specify the number of RNN layers, for example, `1`.
  * `class_num`: Specify the number of classes, for example, `4`.
  * `lr`: Specify the learning rate, for example, `0.001`.
  * `train`: Specify the file path of the training data, for example, `./data/archive/train_clean2.csv`.
  * `val`: Specify the file path of the validation data, for example, `./data/archive/val_clean.csv`.
  * `test`: Specify the file path of the testing data, for example, `./data/archive/test_clean.csv`.

- Running the Code

  Ensure that you have correctly set the variables mentioned above. Run the corresponding Python script to perform the task, for example:
  ```python
  $ python whatwhytrain.py
  ```

### Construction of test cases:

#### Construction of base samples

![数据集采样结果](https://github.com/user-attachments/assets/1d25ea25-3347-452b-a844-b967c5dd0890)

#### Construction of test cases

- Configuration Variables

  Before running the code, please modify the following variables according to your requirements:

  * `file`: Specify the file path containing the generated text, for example, `csgptnoexample.jsonl`.
  * `nlp_file_path`: Specify the file path containing the reference text for automated evaluation metrics, for example, `gptjavainnlp.jsonl`.

- Running the Code

  Ensure that you have correctly set the above variables. Run the corresponding Python script to perform the task of calculating automated evaluation metrics, for example:
  ```python
  $ python metrics.py
  ```

### LLM Commit Message Generation:

#### Configuration Variables
Before running the code, please modify the following variables according to the version and type of LLM:
- With Example (with_example):

  * `lan`: Specify the path of the language data file, for example, `java.jsonl`.
  * `output_filenames`: A dictionary specifying the paths of multiple output files, where each output file corresponds to different numbers of examples (1, 3, 5, 10). For example, `pyaddresult_gemini_1nov.jsonl`.
  * `best_file`: Specify the path containing the file with the best examples.
  * `key`: In Gemini's running code, fill in your Gemini API key.
  * `key_list`: For GPT-3.5's running code, a list containing multiple keys can be used. You can fill in a certain number of GPT API keys according to your actual needs.

- Without Example (without_example):

  * `lan`: Specify the path of the language data file, for example, `java.jsonl`.
  * `output_filename`: Specify the output file path, for example, `java_result_7B.jsonl`.
  * `key`: In Gemini's running code, fill in your Gemini API key.
  * `key_list`: For GPT-3.5's running code, you can fill in a certain number of GPT API keys according to your actual needs.

#### Running the Code
Ensure that you have correctly set the variables mentioned above. Run the corresponding Python script to perform the commit message generation task, for example:
```python
$ python GPT_with_example.py
```

### Calculate Automated Evaluation Metrics:

- Configuration Variables

  Before running the code, please modify the following variables according to your requirements:

  * `file`: Specify the file path containing the generated text, for example, `csgptnoexample.jsonl`.
  * `nlp_file_path`: Specify the file path containing the reference text for automated evaluation metrics, for example, `gptjavainnlp.jsonl`.

- Running the Code

  Ensure that you have correctly set the above variables. Run the corresponding Python script to perform the task of calculating automated evaluation metrics, for example:
  ```python
  $ python metrics.py
  ```

### Retrieve the Best Examples:
Enter the `tools` directory and choose the appropriate retrieval tool:

#### Lexical Retrieval of Best Examples
- Configuration Variables

  Before running the code, please modify the following variables according to your requirements:

  * `lan`: Specify the path of the language data file, for example, `java.jsonl`.
  * `train`: Specify the path of the training data file, for example, `javatrain.jsonl`.
  * `output_filename`: Specify the path of the output file, for example, `java_with_best.jsonl`.

- Running the Code

  Ensure that you have correctly set the above variables. Run the corresponding Python script to perform the lexical retrieval task, for example:
  ```python
  $ python lexical_retrieval.py
  ```

#### Semantic Retrieval of Best Examples
Semantic retrieval consists of two steps, vectorization, and finding the best examples:

- Step 1: Vectorization:

  - Configuration Variables
  
    Before running the code, please modify the following variables according to your requirements:

    - `lan`: Specify the path of the language data file, for example, `py1.jsonl`.
    - `output_file`: Specify the path of the output vector data file, for example, `vpy1no.jsonl`.

  - Running the Code
  
  Ensure that you have correctly set the above variables. Run the corresponding Python script to perform the vectorization task, for example:
  ```python
  $ python semantic_retrieval_vectorization.py
  ```

- Step 2: Finding the Best Examples:

  - Configuration Variables
  
    Before running the code, please modify the following variables according to your requirements:
  
    - `vlan`: Specify the path of the language vector data file, for example, `vpy1no.jsonl`.
    - `vtrain`: Specify the path of the training vector data file, for example, `encoded_diffspy2.jsonl`.
    - `output_file`: Specify the path of the output file containing the retrieved best matches, for example, `pybest_no_selectv.jsonl`.
    - `input_file`: Specify the file path being retrieved, for example, `pytrain_no_selectv.jsonl`.

  - Running the Code
  
  Ensure that you have correctly set the above variables. Run the corresponding Python script to perform the semantic retrieval task, for example:
  ```python
  $ python semantic_retrieval_find.py
  ```

## Contribution
Contributions to this project through Pull Requests or Issues are welcome.

## License
This project is licensed under the `MIT` License.
