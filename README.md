# 🇹🇭 OpenThaiGPT 0.1.0-beta
<img src="https://1173516064-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FvvbWvIIe82Iv1yHaDBC5%2Fuploads%2Fb8eiMDaqiEQL6ahbAY0h%2Fimage.png?alt=media&token=6fce78fd-2cca-4c0a-9648-bd5518e644ce" width="200px">

OpenThaiGPT Version 0.1.0-beta is a 7B-parameter LLaMA model finetuned to follow Thai translated instructions below and makes use of the Huggingface LLaMA implementation. 

## Support
- Offical wbsite: https://openthaigpt.aieat.or.th
- Facebook page: https://web.facebook.com/groups/openthaigpt
- A Discord server for discussion and support [here](https://discord.gg/rUTp6dfVUF)
- E-mail: kobkrit@aieat.or.th

## License
- **Source Code**: Apache Software License 2.0.<br>
- **Weight**: For research use only (due to the Facebook LLama's Weight LICENSE).<br>
- <i>Note that: A commercial use license for OpenThaiGPT 0.1.0 weight will be released later soon!</i>

## Code and Weight

- **Libary Code**: https://github.com/OpenThaiGPT/openthaigpt<br>
- **Finetune Code**: https://github.com/OpenThaiGPT/openthaigpt-finetune-010beta<br>
- **Weight**: https://huggingface.co/kobkrit/openthaigpt-0.1.0-beta

## Sponsors
Pantip.com, ThaiSC<br>
<table>
<tr><td>
<img src="https://1173516064-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FvvbWvIIe82Iv1yHaDBC5%2Fuploads%2FiWjRxBQgo0HUDcpZKf6A%2Fimage.png?alt=media&token=4fef4517-0b4d-46d6-a5e3-25c30c8137a6" width="100px"></td><td>
<img src="https://1173516064-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FvvbWvIIe82Iv1yHaDBC5%2Fuploads%2Ft96uNUI71mAFwkXUtxQt%2Fimage.png?alt=media&token=f8057c0c-5c5f-41ac-bb4b-ad02ee3d4dc2" width="100px"></td>
</tr><table>

### Powered by
OpenThaiGPT Volunteers, Artificial Intelligence Entrepreneur Association of Thailand (AIEAT), and Artificial Intelligence Association of Thailand (AIAT)

<table>
<tr>
<td>
<img src="https://1173516064-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FvvbWvIIe82Iv1yHaDBC5%2Fuploads%2F6yWPXxdoW76a4UBsM8lw%2Fimage.png?alt=media&token=1006ee8e-5327-4bc0-b9a9-a02e93b0c032" width="100px"></td><td><img src="https://1173516064-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FvvbWvIIe82Iv1yHaDBC5%2Fuploads%2FBwsmSovEIhW9AEOlHTFU%2Fimage.png?alt=media&token=5b550289-e9e2-44b3-bb8f-d3057d74f247" width="100px"></td></tr><table>

### Authors
Kobkrit Viriyayudhakorn (kobkrit@aieat.or.th), Sumeth Yuenyong (sumeth.yue@mahidol.edu) and Thaweewat Ruksujarit (thaweewr@scg.com).

<i>Disclaimer: Provided responses are not guaranteed.</i>

### Local Setup

1. Install dependencies

   ```bash
   pip install -r requirements.txt
   ```

1. If bitsandbytes doesn't work, [install it from source.](https://github.com/TimDettmers/bitsandbytes/blob/main/compile_from_source.md) Windows users can follow [these instructions](https://github.com/tloen/alpaca-lora/issues/17).

### Training (`finetune.py`)

This file contains a straightforward application of PEFT to the LLaMA model,
as well as some code related to prompt construction and tokenization.
PRs adapting this code to support larger models are always welcome.

Example usage:

```bash
python finetune.py \
    --base_model 'decapoda-research/llama-7b-hf' \
    --data_path 'Thaweewat/alpaca-cleaned-52k-th' \
    --output_dir './openthaigpt-010-beta'
```

We can also tweak our hyperparameters:

```bash
python finetune.py \
    --base_model 'decapoda-research/llama-7b-hf' \
    --data_path 'Thaweewat/alpaca-cleaned-52k-th' \
    --output_dir './openthaigpt-010-beta' \
    --batch_size 128 \
    --micro_batch_size 4 \
    --num_epochs 3 \
    --learning_rate 1e-4 \
    --cutoff_len 512 \
    --val_set_size 2000 \
    --lora_r 8 \
    --lora_alpha 16 \
    --lora_dropout 0.05 \
    --lora_target_modules '[q_proj,v_proj]' \
    --train_on_inputs \
    --group_by_length
```

### Inference (`generate.py`)

This file reads the foundation model from the Hugging Face model hub and the LoRA weights from `kobkrit/openthaigpt-0.1.0-beta`, and runs a Gradio interface for inference on a specified input. Users should treat this as example code for the use of the model, and modify it as needed.

Example usage:

```bash
python generate.py \
    --load_8bit \
    --base_model 'decapoda-research/llama-7b-hf' \
    --lora_weights 'kobkrit/openthaigpt-0.1.0-beta'
```

### Official weights

The most recent "official" OpenThaiGPT 0.1.0-beta adapter available at [`kobkrit/openthaigpt-0.1.0-beta`](https://huggingface.co/kobkrit/openthaigpt-0.1.0-beta) was trained on May 13 with the following command:

```bash
python finetune.py \
    --base_model='decapoda-research/llama-7b-hf' \
    --data_path '../datasets/cleaned' \
    --num_epochs=3 \
    --cutoff_len=2048 \
    --group_by_length \
    --output_dir='./openthaigpt-010-beta' \
    --lora_target_modules='[q_proj,k_proj,v_proj,o_proj]' \
    --lora_r=64 \
    --batch_size=64 \
    --micro_batch_size=4
```

### Checkpoint export (`export_*_checkpoint.py`)

These files contain scripts that merge the LoRA weights back into the base model
for export to Hugging Face format and to PyTorch `state_dicts`.
They should help users
who want to run inference in projects like [llama.cpp](https://github.com/ggerganov/llama.cpp)
or [alpaca.cpp](https://github.com/antimatter15/alpaca.cpp).

### Docker Setup & Inference

1. Build the container image:

```bash
docker build -t openthaigpt-finetune-010beta .
```

2. Run the container (you can also use `finetune.py` and all of its parameters as shown above for training):

```bash
docker run --gpus=all --shm-size 64g -p 7860:7860 -v ${HOME}/.cache:/root/.cache --rm openthaigpt-finetune-010beta generate.py \
    --load_8bit \
    --base_model 'decapoda-research/llama-7b-hf' \
    --lora_weights 'kobkrit/openthaigpt-0.1.0-beta'
```

3. Open `https://localhost:7860` in the browser

### Docker Compose Setup & Inference

1. (optional) Change desired model and weights under `environment` in the `docker-compose.yml`

2. Build and run the container

```bash
docker-compose up -d --build
```

3. Open `https://localhost:7860` in the browser

4. See logs:

```bash
docker-compose logs -f
```

5. Clean everything up:

```bash
docker-compose down --volumes --rmi all
```

