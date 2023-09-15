# Adaptive Planning and Control with Time-Varying Tire Models for Autonomous Racing Using Extreme Learning Machine

## Hardware specifications

All experiments have been performed on AMD Ryzen 7 5000 series CPU with 16 GB RAM. For the RC car experiments, it is equipped with a Lidar, a depth dual camera, an IMU and wheel encoders. We however use the Vicon based localization to get an accurate position and orientation of the vehicle with high accuracy (of about 1 cm and 0.01 rad).

## Numerical simulation experiments 

We first present all the results from numeric simulation

### Tracks 

The tracks along with the racing lines used for the experiments are as follows :-

1. ETHZ

| ![Track](https://github.com/dvij542/apacrace/assets/43860166/7031dbe5-4ebb-481e-af0a-91de0672177a) |
|:--:| 
| *Track* |

Rendered racelines at :-

| ![mu=0.6](https://github.com/dvij542/apacrace/assets/43860166/cfbb4a36-e02c-4370-bdcd-914c74f0a686) | ![mu=0.7](https://github.com/dvij542/apacrace/assets/43860166/eaf8f0d5-c88a-47eb-93a7-22592b26b452) | ![mu=0.8](https://github.com/dvij542/apacrace/assets/43860166/cbb1d89f-941d-48ef-9f60-d9a2e2d82c00) | ![mu=0.9](https://github.com/dvij542/apacrace/assets/43860166/6c9e0cd9-d8fc-45b4-b014-37b9e3d4baec) | ![mu=1.0](https://github.com/dvij542/apacrace/assets/43860166/38291ddb-2e58-413d-ae41-9ca07071b430) |
|:--:|:--:|:--:|:--:|:--:| 
| $\mu=0.6$ | $\mu=0.7$ | $\mu=0.8$ | $\mu=0.9$ | $\mu=1.0$ | 

2. ETHZMobil

| ![Track](https://github.com/dvij542/apacrace/assets/43860166/6a0714ae-1a0d-44f5-a081-9c773b0a4798) |
|:--:| 
| *Track* |

Rendered racelines at :-

| ![mu=0.6](https://github.com/dvij542/apacrace/assets/43860166/ac77b1b8-6347-4b07-8a5e-b6c97105591a) | ![mu=0.7](https://github.com/dvij542/apacrace/assets/43860166/9cccc787-a517-4095-a7b1-79ab5ed4df14) | ![mu=0.8](https://github.com/dvij542/apacrace/assets/43860166/f0cdaf09-fcec-49ac-ad80-d56a84e246bb) | ![mu=0.9](https://github.com/dvij542/apacrace/assets/43860166/4d547a5a-5efe-49af-8212-e2778d1b792f) | ![mu=1.0](https://github.com/dvij542/apacrace/assets/43860166/977537e3-072b-4ac7-bf0a-8d53345a5f8f) |
|:--:|:--:|:--:|:--:|:--:| 
| $mu=0.6$ | $mu=0.7$ | $mu=0.8$ | $mu=0.9$ | $mu=1.0$ | 

### Results on ETHZ track with constant friction decline 

This is to simulate wear and tear of the tires. We reduce the friction coefficients $D_f$ and $D_r$ according to the following plot :-
| ![max_friction_forces](https://github.com/dvij542/apacrace/assets/43860166/683361dd-918f-4b4e-aeda-0cfd74263175) |
|:--:| 
| time vs $D_f/D_r$ variation |

The resultant trajectory, speed and $\mu$ (used to render raceline reference speeds) plots for different algorithms are as follows :-
| Method | Trajectory | Speeds | $\mu$ used for raceline |
|:--:|:--:|:--:|:--:|
| Without any adaptation | ![track_run1](https://github.com/dvij542/apacrace/assets/43860166/5926a316-9ec5-442e-9343-a19ef6719039) | ![speeds(5)](https://github.com/dvij542/apacrace/assets/43860166/6d3857e5-4e89-403a-9d3f-65d4a88f72b9) | Same as beginning ($\mu=1.0$) |
| With only model adaptation | ![track_run1(2)](https://github.com/dvij542/apacrace/assets/43860166/5e3267ed-e14e-4af3-bba1-13ede6c88f8b) |  ![speeds(4)](https://github.com/dvij542/apacrace/assets/43860166/ebd866d6-4d0d-4e92-a304-d34d24512656) | Same as beginning ($\mu=1.0$) |
| With model + reference speed adaptations (ours) | ![track_run1(1)](https://github.com/dvij542/apacrace/assets/43860166/ec311b82-e156-4664-ada1-8f652fbaae1d) | ![speeds(2)](https://github.com/dvij542/apacrace/assets/43860166/2de032d9-f364-4c2e-87ee-45854aae2b39) | ![mus](https://github.com/dvij542/apacrace/assets/43860166/cc0e9485-c315-4853-a370-8302c1a570a6) |
| With GP for model difference | ![track_run1(3)](https://github.com/dvij542/apacrace/assets/43860166/bb7206f9-cc1a-4cc8-8c01-199340e177b9) | ![speeds(1)](https://github.com/dvij542/apacrace/assets/43860166/3c84b375-fd17-4539-899e-b9a398fdb461) | Same as GT |
| Oracle | ![track_run1(4)](https://github.com/dvij542/apacrace/assets/43860166/b05c9edc-7c32-4138-a641-c614d925a079) | ![speeds](https://github.com/dvij542/apacrace/assets/43860166/3550dfa0-ec21-4dc6-bb68-7ad560b71019) | Same as GT |

### Results on ETHZMobil track with constant friction decline 

We reduce the friction coefficients $D_f$ and $D_r$ according to the following plot :-
| ![max_friction_forces](https://github.com/dvij542/apacrace/assets/43860166/75e8377f-8fad-4d5f-8bd2-a65e51c1ed86) |
|:--:| 
| time vs $D_f/D_r$ variation |

The resultant trajectory, speed and $\mu$ (used to render raceline reference speeds) plots for different algorithms are as follows :-
| Method | Trajectory | Speeds | $\mu$ used for raceline |
|:--:|:--:|:--:|:--:|
| Without any adaptation | ![track_run1](https://github.com/dvij542/apacrace/assets/43860166/5926a316-9ec5-442e-9343-a19ef6719039) | ![speeds(5)](https://github.com/dvij542/apacrace/assets/43860166/6d3857e5-4e89-403a-9d3f-65d4a88f72b9) | Same as beginning ($\mu=1.0$) |
| With only model adaptation | ![track_run1(2)](https://github.com/dvij542/apacrace/assets/43860166/5e3267ed-e14e-4af3-bba1-13ede6c88f8b) |  ![speeds(4)](https://github.com/dvij542/apacrace/assets/43860166/ebd866d6-4d0d-4e92-a304-d34d24512656) | Same as beginning ($\mu=1.0$) |
| With model + reference speed adaptations (ours) | ![track_run1(1)](https://github.com/dvij542/apacrace/assets/43860166/ec311b82-e156-4664-ada1-8f652fbaae1d) | ![speeds(2)](https://github.com/dvij542/apacrace/assets/43860166/2de032d9-f364-4c2e-87ee-45854aae2b39) | ![mus](https://github.com/dvij542/apacrace/assets/43860166/cc0e9485-c315-4853-a370-8302c1a570a6) |
| With GP for model difference | ![track_run1(3)](https://github.com/dvij542/apacrace/assets/43860166/bb7206f9-cc1a-4cc8-8c01-199340e177b9) | ![speeds(1)](https://github.com/dvij542/apacrace/assets/43860166/3c84b375-fd17-4539-899e-b9a398fdb461) | Same as GT |
| Oracle | ![track_run1(4)](https://github.com/dvij542/apacrace/assets/43860166/b05c9edc-7c32-4138-a641-c614d925a079) | ![speeds](https://github.com/dvij542/apacrace/assets/43860166/3550dfa0-ec21-4dc6-bb68-7ad560b71019) | Same as GT |
