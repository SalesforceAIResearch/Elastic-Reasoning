<div align="center">

# Elastic Reasoning
<div>
<div>
<h3>🚀 Scalable Chain of Thoughts via Elastic Reasoning 🌟
</div>
</div>
<br>
<div align="center">

[![Paper](https://img.shields.io/badge/paper-A42C25?style=for-the-badge&logo=arxiv&logoColor=white)](https://arxiv.org/pdf/2505.05315)
[![Hugging Face Collection](https://img.shields.io/badge/Elastic_Reasoning-fcd022?style=for-the-badge&logo=huggingface&logoColor=000&labelColor)](https://huggingface.co/Salesforce/Elastic-Reasoning)

</div>
</div>

## Usage


### Installation
```bash
# Installing Python 3.10 Environment.
conda create -n e1 python=3.10 -y
conda activate e1

# Installing dependencies.
cd Elastic-Reasoning
pip install -e ./verl
pip install -e .
```
### Data
Our raw training data is in `rllm/data/[train|test]/[code|math]/`, along with preprocessing scripts in `rllm/data/preprocess`. To convert the raw data into Parquet files for training, run:

```bash
# Download datasets from GDrive, populates rllm/data/[train|test]/[math|code]/*.json
python scripts/data/download_datasets.py

# Generate parquet files for Deepcoder/DeepscaleR in data/*.parquet
python scripts/data/[deepcoder|deepscaler]_dataset.py
```
## Training
```bash
export MODEL_PATH="agentica-org/DeepScaleR-1.5B-Preview"
./scripts/e1-math/e1_math_1.5b_1k_1k.sh --model $MODEL_PATH
```

## Evaluation ⚖️

To run our evaluation scripts, run:
```bash
./scripts/eval/eval_model.sh --model [CHECKPOINT_PATH] --datasets [DATASET1] [DATASET2] --output-dir [OUTPUT_DIR] --n [N_PASSES] --tp [TENSOR_PARALLEL_SIZE] --e1-mode [SEPARATE_BUDGETING] --e1-thinking-length [THINKING_LENGTH] --e1-solution-length [SOLUTION_LENGTH]
```

### Example on MATH
```bash
./scripts/eval/eval_model.sh --model /fsx/home/yuhui/e1_checkpoints/checkpoints/deepscaler/deepscaler-1.5b-2k-1k-1k-truncate/actor/global_step_150 --datasets aime math amc minerva olympiad_bench --output-dir $HOME/DeepScaleR-1.5B-Preview --tp 1 --n 16 --e1-mode True --e1-thinking-length 1024 --e1-solution-length 1024
```
### Example on LiveCodeBench
```bash
./scripts/eval/eval_model.sh --model agentica-org/DeepCoder-14B-Preview --datasets test_livecodebench --output-dir $HOME/DeepCoder-14B-Preview --tp 4 --e1-mode True --e1-thinking-length 1024 --e1-solution-length 1024
```

### Example on Codeforces
```bash
./scripts/eval/eval_model.sh --model agentica-org/DeepCoder-14B-Preview --datasets test_codeforces --output-dir $HOME/DeepCoder-14B-Preview --tp 4 --n 8 --e1-mode True --e1-thinking-length 1024 --e1-solution-length 1024
```
```bash
python scripts/deepcoder/benchmark/cf_elo_calc.py --results_path [RESULTS_JSON_PATH] --pass_n 8
```

## Acknowledgement
We greatly thanks [rllm](https://github.com/agentica-project/rllm) and [verl](https://github.com/volcengine/verl) for providing the awesome codebase!

## Citation


```bibtex
@article{xu2025scalable,
  title={Scalable Chain of Thoughts via Elastic Reasoning},
  author={Xu, Yuhui and Dong, Hanze and Wang, Lei and Sahoo, Doyen and Li, Junnan and Xiong, Caiming},
  journal={arXiv preprint arXiv:2505.05315},
  year={2025}
}
```

## Ethical Considerations
This release is for research purposes only in support of an academic paper. Our models, datasets, and code are not specifically designed or evaluated for all downstream purposes. We strongly recommend users evaluate and address potential concerns related to accuracy, safety, and fairness before deploying this model. We encourage users to consider the common limitations of AI, comply with applicable laws, and leverage best practices when selecting use cases, particularly for high-risk scenarios where errors or misuse could significantly impact people’s lives, rights, or safety. For further guidance on use cases, refer to our AUP and AI AUP.