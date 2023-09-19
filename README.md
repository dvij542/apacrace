# Adaptive Planning and Control with Time-Varying Tire Models for Autonomous Racing Using Extreme Learning Machine

## Table of contents
- [Overview](#overview)
- [Hardware specifications](#hardware-specifications)
- [High Quality submission video](#hardware-specifications) 
- [Numerical simulation experiments](#numerical-simulation-experiments)
  - [Tracks](#tracks)   
    - ETHZ  
    - ETHZMobil
  - [Offline validation](#offline-validation)
  - [Results on ETHZ track with constant friction decline](#results-on-ethz-track-with-constant-friction-decline)
  - [Results on ETHZMobil track with constant friction decline](#results-on-ethzmobil-track-with-constant-friction-decline)
  - [Results on ETHZ track with sudden friction decline](#results-on-ethz-track-with-sudden-friction-decline) 
  - [Results on ETHZ track with different friction decline rates](#results-on-ethz-track-with-sudden-friction-decline)
- [Carla simulation experiment](#carla-simulation-experiment)
  - [Offline validation](#offline-validation)
  - [Online run with constant friction decay](#online-run-with-constant-friction-decay)
- [RC Car experiments](#rc-car-experiments)
  - [Experiment Setup](#experiment-setup)
  - [Results on Oval track](#results-on-oval-track)
  - [Results on Diamond track](#results-on-diamond-track)

## Overview 

![overview](https://github.com/dvij542/apacrace/assets/43860166/8c1f713e-0d0e-40d2-b97b-3cd658a53c7d)

## Hardware specifications

All experiments have been performed on AMD Ryzen 7 5000 series CPU with 16 GB RAM. For the RC car experiments, it is equipped with a Lidar, a depth dual camera, an IMU and wheel encoders. We however use the Vicon based localization to get an accurate position and orientation of the vehicle with high accuracy (of about 1 cm and 0.01 rad).

## High Quality submission video

[![Watch the video](https://img.youtube.com/vi/R1idS-p2MLA/maxresdefault.jpg)](https://youtu.be/R1idS-p2MLA)

## Numerical simulation experiments 

We first present all the results from numeric simulation

### Tracks 

The tracks along with the racing lines used for the experiments are as follows :-

1. ETHZ

| ![Track](https://github.com/dvij542/apacrace/assets/43860166/7031dbe5-4ebb-481e-af0a-91de0672177a) |
|:--:| 
| *Track* |

Rendered racelines at :-

<div class="table-wrapper" markdown="block">

| <img src="https://github.com/dvij542/apacrace/assets/43860166/cfbb4a36-e02c-4370-bdcd-914c74f0a686"> |  ![ethz_raceline1](https://github.com/dvij542/apacrace/assets/43860166/5e394135-a344-4da0-8f0b-ee14654c3821) | ![ethz_raceline2](https://github.com/dvij542/apacrace/assets/43860166/4e3e5501-5af8-42f8-a464-7a65efb2faa1) | ![ethz_raceline3](https://github.com/dvij542/apacrace/assets/43860166/c700b49f-5345-48b1-a001-4d8b537515fc) | ![ethz_raceline4](https://github.com/dvij542/apacrace/assets/43860166/6f975e8f-38a4-43fd-9b72-85990c750e01) |
|:--:|:--:|:--:|:--:|:--:| 
| $$\mu=0.6$$ | $$\mu=0.7$$ | $$\mu=0.8$$ | $$\mu=0.9$$ | $$\mu=1.0$$ | 

</div>

2. ETHZMobil

| ![Track](https://github.com/dvij542/apacrace/assets/43860166/6a0714ae-1a0d-44f5-a081-9c773b0a4798) |
|:--:| 
| *Track* |

Rendered racelines at :-

| ![mu=0.6](https://github.com/dvij542/apacrace/assets/43860166/ac77b1b8-6347-4b07-8a5e-b6c97105591a) | ![mu=0.7](https://github.com/dvij542/apacrace/assets/43860166/9cccc787-a517-4095-a7b1-79ab5ed4df14) | ![mu=0.8](https://github.com/dvij542/apacrace/assets/43860166/f0cdaf09-fcec-49ac-ad80-d56a84e246bb) | ![mu=0.9](https://github.com/dvij542/apacrace/assets/43860166/4d547a5a-5efe-49af-8212-e2778d1b792f) | ![mu=1.0](https://github.com/dvij542/apacrace/assets/43860166/977537e3-072b-4ac7-bf0a-8d53345a5f8f) |
|:--:|:--:|:--:|:--:|:--:| 
| $$\mu=0.6$$ | $$\mu=0.7$$ | $$\mu=0.8$$ | $$\mu=0.9$$ | $$\mu=1.0$$ | 

### Offline validation

To demonstrate that our proposed ELM model can really learn from dynamics data and to get an idea of how much data is required to train a reasonable model, we perform this offline validation where we collect training data by running single lap on a ETHZMobil Track with pure pursuit for steering and PID for longitudinal control :-

![track_training](https://github.com/dvij542/apacrace/assets/43860166/612f61d8-b880-492f-9ad6-6503b11f7b97)

We use this data to train ELM models offline and also train GP models as proposed in [BayesRace](https://arxiv.org/pdf/2005.04755.pdf) work for comparison. Then, we make a run with NMPC using actual model parameters to run 1 lap on ETHZ track and use the trained models to predict difference in $vx$, $vy$ and $\omega$ between the actual values and the e-kinematic model. Here are the plots for $vx$, $vy$ and $\omega$ predictions for both ours and BayesRace against GT values.

| ![y_comps_vx](https://github.com/dvij542/apacrace/assets/43860166/c6e343e5-91da-4649-8d43-1d77e779fdf7) | ![y_comps_vy](https://github.com/dvij542/apacrace/assets/43860166/750d3c26-e358-4469-9b17-cb4acd7408a6) | ![y_comps_omega](https://github.com/dvij542/apacrace/assets/43860166/393c9166-c27f-48f1-ae4f-9581afcf2e07) |
|:--:|:--:|:--:| 
| vx | vy | $\omega$ |  

As can be seen the predictions from both GP (from BayesRace) and our proposed ELM model are nearly close to the GT with training data used from only 1 lap of training data with limiting speeds (Note that the predictions are not very close if the training data is collected at low speeds with little lateral slips). This suggests that our proposed ELM is indeed as good as (if not worse) using GP for predicting difference between actual and e-kinematic model of the vehicle. But the obvious advantage of using our ELM is that it's can be trained in an adaptive manner and with very less computation time allowing it to update online while GP's training time is huge and it scales with more training data. One way to get rid of increaing computation on data size is to use only last few samples for training but it needs to be trained each time for each cycle due to which it contributes larger computation time for each cycle. Also, ELM has significantly less non-linearity which allows us to use it in building a real-time NMPC controller with only around 0.05 $\pm$ 0.02 s computation time while GP takes about 0.3s $\pm$ 0.05s practically on our machine when used online (and even offline it takes 0.25s on avg as also reported by BayesRace) with IPOPT optimization. Also, with estimated tire curve from ELM, we can even estimate the tire friction coefficient by taking the max value of the prediction while for GP we need to use an estimator separately.

### Results on ETHZ track with constant friction decline 

This is to simulate wear and tear of the tires. We reduce the friction coefficients $$D_f$$ and $$D_r$$ according to the following plot :-

| ![max_friction_forces](https://github.com/dvij542/apacrace/assets/43860166/683361dd-918f-4b4e-aeda-0cfd74263175) |
|:--:| 
| time vs $D_f/D_r$ variation |

The resultant trajectory, speed and $$\mu$$ (used to render raceline reference speeds) plots for different algorithms are as follows :-

| Method | Trajectory | Speeds | $$\mu$$ used for raceline |
|:--:|:--:|:--:|:--:|
| Without any adaptation | ![track_run1](https://github.com/dvij542/apacrace/assets/43860166/5926a316-9ec5-442e-9343-a19ef6719039) | ![speeds(5)](https://github.com/dvij542/apacrace/assets/43860166/6d3857e5-4e89-403a-9d3f-65d4a88f72b9) | Same as beginning ($$\mu=1.0$$) |
| With only model adaptation | ![track_run1(2)](https://github.com/dvij542/apacrace/assets/43860166/5e3267ed-e14e-4af3-bba1-13ede6c88f8b) |  ![speeds(4)](https://github.com/dvij542/apacrace/assets/43860166/ebd866d6-4d0d-4e92-a304-d34d24512656) | Same as beginning ($$\mu=1.0$$) |
| With model + reference speed adaptations (ours) | ![track_run1(1)](https://github.com/dvij542/apacrace/assets/43860166/ec311b82-e156-4664-ada1-8f652fbaae1d) | ![speeds(2)](https://github.com/dvij542/apacrace/assets/43860166/2de032d9-f364-4c2e-87ee-45854aae2b39) | ![mus](https://github.com/dvij542/apacrace/assets/43860166/cc0e9485-c315-4853-a370-8302c1a570a6) |
| With GP for model difference | ![track_run1(3)](https://github.com/dvij542/apacrace/assets/43860166/bb7206f9-cc1a-4cc8-8c01-199340e177b9) | ![speeds(1)](https://github.com/dvij542/apacrace/assets/43860166/3c84b375-fd17-4539-899e-b9a398fdb461) | Same as GT |
| Oracle | ![track_run1(4)](https://github.com/dvij542/apacrace/assets/43860166/b05c9edc-7c32-4138-a641-c614d925a079) | ![speeds](https://github.com/dvij542/apacrace/assets/43860166/3550dfa0-ec21-4dc6-bb68-7ad560b71019) | Same as GT |

Here is the video comparison with 3 runs (without adaptation, with only model adaptation, with model + reference speed adaptation) in parallel :-
[![Watch the video](https://img.youtube.com/vi/eNJlvr7D7q0/maxresdefault.jpg)](https://youtu.be/eNJlvr7D7q0)

### Results on ETHZMobil track with constant friction decline 

We reduce the friction coefficients $$D_f$$ and $$D_r$$ according to the following plot :-

| ![max_friction_forces](https://github.com/dvij542/apacrace/assets/43860166/75e8377f-8fad-4d5f-8bd2-a65e51c1ed86) |
|:--:| 
| time vs $$D_f/D_r$$ variation |

The resultant trajectory, speed and $$\mu$$ (used to render raceline reference speeds) plots for different algorithms are as follows :-

| Method | Trajectory | Speeds | $$\mu$$ used for raceline |
|:--:|:--:|:--:|:--:|
| Without any adaptation | ![traj_withoutmobil](https://github.com/dvij542/apacrace/assets/43860166/7a71e6f2-914c-4478-93df-513d1907d9cb) | ![speeds](https://github.com/dvij542/apacrace/assets/43860166/a3cc0ccb-5137-4e01-b38d-220011f605e5) | Same as beginning ($$\mu=1.0$$) |
| With only model adaptation | ![traj_withconstmobil](https://github.com/dvij542/apacrace/assets/43860166/129abd2d-b720-4691-b03f-13203913490b) | ![speeds](https://github.com/dvij542/apacrace/assets/43860166/f92ae393-c9bf-4038-ae92-6d16fe24b5ba) | Same as beginning ($$\mu=1.0$$) |
| With model + reference speed adaptations (ours) | ![traj_withmobil](https://github.com/dvij542/apacrace/assets/43860166/7c90a09a-9473-4efd-8e7a-965f11fea833) | ![speeds](https://github.com/dvij542/apacrace/assets/43860166/d8d6f7dd-2d25-4be1-bd98-dc0891a9a053) |  ![mus](https://github.com/dvij542/apacrace/assets/43860166/8e076481-5d96-4942-bccb-633591ab1367) |
| Oracle | ![traj_oraclemobil](https://github.com/dvij542/apacrace/assets/43860166/3384369e-55d1-4e38-a0ff-c824b5036675) |  ![speeds_oracle](https://github.com/dvij542/apacrace/assets/43860166/97e7b0a6-6fa8-49cb-aa04-c88b1bae241a) | Same as GT |

### Results on ETHZ track with sudden friction decline

Next, to test how our method would perform if there is a suddent drop in maximum friction parameter which can be caused for example by sudden rain/change in weather condition etc., we suddenly drop the friction coefficient by about 30\% at a given time according to the given plot :-

| ![max_friction_forces](https://github.com/dvij542/apacrace/assets/43860166/508a9e8a-40d2-469d-8a42-8357bd334613) |
|:--:|
| time vs $$D_f/D_r$$ variation |

| Method | Trajectory | Speeds | $$\mu$$ used for raceline |
|:--:|:--:|:--:|:--:|
| Without any adaptation | ![track_run1(1)](https://github.com/dvij542/apacrace/assets/43860166/79053d1c-2b6d-4e32-96fb-ae9d5abd9b77) | ![speeds(1)](https://github.com/dvij542/apacrace/assets/43860166/f892b908-8348-4831-b715-2bcc8c4cee96) | Same as beginning ($$\mu=1.0$$) |
| With model + reference speed adaptations (ours) | ![track_run1](https://github.com/dvij542/apacrace/assets/43860166/d78894c3-ef25-4b88-a73f-f2c191b10739) | ![speeds](https://github.com/dvij542/apacrace/assets/43860166/3d7ad598-8ebf-4e2b-83c2-8a7108ed081a) |  ![mus](https://github.com/dvij542/apacrace/assets/43860166/8e076481-5d96-4942-bccb-633591ab1367) |

### Results on ETHZ track with different friction decline rates

## Carla simulation experiment 

To demonstrate our algorithm working on a high fidelity simulator in realtime with full sized car, we deploy it on Carla simulator. Deploying on a simulator gives us additional confidence in terms of the performance guarantee in presence of sensor and model noise. We use the following trackmap (Town07) :- 

| ![image](https://github.com/dvij542/apacrace/assets/43860166/acdf0340-1181-4662-a515-9cb49befdd74) |
|:--:|
| Town07 map |

We use Audi TT car in Carla as follows

| ![carla_car](https://github.com/dvij542/apacrace/assets/43860166/bddfa68c-d7cd-48be-b9c2-258c3d2d3429) |
|:--:|
| Car used for simulation |

We start with the default model parameters as defined in Carla and reduce the tire_friction parameter (which is the coefficient of friction of the tire) of the vehicle linearly as described below :-

### Offline validation

For offline validation, we check the lateral and longitudinal forces from the trained model on one lap of data and compare them to the lateral and longitudinal force values obtained from the simulator. As shown in Fig. \ref{fig:offline_val_carla}, although these values are noisy, our trained model archives high fitting accuracy. This is to ensure that the fitted tire curves from offline collected data just from the trajectory data actually confirms with the actual force recorded by carla simulator (available from privileged forces on tyres from carla). We also present the difference in vx, vy and $\omega$ wrt the e-kinematic model.

| ![slips_forces](https://github.com/dvij542/apacrace/assets/43860166/6abab9f5-fe29-4e3b-8534-9995ea6b3e6c) |  
|:--:| 
| slip vs force (Predicted (line) vs recorded (scatter)) |  

### Online run with constant friction decay

For the online run, we set $\alpha=0.002, \gamma=0.9, \mu_{start} = 1.0, K_{batch} = 2000, t_{th}=44s, n_h=40, N=50, Q=\begin{bmatrix} 0.1 & 0 \\ 0 & 0.1 \end{bmatrix}, R = \begin{bmatrix} 0.005 & 0 \\ 0 & 1 \end{bmatrix}$. The run plots for without adaptation are as follows :-

| Trajectory | ![traj_without](https://github.com/dvij542/apacrace/assets/43860166/2e1116aa-8516-44d2-98ad-efd72629f48b) |
|:--:|:--:| 
| Speeds | ![speeds_carla_without](https://github.com/dvij542/apacrace/assets/43860166/22a26ea6-1f2e-49d5-ab02-973c24ab8948) |
| $\mu$ taken to get raceline | Same as at the beginning ($\mu=1$) |

As can be seen from the speed plot, the car crashes at about 120s. The plots for with adaptation are as follows :-

| Trajectory |![traj_with](https://github.com/dvij542/apacrace/assets/43860166/46e18952-baaf-404e-a99d-900f919c1076) |
|:--:|:--:| 
| Speeds | ![speeds_carla_with](https://github.com/dvij542/apacrace/assets/43860166/6f74fc65-ea96-4ed3-ad96-95c7e399381d) |
| $\mu$ taken to get raceline | ![mus_carla](https://github.com/dvij542/apacrace/assets/43860166/b573bed2-c370-41ab-8ccf-edcc8cd49c13) |

The trajectory plots may not be visible clearly due to large track size. The video of the runs help to observe the results more clearly :-

[![Watch the video](https://img.youtube.com/vi/LJGpZviw_Eg/maxresdefault.jpg)](https://youtu.be/LJGpZviw_Eg)

## RC Car experiments
Finally, we test our algorithm on a real RC car to demonstrate working on a real system with real sensor, model noises and that we can implement in realtime. The car is equipped with a Lidar, a depth dual camera, an IMU and wheel encoders. 

| ![rc_car](https://github.com/dvij542/apacrace/assets/43860166/6639e87d-4154-4622-a585-161b65afd132) |
|:--:|
| RC car hardware |

Despite the onboard computation available, we use a windows machine with 16 GB RAM, AMD Ryzen 5000 processor for computation which also runs the Vicon tracker software for localization and communicate the commands directly to the vehicle

![Screenshot from 2023-09-16 02-19-06](https://github.com/dvij542/apacrace/assets/43860166/775259ca-940f-40a1-815b-800c7bfdff04)

### Experiment Setup

We perform the experiment on 2 track as shown below. The lane width is about 0.5 m. The reference line is taken to be the centerline with limiting speeds rendered according to \cite{doi:10.1080/00423114.2019.1631455}. The speeds are adjusted according to the estimated friction coefficient. We conduct the experiment by starting a friction coefficient equal to $1.5$ and incrementally increase target speeds from $50\%$ of the reference speeds to $100\%$ of the reference speeds from $t=10s$ to $t=35s$. This is to ensure physical safety and also allow sufficient time and data for the adaptation algorithm to estimate the friction and aerodynamic parameters. 

### Results on Oval track

| ![rounded_rect_ref](https://github.com/dvij542/apacrace/assets/43860166/96f3e4d1-fb6c-41f0-a86f-c45e9c582204) | ![rounded_diamond_ref](https://github.com/dvij542/apacrace/assets/43860166/b931b094-3c2a-4b5c-9cb8-734be118155e) |
|:--:|:--:| 
| Oval track | Diamond track | 

The trajectories followed if adaptation is used vs not used are as follows :-

| Method | Trajectory | $$\mu$$ used for raceline |
|:--:|:--:|:--:|
| Without adaptation | ![path](https://github.com/dvij542/apacrace/assets/43860166/5e4a9cab-4fd0-4e9b-9286-5c0657fdfe25) | Same as beginning ($$\mu=1.0$$) |
| With model + reference speed adaptations (ours) | ![path(1)](https://github.com/dvij542/apacrace/assets/43860166/decd881f-99b6-413b-aaca-6636ba3e21ae) | ![mu_preds(1)](https://github.com/dvij542/apacrace/assets/43860166/585115b2-28d4-4bdc-bdd8-a89fed070b64) |

Then, we change the surface by adding wooden planks on the ground as shown below to change the surface :-

![Screenshot from 2023-09-16 02-43-22](https://github.com/dvij542/apacrace/assets/43860166/d19e47a7-8e8a-4ec8-9eb2-d44e81e8a39d)

Starting with the same predicted model and friction coefficient, we again increase the speeds gradually from 0.5 of raceline speeds to 1. The trajectory followed by the car is as follows :-

| Trajectory | $$\mu$$ used for raceline |
|:--:|:--:|
| ![path(2)](https://github.com/dvij542/apacrace/assets/43860166/eea3c937-3908-4106-8e8e-351e9bc01b79) | ![mu_preds(2)](https://github.com/dvij542/apacrace/assets/43860166/51c3f664-b27b-4d7c-8b3f-6b50291f95a6) |

As can be clearly seen, the vehicle does move out of the track at the beginning but is able to use the experience to tune the model and friction coefficient to adjust the next lap accordingly and is able to adapt well to the new surface. The video of the whole run is as below :-

[![Watch the video](https://img.youtube.com/vi/lFokBhqTKo0/maxresdefault.jpg)](https://youtu.be/lFokBhqTKo0)

### Results on Diamond track

