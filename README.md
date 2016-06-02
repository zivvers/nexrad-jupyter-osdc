# JUPYTER NOTEBOOK TO RUN *"DE-BUGGING" NEXRAD LEVEL 2 PLOTTING* tutorial

*"DE-BUGGING" NEXRAD LEVEL 2 PLOTTING* is a simple introduction to plotting and animating NEXRAD LEVEL 2 data. It introduces the Griffin user the index service to download NEXRAD data. It demonstrates radar reflectivity plotting using the Python module pyart, and some simple filtering techniques that can be used to clean the NEXRAD L2 data. Finally, this tutorial uses some clever HTML manipulations to allow the user to produce weather animations within the Jupyter Notebook.  

### SECTION I: How to configure vanilla Ubuntu VM for this tutorial

*NOTE: the Griffin public image is already configured & ready to go*
	
1. Add Griffin proxies as variables in .bashrc

	```
	printf "export no_proxy='griffin-objstore.opensciencedatacloud.org'\nfunction with_proxy() {\n\tPROXY='http://cloud-proxy:3128'\n\thttp_proxy='\${PROXY}' https_proxy='\${PROXY}' \$@\n}" >> ~/.bashrc
	```
2. Source .bashrc

	```
	. .bashrc
	```

3. Install Miniconda

	```
	with_proxy wget https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh
	bash Miniconda2-latest-Linux-x86_64.sh -b 
	```

4. Install requisite Python modules

	```
	with_proxy conda update conda
	with_proxy conda install -c https://conda.binstar.org/jjhelmus pyart
	with_proxy conda install jupyter
	with_proxy conda install basemap
	```

5. Install tools for visualizations

	```
	with_proxy sudo -E apt-get update
	with_proxy sudo -E apt-get install -y libav-tools 
	with_proxy sudo -E apt-get install -y python-qt4
	```

### SECTION II: How to run Jupyter Notebook

1. *From your Local Machine (e.g. your laptop)*:add following lines to ~/.ssh (replacing \<OSDC username\> with your OSDC username)

    ```
	Host 172.17.*
	User ubuntu
	ProxyCommand ssh <OSDC username>@griffin.opensciencedatacloud.org nc %h %p 2> /dev/null
    ```

2. *From Griffin VM*

    ```
    	jupyter notebook --no-browser
    ```

3. *From your Local Machine*

    ```
	ssh -L <local port>:localhost:<griffin port> -N <Griffin VM IP Address>
    ```
	replacing \<griffin port\> with \<port\> given in `jupyter notebook --no-browser` output as
	
	> The Jupyter Notebook is running at: http://localhost:<port>/	

4. *From your Local Machine*

    open browser, enter `http://localhost:<local port>`


### SECTION III: What are these directories and files?

- `miniconda`: from Miniconda installation, ignore
- `nexrad`: working directory for this tutorial
    - `mayfly_arks.txt`: file containing ark IDs for relevant NEXRAD L2 data
    - `mayfly_data`: directory into which we will download NEXRAD L2 data
    - `nexrad_display_id_service.ipynb`: Jupter Notebook for tutorial
    - `plots`: directory into which we will save weather radar plots
    - `README.md`: file you are currently viewing
