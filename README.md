# _Explaining the Model, Protecting Your Data_: Revealing and Mitigating the Data Privacy Risks of Post-Hoc Model Explanations via Membership Inference
Code for the paper "_Explaining the Model, Protecting Your Data_: Revealing and Mitigating the Data Privacy Risks of Post-Hoc Model Explanations via Membership Inference"

---

### Abstract
Predictive machine learning models are becoming increasingly deployed in high-stakes contexts involving sensitive personal data; in these contexts, there is a trade-off between model _explainability_ and data _privacy. In this work, we push the boundaries of this trade-off: with a focus on foundation models for image classification fine-tuning, we reveal unforeseen privacy risks of post-hoc model explanations and subsequently offer mitigation strategies for such risks. First, we construct VAR-LRT and L1/L2-LRT, two novel membership inference attacks based on feature attribution explanations that are significantly more successful than existing attacks, particularly in the low false-positive rate regime that allows an adversary to identify specific training set members with confidence. Second, we find empirically that optimized differentially private fine-tuning substantially diminishes the success of the aforementioned attacks, while maintaining high model accuracy. This analysis fills a gap in literature---there is no prior work that thoroughly quantifies the relationship between differential privacy and the subsequent privacy risks of post-hoc explanations in a deep learning setting. We carry out a rigorous empirical analysis with 2 novel attacks, 5 vision transformer architectures, 5 benchmark datasets, 4 state-of-the-art post-hoc explanation methods, and 4 privacy strength settings.

---
### Package Requirements
* Python >= 3.8
* PyTorch >= 2.0
* Captum (`conda install captum -c pytorch` or `conda install captum -c conda-forge`
* Opacus (`conda install -c conda-forge opacus`)

---
### Running Experiments
The scripts, as they are configured, generate attack metrics and (log-scaled and linearly scaled) ROC curves for one singular setting (of dataset, model, explanation type, and attack type). Run the scripts, in the following order, to run the pipeline:
* `sbatch scripts/train_driver.sh` to fine-tune models and save their state dictionaries
* `sbatch scripts/get_explanations_driver.sh` to compute post-hoc explanations and save per-example attack scores: explanation variance, L1 norm, L2 norm
* `python3 run_attack.py` to run the attack and generate metrics + plots

---
### Code Organization
This repository is organized as follows:
* `train.py` is the script for fine-tuning a single vision transformer model and saving its state dictionary.
* `get_explanations.py` is the script that, for a single model, computes post-hoc explanations on all data examples and saves per-example attack scores: explanation variance, L1 norm, L2 norm.
* `run_attack.py` is the script that, for a single attack parameter setting, runs the attack and generates metrics + plots
* `scripts/` holds experiment bash scripts. As of right now, the scripts must be run using the `sbatch` Slurm command, but we are working towards having at least some scripts be runnable with `bash`.
* `attack_data/` holds data helpful in the attack pipeline, from model state dicts to explanation scores, as well as any reported metrics, such as model accuracies and attack TPRs at certain FPRs.
* `data/` holds the datasets [CIFAR-10, CIFAR-100, GTSRB, SVHN, Food 101] downloaded off of `torchvision.datasets` but is empty in this repository.

