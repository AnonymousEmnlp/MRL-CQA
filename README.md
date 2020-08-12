# MRL-CQA: MRL-CQA for EMNLP 2020 submission.
## 1. Experiment environment.
 (1). Python = 3.6.4
 (2). PyTorch = 1.1.0
 (3). TensorFlow = 1.7.0
 (4). tensorboardX = 2.0
 (5). ptan = 0.4
 (6). flask = 1.1.1
 (7). requests = 2.22.0
  
 All the materials required for running the KG sever, training the model, and inferring could be found from the data link: `https://drive.google.com/drive/folders/1_3j6QsrLM2Sbq4e79ZpoBfUfkAUnt8iV?usp=sharing`.
  
## 2. Accessing knowledge graph.
 (1). Assign the IP address and the port number for the KG server.  

 Manually assign the IP address and the port number in the file of the project `MRL-CQA/BFS/server.py`.
 Insert the host address and the post number for your server in the following line of the code:  
 ```
 app.run(host='**.***.**.**', port=####, use_debugger=True)
 ```

 Manually assign the IP address and the port number in the file of the project `MRL-CQA/S2SRL/SymbolicExecutor/symbolics.py`.
 Insert the host address and the post number for your server in the following three lines of the code in the `symbolics.py`: 
 ```
 content_json = requests.post("http://**.***.**.**:####/post", json=json_pack).json()
 ```
  
 (2). Run the KG server.
 Download the bfs data `bfs_data.zip` from the provided data link. 
 We need to uncompress the file `bfs_data.zip` and copy the three pkl files into the folder `MRL-CQA/data/bfs_data`. 
 Run file `MRL-CQA/BFS/server.py` to activate the KG server for retrieval: 
 ```
 python server.py
 ``` 
 
 ## 3. MAML training.
 (1). Load pre-trained model.
 We pre-trained a model based on Reinforcement learning, and further trained our MAML model on the basis of the RL model. 
 We could download and uncompress the RL model `truereward_0.739_29.zip` in the folder `MRL-CQA/data/saves/rl_even_TR_batch8_1%` from the provided data link.
 
 (2). Train the MAML model.
 Furthermore, we need some extra files in folder `MRL-CQA/data/auto_QA_data` for training the model: `share.question` (vocabulary), `CSQA_DENOTATIONS_full_944K.zip` (the file that records the information relevant to all the training questions), `CSQA_result_question_type_944K.json`, `CSQA_result_question_type_count944K.json`, `CSQA_result_question_type_count944k_orderlist.json`, and `944k_rangeDict.json` (the files that are used to retrieve the support sets).
 Also, we have processed the training dataset and thus we need to download the file `RL_train_TR_new_2k.question` from the folder `MRL-CQA/data/auto_QA_data/mask_even_1.0%`.
 
 All we need is to download aforementioned files from the data link and further put them under the corresponding folders in our project. 
 Then in the folder `MRL-CQA/S2SRL`, we run the python file to train the MAML model: 
 ```
 python train_reptile_maml_true_reward.py
 ```
 The trained models would be stored in the folder `MRL-CQA/data/saves/maml_reptile`. 
 
 ## 4. MAML testing.
  (1). Load trained model.
  The trained models are stored in the folder `MRL-CQA/data/saves/maml_reptile`.
  We also saved a trained model `epoch_020_0.784_0.741.zip` in this folder, which could lead to the SOTA result.
  We could download the model from the data link.
  When testing the model, we could choose a best model from all the saved models, or simply use the `epoch_020_0.784_0.741` model.
  
  (2). Load the testing dataset.
  We also processed the testing dataset `SAMPLE_FINAL_MAML_test.question` (which is 1/20 of the full testing dataset) and `FINAL_MAML_test.question` (which is the full testing dataset), and saved them in the folder `MRL-CQA/data/auto_QA_data/mask_test`.
  We could download the files from the data link and put them under the folder `MRL-CQA/data/auto_QA_data/mask_test` in the project.
  
  (3). Testing.
  In the file `MRL-CQA/S2SRL/data_test_maml.py`, we could change the parameter following to meed our requirement.
  In the command line: 
  ```
  sys.argv = ['data_test_maml.py', '-m=epoch_020_0.784_0.741.dat', '-p=sample_final_maml',
  '--n=maml_reptile', '--cuda', '-s=5', '-a=0', '--att=0', '--lstm=1',
  '--fast-lr=1e-4', '--meta-lr=1e-4', '--steps=5', '--batches=1', '--weak=1', '--embed-grad']
  ```
  , we could change the following settings.
  If we want to use the sample testing dataset to get an approximation testing result, we set `-p=sample_final_maml`, or we could set `-p=final_maml` to infer all the testing questions.
  If we want to use the models stored in the named folder `MRL-CQA/data/saves/maml_reptile`, we set `--n=maml_reptile`.
  If we want to use our saved model `epoch_020_0.784_0.741` in the named folder to test the questions, we set `-m=epoch_020_0.784_0.741.dat`.
  
  After setting, we run the file `MRL-CQA/S2SRL/data_test_maml.py` to generate the action sequence for each testing question:
  ```
  python data_test_maml.py
  ```
  We could find the generated action sequences in the folder where the model is in (for instance `MRL-CQA/data/saves/maml_reptile`), which is stored in the file `final_maml_predict.actions` or `sample_final_maml_predict.actions`. 
  
  (4). Calculating the result.
  After generating the actions, we could use them to compute the QA result.
  For example, we use the saved model `MRL-CQA/data/saves/maml_reptile/epoch_020_0.784_0.741.dat`, and therefore generate a file `MRL-CQA/data/saves/maml_reptile/final_maml_predict.actions` to record the generated actions for the testing questions.
  Then in file `MRL-CQA/S2SRL/SymbolicExecutor/calculate_sample_test_dataset.py`, we set the parameters.
  In the function `transMask2ActionMAML()`, we have a line of the code: 
  ```
 with open(path, 'r') as load_f, open("../../data/saves/maml_reptile/sample_final_maml_predict.actions", 'r') as predict_actions:
  ```
 , which is used to compute the accuracy of the actions stored in the file `MRL-CQA/data/saves/maml_reptile/final_maml_predict.actions`.
 We could change the path of the generated file in the above line of the code.
 Then we run the file `MRL-CQA/S2SRL/SymbolicExecutor/calculate_sample_test_dataset.py` to compute the final result:
 ```
 python calculate_sample_test_dataset.py
 ```
 The result will be stored in the file `MRL-CQA/data/auto_QA_data/test_result/maml_reptile.txt`.
