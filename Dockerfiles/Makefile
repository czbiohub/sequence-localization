build:
	nvidia-docker build -t sequence-localization .

run_gpu0:
	# Ports 8800 and 6600
	sudo NV_GPU=0
	run_jupyter

run_gpu1:
	# Ports 8801 and 6601
	sudo NV_GPU=1
	run_jupyter

run_gpu2:
	# Ports 8802 and 6602
	sudo NV_GPU=2
	run_jupyter

run_gpu3:
	# Ports 8803 and 6603
	export NV_GPU=3
	run_jupyter

run_jupyter:
	echo "Using port 880${NV_GPU} for Jupyter and 660${NV_GPU} for Tensorboard"
	sudo nvidia-docker run -it -p 880${NV_GPU}:880${NV_GPU} -p 660${NV_GPU}:660${NV_GPU} -v $HOME:/src/myhome sequence-localization jupyter notebook --allow-root --port 880$NV_GPU --ip=0.0.0.0
