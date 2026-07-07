# Qwen3-4B LoRA + CPT Fine-tuning

This repository contains data and notebooks to fine-tune a Qwen3-4B model using a simple two-step approach:

1. Continued Pre-Training (CPT) on raw domain text for knowledge injection.
2. Supervised Fine-Tuning (SFT) on question-answer conversations using LoRA.

The project is built around the Dun & Bradstreet Global Outlook report dataset.

## Files To Upload

Keep only these items in the GitHub repository:

```text
Data/
data-augumentation.ipynb
qwen3-4b-finetuning-v4.ipynb
README.md
```

Do not upload `.env`, checkpoints, duplicate JSON backups, trained model outputs, or local experiment notebooks.

## Repository Structure

```text
.
├── Data/
│   ├── Global_outlok.json
│   └── global_outlook_raw_text.txt
├── data-augumentation.ipynb
├── qwen3-4b-finetuning-v4.ipynb
└── README.md
```

## Data

The `Data/` folder should contain:

- `Global_outlok.json`: conversation-format question-answer dataset.
- `global_outlook_raw_text.txt`: raw report text used for CPT.

The expected JSON format is:

```json
{
  "conversations": [
    [
      {
        "role": "user",
        "content": "Question text"
      },
      {
        "role": "assistant",
        "content": "Answer text"
      }
    ]
  ]
}
```

## Data Augmentation

Use `data-augumentation.ipynb` to create paraphrased question-answer pairs with Azure OpenAI.

Create a local `.env` file before running the notebook:

```text
AZURE_OPENAI_ENDPOINT=https://your-resource.openai.azure.com/
AZURE_OPENAI_API_KEY=your-key
AZURE_OPENAI_API_VERSION=2024-10-21
AZURE_OPENAI_DEPLOYMENT=your-chat-deployment
```

The notebook:

- Reads the source Q&A dataset.
- Uses Azure OpenAI to create paraphrased examples.
- Validates generated output with Pydantic structured output.
- Saves the final conversation-format dataset.

## Fine-tuning

Use `qwen3-4b-finetuning-v4.ipynb` to train the model.

The notebook performs:

1. Model loading with Unsloth.
2. LoRA adapter setup.
3. CPT on the raw report text.
4. SFT on the Q&A conversation dataset.
5. Inference checks after training stages.

The notebook is designed for Google Colab and expects the dataset files to be available in your configured Drive path.

## Recommended Setup

Run the notebooks in Google Colab/Kaggle with GPU enabled.

Main libraries used:

- `unsloth`
- `transformers`
- `datasets`
- `trl`
- `torch`
- `openai`
- `pydantic`
- `python-dotenv`
