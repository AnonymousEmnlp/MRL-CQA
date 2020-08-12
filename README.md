# MRL-CQA: MRL-CQA for EMNLP 2020 submission.
## 1. Experiment environment
 (1). 
## 2. Accessing knowledge graph.
 (1). Assign the IP address and the port number for the KG server.  

     Manually assign the IP address and the port number in the file of the project 'MRL-CQA/BFS/server.py'.
     Insert the host address and the post number for your server in the following line of the code:  
     app.run(host='**.***.**.**', port=####, use_debugger=True)
    
     Manually assign the IP address and the port number in the file of the project 'MRL-CQA/S2SRL/SymbolicExecutor/symbolics.py'.
     Insert the host address and the post number for your server in the following three lines of the code in the 'symbolics.py': 
     content_json = requests.post("http://**.***.**.**:####/post", json=json_pack).json()
  
 (2). Run the KG server.
 
    Download the wiki data from the link: .
    Uncompress the file 'bfs_data.zip' and copy the three pkl files into the folder 'MRL-CQA/data/bfs_data'.
    Run file 'MRL-CQA/BFS/server.py' to activate the KG server for retrieval: python server.py 
