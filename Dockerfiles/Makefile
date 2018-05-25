build:
	nvidia-docker build -t sequence-localization .

run:
	sudo NV_GPU=2 nvidia-docker run -it -p 7781:7781 -p 6006:6006 -v $HOME:/src/myhome sequence-localization jupyter notebook --allow-root --port 7781 --ip=0.0.0.0