# Scalable Bayesian divergence time estimation with ratio transformations
This repository contains the instructions and files to reproduce the analyses performed in the "Scalable Bayesian divergence time estimation with ratio transformations" paper by Ji et al.

We provide two ways for users to setup software to reproduce our results.
Please kindly refer to the section `Docker image` if you'd like to run BEAST within docker container which is local and should not disturb your other BEAST installations.
Please kindly refer to the section `Global installation` if you'd like to install BEAST and BEAGLE gloably which could potentially interfere with your other BEAST installations.
You could find a separate `Reproducing the analyses` section under each installation method to follow.
### Docker image
We provide a docker image to streamline software setup.
To build the docker image for `x86_64` systems (e.g. Intel/AMD chips), please run
```
cd where_this_repository_is_stored
docker build --platform x86_64 -t beast docker
```
To build the docker image for `arm_64` systems (e.g. Apple silicon chips), please run
```
cd where_this_repository_is_stored
docker build --platform ARM64 -t beast docker
```

#### Reproducing the analyses

You may use the following commands for each case of the three data sets as described in the manuscript.

Let's open an interactive bash session under docker container.

```
docker run --mount type=bind,src="$(pwd)",target=/tmp -it beast bash
```

To run each of the analysis, please use the corresponding command below in the interactive bash shell.

#### West Nile Virus

* "time" scenario
	* HMC

	```
	beast -load_state xmls/WNV/WNV_skyline_onlyHeights_HMC_save -overwrite xmls/WNV/WNV_Skyline_onlyHeights_HMC.xml
	```

	* Univariable

	```
	beast -load_state xmls/WNV/WNV_skyline_onlyHeights_Serial_save -overwrite xmls/WNV/WNV_skyline_onlyHeight_Univariable.xml
	```

* "rate & time" scenario
	* HMC

	```
	beast -load_state xmls/WNV/WNV_skyline_rateNTime_HMC_save -overwrite xmls/WNV/WNV_Skyline_rateNTime_only_HMC.xml
	```

	* Univariable

	```
	beast -load_state xmls/WNV/WNV_skyline_rateNTime_Serial_save -overwrite xmls/WNV/WNV_skyline_rateNTime_Univariable.xml
	```

#### Rabies virus

* "time" scenario
	* HMC

	```
	beast -load_state xmls/RRV/RABV_Exp_onlyHeights_HMC_save -overwrite xmls/RRV/RacRABV_Exp_onlyHeights_HMC.xml
	```

	* Univariable

	```
	beast -load_state xmls/RRV/RABV_Exp_onlyHeights_Univariable_save -overwrite xmls/RRV/RacRABV_Exp_onlyHeights_Univariable.xml
	```

* "rate & time" scenario
	* HMC

	```
	beast -load_state xmls/RRV/RABV_Exp_HMC_rateNtime_save -overwrite xmls/RRV/RacRABV_Exp_rateNTime_HMC.xml
	```

	* Univariable

	```
	beast -load_state xmls/RRV/RABV_Exp_rateNTime_Univariable_save -overwrite xmls/RRV/RacRABV_Exp_rateNTime_Univariable.xml
	```

#### Lassa Virus

* "time" scenario
	* HMC

	```
	beast -load_state xmls/Lassa/Lassa_skygrid_onlyHeights_HMC_save -overwrite xmls/Lassa/Lassa_S_skygrid_onlyHeights_HMC.xml
	```

	* Univariable

	```
	beast -load_state xmls/Lassa/Lassa_skygrid_onlyHeights_Univariable_save -overwrite xmls/Lassa/Lassa_S_skygrid_onlyHeights_Univariable.xml
	```

* "rate & time" scenario
	* HMC

	```
	beast -load_state xmls/Lassa/Lassa_skygrid_rateNTime_HMC_save -overwrite xmls/Lassa/Lassa_S_skygrid_rateNTime_HMC.xml
	```

	* Univariable

	```
	beast -load_state xmls/Lassa/Lassa_skygrid_rateNTime_Univariable_save -overwrite xmls/Lassa/Lassa_S_skygrid_rateNTime_Univariable.xml
	```


