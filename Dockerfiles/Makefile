build:
	nvidia-docker build -t sequence-localization .

run_jupyter:
	# Make sure that NV_GPU variable is set.
	# Usage: $ NV_GPU=0 run_jupyter
	echo "Using port 880${NV_GPU} for Jupyter and 660${NV_GPU} for Tensorboard"
	sudo nvidia-docker run -it -p 880${NV_GPU}:880${NV_GPU} -p 660${NV_GPU}:660${NV_GPU} -v ${HOME}:/src/myhome sequence-localization jupyter notebook --allow-root --port 880${NV_GPU} --ip=0.0.0.0
