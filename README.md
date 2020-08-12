# MRL-CQA: MRL-CQA for EMNLP 2020 submission.
## 1. Experiment environment.
 (1). Python = 3.6.4
 (2). PyTorch = 1.1.0
 (3). TensorFlow = 1.7.0
 (4). tensorboardX = 2.0
 (5). ptan = 0.4
 (6). flask = 1.1.1
 (7). requests = 2.22.0
  
 All the materials required for running the KG sever, training the model, and inferring could be found from the data link: ```https://drive.google.com/drive/folders/1_3j6QsrLM2Sbq4e79ZpoBfUfkAUnt8iV?usp=sharing```.
  
## 2. Accessing knowledge graph.
 (1). Assign the IP address and the port number for the KG server.  

 Manually assign the IP address and the port number in the file of the project `MRL-CQA/BFS/server.py`.
 Insert the host address and the post number for your server in the following line of the code:  
 ```app.run(host='**.***.**.**', port=####, use_debugger=True)```

 Manually assign the IP address and the port number in the file of the project `MRL-CQA/S2SRL/SymbolicExecutor/symbolics.py`.
 Insert the host address and the post number for your server in the following three lines of the code in the `symbolics.py`: 
 ```content_json = requests.post("http://**.***.**.**:####/post", json=json_pack).json()```
  
 (2). Run the KG server.
 
 Download the wiki data from the provided data link.
 Uncompress the file `bfs_data.zip` and copy the three pkl files into the folder `MRL-CQA/data/bfs_data`.
 Run file `MRL-CQA/BFS/server.py` to activate the KG server for retrieval: ```python server.py```. 
 
 ## 3. MAML training.
 (1). Load pre-trained model.
 We pre-trained a model based on Reinforcement learning, and further trained our MAML model on the basis of the RL model. 
 We could download and uncompress the RL model `truereward_0.739_29.zip` in the folder `MRL-CQA/data/saves/rl_even_TR_batch8_1%` from the provided data link.
 
 (2). Train the MAML model.
 Furthermore, we need some extra files in folder `MRL-CQA/data/auto_QA_data` for training the model: `share.question` (vocabulary), `CSQA_DENOTATIONS_full_944K.zip` (the file that records the information relevant to all the training questions), `CSQA_result_question_type_944K.json`, `CSQA_result_question_type_count944K.json`, `CSQA_result_question_type_count944k_orderlist.json`, and `944k_rangeDict.json` (the files that are used to retrieve the support sets).
 Also, we have processed the training dataset and thus we need to download the file `RL_train_TR_new_2k.question` from the folder `MRL-CQA/data/auto_QA_data/mask_even_1.0%`.
 
 All we need is to download aforementioned files from the data link and further put them under the corresponding folders in our project. 
 Then in the folder `MRL-CQA/S2SRL`, we run the python file to train the MAML model: ```python train_reptile_maml_true_reward.py```.
 The trained models would be stored in the folder `MRL-CQA/data/saves/maml_reptile`. 
       