#### Ebola Virus


* HMC

```
	beast -seed 666 -overwrite xmls/EVD/ebov_HMC_all.xml
```

#### Algae

* "time" scenario
	* HMC

	```
	beast -load_state xmls/Algae/algae_hmc_onlyHeight_save -overwrite xmls/Algae/Algae_HMC_onlyHeight.xml
	```

	* Univariable

	```
	beast -load_state xmls/Algae/algae_univariate_onlyHeight_save -overwrite xmls/Algae/Algae_univariate_onlyHeight.xml
	```

* "rate & time" scenario
	* HMC

	```
	beast -load_state xmls/Algae/algae_hmc_rateNTime_save -overwrite xmls/Algae/Algae_HMC_rateNTime.xml
	```

	* Univariable

	```
	beast -load_state xmls/Algae/algae_univariate_rateNTime_save -overwrite xmls/Algae/Algae_univariate_rateNTime.xml
	```



### Global installations

####Setting up BEAGLE
Please follow the [BEAGLE installation instructions](https://github.com/beagle-dev/beagle-lib).
But get the `hmc-clock` branch.

For Mac users, the following commands will compile the CPU version of BEAGLE.
Follow the [instructions](https://github.com/beagle-dev/beagle-lib) if you need to install any other dependent software.

```
xcode-select --install
brew install libtool autoconf automake
git clone -b hmc-clock https://github.com/beagle-dev/beagle-lib.git
git checkout 3bdb30bd645e15983f8c8cf952564813e306ad83
cd beagle-lib
mkdir build
cd build
cmake -DBUILD_CUDA=OFF -DBUILD_OPENCL=OFF ..
sudo make install
```


For Linux users, the commands are similar.

```
sudo apt-get install build-essential autoconf automake libtool git pkg-config openjdk-9-jdk
git clone -b hmc-clock https://github.com/beagle-dev/beagle-lib.git
git checkout 3bdb30bd645e15983f8c8cf952564813e306ad83
cd beagle-lib
mkdir build
cd build
cmake -DBUILD_CUDA=OFF -DBUILD_OPENCL=OFF ..
sudo make install
```


The libraries are installed into `/usr/local/lib`.
You can find them by `ls /usr/local/lib/*beagle*`.


#### Setting up BEAST

The following commands will compile the `hmc-clock` branch of BEAST.

```
git clone -b hmc-clock https://github.com/beast-dev/beast-mcmc.git
cd beast-mcmc
git checkout 17da204e2d9bdadb6c8284fd092413054f161bdc
ant
```

For Mac users, you may need to install ant by `brew install ant` through [Homebrew](https://brew.sh/).

For Linux users, you can install ant by `sudo apt-get install ant`.

This will compile the `jar` files under `beast-mcmc/build/dist/` where you can find `beast.jar`, `beauti.jar` and `trace.jar`.

#### Reproducing the analyses

You may use the following commands for each case of the three data sets as described in the manuscript.

Change your working directory to where you want to store the resulting log files first.

```
cd where_you_want_to_save_results
```

#### West Nile Virus

* "time" scenario
	* HMC

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -load_state where_this_repository_is_stored/xmls/WNV/WNV_skyline_onlyHeights_HMC_save -overwrite where_this_repository_is_stored/xmls/WNV/WNV_Skyline_onlyHeights_HMC.xml
	```

	* Univariable

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -load_state where_this_repository_is_stored/xmls/WNV/WNV_skyline_onlyHeights_Serial_save -overwrite where_this_repository_is_stored/xmls/WNV/WNV_skyline_onlyHeight_Univariable.xml
	```

* "rate & time" scenario
	* HMC

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -load_state where_this_repository_is_stored/xmls/WNV/WNV_skyline_rateNTime_HMC_save -overwrite where_this_repository_is_stored/xmls/WNV/WNV_Skyline_rateNTime_only_HMC.xml
	```

	* Univariable

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -load_state where_this_repository_is_stored/xmls/WNV/WNV_skyline_rateNTime_Serial_save -overwrite where_this_repository_is_stored/xmls/WNV/WNV_skyline_rateNTime_Univariable.xml
	```

#### Rabies virus

* "time" scenario
	* HMC

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -load_state where_this_repository_is_stored/xmls/RRV/RABV_Exp_onlyHeights_HMC_save -overwrite where_this_repository_is_stored/xmls/RRV/RacRABV_Exp_onlyHeights_HMC.xml
	```

	* Univariable

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -load_state where_this_repository_is_stored/xmls/RRV/RABV_Exp_onlyHeights_Univariable_save -overwrite where_this_repository_is_stored/xmls/RRV/RacRABV_Exp_onlyHeights_Univariable.xml
	```

* "rate & time" scenario
	* HMC

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -load_state where_this_repository_is_stored/xmls/RRV/RABV_Exp_HMC_rateNtime_save -overwrite where_this_repository_is_stored/xmls/RRV/RacRABV_Exp_rateNTime_HMC.xml
	```

	* Univariable

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -load_state where_this_repository_is_stored/xmls/RRV/RABV_Exp_rateNTime_Univariable_save -overwrite where_this_repository_is_stored/xmls/RRV/RacRABV_Exp_rateNTime_Univariable.xml
	```

#### Lassa Virus

* "time" scenario
	* HMC

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -load_state where_this_repository_is_stored/xmls/Lassa/Lassa_skygrid_onlyHeights_HMC_save -overwrite where_this_repository_is_stored/xmls/Lassa/Lassa_S_skygrid_onlyHeights_HMC.xml
	```

	* Univariable

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -load_state where_this_repository_is_stored/xmls/Lassa/Lassa_skygrid_onlyHeights_Univariable_save -overwrite where_this_repository_is_stored/xmls/Lassa/Lassa_S_skygrid_onlyHeights_Univariable.xml
	```

* "rate & time" scenario
	* HMC

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -load_state where_this_repository_is_stored/xmls/Lassa/Lassa_skygrid_rateNTime_HMC_save -overwrite where_this_repository_is_stored/xmls/Lassa/Lassa_S_skygrid_rateNTime_HMC.xml
	```

	* Univariable

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -load_state where_this_repository_is_stored/xmls/Lassa/Lassa_skygrid_rateNTime_Univariable_save -overwrite where_this_repository_is_stored/xmls/Lassa/Lassa_S_skygrid_rateNTime_Univariable.xml
	```


#### Ebola Virus


* HMC

```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -seed 666 -overwrite where_this_repository_is_stored/xmls/EVD/ebov_HMC_all.xml
```


#### Algae

* "time" scenario
	* HMC

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -load_state where_this_repository_is_stored/xmls/Algae/algae_hmc_onlyHeight_save -overwrite where_this_repository_is_stored/xmls/Algae/Algae_HMC_onlyHeight.xml
	```

	* Univariable

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -load_state where_this_repository_is_stored/xmls/Algae/algae_univariate_onlyHeight_save -overwrite where_this_repository_is_stored/xmls/Algae/Algae_univariate_onlyHeight.xml
	```

* "rate & time" scenario
	* HMC

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -load_state where_this_repository_is_stored/xmls/Algae/algae_hmc_rateNTime_save -overwrite where_this_repository_is_stored/xmls/Algae/Algae_HMC_rateNTime.xml
	```

	* Univariable

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -load_state where_this_repository_is_stored/xmls/Algae/algae_univariate_rateNTime_save -overwrite where_this_repository_is_stored/xmls/Algae/Algae_univariate_rateNTime.xml
	```
